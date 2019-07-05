---
title: Azure Otomasyonu sertifika varlıkları
description: Güvenli bir şekilde sertifikaları Azure Automation'da olduğundan, runbook veya DSC yapılandırmaları, Azure ve üçüncü taraf kaynaklarına karşı kimlik doğrulaması için tarafından erişilebilir.  Bu makalede, sertifikalar ve hem metin hem de grafik yazma ile çalışma ayrıntılarını açıklar.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: bobbytreed
ms.author: robreed
ms.date: 04/02/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: da116dd8c84e4aa96cc3254218f1ab5d14a8bd6b
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67478188"
---
# <a name="certificate-assets-in-azure-automation"></a>Azure Otomasyonu sertifika varlıkları

Sertifikaları depolanır güvenli bir şekilde Azure Otomasyonu'nda runbook veya DSC yapılandırmaları tarafından erişilebilen şekilde **Get-AzureRmAutomationCertificate** Azure Resource Manager kaynaklarını için etkinlik. Bu özellik, runbook'ları ve kimlik doğrulaması için sertifikalar kullanmak DSC yapılandırmaları oluşturmanıza olanak tanır veya bunları Azure veya üçüncü taraf kaynaklar ekler.

>[!NOTE]
>Azure automation'da güvenli varlıkların kimlik bilgileri, sertifikalar, bağlantılar ve şifrelenmiş değişkenler içerir. Bu varlıklar şifrelenir ve her Otomasyon hesabı için oluşturulan benzersiz bir anahtar kullanarak Azure automation'da depolanır. Bu anahtar depolanan bir sistem anahtar kasası yönetilen. Güvenli bir varlık depolamadan önce anahtarı Key Vault'tan yüklenir ve sonra varlık şifrelemek için kullanılır. Bu işlem, Azure Otomasyonu tarafından yönetilir.

## <a name="azurerm-powershell-cmdlets"></a>AzureRM PowerShell cmdlet'leri

AzureRM için aşağıdaki tabloda yer alan cmdlet'ler Windows PowerShell ile Otomasyon kimlik bilgisi varlıkları oluşturmak ve yönetmek için kullanılır. Bunlar parçası olarak gönderilen [AzureRM.Automation Modülü](/powershell/azure/overview), olduğu Automation runbook'ları ve DSC yapılandırmaları için kullanılabilir.

|Cmdlet'ler|Açıklama|
|:---|:---|
|[Get-AzureRmAutomationCertificate](/powershell/module/azurerm.automation/get-azurermautomationcertificate)|Runbook'tan veya DSC yapılandırmasından içinde kullanmak üzere bir sertifika hakkındaki bilgileri alır. Bu gibi durumlarda, sertifika yalnızca Get-AutomationCertificate etkinliğinden alabilirsiniz.|
|[New-AzureRmAutomationCertificate](/powershell/module/azurerm.automation/new-azurermautomationcertificate)|Azure Automation'a yeni bir sertifika oluşturur.|
[Remove-AzureRmAutomationCertificate](/powershell/module/azurerm.automation/remove-azurermautomationcertificate)|Azure Otomasyonu sertifika kaldırır.|
|[Set-AzureRmAutomationCertificate](/powershell/module/azurerm.automation/set-azurermautomationcertificate)|Sertifikayı karşıya yükleme ve bir .pfx için parolayı ayarlama da dahil olmak üzere mevcut bir sertifikayı özelliklerini ayarlar.|
|[Add-AzureCertificate](/powershell/module/servicemanagement/azure/add-azurecertificate)|Belirtilen bulut hizmeti için hizmet sertifikası yükler.|

## <a name="activities"></a>Etkinlikler

Aşağıdaki tablodaki etkinlikler bir runbook ve DSC yapılandırmaları sertifikaları erişmek için kullanılır.

| Etkinlikler | Açıklama |
|:---|:---|
|Get-AutomationCertificate|Runbook'tan veya DSC yapılandırmasından içinde kullanmak üzere bir sertifika alır. Döndürür bir [System.Security.Cryptography.X509Certificates.X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) nesne.|

> [!NOTE] 
> Değişkenleri kullanmaktan kaçınmanız gerekir – Name parametresinde **Get-AutomationCertificate** runbook veya DSC yapılandırması runbook veya DSC yapılandırma ve Otomasyon değişkenleri arasındaki bağımlılıkları karmaşık hale getirir. Tasarım zamanında.

## <a name="python2-functions"></a>Python2 işlevleri

Aşağıdaki tabloda işlevi Python2 runbook Sertifikalar'da erişmek için kullanılır.

| İşlev | Açıklama |
|:---|:---|
| automationassets.get_automation_certificate | Sertifika varlığı hakkındaki bilgileri alır. |

> [!NOTE]
> İçeri aktarmalısınız **automationassets** başına varlık işlevleri erişmek için Python runbook, modül.

## <a name="creating-a-new-certificate"></a>Yeni bir sertifika oluşturma

Yeni bir sertifika oluşturduğunuzda, Azure Otomasyonu'na ekleme bir .cer veya .pfx dosyasını karşıya yükleyin. Sonra sertifikanın dışarı aktarılabilir olarak işaretlerseniz, dışında Azure Otomasyonu sertifika deposuna aktarabilirsiniz. Dışarı aktarılabilir listelenmiyorsa, ardından bunu yalnızca runbook veya DSC yapılandırması içinde imzalamak için kullanılabilir. Azure Otomasyonu sağlayıcısı için sertifika gerektirir: **Microsoft Gelişmiş RSA ve AES şifreleme sağlayıcısı**.

### <a name="to-create-a-new-certificate-with-the-azure-portal"></a>Azure portalı ile yeni bir sertifika oluşturmak için

