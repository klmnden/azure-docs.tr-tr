---
title: Azure HPC Pack 2016 kümede | Microsoft Docs
description: Azure HPC Pack 2016 kümede dağıtmayı öğrenin
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 3dde6a68-e4a6-4054-8b67-d6a90fdc5e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 03/09/2018
ms.author: danlep
ms.openlocfilehash: 91f067de33d1ff4bc272773e3db49de47fac2feb
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="deploy-an-hpc-pack-2016-cluster-in-azure"></a>Azure'da bir HPC Pack 2016 kümeyi dağıtma

Dağıtmak için bu makaledeki adımları bir [Microsoft HPC Pack 2016 güncelleştirme 1](https://technet.microsoft.com/library/cc514029) Azure sanal makinelerde küme. HPC Pack, Microsoft'un Microsoft Azure ve Windows Server teknolojileri kurulu ücretsiz HPC çözüm ve geniş HPC iş yüklerini destekler.

Aşağıdakilerden birini kullanmak [Azure Resource Manager şablonları](https://github.com/MsHpcPack/HPCPack2016) HPC Pack 2016 küme dağıtmak için. Farklı numaralarıyla küme topolojisinin birkaç seçeneğiniz vardır ve türleri, küme düğümleri head ve işlem düğümleri.

## <a name="prerequisites"></a>Önkoşullar

### <a name="pfx-certificate"></a>PFX sertifikası

Microsoft HPC Pack 2016 kümesi HPC düğümler arasındaki iletişimin güvenliğini sağlamak için bir kişisel bilgi değişimi (PFX) sertifika gerektirir. Sertifikanın aşağıdaki gereksinimleri karşılamalıdır:

* Anahtar değişimi yeteneğine sahip bir özel anahtara sahip olmalıdır
* Dijital imza ve anahtar şifreleme anahtar kullanımı içerir
* İstemci kimlik doğrulaması ve sunucu kimlik doğrulaması Gelişmiş anahtar kullanımı içerir

Bu gereksinimleri karşılayan bir sertifika yoksa, bir sertifika yetkilisinden sertifika isteyebilir. Alternatif olarak, komutu çalıştırdığınız işletim sistemi temelinde otomatik olarak imzalanan sertifika oluşturmak için aşağıdaki komutları kullanın. Sonra sertifika özel anahtarıyla parola korumalı bir PFX dosyası olarak dışarı aktarın.

* **Windows 10 veya Windows Server 2016 için**, yerleşik çalıştırmak **yeni SelfSignedCertificate** PowerShell cmdlet'ini şekilde:

  ```PowerShell
  New-SelfSignedCertificate -Subject "CN=HPC Pack 2016 Communication" -KeySpec KeyExchange -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1,1.3.6.1.5.5.7.3.2") -CertStoreLocation cert:\CurrentUser\My -KeyExportPolicy Exportable -NotAfter (Get-Date).AddYears(5)
  ```
* **Windows 10 veya Windows Server 2016'den önceki işletim sistemleri için**, indirme [otomatik olarak imzalanan sertifika Oluşturucu](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) Microsoft Script Center gelen. İçeriğini ayıklayın ve bir PowerShell komut isteminde aşağıdaki komutları çalıştırın:

    ```PowerShell 
    Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
  
    New-SelfSignedCertificateEx -Subject "CN=HPC Pack 2016 Communication" -KeySpec Exchange -KeyUsage "DigitalSignature,KeyEncipherment" -EnhancedKeyUsage "Server Authentication","Client Authentication" -StoreLocation CurrentUser -Exportable -NotAfter (Get-Date).AddYears(5)
    ```

Sertifika oluşturulduktan sonra geçerli kullanıcı deposunda sertifikayı özel anahtarıyla bir parola korumalı PFX dosyası olarak dışa Sertifikalar ek bileşenini kullanın. Sertifikayı kullanarak da verebilirsiniz [verme Pfxcertificate](/powershell/module/pkiclient/export-pfxcertificate?view=win10-ps) PowerShell cmdlet'i.

### <a name="upload-certificate-to-an-azure-key-vault"></a>Bir Azure anahtar kasası sertifikasını karşıya yükle

HPC küme dağıtmadan önce için PFX sertifikasını karşıya bir [Azure anahtar kasası](../../key-vault/index.yml) gizli anahtarı ve kayıt kullanmak için aşağıdaki bilgileri dağıtım sırasında olarak: **kasa adı**, **kasası kaynak grubu**, **sertifika URL'si**, ve **sertifika parmak izi**.

Sertifikayı karşıya yükleyin, anahtar kasası oluşturma ve gerekli bilgileri oluşturmak için bir PowerShell komut dosyası izler. Bir Azure anahtar kasası için bir sertifika karşıya yükleme hakkında daha fazla bilgi için bkz: [Azure anahtar kasası ile çalışmaya başlama](../../key-vault/key-vault-get-started.md).

```powershell
#Give the following values
$VaultName = "mytestvault"
$SecretName = "hpcpfxcert"
$VaultRG = "myresourcegroup"
$location = "westus"
$PfxFile = "c:\Temp\mytest.pfx"
$Password = "yourpfxkeyprotectionpassword"
#Validate the pfx file
try {
    $pfxCert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList $PfxFile, $Password
}
catch [System.Management.Automation.MethodInvocationException]
{
    throw $_.Exception.InnerException
}
$thumbprint = $pfxCert.Thumbprint
$pfxCert.Dispose()
# Create and encode the JSON object
$pfxContentBytes = Get-Content $PfxFile -Encoding Byte
$pfxContentEncoded = [System.Convert]::ToBase64String($pfxContentBytes)
$jsonObject = @"
{
"data": "$pfxContentEncoded",
"dataType": "pfx",
"password": "$Password"
}
"@
$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)
#Create an Azure key vault and upload the certificate as a secret
$secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
$rg = Get-AzureRmResourceGroup -Name $VaultRG -Location $location -ErrorAction SilentlyContinue
if($null -eq $rg)
{
    $rg = New-AzureRmResourceGroup -Name $VaultRG -Location $location
}
$hpcKeyVault = New-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $VaultRG -Location $location -EnabledForDeployment -EnabledForTemplateDeployment
$hpcSecret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secret
"The following Information will be used in the deployment template"
"Vault Name             :   $VaultName"
"Vault Resource Group   :   $VaultRG"
"Certificate URL        :   $($hpcSecret.Id)"
"Certificate Thumbprint :   $thumbprint"

```


## <a name="supported-topologies"></a>Desteklenen topolojiler

Aşağıdakilerden birini seçin [Azure Resource Manager şablonları](https://github.com/MsHpcPack/HPCPack2016) HPC Pack 2016 küme dağıtmak için. Üç örnek küme topolojileri üst düzey mimarilerini aşağıda verilmiştir. Yüksek kullanılabilirlik topolojileri birden çok küme baş düğümüne ekleyin.

1. Active Directory etki alanı ile yüksek oranda kullanılabilirlik kümesi

    ![AD etki alanındaki HA küme](./media/hpcpack-2016-cluster/haad.png)


2. Active Directory etki alanı olmadan yüksek oranda kullanılabilirlik kümesi

    ![AD etki alanı olmadan HA küme](./media/hpcpack-2016-cluster/hanoad.png)

3. Tek bir baş düğüm Küme

   ![Baş düğümü ile küme](./media/hpcpack-2016-cluster/singlehn.png)


## <a name="deploy-a-cluster"></a>Bir kümeyi dağıtma

Kümeyi oluşturmak için bir şablon seçin ve **Azure'a Dağıt**. İçinde [Azure portal](https://portal.azure.com), aşağıdaki adımlarda açıklandığı gibi şablon parametreleri belirtin. Her şablon için HPC küme altyapısı için gereken tüm Azure kaynakları oluşturur. Bir Azure sanal ağı, ortak IP adresi, yük dengeleyici (yalnızca için yüksek oranda kullanılabilirlik kümesi), ağ arabirimleri, kullanılabilirlik kümeleri, depolama hesapları ve sanal makineler kaynaklar içerir.

### <a name="step-1-select-the-subscription-location-and-resource-group"></a>1. adım: Abonelik, konum ve kaynak grubu seçin

**Abonelik** ve **konumu** PFX sertifikanızı karşıya belirtilen aynı olmalıdır (önkoşullara bakın). Farklı bir oluşturmanızı öneririz **kaynak grubu** dağıtımı için.

### <a name="step-2-specify-the-parameter-settings"></a>2. adım: parametre ayarlarını belirtin

Girin veya şablon parametre değerlerini değiştirin. Her parametre için Yardım bilgileri yanındaki simgesine tıklayın. Ayrıca yönergeler için bkz. [kullanılabilir VM boyutları](sizes.md).

Kaydettiğiniz Önkoşullar aşağıdaki parametreleri için değerleri belirtin: **kasa adı**, **kasası kaynak grubu**, **sertifika URL'si**, ve **sertifika parmak izi**.

### <a name="step-3-review-terms-and-create"></a>3. Adım Koşullarını gözden geçirin ve oluşturma
Hüküm ve koşulları şablonuyla ilişkili gözden geçirin. Kabul ediyorsa **satın alma** dağıtımı başlatmak için.

Küme topolojisine bağlı olarak dağıtım 30 dakika sürebilir veya tamamlamak için daha uzun.

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma
1. HPC Pack küme dağıtıldıktan sonra Git [Azure portal](https://portal.azure.com). Tıklatın **kaynak grupları**ve küme dağıtıldığı kaynak grup bulunamıyor. Baş düğüm sanal makineleri bulabilirsiniz.

    ![Küme baş düğümler portalında](./media/hpcpack-2016-cluster/clusterhns.png)

2. Bir baş düğümüne tıklayın (bir yüksek kullanılabilirlik kümesinde baş düğümler birini tıklatın). İçinde **genel bakış**, genel IP adresi veya küme tam DNS adını bulabilirsiniz.

    ![Küme bağlantısı ayarları](./media/hpcpack-2016-cluster/clusterconnect.png)

3. Tıklatın **Bağlan** herhangi bir Uzak Masaüstü kullanarak, belirtilen yönetici kullanıcı adınızla baş düğüme oturum. Dağıttığınız küme bir Active Directory etki alanında ise, kullanıcı adı biçimindedir \<privateDomainName >\\\<adminUsername > (örneğin, hpc.local\hpcadmin).

## <a name="next-steps"></a>Sonraki adımlar
* İşlerini kümenize gönderin. Bkz: [HPC bir HPC paketi için gönderme işleri küme Azure'da](hpcpack-cluster-submit-jobs.md) ve [Azure Active Directory'yi kullanarak azure'da bir HPC Pack 2016 kümeyi yönetmek](hpcpack-cluster-active-directory.md).

