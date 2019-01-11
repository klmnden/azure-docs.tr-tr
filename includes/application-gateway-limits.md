---
author: vhorne
ms.service: application-gateway
ms.topic: include
ms.date: 11/09/2018
ms.author: victorh
ms.openlocfilehash: 66dea07a1ff725c6707b19bc6ebdc5563f1b158b
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54211926"
---
| Kaynak | Varsayılan limit | Not |
| --- | --- | --- |
| Application Gateway |Abonelik başına 1000 | |
| Ön uç IP yapılandırmaları |2 |1 ortak ve 1 özel |
| Ön uç bağlantı noktaları |100<sup>1</sup> | |
| Arka uç adres havuzları |100<sup>1</sup> | |
| Havuz başına arka uç sunucuları |1200 | |
| HTTP Dinleyicileri |100<sup>1</sup> | |
| HTTP yük dengeleme kuralları |100<sup>1</sup> | |
| Arka uç HTTP ayarları |100<sup>1</sup> | |
| Ağ geçidi başına örnek |32 | |
| SSL sertifikaları |100<sup>1</sup> |HTTP Dinleyicileri başına 1 |
| Kimlik doğrulama sertifikaları |100 | |
| Güvenilen kök sertifikalar |100 | |
| İstek zaman aşımına min |1 saniye | |
| En fazla istek zaman aşımı |24 saat | |
| Site sayısı |100<sup>1</sup> |HTTP Dinleyicileri başına 1 |
| Dinleyici başına URL eşlemeleri |1 | |
| URL eşlemesi başına en fazla yol temelli kurallar|100||
| Yeniden yönlendirme yapılandırmaları |100<sup>1</sup>| |
| Eş zamanlı WebSocket bağlantılarını |5000| |
| URL uzunluğu üst sınırı|8000||
| En fazla dosya yükleme boyutunu standart |2 GB | |
| En büyük dosya karşıya yükleme boyutu WAF |Orta WAF ağ geçitleri - 100 MB<br>Geniş WAF ağ geçitleri - 500 MB| |
| WAF gövdesi boyutu sınırı (olmadan dosyaları)|128 KB||

<sup>1</sup> WAF özellikli SKU olması durumunda, kaynakları en iyi performans için 40 sayısını sınırlayın önerilir.
