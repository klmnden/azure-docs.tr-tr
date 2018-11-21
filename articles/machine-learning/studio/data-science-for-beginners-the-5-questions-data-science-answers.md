---
title: 5 veri bilimi - yeni başlayanlar için veri bilimi - Azure Machine Learning soruları | Microsoft Docs
description: Yeni başlayanlar için veri bilimi 5 sorular veri Biliminin yanıtladığı ile başlayarak 5 kısa video, temel kavramları öğretir. Azure Machine Learning hizmetinden.
keywords: Veri bilimi, veri bilimi yeni başlayanlar, veri bilimi yapmak, yeni başlayanlar, veri bilimi temelleri, veri bilimi soruları, veri bilimi video, veri bilimi giriş
services: machine-learning
documentationcenter: na
author: ericlicoding
ms.custom: (previous ms.author=hshapiro, author=heatherbshapiro)
ms.author: amlstudiodocs
manager: hjerez
editor: cjgronlund
ms.assetid: a01f93ee-01eb-4afe-abbd-cfa035c119b0
ms.service: machine-learning
ms.component: studio
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2018
ms.openlocfilehash: 2e3d8805bfbe5b55aed111090cf1f7c02acb6a76
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52262049"
---
# <a name="data-science-for-beginners-video-1-the-5-questions-data-science-answers"></a>Yeni Başlayanlar için Veri Bilimi video 1: Veri bilimiyle ilgili 5 sorunun yanıtı
Veri bilimi için hızlı bir giriş yapın *yeni başlayanlar için veri bilimi* beş kısa videolardan bir veri bilimi insanı olarak. Bu videolarda temel ancak yararlı veri bilimi ilginizi çeken veya veri uzmanları ile çalışır.

Bu ilk video, veri bilimi neredeyse tümünü cevaplayabilir soru türleri hakkında olur. En yetersiz serisi almak için tüm bunları izleyin. [Videoları listesine Git](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Shows/SupervisionNotRequired/8/player]
>
>

## <a name="other-videos-in-this-series"></a>Bu serideki diğer videolar
*Yeni başlayanlar için veri bilimi* veri bilimi yaklaşık 25 dakika toplam alma için hızlı bir giriş niteliğindedir. Tüm beş videosuna göz atın:

* Video 1: 5 veri biliminin yanıtladığı sorular
* Video 2: [verileriniz veri bilimi için hazır mı?](data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 dk 56 sn)*
* Video 3: [verilerle yanıt soru](data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 dk 17 sn)*
* Video 4: [basit model ile yanıtı tahmin etme](data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 dk 42 sn)*
* Video 5: [veri bilimi için başkalarının çalışmalarını kopyalama](data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 dk 18 sn)*

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

## <a name="question-1-is-this-a-or-b-uses-classification-algorithms"></a>1. Soru: Bu A veya B mi? Sınıflandırma algoritmaları kullanır.
Sorunun ile başlayalım: Bu A veya B?

![Sınıflandırma algoritmaları: Bu A veya B?](./media/data-science-for-beginners-the-5-questions-data-science-answers/classification-algorithms.png)

Bu algoritma ailesi, iki sınıflı sınıflandırma adı verilir.

Yalnızca iki olası yanıtların olan herhangi bir soru için kullanışlıdır.

Örneğin:

* Bu Lastik sonraki 1.000 mili başarısız olur: Evet veya Hayır?
* Daha fazla müşteriye getirir: $5 kupon veya % 25 indirim?

Bu soru da ikiden fazla seçenekleri içerecek şekilde rephrased: Bu bir A veya B veya C veya D, vb..?  Birkaç olduğunda bu çok sınıflı sınıflandırma ya da onun yararlı adlandırılır — ya da birkaç bin — olası yanıtlar. Sınıflı sınıflandırma büyük olasılıkla seçer.

## <a name="question-2-is-this-weird-uses-anomaly-detection-algorithms"></a>2. Soru: Bu tuhaf mi? anomali algılama algoritmalarını kullanır.
Veri bilimi yanıtlarını alabileceğiniz Sonraki Soru: Bu tuhaf olan? Bu soruyu adlı anomali algılama algoritmalarını ailesi tarafından yanıtlanır.

![Anomali algılama algoritmalarını: Bu tuhaf olan?](./media/data-science-for-beginners-the-5-questions-data-science-answers/anomaly-detection-algorithms.png)

Bir kredi kartınız varsa, anomali algılama zaten benefited. Böylece bunlar, olası bir sahtekarlık Uyarısı, kredi kartı şirketiniz, satın alma düzenleri analiz eder. Burada, normalde alışveriş olmayan bir mağaza ya da çok pahalı bir öğe satın satın "tuhaf" ücretleri olabilir.

