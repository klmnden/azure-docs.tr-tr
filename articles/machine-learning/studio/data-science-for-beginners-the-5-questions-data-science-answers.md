---
title: Yeni Başlayanlar için Veri Bilimleri
titleSuffix: Azure Machine Learning Studio
description: Yeni başlayanlar için veri bilimi 5 sorular veri Biliminin yanıtladığı ile başlayarak 5 kısa video, temel kavramları öğretir. Azure Machine Learning hizmetinden.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: sdgilley
ms.author: sgilley
ms.custom: seodec18
ms.date: 03/22/2019
ms.openlocfilehash: d89a701f1d4528e1f3dff08daf31873891778f07
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58369199"
---
# <a name="data-science-for-beginners-video-1-the-5-questions-data-science-answers"></a>Veri bilimi video 1 yeni başlayanlar için: 5 veri biliminin yanıtladığı sorular
Veri bilimi için hızlı bir giriş yapın *yeni başlayanlar için veri bilimi* beş kısa videolardan bir veri bilimi insanı olarak. Bu videolarda temel ancak yararlı veri bilimi ilginizi çeken veya veri uzmanları ile çalışır.

Bu ilk video, veri bilimi neredeyse tümünü cevaplayabilir soru türleri hakkında olur. En yetersiz serisi almak için tüm bunları izleyin. [Videoları listesine Git](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Shows/SupervisionNotRequired/8/player]
>
>

## <a name="other-videos-in-this-series"></a>Bu serideki diğer videolar
*Yeni başlayanlar için veri bilimi* veri bilimi yaklaşık 25 dakika toplam alma için hızlı bir giriş niteliğindedir. Tüm beş videosuna göz atın:

* Video 1: 5 veri biliminin yanıtladığı sorular
* Video 2: [Verileriniz veri bilimi için hazır mı?](data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 dk 56 sn)*
* Video 3: [Yanıt verileri ile soru](data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 dk 17 sn)*
* Video 4: [Basit bir model ile yanıtı tahmin etme](data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 dk 42 sn)*
* Video 5: [Veri bilimi için başkalarının çalışmalarını kopyalama](data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 dk 18 sn)*

## <a name="transcript-the-5-questions-data-science-answers"></a>Transkript: 5 veri biliminin yanıtladığı sorular
Merhaba! Video serisine Hoş Geldiniz *yeni başlayanlar için veri bilimi*.

Veri bilimi, denklemler veya bilgisayar terminolojisinin programlama olmadan temel tanıtmak böylece göz korkutucu, olabilir.

İlk Bu videoda, "5 veri biliminin yanıtladığı sorular hakkında." konuşacağız

Veri bilimi numaraları ve adları (kategorileri veya etiketleri olarak da bilinir) soruların yanıtlarını tahmin eder.

Sizi şaşırtabilir ancak *söz konusu veri biliminin yanıtladığı yalnızca beş soru vardır*:

* Bu A veya B mi?
* Bu tuhaf mi?
* Ne kadar – veya – ne kadar?
* Bu nasıl düzenlendiği?
* Ne sonraki yapmalıyım?

Bu soruların her biri ayrı bir makine öğrenme yöntemleri ve algoritma adı verilen, ailesi tarafından yanıtlanır.

Bir tarifi olarak bir algoritma ve verilerinizi malzemeleri gerektiği hakkında düşünmek faydalıdır. Algoritma, birleştirmek ve bir yanıt almak için verileri karıştırın anlatır. Blender'ı gibi bilgisayarlardır. Bunlar algoritmasının sabit işin çoğunu sizin için gerçekleştirmesini ve bunlar oldukça çabuk yapın.

## <a name="question-1-is-this-a-or-b-uses-classification-algorithms"></a>Soru 1: Bu A veya B mi? Sınıflandırma algoritmaları kullanır.
Sorunun ile başlayalım: Bu A veya B mi?

![Sınıflandırma algoritmaları: Bu A veya B mi?](./media/data-science-for-beginners-the-5-questions-data-science-answers/classification-algorithms.png)

Bu algoritma ailesi, iki sınıflı sınıflandırma adı verilir.

Yalnızca iki olası yanıtların olan herhangi bir soru için kullanışlıdır.

Örneğin:

* Bu Lastik sonraki 1.000 mili başarısız olur: Evet veya Hayır?
* Daha fazla müşteriye getirir: $5 kupon veya % 25 indirim?

Bu soruyu ikiden fazla seçenekleri içerecek şekilde de rephrased: Bu bir A veya B veya C veya D, vb..?  Birkaç olduğunda bu çok sınıflı sınıflandırma ya da onun yararlı adlandırılır — ya da birkaç bin — olası yanıtlar. Sınıflı sınıflandırma büyük olasılıkla seçer.

## <a name="question-2-is-this-weird-uses-anomaly-detection-algorithms"></a>Soru 2: Bu tuhaf mi? anomali algılama algoritmalarını kullanır.
Veri bilimi neredeyse tümünü cevaplayabilir sonraki soru da şudur: Bu tuhaf mi? Bu soruyu adlı anomali algılama algoritmalarını ailesi tarafından yanıtlanır.

