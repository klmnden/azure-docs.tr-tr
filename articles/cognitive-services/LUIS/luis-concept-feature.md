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
ms.openlocfilehash: b82d5261bbe9d9b153be1cb6e1ff1ba61803c8c2
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37928939"
---
# <a name="phrase-list-features-in-luis"></a>LUIS, ifade listesi özellikleri

Machine learning'de bir *özellik* bir ayırt edici nitelik veya sisteminizin gözlemler veri özniteliği. 

Bir dil modeli, ipuçlarını, etiket veya sınıflandırmak istediğiniz giriş anlamayı hakkında sağlamaya özellikleri ekleyin. Özellikleri, hedefleri ve varlıkları hem LUIS yardımcı olur, ancak özellikleri, hedefleri ve varlıkları kendilerini değildir. Bunun yerine, özellikler, ilgili koşulları örnekleri sağlayabilir.  

## <a name="what-is-a-phrase-list-feature"></a>Bir ifade listesi özelliği nedir?
Bir ifade listesi, aynı sınıfa ait benzer şekilde (örneğin, şehirler ya da ürün adlarını) işlenmesi gereken değerleri (sözcük ve tümcecikleri) bir grup içerir. LUIS bunları biri hakkında ne öğrenir otomatik olarak başkaları için de uygulanır. Bu liste kapalı değil [varlık listesinde](luis-concept-entity-types.md#types-of-entities) (tam metin eşleşmesini) eşleşen bir kelimelerin.

Bir ifade listesi uygulama etki alanının sözlüğü LUIS için ikinci bir sinyal sözcükleri ilgili olarak ekler.

## <a name="how-to-use-phrase-lists"></a>İfade listeleri kullanma
İnsan Kaynakları uygulamasının [varlığın öğretici](luis-quickstart-primary-and-secondary-data.md), uygulamanın kullandığı bir **iş** Programcı roofer ve dış gibi iş türleri ifade listesi. Şu değerlerden biri olarak makine öğrenilen bir varlık olarak etiketlerseniz LUIS diğer öğrenir. 

Değiştirilebilir veya değiştirilebilir olmayan bir ifade listesi olabilir. Bir *birbirinin yerine* tümcecik listedir eş anlamlılar için değerler ve *değiştirilebilir olmayan* tümcecik listesi uygulamasında ek bir sinyal eş anlamlı sözcükler, ancak hala olmayan değerlere ihtiyacınız için tasarlanmıştır. 

<a name="phrase-lists-help-identify-simple-exchangeable-entities"></a>
## <a name="phrase-lists-help-identify-simple-interchangeable-entities"></a>Tümcecik listeler Yardım basit birbirinin yerine varlıkları tanımlama
Birbirinin yerine ifade listeleri LUIS uygulamanızın performansını ayarlamak için iyi bir yoludur. Uygulamanızın doğru amaç konuşma tahmin etme veya varlık tanıma sorun varsa, konuşma olağan dışı bir sözcük veya anlamları belirsiz olabilir sözcükler içeren hakkında düşünün. Bu sözcükler tümcecik listesinde içermek için iyi adaylar değildir.

## <a name="phrase-lists-help-identify-intents-by-better-understanding-context"></a>Tümcecik listeler Yardım hedefleri daha iyi anlama bağlamdan tanımlayın
Bir ifade listesi LUIS sıkı eşleştirme işlemi yapmak veya her zaman deyim listesindeki tüm koşulları tamamen aynı etiket için bir yönerge değil. Bu, yalnızca bir ipucu verir. Örneğin, "Patti" ve "Selma" adların olup olmadığını belirten bir ifade listesi olabilir, ancak LUIS kullanmaya devam edebilirsiniz bağlamsal bilgiler farklı bir şey anlamına olduğunu bilmek "2 için bir ayırma sırasında Patti'nın Diner Akşam olun" ve "bana sürüş bulma yönlendirmeler Selma, Gürcistan". 

Bir ifade listesine eklenmesi, daha fazla örnek konuşma eklemeye yönelik bir amacı bir alternatiftir. 

## <a name="an-interchangeable-phrase-list"></a>Birbirinin yerine ifade listesi
Sözcükleri veya aşamaları listesini bir sınıf veya grup oluşturduğunuzda bir birbirinin yerine bir ifade listesini kullanın. Örnek bir ay "Ocak", "Şubat", "Mart"; listesidir. veya, adları "John", "Gamze", "Ferdi" ister.  İfade listesinden farklı bir sözcük kullanıldıysa utterance aynı hedefi veya varlık etiketlenmesi, bu listeleri birbirinin yerine kullanılabilir. Örneğin, "Takvim Ocak ayında aynı Göster" "Şubat Takvimi Göster", ardından sözcükleri birbirinin yerine bir listede olması gerektiği gibi hedefi. 

## <a name="a-non-interchangeable-phrase-list"></a>Değiştirilebilir olmayan bir ifade listesi
Değiştirilebilir olmayan bir ifade listesini eşanlamlı olmayan sözcükler veya etki alanınızda gruplandırılabilir ifadeler kullanın. 

Bir örnek olarak, değiştirilebilir olmayan bir ifade listesini için nadir, özel ve yabancı kelimeler kullanın. LUIS yabancı kelimeler (dışında uygulamanın kültür) yanı sıra, nadir ve özel sözcük tanıyamadı olabilir. Nadir bir sözcükler kümesini LUIS tanımayı öğrenin bir sınıf oluşturur, ancak eş anlamlılar olmayan değiştirilebilir olmayan ayarını gösterir veya birbirleri ile değiştirilebilir.

## <a name="when-to-use-phrase-lists-versus-list-entities"></a>Ne zaman tümceciğini kullanın varlıklar listesi listeler
Bir ifade listesi hem liste varlıklar arasında tüm hedefleri konuşma etkileyebilir, ancak her bunu farklı bir şekilde yapar. Hedefi tahmin puanı etkilemek için bir ifade listesini kullanın. Bir liste varlığı, bir tam metin eşleşmesi için varlık ayıklama etkilemek için kullanın. 

### <a name="use-a-phrase-list"></a>Bir ifade listesini kullanın
Bir ifade listesiyle LUIS hala bağlam dikkate alın ve benzer öğeleri, ancak tam bir eşleşme yok, listedeki öğeleri tanımlamak için generalize. LUIS uygulamanızı generalize ve bir kategorideki yeni öğeleri tanımlamak için gerekiyorsa, bir ifade listesini kullanın. 

Bir varlıktaki yeni kişileri veya yeni ürün tanımalıdır bir envanter uygulama adlarını tanımalıdır bir toplantı Zamanlayıcı yeni örneklerini tanıyabilirsiniz istediğinizde başka türde bir makine öğrenilen varlık gibi basit bir kullanın veya hiyerarşik varlık. Ardından, başka bir deyişle varlığa benzer Bul LUIS yardımcı olan bir ifade listesini sözcük ve tümcecikleri oluşturun. Bu liste değeri olarak sözcükleri ek anlam ekleyerek varlık örnekleri tanımak için LUIS size yol gösterir. 

Hedefleri ve varlıkları anlama kalitesini geliştirme ile yardımcı etki alanına özel sözlük ifade listeleri gibidir. İfade listesinin ortak kullanım Şehir adları gibi uygun isimleri ' dir. Bir şehir adı kısa çizgi veya kesme işaretleri dahil olmak üzere birkaç sözcük olabilir.
 
### <a name="dont-use-a-phrase-list"></a>Bir ifade listesi kullanma 
Bir liste varlık varlığın alabilir ve yalnızca tam olarak aynı değerleri tanımlar. her bir değeri açıkça tanımlar. Bir liste varlık varlığın tüm örneklerini'nın bilinen ve genellikle değişmez bir uygulama için uygun olabilir. Seyrek değişen bir restoran menüsü Gıda öğelerde verilebilir. Bir varlığın bir tam metin eşleşmesi gerekiyorsa, ifade listesi kullanmayın. 

## <a name="best-practices"></a>En iyi uygulamalar
Bilgi [en iyi uygulamalar](luis-concept-best-practices.md).

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [Özellik Ekle](luis-how-to-add-features.md) LUIS uygulamanızı özellikleri ekleme hakkında daha fazla bilgi için.
