---
title: include dosyası
description: include dosyası
services: frontdoor
author: sharad4u
ms.service: frontdoor
ms.topic: include
ms.date: 05/09/2019
ms.author: sharadag
ms.custom: include file
ms.openlocfilehash: e1f5a1c8229544d97d9ff64748390f0d5237ab97
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66238753"
---
| Resource | Varsayılan/üst sınır |
| --- | --- |
| Abonelik başına Azure ön kapısı hizmet kaynakları | 100 |
| Kaynak başına özel etki alanları içeren ön uç ana bilgisayarları | 100 |
| Her bir kaynak için yönlendirme kuralları | 100 |
| Kaynak başına arka uç havuzları | 50 |
| Arka uç havuz başına arka uçları | 100 |
| Bir yönlendirme kural ile eşleştirilecek yol desenleri | 25 |
| Özel bir web uygulaması güvenlik duvarı kurallarını ilke başına | 10 |
| Web uygulaması güvenlik duvarı ilkesi kaynak başına | 100 |

### <a name="timeout-values"></a>Zaman aşımı değerleri
#### <a name="client-to-front-door"></a>İstemci ön kapısı
- Ön kapısı 61 saniye boşta TCP bağlantı zaman aşımı vardır.

#### <a name="front-door-to-application-back-end"></a>Uygulama arka uca ön kapısı
- Öbekli yanıt yanıt ise 200 varsa veya ilk öbek alındığında döndürülür.
- HTTP isteği arka uca iletilir sonra ön kapısı arka uç ilk paketinden 30 saniye bekler. Ardından istemciye 503 hatası döndürür.
- İlk paket arka ucundan alındıktan sonra ön kapısı bir boşta kalma zaman aşımını 30 saniye bekler. Ardından istemciye 503 hatası döndürür.
- Arka uç TCP oturumu zaman aşımı için ön kapı 30 dakikadır.

### <a name="upload-and-download-data-limit"></a>Karşıya yükleme ve indirme veri sınırı

|  | Öbekli aktarım kodlamasını (CTE) ile | HTTP Öbekleme olmadan |
| ---- | ------- | ------- |
| **İndir** | İndirme boyutu sınırı yoktur. | İndirme boyutu sınırı yoktur. |
| **Karşıya yükleme** |  Her CTE karşıya yükleme 2 GB'tan daha az olduğu sürece sınır yoktur. | Boyutu 2 GB'tan büyük olamaz. |
