---
title: Azure Güvenlik Merkezi Ayarları | Microsoft Docs
description: Azure Güvenlik Merkezi ayarlarını yapılandırın.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: f24b1e4a-cc36-4542-b21e-041453cdfcd8
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/3/2018
ms.author: rkarlin
ms.openlocfilehash: 2aa4545aa79260bea792392e1bebf4166253fc87
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52315724"
---
# <a name="security-center-settings"></a>Güvenlik Merkezi Ayarları
Bu makalede Güvenlik Merkezi'ndeki ayarları genel bir bakış sağlar.

Güvenlik İlkesi altında aşağıdaki ayarları ulaşılabilir:

- **Veri toplama**: aracı sağlama belirler ve [veri toplama](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection) ayarları.
- **Güvenlik İlkesi**: hangi denetimleri izlediğini Güvenlik Merkezi ve önerir belirler. Düzenleyebileceğiniz [Güvenlik İlkesi](security-center-azure-policy.md) Güvenlik Merkezi'nde. Ayrıca [Azure İlkesi](security-center-azure-policy.md) yeni tanımları oluşturmak için ek ilkeler tanımlamak ve yönetim gruplarına ilkeler atama. 
- **E-posta bildirimleri**: güvenlik ilgili kişi belirler ve [e-posta bildirimi](security-center-provide-security-contact-details.md) ayarları.
- **Fiyatlandırma katmanı**: ücretsiz veya standart tanımlar [fiyatlandırma seçimi](security-center-pricing.md). Seçtiğiniz katman, kapsam dahilindeki kaynaklar için hangi Güvenlik Merkezi özelliklerinin kullanılabilir olduğunu belirler. Bir katman için abonelik, kaynak grupları ve çalışma alanları belirtebilirsiniz.

> [!NOTE]
> Tüm bunların abonelik başına ayarlayabilirsiniz. Çalışma alanları için yalnızca veri toplama ve fiyatlandırma katmanı ayarlayabilirsiniz. Kaynak grupları için fiyatlandırma katmanı ayarlayabilirsiniz.
>


## <a name="who-can-edit-security-policies"></a>Güvenlik ilkeleri düzenleyebilecek kişiler
Güvenlik Merkezi, kullanıcıları, grupları ve Azure Hizmetleri için atanan yerleşik roller sağlayan rol tabanlı erişim denetimi (RBAC) kullanır. Kullanıcı Güvenlik Merkezi'ni açtığında erişime sahip oldukları kaynakları ile ilgili bilgiler yalnızca görürler. Kullanıcı rolüne atanmasını *sahibi*, *katkıda bulunan*, veya *okuyucu* bir kaynak barındıran abonelik veya kaynak grubu için. Bu rollere ek olarak iki özel Güvenlik Merkezi rolü vardır:

- **Güvenlik okuyucusu**: sahip görünümü hakları içerir. öneriler, uyarılar, ilke ve sistem durumu, Güvenlik Merkezi için ancak bunlar üzerinde değişiklik yapamaz.
- **Güvenlik Yöneticisi**: aynı görüntüleme haklarına sahip *güvenlik okuyucusu*, ve bunlar da güvenlik ilkesini güncelleştirebilir ve öneriler ve Uyarıları kapat.


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure Güvenlik Merkezi'nde güvenlik ilkeleri hakkında bilgi edindiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-azure-policy.md): Azure Abonelikleriniz ve kaynak grupları için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md): Güvenlik Merkezi önerilerini Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md): Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md): Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md): İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
- [Azure Güvenlik Merkezi veri güvenliği](security-center-data-security.md): Güvenlik Merkezi'nin nasıl yönetir ve verileri koruyan öğrenin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md): Hizmet kullanımı ile ilgili sık sorulan soruların yanıtlarını alın.
* [Azure güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/): en son Azure güvenlik haberlerini ve bilgilerini edinin.