![Anomali algılama algoritmalarını: Bu tuhaf mi?](./media/data-science-for-beginners-the-5-questions-data-science-answers/anomaly-detection-algorithms.png)

Bir kredi kartınız varsa, anomali algılama zaten benefited. Böylece bunlar, olası bir sahtekarlık Uyarısı, kredi kartı şirketiniz, satın alma düzenleri analiz eder. Burada, normalde alışveriş olmayan bir mağaza ya da çok pahalı bir öğe satın satın "tuhaf" ücretleri olabilir.

Bu soruyu yolu çok sayıda yararlı olabilir. Örneğin:

* Basınç Ölçerleri içeren bir araba varsa bilmek isteyebilirsiniz: Bu baskısı ölçer normal okunuyor?
* İnternet izliyorsanız bilmek istersiniz: Bu ileti, tipik internet'ten mi?

Anomali algılama, olağan dışı ya da beklenmeyen olaylar veya davranışları işaretler. Bu sorunları aranacağı nedene verir.

## <a name="question-3-how-much-or-how-many-uses-regression-algorithms"></a>Soru 3: Ne kadar? veya nasıl birçok? regresyon algoritmaları kullanır.
Machine learning ayrıca nasıl yanıt çok tahmin edebilir? veya nasıl birçok? Bu soruyu yanıtlar algoritması ailesini regresyon çağrılır.

![Regresyon algoritmaları: Ne kadar? veya nasıl birçok?](./media/data-science-for-beginners-the-5-questions-data-science-answers/regression-algorithms.png)

Regresyon algoritmaları gibi sayısal Öngörüler oluşturun:

* Sonraki Salı ne sıcaklık olacaktır?  
* Ne my dördüncü quarter sales olacaktır?

Bunlar, bir numarasını sorar herhangi bir sorunun yanıtlanmasına yardımcı olmak.

## <a name="question-4-how-is-this-organized-uses-clustering-algorithms"></a>Soru 4: Bu nasıl düzenlendiği? algoritmalar kümeleme kullanır.
Son iki soruları daha gelişmiş bir bit sunulmuştur.

Bazen bir veri kümesi - yapısını anlamak istediğiniz nasıl bu düzenlenir? Bu soru için sonuçlarını zaten bildiğiniz örnek yok.

Çok sayıda veri yapısını çıkarabilmek için yol vardır. Bir yaklaşım kümeleme. Bu veri doğal "kümeler," daha kolay yorumlanması için ayırır. Kümeleme ile bir doğru yanıt yoktur.

![Kümeleme algoritmaları: Bu nasıl düzenlendiği?](./media/data-science-for-beginners-the-5-questions-data-science-answers/clustering-algorithms.png)

Genel kümeleme sorular örnekleri şunlardır:

* Hangi izleyicilerin filmler aynı türde beğendiniz?
* Hangi yazıcı modelleri, aynı şekilde başarısız?

Veriler nasıl düzenlenir anlayarak daha iyi anlayın - ve tahmin - davranışları ve olaylar.  

## <a name="question-5-what-should-i-do-now-uses-reinforcement-learning-algorithms"></a>Soru 5: Ne şimdi yapmam? pekiştirmeye öğrenimi algoritmaları kullanır.
Son soru – ne yapmalıyım? – adlı pekiştirmeye dayalı öğrenme algoritmaları ailesini kullanır.

RAT ve insanlar brains punishment ve ödüller için vereceği tarafından pekiştirmeye dayalı öğrenme ilham. Bu algoritmalar sonuçlarını öğrenin ve sonraki eylemi karar verin.

Genellikle, pekiştirmeye dayalı öğrenme İnsan yönergeler olmayan küçük kararları çok sayıda yapmak zorunda otomatik sistemler için uygun olur.

![Pekiştirmeye öğrenme algoritmalarını: Ne sonraki yapmalıyım?](./media/data-science-for-beginners-the-5-questions-data-science-answers/reinforcement-learning-algorithms.png)

Bu yanıtlar sorular, hangi eylemi hakkında - genellikle bir makine ya da bir robot tarafından gerçekleştirilmelidir her zaman vardır. Örnekler şunlardır:

* Sıcaklık denetimi Sistemi'nde bir ev müşterisiysem: Sıcaklık ayarlamak veya olduğu bırakın?  
* Sürücüsüz araba müşterisiysem: Sarı bir ışık hızınızı veya hızlandırın?  
* İçin robot elektrikli: Vacuuming koruyabilir veya şarj istasyona geri dönün?

Deneme yanılma öğrenme Git gibi pekiştirmeye öğrenme algoritmalarını verileri toplayın.

-Bu, bu nedenle, 5 sorular veri bilimi yanıt verebilir.

## <a name="next-steps"></a>Sonraki adımlar
* [Bir Machine Learning Studio'da ilk veri bilimi deneyinde deneyin](create-experiment.md)
* [Microsoft Azure'da Machine learning'e giriş yapın](/azure/machine-learning/preview/overview-what-is-azure-ml)
