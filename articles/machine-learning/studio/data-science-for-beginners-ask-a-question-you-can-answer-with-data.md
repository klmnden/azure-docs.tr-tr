---
title: "Bir soru verileri yanıt - veri bilimi sorunlarını - Azure Machine Learning isteyin | Microsoft Docs"
description: "Video 3 yeni başlayanlar için veri bilimi sharp veri bilimi sorusuna formüle öğrenin. Sınıflandırma ve regresyon soruları karşılaştırması içerir."
keywords: "Veri bilimi sorunları, veri bilimi soruları formüle sorular, regresyon sorular, sınıflandırma sorular, sharp soru"
services: machine-learning
documentationcenter: na
author: cjgronlund
manager: jhubbard
editor: cjgronlund
ms.assetid: 5b9501e3-9964-417a-8ffc-8913103da77b
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2018
ms.author: cgronlun
ms.openlocfilehash: 2f3d8d5c2e7cf1ebc88dc1ff1d7d03bf85383884
ms.sourcegitcommit: 4bd369fc472dced985239aef736fece42fecfb3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2018
---
# <a name="ask-a-question-you-can-answer-with-data"></a>Verilerle yanıtlayabileceğiniz bir soru sorun
## <a name="video-3-data-science-for-beginners-series"></a>Video 3: Yeni başlayanlar seri için veri bilimi
Video 3 yeni başlayanlar için veri bilimi sorusuna içine veri bilimi sorunu formüle öğrenin. Bu videoda, Sınıflandırma ve regresyon algoritmalar için sorular karşılaştırması içerir.

Serinin en dışında almak için tümünü izleyin. [Videolar listesine Git](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Data-science-for-beginners-Ask-a-question-you-can-answer-with-data/player]
>
>

## <a name="other-videos-in-this-series"></a>Bu serideki diğer videolar
*Yeni başlayanlar için veri bilimi* veri bilimi beş kısa videolar içindeki bir giriş değil.

* Video 1: [5 veri bilimi yanıtlar sorular](data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 dakika 14 saniye)*
* Video 2: [verileriniz için veri bilimi hazır?](data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 min 56 sn)*
* Video 3: verilerle yanıtlayabilir bir soru sorun
* Video 4: [basit bir modelle bir yanıt tahmin](data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min 42 sn)*
* Video 5: [veri bilimi yapmak için diğer kişilerin çalışma kopyalama](data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 dakika 18 saniye)*

## <a name="transcript-ask-a-question-you-can-answer-with-data"></a>Dökümü: verilerle yanıtlayabilir bir soru sorun
Serideki üçüncü video "Yeni başlayanlar için veri bilimi." Hoş Geldiniz  

Bu birinde verilerle yanıtlayabilir soru formulating için bazı ipuçları elde edersiniz.

İki önceki videolar bu serideki ilk izleme, bu videoda dışında daha fazla bilgi alabilirsiniz: "5 soruları veri bilimi yanıtlayabilir" ve "verileriniz için veri bilimi hazır mı?"

## <a name="ask-a-sharp-question"></a>NET bir soru sorun
Veri bilimi bir yanıtını tahmin etmek için (kategorileri veya etiketleri olarak da adlandırılır) adlarını ve numaralarını kullanma işlemi nasıl olduğu hakkında açıklandı. Ancak herhangi bir soru olamaz; olmak zorundadır bir *sharp soru.*

Belirsiz bir soru bir ad veya sayı ile yanıtlanması gereken sahip değil. Sharp soru gerekir.

Sihirli ampul istenir soru truthfully yanıtlayacak bir genie ile bulunan düşünün. Ancak yaramaz genie olan ve kendisine kendisi yerine ile alabilirsiniz gibi kendi yanıt belirsiz ve karmaşık hale getirmek deneyeceksiniz. Bir soruyla kendisi olamaz yardımcı olur ancak bilmek istediğiniz söyleyin, bu nedenle airtight sabitlemek ona istediğiniz.

