---
title: Azure ön kapısı hizmeti - uygulama katman güvenlik | Microsoft Docs
description: Bu makale, nasıl Azure ön kapısı Service, uygulama arka uçları korunmasına olanak anlamanıza yardımcı olur.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: c7b99548e2fe1ad0c1cab39953e28a97e7ebff4b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60193926"
---
# <a name="application-layer-security-with-front-door"></a>Uygulama Katmanı Güvenliği ile ön kapısı
Azure ön kapısı hizmeti, ağ saldırılarına ve SQL ekleme veya çoklu Site komut dizisi (XSS) gibi yaygın web güvenlik açıklarına web uygulamalarınızı korumak için web uygulaması koruma özelliği sağlar. Http (s) için ön uç etkin, ön kapısı'nın uygulama katman güvenlik dünya çapında dağıtılır ve her zaman açık, Azure'nın Network durdurma kötü amaçlı saldırıları, uzakta uçlarınıza kenar. Ekstra güvenlik ve performans iyileştirme sayesinde ön kapısı hızlı sunar ve son kullanıcılarınızın güvenli web deneyimleri.

## <a name="application-protection"></a>Uygulama koruması
Ön kapısı'nın uygulama koruması uygulamaları ayarlarına uygun olarak dünyanın her edge ortamında yapılandırılmış ve otomatik olarak olmayan engeller-http (s) trafiğinin ulaşmasını web uygulamalarınızı. Çok kiracılı dağıtılmış mimarimiz performanstan ödün vermeden uygun ölçekte genel koruma sağlar. Http (s) iş yükleri için ön kapı'nın web uygulaması koruma hizmeti için özel kurallar, önceden yapılandırılmış bir kural kümesi genel saldırılara karşı bir zengin kurallar altyapısı sağlar ve tüm istekler için günlük ayrıntılı bir kural eşleşen. Esnek Eylemler dahil olmak üzere izin verme, engelleme veya günlük yalnızca desteklenir.

## <a name="custom-access-control-rules"></a>Özel erişim denetimi kuralları
- **IP listesi ve Engellenenler listesine izin ver:** Web uygulamalarınızı istemci IP adreslerinin listesine göre erişimi denetlemek için özel kurallar yapılandırabilirsiniz. IP v4 hem de IP v6 desteklenir
- **Coğrafi tabanlı erişim denetimi:** Web uygulamalarınızı istemci IP'si dandır ülke kodu göre erişimi denetlemek için özel kurallar yapılandırabilirsiniz.
- **HTTP parametreleri: filtreleme** Üst bilgiler, URL ve sorgu dizeleri dahil olmak üzere http (s) istek parametrelerini eşleşmesi temeline göre özel erişim kuralları yapılandırabilirsiniz.

## <a name="azure-managed-rules"></a>Azure tarafından yönetilen kuralları
- Önceden yapılandırılmış üst OWASP yaygın güvenlik açıklarına karşı kurallar kümesi varsayılan olarak etkindir. Önizleme'de sqli ve xss istekleri denetleme kuralları kümesi içerir. Ek kuralları eklenir. Önceden yapılandırılmış bir kural iş uygulamalarınız için beklendiği gibi doğrulamak için yalnızca eylem günlüğü başlayacağınızı seçebilirsiniz 

## <a name="rate-limiting"></a>Oran sınırlandırma
- Oranı denetimi kuralı, tüm istemci IP olağandışı yüksek trafik çalıştırmanız gerekir.  İstemci IP tarafından bir dakikalık süre izin verilen web isteklerinin sayısı bir eşiği ayarlayabilir.

## <a name="centralized-protection-policy"></a>Merkezi koruma İlkesi
- Birden çok koruma kuralları tanımlayın ve bunları öncelik sırasına göre bir ilke eklemeniz. Özel kurallar, özel durumlara izin vermek için yönetilen ruleset daha yüksek önceliğe sahiptir. Web uygulamanız için tek bir ilke ilişkilidir.  Aynı web uygulama koruma İlkesi, tüm konumlardaki tüm uç sunucularına çoğaltılır, tutarlı bir güvenlik ilkesi tüm bölgelerde emin olun

## <a name="configuration"></a>Yapılandırma
- Önizleme süresince, ön kapısı'nın uygulama koruma kuralları ve ilkeleri oluşturmak ve dağıtmak için REST API'ler, PowerShell veya CLI kullanabilir. Hizmet genel olarak kullanılabilir hale gelmeden önce portala erişim desteklenmez. 


## <a name="monitoring"></a>İzleme
Ön kapısı uyarıları izleyin ve eğilimleri daha kolay izlemek için Azure İzleyici ile tümleşik olan gerçek zamanlı ölçümler kullanarak saldırılarına karşı web uygulamalarını izleme olanağı sağlar.

## <a name="pricing"></a>Fiyatlandırma
Ön kapısı'nın uygulama katmanı güvenliği, Önizleme süresince ücretsizdir.


## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.
