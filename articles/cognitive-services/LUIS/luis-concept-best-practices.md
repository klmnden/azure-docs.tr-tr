---
title: HALUK en iyi yöntemler - Azure anlama | Microsoft Docs
description: En iyi sonuçları almak için HALUK en iyi yöntemleri öğrenin.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/08/2018
ms.author: v-geberr;
ms.openlocfilehash: 729f510de59fe27761389fb1f6edb4025021565a
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35356394"
---
# <a name="best-practices"></a>En iyi uygulamalar
İşlem yazma uygulama HALUK uygulamanızı oluşturmak için kullanın. 

* Dil modeli oluşturma
* Birkaç eğitim örnek utterances (10-15 hedefi başına) Ekle
* Yayımlama 
* Uç noktasından test 
* Özellik ekleme

Uygulamanızı olduğunda [yayımlanan](publishapp.md)Özellik Ekle yazma döngüsünü kullanın, yayımlama ve test uç noktasından. Sonraki Geliştirme döngüsü daha fazla örnek utterances ekleyerek başlamaz. Bu, gerçek kullanıcı utterances modelinizi öğrenin HALUK izin vermez. 

Olan geçerli örnek ve uç nokta utterances kümesini döndüren kadar emin, yüksek tahmin puanları HALUK öğrenme işini etkili olması için sırayla utterances genişletmeyin. Etkin öğrenme kullanarak puanlarını iyileştirmek [desenleri](luis-concept-patterns.md), ve [tümceyi listeleri](luis-concept-feature.md). 

## <a name="do-and-dont"></a>Bunu yapmak ve verme
Aşağıdaki listede HALUK uygulamalar için en iyi yöntemleri içerir:

