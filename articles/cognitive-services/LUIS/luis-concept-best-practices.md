---
title: En iyi uygulamalar
titleSuffix: Language Understanding - Azure Cognitive Services
description: LUIS uygulama modelinden en iyi sonuçları almak için LUIS en iyi adımları öğrenin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: diberry
ms.openlocfilehash: 9a6f9d54c52f36b8f709eacaf25d3fea31dbe516
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60812920"
---
# <a name="best-practices-for-building-a-language-understanding-app-with-cognitive-services"></a>Bilişsel hizmetler dil anlama uygulamayla oluşturmaya yönelik en iyi uygulamalar
LUIS uygulamanızı oluşturmak için yazma işleminin hızlandırılmasının uygulamayı kullanın. 

* Dil modeli oluşturun
* Birkaç eğitim örnek konuşma (10-15 hedefi başına) ekleme
* Yayımlama 
* Uç noktasından test 
* Özellik ekleme

Uygulamanız olduğunda [yayımlanan](luis-how-to-publish-app.md), Özellik Ekle geliştirme döngüsünü kullanın, yayımlama ve test uç noktasından. Sonraki geliştirme döngüsünü daha fazla örnek konuşma ekleyerek başlamaz. Modelinizi gerçek kullanıcı konuşma öğrenin LUIS izin vermez. 

Geçerli örnek hem uç nokta konuşma döndürüyor kadar başarılara, yüksek tahmin puanları LUIS öğrenme işini etkili olması için sırada konuşma genişletmeyin. Puanlarını kullanarak iyileştirmek [etkin olarak öğrenmeye](luis-concept-review-endpoint-utterances.md), [desenleri](luis-concept-patterns.md), ve [tümcecik listeleri](luis-concept-feature.md). 

## <a name="do-and-dont"></a>Bunu yapmak ve yok
Aşağıdaki listede LUIS uygulamaları için en iyi uygulamaları içerir:

