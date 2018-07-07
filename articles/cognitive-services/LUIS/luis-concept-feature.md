---
title: Azure'da LUIS uygulama özelliklerini anlama | Microsoft Docs
description: Bir LUIS uygulamanın performansını iyileştirmeye yardımcı olan özellikler hakkında bilgi edinin. İfade listeleri ve düzenleri için normal ifadeleri tanıma özellikleri içerir.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 04/18/2018
ms.author: v-geberr
ms.openlocfilehash: 597948947303b7fdf16f24576620d6f39d7c51f4
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37887455"
---
# <a name="phrase-list-features-in-luis"></a>LUIS, ifade listesi özellikleri

Machine learning'de bir *özellik* bir ayırt edici nitelik veya sisteminizin gözlemler veri özniteliği. 

Bir dil modeli, ipuçlarını, etiket veya sınıflandırmak istediğiniz giriş anlamayı hakkında sağlamaya özellikleri ekleyin. Özellikleri, hedefleri ve varlıkları hem LUIS yardımcı olur, ancak özellikleri, hedefleri ve varlıkları kendilerini değildir. Bunun yerine, özellikler, ilgili koşulları örnekleri sağlayabilir.  

## <a name="what-is-a-phrase-list-feature"></a>Bir ifade listesi özelliği nedir?
Bir ifade listesi, aynı sınıfa ait benzer şekilde (örneğin, şehirler ya da ürün adlarını) işlenmesi gereken değerleri (sözcük ve tümcecikleri) bir grup içerir. LUIS bunları biri hakkında ne öğrenir otomatik olarak başkaları için de uygulanır. Kapalı değil [varlık listesinde](luis-concept-entity-types.md#types-of-entities) (tam metin eşleşmesini) eşleşen bir kelimelerin.

Bir ifade listesi uygulama etki alanının sözlüğü LUIS için ikinci bir sinyal sözcükleri ilgili olarak ekler.

## <a name="how-to-use-phrase-lists"></a>İfade listeleri kullanma
Bir seyahat aracı uygulaması, Londra, Paris ve Cairo değerlerini içeren "Şehir" adlı bir ifade listesini oluşturun. Basit bir varlık olarak şu değerlerden biri olarak etiket varsa bir [örnek utterance](luis-how-to-add-example-utterances.md#add-simple-entity-label) bir amaca LUIS diğerleri öğrenir. 

Değiştirilebilir veya değiştirilebilir olmayan bir ifade listesi olabilir. Bir *birbirinin yerine* tümcecik listedir eş anlamlılar için değerler ve *değiştirilebilir olmayan* tümcecik listesi, eş anlamlılar bulunmayan, ancak başka bir yolla benzer değerler için tasarlanmıştır. 

## <a name="phrase-lists-help-identify-simple-exchangeable-entities"></a>Tümcecik listeler Yardım basit değiştirilebilir varlıkları tanımlama
Değiştirilebilir ifade listeleri LUIS uygulamanızın performansını ayarlamak için iyi bir yoludur. Uygulamanızın doğru amaç konuşma tahmin etme veya varlık tanıma sorun varsa, konuşma olağan dışı bir sözcük veya anlamları belirsiz olabilir sözcükler içeren hakkında düşünün. Bu sözcükler tümcecik listesinde içermek için iyi adaylar değildir.

## <a name="phrase-lists-help-identify-intents-by-better-understanding-context"></a>Tümcecik listeler Yardım hedefleri daha iyi anlama bağlamdan tanımlayın
İfade listeleri için nadir, özel ve yabancı kelimeler kullanın. LUIS yabancı kelimeler (dışında uygulamanın kültür) yanı sıra, nadir ve özel sözcük tanıyamadı olabilir ve bu nedenle bunlar bir ifade listesine eklenmesi gereken. Bu ifade listesi, olmayan-birbirinin yerine, nadir bir sözcükler kümesini LUIS tanımayı öğrenin bir sınıf oluşturur, ancak bunlar eş anlamlı sözcükler değildir belirtmek için veya birbirinin yerine birbirleri ile işaretlenmelidir.

Bir ifade listesi LUIS sıkı eşleştirme işlemi yapmak veya her zaman deyim listesindeki tüm koşulları tamamen aynı etiket için bir yönerge değil. Bu, yalnızca bir ipucu verir. Örneğin, "Patti" ve "Selma" adların olup olmadığını belirten bir ifade listesi olabilir, ancak LUIS kullanmaya devam edebilirsiniz bağlamsal bilgiler farklı bir şey anlamına olduğunu bilmek "2 için bir ayırma sırasında Patti'nın Diner Akşam olun" ve "bana sürüş bulma yönlendirmeler Selma, Gürcistan". 

Bir ifade listesine eklenmesi, daha fazla örnek konuşma eklemeye yönelik bir amacı bir alternatiftir. 

## <a name="when-to-use-phrase-lists-versus-list-entities"></a>Ne zaman tümceciğini kullanın varlıklar listesi listeler
Bir ifade listesi hem liste varlıklar arasında tüm hedefleri konuşma etkileyebilir, ancak her bunu farklı bir şekilde yapar. Hedefi tahmin puanı etkilemek için bir ifade listesini kullanın. Bir liste varlığı, bir tam metin eşleşmesi için varlık ayıklama etkilemek için kullanın. 

### <a name="use-a-phrase-list"></a>Bir ifade listesini kullanın
Bir ifade listesiyle LUIS hala bağlam dikkate alın ve benzer öğeleri, ancak tam bir eşleşme yok, listedeki öğeleri tanımlamak için generalize. LUIS uygulamanızı generalize ve bir kategorideki yeni öğeleri tanımlamak için gerekiyorsa, bir ifade listesini kullanın. 

Bir varlıktaki yeni kişileri veya yeni ürün tanımalıdır bir envanter uygulama adlarını tanımalıdır bir toplantı Zamanlayıcı yeni örneklerini tanıyabilirsiniz istediğinizde başka türde bir makine öğrenilen varlık gibi basit bir kullanın veya hiyerarşik varlık. Ardından, başka bir deyişle varlığa benzer Bul LUIS yardımcı olan bir ifade listesini sözcük ve tümcecikleri oluşturun. Bu liste değeri olarak sözcükleri ek anlam ekleyerek varlık örnekleri tanımak için LUIS size yol gösterir. 

Hedefleri ve varlıkları anlama kalitesini geliştirme ile yardımcı etki alanına özel sözlük ifade listeleri gibidir. İfade listesinin ortak kullanım Şehir adları gibi uygun isimleri ' dir. Bir şehir adı kısa çizgi veya kesme işaretleri dahil olmak üzere birkaç sözcük olabilir.
 
### <a name="dont-use-a-phrase-list"></a>Bir ifade listesi kullanma 
Bir liste varlık varlığın alabilir ve yalnızca tam olarak aynı değerleri tanımlar. her bir değeri açıkça tanımlar. Bir liste varlık varlığın tüm örneklerini bilinen ve sık sık, seyrek değişen bir restoran menüsü Gıda öğeleri gibi değişmez bir uygulama için uygun olabilir. Bir varlığın bir tam metin eşleşmesi gerekiyorsa, ifade listesi kullanmayın. 

## <a name="best-practices"></a>En iyi uygulamalar
Bilgi [en iyi uygulamalar](luis-concept-best-practices.md).

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [Özellik Ekle](luis-how-to-add-features.md) LUIS uygulamanızı özellikleri ekleme hakkında daha fazla bilgi için.
