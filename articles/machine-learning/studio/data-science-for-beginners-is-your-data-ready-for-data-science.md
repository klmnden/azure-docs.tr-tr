---
title: Veri değerlendirme
titleSuffix: Azure Machine Learning Studio
description: Dört ölçütleri verilerinizin veri bilimi için hazır olması için karşılaması gerekir. Bu video ile temel veri değerlendirme yardımcı olmak için somut örnekler vardır.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: sdgilley
ms.author: sgilleye
ms.custom: seodec18
ms.date: 03/22/2019
ms.openlocfilehash: d38a5066304a11ff2cd53a0168e51a0d74fda555
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58370355"
---
# <a name="is-your-data-ready-for-data-science"></a>Verileriniz veri bilimi için hazır mı?
## <a name="video-2-data-science-for-beginners-series"></a>Video 2: Seri yeni başlayanlar için veri bilimi
Veri bilimi için hazır olmasını temel ölçütleri karşıladığından emin olmak için verilerinizi değerlendirin öğrenin.

En yetersiz serisi almak için tüm bunları izleyin. [Videoları listesine Git](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Shows/SupervisionNotRequired/9/player]
>
>

## <a name="other-videos-in-this-series"></a>Bu serideki diğer videolar
*Yeni başlayanlar için veri bilimi* beş kısa videoyu veri bilimine hızlı bir giriş niteliğindedir.

* Video 1: [5 veri biliminin yanıtladığı sorular](data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 en az 14 sn)*
* Video 2: Verileriniz veri bilimi için hazır mı?
* Video 3: [Yanıt verileri ile soru](data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 dk 17 sn)*
* Video 4: [Basit bir model ile yanıtı tahmin etme](data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 dk 42 sn)*
* Video 5: [Veri bilimi için başkalarının çalışmalarını kopyalama](data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 dk 18 sn)*

## <a name="transcript-is-your-data-ready-for-data-science"></a>Transkript: Verileriniz veri bilimi için hazır mı?
Hoş Geldiniz "verileriniz veri bilimi için hazır mı?" serinin ikinci videoda *yeni başlayanlar için veri bilimi*.  

Veri bilimi istediğiniz yanıt vermeden önce çalışmak için bazı yüksek kaliteli ham madde vermeniz gerekir. Pizza, daha iyi, daha iyi son ürün Başlat malzemeleri yalnızca yapma gibi. 

## <a name="criteria-for-data"></a>Veriler için ölçüt
Veri bilimi içinde birlikte dahil olmak üzere çekilmesi gereken belirli malzemeleri vardır:

* İlgili
* Bağlanıldı
* Doğru
* Çalışmak için yeterli

## <a name="is-your-data-relevant"></a>Verilerinizi ilgili mi?
Bu nedenle ilk OS'nin - ilgili veriler gerekir.

![İlgili verileri ve ilgisiz verilerin - veri değerlendir](./media/data-science-for-beginners-is-your-data-ready-for-data-science/relevant-and-irrelevant-data.png)

Sol tarafta, tablonun Boston çubuğu, son oyunu kırmızı Sox batting ortalama ve en yakın Market içinde sütlü fiyatı dışında test yedi kişiler tansiyon Alkol düzeyini gösterir.

Tüm mükemmel yasal veri budur. Onun yalnızca ilgili olmayan bir hatadır. Bu sayı arasında belirgin bir ilişki yoktur. Birisi size sütlü ve kırmızı Sox batting ortalama geçerli fiyatını verdiyse tansiyon Alkol içeriklerini tahmin edilemedi hiçbir yolu yoktur.

Şimdi sağdaki tabloya bakın. Bu sefer her kişinin gövdesi yığın, sahip oldukları İçecekler sayısını yanı sıra ölçüldü.  Her satır numaraları artık birbiriyle ilgili olur. My gövdesi miyim verdiğiniz, yığın ve ben sahip Margaritas sayısı, yaptığınız bir tahmin my tansiyon adresindeki Alkol içerik.

## <a name="do-you-have-connected-data"></a>Olan bağlı veri?
Sonraki OS'nin bağlı verilerdir.

