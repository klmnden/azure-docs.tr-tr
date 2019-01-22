---
title: Azure automation'da Azure modüllerini güncelleştir
description: Bu makalede, Azure automation'da varsayılan tarafından sağlanan genel Azure PowerShell modüllerini nasıl şimdi güncelleştirebilirsiniz açıklanır.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 12/04/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: c0636a3e1fa50f90c68393aea910f36d38d8eaf5
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54437276"
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a>Azure automation'da Azure PowerShell modüllerini güncelleştirme

En yaygın Azure PowerShell modüllerinin her Otomasyon hesabında varsayılan olarak sağlanır. Azure ekibi, Azure modüllerini düzenli olarak güncelleştirir. Otomasyon hesabınızı portaldan yeni sürümler kullanılabilir olduğunda hesabı modülleri güncelleştirmek için bir yol sunulur.

> [!NOTE]
> Yeni [Az Azure PowerShell Modülü](/powershell/azure/new-azureps-module-az?view=azurermps-6.13.0) Azure Automation'da desteklenmez.

Modüller düzenli olarak ürün grubu tarafından güncelleştirildiğinden, değişiklikleri dahil edilen cmdlet'ler ile ortaya çıkabilir. Bu eylem, bir parametre yeniden adlandırma ya da tamamen bir cmdlet kullanım dışı bırakılıyor gibi değişiklik türüne bağlı olarak runbook'larınızı olumsuz yönde etkileyebilir. Runbook'larınızı ve bunlar otomatikleştirmek işlemleri etkilemesini önlemek için test edin ve devam etmeden önce doğrulayın. Hedeflenen bu amaç için adanmış bir Otomasyon hesabınız yoksa, böylece birçok farklı senaryo runbook'larınızı geliştirilmesi sırasında test oluşturma göz önünde bulundurun. Bu testi PowerShell modüllerini güncelleştirme gibi yinelemeli değişiklikleri içermelidir. Betiklerinizi yerel olarak geliştirme yapıyorsanız, emin olmak için testi aynı sonuçları alırsınız, Otomasyon hesabınızda kullandığınız aynı modülü yerel olarak sürümlerde önerilir. Sonuçları doğrulanır ve gereken değişiklikleri uyguladığınız sonra değişiklikleri üretime taşıyabilirsiniz.

> [!NOTE]
> Yeni bir Otomasyon hesabı, en yeni modülleri içermeyebilir.

## <a name="updating-azure-modules"></a>Azure modülleri güncelleştiriliyor

1. Otomasyon hesabınızın modülleri sayfasında adlı bir seçenek yoktur **Azure modüllerini güncelleştirme**. Her zaman etkindir.<br><br> ![Modüller sayfasında seçenek Azure modüllerini güncelleştir](media/automation-update-azure-modules/automation-update-azure-modules-option.png)

  > [!NOTE]
  > Bir test emin olmak için Otomasyon hesabı güncelleştirmeniz önerilir, Azure modülleri güncelleştirmeden önce mevcut komut dosyalarınızı Azure modüllerinizi güncelleştirmeden önce beklendiği gibi çalışır.
  >
  > **Azure modüllerini güncelleştirme** düğmesi kullanılabilir yalnızca genel bulutta. mevcut değildir [bağımsız bölgeler](https://azure.microsoft.com/global-infrastructure/). Lütfen bkz [modüllerinizi güncelleştirmesi için alternatif yöntemler](#alternative-ways-to-update-your-modules) bölümü daha fazla bilgi edinin.

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

## <a name="alternative-ways-to-update-your-modules"></a>Modüllerinizi güncelleştirmesi için alternatif yöntemler

Belirtildiği gibi **Azure modüllerini güncelleştirme** düğmesi bağımsız bulutlarda kullanılamaz, yalnızca Azure genel bulutunda kullanılabilir. Azure PowerShell modülleri PowerShell Galerisi'nden en son sürümünü şu anda bu bulutlara dağıtılan Resource Manager kaynaklarını ile çalışmayabilir Bunun nedeni, budur.

İçeri aktarma ve çalıştırma [güncelleştirme AzureModule.ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AzureModule.ps1) Otomasyon hesabınızı Azure modüllerini güncelleştirme girişimi için runbook. Aynı anda tüm Azure modüllerini güncelleştirmek için genellikle iyi bir fikirdir. Ancak bu işlem olmayan Galeriden almaya çalıştığınız sürümleri, geçerli hedef Azure ortamına dağıtılan Azure hizmetleriyle uyumlu başarısız olabilir. Bu modüller uyumlu sürümlerini runbook parametrelerinde belirtilen doğrulamanızı gerektirebilir.

Kullanım `AzureRmEnvironment` parametresini kullanarak doğru ortamı runbook'una geçirebilirsiniz.  Kabul edilebilir değerler **AzureCloud**, **AzureChinaCloud**, **AzureGermanCloud**, ve **AzureUSGovernment**. Bu değerleri kullanarak alınabilir `Get-AzureRmEnvironment | select Name`. Bu parametre için bir değer geçirmezseniz runbook, Azure ortak bulutuna varsayılacaktır **AzureCloud**

Belirli bir Azure PowerShell modülü sürüm yerine en son kullanılabilir PowerShell galerisinde kullanmak istiyorsanız, bu sürümleri için isteğe bağlı geçirmek `ModuleVersionOverrides` parametresinin **güncelleştirme AzureModule** runbook. Örnekler için bkz [güncelleştirme AzureModule.ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AzureModule.ps1) runbook. İçinde belirtilmeyen azure PowerShell modüllerini `ModuleVersionOverrides` parametresi ve PowerShell Galerisi'ndeki son modülü sürümleriyle güncelleştirilir. Nothing olarak geçirirseniz `ModuleVersionOverrides` parametresi, tüm modülleri en son modülü PowerShell Galerisi sürümlerinde güncelleştirilir. Bu aynı, davranıştır **Azure modüllerini güncelleştirme** düğmesi.

## <a name="next-steps"></a>Sonraki adımlar

* Tümleştirme modülleri ve daha fazla Otomasyon diğer sistemleri, hizmetler veya çözümler ile tümleştirmek için özel modüller oluşturma hakkında daha fazla bilgi için bkz: [tümleştirme modülleri](automation-integration-modules.md).


