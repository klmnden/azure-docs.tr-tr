---
title: Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme | Microsoft Docs
description: Bu belge Azure Güvenlik Merkezi'nde öneriler, Azure kaynaklarınızı korumanıza ve güvenlik ilkelerine uygun kalın nasıl yardımcı aracılığıyla size yol gösterir.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 86c50c9f-eb6b-4d97-acb3-6d599c06133e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/13/2018
ms.author: rkarlin
ms.openlocfilehash: e3b4da1c1d835e9d630c000055af058aa7b45968
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60905951"
---
# <a name="managing-security-recommendations-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme
Bu belge, Azure kaynaklarınızı korumanıza yardımcı olması için Azure Güvenlik Merkezi'nde öneriler kullanma hakkında bilgi vermektedir.

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır.  Bu belgede, adım adım bir kılavuz değildir.
>
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
Bir güvenlik ilkesi tanımladıktan sonra, Güvenlik Merkezi olası güvenlik açıklarını tanımlamak için kaynaklarınızın güvenlik durumunu analiz eder. **Önerileri** altında kutucuğuna **genel bakış** Güvenlik Merkezi tarafından tanımlanan öneriler toplam sayısı bildiğiniz sağlar.

![Öneriler kutucuğu][1]

Her önerinin ayrıntıları görmek için seçin **önerileri kutucuğuna** altında **genel bakış**. **Öneriler** açılır.

![Filtre önerileri][2]

Öneriler filtreleyebilirsiniz. Öneriler filtre uygulamak için seçin **filtre** üzerinde **önerileri** dikey penceresi. **Filtre** dikey penceresi açılır ve görmek istediğiniz önem ve durum değerleri seçin.


* **ÖNERİLER**: Öneri.
* **PUAN ETKİSİ GÜVENLİ**:
* **KAYNAK**: Bu önerinin geçerli olduğu kaynakları listeler.
* **DURUM ÇUBUKLARI**:  Belirli bir önerinin önem açıklanmaktadır:
   * **Yüksek (kırmızı)**: Bir güvenlik açığı anlamlı bir kaynakta (örneğin, bir uygulama, bir VM veya ağ güvenlik grubu) ile var ve ilgilenilmesi gerekiyor.
   * **Orta (turuncu)**: Bir güvenlik açığı var ve bunu ortadan kaldırmak için veya bir işlemi tamamlamak için kritik olmayan veya ek adımlar gereklidir.
   * **Düşük (mavi)**: Mevcut bir güvenlik açığı olan ele alınması gerekiyor ancak hemen ilgilenilmesi gerekmiyor. (Varsayılan olarak, düşük öneriler almazsınız ancak bunları görüntülemek istiyorsanız düşük öneriler filtresini kullanabilirsiniz.) 
   * **Sağlıklı (yeşil)**:
   * **Kullanılabilir değil (gri)**:
 


> [!NOTE]
> Öğrenmek istediğiniz [Klasik ve Resource Manager dağıtım modellerinde](../azure-classic-rm.md) Azure kaynakları için.
> 
> 
> ### <a name="apply-recommendations"></a>Önerileri uygulama
> Tüm önerileri gözden geçirdikten sonra bir önce uygulamanız karar verebilirsiniz. Hangi önerileri değerlendirmek için ana parametresi ilk uygulanması gereken şekilde önem derecesi kullanmanızı öneririz.



## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Güvenlik Merkezi'nde güvenlik önerilerini yaptınız. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) — Azure Abonelikleriniz ve kaynak grupları için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md) — yönetme ve güvenlik uyarılarını yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.

<!--Image references-->
[1]: ./media/security-center-recommendations/recommendations-tile.png
[2]: ./media/security-center-recommendations/filter-recommendations.png
