---
title: Verileriniz veri bilimi için hazır mı? Veri değerlendirmesi - Azure Machine Learning | Microsoft Docs
description: Dört ölçüt verilerinizi veri bilimi için hazır olmasını gereksinimlerini karşılamak için. Bu görüntü ile temel veri değerlendirme yardımcı olmak için somut örnekler vardır.
keywords: ilgili verileri verileri değerlendirmek, verileri, veri ölçütlerini, veri hazır hazırlayın
services: machine-learning
documentationcenter: na
author: heatherbshapiro
ms.author: hshapiro
manager: hjerez
editor: cjgronlund
ms.assetid: d502062c-da70-4b21-9054-0bfd9902612e
ms.service: machine-learning
ms.component: studio
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2018
ms.openlocfilehash: 5ab3c7716485053432240cb74be8ebc60c9ad274
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34834626"
---
# <a name="is-your-data-ready-for-data-science"></a>Verileriniz veri bilimi için hazır mı?
## <a name="video-2-data-science-for-beginners-series"></a>Video 2: Veri bilimi yeni başlayanlar seri için
Verilerinizi veri bilimi için hazır olması için temel ölçütlerini karşıladığından emin olmak için değerlendirilecek öğrenin.

Serinin en dışında almak için tümünü izleyin. [Videolar listesine Git](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Shows/SupervisionNotRequired/9/player]
>
>

## <a name="other-videos-in-this-series"></a>Bu serideki diğer videolar
*Yeni başlayanlar için veri bilimi* veri bilimi beş kısa videolar içindeki bir giriş değil.

* Video 1: [5 veri bilimi yanıtlar sorular](data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 dakika 14 saniye)*
* Video 2: verileriniz için veri bilimi hazır mı?
* Video 3: [verilerle yanıt soru](data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 sn)*
* Video 4: [basit bir modelle bir yanıt tahmin](data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min 42 sn)*
* Video 5: [veri bilimi yapmak için diğer kişilerin çalışma kopyalama](data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 dakika 18 saniye)*

## <a name="transcript-is-your-data-ready-for-data-science"></a>Dökümü: verileriniz için veri bilimi hazır mı?
Hoş Geldiniz "verileriniz için veri bilimi hazır mı?" serideki ikinci video *yeni başlayanlar için veri bilimi*.  

Veri bilimi istediğiniz yanıt vermeden önce bazı yüksek kaliteli hammaddeleri çalışmak için vermeniz gerekir. Daha iyi bir pizza daha iyi son ürünle, başlangıç malzemeleri yalnızca yapma gibi. 

## <a name="criteria-for-data"></a>Veriler için ölçüt
Veri bilimi içinde birlikte dahil olmak üzere çekebilir gereken belirli malzemeleri vardır:

* İlgili
* Bağlanıldı
* Doğru
* Yeterli çalışmak için

## <a name="is-your-data-relevant"></a>Verilerinizi ilgili mi?
Bu nedenle ilk tarifi - ilgili olan verileri gerekir.

![İlgili verileri ilgisiz verilerin - karşılaştırması veri değerlendir](./media/data-science-for-beginners-is-your-data-ready-for-data-science/relevant-and-irrelevant-data.png)

Sol bölmede, tablo dışında Boston çubuğu, son kullanıcıların oyun kırmızı Sox batting ortalama ve en yakın Market sütlü fiyat test yedi kişi kan Alkol düzeyini gösterir.

Bu tüm mükemmel yasal verilerdir. İlgili değil, yalnızca hataya değildir. Bu sayılar arasında belirgin ilişkisi yoktur. Birisi size geçerli fiyat sütlü ve kırmızı Sox batting ortalama verdiyse kan Alkol içeriklerini tahmin edebilir yolu yoktur.

Şimdi sağ tarafta tabloya bakın. Bu sefer her birinin gövde yığın sahip oldukları İçecekler sayısını yanı sıra ölçülen.  Her satır numaraları şimdi birbiriyle ilgili olur. I size my gövde verdiyse yığın ve ı vardı Margaritas sayısı, yaptığınız my kan konumundaki tahmin Alkol içerik.

## <a name="do-you-have-connected-data"></a>Sahip bağlı veri?
Sonraki tarifi bağlı verilerdir.

