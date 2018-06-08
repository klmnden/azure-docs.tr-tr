---
title: 5 veri bilimi - yeni başlayanlar için veri bilimi - Azure Machine Learning sorular | Microsoft Docs
description: Yeni başlayanlar için veri bilimi 5 sorular veri bilimi yanıtlar ile başlayarak 5 kısa videoları, temel kavramlar öğretir. Azure Machine Learning gelen.
keywords: Veri bilimi, veri bilimi yeni başlayanlar, veri bilimi yapılması yeni başlayanlar, veri bilimi temelleri, veri bilimi soruları, veri bilimi video, veri bilimi giriş
services: machine-learning
documentationcenter: na
author: heatherbshapiro
ms.author: hshapiro
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
ms.openlocfilehash: 1d004eb16fcac13d6ba7592cbe432cbeac0401e8
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34834160"
---
# <a name="data-science-for-beginners-video-1-the-5-questions-data-science-answers"></a>Yeni Başlayanlar için Veri Bilimi video 1: Veri bilimiyle ilgili 5 sorunun yanıtı
Bir giriş veri bilimi alma *yeni başlayanlar için veri bilimi* üst veri Bilimcisi beş kısa videoların içinde. Bu videolar temel ancak yararlı yaparak veri bilimi ilgilenen ya da veri bilimcilerine ile çalışır.

Bu ilk video tür veri bilimi yanıtlayabilir soru hakkında ' dir. Serinin en dışında almak için tümünü izleyin. [Videolar listesine Git](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Shows/SupervisionNotRequired/8/player]
>
>

## <a name="other-videos-in-this-series"></a>Bu serideki diğer videolar
*Yeni başlayanlar için veri bilimi* veri bilimi yaklaşık 25 dakika toplam alan için bir giriş değil. Tüm beş videosuna göz atın:

* Video 1: 5 veri bilimi yanıtlar sorular
* Video 2: [verileriniz için veri bilimi hazır?](data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 min 56 sn)*
* Video 3: [verilerle yanıt soru](data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 sn)*
* Video 4: [basit bir modelle bir yanıt tahmin](data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min 42 sn)*
* Video 5: [veri bilimi yapmak için diğer kişilerin çalışma kopyalama](data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 dakika 18 saniye)*

## <a name="transcript-the-5-questions-data-science-answers"></a>Dökümü: 5 veri bilimi yanıtlar sorular
Merhaba! Hoş Geldiniz video serisi *yeni başlayanlar için veri bilimi*.

Herhangi bir denklemini veya bilgisayar terimler programlama olmadan temelleri tanıtmak böylece veri bilimi göz korkutucu, olabilir.

İlk Bu videoda, "5 veri bilimi yanıtlar sorular hakkında." biz konuşun

Veri bilimi soruların yanıtlarını tahmin etmek için numaraları ve adları (kategorileri veya etiketleri olarak da bilinir) kullanır.

Bu, beklenmedik ancak *o veri bilimi yanıtlar yalnızca beş sorular verilmiştir*:

* Bu A veya B mi?
* Bu tuhaf mi?
* Ne kadar – veya – kaç tane?
* Bu nasıl düzenlenir?
* Ne yapmam?

Bu soruları her biri ayrı bir algoritmaları adlı machine learning yöntemler ailesi tarafından yanıtlanan.

Bir tarifi olarak bir algoritma ve verilerinizi malzemeleri gerektiği hakkında düşünmek faydalıdır. Algoritma, birleştirme ve verileri bir yanıt alabilmek için karışık anlatır. Gibi bir blender bilgisayarlardır. Algoritmanın sabit işlerin çoğunu sizin yerinize yapmaları ve bunlar oldukça hızlı yapın.

## <a name="question-1-is-this-a-or-b-uses-classification-algorithms"></a>Soru 1: Bu A veya B mi? Sınıflandırma algoritmalarını kullanır
Soruyla başlayalım: Bu A veya B?

![Sınıflandırma algoritmaları: Bu A veya B?](./media/data-science-for-beginners-the-5-questions-data-science-answers/classification-algorithms.png)

Bu algoritma ailesi, iki sınıflı sınıflandırma adı verilir.

Yalnızca iki olası yanıt olan herhangi bir soru için yararlı olacaktır.

Örneğin:

* Bu lastiği sonraki 1.000 mili başarısız olur: Evet veya Hayır?
* Daha fazla müşteriler getirir: $5 kupon veya % 25 indirim?

Bu soruya da ikiden fazla seçenekler içerecek şekilde rephrased: Bu A veya B veya C veya D, vb..?  Birkaç varsa, bu çok sınıflı sınıflandırma ve onun yararlı adlandırılır — ya da birkaç bin — olası yanıtlar. Çok sınıflı sınıflandırma büyük olasılıkla bir seçer.

## <a name="question-2-is-this-weird-uses-anomaly-detection-algorithms"></a>Soru 2: Bu tuhaf mi? anomali algılama algoritmalarını kullanır
Veri bilimi yanıt Sonraki Soru: Bu tuhaf olduğu? Bu soru bir algoritma ailesi, anomali algılama adlı tarafından yanıtlanan.

