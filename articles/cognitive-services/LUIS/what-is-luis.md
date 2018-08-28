---
title: Language Understanding (LUIS) - Azure Bilişsel Hizmetler | Microsoft Docs
description: Language Understanding (LUIS), genel anlamı tahmin etmek ve ilgili, ayrıntılı bilgileri çekme amacıyla kullanıcının konuşmasına, doğal dil metnine özel makine öğrenimi zekası uygulayan bulut tabanlı API hizmetidir. LUIS için istemci uygulaması, bir görevi tamamlamak için kullanıcıyla doğal dil kullanarak iletişim kuran konuşma uygulamasıdır. İstemci uygulamalarına örnek olarak sosyal medya uygulamaları, sohbet botları ve konuşma özellikli masaüstü uygulamaları verilebilir.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: overview
ms.date: 08/15/2018
ms.author: diberry
ms.openlocfilehash: e74abb30709f186d3c1139793cf34d3e033ff967
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2018
ms.locfileid: "40191648"
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

<!--

### Example of JSON endpoint response

The minimum JSON endpoint response contains the query utterance, and the top scoring intent. It can also extract data such as the following **Contact Type** entity. 

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
-->

<a name="Key-LUIS-concepts"></a>
<a name="what-is-a-luis-model"></a>

## <a name="natural-language-processing"></a>Doğal dil işleme

LUIS uygulaması, etki alanına özgü doğal dil modeli içerir. LUIS uygulamasını önceden oluşturulmuş bir etki alanı modeliyle başlatabilir, kendi modelinizi oluşturabilir veya önceden oluşturulmuş etki alanının belirli bölümlerini kendi özel bilgilerinizle karıştırabilirsiniz.

* **Önceden oluşturulmuş model**: LUIS amaç, konuşma ve önceden oluşturulmuş varlık içeren birçok önceden oluşturulmuş etki alanı modeline sahiptir. Önceden oluşturulmuş modelin amaçlarını ve konuşmalarını kullanmak zorunda kalmadan önceden oluşturulmuş varlıkları kullanabilirsiniz. [Önceden oluşturulmuş etki alanı modelleri](luis-how-to-use-prebuilt-domains.md), tasarımın tamamını içerir ve LUIS hizmetini kullanmaya başlamak için ideal bir yoldur.

* **Özel Varlıklar**: LUIS makine öğrenimi varlıkları, özellik veya değişmez varlıklar ve makine öğrenimi ve değişmez varlıkların birleşimi dahil olmak üzere kendi özel amaçlarınızı ve varlıklarınızı tanımlamak için birkaç farklı yöntem sunar.

