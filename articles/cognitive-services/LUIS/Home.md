---
title: İlgili dil anlama (HALUK) Azure | Microsoft Docs
description: Machine learning uygulamalarınıza güç getirmek için dil anlama (HALUK) kullanmayı öğrenin.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/22/2017
ms.author: v-geberr
ms.openlocfilehash: 5656ce051a171812e6d008d56b42bf16c0c2a2ca
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "35356130"
---
# <a name="what-is-language-understanding-luis"></a>Dil anlama (HALUK) nedir?
Dil anlama (HALUK) metne genel anlamı tahmin etmek ve ilgili, ayrıntılı bilgileri almak için bir kullanıcının konuşma, doğal dil özel machine learning uygulayan bir bulut tabanlı bir hizmettir. 

Bir istemci uygulaması HALUK için bir görevi tamamlamak için iletişim kuran tüm konuşma uygulama doğal dilde bir kullanıcıyla olabilir. İstemci uygulamaları örnekleri, sosyal medya uygulamalar, chatbots ve konuşma özellikli Masaüstü uygulamaları içerir.  

![Kavramsal bilgiler bilgi HALUK Besleme 3 uygulamaları görüntüsü](./media/luis-overview/luis-entry-point.png)

İstemci uygulamanız (örneğin, bir chatbot) bir kişi bir HTTP isteğindeki HALUK için kendi yazılı isteyen kullanıcı metin gönderir. HALUK öğrenilen modelinizi kullanıcı girişi anlamlı için doğal dil uygular ve JSON biçimi yanıtı döndürür. İstemci uygulamanızın JSON yanıtı kullanıcının isteklerini karşılamak için kullanır. 

![Chatbot ile çalışma HALUK, kavramsal görüntüler](./media/luis-overview/luis-overview-process-2.png)

## <a name="what-is-a-luis-app"></a>HALUK uygulaması nedir?
HALUK uygulama tasarım bir etki alanına özgü dil modelidir. Önceden oluşturulmuş bir etki alanı modeli ile uygulamanızı başlatmak, kendi oluşturabilir veya kendi özel bilgilerle önceden oluşturulmuş bir etki alanı parçalarını blend.

Bir model adlı genel kullanıcı amaçları listesi ile başlar _hedefleri_, örneğin "Kitap uçuş" veya "Kişi Yardım Masası." Adlı kullanıcının örnek tümcecikleri sağladığınız _utterances_ amaçlar için. Önemli sözcük veya tümcecik adlı utterance işaretlemek _varlıklar_.

[Önceden oluşturulmuş bir etki alanı modelleri] [ prebuilt-domains] tüm parçaların eklediğiniz ve HALUK hızlı şekilde kullanmaya başlamak için harika bir yoludur.

<a name="Accessing-LUIS"></a>

Modelinizi oluşturulan ve yayımlanan sonra istemci uygulamanız utterances HALUK gönderir. [endpoint API] [ endpoint-apis] ve JSON yanıt olarak tahmin sonuçlarını alır.

### <a name="example-of-json-endpoint-response"></a>JSON bitiş noktası yanıt örneği

JSON bitiş noktası yanıt en az sorgu utterance ve amacı Puanlama üst içerir. 

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

<a name="Key-LUIS-concepts"></a>

## <a name="what-is-a-luis-model"></a>HALUK modeli nedir?
HALUK model şunları içerir:

