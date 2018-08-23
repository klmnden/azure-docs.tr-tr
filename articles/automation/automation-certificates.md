---
title: Azure Otomasyonu sertifika varlıkları
description: Runbook'ları veya Azure ve üçüncü taraf kaynaklarına karşı kimlik doğrulaması için DSC yapılandırmaları tarafından erişilebilen için Azure Otomasyonu'nda sertifikaları güvenli bir şekilde depolanabilir.  Bu makalede, sertifikalar ve hem metin hem de grafik yazma ile çalışma ayrıntılarını açıklar.
services: automation
ms.service: automation
ms.component: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 03/15/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: c13da6ff7c864ffa365dbad33d6eb0cf2e35fa42
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42056012"
---
# <a name="certificate-assets-in-azure-automation"></a>Azure Otomasyonu sertifika varlıkları

Sertifikaları depolanabilir güvenli bir şekilde Azure Otomasyonu'nda runbook veya DSC yapılandırmaları tarafından erişilebilen şekilde **Get-AzureRmAutomationCertificate** Azure Resource Manager kaynaklarını için etkinlik. Bu özellik, runbook'ları ve kimlik doğrulaması için sertifikalar kullanmak DSC yapılandırmaları oluşturmanıza olanak tanır veya bunları Azure veya üçüncü taraf kaynaklar ekler.

>[!NOTE]
>Azure automation'da güvenli varlıkların kimlik bilgileri, sertifikalar, bağlantılar ve şifrelenmiş değişkenler içerir. Bu varlıklar şifrelenir ve her Otomasyon hesabı için oluşturulan benzersiz bir anahtar kullanarak Azure automation'da depolanır. Bu anahtar, anahtar Kasası'nda depolanır. Güvenli bir varlık depolamadan önce anahtarı Key Vault'tan yüklenir ve sonra varlık şifrelemek için kullanılır.

## <a name="azurerm-powershell-cmdlets"></a>AzureRM PowerShell cmdlet'leri
AzureRM için aşağıdaki tabloda yer alan cmdlet'ler Windows PowerShell ile Otomasyon kimlik bilgisi varlıkları oluşturmak ve yönetmek için kullanılır. Bunlar parçası olarak gönderilen [AzureRM.Automation Modülü](/powershell/azure/overview) olduğu Automation runbook'ları ve DSC yapılandırmaları için kullanılabilir.

