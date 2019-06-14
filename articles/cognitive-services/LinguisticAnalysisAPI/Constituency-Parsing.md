---
title: Constituency ayrıştırma - dil analizi API'si
titlesuffix: Azure Cognitive Services
description: Diğer adıyla "yapısını ayrıştırmaya, tümcecik" ayrıştırma metin tümcecikleri nasıl tanımlar hakkında bilgi edinin.
services: cognitive-services
author: RichardSunMS
manager: nitinme
ms.service: cognitive-services
ms.subservice: linguistic-analysis
ms.topic: conceptual
ms.date: 03/21/2016
ms.author: lesun
ROBOTS: NOINDEX
ms.openlocfilehash: 7611f5f16111b5d8b0d2d293750f658125e50837
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60535445"
---
# <a name="constituency-parsing"></a>Ayrıştırma

> [!IMPORTANT]
> Dilbilimsel Analiz önizleme sürümü 9 Ağustos 2018 tarihinde kullanımdan kaldırılmıştır. Metin işleme ve analiz için [Azure Machine Learning metin analizi modüllerini](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/text-analytics) kullanmanızı öneririz.

("Ayrıştırma olarak da bilinen aşama yapısı") ayrıştırma amacı, metin tümcecikleri belirlemektir.
Bu bilgiler metinden ayıklanırken yararlı olabilir.
Müşteriler, özellik adları veya büyük metin gövdesinden anahtar ifadeleri bulmak ve değiştiriciler ve her bir tümceciği çevreleyen eylemleri görmek için isteyebilirsiniz.

## <a name="phrases"></a>İfadeleri

Bir dil için bir *tümcecik* daha fazlasını bir kelimelerin sırasıdır.
Bir ifade olmasını tümce içinde belirli bir rol oynamaya birlikte gelen bir kelimelerin grubu gerekir.
Bu gruba sözcük birlikte taşınır veya bir bütün olarak değiştirildi ve cümlenin fluent ve dilbilgisi kalmalıdır.

Cümlenin göz önünde bulundurun.

> Bluetooth ile yeni bir karma otomobil bulmak istiyorsanız.

Bu cümleyi isim tümceciğini içeriyor: "bir yeni hibrit Otomobille Bluetooth".
Nasıl Biz bu tümcecik olduğunu biliyor musunuz?
Tam tümcecik önüne geçerek şu cümleyi (biraz poetically) yazabilirsiniz:

> Bluetooth ile yeni bir karma otomobil öğrenmek istiyorum.

Veya benzer bir işlev ve anlamı, "yeni bir başvurmaktan araba" gibi bir ifade ile Biz bu tümceciği yerini alabilir:

> Süslü yeni bir araba bulmak istiyorsanız.

Bunun yerine biz farklı alt sözcük kaldırdıysa değiştirme görevleri garip veya okunamaz cümlelere yol açar.
"Yeni bir öne bulma" taşıma ne olacağını göz önünde bulundurun:

> Yeni Karma otomobil Bluetooth istiyorum bulun.

Sonuçları okumanız ve anlamanız oldukça zor olur.

Bir Ayrıştırıcı amacı, bu tür ifadeleri bulmaktır.
İlginçtir ki, doğal dilde ifadeleri birbiriyle içinde iç içe geçmiş eğilimindedir.
Bu tümcecikleri doğal bir temsilini aşağıdaki gibi bir ağaç verilmiştir:

![Ağaç](./Images/tree.png)

Bu ağacında "NP" işaretli dalları isim deyime dikkat etmelisiniz.
Bu tür ifadeleri vardır: *Ben*, *yeni bir karma otomobil*, *Bluetooth*, ve *Bluetooth ile yeni bir karma otomobil*.

## <a name="phrase-types"></a>İfade türleri

| Etiket | Açıklama | Örnek |
|-------|-------------|---------|
|ADJP   | Sıfat tümceciği | "Bu nedenle rude" |
|ADVP   | Zarfı tümceciği | "clear üzerinden" |
|CONJP  | Birlikte tümceciği | "olarak" |
|FRAG   | Eksik ya da fragmentary girdiler için kullanılan bir parça | "Kesinlikle önerilir..." |
|INTJ   | İnterjection | "Hooray" |
|LST    | Noktalama işaretleri dahil olmak üzere, liste işaretçisi | "#4)" |
|NAC    | Olmayan bir destekçi olmayan tümcecik kapsam göstermek için kullanılan bağlı, |  "ve iyi bir fırsat için" ", işlerinizi ve için iyi bir Dağıt" |
|NP | İsim tümceciği | "tasty Patates Krep" |
|NX | İçinde karmaşık belirli NPs baş işaretlemek için kullanılan| |
|PP | Edat tümceciği| "havuzda" |
|PRN    | Parantez| "(Bu nedenle denir)" |
|PRT    | Parçacık| içinde "kopyalanan"out"" |
|QP | Bir isim deyimi içindeki miktar tümcecik (yani, karmaşık ölçü/tutar)| "$75" |
|RRC    | Azaltılmış göreli yan tümcesi.| "" birçok sorunları hala çözülmemiş"hala çözülmemiş" |
|S  | Veya yan tümce. | "Bir cümle sağlıyor."
|SBAR   | Genellikle bir subordinating birlikte tarafından sunulan alt yan tümcesi | "ı sol" "Anlayabilirim gibi ı biraz."|
|SBARQ  | Wh sözcük veya tümcecik tarafından - tanıtılan doğrudan soru | "Noktası neydi?" |
|SINV   | Ters bildirim temelli cümle | "Hiçbir zaman bunlar kullanan." (Not fiili "bundan sonra" nasıl normal konu "," taşındı) |
|SQ | Evet/Hayır soru veya ana yan ne soru ters | "Araba aldıkları?" |
|UCP    | Eşgüdümlü tümceciği| "küçük ve hatalarda" (nasıl bir sıfat ve preposition tümcecik ile conjoined Not "ve")|
|SORUMLU BAŞKAN YARDIMCISI | Fiili tümceciği | "özdemir ran" |
|WHADJP | Wh sıfat tümceciği | "nasıl rahatsız" |
|WHADVP | Wh zarfı tümceciği| "" |
|WHNP   | Wh isim tümceciği| "hangi Patates", "A'dan ne kadar"|
|WHPP   | Wh prepositional tümceciği| "hangi ülkede"|
|X  | Bilinmeyen, belirsiz veya unbracketable.| ilk "", "... A'dan" |


## <a name="specification"></a>Belirtimi

Buraya ağacı gelen S ifadeleri kullanın [da Treebank](https://catalog.ldc.upenn.edu/LDC99T42).
