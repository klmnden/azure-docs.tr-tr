---
title: Azure automation'da kimlik bilgisi varlıkları
description: Azure automation'da kimlik bilgisi varlıkları runbook veya DSC yapılandırması tarafından erişilen kaynaklar için kimlik doğrulaması yapmada kullanılan güvenlik kimlik bilgilerini içerir. Bu makalede kimlik bilgisi varlıkları oluşturma ve bunları runbook'tan veya DSC yapılandırmasından içinde kullanın.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: bobbytreed
ms.author: robreed
ms.date: 04/12/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 44bc49d10c492822c1b5d30ad5794ac2522cb918
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67478146"
---
# <a name="credential-assets-in-azure-automation"></a>Azure automation'da kimlik bilgisi varlıkları

Bir Otomasyon kimlik bilgisi varlığı, bir kullanıcı adı ve parola gibi güvenlik kimlik bilgilerini içeren bir nesne içerir. Kullanıcı adı ve parola için bazı uygulama veya hizmet kimlik doğrulaması gerektiren sağlamak PSCredential nesnesinin ayıklamak veya kimlik doğrulaması için bir PSCredential nesnesi kabul cmdlet'ler Runbook'larda ve DSC yapılandırmaları kullanabilir. Kimlik bilgileri özellikleri Azure Automation'da güvenli bir şekilde depolanır ve runbook veya DSC yapılandırması ile erişilebilen [Get-AutomationPSCredential](#activities) etkinlik.

[!INCLUDE [gdpr-dsr-and-stp-note.md](../../../includes/gdpr-dsr-and-stp-note.md)]

> [!NOTE]
> Azure automation'da güvenli varlıkların kimlik bilgileri, sertifikalar, bağlantılar ve şifrelenmiş değişkenler içerir. Bu varlıklar şifrelenir ve her Otomasyon hesabı için oluşturulan benzersiz bir anahtar kullanarak Azure automation'da depolanır. Bu anahtar, anahtar Kasası'nda depolanır. Güvenli bir varlık depolamadan önce anahtarı Key Vault'tan yüklenir ve sonra varlık şifrelemek için kullanılır.

## <a name="azure-classic-powershell-cmdlets"></a>Azure Klasik PowerShell cmdlet'leri

Aşağıdaki tabloda yer alan cmdlet'ler Windows PowerShell ile Otomasyon kimlik bilgisi varlıkları oluşturmak ve yönetmek için kullanılır.  Bunlar parçası olarak gönderilen [Azure PowerShell Modülü](/powershell/azure/overview), olduğu Automation runbook'ları ve DSC yapılandırmaları için kullanılabilir.

| Cmdlet'ler | Açıklama |
|:--- |:--- |
| [Get-AzureAutomationCredential](/powershell/module/servicemanagement/azure/get-azureautomationcredential) |Bir kimlik bilgisi varlığı hakkındaki bilgileri alır. Yalnızca kimlik almak gelen **Get-AutomationPSCredential** etkinlik. |
| [New-AzureAutomationCredential](/powershell/module/servicemanagement/azure/new-azureautomationcredential) |Yeni bir Otomasyon kimlik bilgisi oluşturur. |
| [Remove-AzureAutomationCredential](/powershell/module/servicemanagement/azure/new-azureautomationcredential) |Otomasyon kimlik bilgileri kaldırır. |
| [Set-AzureAutomationCredential](/powershell/module/servicemanagement/azure/new-azureautomationcredential) |Var olan Otomasyon kimlik bilgileri özelliklerini ayarlar. |

## <a name="azurerm-powershell-cmdlets"></a>AzureRM PowerShell cmdlet'leri

AzureRM için aşağıdaki tabloda yer alan cmdlet'ler Windows PowerShell ile Otomasyon kimlik bilgisi varlıkları oluşturmak ve yönetmek için kullanılır.  Bunlar parçası olarak gönderilen [AzureRM.Automation Modülü](/powershell/azure/overview), olduğu Automation runbook'ları ve DSC yapılandırmaları için kullanılabilir.

| Cmdlet'ler | Açıklama |
|:--- |:--- |
| [Get-AzureRmAutomationCredential](/powershell/module/azurerm.automation/get-azurermautomationcredential) |Bir kimlik bilgisi varlığı hakkındaki bilgileri alır. Bu, bir PSCredential nesnesi döndürmez.  |
| [New-AzureRmAutomationCredential](/powershell/module/azurerm.automation/new-azurermautomationcredential) |Yeni bir Otomasyon kimlik bilgisi oluşturur. |
| [Remove-AzureRmAutomationCredential](/powershell/module/azurerm.automation/remove-azurermautomationcredential) |Otomasyon kimlik bilgileri kaldırır. |
| [Set-AzureRmAutomationCredential](/powershell/module/azurerm.automation/set-azurermautomationcredential) |Var olan Otomasyon kimlik bilgileri özelliklerini ayarlar. |

