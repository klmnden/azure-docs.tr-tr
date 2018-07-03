---
title: İlgili dil anlama (LUIS) azure'da | Microsoft Docs
description: Language Understanding (LUIS), machine learning, uygulamalarınıza gücünü katın için kullanmayı öğrenin.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/22/2017
ms.author: v-geberr
ms.openlocfilehash: da8ea6dead6b22d97e7338b2aa57a892be475417
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2018
ms.locfileid: "37344840"
---
# <a name="what-is-language-understanding-luis"></a>Language Understanding (LUIS) nedir?
Language Understanding (LUIS) genel anlamı tahmin edin ve ilgili ve ayrıntılı bilgileri almak için bir kullanıcının konuşma, doğal dil metinlerinde özel makine öğrenimi uygulanan bir bulut tabanlı bir hizmettir. 

LUIS, bir istemci uygulamanın bir görevi tamamlamak için iletişim kurduğu herhangi damıtarak konuşma bağlamında kullanılabilen bir uygulama ile doğal dildeki bir kullanıcı olabilir. İstemci uygulamalara örnek olarak, sosyal medya uygulamaları, sohbet robotları ve konuşma özellikli Masaüstü uygulamaları içerir.  

![Kavramsal bilgiler bilgi LUIS Besleme 3 uygulama görüntüsü](./media/luis-overview/luis-entry-point.png)

## <a name="what-is-a-luis-app"></a>Bir LUIS uygulaması nedir?
Bir LUIS uygulaması tasarım bir etki alanına özgü doğal dil modeli içerir. Önceden oluşturulmuş etki alanı modeli ile LUIS uygulamanızı başlatın, kendi uzantınızı oluşturun veya kendi özel bilgilerinizi parçalarını önceden oluşturulmuş bir etki alanı karıştırma.

[Önceden oluşturulmuş etki alanı modelleri](luis-how-to-use-prebuilt-domains.md) bu parçaların tamamı eklediğiniz ve LUIS hızlıca kullanmaya başlamak için harika bir yoludur.

LUIS uygulaması da Tümleştirme ayarlarını içeren [ortak çalışanlar](luis-concept-collaborator.md), ve [sürümleri](luis-concept-version.md).

## <a name="using-a-luis-app"></a>Bir LUIS uygulaması kullanma
<a name="Accessing-LUIS"></a> LUIS uygulamanızı yayımladıktan sonra istemci uygulamanıza konuşma için LUIS gönderir. [API uç noktası] [ endpoint-apis] ve JSON yanıt olarak tahmin sonuçlarını alır.

Aşağıdaki diyagramda, istemci sohbet botu bir kişi kendi kelimelerinizle LUIS için bir HTTP isteği, istediği kullanıcının metin önce gönderir. İkinci olarak, LUIS öğrenilen modelinizin kullanıcı girişi anlamlı için doğal dil için geçerlidir ve bir JavaScript nesne gösterimi (JSON) biçiminde yanıt verir. Üçüncü olarak, kullanıcı isteklerini karşılamak için JSON yanıt, istemci sohbet botu kullanır. 

![LUIS ile Sohbet Botu çalışma, kavramsal tanımayı](./media/luis-overview/luis-overview-process-2.png)

### <a name="example-of-json-endpoint-response"></a>Uç nokta yanıt JSON örneği

JSON uç nokta yanıt en az sorguyu utterance ve hedefi Puanlama üst içerir. 

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
Bir model adlı genel kullanıcı amaçları listesi ile başlar _hedefleri_gibi "Kitap uçuş" veya "Kişi Yardım Masası." Adlı kullanıcının örnek metin sağladığınız _örnek konuşma_ hedefleri için. Ardından önemli sözcükleri veya tümcecikleri adlı utterance işaretlemek _varlıkları_.


Bir model şunları içerir:

