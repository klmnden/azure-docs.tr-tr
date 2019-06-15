---
title: Language Understanding (LUIS) - Azure Bilişsel Hizmetler | Microsoft Docs
description: Language Understanding (LUIS), genel anlamı tahmin etmek ve ilgili, ayrıntılı bilgileri çekme amacıyla kullanıcının konuşmasına, doğal dil metnine özel makine öğrenimi zekası uygulayan bulut tabanlı API hizmetidir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: overview
ms.date: 06/11/2019
ms.author: diberry
ms.openlocfilehash: 569b33d299f52f0da50d8a8992420754aa85b533
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67062158"
---
# <a name="what-is-language-understanding-luis"></a>Language Understanding (LUIS) nedir?

Language Understanding (LUIS), genel anlamı tahmin etmek ve ilgili, ayrıntılı bilgileri çekme amacıyla kullanıcının konuşmasına, doğal dil metnine özel makine öğrenimi zekası uygulayan bulut tabanlı API hizmetidir. 

LUIS için istemci uygulaması, bir görevi tamamlamak için kullanıcıyla doğal dil kullanarak iletişim kuran konuşma uygulamasıdır. İstemci uygulamalarına örnek olarak sosyal medya uygulamaları, sohbet botları ve konuşma özellikli masaüstü uygulamaları verilebilir.  

![Bilişsel Hizmetler Language Understanding (LUIS) ile çalışan 3 istemci uygulamasının kavramsal görüntüsü](./media/luis-overview/luis-entry-point.png "Bilişsel Hizmetler Language Understanding (LUIS) ile çalışan 3 istemci uygulamasının kavramsal görüntüsü")

## <a name="use-luis-in-a-chat-bot"></a>Sohbet botunda LUIS kullanımı

<a name="Accessing-LUIS"></a>