![Anomali algılama algoritmalarını: Bu tuhaf olduğu?](./media/data-science-for-beginners-the-5-questions-data-science-answers/anomaly-detection-algorithms.png)

Bir kredi kartı varsa, zaten anomali algılamanın dışında benefited. Böylece bunlar için olası sahtekarlık uyarabilir kredi kartı şirketiniz satın alma alışkanlıklarınıza analiz eder. "Tuhaf" ücretleri satın alma bir depolama Burada, normalde alışveriş yok veya olağandışı pricey öğeyi satın olabilir.

Bu soru içinde yollarından biri çok yararlı olabilir. Örneğin:

* Bir araba baskının ölçerler ile varsa, bilmek isteyebilirsiniz: olan normal okuma bu baskısı ölçer?
* İnternet izliyorsanız bilmek isteyeceğiniz: Bu ileti internet'ten normaldir?

Anomali algılama beklenmeyen veya olağandışı olayları veya davranışları işaretler. Bu ipuçları nerede sorun olup olmadığına bakın sağlar.

## <a name="question-3-how-much-or-how-many-uses-regression-algorithms"></a>Soru 3: Ne kadar? veya nasıl birçok? regresyon algoritması kullanır
Machine learning de nasıl yanıt çok tahmin edebilirsiniz? veya nasıl birçok? Bu soruya yanıt algoritma ailesi regresyon adı verilir.

![Regresyon algoritmaları: ne kadar? veya nasıl birçok?](./media/data-science-for-beginners-the-5-questions-data-science-answers/regression-algorithms.png)

Regresyon algoritmalar gibi sayısal tahminleri olun:

* Sonraki Salı ne sıcaklık olacak?  
* My dördüncü çeyreği satış ne olacak?

Bunlar için bir sayı ister soruyu yanıtlayın yardımcı olur.

## <a name="question-4-how-is-this-organized-uses-clustering-algorithms"></a>Soru 4: Bu nasıl düzenlendiğini? algoritmalar kümeleme kullanır
Şimdi son iki sorular, daha gelişmiş bir bittir.

Bazen, bir veri kümesi - yapısını anlamak istediğiniz nasıl bu düzenlenmiş? Bu soru için anlamanız için sonuçlarını zaten bildiğiniz örnekler yok.

Çok miktarda veri yapısı tease yolları vardır. Bir yaklaşım kümeleme. Bu veri doğal "kümeler," için daha kolay yorumlama ayırır. Kümeleme ile bir sağ yanıt yoktur.

![Algoritmalar kümeleme: nasıl bu düzenlenmiş?](./media/data-science-for-beginners-the-5-questions-data-science-answers/clustering-algorithms.png)

Kümeleme soruları ortak örnekleri şunlardır:

* Hangi görüntüleyiciler filmler aynı türde ister?
* Hangi yazıcı modellerini aynı şekilde başarısız?

Verileri nasıl düzenlendiğini anlayarak daha iyi anlamak - ve tahmin - davranışları ve olaylar.  

## <a name="question-5-what-should-i-do-now-uses-reinforcement-learning-algorithms"></a>Soru 5: Ne yapmalıyım? learning algoritmaları öğrenmeyi kullanır
Son soru – ne yapmalıyım? – bir algoritma ailesi, öğrenmeyi öğrenme adlı kullanır.

Öğrenmeyi öğrenme rats ve insanlar brains punishment ve kazanımları vereceği tarafından neden olacak. Bu algoritmalar sonuçları öğrenin ve bir sonraki eylem karar verin.

Genellikle, çok sayıda küçük kararları İnsan Kılavuzu olmadan yapmak zorunda otomatik sistemler için iyi bir tercihtir öğrenmeyi öğrenme olur.

![Learning algoritmaları öğrenmeyi: ne yapmalıyım?](./media/data-science-for-beginners-the-5-questions-data-science-answers/reinforcement-learning-algorithms.png)

Bu yanıtlar sorular, hangi hakkında - genellikle bir makine veya bir robot tarafından yapılması gerekir her zaman vardır. Örnekler şunlardır:

* Bir ev için bir sıcaklık denetim sistemi 'M: sıcaklık ayarlamak veya olduğu bırakın?  
* Kendi kendine yönlendirmeli araba ben varsa: sarı bir ışık Fren veya hızlandırmak?  
* Bir robot vakum için: vacuuming tutun veya doluyor istasyona geri dönün?

Deneme yanılma öğrenme Git gibi öğrenmeyi learning algoritmaları verileri toplayın.

Bunu - olacak şekilde 5 soruları veri bilimi yanıt verebilir.

## <a name="next-steps"></a>Sonraki adımlar
* [İlk veri bilimi denemeyi Machine Learning Studio'da deneyin](create-experiment.md)
* [Microsoft Azure Machine Learning giriş Al](what-is-machine-learning.md)
