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
ms.openlocfilehash: f0c2d1501b9aa19dec8c4ad157e004a57e0e5070
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47006540"
---
| Kaynak | Varsayılan limit |
| --- | --- |
| Abonelik başına ön kapısı kaynakları | 100 |
| Kaynak başına özel etki alanları dahil olmak üzere ön uç ana bilgisayarları | 100 |
| Her bir kaynak için yönlendirme kuralları | 100 |
| Kaynak başına arka uç havuzları | 50 |
| Arka uç havuz başına arka uçları | 100 |
| Bir yönlendirme kural ile eşleştirilecek yol desenleri | 25 |
| Özel bir web uygulaması güvenlik duvarı kurallarını ilke başına | 10 |
| Web uygulaması güvenlik duvarı ilkesi kaynak başına | 100 |

### <a name="timeout-values"></a>Zaman aşımı değerleri
#### <a name="client-to-front-door"></a>İstemci ön kapısı
- Ön kapısı 61 saniye boşta TCP bağlantı zaman aşımı vardır.
#### <a name="front-door-to-application-backend"></a>Uygulama arka uca ön kapısı
- Öbekli yanıt yanıt ise 200 döndürülecek olursa / ilk öbek alındığında.
- HTTP isteği arka uca iletilir sonra 30 saniye için arka uç, ilk paketi 503 hatası istemciye döndürmeden önce ön kapısı bekler.
- İlk paketi arka ucundan alındıktan sonra ön kapısı 503 hatası istemciye döndürmeden önce (boşta kalma zaman aşımı) 30 saniye bekler.
- Arka uç TCP oturumu zaman aşımı için ön kapı 30 dakikadır.

### <a name="upload--download-data-limit"></a>Karşıya yükleme / veri sınırı indirin

|  | Öbekli aktarım kodlamasını (CTE) ile | HTTP Öbekleme olmadan |
| ---- | ------- | ------- |
| **İndir** | İndirme boyutu sınırı yoktur | İndirme boyutu sınırı yoktur |
| **Karşıya yükleme** |  Her CTE karşıya sürece 28.6 MB'tan küçük bir sınır yoktur | Boyut 28.6 MB'den büyük olamaz. |
