---
title: "Hızlı Başlangıç: Python SDK'sını kullanarak metin analizi hizmeti çağırma"
titleSuffix: Azure Cognitive Services
description: Hızlı bir şekilde yardımcı olması için alma bilgileri ve kod örnekleri, Azure Bilişsel hizmetler metin analizi API'sini kullanarak başlayın.
services: cognitive-services
author: ctufts
manager: assafi
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 03/28/2019
ms.author: aahi
ms.openlocfilehash: b319abf22f9aa4cdd9a5fef91be0628672d47bd4
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66297796"
---
# <a name="quickstart-call-the-text-analytics-service-using-the-python-sdk"></a>Hızlı Başlangıç: Python SDK'sını kullanarak metin analizi hizmeti çağırma 
<a name="HOLTop"></a>

Bu hızlı başlangıçta Python için metin analizi SDK'sı ile dil incelemeye başlamak için kullanın. Metin analizi REST API'si ile çoğu programlama dilinden uyumlu olsa da, SDK hizmeti seri hale getirme ve JSON seri durumdan çıkarılırken uygulamalarınızla tümleştirmek için kolay bir yol sağlar. Bu örnek için kaynak kodu bulunabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/language/text_analytics_samples.py).

## <a name="prerequisites"></a>Önkoşullar

