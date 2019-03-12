---
title: include dosyası
description: include dosyası
services: frontdoor
author: sharad4u
ms.service: frontdoor
ms.topic: include
ms.date: 9/17/2018
ms.author: sharadag
ms.custom: include file
ms.openlocfilehash: e3fa5616518675d8475937ec63afdd8e1742e8c6
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553846"
---
| Kaynak | Varsayılan limit |
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
| **Karşıya yükleme** |  Her CTE karşıya yükleme 28.6 MB'tan az olduğu sürece sınır yoktur. | Boyut 28.6 büyük olamaz. MB. |
