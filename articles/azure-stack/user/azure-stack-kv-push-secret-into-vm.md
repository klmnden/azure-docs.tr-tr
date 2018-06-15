---
title: Azure yığında güvenli şekilde depolanan bir sertifika ile bir sanal makine dağıtma | Microsoft Docs
description: Bir sanal makine dağıtma ve Azure yığınında bir anahtar kasası kullanarak anında iletme sertifikası üzerine hakkında bilgi edinin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 46590eb1-1746-4ecf-a9e5-41609fde8e89
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/10/2018
ms.author: mabrigg
ms.openlocfilehash: 3950c9dfc5ff5f7ea1d170da086b4f97048ed81c
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
ms.locfileid: "34069042"
---
# <a name="create-a-virtual-machine-and-install-a-certificate-retrieved-from-an-azure-stack-key-vault"></a>Bir sanal makine oluşturun ve Azure yığın anahtar Kasası'nı alınan bir sertifika yükleyin

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Bir Azure yığın sanal makine (VM) oluşturmak bir anahtar kasası sertifika yüklü öğrenin.

## <a name="overview"></a>Genel Bakış

Sertifikalar için Active Directory kimlik doğrulaması ya da web trafiği şifreleme gibi çok sayıda senaryolarda kullanılır. Bir Azure yığın anahtar kasasına gizli olarak sertifikaları güvenli bir şekilde depolayabilirsiniz. Azure yığın anahtar kasası kullanmanın avantajları şunlardır:

* Sertifikalar, bir komut dosyası, komut satırı geçmiş veya şablonda açık değil.
* Sertifika Yönetimi işlemi hızlandırıldı.
* Sertifikaları erişim anahtarları denetime sahiptir.

### <a name="process-description"></a>İşlem açıklaması

Aşağıdaki adımlarda, bir sanal makine sertifikası göndermek için gereken işlemi açıklanmaktadır:

1. Gizli bir anahtar kasası oluşturun.
2. Azuredeploy.parameters.json dosyasını güncelleştirin.
3. Şablonu dağıtma

>[!NOTE]
>VPN üzerinden bağlandığı sırada Azure yığın Geliştirme Seti veya bir dış istemcinin aşağıdaki adımları kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Anahtar kasası hizmetindeki içeren bir teklif abone olmalısınız.
* [PowerShell için Azure yığın yükleyin.](azure-stack-powershell-install.md)
* [Azure yığın kullanıcının PowerShell ortamını yapılandırma](azure-stack-powershell-configure-user.md)

## <a name="create-a-key-vault-secret"></a>Gizli bir anahtar kasası oluşturma

Aşağıdaki komut dosyası .pfx biçiminde bir sertifika oluşturur, bir anahtar kasası oluşturur ve sertifika anahtar kasası gizli olarak depolar.

>[!IMPORTANT]
>Kullanmalısınız `-EnabledForDeployment` anahtar hataya oluşturulurken parametre. Bu parametre, anahtar kasası Azure Resource Manager şablonlarını başvurulabilir sağlar.

```powershell

# Create a certificate in the .pfx format
New-SelfSignedCertificate `
  -certstorelocation cert:\LocalMachine\My `
  -dnsname contoso.microsoft.com

$pwd = ConvertTo-SecureString `
  -String "<Password used to export the certificate>" `
  -Force `
  -AsPlainText

Export-PfxCertificate `
  -cert "cert:\localMachine\my\<Certificate Thumbprint that was created in the previous step>" `
  -FilePath "<Fully qualified path where the exported certificate can be stored>" `
  -Password $pwd

# Create a key vault and upload the certificate into the key vault as a secret
$vaultName = "contosovault"
$resourceGroup = "contosovaultrg"
$location = "local"
$secretName = "servicecert"
$fileName = "<Fully qualified path where the exported certificate can be stored>"
$certPassword = "<Password used to export the certificate>"

$fileContentBytes = get-content $fileName `
  -Encoding Byte

$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
$jsonObject = @"
{
"data": "$filecontentencoded",
"dataType" :"pfx",
"password": "$certPassword"
}
"@
$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

New-AzureRmResourceGroup `
  -Name $resourceGroup `
  -Location $location

New-AzureRmKeyVault `
  -VaultName $vaultName `
  -ResourceGroupName $resourceGroup `
  -Location $location `
  -sku standard `
  -EnabledForDeployment

$secret = ConvertTo-SecureString `
  -String $jsonEncoded `
  -AsPlainText -Force

Set-AzureKeyVaultSecret `
  -VaultName $vaultName `
  -Name $secretName `
   -SecretValue $secret

