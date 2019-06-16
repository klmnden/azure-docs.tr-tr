---
title: Veri yanıtlayabileceğiniz bir soru sor
titleSuffix: Azure Machine Learning Studio
description: Video 3 yeni başlayanlar için veri bilimi sharp veri bilimi soru formüle öğrenin. Sınıflandırma ve regresyon sorular karşılaştırması içerir.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: sdgilley
ms.author: sgilley
ms.date: 03/22/2019
ms.openlocfilehash: 7343692e8484e50a02963b4528889a35cc1fcaa6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66239070"
---
# <a name="ask-a-question-you-can-answer-with-data"></a>Verilerle yanıtlayabileceğiniz bir soru sorun
## <a name="video-3-data-science-for-beginners-series"></a>Video 3: Seri yeni başlayanlar için veri bilimi
Soru-video 3 yeni başlayanlar için veri bilimi uygulamasına bir veri bilimi sorun formüle öğrenin. Bu videoda, soru sınıflandırma ve regresyon algoritmalar için bir karşılaştırma içerir.

En yetersiz serisi almak için tüm bunları izleyin. [Videoları listesine Git](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Data-science-for-beginners-Ask-a-question-you-can-answer-with-data/player]
>
>

## <a name="other-videos-in-this-series"></a>Bu serideki diğer videolar
*Yeni başlayanlar için veri bilimi* beş kısa videoyu veri bilimine hızlı bir giriş niteliğindedir.

* Video 1: [5 veri biliminin yanıtladığı sorular](data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 en az 14 sn)*
* Video 2: [Verileriniz veri bilimi için hazır mı?](data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 dk 56 sn)*
* Video 3: Verilerle yanıtlayabileceğiniz bir soru sorun
* Video 4: [Basit bir model ile yanıtı tahmin etme](data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 dk 42 sn)*
* Video 5: [Veri bilimi için başkalarının çalışmalarını kopyalama](data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 dk 18 sn)*

## <a name="transcript-ask-a-question-you-can-answer-with-data"></a>Transkript: Verilerle yanıtlayabileceğiniz bir soru sorun
Serinin üçüncü videoda "Yeni başlayanlar için veri bilimi." Hoş Geldiniz  

Bunu, verilerle yanıtlayabileceğiniz bir soru formulating için bazı ipuçları elde edersiniz.

Varsa, ilk iki önceki bu bölümdeki videoları dışında bu videoda, daha fazla bilgi alabilirsiniz: "5 sorular veri bilimi neredeyse tümünü cevaplayabilir" ve "Is. verileriniz veri bilimi için hazır mı?"

## <a name="ask-a-sharp-question"></a>NET bir soru sorun
Veri bilimi (kategorileri veya etiketleri olarak da bilinir) adlarını ve numaralarını kullanarak bir soruya yanıtı tahmin etme işleminin ne olduğu hakkında konuştuk. Ancak herhangi bir soru olamaz. olmak zorundadır bir *sharp soru.*

Bir ad veya sayı ile yanıtlanması unutmayacağınız bir soru yok. Bir bir sharp soru gerekir.

Imagine Beyanları, sormak istediğiniz soru yanıt bir genie ile Sihirli bir lamp bulunamadı. Ancak bunlar hemen kullanmaya başlayabilirsiniz gibi kendi yanıt belirsiz ve karmaşık hale çalışan yaramaz genie. Bunlar olamaz yardımcı ancak öğrenmek istediklerinizi söyleyin, bu nedenle airtight soru sabitleme istiyorsunuz.

Genie yanıt "my hisse olmasını neler oluyor?", "fiyat değişir" gibi belirsiz bir soru sormak için olsaydı. Doğru yanıt olan, ancak bu çok yararlı değildir.

Ancak gibi "ne my stok ait satış fiyatı sonraki hafta?", genie olamaz yardımcı ancak size belirli bir bir sharp soru sorun olsaydı yanıt ve satış fiyatı tahmin edin.