|Cmdlet'leri|Açıklama|
|:---|:---|
|[Get-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationcertificate)|Runbook'tan veya DSC yapılandırmasından içinde kullanmak üzere bir sertifika hakkındaki bilgileri alır. Bu gibi durumlarda, sertifika yalnızca Get-AutomationCertificate etkinliğinden alabilirsiniz.|
|[Yeni AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/new-azurermautomationcertificate)|Azure Automation'a yeni bir sertifika oluşturur.|
[Remove-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/remove-azurermautomationcertificate)|Azure Otomasyonu sertifika kaldırır.|Azure Automation'a yeni bir sertifika oluşturur.
|[Set-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/set-azurermautomationcertificate)|Sertifikayı karşıya yükleme ve bir .pfx için parolayı ayarlama da dahil olmak üzere mevcut bir sertifikayı özelliklerini ayarlar.|
|[Add-AzureCertificate](https://msdn.microsoft.com/library/azure/dn495214.aspx)|Belirtilen bulut hizmeti için hizmet sertifikası yükler.|

## <a name="activities"></a>Etkinlikler
Aşağıdaki tablodaki etkinlikler bir runbook ve DSC yapılandırmaları sertifikaları erişmek için kullanılır.

| Etkinlikler | Açıklama |
|:---|:---|
|Get-AutomationCertificate|Runbook'tan veya DSC yapılandırmasından içinde kullanmak üzere bir sertifika alır. Döndürür bir [System.Security.Cryptography.X509Certificates.X509Certificate2](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate2.aspx) nesne.|

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

Yeni bir sertifika oluşturduğunuzda, Azure Otomasyonu'na ekleme bir .cer veya .pfx dosyasını karşıya yükleyin. Sonra sertifikanın dışarı aktarılabilir olarak işaretlerseniz, dışında Azure Otomasyonu sertifika deposuna aktarabilirsiniz. Dışarı aktarılabilir olmadığı, ardından bunu yalnızca runbook veya DSC yapılandırması içinde imzalamak için kullanılabilir. Azure Otomasyonu sağlayıcısı için sertifika gerektirir: **Microsoft Gelişmiş RSA ve AES şifreleme sağlayıcısı**.

### <a name="to-create-a-new-certificate-with-the-azure-portal"></a>Azure portalı ile yeni bir sertifika oluşturmak için

1. Otomasyon hesabınızdan tıklayın **varlıklar** açmak için kutucuğa **varlıklar** dikey penceresi.
1. Tıklayın **sertifikaları** açmak için kutucuğa **sertifikaları** dikey penceresi.
1. Tıklayın **sertifika ekleme** dikey penceresinin üstünde.
1. Sertifika için bir ad yazın **adı** kutusu.
1. Bir .cer veya .pfx dosyasına gözatmak için tıklayın **bir dosya seçin** altında **bir sertifika dosyası karşıya**. Bir .pfx dosyası seçerseniz, parola ve bunu dışarı aktarılmasına izin verilip verilmediğine belirtin.
1. Tıklayın **Oluştur** yeni sertifika varlığı kaydetmek için.

### <a name="to-create-a-new-certificate-with-windows-powershell"></a>Windows PowerShell ile yeni bir sertifika oluşturmak için

Aşağıdaki örnek yeni bir Otomasyon sertifikası oluşturma ve dışarı aktarılabilir olarak işaretle gösterir. Bu, varolan bir .pfx dosyasına aktarır.

```powershell-interactive
$certName = 'MyCertificate'
$certPath = '.\MyCert.pfx'
$certPwd = ConvertTo-SecureString -String 'P@$$w0rd' -AsPlainText -Force
$ResourceGroup = "ResourceGroup01"

New-AzureRmAutomationCertificate -AutomationAccountName "MyAutomationAccount" -Name $certName -Path $certPath –Password $certPwd -Exportable -ResourceGroupName $ResourceGroup
```

## <a name="using-a-certificate"></a>Bir sertifika kullanıyor

Bir sertifika kullanmak için **Get-AutomationCertificate** etkinlik. Kullanamazsınız [Get-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationcertificate?view=azurermps-6.6.0) cmdlet'i bu yana sertifika varlığı ama sertifikanın kendisi değil, ilgili bilgileri döndürür.

### <a name="textual-runbook-sample"></a>Metinsel runbook örneği

Aşağıdaki örnek kod, bir runbook'ta bir bulut hizmeti için bir sertifika eklemek gösterilmektedir. Bu örnekte, parolayı bir şifrelenmiş Otomasyon değişkeninden alınmaktadır.

```powershell-interactive
$serviceName = 'MyCloudService'
$cert = Get-AutomationCertificate -Name 'MyCertificate'
$certPwd = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name 'MyCertPassword'
Add-AzureCertificate -ServiceName $serviceName -CertToDeploy $cert
```

### <a name="graphical-runbook-sample"></a>Grafik runbook örneği

Eklediğiniz bir **Get-AutomationCertificate** sağ tıklayarak grafik düzenleyicisini Kitaplık bölmesinde sertifika ve seçerek bir grafik runbook **tuvale Ekle**.

![Sertifika tuvale Ekle](media/automation-certificates/automation-certificate-add-to-canvas.png)

Aşağıdaki görüntüde, grafik bir runbook'ta bir sertifika kullanarak bir örnek gösterilmektedir. Bu sertifika, metinsel bir runbook'tan bir bulut hizmetine eklemek için önceki örnekte aynıdır.

![Örnek grafik yazma ](media/automation-certificates/graphical-runbook-add-certificate.png)

### <a name="python2-sample"></a>Python2 örnek
Aşağıdaki örnek Python2 runbook'ları sertifikaları nasıl gösterir.

```python
# get a reference to the Azure Automation certificate
cert = automationassets.get_automation_certificate("AzureRunAsCertificate")

# returns the binary cert content  
print cert 
```

## <a name="next-steps"></a>Sonraki adımlar

- Runbook'unuzu gerçekleştirmek üzere tasarlanan etkinliklerin mantıksal akışını denetlemek için bağlantılar ile çalışma hakkında daha fazla bilgi edinmek için [grafik yazma içinde bağlantıları](automation-graphical-authoring-intro.md#links-and-workflow). 
