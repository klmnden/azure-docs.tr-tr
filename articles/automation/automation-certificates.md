---
title: "Azure Otomasyonu sertifika varlıkları"
description: "Runbook'ları veya Azure ve üçüncü taraf kaynaklarına karşı kimlik doğrulaması için DSC yapılandırması tarafından erişilebilecek şekilde sertifikaları güvenli bir şekilde Azure Otomasyonu'nda depolanabilir.  Bu makalede, sertifikalar ve bunlarla metinsel ve grafik yazma çalışma ayrıntılarını açıklanmaktadır."
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/15/2018
ms.topic: article
manager: carmonm
ms.devlang: na
ms.tgt_pltfrm: na
ms.openlocfilehash: d4e205365b884b683928e42d538c085c4df2d6ed
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="certificate-assets-in-azure-automation"></a>Azure Otomasyonu sertifika varlıkları

Sertifikaları depolanabilir güvenli bir şekilde Azure Otomasyonu'nda runbook'lar veya kullanarak DSC yapılandırması tarafından erişilebilmeleri adına **Get-AzureRmAutomationCertificate** etkinlik Azure Resource Manager kaynakları için. Bu özellik, runbook'ları ve kimlik doğrulaması için sertifikalar kullanmak DSC yapılandırmaları oluşturmanıza olanak sağlar veya Azure veya üçüncü taraf kaynakları ekler.

>[!NOTE]
>Azure Automation güvenli varlıkların kimlik bilgileri, sertifikalar, bağlantıları ve şifrelenmiş değişkenler içerir. Bu varlıklar şifrelenir ve her Otomasyon hesabı için oluşturulan benzersiz bir anahtar kullanarak Azure automation'da depolanır. Bu anahtar, anahtar kasasında depolanır. Güvenli bir varlık depolamadan önce anahtarı anahtar Kasası'nı yüklenir ve varlık şifrelemek için kullanılır.

## <a name="azurerm-powershell-cmdlets"></a>AzureRM PowerShell cmdlet'leri
AzureRM için aşağıdaki tablodaki cmdlet'ler oluşturmak ve Windows PowerShell ile automation kimlik bilgisi varlıkları yönetmek için kullanılır. Bir parçası olarak sevk [AzureRM.Automation Modülü](/powershell/azure/overview) olduğu Automation runbook'ları ve DSC yapılandırmaları için kullanılabilir.