|Yapın|Yapmayın|
|--|--|
|[Ayrı hedefleri tanımlayın](#do-define-distinct-intents) |[Birçok örnek utterances amaçlar için ekleme](#dont-add-many-example-utterances-to-intents) |
|[Her hedefini Lezzetli nokta çok genel arasında çok belirli Bul](#do-find-sweet-spot-for-intents)|[Eğitim platform olarak HALUK kullanın](#dont-use-luis-as-a-training-platform)|
|[Uygulamanızı tekrarlayarak oluşturun](#do-build-the-app-iteratively)|[Diğer biçimlere yoksayılıyor aynı biçimde birçok örnek utterances Ekle](#dont-add-many-example-utterances-of-the-same-format-ignoring-other-formats)|
|[Tümcecik listeler ve desenler sonraki yinelemelerde ekleme](#do-add-phrase-lists-and-patterns-in-later-iterations)|[Hedefleri ve varlıkları tanımını karışımı](#dont-mix-the-definition-of-intents-and-entities)|
|[Örnek utterances hedefi Yok'a ekleme](#do-add-example-utterances-to-none-intent)|[Tüm olası değerlerini tümcecik listeleri oluşturma](#dont-create-phrase-lists-with-all-the-possible-values)|
|[Etkin öğrenme için önerilen özelliğinden yararlanın](#do-leverage-the-suggest-feature-for-active-learning)|[Çok fazla sayıda desenleri ekleme](#dont-add-many-patterns)|
|[Uygulamanızın performansını izleme](#do-monitor-the-performance-of-your-app)|[Eğitmek ve eklenen her tek örnek utterance ile yayımlama](#dont-train-and-publish-with-every-single-example-utterance)|

## <a name="do-define-distinct-intents"></a>Ayrı hedefleri tanımlayın
Yalnızca bu amaç için ve farklı bir hedefi ile çakışan her amaç için Sözlük olduğundan emin olun. Örneğin, uçak uçuşlar ve otel gibi yolculuk hazırlığı işleyen bir uygulama sahip olmak istiyorsanız, bunları ayrı hedefleri veya utterance içinde özel veriler için varlıklarla aynı hedefi olarak sahip olmayı seçebilirsiniz.

İki hedefleri arasında sözlük aynıysa hedefi birleştirin ve varlıkları kullanın. 

Aşağıdaki örnek utterances göz önünde bulundurun:

```
Book a flight
Book a hotel
```

"Bir uçuş kitap" ve "bir otel kitap" aynı sözlüğünü kullanma "kitap bir". Aynı olması gerekir böylece bu çakışan uçuş ve Otel farklı sözcükler amacıyla ayıklanan varlıklar. 

## <a name="do-find-sweet-spot-for-intents"></a>Lezzetli nokta hedefleri için Bul
HALUK tahmin verilerini, hedefleri örtüşmez belirlemek için kullanın. Hedefleri çakışan HALUK kafasını karıştırabilirler. Hedefi Puanlama en yakın olduğunu sonucu olan başka bir hedefi. HALUK her eğitim için verilerde tam aynı yol kullanmadığından, çakışan amacına ilk veya ikinci eğitim olma olasılığı vardır. Bu gerçekleşmez şekilde birbirinden olmasını her amaç için utterance's puan istiyor. İyi ayrım amaçlar için her zaman içinde beklenen üst hedefi neden olur. 
 
## <a name="do-build-the-app-iteratively"></a>Uygulama yinelemeli olarak derleme
Ayrı tutmak *görme* test olarak kullanılmaz kümesi [örnek utterances](luis-concept-utterance.md) veya uç nokta utterances. Sınama kümesi için uygulama geliştirme tutun. Gerçek kullanıcı utterances yansıtmak için test uyarlayın. Her yineleme değerlendirmek üzere bu görme test kullanın. 

Geliştiriciler, üç veri kümesi olması gerekir. İlk model oluşturmaya yönelik örnek utterances ' dir. Uç nokta modeli test etmek için saniyedir. Üçüncü blind test kullanılan verileri olan [toplu sınama](luis-how-to-batch-test.md). Bu son kümesi değil uygulama eğitim kullanılan veya uç noktada gönderilir.  

## <a name="do-add-phrase-lists-and-patterns-in-later-iterations"></a>Tümcecik listeler ve desenler sonraki yinelemelerde ekleme
[Tümcecik listeleri](luis-concept-feature.md) , uygulama etki alanınıza ilgili sözcükler sözlükleri tanımlamanıza olanak sağlar. Çekirdek, tümcecik ile birkaç sözcükleri listeler sonra hakkında daha fazla sözlük sözcükleri HALUK bilmesi için önerilen özelliğini kullanın. Tümcecik listesi tam bir eşleşme değil bu yana her word sözlüğü eklemeyin. 

Gerçek kullanıcı utterances birbirine çok benzer uç noktasından word seçim ve yerleştirme desenlerini olduğunu gösterebilir. [Düzeni](luis-concept-patterns.md) Bu word seçim ve normal ifadeler birlikte yerleştirme tahmin doğruluğunu artırmak için özelliğini alır. Normal ifade deseni ile sözcükleri ve hala desen eşleştirme sırasında yoksay düşündüğünüz noktalama sağlar. 

Noktalama işaretleri göz ardı edilebilir şekilde noktalama deseninin isteğe bağlı sözdizimini kullanın.

Uygulamanızı uç nokta isteği aldı ve güven Eğer çünkü önce bu yöntemler geçerli değildir.  

## <a name="do-add-example-utterances-to-none-intent"></a>Örnek utterances hedefi Yok'a ekleme
Geri dönüş budur amacı, uygulamanızın dışında her şeyi gösterilir. Bir örnek utterance, hiçbiri her 10 örnek utterances HALUK uygulamanızı geri kalanında hedefi ekleyin.

## <a name="do-leverage-the-suggest-feature-for-active-learning"></a>Etkin öğrenme için önerilen özelliğinden yararlanın
Kullanım [etkin öğrenme](label-suggested-utterances.md)'s **gözden geçirin, uç nokta utterances** amaçlar için daha fazla örnek utterances eklemek yerine düzenli olarak,. Uygulama uç noktası utterances sürekli olarak almak için bu listeyi büyüyen ve değiştirme.

## <a name="do-monitor-the-performance-of-your-app"></a>Uygulamanızın performansını izleme
Bir sınama kümesi kullanarak tahmin doğruluğunu izleyin. 

## <a name="dont-add-many-example-utterances-to-intents"></a>Birçok örnek utterances amaçlar için ekleme
Yalnızca uygulama yayımlandıktan sonra utterances yinelemeli işlemde etkin öğrenme ekleme. Utterances çok benzer bir desen ekleyin. 

## <a name="dont-use-luis-as-a-training-platform"></a>HALUK eğitim platform olarak kullanmayın
Bir dil modelinin etki alanına HALUK özeldir. Genel eğitim platform olarak çalışmak üzere tasarlanmamıştır. 

## <a name="dont-add-many-example-utterances-of-the-same-format-ignoring-other-formats"></a>Diğer biçimlere yoksayılıyor aynı biçimde birçok örnek utterances ekleme
HALUK Çeşitlemeler amacına 's utterances bekliyor. Utterances genel ile aynı anlamı yaparken farklılık gösterebilir. Çeşitlemeler utterance uzunluğu, sözcük seçimi ve word yerleştirme içerebilir. 

|Aynı biçimi kullanma|Değişen biçimini kullanın|
|--|--|
|Seattle bilet satın alın<br>Paris bilet satın alın<br>İn konak İlçesindeki bilet satın alın|Seattle için 1 anahtarı satın alma<br>Ayırma Paris sonraki Pazartesi için kırmızı göz üzerindeki iki bilgisayar lisansı<br>3 biletleri için in konak İlçesindeki yay kesmesi için kitap istiyorum|

İkinci sütun kullanır farklı fiiller (satınalma, ayrılmış, kitap), farklı miktarları (1, iki, 3), ve farklı düzenlemeler sözcükleri ancak tüm uçak bileti seyahat için satın alma aynı amaca sahiptir. 

## <a name="dont-mix-the-definition-of-intents-and-entities"></a>Hedefleri ve varlıkları tanımı bir arada kullanmayın
Bot sürer amacına için herhangi bir eylem oluşturun. Varlıklar, bu eylem mümkün kılan parametre olarak kullanın. 

Uçak uçuşlar kitap chatbot için oluşturma bir **BookFlight** hedefi. Her uçak ya da her hedef için amacına oluşturmayın. Bu verileri olarak parçalarını kullanmak [varlıklar](luis-concept-entity-types.md) ve örnek utterances işaretleyin. 

## <a name="dont-create-phrase-lists-with-all-the-possible-values"></a>Olası tüm değerlerle tümcecik listeleri oluşturma
Bazı örneklerde sağlamak [tümceyi listeleri](luis-concept-feature.md) ancak her word. HALUK genelleştirir ve bağlam dikkate alır. 

## <a name="dont-add-many-patterns"></a>Birçok desenleri ekleme
Eklemeyin çok fazla [desenleri](luis-concept-patterns.md). HALUK hızlı bir şekilde daha az örnekleriyle öğrenmek için tasarlanmıştır. Sistem gereksiz yere tekrar yok.

## <a name="dont-train-and-publish-with-every-single-example-utterance"></a>Yoksa eğitmek ve her tek örnek utterance ile yayımlama
10 veya 15 utterances eğitim ve yayımlama önce ekleyin. Tahmin doğruluğunu etkisini görmenize olanak sağlar. Tek bir utterance ekleme görünür bir etkisi üzerinde puan olmayabilir. 

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [uygulamanızı planlama](plan-your-app.md) HALUK uygulamanızda.
[HALUK]: Haluk başvuru regions.md #luis-Web sitesi
