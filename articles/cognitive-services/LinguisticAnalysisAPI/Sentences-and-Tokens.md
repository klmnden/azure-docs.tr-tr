---
title: Cümleleri ve dil analiz API belirteçleri | Microsoft Docs
description: Tümce ayırma ve Bilişsel hizmetler dil analiz API'sindeki simgeleştirme hakkında bilgi edinin.
services: cognitive-services
author: DavidLiCIG
manager: wkwok
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: article
ms.date: 03/21/2016
ms.author: davl
ms.openlocfilehash: 78e539f365728ad540308e9cfb07af44bf6d8fe7
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37084051"
---
# <a name="sentence-separation-and-tokenization"></a>Tümce ayırma ve belirteç oluşturma

## <a name="background-and-motivation"></a>Arka plan ve amacı

Metin gövdesi verildiğinde, ilk dil analiz cümleleri ve belirteçler ayırmak için bir adımdır.

### <a name="sentence-separation"></a>Tümce ayırma

İlk bakışta metin cümleleri çiğnemekten basit olduğunu görünüyor: yalnızca cümle son işaretçileri bulmak ve bölün cümleleri vardır.
Ancak, bu işaretleri genellikle karmaşık ve belirsiz.

Aşağıdaki örnek metin göz önünde bulundurun:

> Ne dedin?!? Yönetmenin "Yeni teklifi hakkında." yanıt alamadık Bay ve Merve Smith için önemlidir.

Bu metin üç cümleleri içerir:

- Ne dedin?!?
- Yönetmenin "Yeni teklifi hakkında." yanıt alamadık
- Bay ve Merve Smith için önemlidir.

Cümleleri uçlarından çok farklı yollarla nasıl işaretlenmiş unutmayın.
İlk soru işaretlerini (bazen bir interrobang denir) ünlem ve bir arada sona erer.
İkinci uç nokta veya tam durma, ancak aşağıdaki tırnak işareti önceki cümle çekilen.
Üçüncü cümlede de kısaltmalar işaretlemek için aynı bu nokta karakteri'nın nasıl kullanılabileceğini görebilirsiniz.
Yalnızca noktalama arayan iyi bir adaydır kümesi sağlar, ancak daha fazla iş true cümle sınırlarını tanımlamak için gereklidir.

### <a name="tokenization"></a>Simgeleştirme

Sonraki görev Bu cümleleri belirteçlere ayırmasına belirlemektir.
Çoğunlukla, İngilizce belirteçleri boşluk tarafından sınırlandırılır.
(Belirteçleri veya sözcükleri bulma nerede alanları sözcükler arasında çoğunlukla değil Çince, İngilizce'den çok daha kolay kullanılır.
İlk cümle "Whatdidyousay?" yazılmış)

Birkaç zor durumlar vardır.
İlk olarak, genellikle (ancak her zaman) noktalama gereken, bölünmüş bağlam çevreleyen, ayrılmayın.
İkinci olarak, İngilizce sahip *kısaltmalar*, "siz" veya "Bu", burada sözcükler alınan sıkıştırılmış ve daha küçük parçalara kısaltılmış gibi. Belirteç Oluşturucu sözcüklere karakter dizisi bölüneceği hedefidir.

Şimdi örnek cümleleri üstten dönün.
Biz "merkezi nokta" yerleştirdiğiniz artık (&middot;) her ayrı belirteç arasında.

- Ne &middot; vermedi &middot; , &middot; söyleyin &middot; ?!?
- I &middot; vermedi &middot; ma &middot; duymak &middot; hakkında &middot; &middot; director &middot; 's &middot; " &middot; yeni &middot; teklifi &middot; . &middot; "
- Bu &middot; 's &middot; önemli &middot; için &middot; Bay &middot; ve &middot; Mrs. &middot; Smith &middot; .

Nasıl çoğu belirteçleri Bul sözlükte sözcüklerdir unutmayın (örneğin, *önemli*, *director*).
Başkalarının yalnızca noktalama oluşur.
Son olarak, kısaltmalar gibi göstermek için daha olağan dışı belirteçleri vardır *ma* için *değil*, iyelik ister *'s*, vb. Bu simgeleştirme bize word işlemeye izin verir *kaydetmedi* ve ifade *belirtmiyor* daha tutarlı bir şekilde örneği için.

## <a name="specification"></a>Belirtimi

Ne bir cümle ve bir belirteç oluşur hakkında tutarlı kararları önemlidir.
Biz belirtiminden Bel [Penn Treebank](https://catalog.ldc.upenn.edu/ldc99t42) (bazı ek ayrıntıları ftp://ftp.cis.upenn.edu/pub/treebank/public_html/tokenization.html kullanılabilir).
