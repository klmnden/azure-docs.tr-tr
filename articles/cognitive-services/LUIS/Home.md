---
title: İlgili dil anlama (HALUK) Azure | Microsoft Docs
description: Machine learning uygulamalarınıza güç getirmek için dil anlama (HALUK) kullanmayı öğrenin.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/22/2017
ms.author: v-geberr
ms.openlocfilehash: bbd0a532e54f9b221739c8ae9ff097fe44fdc4df
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751604"
---
# <a name="what-is-language-understanding-luis"></a>Dil anlama (HALUK) nedir?
Dil anlama (HALUK) metne genel anlamı tahmin etmek ve ilgili, ayrıntılı bilgileri almak için bir kullanıcının konuşma, doğal dil özel machine learning uygulayan bir bulut tabanlı bir hizmettir. 

Bir istemci uygulaması HALUK için bir görevi tamamlamak için iletişim kuran tüm konuşma uygulama doğal dilde bir kullanıcıyla olabilir. İstemci uygulamaları örnekleri, sosyal medya uygulamalar, chatbots ve konuşma özellikli Masaüstü uygulamaları içerir.  

![Kavramsal bilgiler bilgi HALUK Besleme 3 uygulamaları görüntüsü](./media/luis-overview/luis-entry-point.png)

## <a name="what-is-a-luis-app"></a>HALUK uygulaması nedir?
HALUK uygulama tasarım bir etki alanına özgü doğal dil modeli içerir. Önceden oluşturulmuş bir etki alanı modeli ile HALUK uygulamanızı başlatmak, kendi oluşturabilir veya kendi özel bilgilerle önceden oluşturulmuş bir etki alanı parçalarını blend.

[Önceden oluşturulmuş bir etki alanı modelleri](luis-how-to-use-prebuilt-domains.md) tüm parçaların eklediğiniz ve HALUK hızlı şekilde kullanmaya başlamak için harika bir yoludur.

HALUK uygulama aynı zamanda tümleştirme ayarlarını içeren [ortak](luis-concept-collaborator.md), ve [sürümleri](luis-concept-version.md).

## <a name="using-a-luis-app"></a>HALUK uygulamasını kullanarak
<a name="Accessing-LUIS"></a> Bir kez HALUK uygulamanızı yayımlandıktan sonra istemci uygulamanızı utterances HALUK gönderir. [endpoint API] [ endpoint-apis] ve JSON yanıt olarak tahmin sonuçlarını alır.

Aşağıdaki şemada, bir kişi bir HTTP isteğindeki HALUK için kendi yazılı isteyen kullanıcı metin önce istemci chatbot gönderir. İkinci olarak, HALUK öğrenilen modelinizi kullanıcı girişi anlamlı için doğal dil için geçerlidir ve JavaScript nesne gösterimi (JSON) biçiminde yanıt verir. Üçüncü istemci chatbot kullanıcının isteklerini karşılamak için JSON yanıtı kullanır. 

![Chatbot ile çalışma HALUK, kavramsal görüntüler](./media/luis-overview/luis-overview-process-2.png)

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
<a name="what-is-a-luis-model"></a>
## <a name="what-is-a-natural-language-model"></a>Doğal dil modeli nedir?
Bir model adlı genel kullanıcı amaçları listesi ile başlar _hedefleri_, örneğin "Kitap uçuş" veya "Kişi Yardım Masası." Adlı kullanıcının örnek metin sağladığınız _örnek utterances_ amaçlar için. Önemli sözcük veya tümcecik adlı utterance işaretlemek _varlıklar_.


Bir model şunları içerir:

