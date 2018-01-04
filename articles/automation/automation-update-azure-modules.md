---
title: "Azure automation'da Azure modülleri güncelleştirme | Microsoft Docs"
description: "Bu makalede, Azure automation'da varsayılan olarak sağlanan ortak Azure PowerShell modülleri nasıl şimdi güncelleştirebilirsiniz açıklanmaktadır."
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/13/2017
ms.author: magoedte
ms.openlocfilehash: f5e7c66cfd26bd6927d48ffd8bc0f82e9a3e2d13
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a>Azure PowerShell modülleri Azure Automation güncelleştirme

En yaygın Azure PowerShell modülleri, varsayılan olarak her Automation hesabı sağlanır.  Yeni sürümler portaldan hazır olduğunda hesabı modülleri güncelleştirmek bir yol sağladığımız Automation hesabında şekilde Azure ekibi Azure modülleri düzenli olarak güncelleştirir.  

Modüller ürün grubu tarafından düzenli olarak güncelleştirilen olduğundan, bir parametre yeniden adlandırma veya tamamen bir cmdlet onaysız kılınmadan gibi değişiklik türüne bağlı olarak, runbook'ları olumsuz yönde etkileyebilir dahil cmdlet'leri değişiklikler oluşabilir. Runbook'larınızı ve bunlar otomatikleştirmek işlemlerini etkileyen önlemek için test ve devam etmeden önce doğrulamak önerilir.  Hedeflenen bu amaç için adanmış bir Otomasyon hesabı yoksa, böylece PowerShell modülleri güncelleştirme gibi yinelemeli değişikliklere ek olarak, runbook'ların geliştirme sırasında birçok farklı senaryolar ve permütasyon sınayabilirsiniz bir oluşturmayı düşünün.  Sonuçları doğrulanır ve gereken değişiklikleri uyguladığınız sonra değiştirilmesi gereken runbook'ları geçişini Eşgüdümleme ile devam etmek ve üretimde aşağıda açıklandığı gibi güncelleştirme gerçekleştirin.     

## <a name="updating-azure-modules"></a>Azure modülleri güncelleştiriliyor

1. Otomasyon hesabınızı modülleri sayfasında adlı bir seçenek yoktur **Update Azure modülleri**. Her zaman etkindir.<br><br> ![Azure modülleri seçeneği modülleri sayfasındaki güncelleştir](media/automation-update-azure-modules/automation-update-azure-modules-option.png)

2. Tıklatın **Update Azure modülleri** ve devam etmek isteyip istemediğinizi soran bir onay bildirimi sunulur.<br><br> ![Azure modülleri bildirim güncelleştir](media/automation-update-azure-modules/automation-update-azure-modules-popup.png)

3. Tıklatın **Evet** ve modül güncelleştirme işlemi başlar. Güncelleştirme işlemini aşağıdaki modüller güncelleştirmek için yaklaşık 15-20 dakika sürer:

  * Azure
  * Azure Depolama
  * AzureRm.Automation
  * AzureRm.Compute
  * AzureRm.Profile
  * AzureRm.Resources
  * AzureRm.Sql
  * AzureRm.Storage

    Modülleri zaten güncel olup olmadığını işlemi birkaç saniye içinde tamamlanır. Güncelleştirme işlemi tamamlandığında size bildirilecek.<br><br> ![Azure modülleri güncelleştirme durumunu güncelleştir](media/automation-update-azure-modules/automation-update-azure-modules-updatestatus.png)

> [!NOTE]
> Yeni bir zamanlanan iş çalıştırdığınızda azure Automation Otomasyon hesabınızda son modülleri kullanır.    

Azure kaynaklarınızı yönetmek için runbook'larda bu Azure PowerShell modülleri cmdlet'leri kullanın, sonra her ay bu güncelleştirme işlemini gerçekleştirmek için veya böylece son modülleri olmasını güvence altına almak için istediğiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Tümleştirme modülleri ve daha fazla Otomasyon diğer sistemler, hizmetleri veya çözümlerini tümleştirmek için özel modüller oluşturma hakkında daha fazla bilgi için bkz: [tümleştirme modülleri](automation-integration-modules.md).

* Kaynak denetimi tümleştirmesi kullanmayı [GitHub Kurumsal](automation-scenario-source-control-integration-with-github-ent.md) veya [Visual Studio Team Services](automation-scenario-source-control-integration-with-vsts.md) merkezi olarak yönetmek ve Otomasyon runbook ve yapılandırma Portföy sürümleri denetlemek için.  