![Bağlı veri bağlantısı kesilmiş verileri - veri ölçütlerini hazır karşılaştırması](./media/data-science-for-beginners-is-your-data-ready-for-data-science/connected-vs-disconnected-data.png)

Hamburgers kalitesini bazı ilgili bilgiler aşağıdadır: sıcaklık, patty ağırlık ve dergi yerel yemek derecesi maske. Ancak sol tablodaki boşlukları dikkat edin.

Çoğu veri kümelerini bazı değerler eksik. Böyle delik sağlamak için yaygın bir durumdur ve etrafında çalışmaya yolu vardır. Ancak, verilerinizi varsa çok fazla eksik, İsviçre Peynir gibi ara başlar.

Sol tarafta tabloya bakarsanız, olmadığından eksik kadar veri grill sıcaklık ve patty ağırlık ilişkiyi her türlü gündeme sabit. Bu örnek, bağlantısı kesilmiş veri gösterir.

Ancak sağ taraftaki tablo dolu olduğunda ve tamamlandı - bağlı veri örneği.

## <a name="is-your-data-accurate"></a>Verilerinizi geçerli mi?
Sonraki tarifi doğruluğu ' dir. İsabet dört hedefleri şunlardır.

![Doğru verileri yanlış data - veri ölçütlerini karşılaştırması](./media/data-science-for-beginners-is-your-data-ready-for-data-science/inaccurate-vs-accurate-data.png)

Sağ üst hedef bakın. Şu hedefe tam isabet etmiş göz etrafında sıkı bir gruplandırma yok. Doğal olarak, doğru olur. Arasında veri bilimi dilde altındaki hedef sağdaki performans da doğru olarak değerlendirilir.

Bu okları merkezi eşlenmiş, bu çok hedefe tam isabet etmiş göz yakın olduğunu görürsünüz. Oklar, tüm hedef geçici kesin olmayan kabul ancak doğru kabul şekilde hedefe tam isabet etmiş göz ortalanmış yayılır.

Artık sol üst hedefe arayın. Burada okları çok birbirine sıkı bir gruplandırma ulaştı. Kesin, ancak merkezi şekilde hedefe tam isabet etmiş göz devre dışı olduğundan, bunlar yanlış. Sol alt hedef okları yanlış ve kesin. Bu archer daha fazla uygulama gerekir.

## <a name="do-you-have-enough-data-to-work-with"></a>Çalışmak için yeterli veri var mı?
Son olarak, malzeme #4 yeterli verilerdir.

![Analiz için yeterli veri var mı? Veri değerlendirme](./media/data-science-for-beginners-is-your-data-ready-for-data-science/barely-enough-data.png)

Her veri noktası tablosundaki boyama içinde fırça vuruş olarak düşünün. Yalnızca birkaç tanesi varsa, boyama benzer - ne olduğunu ayırt etmek zordur.

Daha fazla bazı fırça vuruşları eklerseniz, boyama biraz daha net alma başlar.

Neredeyse hiç yeterli vuruşları sahip olduğunuzda, yalnızca bazı geniş kararlar almak için yeterli görürsünüz. Bu bir yere ziyaret etmek istediğiniz mi? Açık görünüyor, temiz su gibi – Evet, burada tatile kullanacağım olan arar.

Daha fazla veri ekleme gibi resmi daha anlaşılır olur ve daha ayrıntılı kararlarını verebilir. Artık sol bank üzerinde üç Oteller bakabilirsiniz. Ön planda biri mimari özelliklere dikkat edin. Görünüm nedeniyle üzerinde üçüncü kat kalmak bile seçebilirsiniz.

İlgili, bağlı, doğru veri ve yeterli ile sahip tüm malzemeleri gereken bazı yüksek kaliteli veri bilimi yapmak.

Diğer dört videoları kullanıma özen *yeni başlayanlar için veri bilimi* Microsoft Azure Machine learning'in.

## <a name="next-steps"></a>Sonraki adımlar
* [İlk veri bilimi denemeyi Machine Learning Studio'da deneyin](create-experiment.md)
* [Microsoft Azure Machine Learning giriş Al](what-is-machine-learning.md)
