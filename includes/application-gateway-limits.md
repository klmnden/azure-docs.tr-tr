---
author: vhorne
ms.service: application-gateway
ms.topic: include
ms.date: 6/5/2019
ms.author: victorh
ms.openlocfilehash: 592e1973344b231693077f8286a41dfd67a8d188
ms.sourcegitcommit: 6932af4f4222786476fdf62e1e0bf09295d723a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66689097"
---
| Resource | Varsayılan/üst sınır | Not |
| --- | --- | --- |
| Azure Application Gateway |Abonelik başına 1.000 | |
| Ön uç IP yapılandırmaları |2 |1 ortak ve 1 özel |
| Ön uç bağlantı noktaları |100<sup>1</sup> | |
| Arka uç adres havuzları |100<sup>1</sup> | |
| Havuz başına arka uç sunucuları |1,200 | |
| HTTP dinleyicileri |100<sup>1</sup> | |
| HTTP Yük Dengeleme kuralları |100<sup>1</sup> | |
| Arka uç HTTP ayarları |100<sup>1</sup> | |
| Ağ geçidi başına örnek |32 | |
| SSL sertifikaları |100<sup>1</sup> |HTTP dinleyicileri başına 1 |
| SSL sertifikası üst sınırı |V1 SKU - 10 KB<br>V2 SKU - 25 KB| |
| Kimlik doğrulama sertifikaları |100 | |
| Güvenilen kök sertifikalar |100 | |
| İstek zaman aşımı en düşük |1 saniye | |
| En fazla istek zaman aşımı |24 saat | |
| Site sayısı |100<sup>1</sup> |HTTP dinleyicileri başına 1 |
| Dinleyici başına URL eşlemeleri |1 | |
| URL eşlemesi başına en fazla yol temelli kurallar|100||
| Yeniden yönlendirme yapılandırmaları |100<sup>1</sup>| |
| Eş zamanlı WebSocket bağlantılarını |Orta ağ geçitleri 20 bin<br> Büyük ağ geçitleri 50 bin| |
| URL uzunluğu üst sınırı|8,000||
| En büyük dosya yükleme boyutunu, standart |2 GB | |
| En büyük dosya karşıya yükleme boyutu WAF |Orta WAF ağ geçitleri, 100 MB<br>Geniş WAF ağ geçitleri, 500 MB| |
| Dosyaları olmadan WAF gövdesi boyutu sınırı|128 KB||
|En fazla WAF özel kurallar|100||

<sup>1</sup> WAF özellikli SKU'ları durumunda kaynakların en iyi performans için 40 sayısını sınırla olan öneririz.