* **[ıntents](#intents)**: kullanıcı amaçları (istenen eylemi veya sonuç) kategorileri
* **[varlıkları](#entities)**: belirli konuşma numarası, e-posta veya ad gibi veri türleri
* **[Örnek konuşma](#example-utterances)**: kullanıcının girdiği istemci uygulamasında örnek metin

### <a name="intents"></a>Hedefler 
Bir [hedefi](luis-how-to-add-intents.md), kısaltması _engellemekse_, bir amaç veya hedef ifade bir kullanıcının utterance, bir uçuştaki kayıt, fatura ödeme veya haber makalesinin bulma gibi. Her eylem için bir amacı oluşturursunuz. LUIS seyahat uygulaması "BookFlight." adlı bir hedefi tanımlayabilirsiniz. İstemci uygulamanızı hedefi Puanlama üst bir eylem tetiklemek için kullanabilirsiniz. Örneğin, "BookFlight" hedefi LUIS döndürüldüğünde, istemci uygulamanızın uçak bileti kayıt için bir dış hizmet için bir API çağrısı tetikleyebilir.

### <a name="entities"></a>Varlıklar
Bir [varlık](luis-how-to-add-entities.md) kullanıcının isteği ile ilgili utterance bulunan ayrıntılı bilgileri temsil eder. Örneğin, tek bilet istenen utterance "Kitap Paris bilet" ve "Paris" bir konumdur. İki varlık "tek bilet ve"hedef belirten Paris"gösteren bir bilet" bulunamadı. 

LUIS kullanıcının utterance içinde bulunan varlıklara döndükten sonra istemci uygulama bir eylem tetiklemek için parametre olarak varlıkların listesini kullanabilirsiniz. Örneğin, bir uçuştaki kayıt seyahat hedef, tarih ve Havayolu gibi varlıkları gerektirir.

LUIS, tanımlamak ve varlıkları kategorilere ayırmak için birçok yol sağlar.

* **Önceden oluşturulmuş varlıklarla** LUIS olan hedefleri, konuşma, dahil olmak üzere birçok önceden oluşturulmuş etki alanı modelleri ve [önceden oluşturulmuş varlıklarla](luis-prebuilt-entities.md). Hedefleri ve önceden oluşturulmuş modelinin konuşma kullanmak zorunda kalmadan, önceden oluşturulmuş varlıklarla kullanabilirsiniz. Önceden oluşturulmuş varlıklar size zaman kazandırır.

* **Özel varlıklar** LUIS size kendi özel tanımlamak için birkaç yol [varlıkları](luis-concept-entity-types.md) makine öğrenilen varlıklar, belirli veya değişmez değer varlıkları ve makine hakkında bilgi edindiniz ve değişmez değerli bir birleşimini dahil.

### <a name="example-utterances"></a>Örnek konuşmalar
Bir örnek [utterance](luis-how-to-add-example-utterances.md) metin girişi kullanıcıdan istemci uygulaması anlaması gerekir. "Bir bilet Paris için kitap" veya "Kayıt" gibi bir cümle bir parçasını gibi bir cümle olabilir ya da "Paris uçuş." Konuşma her zaman iyi biçimlendirilmiş değil ve özel bir amaç için birçok utterance farklılıklar olabilir. Her ıntent'e 10 ila 20 örnek Konuşma ekleme ve her utterance varlıklarda işaretleyin.

|Örnek kullanıcı utterance|Hedefi|Varlıklar|
|-----------|-----------|-----------|
|"Bir uçuş için kitap __Seattle__?"|BookFlight|Seattle|
|"Ne zaman deponuz yok __açın__?"|StoreHoursAndLocation|aç|
|"Bir toplantıda zamanlama __13'te__ ile __Bob__ dağıtımındaki"|ScheduleMeeting|13'te, Bob|

## <a name="improve-prediction-accuracy"></a>Tahmin doğruluğunu iyileştirme
LUIS uygulamanızı yayımlanır ve gerçek kullanıcı konuşma aldıktan sonra LUIS tahmin doğruluğunu artırmak için çeşitli yöntemler sunar: [etkin olarak öğrenmeye](#active-learning) konuşma uç noktası,'ın [tümcecik listeleri](#phrase-lists) etki alanı ekleme, Word ve [desenleri](#patterns) gerekli konuşma sayısını azaltmak için.

### <a name="active-learning"></a>Etkin öğrenme
İçinde [etkin olarak öğrenmeye](label-suggested-utterances.md) işlem, LUIS, aldığı gözden geçirmeniz için uç noktasında konuşma seçerek LUIS uygulamanızı gerçek konuşma uyum sağlar. Kabul edebilir veya uç nokta tahmin, retrain ve sitemde yeniden yayınlayacağım düzeltin. LUIS ile yinelemeli bu işlem, en az miktarda zaman ve çaba alma hızlı öğrenir. 

### <a name="phrase-lists"></a>Tümcecik listeleri 
LUIS sağlar [listeleri tümceleri](luis-concept-feature.md) modeli etki alanınıza önemli bir sözcük ve tümcecikleri belirtmek için. LUIS, bu sözcükler ve aksi takdirde modelde bulunamaz tümcecikleri ek anlam eklemek için bu listeleri kullanır.

### <a name="patterns"></a>Desenler 
Desenleri, yaygın bir amaç'ın utterance koleksiyona basitleştirmek izin [şablonları](luis-concept-patterns.md) sözcük seçimi ve sözcük sırasını. Bu, daha hızlı hedefleri için daha az örnek konuşma ihtiyaç duyan bilgi edinmek LUIS sağlar. Hibrit bir sistemin normal ifadeler ve deyimler makine öğrendiniz desenleri alır. 

<a name="using-luis"></a>

## <a name="authoring-and-accessing-luis"></a>Yazma ve LUIS erişme
LUIS uygulamanızı LUIS Web sitesinden veya programlama yoluyla ile derleme [yazma](https://aka.ms/luis-authoring-apis) API'leri veya kullanım hem de yazma ihtiyacına bağlı olarak. Sorgu tarafından yayımlanan LUIS uygulamanızı erişim [uç nokta](https://aka.ms/luis-endpoint-apis). 

LUIS yazma bölgenizi bağlı olarak dünyanın dört bir yanındaki üç Web siteleri sağlar. Yazma bölgesi LUIS uygulamanızı nerede yayımlayabilmeniz için Azure bölgesini belirler.
<!--
|Authoring region|Publishing region(s)|
|--|--|
|[www.luis.ai](https://www.luis.ai)|**U.S.**<br>West US<br>West US 2<br>East US<br>East US 2<br>South Central US<br>West Central US<br><br>**Asia**<br>Southeast Asia<br>East Asia<br><br>**South America**<br>Brazil South |
|[au.luis.ai](https://au.luis.ai)|Australia East|
|[eu.luis.ai](https://eu.luis.ai)|West Europe<br>North Europe|
-->

Bilgi [daha fazla](luis-reference-regions.md) yazma ve bölgeleri yayımlama hakkında.

## <a name="what-technologies-work-with-luis"></a>Hangi teknolojileri LUIS ile çalışıyor?
Çeşitli Microsoft teknolojileri LUIS ile çalışır:

* [Bing yazım denetimi API'si](../bing-spell-check/proof-text.md) tahmin önce metin düzeltme sağlar. 
* [Bot Framework] [ bot-framework] bir sohbet Robotu, metin girişi aracılığıyla bir kullanıcıyla iletişim kurmasına izin verir. Seçin [3.x](https://github.com/Microsoft/BotBuilder) veya [4.x](https://github.com/Microsoft/botbuilder-dotnet) eksiksiz bir bot deneyimi için SDK'sı.
* [Soru-cevap Oluşturucu] [ qnamaker] birkaç soru ve yanıt bankası birleştirmek için metin türlerine izin verir.
* [Konuşma](../Speech/home.md) istekleri konuşma dilini metne dönüştürür. Metne dönüştürüldükten sonra LUIS istekleri işler. Bkz: [Speech SDK'sı](https://aka.ms/csspeech) daha fazla bilgi için.
* [Metin analizi](../text-analytics/overview.md) yaklaşım analizi ve anahtar tümcecik veri ayıklama sağlar.

## <a name="next-steps"></a>Sonraki adımlar
İle yeni bir LUIS uygulaması oluşturma bir [önceden oluşturulmuş](luis-get-started-create-app.md) veya [özel](luis-quickstart-intents-only.md) etki alanı.

<!-- Reference-style links -->
[bot-framework]: https://docs.microsoft.com/bot-framework/
[flow]: https://docs.microsoft.com/connectors/luis/
[authoring-apis]: https://aka.ms/luis-authoring-api
[endpoint-apis]: https://aka.ms/luis-endpoint-apis
[qnamaker]: https://qnamaker.ai/