---
title: Özellikler
titleSuffix: Language Understanding - Azure Cognitive Services
description: Bir dil modeli, ipuçlarını, etiket veya sınıflandırmak istediğiniz giriş anlamayı hakkında sağlamaya özellikleri ekleyin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: diberry
ms.openlocfilehash: 7889f223b607912fd88c798b31ec028f97dfbbd6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60812930"
---
# <a name="phrase-list-features-in-your-luis-app"></a>LUIS uygulamanızı ifade listesi özellikleri

Machine learning'de bir *özellik* bir ayırt edici nitelik veya sisteminizin gözlemler veri özniteliği. 

Bir dil modeli, ipuçlarını, etiket veya sınıflandırmak istediğiniz giriş anlamayı hakkında sağlamaya özellikleri ekleyin. Özellikleri, hedefleri ve varlıkları hem LUIS yardımcı olur, ancak özellikleri, hedefleri ve varlıkları kendilerini değildir. Bunun yerine, özellikler, ilgili koşulları örnekleri sağlayabilir.  

## <a name="what-is-a-phrase-list-feature"></a>Bir ifade listesi özelliği nedir?
Bir ifade listesi sözcük ve tümcecikleri uygulamanız için önemli olan bir listesidir konuşma başka bir deyişle daha şekilde. Bir ifade listesi uygulama etki alanının sözlüğü LUIS için ek bir sinyal sözcükleri hakkında olarak ekler. LUIS bunları biri hakkında ne öğrenir otomatik olarak başkaları için de uygulanır. Bu liste kapalı değil [varlık listesinde](luis-concept-entity-types.md#types-of-entities) tam metnin eşleşir.

İfade listeleri, herhangi bir önemli sözlük sözcük ve tümcecikleri için dallanma çeşitli kullanan utterance örnekler eklemeniz gereken şekilde ayırmayla yardımcı yapın.

## <a name="phrase-lists-help-all-models"></a>Tüm modelleri tümcecik listeler yardımcı

İfade listeleri bir belirli bir amaç veya varlığa bağlı değildir, ancak tüm hedefleri ve varlıklar için önemli bir boost eklenir. Amacı hedefi olan algılama ve varlık sınıflandırma artırmaktır.

## <a name="how-to-use-phrase-lists"></a>İfade listeleri kullanma

Uygulamanızı sözcük ve tümcecikleri gibi uygulama için önemli olan sahip olduğunda bir ifade listesi oluşturun:

* sektör terimleri
* argo kullanımlar
* kısaltmalar
* şirkete özgü dili
* başka bir dilden ancak uygulamanızda sık kullanılan dil
* anahtar sözcük ve tümcecikleri, örnek konuşma

Birkaç sözcük ve tümcecikleri girdikten sonra kullanın **önerilir** ilgili değerleri bulmak için özellik. İlişkili değerleri, ifade listesi değerlerinizi eklemeden önce gözden geçirin.

|Liste türü|Amaç|
|--|--|
|Değiştirilebilir|Eş Anlamlılar veya sözcük başka bir sözcüğe listesinde değiştirildiğinde aynı amacı ve varlık ayıklama sahip.|
|Olmayan değiştirilebilir|Uygulama sözlük, uygulamanıza, genellikle daha başka bir dilde sözcükler için daha fazla belirli.|

### <a name="interchangeable-lists"></a>Birbirinin yerine listeler

Bir *birbirinin yerine* tümcecik listedir eş anlamlılar için değerler. Örneğin, isterseniz tüm gövdeleri su bulundu ve örnek konuşma gibi sahip: 

* Great Lakes yakın şehirler nelerdir? 
* Hangi yol Lake Havasu çalışır?
* Burada nil başlayıp bitmeyen? 

Her utterance hedefi hem varlıkları su gövdesinin bağımsız olarak belirlenmesi: 

* [BodyOfWater yakın] şehirler nelerdir?
* Hangi yol [bodyOfWater] çalışır?
* Burada [bodyOfWater] başlayıp bitmeyen? 

Sözcük ve tümcecikleri su gövdesi için eşanlamlı ve konuşma içinde birbirinin yerine kullanılabilir olduğundan kullanın **Interchangeable** tümcecik listede ayarlama. 

### <a name="non-interchangeable-lists"></a>Olmayan değiştirilebilir listeler

Değiştirilebilir olmayan bir ifade listesini algılamaya ve LUIS artırıyor bir sinyaldir. Sözcük ve tümcecikleri daha önemli olan, ifade listesi, başka bir deyişle gösterir. Bu, her iki belirleme amacı ve varlık algılamayla yardımcı olur. Örneğin, bir konu etki alanı gibi genel (anlamı kültürler arasında ancak hala tek bir dilde) seyahat olduğunu varsayalım. Sözcükleri ve uygulamaya önemlidir, ancak eşanlamlı olmayan tümcecikler vardır. 

Başka bir örnek için değiştirilebilir olmayan bir ifade listesini için nadir, özel ve yabancı kelimeler kullanın. LUIS yabancı kelimeler (dışında uygulamanın kültür) yanı sıra, nadir ve özel sözcük tanıyamadı olabilir. Nadir bir sözcükler kümesini LUIS tanımayı öğrenin bir sınıf oluşturur, ancak eş anlamlılar olmayan değiştirilebilir olmayan ayarını gösterir veya birbirleri ile değiştirilebilir.

Değil her olası bir sözcük veya tümcecik bir ifade listesine ekleyin, birkaç sözcük ve tümcecikleri bir anda eklemek daha sonra yeniden eğitme ve yayımlama. 

İfade listesi zamanla büyüdükçe, bazı terimler sahip birçok form (eş anlamlılar) bulabilirsiniz. Bunlar değiştirilebilir başka bir ifade listesine ayırmak. 

<a name="phrase-lists-help-identify-simple-exchangeable-entities"></a>

## <a name="phrase-lists-help-identify-simple-interchangeable-entities"></a>Tümcecik listeler Yardım basit birbirinin yerine varlıkları tanımlama
Birbirinin yerine ifade listeleri LUIS uygulamanızın performansını ayarlamak için iyi bir yoludur. Uygulamanızın doğru amaç konuşma tahmin etme veya varlık tanıma sorun varsa, konuşma olağan dışı bir sözcük veya anlamları belirsiz olabilir sözcükler içeren hakkında düşünün. Bu sözcükler tümcecik listesinde içermek için iyi adaylar değildir.

## <a name="phrase-lists-help-identify-intents-by-better-understanding-context"></a>Tümcecik listeler Yardım hedefleri daha iyi anlama bağlamdan tanımlayın
Bir ifade listesi LUIS sıkı eşleştirme işlemi yapmak veya her zaman deyim listesindeki tüm koşulları tamamen aynı etiket için bir yönerge değil. Bu, yalnızca bir ipucu verir. Örneğin, "Patti" ve "Selma" adların olup olmadığını belirten bir ifade listesi olabilir, ancak LUIS kullanmaya devam edebilirsiniz bağlamsal bilgiler farklı bir şey anlamına olduğunu bilmek "2 için bir ayırma sırasında Patti'nın Diner Akşam olun" ve "bana sürüş bulma yönlendirmeler Selma, Gürcistan". 

Bir ifade listesine eklenmesi, daha fazla örnek konuşma eklemeye yönelik bir amacı bir alternatiftir. 

## <a name="when-to-use-phrase-lists-versus-list-entities"></a>Ne zaman tümceciğini kullanın varlıklar listesi listeler
Bir ifade listesi hem liste varlıklar arasında tüm hedefleri konuşma etkileyebilir, ancak her bunu farklı bir şekilde yapar. Hedefi tahmin puanı etkilemek için bir ifade listesini kullanın. Bir liste varlığı, bir tam metin eşleşmesi için varlık ayıklama etkilemek için kullanın. 

### <a name="use-a-phrase-list"></a>Bir ifade listesini kullanın
Bir ifade listesiyle LUIS hala bağlam dikkate alın ve benzer öğeleri, ancak tam bir eşleşme yok, listedeki öğeleri tanımlamak için generalize. LUIS uygulamanızı generalize ve bir kategorideki yeni öğeleri tanımlamak için gerekiyorsa, bir ifade listesini kullanın. 

Başka türde bir makine öğrenilen varlık gibi basit bir varlık, bir varlığın yeni kişileri veya yeni ürün tanımalıdır bir envanter uygulama adlarını tanımalıdır bir toplantı Zamanlayıcı gibi yeni örnekleri tanıyabilirsiniz istediğinizde kullanın. Ardından, başka bir deyişle varlığa benzer Bul LUIS yardımcı olan bir ifade listesini sözcük ve tümcecikleri oluşturun. Bu liste değeri olarak sözcükleri ek anlam ekleyerek varlık örnekleri tanımak için LUIS size yol gösterir. 

Hedefleri ve varlıkları anlama kalitesini geliştirme ile yardımcı etki alanına özel sözlük ifade listeleri gibidir. İfade listesinin ortak kullanım Şehir adları gibi uygun isimleri ' dir. Bir şehir adı kısa çizgi veya kesme işaretleri dahil olmak üzere birkaç sözcük olabilir.
 
### <a name="dont-use-a-phrase-list"></a>Bir ifade listesi kullanma 
Bir liste varlık varlığın alabilir ve yalnızca tam olarak aynı değerleri tanımlar. her bir değeri açıkça tanımlar. Bir liste varlık varlığın tüm örneklerini'nın bilinen ve genellikle değişmez bir uygulama için uygun olabilir. Seyrek değişen bir restoran menüsü Gıda öğelerde verilebilir. Bir varlığın bir tam metin eşleşmesi gerekiyorsa, ifade listesi kullanmayın. 

## <a name="best-practices"></a>En iyi uygulamalar
Bilgi [en iyi uygulamalar](luis-concept-best-practices.md).

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [Özellik Ekle](luis-how-to-add-features.md) LUIS uygulamanızı özellikleri ekleme hakkında daha fazla bilgi için.