## <a name="activities"></a>Etkinlikler

Aşağıdaki tablodaki etkinlikler bir runbook ve DSC yapılandırmaları kimlik bilgilerini erişmek için kullanılır.

| Etkinlikler | Açıklama |
|:--- |:--- |
| Get-AutomationPSCredential |Runbook'tan veya DSC yapılandırmasından içinde kullanmak için bir kimlik bilgisi alır. Döndürür bir [System.Management.Automation.PSCredential](/dotnet/api/system.management.automation.pscredential) nesne. |

> [!NOTE]
> Değişkenleri kullanmaktan kaçınmanız gerekir – Name parametresinde, Get-AutomationPSCredential olduğundan bu runbook veya DSC yapılandırmaları arasındaki bağımlılıkları genişlemesiyle ve kimlik bilgisi varlıkları tasarım zamanında.

## <a name="python2-functions"></a>Python2 işlevleri

Aşağıdaki tabloda işlevi Python2 runbook kimlik bilgilerine erişmek için kullanılır.

| İşlev | Açıklama |
|:---|:---|
| automationassets.get_automation_credential | Bir kimlik bilgisi varlığı hakkındaki bilgileri alır. |

> [!NOTE]
> Varlık işlevlerine erişmek için Python runbook'unuzu üst kısmında "automationassets" modülü içeri aktarmalısınız.

## <a name="creating-a-new-credential-asset"></a>Yeni bir kimlik bilgisi varlığı oluşturma

### <a name="to-create-a-new-credential-asset-with-the-azure-portal"></a>Azure portalı ile yeni bir kimlik bilgisi varlığı oluşturmak için

1. Otomasyon hesabınızdan seçin **kimlik bilgilerini** altında **paylaşılan kaynakları**.
1. Tıklayın **+ kimlik bilgisi Ekle**.
1. Formu tamamlayıp tıklayın **Oluştur** yeni kimlik bilgilerini kaydetmek için.

> [!NOTE]
> Azure Otomasyonu kullanmak için çok faktörlü kimlik doğrulaması kullanan kullanıcı hesapları desteklenmez.

### <a name="to-create-a-new-credential-asset-with-windows-powershell"></a>Windows PowerShell ile yeni bir kimlik bilgisi varlığı oluşturmak için

Aşağıdaki örnek komutlar, yeni bir Otomasyon kimlik bilgisi oluşturma işlemini gösterir. Bir PSCredential nesnesi adı ve parola ile ilk oluşturulur ve ardından kimlik bilgileri varlığı oluşturmak için kullanılır. Alternatif olarak, kullanabileceğinizi **Get-Credential** cmdlet'ini bir adı ve parola yazın istenir.

```powershell
$user = "MyDomain\MyUser"
$pw = ConvertTo-SecureString "PassWord!" -AsPlainText -Force
$cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $user, $pw
New-AzureAutomationCredential -AutomationAccountName "MyAutomationAccount" -Name "MyCredential" -Value $cred
```

## <a name="using-a-powershell-credential"></a>Bir PowerShell kimlik bilgisi kullanma

Bir runbook'tan veya DSC yapılandırmasından ile kimlik bilgisi varlığı almak **Get-AutomationPSCredential** etkinlik. Bu döndürür bir [PSCredential nesnesinin](/dotnet/api/system.management.automation.pscredential) bir etkinlik veya cmdlet'i parametre gerektiren bir PSCredential ile kullanabilirsiniz. Tek tek kullanmak için kimlik bilgisi nesnesinin özelliklerini de alabilirsiniz. Nesnesi için kullanıcı adı ve parolayı güvenli bir özelliğe sahiptir ve kullanabileceğiniz **GetNetworkCredential** döndürülecek yöntemi bir [NetworkCredential](/dotnet/api/system.net.networkcredential) nesnesini güvenli olmayan bir sürümü sağlar parola.

> [!NOTE]
> **Get-AzureRmAutomationCredential** döndürmez bir **PSCredential** kimlik doğrulaması için kullanılabilir. Yalnızca kimlik bilgileri hakkında bilgi sağlar. Bir runbook'ta kimlik bilgileri kullanmanız gerekiyorsa kullanmalısınız **Get-AutomationPSCredential** alınacak **PSCredential** nesne.

