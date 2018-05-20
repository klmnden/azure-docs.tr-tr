---
title: Azure automation'da Azure modülleri güncelleştir
description: Bu makalede, Azure automation'da varsayılan olarak sağlanan ortak Azure PowerShell modülleri nasıl şimdi güncelleştirebilirsiniz açıklanmaktadır.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: fe263346d8794b3dc85b6420d8b9b02efa5f9684
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a>Azure PowerShell modülleri Azure Automation güncelleştirme

En yaygın Azure PowerShell modülleri, varsayılan olarak her Automation hesabı sağlanır. Otomasyon hesabı hesabın modülleri yeni sürümler portaldan kullanılabilir olduğunda güncelleştirmek için bir yol sağlanır şekilde Azure ekibi Azure modülleri düzenli olarak güncelleştirir.  

Modüller ürün grubu tarafından düzenli olarak güncelleştirilen olduğundan, bir parametre yeniden adlandırma veya tamamen bir cmdlet onaysız kılınmadan gibi değişiklik türüne bağlı olarak, runbook'ları olumsuz yönde etkileyebilir dahil cmdlet'leri değişiklikler oluşabilir. Runbook'larınızı ve bunlar otomatikleştirmek işlemlerini etkileyen önlemek için test ve devam etmeden önce doğrulamak önerilir. Hedeflenen bu amaç için adanmış bir Otomasyon hesabı yoksa, böylece PowerShell modülleri güncelleştirme gibi yinelemeli değişikliklere ek olarak, runbook'ların geliştirme sırasında birçok farklı senaryolar ve permütasyon sınayabilirsiniz bir oluşturmayı düşünün. Sonuçları doğrulanır ve gereken değişiklikleri uyguladığınız sonra değiştirilmesi gereken runbook'ları geçişini Eşgüdümleme ile devam etmek ve üretimde açıklandığı gibi aşağıdaki güncelleştirmeyi gerçekleştirin.

## <a name="updating-azure-modules"></a>Azure modülleri güncelleştiriliyor

1. Otomasyon hesabınızı modülleri sayfasında adlı bir seçenek yoktur **Update Azure modülleri**. Her zaman etkindir.<br><br> ![Azure modülleri seçeneği modülleri sayfasındaki güncelleştir](media/automation-update-azure-modules/automation-update-azure-modules-option.png)

2. Tıklatın **Update Azure modülleri**, devam etmek isteyip istemediğinizi soran bir onay bildirimi gösterilir.<br><br> ![Azure modülleri bildirim güncelleştir](media/automation-update-azure-modules/automation-update-azure-modules-popup.png)

3. Tıklatın **Evet** ve modül güncelleştirme işlemi başlar. Güncelleştirme işlemini aşağıdaki modüller güncelleştirmek için yaklaşık 15-20 dakika sürer:

  * Azure
  * Azure Depolama
  * AzureRm.Automation
  * AzureRm.Compute
  * AzureRm.Profile
  * AzureRm.Resources
  * AzureRm.Sql
  * AzureRm.Storage

    Modülleri zaten güncel olup olmadığını işlemi birkaç saniye içinde tamamlanır. Güncelleştirme işlemi tamamlandığında size bildirilir.<br><br> ![Azure modülleri güncelleştirme durumunu güncelleştir](media/automation-update-azure-modules/automation-update-azure-modules-updatestatus.png)

> [!NOTE]
> Yeni bir zamanlanan iş çalıştırdığınızda azure Automation Otomasyon hesabınızda son modülleri kullanır.    

Bu Azure PowerShell modülleri cmdlet'leri, runbook'ları kullanırsanız, bu güncelleştirme işlemi her ay çalışacak şekilde veya böylece son modülleri sahip olduğunuzdan emin olmak istiyorsunuz.

## <a name="next-steps"></a>Sonraki adımlar

* Tümleştirme modülleri ve daha fazla Otomasyon diğer sistemler, hizmetleri veya çözümlerini tümleştirmek için özel modüller oluşturma hakkında daha fazla bilgi için bkz: [tümleştirme modülleri](automation-integration-modules.md).

* Kaynak denetimi tümleştirmesi kullanmayı [GitHub Kurumsal](automation-scenario-source-control-integration-with-github-ent.md) veya [Visual Studio Team Services](automation-scenario-source-control-integration-with-vsts.md) merkezi olarak yönetmek ve Otomasyon runbook ve yapılandırma Portföy sürümleri denetlemek için.  