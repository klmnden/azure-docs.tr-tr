---
title: Dil analiz API'de ayrıştırma constituency | Microsoft Docs
description: Ayrıştırma, olarak da bilinen "yapısını ayrıştırmaya, tümcecik" constituency metin tümcecikleri nasıl tanımlayan hakkında bilgi edinin.
services: cognitive-services
author: RichardSunMS
manager: wkwok
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: article
ms.date: 03/21/2016
ms.author: lesun
ms.openlocfilehash: 1cd5ac3eceb9b36654f1b012bce482c5151c4462
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351754"
---
# <a name="constituency-parsing"></a>Ayrıştırma constituency

("Ayrıştırma olarak da bilinen tümcecik yapısı") ayrıştırma constituency amacı, metindeki tümcecikleri belirlemektir.
Bu bilgi metinden çıkarılırken yararlı olabilir.
Müşteriler, özellik adları veya büyük bir metin gövdesi gelen anahtar tümcecikleri bulmak ve değiştiricileri ve her tür tümceciği çevreleyen eylemleri görmek için isteyebilirsiniz.

## <a name="phrases"></a>Deyimleri

Bir dilbilimci için bir *tümcecik* daha fazlasını sözcükleri bir dizisidir.
Bir ifade olması belirli bir rol cümlede yürütmek için bir araya gelmesi bir grup sözcük gerekir.
Sözcükler grubu birlikte taşınan ya da bir bütün olarak değiştirildi ve cümle fluent ve dilbilgisi kalmalıdır.

Tümce göz önünde bulundurun

> Bluetooth ile yeni bir karma otomobil bulmak istediğiniz.

Bu cümleyi isim tümcecik içerir: "bir yeni karma otomobil Bluetooth".
Bu bir tümceciği olduğunu nasıl biliyoruz?
Tüm bu tümceciği öne taşıyarak biz cümle (biraz poetically) yazabilirsiniz:

> Bulmak istediğiniz Bluetooth ile yeni bir karma otomobil.

Veya benzer işlevi ve "yeni bir süslü araba" gibi bir anlam ifade Biz bu tümceciği değiştirin:

> Süslü yeni otomobil bulmak istediğiniz.

Bunun yerine biz sözcüklerin farklı alt kaldırdıysa, bu değiştirme görevleri garip veya okunamaz cümlelere yol açar.
"Yeni bir öne Bul" taşıdığınızda, ne olacağını düşünün:

> Yeni Karma otomobil Bluetooth istiyorum bulun.

Sonuçları okuduğunuzdan ve anladığınızdan oldukça zor.

Bir ayrıştırıcı tüm tümcecikleri bulmanızı hedefidir.
İlginçtir ki, doğal dil tümcecikleri başka içinde iç içe geçmiş eğilimindedir.
Şu tümceciklerden doğal gösterimi aşağıdaki gibi bir ağaç ise:

![Ağaç](./Images/tree.png)

Bu ağacında "NP" olarak işaretlenmiş dalları isim tümcecikleri ' dir.
Birkaç tümcecikleri vardır: *ı*, *yeni bir karma otomobil*, *Bluetooth*, ve *Bluetooth ile yeni bir karma otomobil*.

## <a name="phrase-types"></a>İfade türleri

| Etiket | Açıklama | Örnek |
|-------|-------------|---------|
|ADJP   | Sıfat deyimi | "Bu nedenle utangaç" |
|ADVP   | Zarfı deyimi | "aracılığıyla Temizle" |
|CONJP  | Birlikte deyimi | "yanı" |
|PARÇA   | Parça, eksik veya fragmentary girişleri için kullanılır | "Tavsiye..." |
|INTJ   | İnterjection | "Hooray" |
|LST    | Noktalama işaretleri dahil olmak üzere liste işaretçisi | "#4)" |
|NAC    | Değil A destekçi olmayan ifadesi kapsamı göstermek için kullanılan bağlı, |  "ve iyi bir anlaşma için" ", işlerinizi ve için iyi bir Dağıt" |
|NP | İsim deyimi | "tasty Patates Krep" |
|NX | Head işaretlemek için belirli karmaşık NPs içinde kullanılan| |
|GÖ | Edat deyimi| "havuzunda" |
|PRN    | Parantez| "(Bu nedenle denir)" |
|PRT    | Parçacık| içinde "kopyalanan"out"" |
|QP | Bir isim deyimi içinde miktar tümcecik (yani, karmaşık ölçü/tutar)| "$75" |
|RRC    | Azaltılmış göreli yan tümcesi.| "" birçok sorunları hala çözümlenmemiş"hala çözümlenmemiş" |
|S  | Tümce veya yan tümcesi. | "Bir cümle budur."
|SBAR   | Genellikle subordinating birlikte tarafından sunulan alt yan tümcesi | "g sol" "ı bıraktığınız ı Aranan."|
|SBARQ  | Uyu sözcük veya tümcecik tarafından - tanıtılan doğrudan soru | "Noktası neydi?" |
|SINV   | Ters bildirim temelli cümle | "Hiçbir zaman uyumlu oldukları." (fiil "bundan sonra" nasıl normal konu "," taşındı unutmayın) |
|SQ | Evet/Hayır soru veya ana yan tümcesi uyu soru ters | "Araba elde?" |
|UCP    | Eşgüdümlü deyimi| "küçük ve hatalarda" (nasıl gelen ve bir preposition deyimi ile conjoined Not "ve")|
|SES İZİ | Fiili deyimi | "özdemir çalışma konumu" |
|WHADJP | Uyu gelen deyimi | "nasıl rahatsız" |
|WHADVP | Uyu zarfı deyimi| "zaman" |
|WHNP   | Uyu isim deyimi| "Patates hangi", "Çorbası ne kadar"|
|WHPP   | Uyu prepositional deyimi| "hangi ülkede"|
|X  | Bilinmeyen, belirsiz veya unbracketable.| ilk "", "... Çorbası" |


## <a name="specification"></a>Belirtimi

Burada ağaçları kullanma gelen S ifadeleri [Penn Treebank](https://www.cis.upenn.edu/~treebank/).