1. Otomasyon hesabınızdan tıklayın **varlıklar** açmak için kutucuğa **varlıklar** sayfası.
2. Tıklayın **sertifikaları** açmak için kutucuğa **sertifikaları** sayfası.
3. Tıklayın **sertifika ekleme** sayfanın üstünde.
4. **Ad** kutusuna sertifika için bir ad yazın.
5. Bir .cer veya .pfx dosyasına gözatmak için tıklayın **bir dosya seçin** altında **bir sertifika dosyası karşıya**. Bir .pfx dosyası seçerseniz, parola ve aktarılabilir olup olmadığını belirtin.
6. Tıklayın **Oluştur** yeni sertifika varlığı kaydetmek için.

### <a name="to-create-a-new-certificate-with-powershell"></a>PowerShell ile yeni bir sertifika oluşturmak için

Aşağıdaki örnek yeni bir Otomasyon sertifikası oluşturma ve dışarı aktarılabilir olarak işaretle gösterir. Bu, varolan bir .pfx dosyasına aktarır.

```powershell-interactive
$certificateName = 'MyCertificate'
$PfxCertPath = '.\MyCert.pfx'
$CertificatePassword = ConvertTo-SecureString -String 'P@$$w0rd' -AsPlainText -Force
$ResourceGroup = "ResourceGroup01"

New-AzureRmAutomationCertificate -AutomationAccountName "MyAutomationAccount" -Name $certificateName -Path $PfxCertPath –Password $CertificatePassword -Exportable -ResourceGroupName $ResourceGroup
```

### <a name="create-a-new-certificate-with-resource-manager-template"></a>Resource Manager şablonu ile yeni bir sertifika oluşturma

Aşağıdaki örnek bir Resource Manager şablonu aracılığıyla PowerShell kullanarak Automation hesabınız için bir sertifika dağıtmayı gösterir:

```powershell-interactive
$AutomationAccountName = "<automation account name>"
$PfxCertPath = '<PFX cert path'
$CertificatePassword = '<password>'
$certificateName = '<certificate name>'
$AutomationAccountName = '<automation account name>'
$flags = [System.Security.Cryptography.X509Certificates.X509KeyStorageFlags]::Exportable `
    -bor [System.Security.Cryptography.X509Certificates.X509KeyStorageFlags]::PersistKeySet `
    -bor [System.Security.Cryptography.X509Certificates.X509KeyStorageFlags]::MachineKeySet
# Load the certificate into memory
$PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPath, $CertificatePassword, $flags)
# Export the certificate and convert into base 64 string
$Base64Value = [System.Convert]::ToBase64String($PfxCert.Export([System.Security.Cryptography.X509Certificates.X509ContentType]::Pkcs12))
$Thumbprint = $PfxCert.Thumbprint


$json = @"
{
    '`$schema': 'https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#',
    'contentVersion': '1.0.0.0',
    'resources': [
        {
            'name': '$AutomationAccountName/$certificateName',
            'type': 'Microsoft.Automation/automationAccounts/certificates',
            'apiVersion': '2015-10-31',
            'properties': {
                'base64Value': '$Base64Value',
                'thumbprint': '$Thumbprint',
                'isExportable': true
            }
        }
    ]
}
"@

$json | out-file .\template.json
New-AzureRmResourceGroupDeployment -Name NewCert -ResourceGroupName TestAzureAuto -TemplateFile .\template.json
```

## <a name="using-a-certificate"></a>Bir sertifika kullanıyor

Bir sertifika kullanmak için **Get-AutomationCertificate** etkinlik. Kullanamazsınız [Get-AzureRmAutomationCertificate](/powershell/module/azurerm.automation/get-azurermautomationcertificate) cmdlet'i bu yana sertifika varlığı ama sertifikanın kendisi değil, ilgili bilgileri döndürür.

### <a name="textual-runbook-sample"></a>Metinsel runbook örneği

Aşağıdaki örnek kod, bir runbook'ta bir bulut hizmeti için bir sertifika eklemek gösterilmektedir. Bu örnekte, parolayı bir şifrelenmiş Otomasyon değişkeninden alınmaktadır.

```powershell-interactive
$serviceName = 'MyCloudService'
$cert = Get-AutomationCertificate -Name 'MyCertificate'
$certPwd = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name 'MyCertPassword'
Add-AzureCertificate -ServiceName $serviceName -CertToDeploy $cert
```

### <a name="graphical-runbook-sample"></a>Grafik runbook örneği

Eklediğiniz bir **Get-AutomationCertificate** sağ tıklayarak sertifikayı Kitaplık bölmesinde ve seçerek bir grafik runbook **tuvale Ekle**.

![Sertifika tuvale Ekle](../media/certificates/automation-certificate-add-to-canvas.png)

Aşağıdaki görüntüde, grafik bir runbook'ta bir sertifika kullanarak bir örnek gösterilmektedir. Bu gösteren bir metinsel runbook'tan bir bulut hizmeti için bir sertifika eklemek önceki örnekte aynıdır.

![Örnek grafik yazma](../media/certificates/graphical-runbook-add-certificate.png)

### <a name="python2-sample"></a>Python2 örnek

Aşağıdaki örnek Python2 runbook'ları sertifikaları nasıl gösterir.

```python
# get a reference to the Azure Automation certificate
cert = automationassets.get_automation_certificate("AzureRunAsCertificate")

# returns the binary cert content  
print cert
```

## <a name="next-steps"></a>Sonraki adımlar

- Runbook'unuzu gerçekleştirmek üzere tasarlanan etkinliklerin mantıksal akışını denetlemek için bağlantılar ile çalışma hakkında daha fazla bilgi edinmek için [grafik yazma içinde bağlantıları](../automation-graphical-authoring-intro.md#links-and-workflow). 