Bu soruyu yolu çok sayıda yararlı olabilir. Örneğin:

* Basınç Ölçerleri içeren bir araba varsa bilmek isteyebilirsiniz: olduğundan bu baskısı normal okuma ölçer?
* İnternet izliyorsanız bilmek istersiniz: internet'ten bu iletiyi normaldir?

Anomali algılama, olağan dışı ya da beklenmeyen olaylar veya davranışları işaretler. Bu sorunları aranacağı nedene verir.

## <a name="question-3-how-much-or-how-many-uses-regression-algorithms"></a>Soru 3: Ne kadar? veya nasıl birçok? regresyon algoritmaları kullanır.
Machine learning ayrıca nasıl yanıt çok tahmin edebilir? veya nasıl birçok? Bu soruyu yanıtlar algoritması ailesini regresyon çağrılır.

![Regresyon algoritmaları: ne kadar? veya nasıl birçok?](./media/data-science-for-beginners-the-5-questions-data-science-answers/regression-algorithms.png)

Regresyon algoritmaları gibi sayısal Öngörüler oluşturun:

* Sonraki Salı ne sıcaklık olacaktır?  
* Ne my dördüncü quarter sales olacaktır?

Bunlar, bir numarasını sorar herhangi bir sorunun yanıtlanmasına yardımcı olmak.

## <a name="question-4-how-is-this-organized-uses-clustering-algorithms"></a>4. Soru: Bu nasıl düzenlenir? algoritmalar kümeleme kullanır.
Son iki soruları daha gelişmiş bir bit sunulmuştur.

Bazen bir veri kümesi - yapısını anlamak istediğiniz nasıl bu düzenlenir? Bu soru için sonuçlarını zaten bildiğiniz örnek yok.

Çok sayıda veri yapısını çıkarabilmek için yol vardır. Bir yaklaşım kümeleme. Bu veri doğal "kümeler," daha kolay yorumlanması için ayırır. Kümeleme ile bir doğru yanıt yoktur.

![Algoritmalar kümeleme: nasıl bu düzenlenir?](./media/data-science-for-beginners-the-5-questions-data-science-answers/clustering-algorithms.png)

Genel kümeleme sorular örnekleri şunlardır:

* Hangi izleyicilerin filmler aynı türde beğendiniz?
* Hangi yazıcı modelleri, aynı şekilde başarısız?

Veriler nasıl düzenlenir anlayarak daha iyi anlayın - ve tahmin - davranışları ve olaylar.  

## <a name="question-5-what-should-i-do-now-uses-reinforcement-learning-algorithms"></a>Soru 5: Ne şimdi yapmam? pekiştirmeye öğrenimi algoritmaları kullanır.
Son soru – ne yapmalıyım? – adlı pekiştirmeye dayalı öğrenme algoritmaları ailesini kullanır.

RAT ve insanlar brains punishment ve ödüller için vereceği tarafından pekiştirmeye dayalı öğrenme ilham. Bu algoritmalar sonuçlarını öğrenin ve sonraki eylemi karar verin.

Genellikle, pekiştirmeye dayalı öğrenme İnsan yönergeler olmayan küçük kararları çok sayıda yapmak zorunda otomatik sistemler için uygun olur.

![Öğrenimi algoritmaları pekiştirmeye: ne yapmalıyım?](./media/data-science-for-beginners-the-5-questions-data-science-answers/reinforcement-learning-algorithms.png)

Bu yanıtlar sorular, hangi eylemi hakkında - genellikle bir makine ya da bir robot tarafından gerçekleştirilmelidir her zaman vardır. Örnekler şunlardır:

* Sıcaklık denetimi Sistemi'nde bir ev 'M: sıcaklık ayarlamak veya olduğu bırakın?  
* Sürücüsüz araba müşterisiysem,: sarı bir ışık hızınızı veya hızlandırın?  
* İçin robot elektrikli: vacuuming koruyabilir veya şarj istasyona geri dönün?

Deneme yanılma öğrenme Git gibi pekiştirmeye öğrenme algoritmalarını verileri toplayın.

-Bu, bu nedenle, 5 sorular veri bilimi yanıt verebilir.

## <a name="next-steps"></a>Sonraki adımlar
* [Bir Machine Learning Studio'da ilk veri bilimi deneyinde deneyin](create-experiment.md)
* [Microsoft Azure'da Machine learning'e giriş yapın](what-is-machine-learning.md)