Genie yanıt "Benim hisse olmasını neler olduğunu?", "fiyat değişir" gibi belirsiz bir soru sormak için olsaydı. Gerçek bir yanıt olan, ancak çok yararlı değildir.

Ancak "ne my hisse senedi 's satış fiyatı sonraki hafta?", genie olamaz Yardım ancak belirli bir size gibi keskin bir soru sorun olsaydı yanıtlayın ve satış fiyatını tahmin etmek.

## <a name="examples-of-your-answer-target-data"></a>Yanıtınız örnekleri: hedef veri
Sorunuzun yanıtını formüle sonra verilerinizi yanıt örnekleri yüklü olup olmadığını denetleyin.

Bizim Soru "Ne my hisse senedi 's satış fiyatı sonraki hafta olacak?" ise ardından verilerimizi stok fiyat geçmişi içerdiğinden emin olmak sahibiz.

Bizim Soru "hangi otomobil my Donanma içinde ilk başarısız olacağını?" ise ardından verilerimizi önceki hataları hakkında bilgi içeren emin olmak sahibiz.

![Hedef verileri - yanıtınız örnekleri. Bir veri bilimi soru formüle.](./media/data-science-for-beginners-ask-a-question-you-can-answer-with-data/target-data.png)

Bu örnek yanıt olarak bir hedef adı verilir. Bir kategori veya bir sayı olup olmadığını ne numaralı gelecekteki veri noktaları hakkında tahmin etmek çalışıyoruz hata hedefidir.

Herhangi bir hedef veri yoksa, bazı almanız gerekir. Sorunuzu olmadan yanıt vermiyor.

## <a name="reformulate-your-question"></a>Sorunuzu reformulate
Bazen, sorunuzu daha kullanışlı bir yanıt almak için yeniden ifade et.

"Bu veri noktası A veya B mi?" sorusunu Kategori (adını veya etiketi), bir şeyin tahmin eder. Yanıt için kullanırız bir *sınıflandırma algoritmasıdır*.

Soru "Ne kadar?" veya "Kaç?" bir miktar tahmin eder. Yanıt için kullanırız bir *regresyon algoritması*.

Biz bu nasıl dönüştürebilirsiniz görmek için "hangi haber öykü bu okuyucuya en ilginç mi?" sorusunu bakalım Bu çok sayıda olasılıklar - arasından tek bir seçim tahminini diğer bir deyişle "Bu A veya B veya C D mi?" ister - ve bir sınıflandırma algoritmasıdır kullanırsınız.

Ancak, bu soruyu "Bu okuyucu için bu listede her hikaye nasıl ilginç?" olarak yeniden ifade et varsa yanıt daha kolay olabilir Şimdi her makale sayısal bir puan verebilirsiniz ve sonra Puanlama yüksek makaleyi tanımlamak kolaydır. Sınıflandırma soru regresyon soru içine yeniden budur veya ne kadar?

![Sorunuzu reformulate. Sınıflandırma soru regresyon soru karşılaştırması.](./media/data-science-for-beginners-ask-a-question-you-can-answer-with-data/classification-question-vs-regression-question.png)

Bir soru algoritmayı bir ipucu olan isteyin nasıl yanıt verebilirsiniz.

Algoritma - haber Öykü örneğimizde - olanlar gibi bazı aileleri yakından ilişkilidir bulabilirsiniz. Sorunuzu en yararlı yanıt verir algoritmayı reformulate.

Ancak, en önemlisi, o sharp soru - verilerle yanıtlayabilir soru sorun. Ve yanıtlamak için doğru verilere sahip olduğundan emin olun.

Verilerle yanıtlayabilir bir soru sormak bazı temel ilkeleri hakkında açıklandı.

"Veri bilimi için yeni başlayanlar" Microsoft Azure Machine learning'in diğer videoları kullanıma emin olun.

## <a name="next-steps"></a>Sonraki adımlar
* [İlk veri bilimi denemeyi Machine Learning Studio'da deneyin](create-experiment.md)
* [Microsoft Azure Machine Learning giriş Al](what-is-machine-learning.md)
