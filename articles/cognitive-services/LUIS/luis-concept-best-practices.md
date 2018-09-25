---
title: LUIS - Language Understanding ile uygulama oluşturmaya yönelik en iyi uygulamalar
titleSuffix: Azure Cognitive Services
description: En iyi sonuçları almak için LUIS en iyi adımları öğrenin.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 511e6c732613cc577644365e38b271135659f2d3
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47042253"
---
# <a name="best-practices-for-building-a-language-understanding-app-with-cognitive-services"></a>Bilişsel hizmetler dil anlama uygulamayla oluşturmaya yönelik en iyi uygulamalar
LUIS uygulamanızı oluşturmak için yazma işleminin hızlandırılmasının uygulamayı kullanın. 

* Dil modeli oluşturun
* Birkaç eğitim örnek konuşma (10-15 hedefi başına) ekleme
* Yayımlama 
* Uç noktasından test 
* Özellik ekleme

Uygulamanız olduğunda [yayımlanan](luis-how-to-publish-app.md), Özellik Ekle geliştirme döngüsünü kullanın, yayımlama ve test uç noktasından. Sonraki geliştirme döngüsünü daha fazla örnek konuşma ekleyerek başlamaz. Modelinizi gerçek kullanıcı konuşma öğrenin LUIS izin vermez. 

Geçerli örnek hem uç nokta konuşma döndürüyor kadar başarılara, yüksek tahmin puanları LUIS öğrenme işini etkili olması için sırada konuşma genişletmeyin. Etkin olarak öğrenmeye kullanarak puanlarını geliştirmeleri [desenleri](luis-concept-patterns.md), ve [tümcecik listeleri](luis-concept-feature.md). 

## <a name="do-and-dont"></a>Bunu yapmak ve yok
Aşağıdaki listede LUIS uygulamaları için en iyi uygulamaları içerir:

