---
title: "Hızlı Başlangıç: Metin analizi API'sini çağırmak için node.js kullanma"
titleSuffix: Azure Cognitive Services
description: Metin Analizi API'sini kısa sürede kullanmaya başlamanıza yardımcı olacak bilgi ve kod örnekleri alın.
services: cognitive-services
author: raymondl
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 06/11/2019
ms.author: shthowse
ms.openlocfilehash: 7e43d53c0916cf7fdc684c9e044e632015662c3b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67081522"
---
# <a name="quickstart-using-nodejs-to-call-the-text-analytics-cognitive-service"></a>Hızlı Başlangıç: Metin analizi Bilişsel hizmetini çağırmak için node.js kullanma
<a name="HOLTop"></a>

Bu hızlı başlangıçta, Node.js için metin analizi SDK'sı ile dil incelemeye başlamak için kullanın. Sırada [metin analizi](//go.microsoft.com/fwlink/?LinkID=759711) çoğu programlama dilleri ile uyumlu REST API, SDK hizmeti uygulamalarınızla tümleştirmek için kolay bir yol sağlar. Bu örnek için kaynak kodu bulunabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/textAnalytics.js).

API'lerle ilgili teknik bilgiler için [API tanımları](//go.microsoft.com/fwlink/?LinkID=759346) sayfasını inceleyin.

## <a name="prerequisites"></a>Önkoşullar

