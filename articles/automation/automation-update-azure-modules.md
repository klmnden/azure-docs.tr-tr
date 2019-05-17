---
title: Azure automation'da Azure modüllerini güncelleştir
description: Bu makalede, Azure automation'da varsayılan tarafından sağlanan genel Azure PowerShell modüllerini nasıl şimdi güncelleştirebilirsiniz açıklanır.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 05/13/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: a2f55891ddd383ea15da499495909b56ffb0e06d
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65786144"
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a>Azure automation'da Azure PowerShell modüllerini güncelleştirme

Otomasyon hesabınızdaki kullanması gereken Azure modüllerini güncelleştirmek için [güncelleştirme Azure modülleri runbook](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update), açık kaynak kullanılabilen. Kullanmaya başlamak için **güncelleştirme AutomationAzureModulesForAccount** runbook, Azure modüllerini güncelleştirmek için buradan indirin [güncelleştirme Azure modülleri runbook depo](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) GitHub üzerinde. Ardından, Otomasyon hesabına aktarın veya bir betik olarak çalıştırmak. Bunu yapmak yönergeler bulunabilir [güncelleştirme Azure modülleri runbook depo](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update).

En yaygın AzureRM PowerShell modüllerine her Otomasyon hesabında varsayılan olarak sağlanır. Azure ekibi Azure modüllerini düzenli olarak güncelleştirir, bu nedenle güncel tutmak için kullanmak istediğiniz [güncelleştirme AutomationAzureModulesForAccount](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) runbook Otomasyon hesaplarınız modülleri güncelleştirilecek.

Modüller düzenli olarak ürün grubu tarafından güncelleştirildiğinden, değişiklikleri dahil edilen cmdlet'ler ile ortaya çıkabilir. Bu eylem, bir parametre yeniden adlandırma ya da tamamen bir cmdlet kullanım dışı bırakılıyor gibi değişiklik türüne bağlı olarak runbook'larınızı olumsuz yönde etkileyebilir.

Runbook'larınızı ve bunlar otomatikleştirmek işlemleri etkilemesini önlemek için test edin ve devam etmeden önce doğrulayın. Hedeflenen bu amaç için adanmış bir Otomasyon hesabınız yoksa, böylece birçok farklı senaryo runbook'larınızı geliştirilmesi sırasında test oluşturma göz önünde bulundurun. Bu testi PowerShell modüllerini güncelleştirme gibi yinelemeli değişiklikleri içermelidir.

Betiklerinizi yerel olarak geliştirme yapıyorsanız, emin olmak için testi aynı sonuçları alırsınız, Otomasyon hesabınızda kullandığınız aynı modülü yerel olarak sürümlerde önerilir. Sonuçları doğrulanır ve gereken değişiklikleri uyguladığınız sonra değişiklikleri üretime taşıyabilirsiniz.

> [!NOTE]
> Yeni bir Otomasyon hesabı, en yeni modülleri içermeyebilir.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Bu işlem, Azure modüllerini güncelleştir kullanırken dikkate almanız gereken bazı noktalar şunlardır:

* Bu runbook, yalnızca güncelleştirme destekler **Azure** ve **AzureRm** şu anda modüller. [Azure PowerShell Az modülleri](/powershell/azure/new-azureps-module-az) Automation hesaplarında desteklenir, ancak bu runbook'la güncelleştirilemiyor. Kullanırken dikkate almanız gereken önemli faktörler vardır `Az` modüller Otomasyon hesabınızdaki daha fazla bilgi edinmek için bkz: [kullanarak Az modüller Otomasyon hesabınızdaki](az-modules.md).

* Bu runbook içeren Az modülleri Automation hesapları başlatma kaçının.

* Bu runbook başlatmadan önce Otomasyon hesabınızın sahip olduğundan emin olun bir [Azure farklı çalıştır hesabı kimlik bilgisi](manage-runas-account.md) oluşturulur.

* Bir runbook yerine normal bir PowerShell Betiği olarak bu kodu kullanabilirsiniz: Azure'ı kullanmaya hemen oturum [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) ilk komutunu ve ardından geçirmek `-Login $false` komut dosyası.

* Bu runbook, bağımsız bulutlarda kullanmak için `AzureRmEnvironment` parametresini kullanarak doğru ortamı runbook'una geçirebilirsiniz.  Kabul edilebilir değerler **AzureCloud**, **AzureChinaCloud**, **AzureGermanCloud**, ve **AzureUSGovernment**. Bu değerleri kullanarak alınabilir `Get-AzureRmEnvironment | select Name`. Bu parametre için bir değer geçirmezseniz runbook, Azure ortak bulutuna varsayılacaktır **AzureCloud**

* Belirli bir Azure PowerShell modülü sürüm yerine en son kullanılabilir PowerShell galerisinde kullanmak istiyorsanız, bu sürümleri için isteğe bağlı geçirmek `ModuleVersionOverrides` parametresinin **güncelleştirme-AutomationAzureModulesForAccount**runbook. Örnekler için bkz [güncelleştirme AutomationAzureModulesForAccount.ps1](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update/blob/master/Update-AutomationAzureModulesForAccount.ps1
) runbook. İçinde belirtilmeyen azure PowerShell modüllerini `ModuleVersionOverrides` parametresi ve PowerShell Galerisi'ndeki son modülü sürümleriyle güncelleştirilir. Nothing olarak geçirirseniz `ModuleVersionOverrides` parametresi, tüm modülleri en son modülü PowerShell Galerisi sürümlerinde güncelleştirilir. Bu aynı, davranıştır **Azure modüllerini güncelleştirme** düğmesi.

## <a name="known-issues"></a>Bilinen sorunlar

0 ile başlayan sayısal bir ada sahip bir kaynak grubu içinde bir Otomasyon hesabı AzureRM modülleri güncelleştirme ile ilgili bilinen bir sorun yoktur. Otomasyon hesabınızda, Azure modüllerini güncelleştir için alfasayısal bir ada sahip bir kaynak grubunda olmalıdır. 0 ile başlayan sayısal adlara sahip kaynak gruplarını AzureRM modülleri şu anda güncelleştiremiyor.

## <a name="next-steps"></a>Sonraki adımlar

Açık kaynak ziyaret [güncelleştirme Azure modülleri runbook](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) hakkında daha fazla bilgi edinmek için.
