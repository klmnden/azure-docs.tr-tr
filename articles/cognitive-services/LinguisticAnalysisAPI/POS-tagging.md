---
title: Konuşma bölümü etiketleme - dil analizi API'si
description: Dil analizi API'si, konuşma bölümü etiketleme kategori veya metnin her bir sözcüğün sonuna konuşma bölümü nasıl tanımladığına öğrenin.
services: cognitive-services
author: RichardSunMS
manager: cgronlun
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: conceptual
ms.date: 09/27/2016
ms.author: lesun
ROBOTS: NOINDEX
ms.openlocfilehash: a01fcea4ae6c8950d578bacefc2f064586d7306b
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48238541"
---
# <a name="part-of-speech-tagging"></a>Konuşma bölümü etiketleme

> [!IMPORTANT]
> Dil analizi önizlemesi, 9 Ağustos 2018 tarihinde kullanımdan. Kullanmanızı öneririz [Azure Machine Learning metin analiz modüllerini](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/text-analytics) metin işleme ve analiz için.

## <a name="background-and-motivation"></a>Arka plan ve motivasyon

Bir metni tümce ve belirteçlere ayırmaktır ayrılmış sonra sonraki adımı, analiz kategori veya konuşma bölümü her bir sözcüğün belirlemektir.
Kategorileri gibi bunlar *isim* (genellikle, kişiler, yerler, noktalar, fikirleri gösteren, vb.) ve *fiil* (Eylemler, genellikle temsil eden değişiklikleri durumu, vs. -Konuşma bölümü bazı sözcükler için belirsiz değildir (örneğin, *quagmire* yalnızca bir isimdir), ancak birçok diğerleri için bunu ayırt etmek zordur.
*Tablo* bir sit olduğu bir yerde (veya sayı 2B düzeni) olabilir, ancak aynı zamanda bir tartışma "Tablo" olabilir.

## <a name="list-of-part-of-speech-tags"></a>Konuşma bölümü etiketlerin listesi

| Etiket | Açıklama | Örnek sözcükler |
|-----|-------------|---------------|
| $ | dolar | $ |
| \`\` | açılan tırnak işareti | \` \`\` |
| '' | kapanış tırnak işareti | ' '' |
| ( | Açma parantezinden | ( [ { |
| ) | Kapanış ayracı | ) ] } |
| ,  | Virgül | ,  |
| -- | tire | -- |
| . | tümce Sonlandırıcı | . ! ? |
| : | iki nokta üst üste ya da üç nokta | : ; ... |
| BİLGİ | koordine birlikte, | ve ancak veya henüz|
| CD | Kardinal rakamı | dokuz 20 1980 ' 96 |
| DT | determiner |bir bir tüm hem hiçbiri|
| EX | varlıksal vardır | var. |
| FW | Yabancı sözcük | enfant terrible hoi polloi je ne sais quoi |
| GİRİŞ | preposition veya birlikte bağımlı alt oluşturma| bağlı olup olmadığını da iç |
| JJ | sıfat veya sayısal, sıralı | dokuzuncu oldukça execrable multimodal |
| JJR | sıfat karşılaştırma | daha hızlı bir şekilde daha iyi |
| JJS | sıfat superlative | en iyi hızlı, ucuz |
| LS | liste öğesi işaretçisi | (a) (b) 1 2 A B A. b |
| MD | kalıcı yardımcı | nerede depolandığının |
| NN | isim, ortak, tekil veya yığın | Patates para ayakkabı |
| NNP | isim, doğru tekil | Kennedy Roosevelt Chicago Weehauken |
| NNPS | isim, doğru plural | Springfields Bushes |
| NNS | isim, ortak çoğul | parçaları mice alanları |
| PDT | öncesi determiner | Tüm hem yarı birçok oldukça böyle emin bu |
| POS | genitive işaretçisi | ' ın |
| PRP | Kişisel gelebildiği | Filiz kendisi, ı biz bunlar, |
| PRP$ | gelebildiği iyelik | hers, kendi my bizim kendi, |
| RB | Zarfı | clinically yalnızca |
| RBR | Zarfı karşılaştırma | Daha fazla gloomier grander graver büyük grimmer daha zor harsher daha ağır daha yüksek ancak daha büyük daha yalın daha uzun daha az mükemmel daha az lonelier uzun daha yüksek alt daha... |
| KKY | superlative zarfı | en büyük bluntest erken en uzak ilk en uygulamalarınızdaki heartiest yüksek en büyük en az ikinci tightest en kötü en yakın en az |
| RP | Parçacık | üzerinde kapalı yukarı kullanıma hakkında |
| SYM | Sembol | % & |
| TO | preposition veya infinitive işaretçisi olacak şekilde "için" | - |
| HATA | İnterjection | hata hooray howdy Merhaba |
| VB | fiil, taban form | Ata anında verin |
| VBD | fiil, geçmiş şimdiki | atanan verdiğiniz uçtu |
| VBG | fiil, mevcut participle veya gerund | atama Uçan verme |
| VBN | fiil, geçmiş participle | atanan verilen akışı |
| VBP | fiil, şimdiki, tekil değil 3 kişi | Ata anında verin |
| VBZ | fiil, şimdiki, tekil 3 kişi | atar uçarak sağlar |
| WDT | WH determiner | Bu ne olduğu |
| WP | WH gelebildiği | kimin kime |
| WP$ | WH-gelebildiği iyelik | , |
| WRB | Wh zarfı | nasıl ancak her yere |

## <a name="specification"></a>Belirtimi

Simgeleştirme için belirtiminden bağımlı olduğumuz [da Treebank](https://catalog.ldc.upenn.edu/ldc99t42).
