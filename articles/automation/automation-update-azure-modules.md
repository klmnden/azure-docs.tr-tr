---
title: Azure automation'da Azure modüllerini güncelleştir
description: Bu makalede, Azure automation'da varsayılan tarafından sağlanan genel Azure PowerShell modüllerini nasıl şimdi güncelleştirebilirsiniz açıklanır.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 90aa19d690b1b4ab28c3a65a287a10aaf6a03ac6
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37929041"
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a>Azure automation'da Azure PowerShell modüllerini güncelleştirme

En yaygın Azure PowerShell modüllerinin her Otomasyon hesabında varsayılan olarak sağlanır. Azure ekibi otomasyon hesabında hesap modülleri portaldan yeni sürümler kullanılabilir olduğunda güncelleştirilmesine olanak sağlanır. Azure modüllerini düzenli olarak güncelleştirir.

Modüller düzenli olarak ürün grubu tarafından güncelleştirildiğinden, değişiklikleri gibi bir parametre yeniden adlandırma ya da tamamen bir cmdlet kullanım dışı bırakılıyor değişiklik türüne bağlı olarak runbook'larınızı olumsuz etkileyebilir dahil edilen cmdlet'ler ortaya çıkabilir. Runbook'larınızı ve bunlar otomatikleştirmek işlemleri etkilemesini önlemek için test edin ve devam etmeden önce doğrulanması önerilir. Hedeflenen bu amaç için adanmış bir Otomasyon hesabı yoksa, birçok farklı senaryolar ve permütasyon runbook'larınızı, geliştirilmesi sırasında Ayrıca güncelleştirme gibi yinelemeli değişiklikler böylece test oluşturma göz önünde bulundurun PowerShell modülleri. Sonuçları doğrulanır ve gereken değişiklikleri uyguladınız sonra değiştirilmesi gereken runbook'ları geçişini koordine ile devam edin ve üretimde açıklandığı şu güncelleştirmeyi gerçekleştirin.

> [!NOTE]
> Yeni bir Otomasyon hesabı, en yeni modülleri içermeyebilir.

## <a name="updating-azure-modules"></a>Azure modülleri güncelleştiriliyor

1. Otomasyon hesabınızın modülleri sayfasında adlı bir seçenek yoktur **Azure modüllerini güncelleştirme**. Her zaman etkindir.<br><br> ![Modüller sayfasında seçenek Azure modüllerini güncelleştir](media/automation-update-azure-modules/automation-update-azure-modules-option.png)

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

> [!NOTE]
> Yeni bir zamanlanmış iş çalıştırıldığında azure Otomasyonu, Otomasyon hesabınızda en son modüllerini kullanır.    

Bu Azure PowerShell modülleri cmdlet'leri, runbook'ları kullanırsanız, her ay bu güncelleştirme işlemini çalıştırmak için veya bu nedenle en son modülleri sahip olduğunuzdan emin olmak için istiyorsunuz. Azure Otomasyonu kullanır, hizmet sorumlusu süresi doldu veya abonelik düzeyi, modül güncelleştirme artık modülleri güncelleştirirken kimliğini doğrulamak için AzureRunAsConnection bağlantısı başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar

* Tümleştirme modülleri ve daha fazla Otomasyon diğer sistemleri, hizmetler veya çözümler ile tümleştirmek için özel modüller oluşturma hakkında daha fazla bilgi için bkz: [tümleştirme modülleri](automation-integration-modules.md).

* Kaynak denetimi tümleştirmesi kullanmayı [GitHub Enterprise](automation-scenario-source-control-integration-with-github-ent.md) veya [Visual Studio Team Services](automation-scenario-source-control-integration-with-vsts.md) merkezi olarak yönetmek ve Otomasyon runbook ve yapılandırma portföyünüzü sürümlerini denetlemek için.  
