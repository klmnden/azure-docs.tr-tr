---
title: Azure HALUK uygulamalardaki anlama hedefleri | Microsoft Docs
description: Dil anlama akıllı hizmet (HALUK) uygulamalarında hedefleri nedir açıklar.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/04/2018
ms.author: v-geberr
ms.openlocfilehash: 5c2feb0240b676d4e106cbda65aaaed7604a35c5
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265161"
---
# <a name="intents-in-luis"></a>HALUK hedefleri

Eylem kullanıcı gerçekleştirmek istediği veya bir görev amacına temsil eder. Bir amaç veya hedef kullanıcının ifade edilen olduğundan [utterance](luis-concept-utterance.md).

Kullanıcıların uygulamanızda almak istediğiniz eylemler için karşılık gelen bir dizi hedefleri tanımlayın. Örneğin, seyahat uygulama birkaç hedefleri tanımlar:

Seyahat uygulama hedefleri   |   Örnek utterances   | 
------|------|
 BookFlight     |   "Bana uçuş RIO için sonraki hafta kitap" <br/> "Bana RIO için 24 üzerinde uçarak" <br/> "Uçak bileti RIO de Janeiro için sonraki Pazar ihtiyacım"    |
 Karşılama     |   "Merhaba" <br/>"Hello" <br/>"İyi sabah"  |
 CheckWeather | "Gibi hava Boston içinde nedir?" <br/> "Bu hafta için tahmini Göster" |
 None         | "Bana bir tanımlama bilgisi tarif Al"<br>"Lakers win?" |

Önceden tanımlanmış amacıyla, tüm uygulamaları gelen "[hiçbiri](#none-intent-is-fallback-for-app)" geri dönüş hedefi olduğu. 

## <a name="prebuilt-domains-provide-intents"></a>Önceden oluşturulmuş etki alanları hedefleri sağlar
Tanımladığınız hedefleri ek olarak, önceden oluşturulmuş hedefleri önceden oluşturulmuş etki alanlarından birini kullanabilirsiniz. Daha fazla bilgi için bkz: [HALUK uygulamaları önceden oluşturulmuş etki alanlarında kullanmak](luis-how-to-use-prebuilt-domains.md) amacı, uygulamanızda kullanmak için önceden oluşturulmuş bir etki alanından özelleştirme hakkında bilgi edinmek için.