* **[hedefleri](#intents)**: kullanıcı amaçları (hedeflenen eylem veya sonuç) kategorileri
* **[varlıkları](#entities)**: belirli utterances numarası, e-posta veya adı gibi veri türleri
* **[Örnek utterances](#example-utterances)**: bir kullanıcının girdiği istemci uygulamasında örnek metin

### <a name="intents"></a>Hedefler 
Bir [hedefi](luis-how-to-add-intents.md), kısaltması _engellemekse_, bir amacı veya hedef ifade bir uçuş kayıt, bir fatura ödeme ya da bir haber makalesi bulma gibi bir kullanıcının utterance. Her eylem için amacına oluşturun. HALUK seyahat uygulama "BookFlight." adlı bir hedefi tanımlayabilir İstemci uygulamanızı hedefi Puanlama üst bir eylemi tetikleyen için kullanabilirsiniz. Örneğin, "BookFlight" amacı HALUK döndürüldüğünde, istemci uygulamanız bir API çağrısı uçak bileti kayıt için bir dış hizmetine tetikleyebilir.

### <a name="entities"></a>Varlıklar
Bir [varlık](luis-how-to-add-entities.md) ayrıntılı bilgi için kullanıcının isteği ilgili utterance içinde bulunan temsil eder. Örneğin, tek bir bilet utterance "Kitap Paris bilet" istenen ve "Paris" bir konumdur. İki varlık "tek bilet ve"hedef belirten Paris"gösteren bir bilet" bulunamadı. 

HALUK kullanıcının utterance bulunan varlıklar döndürüyor sonra istemci uygulaması bir eylemi tetikleyen için parametre olarak varlıkların listesi kullanabilirsiniz. Örneğin, bir uçuş kayıt seyahat hedef, tarih ve uçak gibi varlıkları gerektirir.

HALUK tanımlamak ve varlıkları kategorilere ayırmak için çeşitli yöntemler sağlar.

* **Önceden oluşturulmuş varlıklar** HALUK hedefleri, utterances, dahil olmak üzere birçok önceden oluşturulmuş bir etki alanı modeli vardır ve [önceden oluşturulmuş varlıklar](pre-builtentities.md). Hedefleri ve önceden oluşturulmuş modelinin utterances kullanmak zorunda kalmadan, önceden oluşturulmuş varlıklar kullanabilirsiniz. Önceden oluşturulmuş varlıklar size zaman kazandırır.

* **Özel varlıklar** HALUK kendi özel tanımlamak için çeşitli yollar size [varlıklar](luis-concept-entity-types.md) makine öğrenilen varlıklar, belirli ya da sabit varlıkları ve makine öğrenilen ve değişmez değerli bileşimini dahil.

### <a name="example-utterances"></a>Örnek utterances
Örnek [utterance](luis-how-to-add-example-utterances.md) metin giriş kullanıcıdan istemci uygulamanın anlamanız gerekir. "Bir bilet Paris için kitap" veya bir parçasını "Kayıt" gibi bir cümle gibi bir cümle olabilir ya da "Paris uçuş." Utterances her zaman doğru biçimlendirilmiş değil ve özel bir amaç için birçok utterance değişimler olabilir. 10 ila 20 örnek utterances her hedefi ekleyin ve her utterance varlıklarda işaretleyin.

|Örnek kullanıcı utterance|Hedefi|Varlıklar|
|-----------|-----------|-----------|
|"Bir uçuş kitap __Seattle__?"|BookFlight|Seattle|
|"Ne zaman deponuza mu __açmak__?"|StoreHoursAndLocation|aç|
|"Bir toplantıya zamanlama __13'te__ ile __Bob__ dağıtımındaki"|ScheduleMeeting|Bob 1 pm|

## <a name="improve-prediction-accuracy"></a>Tahmin doğruluğunu artırmak
HALUK uygulamanızı yayımlanır ve gerçek kullanıcı utterances aldıktan sonra HALUK tahmin doğruluğunu artırmak için çeşitli yöntemler sunar: [etkin öğrenme](#active-learning) endpoint utterances, [tümceyi listeleri](#phrase-lists) etki alanı için ekleme, Word ve [desenleri](#patterns) gerekli utterances sayısını azaltın.

### <a name="active-learning"></a>Etkin öğrenme
İçinde [etkin öğrenme](label-suggested-utterances.md) işlemi HALUK alınan gözden geçirmeniz için uç noktada utterances seçerek HALUK uygulamanıza gerçek utterances uyum olanak tanır. Kabul edin veya uç nokta tahmin, retrain ve yeniden yayımlama düzeltin. HALUK zaman ve çaba alt limiti alma ile yinelemeli bu işlem, hızlı bir şekilde öğrenir. 

### <a name="phrase-lists"></a>Tümcecik listeler 
HALUK sağlar [listeleri tümceleri](luis-concept-feature.md) modeli etki alanınıza önemli kelimeler ve ifadeler belirtmek için. Bu sözcükleri ve aksi durumda modelde bulunamadı tümcecikleri ek anlam eklemek için bu listeleri HALUK kullanır.

### <a name="patterns"></a>Desenler 
Desenler amacına 's utterance ortak koleksiyona basitleştirmek izin [şablonları](luis-concept-patterns.md) word seçeneği ve word order. Bu amaçlar için daha az örnek utterances gerek göre daha hızlı öğrenmek HALUK sağlar. Normal ifadeler ve makine öğrenilen ifadeler bir karma sistem desenleri alır. 

<a name="using-luis"></a>

## <a name="authoring-and-accessing-luis"></a>Yazma ve HALUK erişme
HALUK uygulamanızı HALUK Web sitesinden veya program aracılığıyla ile oluşturmaya [yazma](https://aka.ms/luis-authoring-apis) API'leri ya da kullanım hem de yazma ihtiyacına bağlı olarak. Sorgu tarafından yayımlanan HALUK uygulamanızı erişim [endpoint](https://aka.ms/luis-endpoint-apis). 

HALUK dünyanın geliştirme bölgenize bağlı olarak üç Web siteleri sağlar. Geliştirme bölge HALUK uygulamanızı nerede yayımlayabilirsiniz Azure bölgesi belirler.
<!--
|Authoring region|Publishing region(s)|
|--|--|
|[www.luis.ai](https://www.luis.ai)|**U.S.**<br>West US<br>West US 2<br>East US<br>East US 2<br>South Central US<br>West Central US<br><br>**Asia**<br>Southeast Asia<br>East Asia<br><br>**South America**<br>Brazil South |
|[au.luis.ai](https://au.luis.ai)|Australia East|
|[eu.luis.ai](https://eu.luis.ai)|West Europe<br>North Europe|
-->

Bilgi [daha fazla](luis-reference-regions.md) yazma ve bölgeler yayımlama hakkında.

## <a name="what-technologies-work-with-luis"></a>Hangi teknolojileri ile HALUK çalışıyor?
Birkaç Microsoft teknolojileri ile HALUK çalışır:

* [Bing yazım denetleme API](../bing-spell-check/proof-text.md) tahmin önce metin düzeltme sağlar. 
* [Bot Framework] [ bot-framework] metin girişi aracılığıyla bir kullanıcıyla iletişim kurabilecek şekilde chatbot sağlar. Seçin [3.x](https://github.com/Microsoft/BotBuilder) veya [4.x](https://github.com/Microsoft/botbuilder-dotnet) tam bot deneyimi için SDK.
* [QnA Maker] [ qnamaker] soru ve yanıt bankası birleştirmek için metin birkaç türlerine izin verir.
* [Konuşma](../Speech/home.md) konuşulan dil istekleri metne dönüştürür. Metne dönüştürüldükten sonra HALUK isteklerini işler. Bkz: [konuşma SDK](https://aka.ms/csspeech) daha fazla bilgi için.
* [Metin analizi](../text-analytics/overview.md) düşünceleri analiz ve anahtar tümcecik veri ayıklama sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Yeni bir HALUK uygulamayla oluşturma bir [önceden oluşturulmuş](luis-get-started-create-app.md) veya [özel](luis-quickstart-intents-only.md) etki alanı.

<!-- Reference-style links -->
[bot-framework]: https://docs.microsoft.com/bot-framework/
[flow]: https://docs.microsoft.com/connectors/luis/
[authoring-apis]: https://aka.ms/luis-authoring-api
[endpoint-apis]: https://aka.ms/luis-endpoint-apis
[qnamaker]: https://qnamaker.ai/