---
title: Azure automation'da Azure modüllerini güncelleştir
description: Bu makalede, Azure automation'da varsayılan tarafından sağlanan genel Azure PowerShell modüllerini nasıl şimdi güncelleştirebilirsiniz açıklanır.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: fbb57753117f3c60010fe910616b8d0af5178360
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47434832"
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a>Azure automation'da Azure PowerShell modüllerini güncelleştirme

En yaygın Azure PowerShell modüllerinin her Otomasyon hesabında varsayılan olarak sağlanır. Azure ekibi otomasyon hesabında hesap modülleri portaldan yeni sürümler kullanılabilir olduğunda güncelleştirilmesine olanak sağlanır. Azure modüllerini düzenli olarak güncelleştirir.

Modüller düzenli olarak ürün grubu tarafından güncelleştirildiğinden, değişiklikleri gibi bir parametre yeniden adlandırma ya da tamamen bir cmdlet kullanım dışı bırakılıyor değişiklik türüne bağlı olarak runbook'larınızı olumsuz etkileyebilir dahil edilen cmdlet'ler ortaya çıkabilir. Runbook'larınızı ve bunlar otomatikleştirmek işlemleri etkilemesini önlemek için test edin ve devam etmeden önce doğrulanması önerilir. Hedeflenen bu amaç için adanmış bir Otomasyon hesabı yoksa, birçok farklı senaryolar ve permütasyon runbook'larınızı, geliştirilmesi sırasında Ayrıca güncelleştirme gibi yinelemeli değişiklikler böylece test oluşturma göz önünde bulundurun PowerShell modülleri. Sonuçları doğrulanır ve gereken değişiklikleri uyguladınız sonra değiştirilmesi gereken runbook'ları geçişini koordine ile devam edin ve üretimde açıklandığı şu güncelleştirmeyi gerçekleştirin.

> [!NOTE]
> Yeni bir Otomasyon hesabı, en yeni modülleri içermeyebilir.

## <a name="updating-azure-modules"></a>Azure modülleri güncelleştiriliyor

1. Otomasyon hesabınızın modülleri sayfasında adlı bir seçenek yoktur **Azure modüllerini güncelleştirme**. Her zaman etkindir.<br><br> ![Modüller sayfasında seçenek Azure modüllerini güncelleştir](media/automation-update-azure-modules/automation-update-azure-modules-option.png)

  > [!NOTE]
  > Bir test emin olmak için Otomasyon hesabı güncelleştirmeniz önerilir, Azure modülleri güncelleştirmeden önce mevcut komut dosyalarınızı Azure modüllerinizi güncelleştirmeden önce beklendiği gibi çalışır.
  >
  > **Azure modüllerini güncelleştirme** düğmesi kullanılabilir yalnızca genel bulutta. Mevcut değildir [bağımsız bölgeler](https://azure.microsoft.com/global-infrastructure/). Lütfen bkz [modüllerinizi güncelleştirmesi için alternatif yöntemler](#alternative-ways-to-update-your-modules) bölümü daha fazla bilgi edinin.


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

    Modül zaten güncel olduğundan, işlem birkaç saniye içinde tamamlanır. Güncelleştirme işlemi tamamlandığında size bildirilir.<br><br> ![Azure modüllerini güncelleştirme durumunu güncelleştirme](media/automation-update-azure-modules/automation-update-azure-modules-updatestatus.png)

    .NET core AzureRm modülleri (AzureRm.*. Çekirdek) Azure Automation'da desteklenmez ve içeri aktarılamıyor.

> [!NOTE]
> Yeni bir zamanlanmış iş çalıştırıldığında azure Otomasyonu, Otomasyon hesabınızda en son modüllerini kullanır.  

Bu Azure PowerShell modülleri cmdlet'leri, runbook'ları kullanırsanız, her ay bu güncelleştirme işlemini çalıştırmak için veya bu nedenle en son modülleri sahip olduğunuzdan emin olmak için istiyorsunuz. Azure Otomasyonu kullanır, hizmet sorumlusu süresi doldu veya abonelik düzeyi, modül güncelleştirme artık modülleri güncelleştirirken kimliğini doğrulamak için AzureRunAsConnection bağlantısı başarısız olur.

## <a name="alternative-ways-to-update-your-modules"></a>Modüllerinizi güncelleştirmesi için alternatif yöntemler

Belirtildiği gibi **Azure modüllerini güncelleştirme** düğmesi kullanılamıyorsa bağımsız bulutlarda da, yalnızca Azure genel bulutunda kullanılabilir. Azure PowerShell modülleri PowerShell Galerisi'nden en son sürümünü şu anda bu bulutlara dağıtılan Resource Manager Hizmetleri ile çalışmayabilir Bunun nedeni, budur.

Modülleri güncelleştirme hala yapılabilir içeri aktararak [güncelleştirme AzureModule.ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AzureModule.ps1) runbook Otomasyon hesabınızı ve çalışır.

Kullanım `AzureRmEnvironment` parametresini kullanarak doğru ortamı runbook'una geçirebilirsiniz.  Kabul edilebilir değerler **AzureCloud**, **AzureChinaCloud**, **AzureGermanCloud**, ve **AzureUSGovernmentCloud**. Bu parametre için bir değer geçirmezseniz runbook, Azure ortak bulutuna varsayılacaktır **AzureCloud**.

Belirli bir Azure PowerShell modülü sürüm yerine en son kullanılabilir PowerShell galerisinde kullanmak istiyorsanız, bu sürümleri için isteğe bağlı geçirmek `ModuleVersionOverrides` parametresinin **güncelleştirme AzureModule** runbook. Örnekler için bkz [güncelleştirme AzureModule.ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AzureModule.ps1) runbook. İçinde belirtilmeyen azure PowerShell modüllerini `ModuleVersionOverrides` parametresi ve PowerShell Galerisi'ndeki son modülü sürümleriyle güncelleştirilir. Hiçbir şey gönderilirse `ModuleVersionOverrides` parametresi, tüm modülleri en son modülü PowerShell Galerisi sürümlerinde güncelleştirilir davranışını olduğu **Azure modüllerini güncelleştirme** düğmesi.

## <a name="next-steps"></a>Sonraki adımlar

* Tümleştirme modülleri ve daha fazla Otomasyon diğer sistemleri, hizmetler veya çözümler ile tümleştirmek için özel modüller oluşturma hakkında daha fazla bilgi için bkz: [tümleştirme modülleri](automation-integration-modules.md).

* Kaynak denetimi tümleştirmesi kullanmayı [GitHub Enterprise](automation-scenario-source-control-integration-with-github-ent.md) veya [Azure DevOps](automation-scenario-source-control-integration-with-vsts.md) merkezi olarak yönetmek ve Otomasyon runbook ve yapılandırma portföyünüzü sürümlerini denetlemek için.  