|Cmdlet'leri|Açıklama|
|:---|:---|
|[Get-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationcertificate)|Bir runbook veya DSC yapılandırması kullanmak için bir sertifikayla ilgili bilgileri alır. Bu gibi durumlarda, sertifika yalnızca Get-AutomationCertificate etkinliğinden alabilirsiniz.|
|[New-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/new-azurermautomationcertificate)|Yeni bir sertifika Azure Automation'a oluşturur.|
[Remove-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/remove-azurermautomationcertificate)|Bir sertifika Azure Otomasyon kaldırır.|Yeni bir sertifika Azure Automation'a oluşturur.
|[Set-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/set-azurermautomationcertificate)|Sertifika dosyası karşıya yükleme ve bir .pfx için parolayı ayarlama da dahil olmak üzere var olan bir sertifikayı özelliklerini ayarlar.|
|[Add-AzureCertificate](https://msdn.microsoft.com/library/azure/dn495214.aspx)|Belirtilen bulut hizmeti için hizmet sertifikası yükler.|

## <a name="activities"></a>Etkinlikler
Aşağıdaki tablodaki etkinlikler bir runbook ve DSC yapılandırmaları sertifikalarda erişmek için kullanılır.

| Etkinlikler | Açıklama |
|:---|:---|
|Get-AutomationCertificate|Bir runbook veya DSC yapılandırması kullanmak üzere bir sertifika alır. Döndürür bir [System.Security.Cryptography.X509Certificates.X509Certificate2](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate2.aspx) nesnesi.|

> [!NOTE] 
> Yapmaktan kaçınmalısınız – Name parametresinde **Get-AutomationCertificate** bir runbook ya da DSC yapılandırması runbook'lar veya DSC yapılandırması ve Otomasyon değişkenleri arasındaki bağımlılıkları karmaşıklaştırır Tasarım zamanında.

## <a name="python2-functions"></a>Python2 işlevleri

Aşağıdaki tabloda işlevinde Python2 runbook sertifikalarda erişmek için kullanılır.

| İşlev | Açıklama |
|:---|:---|
| automationassets.get_automation_certificate | Bir sertifika varlığı ilgili bilgileri alır. |

> [!NOTE]
> İçeri aktarmanız gerekir **automationassets** varlık işlevleri erişmek için Python runbook'unuz başlangıcını modülünde.

## <a name="creating-a-new-certificate"></a>Yeni bir sertifika oluşturma

Yeni bir sertifika oluşturduğunuzda, Azure Automation .cer veya .pfx dosyasını karşıya yükleyin. Ardından sertifikası dışarı aktarılabilir olarak işaretlerseniz, bunu Azure Otomasyonu sertifika deposunu dışında aktarabilirsiniz. Dışarı aktarılabilir değilse, ardından bu yalnızca runbook veya DSC yapılandırması içinde imzalamak için kullanılabilir. Azure Otomasyonu sağlayıcı sağlamak için sertifika gerektirir: **Microsoft Gelişmiş RSA ve AES şifreleme sağlayıcısı**.

### <a name="to-create-a-new-certificate-with-the-azure-portal"></a>Azure portalı ile yeni bir sertifika oluşturmak için

1. Otomasyon hesabınızdan tıklatın **varlıklar** açmak için kutucuğa **varlıklar** dikey.
1. Tıklatın **sertifikaları** açmak için kutucuğa **sertifikaları** dikey.
1. Tıklatın **bir sertifika eklemek** dikey pencerenin üstündeki.
1. Sertifika için bir ad yazın **adı** kutusu.
1. Bir .cer veya .pfx dosyasını bulmak için tıklayın **bir dosya seçin** altında **bir sertifika dosyası karşıya**. Bir .pfx dosyası seçerseniz, bir parola ve onu dışarı aktarılmasına izin verilip verilmeyeceğini belirtin.
1. Tıklatın **oluşturma** yeni sertifika varlığı kaydetmek için.

### <a name="to-create-a-new-certificate-with-windows-powershell"></a>Windows PowerShell ile yeni bir sertifika oluşturmak için

Aşağıdaki örnek, yeni bir Otomasyon sertifikası oluşturun ve dışarı aktarılabilir olarak işaretleyin gösterilmiştir. Bu, var olan bir .pfx dosyasını içeri aktarır.

```powershell-interactive
$certName = 'MyCertificate'
$certPath = '.\MyCert.pfx'
$certPwd = ConvertTo-SecureString -String 'P@$$w0rd' -AsPlainText -Force
$ResourceGroup = "ResourceGroup01"

New-AzureRmAutomationCertificate -AutomationAccountName "MyAutomationAccount" -Name $certName -Path $certPath –Password $certPwd -Exportable -ResourceGroupName $ResourceGroup
```

## <a name="using-a-certificate"></a>Bir sertifika kullanma

Bir sertifika kullanmak için **Get-AutomationCertificate** etkinlik. Kullanamazsınız [Get-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603765.aspx) sertifika varlığı ama sertifikanın kendisi hakkında bilgi döndürdüğünden cmdlet'i.

### <a name="textual-runbook-sample"></a>Metin biçiminde runbook örneği

Aşağıdaki örnek kod, bir runbook'ta bir bulut hizmeti için bir sertifika eklemek gösterilmiştir. Bu örnekte, parolayı bir şifrelenmiş Otomasyon değişkeninden alınır.

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

Aşağıdaki resimde bir grafik runbook'ta bir sertifika kullanarak bir örnek gösterilmektedir. Bu sertifika bir metinsel runbook'tan bir bulut hizmetine eklemek için önceki örnekte aynıdır.

![Örnek grafik yazma ](media/automation-certificates/graphical-runbook-add-certificate.png)

### <a name="python2-sample"></a>Python2 örnek
Aşağıdaki örnek, sertifikaları Python2 runbook'lardaki erişim gösterilmektedir.

```python
# get a reference to the Azure Automation certificate
cert = automationassets.get_automation_certificate("AzureRunAsCertificate")

# returns the binary cert content  
print cert 
```

## <a name="next-steps"></a>Sonraki adımlar

- Runbook'unuzda gerçekleştirmek üzere tasarlanmıştır etkinliklerin mantıksal akışını denetlemek için bağlantılar ile çalışma hakkında daha fazla bilgi için bkz: [grafik yazma içinde bağlantılar](automation-graphical-authoring-intro.md#links-and-workflow). 
