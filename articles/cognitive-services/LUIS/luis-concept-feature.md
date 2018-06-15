---
title: Azure HALUK uygulamalardaki özellikleri anlama | Microsoft Docs
description: HALUK uygulamanın performansını iyileştirmeye yardımcı olmak özellikleri hakkında bilgi edinin. Tümcecik listeleri ve normal ifadeleri tanıma desenlerini özellikleri içerir.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 04/18/2018
ms.author: v-geberr
ms.openlocfilehash: 416fadaf52d7a71dcaab81aab3bf6099213e5af5
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35356378"
---
# <a name="phrase-list-features-in-luis"></a>HALUK tümcecik listesi özellikleri

Machine learning'de bir *özelliği* ayırt bir ayırdedici nitelik ya da sisteminizi gözlemleyen veri özniteliği. 

Etiket veya sınıflandırmak istediğiniz giriş tanımak nasıl konusunda ipuçları sağlamak için bir dil modeli özellikleri ekleyin. Hedefleri ve varlıkları tanıması HALUK Yardım özellikleri ancak özellikleri hedefleri veya varlıklar kendilerini değildir. Bunun yerine, özellikler, ilgili koşulları örnekleri sağlayabilir.  

## <a name="what-is-a-phrase-list-feature"></a>Tümcecik liste özelliğini nedir?
Tümcecik listesi bir grup değerlerin aynı sınıfa ait olan ve benzer şekilde (örneğin, şehirler veya ürün adlarını) değerlendirilmesi gerekir (kelimeler ve ifadeler) içerir. HALUK bunlardan birini hakkında ne öğrenir otomatik olarak başkalarına da uygulanır. Bu kapalı değil [listesi varlık](luis-concept-entity-types.md#types-of-entities) (tam metin eşleşir) eşleşen sözcüklerin.

Tümcecik listesi uygulama etki alanı sözlük için ikinci bir HALUK işaret sözcükleri ilgili olarak ekler.

## <a name="how-to-use-phrase-lists"></a>Tümcecik listeleri kullanma
Seyahat Aracısı uygulamada, Londra, Paris ve Kahire değerlerini içeren "Şehir" adlı bir deyim listesini oluşturun. Basit bir varlık gibi bu değerlerden birini etiket varsa bir [örnek utterance](luis-how-to-add-example-utterances.md#add-simple-entity-label) bir amacı, diğerleri tanımak HALUK öğrenir. 

Tümcecik listesi değiştirilebilir veya birbirinin yerine olmayan olabilir. Bir *birbirinin yerine* tümcecik listesidir için eş anlamlı sözcükleri, değerler ve *birbirinin yerine olmayan* tümcecik listesi eş anlamlıları değil, ancak başka bir yolla benzer olmayan değerler için tasarlanmıştır. 

## <a name="phrase-lists-help-identify-simple-exchangeable-entities"></a>Tümcecik listeler Yardım basit değiştirilebilir varlıklar tanımlayın
Değiştirilebilir tümcecik listeleri HALUK uygulamanızın performansı ayarlamak için iyi bir yoldur. Uygulamanız doğru hedefi utterances tahmin etmeye veya varlıklar algılamayı sorun varsa, utterances olağan dışı sözcükleri ya da anlamları belirsiz olabilir sözcükler içeren hakkında düşünün. Bu sözcükleri tümcecik listesini dahil etmek için iyi bir aday değildir.

## <a name="phrase-lists-help-identify-intents-by-better-understanding-context"></a>Tümcecik listeler Yardım daha iyi anlamak bağlamdan hedefleri tanımlayın
Tümcecik listeleri nadir, mülkiyet ve yabancı sözcüklerini kullanın. HALUK (dışında uygulama kültür) yabancı sözcüklerin yanı sıra nadir ve özel sözcükleri tanıyamazsa olabilir ve bu nedenle bunlar tümcecik listesine eklenmesi gerekir. Bu tümcecik listesi olmayan-birbirinin yerine, HALUK tanımak için öğrenmelisiniz bir sınıf nadir sözcükler kümesini oluşturur, ancak bunlar eş anlamlıları değildir belirtmek için veya birbirleri ile karşılıklı olarak işaretlenmelidir.

Tümcecik listesi bir yönerge katı eşleştirme işlemi yapmak veya her zaman tümcecik listesindeki tüm koşulları tam olarak aynı etiket HALUK için değil. Yalnızca bir ipucu olur. Örneğin, "Patti" ve "Selma" adların olup olmadığını belirten bir tümcecik listesi olabilir, ancak HALUK bağlamsal bilgi "2 için bir ayırma Patti'nın Diner için yemeği Make" ve "beş bana yürüten farklı bir şey anlamına olduğunu bilmek kullanmaya devam edebilirsiniz yönlendirmeler Selma, Georgia". 

Tümcecik listesi ekleme, daha fazla örnek utterances bir hedefi eklemek için bir alternatiftir. 

## <a name="when-to-use-phrase-lists-versus-list-entities"></a>Tümcecik kullanmak ne zaman listesi varlıklar karşı listeler
Tümcecik liste ve liste varlıkları utterances tüm hedefleri etkileyebilir, ancak her bunu farklı bir şekilde gerçekleştirir. Hedefi tahmin puanlarını etkilemek için bir ifade listesini kullanın. Bir tam metin eşleşme varlık ayıklama etkilemek için bir liste varlık kullanın. 

### <a name="use-a-phrase-list"></a>Bir ifade listesini kullanın
Tümcecik listesi ile HALUK hala bağlam dikkate alın ve benzer öğeler, ancak tam bir eşleşme değil, listedeki öğeleri tanımlamak için genelleştirin. Generalize ve yeni bir kategori öğeleri tanımlamak için HALUK uygulamanız gerekiyorsa, tümcecik listesini kullanın. 

Bir varlığın yeni kişiler veya yeni ürünlerin tanımalıdır bir stok uygulama adlarını tanımalıdır toplantı Zamanlayıcı gibi yeni örnekleri tanımasına istediğinizde gibi basit bir makine öğrenilen varlığın başka bir türü kullanın veya hiyerarşik varlık. Sözcük, tümcecik listesini oluşturun ve diğer sözcükleri HALUK yardımcı tümcecikleri varlık benzer bulun. Bu liste ek anlamlı sözcükleri değerine ekleyerek varlık örnekleri tanımak için HALUK size rehberlik eder. 

Tümcecik listeleri hedefleri ve varlıkları anlayış kalitesini arttırma ile Yardım etki alanına özgü sözlük gibidir. Bir ortak tümcecik listesini Şehir adları gibi uygun isimleri kullanımdır. Şehir adı kısa çizgi veya kesme dahil olmak üzere birkaç sözcükler olabilir.
 
### <a name="dont-use-a-phrase-list"></a>Tümcecik listesi kullanma 
Bir liste varlık açıkça bir varlık alabilir ve yalnızca tam olarak eşleşen değerleri tanımlar. her değer tanımlar. Bir liste varlık bir varlığın tüm örneklerini bilinir ve sık sık değişmeyen bir restoran menüsündeki yemek öğeleri gibi değişmez bir uygulama için uygun olabilir. Bir varlık olarak bir tam metin eşleşen gerekiyorsa, tümcecik listesi kullanmayın. 

## <a name="best-practices"></a>En iyi uygulamalar
Bilgi [en iyi uygulamalar](luis-concept-best-practices.md).

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [Özellik Ekle](luis-how-to-add-features.md) HALUK uygulamanıza Özellikler ekleme hakkında daha fazla bilgi için.