* [Python 3.x](https://www.python.org/)

* Metin analizi [için python SDK'sı](https://pypi.org/project/azure-cognitiveservices-language-textanalytics/) paketi yükleyebilirsiniz:

    `pip install --upgrade azure-cognitiveservices-language-textanalytics`

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

Sahip olmalısınız [uç noktası ve erişim anahtarı](../How-tos/text-analytics-how-to-access-key.md) , oluşturuldu, kayıt sırasında.

## <a name="create-a-new-python-application"></a>Yeni Python uygulaması oluşturma

Tercih ettiğiniz düzenleyiciyi veya IDE içinde yeni bir Python uygulaması oluşturun. Ardından dosyanızı içeri aktarma deyimlerini ekleyin.

```python
from azure.cognitiveservices.language.textanalytics import TextAnalyticsClient
from msrest.authentication import CognitiveServicesCredentials
```

## <a name="authenticate-your-credentials"></a>Kimlik bilgileriniz kimlik doğrulaması

> [!Tip]
> Üretim sistemlerine gizli dizileri güvenli dağıtımı için kullanmanızı öneririz [Azure anahtar kasası](https://docs.microsoft.com/azure/key-vault/quick-create-net).
>

Metin analizi abonelik anahtarınız için bir değişken yaptıktan sonra örneği bir `CognitiveServicesCredentials` olan nesne.

```python
subscription_key = "enter-your-key-here"
credentials = CognitiveServicesCredentials(subscription_key)
```

## <a name="create-a-text-analytics-client"></a>Metin analizi istemcisi oluşturma

Yeni bir `TextAnalyticsClient` nesnesi ile `credentials` ve `text_analytics_url` bir parametre olarak. Metin analizi aboneliğiniz için doğru Azure bölgesi kullanın (örneğin `westcentralus`).

```
text_analytics_url = "https://westcentralus.api.cognitive.microsoft.com/"
text_analytics = TextAnalyticsClient(endpoint=text_analytics_url, credentials=credentials)
```

## <a name="sentiment-analysis"></a>Yaklaşım analizi

API yükü bir listesinden oluşur `documents`, hangi olan sözlükleri içeren bir `id` ve `text` özniteliği. `text` Öznitelik depolarının Analiz edilecek metin ve `id` herhangi bir değer olabilir. 

```python
documents = [
  {
    "id": "1", 
    "language": "en", 
    "text": "I had the best day of my life."
  },
  {
    "id": "2", 
    "language": "en", 
    "text": "This was a waste of my time. The speaker put me to sleep."
  },  
  {
    "id": "3", 
    "language": "es", 
    "text": "No tengo dinero ni nada que dar..."
  },  
  {
    "id": "4", 
    "language": "it", 
    "text": "L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."
  }
]
```

Çağrı `sentiment()` işlev ve sonucu alın. Ardından sonuçlarını yinelemek ve her bir belgenin kimliği ve yaklaşım puanını yazdırın. Bir puan yakın 1 pozitif yaklaşımı çağrılırken bir puan yakın 0 bir negatif yaklaşımı gösterir.

```python
response = text_analytics.sentiment(documents=documents)
for document in response.documents:
     print("Document Id: ", document.id, ", Sentiment Score: ", "{:.2f}".format(document.score))
```

### <a name="output"></a>Çıktı

```console
Document Id:  1 , Sentiment Score:  0.87
Document Id:  2 , Sentiment Score:  0.11
Document Id:  3 , Sentiment Score:  0.44
Document Id:  4 , Sentiment Score:  1.00
```

## <a name="language-detection"></a>Dil algılama

Sözlük, her analiz etmek istediğiniz belgeyi içeren bir liste oluşturur. `text` Öznitelik depolarının Analiz edilecek metin ve `id` herhangi bir değer olabilir. 

```python
documents = [
    { 
        'id': '1', 
        'text': 'This is a document written in English.' 
    },
    {
        'id': '2', 
        'text': 'Este es un document escrito en Español.' 
    },
    { 
        'id': '3', 
        'text': '这是一个用中文写的文件' 
    }
]
``` 

Daha önce oluşturulan istemciyi kullanarak, çağrı `detect_language()` ve sonucu alın. Ardından sonuçlarını yinelemek ve her bir belgenin kimliği ve ilk döndürülen dil yazdırın.

```python
response = text_analytics.detect_language(documents=documents)
for document in response.documents:
    print("Document Id: ", document.id , ", Language: ", document.detected_languages[0].name)
```

### <a name="output"></a>Çıktı

```console
Document Id:  1 , Language:  English
Document Id:  2 , Language:  Spanish
Document Id:  3 , Language:  Chinese_Simplified
```

## <a name="entity-recognition"></a>Varlık tanıma

Sözlük, çözümlemek istediğiniz belgeleri içeren bir liste oluşturur. `text` Öznitelik depolarının Analiz edilecek metin ve `id` herhangi bir değer olabilir. 


```python
documents = [
    {
        "id": "1",
        "language": "en", 
        "text": "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800."
    },
    {
        "id": "2",
        "language": "es", 
        "text": "La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle."
    }
]
```

Daha önce oluşturulan istemciyi kullanarak, çağrı `entities()` işlev ve sonucu alın. Ardından sonuçlarını yinelemek ve her bir belgenin kimliği ve içerdiği varlıklar yazdırın.

```python
response = text_analytics.entities(documents=documents)

for document in response.documents:
    print("Document Id: ", document.id)
    print("\tKey Entities:")
    for entity in document.entities:
        print("\t\t", "NAME: ",entity.name, "\tType: ", entity.type, "\tSub-type: ", entity.sub_type)
        for match in entity.matches:
            print("\t\t\tOffset: ", match.offset, "\tLength: ", match.length, "\tScore: ",
                  "{:.2f}".format(match.entity_type_score))
```


### <a name="output"></a>Çıktı

```console
Document Id:  1
    Key Entities:
         NAME:  Microsoft   Type:  Organization     Sub-type:  None
            Offset:  0  Length:  9  Score:  1.00
         NAME:  Bill Gates  Type:  Person   Sub-type:  None
            Offset:  25     Length:  10     Score:  1.00
         NAME:  Paul Allen  Type:  Person   Sub-type:  None
            Offset:  40     Length:  10     Score:  1.00
         NAME:  April 4     Type:  Other    Sub-type:  None
            Offset:  54     Length:  7  Score:  0.80
         NAME:  April 4, 1975   Type:  DateTime     Sub-type:  Date
            Offset:  54     Length:  13     Score:  0.80
         NAME:  BASIC   Type:  Other    Sub-type:  None
            Offset:  89     Length:  5  Score:  0.80
         NAME:  Altair 8800     Type:  Other    Sub-type:  None
            Offset:  116    Length:  11     Score:  0.80
Document Id:  2
    Key Entities:
         NAME:  Microsoft   Type:  Organization     Sub-type:  None
            Offset:  21     Length:  9  Score:  1.00
         NAME:  Redmond (Washington)    Type:  Location     Sub-type:  None
            Offset:  60     Length:  7  Score:  0.99
         NAME:  21 kilómetros   Type:  Quantity     Sub-type:  Dimension
            Offset:  71     Length:  13     Score:  0.80
         NAME:  Seattle     Type:  Location     Sub-type:  None
            Offset:  88     Length:  7  Score:  1.00
```

## <a name="key-phrase-extraction"></a>Anahtar tümcecik ayıklama

Sözlük, çözümlemek istediğiniz belgeleri içeren bir liste oluşturur. `text` Öznitelik depolarının Analiz edilecek metin ve `id` herhangi bir değer olabilir. 


```python
documents = [
    {
        "id": "1", 
        "language": "ja", 
        "text": "猫は幸せ"
    },
    {
        "id": "2", 
        "language": "de", 
        "text": "Fahrt nach Stuttgart und dann zum Hotel zu Fu."
    },
    {
        "id": "3", 
        "language": "en",
        "text": "My cat might need to see a veterinarian."
    },
    {
        "id": "4", 
        "language": "es", 
        "text": "A mi me encanta el fútbol!"
    }
]
```

Daha önce oluşturulan istemciyi kullanarak, çağrı `key_phrases()` işlev ve sonucu alın. Ardından sonuçlarını yinelemek ve her bir belgenin kimliği ve içerdiği anahtar ifadeleri yazdırın.

```python
response = text_analytics.key_phrases(documents=documents)

for document in response.documents:
    print("Document Id: ", document.id)
    print("\tKey Phrases:")
    for phrase in document.key_phrases:
        print("\t\t",phrase)
```

### <a name="output"></a>Çıktı

```console
Document Id:  1
    Phrases:
         幸せ
Document Id:  2
    Phrases:
         Stuttgart
         Hotel
         Fahrt
         Fu
Document Id:  3
    Phrases:
         cat
         veterinarian
Document Id:  4
    Phrases:
         fútbol
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Power BI ile Metin Analizi](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Ayrıca bkz.

* [Metin analizi API'si nedir?](../overview.md)
* [Örnek kullanıcı senaryoları](../text-analytics-user-scenarios.md)
* [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)
