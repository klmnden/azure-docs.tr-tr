---
title: "Verileriniz veri bilimi için hazır mı? Veri değerlendirmesi - Azure Machine Learning | Microsoft Docs"
description: "Veri bilimi için hazır olmasını veriler için 4 ölçüt öğrenin. Veri bilimi video 2 yeni başlayanlar için temel veri değerlendirme ile yardımcı olmak için somut örnekler vardır."
keywords: "ilgili verileri verileri değerlendirmek, verileri, veri ölçütlerini, veri hazır hazırlayın"
services: machine-learning
documentationcenter: na
author: cjgronlund
manager: jhubbard
editor: cjgronlund
ms.assetid: d502062c-da70-4b21-9054-0bfd9902612e
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: cgronlun
ms.openlocfilehash: 9b5cf776981af0dff57195d5c7f1923b8d9a3862
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
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
Bu nedenle, veri bilimi durumunda birlikte çıkarmak için ihtiyacımız bazı malzemeleri vardır.

Verileri ihtiyacımız var:

* İlgili
* bağlı
* Doğru
* Yeterli çalışmak için

## <a name="is-your-data-relevant"></a>Verilerinizi ilgili mi?
Bu nedenle ilk tarifi - ilgili olan verileri ihtiyacımız var.

![İlgili verileri ilgisiz verilerin - karşılaştırması veri değerlendir](./media/data-science-for-beginners-is-your-data-ready-for-data-science/relevant-and-irrelevant-data.png)

Sol tarafta tabloya bakın. Biz Boston çubukları dışında yedi kişi yerine, kan Alkol düzeylerini, son kullanıcıların oyun kırmızı Sox batting ortalama ve en yakın Market sütlü fiyat ölçülür.

Bu tüm mükemmel yasal verilerdir. İlgili değil, yalnızca hataya değildir. Bu sayılar arasında belirgin ilişkisi yoktur. I sütlü ve kırmızı Sox batting ortalama geçerli fiyat vermiş, kan Alkol İçeriğim tahmin edebilir yolu yoktur.

Şimdi sağ tarafta tabloya bakın. Bu süre, biz ölçülen her birinin gövde yığın ve sayılan sahip oldukları İçecekler sayısı.  Her satır numaraları şimdi birbiriyle ilgili olur. I size my gövde verdiyse yığın ve ı vardı Margaritas sayısı, yaptığınız my kan konumundaki tahmin Alkol içerik.

## <a name="do-you-have-connected-data"></a>Sahip bağlı veri?
Sonraki tarifi bağlı verilerdir.

![Bağlı veri bağlantısı kesilmiş verileri - veri ölçütlerini hazır karşılaştırması](./media/data-science-for-beginners-is-your-data-ready-for-data-science/connected-vs-disconnected-data.png)

Hamburgers kalitesini bazı ilgili bilgiler aşağıdadır: sıcaklık, patty ağırlık ve dergi yerel yemek derecesi maske. Ancak sol tablodaki boşlukları dikkat edin.

Çoğu veri kümelerini bazı değerler eksik. Böyle delik sağlamak için yaygın bir durumdur ve etrafında çalışmaya yolu vardır. Ancak, verilerinizi varsa çok fazla eksik, İsviçre Peynir gibi ara başlar.

Sol tarafta tabloya bakarsanız, olmadığından eksik kadar veri grill sıcaklık ve patty ağırlık ilişkiyi her türlü gündeme sabit. Bu, bağlantısı kesilmiş veri örneğidir.

Ancak sağ taraftaki tablo dolu olduğunda ve tamamlandı - bağlı veri örneği.

## <a name="is-your-data-accurate"></a>Verilerinizi geçerli mi?
İhtiyacımız sonraki tarifi doğruluğu ' dir. Oklarla isabet isteriz dört hedefleri şunlardır.

![Doğru verileri yanlış data - veri ölçütlerini karşılaştırması](./media/data-science-for-beginners-is-your-data-ready-for-data-science/inaccurate-vs-accurate-data.png)

Sağ üst hedef bakın. Biz sağ hedef merkezi etrafında sıkı bir gruplandırma açıyor. Doğal olarak, doğru olur. Arasında veri bilimi dilde altındaki hedef sağdaki bizim performans da doğru olarak değerlendirilir.

Bu okları merkezi eşlemek için olsaydı, bu çok hedef merkezi yakın olduğunu görürsünüz. Oklar, tüm hedef geçici kesin olmayan kabul ancak doğru kabul şekilde hedef merkezi ortalanmış yayılır.

Artık sol üst hedefe arayın. Burada bizim okları çok birbirine sıkı bir gruplandırma ulaştı. Kesin, ancak merkezi şekilde hedef merkezi devre dışı olduğundan, bunlar yanlış. Ve Elbette, sol alt hedef okları yanlış ve kesin. Bu archer daha fazla uygulama gerekir.

## <a name="do-you-have-enough-data-to-work-with"></a>Çalışmak için yeterli veri var mı?
Son olarak, malzeme #4 - biz yeterli veri olması gerekir.

![Analiz için yeterli veri var mı? Veri değerlendirme](./media/data-science-for-beginners-is-your-data-ready-for-data-science/barely-enough-data.png)

Her veri noktası tablosundaki boyama içinde fırça vuruş olarak düşünün. Yalnızca birkaç tanesi varsa, boyama oldukça benzer - ne olduğunu ayırt etmek zordur.

Daha fazla bazı fırça vuruşları eklerseniz, boyama biraz daha net alma başlar.

Neredeyse hiç yeterli vuruşları olduğunda yetecek kadar geniş bazı kararlar almak için görebilirsiniz. Bu bir yere ziyaret etmek istediğiniz mi? Açık görünüyor, temiz su gibi – Evet, burada tatile kullanacağım olan arar.

Daha fazla veri ekleme gibi resmi daha anlaşılır olur ve daha ayrıntılı kararlarını verebilir. Artık sol bank üzerinde üç Oteller adresindeki gözden geçirebilirsiniz. Bildiğiniz ı gerçekten mimari ön planda biri gibi özellikler. Üçüncü kat üzerinde kalır,.

İlgili, bağlı, doğru veri ve yeterli, biz ile sahip ihtiyacımız için tüm malzemeleri yapmak bazı yüksek kaliteli veri bilimi.

Diğer dört videoları kullanıma özen *yeni başlayanlar için veri bilimi* Microsoft Azure Machine learning'in.

## <a name="next-steps"></a>Sonraki adımlar
* [İlk veri bilimi denemeyi Machine Learning Studio'da deneyin](create-experiment.md)
* [Microsoft Azure Machine Learning giriş Al](what-is-machine-learning.md)