## <a name="return-all-intents-scores"></a>Tüm amaçlar puanları Döndür
Tek bir hedefi için bir utterance atayın. HALUK bir utterance uç aldığında, bu utterance bir üst hedefini döndürür. Utterance için tüm hedefleri puanları isterseniz, sağlayabilir `verbose=true` bayrağı API sorgu dizesinde [endpoint çağrı](https://aka.ms/v1-endpoint-api-docs). 

## <a name="intent-compared-to-entity"></a>Varlığa karşılaştırıldığında hedefi
Hedefi chatbot kullanıcıdan sürer ve tüm utterance üzerinde temel eylemini temsil eder. Varlık kelimeler ve ifadeler utterance içinde yer alan temsil eder. Bir utterance hedefi Puanlama yalnızca bir üst olabilir ancak birçok varlıklar sahip olabilir. 

<a name="how-do-intents-relate-to-entities"></a> Amacına oluşturmak, kullanıcının _engellemekse_ checkweather() işlevi çağrısı gibi istemci uygulamanız bir eylemi tetikler. Ardından eylemi yürütmek için gerekli parametreleri temsil etmek için bir varlık oluşturun. 

|Örnek hedefi   | Varlık | Örnek utterances varlık   | 
|------------------|------------------------------|------------------------------|
| CheckWeather | {"tür": "Konum", "varlığı": "seattle"}<br>{"tür": "builtin.datetimeV2.date","entity": "yarın", "Çözümleme": "2018-05-23"} | Ne hava de olduğu gibi `Seattle` `tomorrow`? |
| CheckWeather | {"tür": "date_range", "varlığı": "Bu hafta sonu"} | Tahmin için Göster `this weekend` | 

## <a name="custom-intents"></a>Özel hedefleri

Benzer şekilde niyetli [utterances](luis-concept-utterance.md) yönelik tek amacı karşılık gelir. Maksadınızı utterances herhangi kullanabilirsiniz [varlık](luis-concept-entity-types.md) varlıklar hedefi özgü olmadığından uygulama. 

## <a name="prebuilt-domain-intents"></a>Önceden oluşturulmuş bir etki alanı hedefleri

[Önceden oluşturulmuş etki alanları](luis-how-to-use-prebuilt-domains.md) utterances ile amacı vardır.  

## <a name="none-intent-is-fallback-for-app"></a>Hedefi hiçbiri uygulaması için geri dönüş
**Hiçbiri** amacı olan bir catch tümü veya bir geri dönüş hedefi. Uygulama etki alanında (konu alanında) önemli olmayan utterances HALUK öğretmeyi kullanılır. **Hiçbiri** amacı, 10 ile uygulamadaki toplam utterances yüzde 20 arasında olmalıdır. Boş bırakmayın. 

### <a name="none-intent-helps-conversation-direction"></a>Hedefi hiçbiri konuşma yönü yardımcı olur
Bir utterance hiçbiri tahmin, hedefi ve bu tahmin ile chatbot döndürülen bot daha fazla soru sormak veya geçerli seçenekleri kullanıcıya chatbot yönlendirmek için bir menü sağlar. 

### <a name="no-utterances-in-none-intent-skews-predictions"></a>Hiçbir utterances hedefi hiçbiri Eğer Öngörüler
İçin tüm utterances eklemezseniz **hiçbiri** hedefi, HALUK etki alanına bir etki alanı hedefleri dışında bir utterance zorlar. Bu tahmin puanları utterance yanlış hedefini HALUK eğitme tarafından eğme. 

### <a name="add-utterances-to-the-none-intent"></a>Utterances Yok'a hedefi ekleyin
**Hiçbiri** hedefi oluşturulur, ancak amaç boş kaldı. Etki alanı dışındaki utterances ile doldurun. İçin iyi bir utterance **hiçbiri** tamamen uygulama endüstri yanı sıra uygulama dışında bir şey hizmet olduğu. Örneğin, seyahat uygulama için tüm utterances kullanmamalıdır **hiçbiri** faturalama, yemek, Restoran, kargo, ınflight eğlence ayırmaları gibi hareket etmek ilişkili. 

Ne tür bir utterances bırakılır için hiçbiri hedefi? Belirli bir şeyi ile bot böyle "ne tür bir dinosaur mavi gösteren var?" yanıt döndürmemelidir Başlat Seyahat uygulama dışında ne kadar çok özel bir soru budur. 

### <a name="none-is-a-required-intent"></a>Gerekli bir hedefi Yok'tur
**Hiçbiri** amacı gerekli amaç ve silinemez veya yeniden adlandırılamaz.

## <a name="negative-intentions"></a>Negatif amaçları 
Pozitif ve negatif amaçları gibi belirlemek istiyorsanız "ı **istediğiniz** bir araba" ve "ı **yok** bir araba istediğiniz", iki hedefleri (bir olumlu ve bir olumsuz) oluşturma ve için uygun utterances ekleyin Her. Veya tek bir hedefi oluşturup iki farklı pozitif ve negatif koşullarını bir varlık olarak işaretleyin.  

## <a name="intent-balance"></a>Hedefi Bakiye
Uygulama etki alanı hedefleri utterances dengesi her amacı arasında olması gerekir. 10 utterances ile bir hedefi ve 500 utterances ile başka bir amaca sahip değilsiniz. Bu dengeli değil. Bu durum varsa, birçok hedefleri içine yeniden düzenlenmiş, görmek için 500 utterances amacıyla gözden geçirin. bir [düzeni](luis-concept-patterns.md). 

**Hiçbiri** bakiyeye hedefi dahil değildir. Bu amaç için uygulama toplam utterances % 10 içermelidir.

## <a name="intent-limits"></a>Hedefi sınırları
Gözden geçirme [sınırları](luis-boundaries.md#model-boundaries) kaç hedefleri anlamak için bir modele ekleyebilirsiniz. 

### <a name="if-you-need-more-than-the-maximum-number-of-intents"></a>En fazla sayısını hedefleri gerekiyorsa 
İlk olarak, sisteminize çok fazla hedefleri kullanıp kullanmadığını göz önünde bulundurun. 

### <a name="can-multiple-intents-be-combined-into-single-intent-with-entities"></a>Birden çok hedefleri tek amacı olan varlık birleştirilebilir 
Çok benzer hedefleri bunları ayırt etmenize HALUK çok daha zor yapabilirsiniz. Hedefleri olması için kullanıcı isteyen, ancak kodunuzu alır her yolu yakalama gerekmez ana görevleri yakalamak için yeterli değişmesi. Örneğin, bir seyahat uygulamasında ayrı hedefleri BookFlight ve FlightCustomerService olabilir, ancak BookInternationalFlight ve BookDomesticFlight çok benzer. Sisteminiz bunları ayırt etmek gerekirse, varlık veya diğer mantığı yerine hedefleri kullanın. 

### <a name="dispatcher-model"></a>Dağıtıcı modeli
HALUK ve QnA maker uygulamalarla birleştirme hakkında daha fazla bilgi [gönderme modeli](luis-concept-enterprise.md#when-you-need-to-combine-several-luis-and-qna-maker-apps). 

### <a name="request-help-for-apps-with-significant-number-of-intents"></a>Hedefleri önemli sayıda olan uygulamalar için yardım iste
Hedefleri sayısını azaltmak ve birden fazla uygulama, hedefleri bölme sizin için işe yaramazsa, desteğe başvurun. Destek Hizmetleri Azure aboneliğinize içeriyorsa, kişi [Azure teknik destek](https://azure.microsoft.com/support/options/). 

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinmek [varlıklar](luis-concept-entity-types.md), önemli sözcükler amaçlar için ilgili olduğu
* Bilgi nasıl [ekleyin ve hedefleri yönetin](luis-how-to-add-intents.md) HALUK uygulamanızda.
* Gözden hedefi [en iyi uygulamalar](luis-concept-best-practices.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website