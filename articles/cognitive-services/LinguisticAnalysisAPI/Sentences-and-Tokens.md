---
title: Cümleler ve belirteçler - dil analizi API'si
titlesuffix: Azure Cognitive Services
description: Tümce ve belirteçlere dil analizi API'si, ayırma hakkında bilgi edinin.
services: cognitive-services
author: DavidLiCIG
manager: nitinme
ms.service: cognitive-services
ms.subservice: linguistic-analysis
ms.topic: conceptual
ms.date: 03/21/2016
ms.author: davl
ROBOTS: NOINDEX
ms.openlocfilehash: 435513023cf74bbc259cb922220d5f9940452d79
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60635475"
---
# <a name="sentence-separation-and-tokenization"></a>Tümce ve belirteçlere ayırma

> [!IMPORTANT]
> Dilbilimsel Analiz önizleme sürümü 9 Ağustos 2018 tarihinde kullanımdan kaldırılmıştır. Metin işleme ve analiz için [Azure Machine Learning metin analizi modüllerini](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/text-analytics) kullanmanızı öneririz.

## <a name="background-and-motivation"></a>Arka plan ve motivasyon

Bir metin gövdesi göz önünde bulundurulduğunda, ilk adımı, dil analizi, tümce ve belirteçlere ayırmaktır bölmektir.

### <a name="sentence-separation"></a>Tümce ayrımı

İlk bakışta üzerinde metin cümleler bozucu basit olduğunu görünüyor: yalnızca cümle son işaretlerinin bulun ve kesme tümceleri vardır.
Ancak, bu işaretler genellikle karmaşık ve belirsiz.

Aşağıdaki örnek metni göz önünde bulundurun:

> Ne dedin?!? Yönetmenin "Yeni Teklif hakkında." yanıt alamadık Bay ve Mrs. Smith için önemlidir.

Bu metin, üç cümleler içerir:

- Ne dedin?!?
- Yönetmenin "Yeni Teklif hakkında." yanıt alamadık
- Bay ve Mrs. Smith için önemlidir.

Cümleleri ucunda çok farklı yollarla nasıl işaretlenmiş unutmayın.
İlk soru işareti (bazen bir interrobang denir) ünlem ve bir arada sona erer.
Önceki cümle nokta veya tam durdurma, ancak aşağıdaki tırnak işareti ikinci biter çekilmesi.
Üçüncü cümlede kısaltmalar de işaretlemek için aynı, nokta karakteri'nın nasıl kullanılabileceğini görebilirsiniz.
Noktalama işaretlerinin yalnızca arayan bir iyi aday kümesi sağlar, ancak daha fazla iş true cümle sınırlarını tanımlamak için gereklidir.

### <a name="tokenization"></a>Simgeleştirme

Sıradaki görev, bu cümleleri belirteçlere ayırmasına sağlamaktır.
Çoğunlukla, İngilizce belirteçleri beyaz boşluk tarafından ayrılmış.
(Belirteçleri veya sözcükler bulma burada alanları kelimeler arasındaki çoğunlukla değil Çince, İngilizce çok daha kolay kullanılır.
İlk cümle "Whatdidyousay?" yazılmış olabilir)

Zor bazı durumlar vardır.
İlk olarak, genellikle (ama her zaman kullanılmaz) noktalama gereken, bölme, içeriğini çevreleyen uzağa.
İkinci olarak, İngilizce olan *kısaltmalar*, "siz" veya "sadece değil", burada sözcükler alınan sıkıştırılmış ve daha küçük parçalara kısaltılmış gibi.
Simgeleştirici karakter dizisi sözcüklere bölmek için hedeftir.

Yukarıdaki örnek cümleleri şimdi geri dönün.
Şimdi biz "merkezi dot" koyduğunuz (&middot;) arasındaki her farklı bir belirteç.

- Hangi &middot; yaptığınız &middot; , &middot; söyleyin &middot; ?!?
- Ben &middot; vermedi &middot; ma &middot; dinleyin &middot; hakkında &middot; &middot; Direktörü &middot; 's &middot; " &middot; yeni &middot; teklifi &middot; . &middot; "
- Bunu &middot; 's &middot; önemli &middot; için &middot; Bay &middot; ve &middot; Mrs. &middot; Smith &middot; .

Bulma sözlüğe sözcüklerin nasıl çoğu belirteçleridir unutmayın (örneğin, *önemli*, *Direktörü*).
Diğerleri yalnızca noktalama işaretlerini oluşur.
Son olarak, kısaltmalar gibi temsil etmek için daha olağan dışı belirteçleri vardır *ma* için *değil*, iyelik gibi *'s*.
Word işlemek bu simgeleştirme kurmamızı *yaramadı* ve tümcecik *belirtmiyor* daha tutarlı bir şekilde.

## <a name="specification"></a>Belirtimi

Ne bir cümle ve bir belirteç oluşur konusunda tutarlı kararlar önemlidir.
Belirtiminden bağımlı olduğumuz [da Treebank](https://catalog.ldc.upenn.edu/LDC99T42) (ftp://ftp.cis.upenn.edu/pub/treebank/public_html/tokenization.html bazı ek ayrıntılar kullanılabilir).
