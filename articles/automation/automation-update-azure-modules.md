---
title: Azure automation'da Azure modüllerini güncelleştir
description: Bu makalede, Azure automation'da varsayılan tarafından sağlanan genel Azure PowerShell modüllerini nasıl şimdi güncelleştirebilirsiniz açıklanır.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/11/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 03174e6336589f8aa49a7fc7197da1301ff54400
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61304324"
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a>Azure automation'da Azure PowerShell modüllerini güncelleştirme

Azure modüllerini Otomasyon hesabınızı güncelleştirmek için bu, artık kullanmak önerilir [güncelleştirme Azure modülleri runbook](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update), şimdi açık kaynak olan. Ayrıca, kullanmaya devam edebilirsiniz **Azure modüllerini Güncelleştir** portalında, Azure modüllerini Güncelleştir düğmesi. Açık kaynak runbook kullanma hakkında daha fazla bilgi edinmek için [açık kaynak runbook ile Azure modüllerini güncelleştirme](#open-source).

En yaygın Azure PowerShell modüllerinin her Otomasyon hesabında varsayılan olarak sağlanır. Azure ekibi, Azure modüllerini düzenli olarak güncelleştirir. Otomasyon hesabınızı portaldan yeni sürümler kullanılabilir olduğunda hesabı modülleri güncelleştirmek için bir yol sunulur.

Modüller düzenli olarak ürün grubu tarafından güncelleştirildiğinden, değişiklikleri dahil edilen cmdlet'ler ile ortaya çıkabilir. Bu eylem, bir parametre yeniden adlandırma ya da tamamen bir cmdlet kullanım dışı bırakılıyor gibi değişiklik türüne bağlı olarak runbook'larınızı olumsuz yönde etkileyebilir.

Runbook'larınızı ve bunlar otomatikleştirmek işlemleri etkilemesini önlemek için test edin ve devam etmeden önce doğrulayın. Hedeflenen bu amaç için adanmış bir Otomasyon hesabınız yoksa, böylece birçok farklı senaryo runbook'larınızı geliştirilmesi sırasında test oluşturma göz önünde bulundurun. Bu testi PowerShell modüllerini güncelleştirme gibi yinelemeli değişiklikleri içermelidir. 

Betiklerinizi yerel olarak geliştirme yapıyorsanız, emin olmak için testi aynı sonuçları alırsınız, Otomasyon hesabınızda kullandığınız aynı modülü yerel olarak sürümlerde önerilir. Sonuçları doğrulanır ve gereken değişiklikleri uyguladığınız sonra değişiklikleri üretime taşıyabilirsiniz.

> [!NOTE]
> Yeni bir Otomasyon hesabı, en yeni modülleri içermeyebilir.

## <a name="open-source"></a>Açık kaynaklı runbook ile Azure modüllerini güncelleştirme

Kullanmaya başlamak için **güncelleştirme AutomationAzureModulesForAccount** runbook, Azure modüllerini güncelleştirmek için buradan indirin [güncelleştirme Azure modülleri runbook depo](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) GitHub üzerinde. Ardından, Otomasyon hesabına aktarın veya bir betik olarak çalıştırmak. Bunu yapmak yönergeler bulunabilir [güncelleştirme Azure modülleri runbook depo](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update).

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Bu işlem, Azure modüllerini güncelleştir kullanırken dikkate almanız gereken bazı noktalar şunlardır:

* Özgün adıyla bu runbook'u içeri aktarırsanız `Update-AutomationAzureModulesForAccount`, bu ada sahip iç runbook geçersiz kılar. Sonuç olarak, içeri aktarılan runbook'un ne zaman çalıştırılır **Azure modüllerini güncelleştirme** düğmesi atıldığında veya ne zaman bu runbook çağrıldığında bu Otomasyon hesabı için Azure Resource Manager API'si aracılığıyla doğrudan.

* Bu runbook, yalnızca güncelleştirme destekler **Azure** ve **AzureRm** şu anda modüller. [Azure PowerShell Az modülleri](/powershell/azure/new-azureps-module-az) Automation hesaplarında desteklenir, ancak bu runbook'la güncelleştirilemiyor.

* Bu runbook içeren Az modülleri Automation hesapları başlatma kaçının.

* Bu runbook başlatmadan önce Otomasyon hesabınızın sahip olduğundan emin olun bir [Azure farklı çalıştır hesabı kimlik bilgisi](manage-runas-account.md) oluşturulur.

* Bir runbook yerine normal bir PowerShell Betiği olarak bu kodu kullanabilirsiniz: yalnızca Azure kullanarak oturum açtığınız [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) ilk komutunu ve ardından geçirmek `-Login $false` komut dosyası.

* Bu runbook, bağımsız bulutlarda kullanmak için `AzureRmEnvironment` parametresini kullanarak doğru ortamı runbook'una geçirebilirsiniz.  Kabul edilebilir değerler **AzureCloud**, **AzureChinaCloud**, **AzureGermanCloud**, ve **AzureUSGovernment**. Bu değerleri kullanarak alınabilir `Get-AzureRmEnvironment | select Name`. Bu parametre için bir değer geçirmezseniz runbook, Azure ortak bulutuna varsayılacaktır **AzureCloud**

* Belirli bir Azure PowerShell modülü sürüm yerine en son kullanılabilir PowerShell galerisinde kullanmak istiyorsanız, bu sürümleri için isteğe bağlı geçirmek `ModuleVersionOverrides` parametresinin **güncelleştirme-AutomationAzureModulesForAccount**runbook. Örnekler için bkz [güncelleştirme AutomationAzureModulesForAccount.ps1](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update/blob/master/Update-AutomationAzureModulesForAccount.ps1
) runbook. İçinde belirtilmeyen azure PowerShell modüllerini `ModuleVersionOverrides` parametresi ve PowerShell Galerisi'ndeki son modülü sürümleriyle güncelleştirilir. Nothing olarak geçirirseniz `ModuleVersionOverrides` parametresi, tüm modülleri en son modülü PowerShell Galerisi sürümlerinde güncelleştirilir. Bu aynı, davranıştır **Azure modüllerini güncelleştirme** düğmesi.

## <a name="update-azure-modules-in-the-azure-portal"></a>Azure portalında Azure modüllerini güncelleştir

1. Otomasyon hesabınızın modülleri sayfasında adlı bir seçenek yoktur **Azure modüllerini güncelleştirme**. Her zaman etkindir.<br><br> ![Modüller sayfasında seçenek Azure modüllerini güncelleştir](media/automation-update-azure-modules/automation-update-azure-modules-option.png)

   > [!NOTE]
   > Bir test emin olmak için Otomasyon hesabı güncelleştirmeniz önerilir, Azure modülleri güncelleştirmeden önce mevcut komut dosyalarınızı Azure modüllerinizi güncelleştirmeden önce beklendiği gibi çalışır.
   >
   > **Azure modüllerini güncelleştirme** düğmesi kullanılabilir yalnızca genel bulutta. mevcut değildir [bağımsız bölgeler](https://azure.microsoft.com/global-infrastructure/). Lütfen kullanın **güncelleştirme AutomationAzureModulesForAccount** runbook, Azure modüllerini güncelleştir. Buradan indirebileceğiniz [güncelleştirme Azure modülleri runbook depo](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update). Açık kaynak runbook kullanma hakkında daha fazla bilgi edinmek için [açık kaynak runbook ile Azure modüllerini güncelleştirme](#open-source).

2. Tıklayın **Azure modüllerini güncelleştirme**, devam etmek isteyip istemediğinizi soran bir onay bildirimi gösterilir.<br><br> ![Bildirim Azure modüllerini güncelleştir](media/automation-update-azure-modules/automation-update-azure-modules-popup.png)

3. Tıklayın **Evet** ve modül güncelleştirme işlemi başlar. Güncelleştirme işlemi aşağıdaki modüllerini güncelleştirmek için yaklaşık 15-20 dakika alır:

   * Azure
   * Azure Depolama
   * AzureRm.Automation
   * AzureRm.Compute
   * AzureRm.Profile
   * AzureRm.Resources
   * AzureRm.Sql
   * AzureRm.Storage

     Modül zaten güncel olduğundan, işlem birkaç saniye içinde tamamlanır. Güncelleştirme işlemi tamamlandığında bildirim alırsınız.<br><br> ![Azure modüllerini güncelleştirme durumunu güncelleştirme](media/automation-update-azure-modules/automation-update-azure-modules-updatestatus.png)

     .NET core AzureRm modülleri (AzureRm.*. Çekirdek) Azure Automation'da desteklenmez ve içeri aktarılamıyor.

> [!NOTE]
> Yeni bir zamanlanmış iş çalıştırıldığında azure Otomasyonu, Otomasyon hesabınızda en son modüllerini kullanır.  

Bu Azure PowerShell modülleri cmdlet'leri, runbook'ları kullanırsanız, her ay bu güncelleştirme işlemini çalıştırmak için veya bu nedenle en son modülleri sahip olduğunuzdan emin olmak için istiyorsunuz. Azure Otomasyonu kullanan `AzureRunAsConnection` modülleri güncelleştirirken kimliğini doğrulamak için bağlantı. Hizmet sorumlusu süresi doldu veya abonelik düzeyinde artık modülü güncelleştirme başarısız olur.

## <a name="known-issues"></a>Bilinen sorunlar

0 ile başlayan sayısal bir ada sahip bir kaynak grubu içinde bir Otomasyon hesabı AzureRM modülleri güncelleştirme ile ilgili bilinen bir sorun yoktur. Otomasyon hesabınızda, Azure modüllerini güncelleştir için alfasayısal bir ada sahip bir kaynak grubunda olmalıdır. 0 ile başlayan sayısal adlara sahip kaynak gruplarını AzureRM modülleri şu anda güncelleştiremiyor.

## <a name="next-steps"></a>Sonraki adımlar

Açık kaynak ziyaret [güncelleştirme Azure modülleri runbook](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) hakkında daha fazla bilgi edinmek için.
