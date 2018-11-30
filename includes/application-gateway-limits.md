---
author: vhorne
ms.service: application-gateway
ms.topic: include
ms.date: 11/09/2018
ms.author: victorh
ms.openlocfilehash: 3d66d825306c5183bdd8d8e611d98904eef2022a
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52440507"
---
| Kaynak | Varsayılan limit | Not |
| --- | --- | --- |
| Application Gateway |Abonelik başına 1000 | |
| Ön Uç IP Yapılandırmaları |2 |1 ortak ve 1 özel |
| Ön Uç Bağlantı Noktaları |40 | |
| Arka Uç Adres Havuzları |40 | |
| Havuz Başına Arka Uç Sunucuları |1200 | |
| HTTP Dinleyicileri |40 | |
| HTTP yük dengeleme kuralları |400 |# HTTP dinleyicilerinin * n |
| Arka uç HTTP ayarları |40 | |
| Ağ geçidi başına örnek |75 | |
| SSL sertifikaları |40 |HTTP Dinleyicileri başına 1 |
| Kimlik doğrulama sertifikaları |40 | |
| İstek zaman aşımına min |1 saniye | |
| En fazla istek zaman aşımı |24 saat | |
| Site sayısı |40 |HTTP Dinleyicileri başına 1 |
| Dinleyici başına URL Eşlemeleri |1 | |
| URL eşlemesi başına en fazla yol temelli kurallar|100|
| Yeniden yönlendirme yapılandırmaları |40| |
| Eş zamanlı WebSocket bağlantılarını |5000| |
|URL uzunluğu üst sınırı|8000|
| En fazla dosya yükleme boyutunu standart |2 GB | |
| En büyük dosya karşıya yükleme boyutu WAF |Orta WAF ağ geçitleri - 100 MB<br>Geniş WAF ağ geçitleri - 500 MB| |
|WAF gövdesi boyutu sınırı (olmadan dosyaları)|128 KB|
