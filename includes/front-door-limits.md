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
ms.openlocfilehash: a3a43c56a49c243390eac964d31988b7d30fbb56
ms.sourcegitcommit: 280d9348b53b16e068cf8615a15b958fccad366a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58407797"
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
| **Karşıya yükleme** |  Her CTE karşıya yükleme 2 GB'tan daha az olduğu sürece sınır yoktur. | Boyutu 2 GB'tan büyük olamaz. |