## <a name="examples-of-your-answer-target-data"></a>Yanıtınız örnekleri: Hedef veri
Sorunuzu formüle sonra verilerinizi yanıt örnekleri sahip olup olmadığını denetleyin.

Bizim soru ise "Ne my stok ait satış fiyatı sonraki hafta olacak?" ardından hisse senedi fiyatı geçmişi verilerimizi içerdiğinden emin olmak sahibiz.

Bizim soru ise "my fleet hangi Arabada ilk başarısız olmasına geçiyor?" ardından verilerimizi önceki hatalarıyla ilgili bilgiler içerdiğinden emin olmak sahibiz.

![Hedef veri - yanıtınız örnekleri. Veri bilimi soru düzenleyin.](./media/data-science-for-beginners-ask-a-question-you-can-answer-with-data/target-data.png)

Bu örnekler yanıtların bir hedef olarak adlandırılır. Bir kategori veya bir sayı olup olmadığını ne gelecekteki veri noktaları hakkında tahmin çalışıyoruz bir hedeftir.

Herhangi bir hedef veri yoksa, bazı almanız gerekir. Bu olmadan, sorunuzun yanıtlanması mümkün olmayacaktır.

## <a name="reformulate-your-question"></a>Sorunuzu yeniden oluşturun
Bazen, sorunuza daha kullanışlı bir yanıt almak için yeniden ifade et.

"Bu veri noktası A veya B nedir?" sorusu Kategori (adını veya etiket) herhangi bir şeyin tahmin eder. Yanıt için kullandığımız bir *sınıflandırma algoritmasıdır*.

Soruya "Ne kadar?" veya "Kaç?" miktarı tahmin eder. Yanıt için kullandığımız bir *regresyon algoritması*.

Biz bu nasıl dönüştürebileceğinizi öğrenmek için "hangi haber hikayesi bu okuyucu en ilginç nedir?" sorusunun göz atalım Bu tahmin birçok olasılıklar - arasından tek bir seçim, diğer bir deyişle "Bu A veya B veya C D mı?" ister - ve bir sınıflandırma algoritmasıdır kullanırsınız.

Ancak, bu soruyu "Bu okuyucu için bu listedeki her hikaye nasıl ilginç nedir?" olarak yeniden ifade et, yanıt daha kolay olabilir Artık her bir makaleyi sayısal bir puan verebilirsiniz ve sonra yüksek puanlı makaleyi kolayca olur. Bu regresyon soru içine sınıflandırma soru anlayamadım, veya ne kadar?

![Sorunuzu yeniden oluşturun. Sınıflandırma soru regresyon soru karşılaştırması.](./media/data-science-for-beginners-ask-a-question-you-can-answer-with-data/classification-question-vs-regression-question.png)

Nasıl bir ipucudur hangi algoritmasına bir soru sorun, yanıt verebilirsiniz.

Algoritma - haber hikayesi örneğimizde - olanlar gibi bazı aileleri yakından ilişkili olduğunu bulabilirsiniz. Sorunuzu en kullanışlı yanıt veren algoritmasını kullanabilmesi için yeniden oluşturun.

Ancak, en önemli - sharp soruya verilerle yanıtlayabileceğiniz bir soru sorun. Ve yanıtlamak için doğru verilere sahip olduğunuzdan emin olun.

Verilerle yanıtlayabileceğiniz bir soru sormaya yönelik bazı temel ilkeler hakkında konuştuk.

"Veri bilimi için yeni başlayanlar" Microsoft Azure Machine Learning Studio'dan diğer videoları kullanıma emin olun.

## <a name="next-steps"></a>Sonraki adımlar
* [Bir Machine Learning Studio'da ilk veri bilimi deneyinde deneyin](create-experiment.md)
* [Microsoft Azure'da Machine learning'e giriş yapın](/azure/machine-learning/preview/overview-what-is-azure-ml)