|Yapın|Yapmayın|
|--|--|
|[Farklı hedefleri tanımlayın](#do-define-distinct-intents) |[Amaçlar için birçok örnek Konuşma ekleme](#dont-add-many-example-utterances-to-intents) |
|[Her amaç için çok geneldir ve çok belirli arasında bir tatlı nokta bulmak](#do-find-sweet-spot-for-intents)|[LUIS eğitim platformu olarak kullanın](#dont-use-luis-as-a-training-platform)|
|[Yinelemeli olarak uygulamanızı oluşturun](#do-build-the-app-iteratively)|[Diğer biçimlere yoksayılıyor aynı biçimde, birçok örnek Konuşma ekleme](#dont-add-many-example-utterances-of-the-same-format-ignoring-other-formats)|
|[Sonraki yinelemelerde tümcecik listeler ve düzenleri ekleyin](#do-add-phrase-lists-and-patterns-in-later-iterations)|[Hedefleri ve varlıkların tanımı karışımı](#dont-mix-the-definition-of-intents-and-entities)|
|[Tüm hedefleri arasında konuşma Bakiye](#balance-your-utterances-across-all-intents) dışındaki hiçbir hedefi.<br>[Hedefi hiçbiri için örnek Konuşma ekleme](#do-add-example-utterances-to-none-intent)|[Olası tüm değerlerle ifade listeleri oluşturma](#dont-create-phrase-lists-with-all-the-possible-values)|
|[Etkin öğrenme için Öner özelliğinden yararlanın](#do-leverage-the-suggest-feature-for-active-learning)|[Çok fazla düzenleri ekleyin](#dont-add-many-patterns)|
|[Uygulamanızın performansını izleyin](#do-monitor-the-performance-of-your-app)|[Eğitin ve yayımlayın her tek örnek utterance eklendi](#dont-train-and-publish-with-every-single-example-utterance)|
|[Her uygulama yineleme için sürümleri kullanın](#do-use-versions-for-each-app-iteration)||

## <a name="do-define-distinct-intents"></a>Farklı hedefleri tanımlayın
Sözlük her hedefi için yalnızca bu amaç için ve farklı bir hedefi ile çakışan olduğundan emin olun. Örneğin, işler ve Havayolu otelden gibi düzenlemeleri seyahat bir uygulama istiyorsanız, ayrı hedefleri veya utterance içindeki belirli veri varlıkları ile aynı hedefi olarak bu konu alanlarını bulunmasını seçebilirsiniz.

Sözlük iki amaçları arasında aynı ise, amacı birleştirin ve varlıkları kullanın. 

Aşağıdaki örnek konuşma göz önünde bulundurun:

|Örnek konuşmalar|
|--|
|Kitap bir uçuş|
|Kitap bir otel|

"Bir kitap" ve "bir otel kitap" aynı sözlüğünü kullanın "kitap bir". Uçuş ve Otel farklı sözcük ile aynı amaç ayıklanan varlıklar olarak olması gerektiği için bu biçim aynıdır. 

Daha fazla bilgi için:
* Kavram: [LUIS uygulamanızda hedefleri hakkında kavramları](luis-concept-intent.md)
* Öğretici: [Kullanıcı amaçları belirlemek için LUIS uygulaması oluşturma](luis-quickstart-intents-only.md)
* Nasıl yapılır: [Konuşma kullanıcı amacınıza belirlemek için hedef ekleme](luis-how-to-add-intents.md)


## <a name="do-find-sweet-spot-for-intents"></a>Tatlı nokta hedefleri için Bul
LUIS verilerden öngörü, hedefleri örtüşmez belirlemek için kullanın. Çakışan bir ıntents LUIS aklını karıştırabilir. Hedefi Puanlama en yakın olduğunu sonucudur başka bir amaç. LUIS her zaman eğitim verilerde aynı tam yolunu kullanmadığından, ilk veya ikinci eğitim olma şansı çakışan bir amacı vardır. Utterance'nın puanı bu Çevir/flop gerçekleşmez birbirinden olması, her amaç için kullanmanız gerekir. İyi ayrım hedefleri için beklenen üst amaca her seferinde neden olur. 
 
## <a name="do-build-the-app-iteratively"></a>Uygulama çalıştırmalarınızı derleme
Ayrı olarak kullanılmayan konuşma birtakım tutmak [örnek konuşma](luis-concept-utterance.md) veya konuşma uç noktası. Uygulamayı test kümeniz için geliştirilir. Gerçek kullanıcı konuşma yansıtmak için test uyarlayın. Her bir yineleme veya uygulama sürümünü değerlendirmek üzere bu test kullanın. 

Geliştiriciler, üç veri kümesi olması gerekir. Model oluşturmak için örnek konuşma davranıştır. Uç nokta modeli test etmek için saniyedir. Üçüncü blind test kullanılan verileri olduğu [toplu test](luis-how-to-batch-test.md). Bu son kümesi olmayan uygulama eğitim içinde kullanılan ya da uç noktada gönderilir.  

Daha fazla bilgi için:
* Kavram: [LUIS uygulama döngüsü yazma](luis-concept-app-iteration.md)

## <a name="do-add-phrase-lists-and-patterns-in-later-iterations"></a>İfade listeleri ve desenler sonraki yinelemelerde ekleme

Uygulamanızı test önce bu uygulamaları uygulamak iyi bir uygulamadır. Bu özellikler örnek konuşma daha yoğun ağırlıklı ve güvenle eğme çünkü deyim listeler ve desenleri eklemeden önce uygulamanın davranışını anlamanız gerekir. 

Uygulamanızın bu nasıl davrandığını anladıktan sonra bu özelliklerin her biri, uygulamanız için geçerli olan ekleyin. Bu özelliklerin her eklemek gerekmez [yineleme](luis-concept-app-iteration.md) veya özellikler her sürümü ile değiştirin. 

Model tasarımınızı başında eklemeden hiçbir zarar yoktur ancak her bir özelliğin model Konuşma ile test edildikten sonra sonuçları nasıl değiştiğini görmek daha kolaydır. 

Aracılığıyla test etmek için en iyi uygulamadır [uç nokta](luis-get-started-create-app.md#query-the-endpoint-with-a-different-utterance) eklenen avantajı elde etmeniz [etkin olarak öğrenmeye](luis-concept-review-endpoint-utterances.md). [Etkileşimli test bölmesini](luis-interactive-test.md) geçerli bir test yöntemi ayrıca olur. 
 

### <a name="phrase-lists"></a>Tümcecik listeleri

[Tümcecik listeleri](luis-concept-feature.md) sözlükleri bir kelimelerin uygulama etki alanınızla ilişkili tanımlamanızı sağlar. Çekirdek, ifade listesi ile birkaç sözcük sonra uygulamanıza LUIS hakkında daha fazla sözcük içindeki belirli kelime bilmesi Öner özelliğini kullanın. Bir ifade listesi, sözcük ve tümcecikleri uygulamanız için önemli olan ile ilişkili sinyal yükseltme tarafından hedefi olan algılama ve varlık sınıflandırması artırır. 

Her sözcük, tümcecik listesi değil tam bir eşleşme olduğundan sözlüğü eklemeyin. 

Daha fazla bilgi için:
* Kavram: [LUIS uygulamanızı ifade listesi özellikleri](luis-concept-feature.md)
* Nasıl yapılır: [Kullanım deyimi word listesinin boost sinyale listeler](luis-how-to-add-features.md)

### <a name="patterns"></a>Desenler

Gerçek kullanıcı konuşma birbirine çok benzer uç noktasından word seçim ve yerleştirme düzenlerini gösterilmesine neden olabilir. [Deseni](luis-concept-patterns.md) özelliği bu sözcük seçimi ve normal ifadeler birlikte yerleştirme, tahmin doğruluğunu artırmak için alır. Bir normal ifade deseninde sözcükler ve noktalama işaretleri hala desen eşleştirme sırasında yok saymak için istediğinize sağlar. 

Kullanım deseninin [isteğe bağlı söz dizimi](luis-concept-patterns.md) noktalama göz ardı edilebilir şekilde noktalama. Kullanım [açık listesi](luis-concept-patterns.md#explicit-lists) pattern.any söz dizimi sorunları için dengelemek için. 

Daha fazla bilgi için:
* Kavram: [Desenlerini tahmin doğruluğunu artırmak](luis-concept-patterns.md)
* Nasıl yapılır: [Nasıl tahmin doğruluğunu artırmak için düzenleri ekleyin](luis-how-to-model-intent-pattern.md)

## <a name="balance-your-utterances-across-all-intents"></a>Tüm hedefleri arasında konuşma Bakiye

Doğru olmasını LUIS tahminler elde etmek için sırada (dışında hiçbiri hedefi), her amaç, örnek konuşma miktarı oldukça eşit olmalıdır. 

100 örnek Konuşma ile bir hedefi ve 20 örnek Konuşma ile bir hedefi varsa, 100 utterance hedefi tahmin daha yüksek fiyatı olacaktır.  

## <a name="do-add-example-utterances-to-none-intent"></a>Örnek konuşma hiçbiri hedefi ekleme

Geri dönüş Bu hedefi olan amacı, uygulamanızın dışında her şeyi gösterilir. Bir örnek utterance hiçbiri LUIS uygulamanızı geri kalanında her 10 örnek konuşma için hedefi ekleyin.

Daha fazla bilgi için:
* Kavram: [LUIS uygulamanızı iyi konuşma neler olduğunu anlama](luis-concept-utterance.md)

## <a name="do-leverage-the-suggest-feature-for-active-learning"></a>Etkin öğrenme için Öner özelliğinden yararlanın

Kullanım [etkin olarak öğrenmeye](luis-how-to-review-endpoint-utterances.md)'s **gözden geçirin, konuşma uç noktası** hedefleri için daha fazla örnek konuşma eklemek yerine düzenli olarak. Uygulamayı sürekli olarak konuşma uç noktası almak için bu listeyi artan ve değiştirme.

Daha fazla bilgi için:
* Kavram: [Etkin öğrenme konuşma uç noktası inceleyerek etkinleştirmek için kavramları](luis-concept-review-endpoint-utterances.md)
* Öğretici: [Öğretici: Konuşma uç noktası inceleyerek emin değilseniz Öngörüler Düzelt](luis-tutorial-review-endpoint-utterances.md)
* Nasıl yapılır: [LUIS portalında konuşma uç noktası İnceleme](luis-how-to-review-endpoint-utterances.md)

## <a name="do-monitor-the-performance-of-your-app"></a>Uygulamanızın performansını izleyin

Tahmin doğruluğunu kullanarak izleme bir [toplu test](luis-concept-batch-test.md) ayarlayın. 

## <a name="dont-add-many-example-utterances-to-intents"></a>Birçok örnek konuşma için hedefleri ekleme

Yalnızca uygulama yayımlandıktan sonra konuşma bir süreçtir, etkin öğrenim ekleyin. Konuşma çok benzer bir desen ekleyin. 

## <a name="dont-use-luis-as-a-training-platform"></a>LUIS eğitim platformu olarak kullanmayın

LUIS, bir dil modeli etki özeldir. Bir genel doğal dil eğitim platformu olarak çalışmak için tasarlanmamıştır. 

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

## <a name="do-use-versions-for-each-app-iteration"></a>Her uygulama yineleme için sürümleri kullanın

Her geliştirme döngüsü içinde yeni bir olmalıdır [sürüm](luis-concept-version.md), var olan bir sürümünden kopyalanan. LUIS, sürümler için sınır yoktur. Dolayısıyla bir URL'de izin yanı sıra bir sürümü için 10 karakter sayısı içinde tutarak karakter seçmek önemli bir sürüm adı API yolun bir parçası olarak kullanılır. Sürümlerinizi düzenli tutmak için bir sürüm adı strateji geliştirin. 

Daha fazla bilgi için:
* Kavram: [Nasıl ve ne zaman anlamak LUIS sürümünü kullanmak için](luis-concept-version.md)
* Nasıl yapılır: [Düzenle ve hazırlık veya üretim uygulamaları etkilemeden test sürümleri kullanın](luis-how-to-manage-versions.md)


## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [uygulamanızı planlama](luis-how-plan-your-app.md) LUIS uygulamanızda.
