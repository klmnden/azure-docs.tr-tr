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
ms.openlocfilehash: deca0034996f6c8ddcac71cd4f191c1a0659b655
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67333396"
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
| Web uygulaması güvenlik duvarı özel Kural başına eşleştirme koşulları | 10 |
| Web uygulaması güvenlik duvarı IP adresi aralıklarını başına koşulu | 600 |
| Web uygulaması güvenlik duvarı dize eşleşme değerleri eşleşme koşulu başına | 10 |
| Web uygulaması güvenlik duvarı dize eşleştirme değer uzunluğu | 256 |
| Web uygulaması güvenlik duvarı POST gövdesini parametre adı uzunluğu | 256 |
| Web uygulaması güvenlik duvarı HTTP üstbilgisi adı uzunluğu | 256 |
| Web uygulaması güvenlik duvarı tanımlama bilgisi adı uzunluğu | 256 |
| Web uygulaması güvenlik duvarı HTTP istek gövdesi boyutu inceledi | 128 KB |
| Web uygulaması güvenlik duvarı özel yanıt gövdesi uzunluğu | 2 KB |

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

### <a name="other-limits"></a>Diğer sınırlamaları
- Ham URL maksimum uzunluğu en fazla URL boyutu - 8.192 bayt - belirtir (düzeni ana bilgisayar adı + bağlantı noktası + yolu + sorgu URL'si dizesi) - en fazla sorgu dizesi boyutunu - 4096 bayt - sorgu dizesi uzunluğu en fazla bayt cinsinden belirtir.