```

Önceki komut dosyasını çalıştırdığınızda, çıktı gizli URI içerir. Bu URI not edin. İçinde başvurmak zorunda [Windows Resource Manager şablonu itme sertifikaya](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/201-vm-windows-pushcertificate). Karşıdan [itme sertifika windows vm şablonu](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/201-vm-windows-pushcertificate) geliştirme bilgisayarınıza klasör. Bu klasörde `azuredeploy.json` ve `azuredeploy.parameters.json` sonraki adımlarda gerekir dosyaları.

Değiştirme `azuredeploy.parameters.json` ortamı değerlerinizi göre dosya. Özel ilgi kasa adını, kasa kaynak grubu ve gizli anahtarı (önceki komut dosyası tarafından oluşturulan gibi) URI parametreleridir. Aşağıdaki dosya, bir parametre dosyası örneğidir:

## <a name="update-the-azuredeployparametersjson-file"></a>Azuredeploy.parameters.json dosyasını güncelleştirme

VaultName, gizli URI, VmName ve diğer değerleri ortamınıza uygun şekilde azuredeploy.parameters.json dosyasını güncelleştirin. Aşağıdaki JSON dosyası şablon parametreleri dosyası örneği gösterilmektedir:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "newStorageAccountName": {
      "value": "kvstorage01"
    },
    "vmName": {
      "value": "VM1"
    },
    "vmSize": {
      "value": "Standard_D1_v2"
    },
    "adminUserName": {
      "value": "demouser"
    },
    "adminPassword": {
      "value": "demouser@123"
    },
    "vaultName": {
      "value": "contosovault"
    },
    "vaultResourceGroup": {
      "value": "contosovaultrg"
    },
    "secretUrlWithVersion": {
      "value": "https://testkv001.vault.local.azurestack.external/secrets/testcert002/82afeeb84f4442329ce06593502e7840"
    }
  }
}
```

## <a name="deploy-the-template"></a>Şablonu dağıtma

Aşağıdaki PowerShell betiğini kullanarak şablonu dağıtma:

```powershell
# Deploy a Resource Manager template to create a VM and push the secret onto it
New-AzureRmResourceGroupDeployment `
  -Name KVDeployment `
  -ResourceGroupName $resourceGroup `
  -TemplateFile "<Fully qualified path to the azuredeploy.json file>" `
  -TemplateParameterFile "<Fully qualified path to the azuredeploy.parameters.json file>"
```

Şablonu başarıyla dağıtıldığında, şunlara sebep olur:

![Şablon dağıtımı sonuçları](media/azure-stack-kv-push-secret-into-vm/deployment-output.png)

Azure yığın dağıtımı sırasında sanal makine sertifikası iter. Sertifikanın konumu sanal makinenin işletim sistemine bağlıdır:

* Windows, LocalMachine sertifika konumla kullanıcı tarafından sağlanan sertifika deposuna sertifika eklenir.
* Linux sertifika dosya adı ile /var/lib/waagent dizinde altına yerleştirilmiş &lt;UppercaseThumbprint&gt;X509 .crt Sertifika dosyası ve &lt;UppercaseThumbprint&gt;.prv özel anahtar için .

## <a name="retire-certificates"></a>Sertifikaları devre dışı bırakma

Sertifikaları devre dışı bırakma, sertifika yönetimi işleminin parçasıdır. Bir sertifika daha eski sürümü silinemez, ancak kullanarak devre dışı bırakabilirsiniz `Set-AzureKeyVaultSecretAttribute` cmdlet'i.

Aşağıdaki örnek, bir sertifika devre dışı bırakmak gösterilmiştir. İçin kendi değerlerinizi kullanmak **VaultName**, **adı**, ve **sürüm** parametreleri.

```powershell
Set-AzureKeyVaultSecretAttribute -VaultName contosovault -Name servicecert -Version e3391a126b65414f93f6f9806743a1f7 -Enable 0
```

## <a name="next-steps"></a>Sonraki adımlar

* [Anahtar Kasası parolası ile VM dağıtma](azure-stack-kv-deploy-vm-with-secret.md)
* [Anahtar kasası erişmek bir uygulama izin verme](azure-stack-kv-sample-app.md)
