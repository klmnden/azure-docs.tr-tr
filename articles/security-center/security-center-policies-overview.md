---
title: Azure Güvenlik Merkezi Ayarları | Microsoft Docs
description: Azure Güvenlik Merkezi ayarlarını yapılandırın.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: f24b1e4a-cc36-4542-b21e-041453cdfcd8
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/3/2018
ms.author: rkarlin
ms.openlocfilehash: ec674641991a1b5a1e0ca92c133be235dd91dfae
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58666503"
---
# <a name="security-center-settings"></a>Güvenlik Merkezi ayarları
Bu makalede Güvenlik Merkezi'ndeki ayarları genel bir bakış sağlar.

Güvenlik İlkesi altında aşağıdaki ayarları ulaşılabilir:

- **Veri toplama**: Sağlama Aracısı belirler ve [veri toplama](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection) ayarları.
- **Güvenlik İlkesi**: Güvenlik Merkezi izleyiciler denetler ve önerir belirler. Düzenleyebileceğiniz [Güvenlik İlkesi](tutorial-security-policy.md) Güvenlik Merkezi'nde. Ayrıca [Azure İlkesi](tutorial-security-policy.md) yeni tanımları oluşturmak için ek ilkeler tanımlamak ve yönetim gruplarına ilkeler atama. 
- **E-posta bildirimleri**: Güvenlik ilgili kişi belirler ve [e-posta bildirimi](security-center-provide-security-contact-details.md) ayarları.
- **Fiyatlandırma katmanı**: Ücretsiz veya standart tanımlar [fiyatlandırma seçimi](security-center-pricing.md). Seçtiğiniz katman, kapsam dahilindeki kaynaklar için hangi Güvenlik Merkezi özelliklerinin kullanılabilir olduğunu belirler. Bir katman için abonelik ve çalışma alanları belirtebilirsiniz.

> [!NOTE]
> Tüm bunların abonelik başına ayarlayabilirsiniz. Çalışma alanları için yalnızca veri toplama ve fiyatlandırma katmanı ayarlayabilirsiniz.
>


## <a name="who-can-edit-security-policies"></a>Güvenlik ilkeleri düzenleyebilecek kişiler
Güvenlik Merkezi, kullanıcıları, grupları ve Azure Hizmetleri için atanan yerleşik roller sağlayan rol tabanlı erişim denetimi (RBAC) kullanır. Kullanıcı Güvenlik Merkezi'ni açtığında erişime sahip oldukları kaynakları ile ilgili bilgiler yalnızca görürler. Kullanıcı rolüne atanmasını *sahibi*, *katkıda bulunan*, veya *okuyucu* kaynağın ait olduğu aboneliğe. Bu rollere ek olarak iki özel Güvenlik Merkezi rolü vardır:

- **Güvenlik okuyucusu**: Güvenlik Merkezi, öneriler, uyarılar, ilke ve sistem durumu içeren görünümü haklarına sahip ancak bunlar üzerinde değişiklik yapamaz.
- **Güvenlik Yöneticisi**: Aynı görüntüleme haklarına sahip *güvenlik okuyucusu*, ve bunlar da güvenlik ilkesini güncelleştirebilir ve öneriler ve Uyarıları kapat.


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure Güvenlik Merkezi'nde güvenlik ilkeleri hakkında bilgi edindiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md): Azure Abonelikleriniz için güvenlik ilkelerinin nasıl yapılandırılacağını öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md): Güvenlik Merkezi önerilerini Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Güvenlik durumunu, Azure Güvenlik Merkezi'nde izleme](security-center-monitoring.md): Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md): Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md): İş ortağı çözümlerinizin sistem durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi veri güvenliği](security-center-data-security.md): Güvenlik Merkezi'nin nasıl yönetir ve verileri koruyan öğrenin.
* [Azure Güvenlik Merkezi SSS](security-center-faq.md): Hizmet kullanımı ile ilgili sık sorulan soruların yanıtlarını alın.
* [Azure güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/): En son Azure güvenlik haberlerini ve bilgilerini alın.
