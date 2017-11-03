---
title: "Azure Güvenlik Merkezi güvenlik ilkelerine giriş | Microsoft Docs"
description: "Azure Güvenlik Merkezi güvenlik ilkeleri ve temel işlevleri hakkında bilgi edinin."
services: security-center
documentationcenter: na
author: YuriDio
manager: MBaldwin
editor: 
ms.assetid: f24b1e4a-cc36-4542-b21e-041453cdfcd8
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2017
ms.author: yurid
ms.openlocfilehash: 95ef2099cb16bcfd550ce2799428f1a16031f535
ms.sourcegitcommit: 5d772f6c5fd066b38396a7eb179751132c22b681
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2017
---
# <a name="security-policies-overview"></a>Güvenlik ilkelerine genel bakış
Bu belge, Güvenlik Merkezi'nde güvenlik ilkelerini genel bir bakış sağlar.

## <a name="what-are-security-policies"></a>Güvenlik ilkeleri nedir?
Güvenlik ilkesi iş yüklerinizin istenen yapılandırmasını tanımlar ve şirketin veya yasal düzenlemelerin gerektirdiği güvenlik gereksinimlerine uyum sağlanmasına yardımcı olur. Güvenlik Merkezi'nde Azure aboneliğinize yönelik ilkeleri tanımlayabilir, bunları iş yükü türüne veya veri gizlilik düzeyine göre ayarlayabilirsiniz. Örneğin, kişisel olarak tanımlanabilir bilgiler diğer iş yüklerini daha yüksek düzeyde güvenlik gerektirebilir gibi uygulamalar, düzenlenen veriler kullanabilirsiniz. 

Güvenlik Merkezi ilkeleri aşağıdaki bileşenleri içerir:

- Veri toplama: sağlama aracısı ve [veri toplama](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection) ayarlar.
- Güvenlik İlkesi: hangi denetimlerin izlenen ve Güvenlik Merkezi tarafından önerilen belirler (Düzenle [Güvenlik İlkesi](https://docs.microsoft.com/en-us/azure/security-center/security-center-policies) Güvenlik Merkezi ya da kullanım [Azure ilke](security-center-azure-policy.md), sınırlı önizlemede Yeni Oluştur tanımları, ek ilkeler tanımlayın ve yönetim grubu içinde ilke ataması).
- E-posta bildirimleri: güvenlik kişiler ve [bildirim e-posta](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details) ayarlar.
- Fiyatlandırma katmanı: boş ya da standart [seçimi fiyatlandırma](https://docs.microsoft.com/azure/security-center/security-center-pricing), hangi belirler (belirtilebilir abonelikler, kaynak grupları ve çalışma alanları için) kapsamdaki kaynaklar için hangi Güvenlik Merkezi özellikleri kullanılabilir. 


## <a name="who-can-edit-security-policies"></a>Kimin güvenlik ilkeleri düzenleyebilirsiniz.
Güvenlik Merkezi, kullanıcıları, grupları ve Azure Hizmetleri için atanan yerleşik roller sağlayan rol tabanlı erişim denetimi (RBAC) kullanır. Bir kullanıcı Güvenlik Merkezi’ni açtığında, yalnızca erişimi olan kaynaklarla ilişkili bilgileri görüntüleyebilir. Bu da bir kaynağın ait olduğu abonelik veya kaynak grubu için kullanıcıya Sahip, Katkıda Bulunan veya Okuyucu rolünün atandığı anlamına gelir. Bu rollere ek olarak iki özel Güvenlik Merkezi rolü vardır:

- Güvenlik okuyucu: Bu role ait kullanıcı öneriler, uyarılar, ilke ve sistem durumu içerir, Güvenlik Merkezi hakkı görüntüleyebilir ancak değişiklik yapmak göremezsiniz.
- Güvenlik Yöneticisi: güvenlik okuyucu ancak güvenlik ilkesi güncelleştirebilirsiniz gibi aynı önerileri ve uyarılar yok sayın.


## <a name="next-steps"></a>Sonraki adımlar
Bu belgede Azure Güvenlik Merkezi'nde güvenlik ilkelerini hakkında öğrendiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) — Azure Abonelikleriniz ve kaynak grupları için güvenlik ilkeleri yapılandırmayı öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) — nasıl önerilerin Azure kaynaklarınızı korumanıza yardımcı öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md) — yönetme ve güvenlik uyarılarını yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
- [Azure Güvenlik Merkezi veri güvenliği](security-center-data-security.md) -verilerin yönetilmesi ve diğer Güvenlik Merkezi'nde korunması nasıl öğrenin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) — en son Azure güvenlik haberlerini ve bilgilerini edinin.