## <a name="build-the-luis-model"></a>LUIS modelini derleme
Modeli [yazma](https://aka.ms/luis-authoring-apis) API'leri veya LUIS portalı ile derleyebilirsiniz.

LUIS modeli, **[amaçlar](luis-concept-intent.md)** olarak adlandırılan kullanıcı amacı kategorileriyle başlar. Her amaç için kullanıcı **[konuşmaları](luis-concept-utterance.md)** örneklerine ihtiyaç duyulur. Her bir konuşma, **[varlıklarla](luis-concept-entity-types.md)** ayıklanabilecek çeşitli veriler sunabilir. 

|Örnek kullanıcı konuşması|Amaç|Varlıklar|
|-----------|-----------|-----------|
|"__Seattle__ için uçak bileti al"|BookFlight|Seattle|
|"Mağazanız saat kaçta __açılıyor__?"|StoreHoursAndLocation|açık|
|"Dağıtım bölümünde saat __13__'te __Bob__ ile toplantı planla"|ScheduleMeeting|13, Bob|

<!--
## What is a natural language model?

A model begins with a list of general user intentions, called _intents_, such as "Book Flight" or "Contact Help Desk." You provide user's example text, called _example utterances_ for the intents. Then mark significant words or phrases in the utterance, called _entities_.


A model includes:

* **[intents](#intents)**: categories of user intentions (overall intended action or result)
* **[entities](#entities)**: specific types of data in utterances such as number, email, or name contained in text
* **[example utterances](#example-utterances)**: example text a user might enter in the client application

### Intents 

An [intent](luis-how-to-add-intents.md), short for _intention_, is a purpose or goal expressed in a user's utterance, such as booking a flight, paying a bill, or finding a news article. You create an intent for each action. A LUIS travel app may define an intent named "BookFlight." Each prediction query includes the top scored intent. 

The client application can use the top scoring intent to trigger an action. For example, when "BookFlight" intent is returned from LUIS, a client application could trigger an API call to an external service for booking a plane ticket.

### Entities

An [entity](luis-how-to-add-entities.md) represents detailed information found within the utterance that is relevant to the user's request. For example, in the utterance "Book a ticket to Paris",  a single ticket is requested, and "Paris" is a location. Two entities are found "a ticket" indicating a single ticket and "Paris" indicating the destination. 

After LUIS returns the entities found in the user’s utterance, the client application can use the list of entities as parameters to trigger an action. For example, booking a flight requires entities like the travel destination, date, and airline.
-->
<!--
### Example utterances

An example [utterance](luis-how-to-add-example-utterances.md) is text input from the user that the client application needs to understand. It may be a sentence, like "Book a ticket to Paris", or a fragment of a sentence, like "Booking" or "Paris flight." Utterances aren't always well-formed, and there can be many utterance variations for a particular intent. Add 10 to 20 example utterances to each intent and mark entities in every utterance.

|Example user utterance|Intent|Entities|
|-----------|-----------|-----------|
|"Book a flight to __Seattle__?"|BookFlight|Seattle|
|"When does your store __open__?"|StoreHoursAndLocation|open|
|"Schedule a meeting at __1pm__ with __Bob__ in Distribution"|ScheduleMeeting|1pm, Bob|
-->
## <a name="query-prediction-endpoint"></a>Sorgu tahmin uç noktası

Model derlendikten ve uç noktada yayımlandıktan sonra istemci uygulaması yayımlanan tahmin [uç noktası](https://aka.ms/luis-endpoint-apis) API'sine konuşma gönderir. API, analiz için modeli metne uygular. API, JSON biçiminde tahmin sonuçlarıyla yanıt verir.  

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

Bir LUIS modeli yayımlandıktan ve gerçek kullanıcı konuşmalarını aldıktan sonra LUIS tarafından tahmin doğruluğunu artırmak için birden çok yöntem sunulur: uç nokta konuşmaları için [etkin öğrenme](#active-learning), etki alanı kelimelerini dahil etmek için [tümcecik listeleri](#phrase-lists) ve gerekli konuşma sayısını azaltmak için [desenler](#patterns).
<!--
### Active learning

In the [active learning](luis-how-to-review-endoint-utt.md) process, LUIS allows you to adapt the LUIS app to real-world utterances by selecting utterances it received at the endpoint for your review. You can accept or correct the endpoint prediction, retrain, and republish. LUIS learns quickly with this iterative process, taking the minimum amount of your time and effort. 

### Phrase lists 

LUIS provides [phrases lists](luis-concept-feature.md) so you can indicate important words or phrases of the model. LUIS uses these lists to add additional significance to those words and phrases that would otherwise not be found in the model.

### Patterns 

Patterns allow you to simplify an intent's utterance collection into common [templates](luis-concept-patterns.md) of word choice and word order. This allows LUIS to learn quicker by needing fewer example utterances for the intents. Patterns are a hybrid system of regular expressions and machine-learned expressions. 
-->
<a name="using-luis"></a>
<!--
## Authoring and accessing models
Author LUIS from the [authoring](https://aka.ms/luis-authoring-apis) APIs or from the LUIS portal. Query the published prediction endpoint of the model from the [endpoint](https://aka.ms/luis-endpoint-apis) APIs.
-->

## <a name="integrating-with-luis"></a>LUIS ile tümleştirme
LUIS, HTTP isteği gönderen tüm ürün, hizmet veya çerçevelerle REST API olarak kullanılabilir. Aşağıdaki liste, LUIS ile birlikte en çok kullanılan Microsoft ürünlerini ve hizmetlerini göstermektedir.

LUIS için Microsoft istemci uygulamaları şunlardır:
* [Web uygulaması botu](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-3.0), kullanıcıyla metin girişi aracılığıyla konuşmak için hızlıca LUIS destekli bir sohbet botu oluşturur. Eksiksiz bot deneyimi için [Bot Framework][bot-framework] sürüm [3.x](https://github.com/Microsoft/BotBuilder) veya [4.x](https://github.com/Microsoft/botbuilder-dotnet) kullanır.
* [Windows Karma Gerçeklik](https://docs.microsoft.com/windows/mixed-reality/): LUIS [karma gerçeklik kursu](https://docs.microsoft.com/windows/mixed-reality/mr-azure-303) ile daha fazla bilgi edinin. 

LUIS ile bot kullanmak için Microsoft araçları:
* [Gönderme](https://github.com/Microsoft/botbuilder-tools/tree/master/Dispatch), çeşitli LUIS ve Soru-Cevap Oluşturma uygulamalarının gönderme modelini kullanan bir üst uygulamadan kullanılmasını sağlar.
* [Konuşma öğrenici](https://docs.microsoft.com/azure/cognitive-services/labs/conversation-learner/overview), LUIS ile daha hızlı bir şekilde sohbet botları oluşturmanızı sağlar.
* [Kişilik sohbeti projesi](https://docs.microsoft.com/azure/cognitive-services/project-personality-chat/overview), bot ile kısa sohbetler yapmanızı sağlar.

LUIS ile kullanılan diğer Bilişsel Hizmetler:
* [Soru-Cevap Oluşturma][qnamaker], farklı metin türlerinin birleştirilerek soru ve yanıt bilgi bankası oluşturulmasını sağlar.
* [Bing Yazım Denetimi API'si](../bing-spell-check/proof-text.md), tahmin işlemi öncesinde metinlerin düzeltilmesini sağlar. 
* [Konuşma hizmeti](../Speech-Service/overview.md), sözlü dil isteklerini metne dönüştürür. 



## <a name="next-steps"></a>Sonraki adımlar

[Önceden oluşturulmuş](luis-get-started-create-app.md) veya [özel](luis-quickstart-intents-only.md) etki alanıyla yeni bir LUIS uygulaması yazma. Genel IoT uygulamasının [tahmin uç noktasını sorgulama](luis-get-started-cs-get-intent.md).

<!-- Reference-style links -->
[bot-framework]: https://docs.microsoft.com/bot-framework/
[flow]: https://docs.microsoft.com/connectors/luis/
[authoring-apis]: https://aka.ms/luis-authoring-api
[endpoint-apis]: https://aka.ms/luis-endpoint-apis
[qnamaker]: https://qnamaker.ai/