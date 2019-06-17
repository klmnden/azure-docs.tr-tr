---
title: Azure Güvenlik Merkezi'nde güvenlik önerilerini | Microsoft Docs
description: Bu belge Azure Güvenlik Merkezi'nde öneriler, Azure kaynaklarınızı korumanıza ve güvenlik ilkelerine uygun kalın nasıl yardımcı aracılığıyla size yol gösterir.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: 86c50c9f-eb6b-4d97-acb3-6d599c06133e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/13/2019
ms.author: v-mohabe
ms.openlocfilehash: fe1d4bf27f3c4bb1f70c1c1fa9767c27f8767998
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67064207"
---
# <a name="security-recommendations-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenlik önerileri 
Bu konu başlığı altında görüntülemek ve Azure kaynaklarınızı korumanıza yardımcı olması için Azure Güvenlik Merkezi'nde öneriler anlamak açıklanmaktadır.

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır.  Bu belgede, adım adım bir kılavuz değildir.
>

## <a name="what-are-security-recommendations"></a>Güvenlik önerilerini nelerdir?
Güvenlik Merkezi düzenli aralıklarla Azure kaynaklarınızın güvenlik durumunu çözümler. Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde öneriler oluşturur. Gerekli denetimlerin yapılandırılması işlemi boyunca öneriler size rehberlik eder.

## <a name="implementing-security-recommendations"></a>Güvenlik önerilerini uygulama
### <a name="set-recommendations"></a>Ayarlama önerileri
İçinde [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md), şunların nasıl yapıldığını öğrenirsiniz:

* Güvenlik ilkeleri yapılandırın.
* Veri toplamayı etkinleştir.
* Güvenlik ilkenizin bir parçası olarak görmek için hangi önerilerin seçin.

Geçerli ilke önerileri Merkezi sistem güncelleştirmeleri, temel kurallar, kötü amaçlı yazılımdan koruma programları etrafında [ağ güvenlik grupları](../virtual-network/security-overview.md) alt ağları ve ağ arabirimleri, SQL veritabanı denetimi, SQL veritabanı saydam veri şifrelemesi, ve web uygulaması güvenlik duvarları.  [Güvenlik ilkelerini ayarlama](tutorial-security-policy.md) her öneri seçeneği açıklamasını sağlar.

### <a name="monitor-recommendations"></a>İzleme önerileri
Bir güvenlik ilkesi tanımladıktan sonra, Güvenlik Merkezi olası güvenlik açıklarını tanımlamak için kaynaklarınızın güvenlik durumunu analiz eder. **Önerileri** altında kutucuğuna **genel bakış** Güvenlik Merkezi tarafından tanımlanan öneriler toplam sayısını gösterir.

![Güvenlik merkezine genel bakış](./media/security-center-recommendations/asc-overview.png)

1. Seçin **önerileri kutucuğuna** altında **genel bakış**. **Önerileri** listesi açılır.
    
      ![Önerileri görüntüleme](./media/security-center-recommendations/view-recommendations.png)

    Öneriler filtreleyebilirsiniz. Öneriler filtre uygulamak için seçin **filtre** üzerinde **önerileri** dikey penceresi. **Filtre** dikey penceresi açılır ve görmek istediğiniz önem ve durum değerleri seçin.

   * **ÖNERİLER**: Öneri.
   * **PUAN ETKİSİ GÜVENLİ**: Güvenlik Merkezi tarafından oluşturulan bir puan her önerinin nasıl önemli olduğunu belirlemek için algoritmalar kullanarak, güvenlik önerileri ve uygulama Gelişmiş. Daha fazla bilgi için [puanı hesaplamaya güvenli](security-center-secure-score.md#secure-score-calculation).
   * **KAYNAK**: Bu önerinin geçerli olduğu kaynakları listeler.
   * **DURUM ÇUBUKLARI**:  Belirli bir önerinin önem açıklanmaktadır:
       * **Yüksek (kırmızı)** : Bir güvenlik açığı anlamlı bir kaynakta (örneğin, bir uygulama, bir VM veya ağ güvenlik grubu) ile var ve ilgilenilmesi gerekiyor.
       * **Orta (turuncu)** : Bir güvenlik açığı var ve bunu ortadan kaldırmak için veya bir işlemi tamamlamak için kritik olmayan veya ek adımlar gereklidir.
       * **Düşük (mavi)** : Mevcut bir güvenlik açığı olan ele alınması gerekiyor ancak hemen ilgilenilmesi gerekmiyor. (Varsayılan olarak, düşük öneriler almazsınız ancak bunları görüntülemek istiyorsanız düşük öneriler filtresini kullanabilirsiniz.) 
       * **Sağlıklı (yeşil)** :
       * **Kullanılabilir değil (gri)** :

1. Her önerinin'ın ayrıntılarını görüntülemek için öneriye tıklayın.

    ![Öneri ayrıntıları](./media/security-center-recommendations/recommendation-details.png)

>[!NOTE] 
> Bkz: [Klasik ve Resource Manager dağıtım modellerinde](../azure-classic-rm.md) Azure kaynakları için.
  
 ### <a name="apply-recommendations"></a>Önerileri uygulama
> Tüm önerileri gözden geçirdikten sonra hangisinin önce uygulamaya karar verin. Güvenli kullanmanızı öneririz puan etkilemeden önce hangi önerilerin uygulanması gereken değerlendirin.

1. Listeden öneriye tıklayın.
1. Bölümündeki yönergeleri *düzeltme adımları* bölümü.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Güvenlik Merkezi'nde güvenlik önerilerini yaptınız. Güvenlik Merkezi hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) — Azure Abonelikleriniz ve kaynak grupları için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md) — yönetme ve güvenlik uyarılarını yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.
