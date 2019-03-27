---
author: vhorne
ms.service: application-gateway
ms.topic: include
ms.date: 3/26/2019
ms.author: victorh
ms.openlocfilehash: 5ad1339c04444bcb4cc550be26e239e65227d2ce
ms.sourcegitcommit: fbfe56f6069cba027b749076926317b254df65e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58495213"
---
| Kaynak | Varsayılan limit | Not |
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

<sup>1</sup> WAF özellikli SKU'ları durumunda kaynakların en iyi performans için 40 sayısını sınırla olan öneririz.