* **[hedefleri](#intents)**: kullanıcı amaçları (hedeflenen eylem veya sonuç) kategorileri
* **[varlıkları](#entities)**: belirli utterances numarası, e-posta veya adı gibi veri türleri
* **[Örnek utterances](#example-utterances)**: bir kullanıcının girdiği istemci uygulamanızı örnek metin

### <a name="intents"></a>Hedefler 
Bir [hedefi][add-intents], kısaltması _engellemekse_, bir amacı veya hedef ifade bir uçuş kayıt, bir fatura ödeme ya da bir haber makalesi bulma gibi bir kullanıcının utterance. Her eylem için amacına oluşturun. Seyahat uygulama "BookFlight." adlı bir hedefi tanımlayabilir İstemci uygulamanızı hedefi Puanlama üst bir eylemi tetikleyen için kullanabilirsiniz. Örneğin, "BookFlight" amacı HALUK döndürüldüğünde, istemci uygulamanız bir API çağrısı uçak bileti kayıt için bir dış hizmetine tetikleyebilir.

### <a name="entities"></a>Varlıklar
Bir [varlık] [ add-entities] ayrıntılı bilgi için kullanıcının isteği ilgili utterance içinde bulunan temsil eder. Örneğin, tek bir bilet utterance "Kitap Paris bilet" istenen ve "Paris" bir konumdur. İki varlık "tek bilet ve"hedef belirten Paris"gösteren bir bilet" bulunamadı. 

HALUK kullanıcının utterance bulunan varlıklar döndürüyor sonra istemci uygulamanızı tetiklenen bir eylem için parametre olarak varlıkların listesi kullanabilirsiniz. Örneğin, bir uçuş kayıt seyahat hedef, tarih ve uçak gibi varlıkları gerektirir.

HALUK tanımlamak ve varlıkları kategorilere ayırmak için çeşitli yöntemler sağlar.

* **Önceden oluşturulmuş varlıklar** HALUK hedefleri, utterances, dahil olmak üzere birçok önceden oluşturulmuş bir etki alanı modeli vardır ve [önceden oluşturulmuş varlıklar][prebuilt-entities]. Hedefleri ve önceden oluşturulmuş modelinin utterances kullanmak zorunda kalmadan, önceden oluşturulmuş varlıklar kullanabilirsiniz. Önceden oluşturulmuş varlıklar size zaman kazandırır.

* **Özel varlıklar** HALUK kendi özel tanımlamak için çeşitli yollar size [varlıklar] [ entity-concept] makine öğrenilen varlıklar, belirli ya da sabit varlıkları ve bir birleşimini dahil Makine öğrenilen ve değişmez.

### <a name="example-utterances"></a>Örnek utterances
Örnek [utterance] [ add-example-utterances] anlamak için uygulamanız gereken kullanıcıdan metin giriş. "Bir bilet Paris için kitap" veya bir parçasını "Kayıt" gibi bir cümle gibi bir cümle olabilir ya da "Paris uçuş." Utterances her zaman doğru biçimlendirilmiş değil ve özel bir amaç için birçok utterance değişimler olabilir. 10 ila 20 örnek utterances her hedefi ekleyin ve her utterance varlıklarda işaretleyin.

|Örnek kullanıcı utterance|Hedefi|Varlıklar|
|-----------|-----------|-----------|
|"Bir uçuş kitap __Seattle__?"|BookFlight|Seattle|
|"Ne zaman deponuza mu __açmak__?"|StoreHoursAndLocation|aç|
|"Bir toplantıya zamanlama __13'te__ ile __Bob__ dağıtımındaki"|ScheduleMeeting|Bob 1 pm|

## <a name="improve-prediction-accuracy"></a>Tahmin doğruluğunu artırmak
Uygulamanızı yayımlanır ve gerçek kullanıcı utterances aldıktan sonra HALUK tahmin doğruluğunu artırmak için çeşitli yöntemler sunar: [etkin öğrenme](#active-learning) endpoint utterances, [tümceyi listeleri](#phrase-lists) için etki alanı word ekleme ve [desenleri](#patterns) gerekli utterances sayısını azaltmak için.

### <a name="active-learning"></a>Etkin öğrenme
İçinde [etkin öğrenme](label-suggested-utterances.md) işlemi HALUK alınan gözden geçirmeniz için uç noktada utterances seçerek uygulamanızı gerçek dünya utterances uyum olanak tanır. Kabul edin veya uç nokta tahmin, retrain ve yeniden yayımlama düzeltin. HALUK zaman ve çaba alt limiti alma ile yinelemeli bu işlem, hızlı bir şekilde öğrenir. 

### <a name="phrase-lists"></a>Tümcecik listeler 
HALUK sağlar [listeleri tümceleri](luis-concept-feature.md) modeli etki alanınıza önemli kelimeler ve ifadeler belirtmek için. Bu sözcükleri ve aksi durumda modelde bulunamadı tümcecikleri ek anlam eklemek için bu listeleri HALUK kullanır.

### <a name="patterns"></a>Desenler 
Desenler amacına 's utterance ortak koleksiyona basitleştirmek izin [şablonları] [ patterns] word seçeneği ve word order. Bu amaçlar için daha az örnek utterances gerek göre daha hızlı öğrenmek HALUK sağlar. Normal ifadeler ve makine öğrenilen ifadeler bir karma sistem desenleri alır. 

## <a name="using-luis"></a>HALUK kullanma
HALUK uygulamanızdan yapı [www.luis.ai](http://www.luis.ai) Web sitesi veya program aracılığıyla ile uygulamanızı oluşturma, can [yazma](https://aka.ms/luis-authoring-apis) API'leri. Sorgu tarafından yayımlanan HALUK uygulamanızı erişim [endpoint](https://aka.ms/luis-endpoint-apis). 

## <a name="what-technologies-work-with-luis"></a>Hangi teknolojileri ile HALUK çalışıyor?
Birkaç Microsoft teknolojileri ile HALUK çalışır:

* [Bing yazım denetleme API] [ bing-spell-check-api] tahmin önce metin düzeltme sağlar. 
* [Bot Framework] [ bot-framework] metin girişi aracılığıyla bir kullanıcıyla iletişim kurabilecek şekilde chatbot sağlar. Seçin [3.x](https://github.com/Microsoft/BotBuilder) veya [4.x](https://github.com/Microsoft/botbuilder-dotnet) tam bot deneyimi için SDK.
* [QnA Maker] [ qnamaker] soru ve yanıt bankası birleştirmek için metin birkaç türlerine izin verir.
* [Konuşma] [ speech] konuşulan dil istekleri metne dönüştürür. Metne dönüştürüldükten sonra HALUK isteklerini işler. Bkz: [konuşma SDK](https://aka.ms/csspeech) daha fazla bilgi için.
* [Metin analizi] [ text-analytics] düşünceleri analiz ve anahtar tümcecik veri ayıklama sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Oluşturma bir [yeni HALUK uygulaması](LUIS-get-started-create-app.md).

<!-- Reference-style links -->
[create-app]:luis-get-started-create-app.md
[azure-portal]:https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account
[publish-app]:PublishApp.md#test-your-published-endpoint-in-a-browser
[luis-concept-entity-types]:luis-concept-entity-types.md
[add-example-utterances]: luis-how-to-add-example-utterances.md
[prebuilt-entities]: pre-builtentities.md
[prebuilt-domains]: luis-how-to-use-prebuilt-domains.md
[label-suggested-utterances]: label-suggested-utterances.md
[intro-video]:https://aka.ms/LUIS-Intro-Video
[bot-framework]:https://docs.microsoft.com/bot-framework/
[speech]:../Speech/index.md
[flow]:https://docs.microsoft.com/connectors/luis/
[entity-concept]:luis-concept-entity-types.md
[add-intents]:luis-how-to-add-intents.md
[add-entities]:luis-how-to-add-entities.md
[authoring-apis]:https://aka.ms/luis-authoring-api
[endpoint-apis]:https://aka.ms/luis-endpoint-apis
[LUIS]:luis-reference-regions.md
[text-analytics]:https://azure.microsoft.com/services/cognitive-services/text-analytics/
[patterns]:luis-concept-patterns.md
[bing-spell-check-api]:https://azure.microsoft.com/services/cognitive-services/spell-check/
[qnamaker]:https://qnamaker.ai/