### <a name="textual-runbook-sample"></a>Metinsel runbook örneği

Aşağıdaki örnek komutlarda PowerShell kimlik bilgilerinin bir runbook'ta nasıl kullanılacağı gösterilmektedir. Bu örnekte, kimlik bilgileri alınır ve kendi kullanıcı adı ve parola değişkenine atanır.

```azurepowershell
$myCredential = Get-AutomationPSCredential -Name 'MyCredential'
$userName = $myCredential.UserName
$securePassword = $myCredential.Password
$password = $myCredential.GetNetworkCredential().Password
```

Azure ile kimlik doğrulaması için bir kimlik bilgisi kullanabilirsiniz [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount). Çoğu durumda, kullanmanız gereken bir [Run As hesabı](../manage-runas-account.md) ve bunu ile alma [Get-AutomationConnection](../automation-connections.md).

```azurepowershell
$myCred = Get-AutomationPSCredential -Name 'MyCredential'
$userName = $myCred.UserName
$securePassword = $myCred.Password
$password = $myCred.GetNetworkCredential().Password

$myPsCred = New-Object System.Management.Automation.PSCredential ($userName,$password)

Connect-AzureRmAccount -Credential $myPsCred
```

### <a name="graphical-runbook-sample"></a>Grafik runbook örneği

Eklediğiniz bir **Get-AutomationPSCredential** sağ tıklayarak grafik düzenleyicisini Kitaplık bölmesinde kimlik bilgisi ve seçerek bir grafik runbook etkinliğine **tuvale Ekle**.

![Kimlik bilgisi tuvale Ekle](../media/credentials/credential-add-canvas.png)

Aşağıdaki görüntüde, grafik bir runbook'ta kimlik bilgisini kullanarak bir örnek gösterilmektedir.  Bu durumda, bunu açıklandığı gibi Azure kaynakları için bir runbook için kimlik doğrulaması sağlamak için kullanılan [Azure AD kullanıcı hesabıyla Runbook kimlik doğrulaması](../automation-create-aduser-account.md).  İlk etkinlik, Azure aboneliğinize erişimi olan kimlik bilgisini alır.  **Add-AzureAccount** etkinlik sonra bu kimlik bilgisi bundan sonra gelen tüm etkinlikler için kimlik doğrulaması sağlamak için kullanır.  A [ardışık düzen bağlantısına](../automation-graphical-authoring-intro.md#links-and-workflow) beri işte **Get-AutomationPSCredential** tek bir nesne bekliyor.  

![Kimlik bilgisi tuvale Ekle](../media/credentials/get-credential.png)

## <a name="using-a-powershell-credential-in-dsc"></a>DSC PowerShell kimlik bilgisi kullanma

DSC yapılandırmaları Azure Automation kimlik bilgisi varlıkları kullanarak başvurabilirsiniz ancak **Get-AutomationPSCredential**, kimlik bilgisi varlıkları de geçirilebilir içindeki parametreler aracılığıyla istiyorsanız. Daha fazla bilgi için [Azure Automation DSC yapılandırmaları derleme](../automation-dsc-compile.md#credential-assets).

## <a name="using-credentials-in-python2"></a>Python2 içinde kimlik bilgilerini kullanarak

Aşağıdaki örnek Python2 runbook'larında kimlik bilgilerine erişirken bir örnek gösterir.

```python
import automationassets
from automationassets import AutomationAssetNotFound

# get a credential
cred = automationassets.get_automation_credential("credtest")
print cred["username"]
print cred["password"]
```

## <a name="next-steps"></a>Sonraki adımlar

* Grafik yazma içinde bağlantılar hakkında daha fazla bilgi için bkz: [grafik yazma içindeki bağlantılar](../automation-graphical-authoring-intro.md#links-and-workflow)
* Otomasyon ile farklı kimlik doğrulama yöntemleri anlamak için bkz: [Azure Automation güvenliği](../automation-security-overview.md)
* Grafik runbook'ları kullanmaya başlamak için bkz. [İlk grafik runbook uygulamam](../automation-first-runbook-graphical.md)
* PowerShell iş akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell iş akışı runbook uygulamam](../automation-first-runbook-textual.md)
* Python2 runbook'larını kullanmaya başlamak için bkz: [ilk Python2 runbook'um](../automation-first-runbook-textual-python2.md) 