* [Node.js](https://nodejs.org/)
* Metin analizi [SDK'sı için Node.js](https://www.npmjs.com/package/azure-cognitiveservices-textanalytics) SDK'sı ile yükleyebilirsiniz:

    `npm install azure-cognitiveservices-textanalytics`

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

Ayrıca kayıt sırasında oluşturulan [uç nokta ve erişim anahtarı](../How-tos/text-analytics-how-to-access-key.md) değerlerine de sahip olmanız gerekir.

## <a name="create-a-nodejs-application-and-install-the-sdk"></a>Node.js uygulaması oluşturma ve SDK'sını yükleyin

Node.js yükledikten sonra bir düğüm projesi oluşturun. Uygulamanız için yeni bir dizin oluşturun ve alt dizinine gidin.

```mkdir myapp && cd myapp```

Çalıştırma ```npm init``` node uygulaması ile bir package.json dosyası oluşturmak için. Yükleme `ms-rest-azure` ve `azure-cognitiveservices-textanalytics` NPM paketleri:

```npm install azure-cognitiveservices-textanalytics ms-rest-azure```

Uygulamanızın package.json dosyası bağımlılıkları ile güncelleştirilecektir.

## <a name="authenticate-your-credentials"></a>Kimlik bilgileriniz kimlik doğrulaması

Yeni bir dosya oluşturun `index.js` projesinde kök ve yüklü kitaplıkları içeri aktarma

```javascript
const CognitiveServicesCredentials = require("ms-rest-azure").CognitiveServicesCredentials;
const TextAnalyticsAPIClient = require("azure-cognitiveservices-textanalytics");
```

Metin analizi abonelik anahtarınız için bir değişken oluşturun.

```javascript
let credentials = new CognitiveServicesCredentials(
    "enter-your-key-here"
);
```

> [!Tip]
> Üretim sistemlerine gizli dizileri güvenli dağıtımı için kullanmanızı öneririz [Azure anahtar kasası](https://docs.microsoft.com/azure/key-vault/quick-create-net).
>

## <a name="create-a-text-analytics-client"></a>Metin analizi istemcisi oluşturma

Yeni bir `TextAnalyticsClient` nesnesi ile `credentials` bir parametre olarak. Metin analizi aboneliğiniz için doğru Azure bölgesi kullanın.

```javascript
//Replace 'westus' with the correct region for your Text Analytics subscription
let client = new TextAnalyticsAPIClient(
    credentials,
    "https://westus.api.cognitive.microsoft.com/"
);
```

## <a name="sentiment-analysis"></a>Yaklaşım analizi

Nesneleri, çözümlemek istediğiniz belgeleri içeren bir liste oluşturur. API yükü bir listesinden oluşur `documents`, aşağıdakileri içeren bir `id`, `language`, ve `text` özniteliği. `text` Öznitelik depolarının Analiz edilecek metnin `language` belge dilidir ve `id` herhangi bir değer olabilir. 

```javascript
const inputDocuments = {documents:[
    {language:"en", id:"1", text:"I had the best day of my life."},
    {language:"en", id:"2", text:"This was a waste of my time. The speaker put me to sleep."},
    {language:"es", id:"3", text:"No tengo dinero ni nada que dar..."},
    {language:"it", id:"4", text:"L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."}
]}
```

Çağrı `client.sentiment` ve sonucu alın. Ardından sonuçlarını yinelemek ve her bir belgenin kimliği ve yaklaşım puanını yazdırın. Bir puan yakın 1 pozitif yaklaşımı çağrılırken bir puan yakın 0 bir negatif yaklaşımı gösterir.

```javascript
const operation = client.sentiment({multiLanguageBatchInput: inputDocuments})
operation
.then(result => {
    console.log(result.documents);
})
.catch(err => {
    throw err;
});
```

Kodunuzu çalıştırmak `node index.js` konsol pencerenizde.

### <a name="output"></a>Çıktı

```console
[ { id: '1', score: 0.8723785877227783 },
  { id: '2', score: 0.1059873104095459 },
  { id: '3', score: 0.43635445833206177 },
  { id: '4', score: 1 } ]
```

## <a name="language-detection"></a>Dil algılama

Belgelerinizi içeren nesnelerin bir listesini oluşturun. API yükü bir listesinden oluşur `documents`, aşağıdakileri içeren bir `id` ve `text` özniteliği. `text` Öznitelik depolarının Analiz edilecek metin ve `id` herhangi bir değer olabilir.

```javascript
// The documents to be submitted for language detection. The ID can be any value.
const inputDocuments = {
    documents: [
        { id: "1", text: "This is a document written in English." },
        { id: "2", text: "Este es un document escrito en Español." },
        { id: "3", text: "这是一个用中文写的文件" }
    ]
    };
```

Çağrı `client.detectLanguage()` ve sonucu alın. Ardından sonuçlarını yinelemek ve her bir belgenin kimliği ve ilk döndürülen dil yazdırın.

```javascript
const operation = client.detectLanguage({
    languageBatchInput: inputDocuments
});
operation
    .then(result => {
    result.documents.forEach(document => {
        console.log(`ID: ${document.id}`);
        document.detectedLanguages.forEach(language =>
        console.log(`\tLanguage: ${language.name}`)
        );
    });
    })
    .catch(err => {
    throw err;
    });
```

Kodunuzu çalıştırmak `node index.js` konsol pencerenizde.

### <a name="output"></a>Çıktı

```console
===== LANGUAGE EXTRACTION ======
ID: 1 Language English
ID: 2 Language Spanish
ID: 3 Language Chinese_Simplified
```

## <a name="entity-recognition"></a>Varlık tanıma

Belgelerinizi içeren nesnelerin bir liste oluşturur. API yükü bir listesinden oluşur `documents`, aşağıdakileri içeren bir `id`, `language`, ve `text` özniteliği. `text` Öznitelik depolarının Analiz edilecek metnin `language` belge dilidir ve `id` herhangi bir değer olabilir.

```javascript

    const inputDocuments = {documents:[
        {language:"en", id:"1", text:"Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800"},
        {language:"es", id:"2", text:"La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle."},
        ]}

}
```

Çağrı `client.entities()` ve sonucu alın. Ardından sonuçlarını yinelemek ve her bir belgenin kimliği yazdırma Her varlık algılanan için wikipedia adını, türünü ve alt türleri yazdırma (varsa var) orijinal metni konumlarda yanı sıra.

```javascript
const operation = client.entities({
    multiLanguageBatchInput: inputDocuments
});
operation
    .then(result => {
    result.documents.forEach(document => {
        console.log(`Document ID: ${document.id}`)
        document.entities.forEach(e =>{
        console.log(`\tName: ${e.name} Type: ${e.type} Sub Type: ${e.type}`)
        e.matches.forEach(match => (
            console.log(`\t\tOffset: ${match.offset} Length: ${match.length} Score: ${match.entityTypeScore}`)
        ))
        })
    });
    })
    .catch(err => {
    throw err;
    });
```

Kodunuzu çalıştırmak `node index.js` konsol pencerenizde.

### <a name="output"></a>Çıktı

```console
Document ID: 1
    Name: Microsoft Type: Organization Sub Type: Organization
            Offset: 0 Length: 9 Score: 1
    Name: Bill Gates Type: Person Sub Type: Person
            Offset: 25 Length: 10 Score: 0.999786376953125
    Name: Paul Allen Type: Person Sub Type: Person
            Offset: 40 Length: 10 Score: 0.9988105297088623
    Name: April 4 Type: Other Sub Type: Other
            Offset: 54 Length: 7 Score: 0.8
    Name: April 4, 1975 Type: DateTime Sub Type: DateTime
            Offset: 54 Length: 13 Score: 0.8
    Name: BASIC Type: Other Sub Type: Other
            Offset: 89 Length: 5 Score: 0.8
    Name: Altair 8800 Type: Other Sub Type: Other
            Offset: 116 Length: 11 Score: 0.8
Document ID: 2
    Name: Microsoft Type: Organization Sub Type: Organization
            Offset: 21 Length: 9 Score: 0.999755859375
    Name: Redmond (Washington) Type: Location Sub Type: Location
            Offset: 60 Length: 7 Score: 0.9911284446716309
    Name: 21 kilómetros Type: Quantity Sub Type: Quantity
            Offset: 71 Length: 13 Score: 0.8
    Name: Seattle Type: Location Sub Type: Location
            Offset: 88 Length: 7 Score: 0.9998779296875
```

## <a name="key-phrase-extraction"></a>Anahtar tümcecik ayıklama

Belgelerinizi içeren nesnelerin bir liste oluşturur. API yükü bir listesinden oluşur `documents`, aşağıdakileri içeren bir `id`, `language`, ve `text` özniteliği. `text` Öznitelik depolarının Analiz edilecek metnin `language` belge dilidir ve `id` herhangi bir değer olabilir.

```javascript
    let inputLanguage = {
    documents: [
        {language:"ja", id:"1", text:"猫は幸せ"},
        {language:"de", id:"2", text:"Fahrt nach Stuttgart und dann zum Hotel zu Fu."},
        {language:"en", id:"3", text:"My cat might need to see a veterinarian."},
        {language:"es", id:"4", text:"A mi me encanta el fútbol!"}
    ]
    };
```

Çağrı `client.keyPhrases()` ve sonucu alın. Ardından sonuçlarını yinelemek ve her bir belgenin kimliği ve algılanan tüm anahtar ifadeleri yazdırın.

```javascript
    let operation = client.keyPhrases({
    multiLanguageBatchInput: inputLanguage
    });
    operation
    .then(result => {
        console.log(result.documents);
    })
    .catch(err => {
        throw err;
    });
```

Kodunuzu çalıştırmak `node index.js` konsol pencerenizde.

### <a name="output"></a>Çıktı

```console
[ 
    { id: '1', keyPhrases: [ '幸せ' ] },
    { id: '2', keyPhrases: [ 'Stuttgart', 'Hotel', 'Fahrt', 'Fu' ] },
    { id: '3', keyPhrases: [ 'cat', 'veterinarian' ] },
    { id: '4', keyPhrases: [ 'fútbol' ] } 
]
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Power BI ile Metin Analizi](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Ayrıca bkz.

 [Metin Analizi'ne genel bakış](../overview.md) [sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)
