---
title: Konuşma bölümü dil analiz API etiketleme | Microsoft Docs
description: Microsoft Bilişsel Hizmetleri'ndeki konuşma bölümü etiketleme kategori veya konuşma parçası metnin her sözcüğün nasıl tanımlayan öğrenin.
services: cognitive-services
author: RichardSunMS
manager: wkwok
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: article
ms.date: 09/27/2016
ms.author: lesun
ms.openlocfilehash: 90fd5b05c2dabdac88c6c8da288ab629177be38d
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37082647"
---
# <a name="part-of-speech-tagging"></a>Konuşma bölümü etiketleme

## <a name="background-and-motivation"></a>Arka plan ve amacı

Bir metin cümleleri ve belirteçleri olarak ayrılmış durumdadır sonra analiz bir sonraki adım kategori veya bölümü, konuşma her sözcüğün belirlemektir.
Kategoriler gibi bunlar *isim* (genellikle, kişiler, yerler, noktalar, fikirleri gösteren, vb.) ve *fiili* (Eylemler, genellikle temsil eden değişir durumu, vs. Konuşma, bölümü bazı sözcükleri için anlaşılır (örneğin, *quagmire* gerçekten yalnızca bir isimdir), ancak birçok diğerleri için onu ayırt etmek zordur.
*Tablo* sit nerede yer (veya 2-D düzeni sayıların) olmalıdır, ancak aynı zamanda bir tartışma "Tablo" olabilir.

## <a name="list-of-part-of-speech-tags"></a>Konuşma bölümü Etiketler listesi

| Etiket | Açıklama | Örnek sözcükler |
|-----|-------------|---------------|
| $ | dolar | $ |
| \`\` | açılan tırnak işareti | \` \`\` |
| '' | kapanış tırnak işareti | ' '' |
| ( | parantez | ( [ { |
| ) | kapatma ayracı | ) ] } |
| , | virgül | , |
| -- | tire | -- |
| . | tümce Sonlandırıcı | . ! ? |
| : | iki nokta üst üste veya üç nokta | : ; ... |
| BİLGİ | birlikte, Eşgüdümleme | ve ancak veya henüz|
| CD | sayı, Kardinal | dokuz 20 1980 ' 96 |
| DT | determiner |bir bir tüm hem hiçbiri|
| EX | varlıksal vardır | yok |
| FW | Yabancı word | enfant terrible hoi polloi je ne sais quoi |
| IN | preposition veya birlikte bağımlı alt oluşturma| mı, sonra da iç |
| JJ | gelen veya rakamı, sıralı | dokuzuncu oldukça execrable multimodal |
| JJR | gelen, karşılaştırma | daha iyi daha hızlı ucuz |
| JJS | gelen superlative | en iyi hızlı ucuz | 
| LS | liste öğesi işaretçisi | (a) (b) 1 2 A B a b |
| MD | kalıcı yardımcı | gelmelidir |
| NN | isim, ortak, tekil veya yığın | Patates para ayakkabı |
| NNP | isim, uygun, tekil | Kennedy Roosevelt Chicago Weehauken |
| NNPS | isim, uygun, çoğul | Springfields Bushes |
| NNS | isim, ortak çoğul | Parça fareler alanları |
| PDT | Ön determiner | Tüm hem yarı birçok oldukça böyle emin bu |
| POS | genitive işaretçisi | ' ın |
| PRP | gelebildiği, kişisel | aynen ki, t biz bunlar, |
| PRP$ | gelebildiği iyelik | kendi kendi kendi my bizim kendi, |
| RB | Zarfı | clinically yalnızca |
| RBR | Zarfı, karşılaştırma | gloomier grander graver büyük grimmer daha zor harsher healthier daha ağır yüksek ancak büyük sonraki daha yalın lengthier küçüktür kusursuz küçük lonelier uzun daha yüksek alt daha daha fazla... |
| KKY | Zarfı, superlative | en iyi büyük bluntest erken en uzak ilk en ilerideki en zor heartiest yüksek en büyük en az ikinci tightest en kötü en yakın en az |
| RP | Parçacık | üzerinde kapalı yukarı çıkışı hakkında |
| SMG | Simgesi | % & |
| TO | preposition veya infinitive işaretçisi olarak "için" | - |
| HATA | İnterjection | hata howdy hello hooray |
| VB | fiil, temel formu | Ata anında verin |
| VBD | fiil, geçmiş zamanın | atanan vermiş uçtu |
| VBG | fiil, mevcut participle veya gerund | atama Uçan vermiş |
| VBN | fiil, geçmiş participle | atanan verilen beklemektedir |
| VBP | fiil, geçerli zamanın, tekil değil 3 kişi | Ata anında verin |
| VBZ | fiil, geçerli zamanın, tekil 3 kişi | atar uçarak sağlar |
| WDT | UYU determiner | hangi hangi |
| WB | UYU gelebildiği | kimin kime |
| WP$ | UYU-gelebildiği iyelik | , |
| WRB | Uyu zarfı | ancak ne zaman, nerede |

## <a name="specification"></a>Belirtimi

Simgeleştirme için olduğu gibi biz belirtiminden Bel [Penn Treebank](https://catalog.ldc.upenn.edu/ldc99t42).