![Bağlantısı kesilmiş verileri - veri ölçütlerini hazır karşılaştırmalı bağlı](./media/data-science-for-beginners-is-your-data-ready-for-data-science/connected-vs-disconnected-data.png)

Hamburgers kalitesini ilgili bazı bilgiler aşağıdadır: sıcaklık ve patty ağırlık dergi yerel Gıda derecesi maske. Ancak, sol taraftaki tablonun boşlukları dikkat edin.

Çoğu veri kümesinde bazı değerler eksik. Böyle boşluklarını çok yaygındır ve etrafında çalışmak için vardır. Ancak, verileriniz varsa çok fazla eksik, İsviçre peynirlerine ayırıyor gibi ara başlar.

Sol taraftaki tabloya bakarsanız, eksik kadar veri grill sıcaklık ve patty ağırlık arasındaki ilişki her türlü gündeme zor, bu nedenle yoktur. Bu örnek, bağlantısı kesilmiş verileri gösterir.

Ancak sağ taraftaki tablo dolu ve tamamlandı - bağlı veri örneği.

## <a name="is-your-data-accurate"></a>Verilerinizi geçerli mi?
Sonraki içerik doğruluğu ' dir. İsabet dört hedefleri şunlardır.

![Doğru veri yanlış bir veri - veri ölçütlerini karşılaştırması](./media/data-science-for-beginners-is-your-data-ready-for-data-science/inaccurate-vs-accurate-data.png)

Sağ üst köşedeki hedef bakın. Şu hedefe tam isabet etmiş göz etrafında sıkı bir gruplandırma yoktur. Doğal olarak, doğru olur. Arasında veri bilimi dilde hedef hemen altındaki performans da doğru olarak kabul edilir.

Bu oklardan merkezimizi eşlenen ederseniz, bu çok hedefe tam isabet etmiş göz yakın olduğunu görürsünüz. Oklar, tüm hedef, geçici olarak kesin kabul edilmeleri ancak doğru kabul edilmeleri için hedefe tam isabet etmiş göz ortalanmış yayılır.

Artık sol hedefte arayın. Burada oklar birbirine yakın çok sıkı bir gruplandırma basın. Bunlar tam, ancak merkezi şekilde hedefe tam isabet etmiş gözü kapalı olduğundan, bunlar tutarsız. Şemadaki oklar, sol alt hedef yanlış ve kesin. Daha fazla uygulama bu archer gerekir.

## <a name="do-you-have-enough-data-to-work-with"></a>Çalışmak için yeterli veri var mı?
Son olarak, içerik #4 yeterli verilerdir.

![Analiz için yeterli veri var mı? Veri değerlendirme](./media/data-science-for-beginners-is-your-data-ready-for-data-science/barely-enough-data.png)

Her veri noktası tablosunda bir fırça vuruşu boyama içinde olarak düşünün. Yalnızca birkaç tanesi varsa boyama belirsiz - ne olduğunu söylemek zordur.

Daha fazla bazı fırça darbesi eklerseniz, boyama biraz daha net alma başlar.

Şekilleri yeterli strokes varsa, yalnızca bazı geniş kararları vermek için yeterli görürsünüz. Bu, bir yere gitmek istediğiniz mi? Parlak görünüyor, bu gibi temiz su – Evet, burada tatile gidiyorum olan görünüyor.

Daha fazla veri ekledikçe resmi daha anlaşılır hale gelir ve daha ayrıntılı kararlar verebilirsiniz. Artık sol bank üzerinde üç hotels bakabilirsiniz. Ön plan bir mimari özelliklerinin olduğunu fark edebilirsiniz. Hatta üçüncü katında nedeniyle görünüm kalmak seçebilirsiniz.

İlgili, bağlı olarak doğru veri ile yeteri kadar sahip tüm malzemeleri gereken bazı yüksek kaliteli veri bilimi.

Diğer dört videoları kullanıma mutlaka *yeni başlayanlar için veri bilimi* Microsoft Azure Machine Learning Studio'dan.

## <a name="next-steps"></a>Sonraki adımlar
* [Bir Machine Learning Studio'da ilk veri bilimi deneyinde deneyin](create-experiment.md)
* [Microsoft Azure'da Machine learning'e giriş yapın](/azure/machine-learning/preview/overview-what-is-azure-ml)
