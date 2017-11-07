---
title: "Azure Güvenlik Merkezi güvenlik ilkelerine giriş | Microsoft Docs"
description: "Azure Güvenlik Merkezi güvenlik ilkeleri ve anahtar özellikleri hakkında bilgi edinin."
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
ms.openlocfilehash: 60cc65bb94e05da1c0b7ee20930c0530f46e71ec
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="security-policies-overview"></a>Güvenlik ilkelerine genel bakış
Bu makalede, Güvenlik Merkezi'nde güvenlik ilkelerini genel bir bakış sağlar.

## <a name="what-are-security-policies"></a>Güvenlik ilkeleri nedir?
Güvenlik ilkesi iş yüklerinizin istenen yapılandırmasını tanımlar ve şirketin veya yasal düzenlemelerin gerektirdiği güvenlik gereksinimlerine uyum sağlanmasına yardımcı olur. Azure Güvenlik Merkezi'nde Azure aboneliklerinizi ilkeleri tanımlayın ve bunları, iş yükü türünü veya verilerinizin duyarlılığına uyarlama. Örneğin, kişisel olarak tanımlanabilir bilgiler gibi düzenlenen veriler kullanan uygulamalar, diğer iş yüklerini daha yüksek düzeyde güvenlik gerektirebilir. 

Güvenlik Merkezi ilkeleri aşağıdaki bileşenleri içerir:

- **Veri toplama**: sağlama Aracısı belirler ve [veri toplama](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection) ayarlar.
- **Güvenlik İlkesi**: Güvenlik Merkezi izleyiciler denetler ve önerir belirler. Düzenleyebileceğiniz [Güvenlik İlkesi](https://docs.microsoft.com/en-us/azure/security-center/security-center-policies) Güvenlik Merkezi'nde. Aynı zamanda [Azure ilke](security-center-azure-policy.md) (önizlemede sınırlı) yeni tanımları oluşturmak için ek ilkeler tanımlayın ve yönetim grubu içinde ilkeleri atayın.
- **E-posta bildirimleri**: güvenlik kişiler belirler ve [bildirim e-posta](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details) ayarlar.
- **Fiyatlandırma katmanı**: boş ya da standart tanımlar [seçimi fiyatlandırma](https://docs.microsoft.com/azure/security-center/security-center-pricing). Seçtiğiniz katmanı kapsamında kaynaklar için hangi Güvenlik Merkezi özellikleri kullanılabileceğini belirler. Abonelik, kaynak grupları ve çalışma alanları için bir katmanı belirtebilirsiniz. 


## <a name="who-can-edit-security-policies"></a>Kimin güvenlik ilkeleri düzenleyebilirsiniz.
Güvenlik Merkezi, kullanıcıları, grupları ve Azure Hizmetleri için atanan yerleşik roller sağlayan rol tabanlı erişim denetimi (RBAC) kullanır. Kullanıcılar Güvenlik Merkezi'ni açtığında kaynaklara erişimi ilgili bilgiler yalnızca görürler. Başka bir deyişle, kullanıcı rolü atanmış *sahibi*, *katkıda bulunan*, veya *okuyucu* bir kaynak barındıran abonelik veya kaynak grubu için. Bu rollere ek olarak iki özel Güvenlik Merkezi rolü vardır:

- **Güvenlik okuyucu**: sahip görünümü hakları öneriler, uyarılar, ilke ve sistem durumu içerir, Güvenlik Merkezi için ancak değişiklik yapamaz.
- **Güvenlik Yönetim**: aynı görünüm haklara sahip *güvenlik okuyucu*, ve bunlar da güvenlik ilkesi güncelleştirebilir ve önerileri ve uyarılar yok sayın.


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure Güvenlik Merkezi'nde güvenlik ilkelerini hakkında öğrendiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md): Azure Abonelikleriniz ve kaynak grupları için güvenlik ilkeleri yapılandırmayı öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md): Güvenlik Merkezi önerilerini Azure kaynaklarınızı korumanıza nasıl yardımcı öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md): Azure kaynaklarınızı sağlığını izlemek öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md): Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md): İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
- [Azure Güvenlik Merkezi veri güvenliği](security-center-data-security.md): Güvenlik Merkezi nasıl yönetir ve verileri koruyan öğrenin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md): Get Hizmeti'ni kullanma hakkında sık sorulan soruları yanıtlar.
* [Azure güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/): en son Azure güvenlik haberlerini ve bilgilerini edinin.