|Yapın|Yapmayın|
|--|--|
|[Farklı hedefleri tanımlayın](#do-define-distinct-intents) |[Amaçlar için birçok örnek Konuşma ekleme](#dont-add-many-example-utterances-to-intents) |
|[Her amaç için çok geneldir ve çok belirli arasında bir tatlı nokta bulmak](#do-find-sweet-spot-for-intents)|[LUIS eğitim platformu olarak kullanın](#dont-use-luis-as-a-training-platform)|
|[Yinelemeli olarak uygulamanızı oluşturun](#do-build-the-app-iteratively)|[Diğer biçimlere yoksayılıyor aynı biçimde, birçok örnek Konuşma ekleme](#dont-add-many-example-utterances-of-the-same-format-ignoring-other-formats)|
|[Sonraki yinelemelerde tümcecik listeler ve düzenleri ekleyin](#do-add-phrase-lists-and-patterns-in-later-iterations)|[Hedefleri ve varlıkların tanımı karışımı](#dont-mix-the-definition-of-intents-and-entities)|
|[Hedefi hiçbiri için örnek Konuşma ekleme](#do-add-example-utterances-to-none-intent)|[Olası tüm değerlerle ifade listeleri oluşturma](#dont-create-phrase-lists-with-all-the-possible-values)|
|[Etkin öğrenme için Öner özelliğinden yararlanın](#do-leverage-the-suggest-feature-for-active-learning)|[Çok sayıda düzenleri ekleyin](#dont-add-many-patterns)|
|[Uygulamanızın performansını izleyin](#do-monitor-the-performance-of-your-app)|[Eğitin ve yayımlayın her tek örnek utterance eklendi](#dont-train-and-publish-with-every-single-example-utterance)|

## <a name="do-define-distinct-intents"></a>Farklı hedefleri tanımlayın
Sözlük her hedefi için yalnızca bu amaç için ve farklı bir hedefi ile çakışan olduğundan emin olun. Örneğin, seyahat düzenlemeleri ve Havayolu otelden gibi işleyen bir uygulama istiyorsanız, bunları ayrı hedefleri veya utterance içindeki belirli veri varlıkları ile aynı hedefi olarak sahip olmayı seçebilirsiniz.

Sözlük iki amaçları arasında aynı ise, amacı birleştirin ve varlıkları kullanın. 

Aşağıdaki örnek konuşma göz önünde bulundurun:

```
Book a flight
Book a hotel
```

"Bir kitap" ve "bir otel kitap" aynı sözlüğünü kullanın "kitap bir". Aynı olmalıdır, böylece bu çakışan uçuş ve Otel farklı sözcük hedefle ayıklanan varlıklar. 

## <a name="do-find-sweet-spot-for-intents"></a>Tatlı nokta hedefleri için Bul
LUIS verilerden öngörü, hedefleri örtüşmez belirlemek için kullanın. Intents çakışan LUIS kafasını karıştırabilirler. Hedefi Puanlama en yakın olduğunu sonucudur başka bir amaç. LUIS her zaman eğitim verilerde aynı tam yolunu kullanmadığından, ilk veya ikinci eğitim olma şansı çakışan bir amacı vardır. Utterance'nın puanı bu gerçekleşmez birbirinden olması, her amaç için kullanmanız gerekir. İyi ayrım hedefleri için beklenen üst amaca her seferinde neden olur. 
 
## <a name="do-build-the-app-iteratively"></a>Uygulama çalıştırmalarınızı derleme
Ayrı tutmak *görme* test kümesi olarak kullanılmaz [örnek konuşma](luis-concept-utterance.md) veya konuşma uç noktası. Uygulamayı test kümeniz için geliştirilir. Gerçek kullanıcı konuşma yansıtmak için test uyarlayın. Her yineleme değerlendirmek üzere bu görme test kullanın. 

Geliştiriciler, üç veri kümesi olması gerekir. Model oluşturmak için örnek konuşma davranıştır. Uç nokta modeli test etmek için saniyedir. Üçüncü blind test kullanılan verileri olduğu [toplu test](luis-how-to-batch-test.md). Bu son kümesi değil uygulama eğitim içinde kullanılan veya uç noktada gönderilir.  

## <a name="do-add-phrase-lists-and-patterns-in-later-iterations"></a>İfade listeleri ve desenler sonraki yinelemelerde ekleme
[Tümcecik listeleri](luis-concept-feature.md) sözlükleri bir kelimelerin uygulama etki alanınızla ilişkili tanımlamanızı sağlar. Çekirdek, tümcecik birkaç sözcük ile liste sonra LUIS hakkında daha fazla sözcük sözlüğünü bilmesi Öner özelliğini kullanın. Her sözcük, tümcecik listesi değil tam bir eşleşme olduğundan sözlüğü eklemeyin. 

Gerçek kullanıcı konuşma birbirine çok benzer uç noktasından word seçim ve yerleştirme düzenlerini gösterilmesine neden olabilir. [Deseni](luis-concept-patterns.md) özelliği bu sözcük seçimi ve normal ifadeler birlikte yerleştirme, tahmin doğruluğunu artırmak için alır. Bir normal ifade deseninde sözcükler ve noktalama işaretleri hala desen eşleştirme sırasında yok saymak için istediğinize sağlar. 

Noktalama işaretleri göz ardı edilebilir şekilde noktalama deseninin isteğe bağlı söz dizimi kullanın.

Bu yöntemler, güvenirlik eğriltir çünkü uygulamanızı uç nokta isteği aldı önce geçerli değildir.  

## <a name="do-add-example-utterances-to-none-intent"></a>Örnek konuşma hiçbiri hedefi ekleme
Bu geri dönüş, amacı, uygulamanızın dışında her şeyi gösterilir. Bir örnek utterance hiçbiri LUIS uygulamanızı geri kalanında her 10 örnek konuşma için hedefi ekleyin.

## <a name="do-leverage-the-suggest-feature-for-active-learning"></a>Etkin öğrenme için Öner özelliğinden yararlanın
Kullanım [etkin olarak öğrenmeye](luis-how-to-review-endoint-utt.md)'s **gözden geçirin, konuşma uç noktası** hedefleri için daha fazla örnek konuşma eklemek yerine düzenli olarak. Uygulamayı sürekli olarak konuşma uç noktası almak için bu listeyi artan ve değiştirme.

## <a name="do-monitor-the-performance-of-your-app"></a>Uygulamanızın performansını izleyin
Bir sınama kümesi kullanarak tahmin doğruluğunu izleyin. 

## <a name="dont-add-many-example-utterances-to-intents"></a>Birçok örnek konuşma için hedefleri ekleme
Yalnızca uygulama yayımlandıktan sonra konuşma bir süreçtir, etkin öğrenim ekleyin. Konuşma çok benzer bir desen ekleyin. 

## <a name="dont-use-luis-as-a-training-platform"></a>LUIS eğitim platformu olarak kullanmayın
LUIS, bir dil modeli etki özeldir. Bir genel eğitim platformu olarak çalışmak üzere tasarlanmamıştır. 

## <a name="dont-add-many-example-utterances-of-the-same-format-ignoring-other-formats"></a>Diğer biçimlere yoksayılıyor aynı biçimde, birçok örnek Konuşma ekleme
LUIS, içinde bir amaç'ın konuşma çeşitlemeleri bekliyor. Konuşma, aynı genel anlamı yaparken farklılık gösterebilir. Değişimleri utterance uzunluğu, word seçeneği ve word yerleştirme içerebilir. 

|Aynı biçimi kullanma|Değişen biçimini kullanın|
|--|--|
|Seattle bilet satın alma<br>Paris bilet satın alma<br>Orlando bilet satın alma|Seattle 1 bilet satın alma<br>Ayırma paris'e sonraki Pazartesi kırmızı göz üzerindeki iki bilgisayar lisansı<br>3 biletleri için spring sonu tarihleri arasında Orlando için kitap istiyorum|

Farklı miktarlarını, ikinci sütun kullanan farklı fiiller (satın alma, ayrılmış, kitabı) (1, iki, 3), ve sözcükler ancak tüm farklı düzenlenişlerini uçak bileti seyahat satın bağdaştırıcılar aynı amaca sahip. 

## <a name="dont-mix-the-definition-of-intents-and-entities"></a>Hedefleri ve varlıkların tanımı bir arada kullanmayın
Botunuzun sürecek bir hedefi için herhangi bir işlem oluşturun. Varlıklar, bu eylem mümkün kılan parametreleri olarak kullanma. 

Havayolu uçuşlar kitap bir sohbet Robotu için oluşturun bir **BookFlight** hedefi. Her Havayolu ya da her hedef için bir hedefi oluşturmayın. Bu parçaları veri kullanan [varlıkları](luis-concept-entity-types.md) ve bunları örnek konuşma işaretleyin. 

## <a name="dont-create-phrase-lists-with-all-the-possible-values"></a>Olası tüm değerlerle ifade listeleri oluşturma
Bazı örneklerde sağlamak [tümcecik listeleri](luis-concept-feature.md) ancak her sözcük. LUIS genelleştirir ve bağlam dikkate alır. 

## <a name="dont-add-many-patterns"></a>Birçok desen ekleme
Ekleme çok fazla [desenleri](luis-concept-patterns.md). LUIS, daha az sayıda örnek ile hızlı bir şekilde öğrenmek için tasarlanmıştır. Sistem gereksiz yere aşırı yükleme yapmaz.

## <a name="dont-train-and-publish-with-every-single-example-utterance"></a>Yoksa, eğitmek ve her tek örnek utterance ile yayımlama
10 veya 15 konuşma, eğitim ve yayımlamadan önce ekleyin. Tahmin doğruluğunu etkisini görmenize olanak sağlar. Tek bir utterance ekleme puanına göre görünür bir etkisi olmayabilir. 

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [uygulamanızı planlama](luis-how-plan-your-app.md) LUIS uygulamanızda.