LUIS uygulaması yayımlandıktan sonra istemci uygulaması konuşmaları (metni) LUIS doğal dil işleme uç noktası [API'sine][endpoint-apis] gönderir ve sonuçları JSON yanıtı olarak alır. Sık kullanılan LUIS istemci uygulamalarından biri, sohbet botudur.


![Doğal dil işleme (NLP) ile kullanıcı metnini tahmin etmek için Sohbet botuyla birlikte çalışan LUIS hizmetinin kavramsal görüntüsü](./media/luis-overview/luis-overview-process-2.png "Doğal dil işleme (NLP) ile kullanıcı metnini tahmin etmek için Sohbet botuyla birlikte çalışan LUIS hizmetinin kavramsal görüntüsü")

|Adım|Eylem|
|:--|:--|
|1|İstemci uygulaması, kullanıcının "İK temsilcimi aramak istiyorum." şeklindeki _konuşmasını_ (kendi kullandıkları kelimelerle) bir HTTP isteği olarak LUIS uç noktasına gönderir.|
|2|LUIS, kullanıcı girişi hakkında zeka anlayışı sunmak için öğrenilen modeli doğal dil metnine uygular. LUIS, "HRContact" üst amacına sahip JSON biçiminde bir yanıt döndürür. JSON uç nokta yanıtı minimumda sorgu konuşmasını ve en yüksek puanlı amacı içerir. Ayrıca Kişi Türü varlığı gibi verileri de ayıklayabilir.|
|3|İstemci uygulaması, JSON yanıtını kullanarak kullanıcının isteklerini gerçekleştirmeyle ilgili kararları verir. Bu kararlar bot çerçeve kodunda karar ağacı ve diğer hizmetlere çağrı içerebilir. |

LUIS uygulaması, istemci uygulamasının akıllı seçimler yapabilmesi için gerekli bilgileri sunar. LUIS bu seçenekleri sağlamaz. 

<a name="Key-LUIS-concepts"></a>
<a name="what-is-a-luis-model"></a>

## <a name="natural-language-processing"></a>Doğal dil işleme

LUIS uygulaması, etki alanına özgü doğal dil modeli içerir. LUIS uygulamasını önceden oluşturulmuş bir etki alanı modeliyle başlatabilir, kendi modelinizi oluşturabilir veya önceden oluşturulmuş etki alanının belirli bölümlerini kendi özel bilgilerinizle karıştırabilirsiniz.

* **Önceden oluşturulmuş model**: LUIS amaç, konuşma ve önceden oluşturulmuş varlık içeren birçok önceden oluşturulmuş etki alanı modeline sahiptir. Önceden oluşturulmuş modelin amaçlarını ve konuşmalarını kullanmak zorunda kalmadan önceden oluşturulmuş varlıkları kullanabilirsiniz. [Önceden oluşturulmuş etki alanı modelleri](luis-how-to-use-prebuilt-domains.md), tasarımın tamamını içerir ve LUIS hizmetini kullanmaya başlamak için ideal bir yoldur.

* **Özel Varlıklar**: LUIS makine öğrenimi varlıkları, özellik veya değişmez varlıklar ve makine öğrenimi ve değişmez varlıkların birleşimi dahil olmak üzere kendi özel amaçlarınızı ve varlıklarınızı tanımlamak için birkaç farklı yöntem sunar.

## <a name="build-the-luis-model"></a>LUIS modelini derleme
Modeli [yazma](https://go.microsoft.com/fwlink/?linkid=2092087) API'leri veya LUIS portalı ile derleyebilirsiniz.

LUIS modeli, **[amaçlar](luis-concept-intent.md)** olarak adlandırılan kullanıcı amacı kategorileriyle başlar. Her amaç için kullanıcı **[konuşmaları](luis-concept-utterance.md)** örneklerine ihtiyaç duyulur. Her bir konuşma, **[varlıklarla](luis-concept-entity-types.md)** ayıklanabilecek çeşitli veriler sunabilir. 

|Örnek kullanıcı konuşması|Amaç|Varlıklar|
|-----------|-----------|-----------|
|"__Seattle__ için uçak bileti al"|BookFlight|Seattle|
|"Mağazanız saat kaçta __açılıyor__?"|StoreHoursAndLocation|açık|
|"Dağıtım bölümünde saat __13__'te __Bob__ ile toplantı planla"|ScheduleMeeting|13, Bob|

## <a name="query-prediction-endpoint"></a>Sorgu tahmin uç noktası

Model derlendikten ve uç noktada yayımlandıktan sonra istemci uygulaması yayımlanan tahmin [uç noktası](https://go.microsoft.com/fwlink/?linkid=2092356) API'sine konuşma gönderir. API, analiz için modeli metne uygular. API, JSON biçiminde tahmin sonuçlarıyla yanıt verir.  

JSON uç nokta yanıtı minimumda sorgu konuşmasını ve en yüksek puanlı amacı içerir. Ayrıca aşağıdaki **Kişi Türü** varlığı gibi verileri de ayıklayabilir. 

```JSON
{
  "query": "I want to call my HR rep.",
  "topScoringIntent": {
    "intent": "HRContact",
    "score": 0.921233
  },
  "entities": [
    {
      "entity": "call",
      "type": "Contact Type",
      "startIndex": 10,
      "endIndex": 13,
      "score": 0.7615982
    }
  ]
}
```

## <a name="improve-model-prediction"></a>Model tahminini geliştirme

Bir LUIS modeli yayımlandıktan ve gerçek kullanıcı konuşmalarını aldıktan sonra LUIS tarafından tahmin doğruluğunu artırmak için birden çok yöntem sunulur: uç nokta konuşmaları için [etkin öğrenme](luis-concept-review-endpoint-utterances.md), etki alanı kelimelerini dahil etmek için [tümcecik listeleri](luis-concept-feature.md) ve gerekli konuşma sayısını azaltmak için [desenler](luis-concept-patterns.md).

<a name="using-luis"></a>

## <a name="development-lifecycle"></a>Geliştirme yaşam döngüsü
LUIS, istemci uygulaması ve dil modeli düzeyinde tam geliştirme yaşam döngüsüyle tümleştirmek için kullanılabilecek araçlar, sürüm oluşturma özellikleri ve diğer LUIS yazarlarıyla işbirliği özellikleri sunmaktadır. 

## <a name="implementing-luis"></a>LUIS uygulama
LUIS, HTTP isteği gönderen tüm ürün, hizmet veya çerçevelerle REST API olarak kullanılabilir. Aşağıdaki liste, LUIS ile birlikte en çok kullanılan Microsoft ürünlerini ve hizmetlerini göstermektedir.

En çok kullanılan LUIS istemci uygulamaları şunlardır:
* [Web uygulaması botu](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0), kullanıcıyla metin girişi aracılığıyla konuşmak için hızlıca LUIS destekli bir sohbet botu oluşturur. Kullanan [Bot Framework] [ bot-framework] sürüm [4.x](https://github.com/Microsoft/botbuilder-dotnet) eksiksiz bir bot bir deneyim.

LUIS'i hızlı ve kolay bir şekilde botla birlikte kullanmanızı sağlayacak uygulamalar:
* [LUIS CLI](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/LUIS) NPM paketi, yazma ve içeri aktarma olarak veya bir tek başına komut satırı aracı ile tahmini sağlar. 
* [LUISGen](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/LUISGen) LUISGen, dışarı aktarılan bir LUIS modelinden ayrıntılı C# ve typescript kaynak kodu yazmak için kullanılan bir araçtır.
* [Gönderme](https://aka.ms/dispatch-tool), çeşitli LUIS ve Soru-Cevap Oluşturma uygulamalarının gönderme modelini kullanan bir üst uygulamadan kullanılmasını sağlar.
* [LUDown](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/Ludown) LUDown, botunuz için dil modellerini yönetmenize yardımcı olan bir komut satırı aracıdır.

LUIS ile kullanılan diğer Bilişsel Hizmetler:
* [Soru-Cevap Oluşturma][qnamaker], farklı metin türlerinin birleştirilerek soru ve yanıt bilgi bankası oluşturulmasını sağlar.
* [Bing Yazım Denetimi API'si](../bing-spell-check/proof-text.md), tahmin işlemi öncesinde metinlerin düzeltilmesini sağlar. 
* [Konuşma hizmeti](../Speech-Service/overview.md), sözlü dil isteklerini metne dönüştürür. 
* [Konuşma öğrenici](https://docs.microsoft.com/azure/cognitive-services/labs/conversation-learner/overview), LUIS ile daha hızlı bir şekilde sohbet botları oluşturmanızı sağlar.
* [Kişilik sohbeti projesi](https://docs.microsoft.com/azure/cognitive-services/project-personality-chat/overview), bot ile kısa sohbetler yapmanızı sağlar.

LUIS kullanan örnekleri:
* [Konuşma özellikli yapay ZEKA](https://github.com/Microsoft/AI) GitHub deposu.
* [Language Understanding'i](https://github.com/Azure-Samples/cognitive-services-language-understanding) Azure örnekleri

## <a name="next-steps"></a>Sonraki adımlar

[Önceden oluşturulmuş](luis-get-started-create-app.md) veya [özel](luis-quickstart-intents-only.md) etki alanıyla yeni bir LUIS uygulaması yazma. Genel IoT uygulamasının [tahmin uç noktasını sorgulama](luis-get-started-cs-get-intent.md).

[bot-framework]: https://docs.microsoft.com/bot-framework/
[flow]: https://docs.microsoft.com/connectors/luis/
[authoring-apis]: https://go.microsoft.com/fwlink/?linkid=2092087
[endpoint-apis]: https://go.microsoft.com/fwlink/?linkid=2092356
[qnamaker]: https://qnamaker.ai/
