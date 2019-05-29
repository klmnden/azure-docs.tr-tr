---
title: "Hızlı Başlangıç: Go SDK'sı kullanarak metin analizi hizmeti çağırma"
titleSuffix: Azure Cognitive Services
description: Microsoft Bilişsel hizmetler metin analizi API'sini kullanarak hızlı bir şekilde yardımcı olmak için bilgi ve kod örnekleri get başlayın.
services: cognitive-services
author: laramume
manager: assafi
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 05/23/2019
ms.author: aahi
ms.openlocfilehash: 44def29292bc882fdaa08ff76667742756f178b8
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66299614"
---
# <a name="quickstart-call-the-text-analytics-service-using-the-go-sdk"></a>Hızlı Başlangıç: Go SDK'sı kullanarak metin analizi hizmeti çağırma 
<a name="HOLTop"></a>

Bu Hızlı Başlangıç, Go için metin analizi SDK'sı ile dil incelemeye başlamak için kullanın. Bu makalede, dili algılayın, yaklaşımı analiz edin, anahtar tümcecikleri ayıklayın ve bağlı varlıkları tanımlama işlemini göstermektedir. SDK, REST API ile çoğu programlama dilinden uyumlu olsa da, hizmet uygulamalarınızla tümleştirmek için kolay bir yol sağlar. Bu örnek için kaynak kodu bulunabilir [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/cognitiveservices).

## <a name="prerequisites"></a>Önkoşullar

* Metin analizi [için Go SDK'sı](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v2.1/textanalytics)

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

Ayrıca kayıt sırasında oluşturulan [uç nokta ve erişim anahtarı](../How-tos/text-analytics-how-to-access-key.md) değerlerine de sahip olmanız gerekir.

## <a name="set-up-a-new-project"></a>Yeni bir projesi kurun

Yeni bir Git proje, sık kullandığınız kod düzenleyicimizi veya IDE'mizi oluşturun. Ardından Git dosyasına aşağıdaki içeri aktarma deyimini ekleyin.

```golang
import (
    "context"
    "encoding/json"
    "fmt"
    "github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics"
    "github.com/Azure/go-autorest/autorest"
)
```

Çoğu parametreleri ve özellikleri için bu hızlı başlangıçta, dize ve Boole işaretçileri beklediğiniz gibi projenize aşağıdaki işlevleri ekleyin.

```golang
// returns a pointer to the string value passed in.
func StringPointer(v string) *string {
    return &v
}

// returns a pointer to the bool value passed in.
func BoolPointer(v bool) *bool {
    return &v
}
```

## <a name="create-text-analytics-client-and-authenticate-credentials"></a>Metin analizi istemci oluşturmanız ve kimlik

Projenizin ana işlevinde yeni bir oluşturma `TextAnalytics` nesne. Metin analizi aboneliğiniz için doğru Azure bölgesi kullanın. Örneğin: `https://eastus.api.cognitive.microsoft.com`. Bir deneme sürümü anahtarı kullanıyorsanız, güncelleştirme konumu gerekmez.

```golang
//Replace 'eastus' with the correct region for your Text Analytics subscription
textAnalyticsClient := textanalytics.New("https://eastus.api.cognitive.microsoft.com")
```

Anahtarınız için bir değişken oluşturun ve işlevine geçirebileceğimiz `autorest.NewCognitiveServicesAuthorizer` , ardından geçirilir istemcinin `authorizer` özelliği.

```golang
subscriptionKey := "<<subscriptionKey>>"
textAnalyticsClient.Authorizer = autorest.NewCognitiveServicesAuthorizer(subscriptionKey)
```

## <a name="sentiment-analysis"></a>Yaklaşım analizi

Adlı yeni bir işlev oluşturma `SentimentAnalysis()` daha önce oluşturduğunuz istemci alır. Bir liste oluşturur `MultiLanguageInput` çözümlemek istediğiniz belgeleri içeren nesne. Her nesne içerecek bir `id`, `Language` ve `text` özniteliği. `text` Öznitelik depolarının Analiz edilecek metnin `language` belge dilidir ve `id` herhangi bir değer olabilir. 

```golang
func SentimentAnalysis(textAnalyticsclient textanalytics.BaseClient) {

    ctx := context.Background()
    inputDocuments := []textanalytics.MultiLanguageInput {
        textanalytics.MultiLanguageInput {
            Language: StringPointer("en"),
            ID:StringPointer("0"),
            Text:StringPointer("I had the best day of my life."),
        },
        textanalytics.MultiLanguageInput {
            Language: StringPointer("en"),
            ID:StringPointer("1"),
            Text:StringPointer("This was a waste of my time. The speaker put me to sleep."),
        },
        textanalytics.MultiLanguageInput {
            Language: StringPointer("es"),
            ID:StringPointer("2"),
            Text:StringPointer("No tengo dinero ni nada que dar..."),
        },
        textanalytics.MultiLanguageInput {
            Language: StringPointer("it"),
            ID:StringPointer("3"),
            Text:StringPointer("L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."),
        },
    }

    batchInput := textanalytics.MultiLanguageBatchInput{Documents:&inputDocuments}
}
```

Aynı işlev çağrısında `textAnalyticsclient.Sentiment()` ve sonucu alın. Ardından sonuçlarını yinelemek ve her bir belgenin kimliği ve yaklaşım puanını yazdırın. Bir puan yakın 1 pozitif yaklaşımı çağrılırken bir puan yakın 0 bir negatif yaklaşımı gösterir.

```golang
result, _ := textAnalyticsclient.Sentiment(ctx, BoolPointer(false), &batchInput)
batchResult := textanalytics.SentimentBatchResult{}
jsonString, _ := json.Marshal(result.Value)
json.Unmarshal(jsonString, &batchResult)

// Printing sentiment results
for _,document := range *batchResult.Documents {
    fmt.Printf("Document ID: %s " , *document.ID)
    fmt.Printf("Sentiment Score: %f\n",*document.Score)
}

// Printing document errors
fmt.Println("Document Errors")
for _,error := range *batchResult.Errors {
    fmt.Printf("Document ID: %s Message : %s\n" ,*error.ID, *error.Message)
}
```

Projenizin ana işlev çağrısında `SentimentAnalysis()`.

### <a name="output"></a>Çıktı

```console
Document ID: 1 , Sentiment Score: 0.87
Document ID: 2 , Sentiment Score: 0.11
Document ID: 3 , Sentiment Score: 0.44
Document ID: 4 , Sentiment Score: 1.00
```

## <a name="language-detection"></a>Dil algılama

Adlı yeni bir işlev oluşturma `LanguageDetection()` daha önce oluşturduğunuz istemci alır. Bir liste oluşturur `LanguageInput` çözümlemek istediğiniz belgeleri içeren nesne. Her nesne içerecek bir `id` ve `text` özniteliği. `text` Öznitelik depolarının Analiz edilecek metin ve `id` herhangi bir değer olabilir. 

```golang
func LanguageDetection(textAnalyticsclient textanalytics.BaseClient) {

    ctx := context.Background()
    inputDocuments := []textanalytics.LanguageInput {
        textanalytics.LanguageInput {
            ID:StringPointer("0"),
            Text:StringPointer("This is a document written in English."),
        },
        textanalytics.LanguageInput {
            ID:StringPointer("1"),
            Text:StringPointer("Este es un document escrito en Español."),
        },
        textanalytics.LanguageInput {
            ID:StringPointer("2"),
            Text:StringPointer("这是一个用中文写的文件"),
        },
    }

    batchInput := textanalytics.LanguageBatchInput{Documents:&inputDocuments}
}
```

Aynı işlev çağrısında `textAnalyticsclient.DetectLanguage()` ve sonucu alın. Ardından sonuçlarını yinelemek ve her bir belgenin kimliği ve algılanan dilin yazdırın.

```golang
result, _ := textAnalyticsclient.DetectLanguage(ctx, BoolPointer(false), &batchInput)

// Printing language detection results
for _,document := range *result.Documents {
    fmt.Printf("Document ID: %s " , *document.ID)
    fmt.Printf("Detected Languages with Score: ")
    for _,language := range *document.DetectedLanguages{
        fmt.Printf("%s %f,",*language.Name, *language.Score)
    }
    fmt.Println()
}

// Printing document errors
fmt.Println("Document Errors")
for _,error := range *result.Errors {
    fmt.Printf("Document ID: %s Message : %s\n" ,*error.ID, *error.Message)
}
```

Projenizin ana işlev çağrısında `LanguageDetection()`.

### <a name="output"></a>Çıktı

```console
Document ID: 0 Detected Languages with Score: English 1.000000,
Document ID: 1 Detected Languages with Score: Spanish 1.000000,
Document ID: 2 Detected Languages with Score: Chinese_Simplified 1.000000,
```

## <a name="entity-recognition"></a>Varlık tanıma

Adlı yeni bir işlev oluşturma `ExtractEntities()` daha önce oluşturduğunuz istemci alır. Bir liste oluşturur `MultiLanguageInput` çözümlemek istediğiniz belgeleri içeren nesne. Her nesne içerecek bir `id`, `language`ve `text` özniteliği. `text` Öznitelik depolarının Analiz edilecek metnin `language` belge dilidir ve `id` herhangi bir değer olabilir. 

```golang
func ExtractKeyPhrases(textAnalyticsclient textanalytics.BaseClient) {

    ctx := context.Background()
    inputDocuments := []textanalytics.MultiLanguageInput {
        textanalytics.MultiLanguageInput {
            Language: StringPointer("en"),
            ID:StringPointer("0"),
            Text:StringPointer("Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800."),
        },
        textanalytics.MultiLanguageInput {
            Language: StringPointer("es"),
            ID:StringPointer("1"),
            Text:StringPointer("La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle."),
        },
    }

    batchInput := textanalytics.MultiLanguageBatchInput{Documents:&inputDocuments}
}
```

Aynı işlevde `call textAnalyticsclient.Entities()` ve sonucu alın. Ardından sonuçlarını ve her bir belgeyi kullanıcının kimliği ve ayıklanan yazdırma aracılığıyla varlıkları puan gezin.

```golang
    result, _ := textAnalyticsclient.Entities(ctx, BoolPointer(false), &batchInput)

    // Printing extracted entities results
    for _,document := range *result.Documents {
        fmt.Printf("Document ID: %s\n" , *document.ID)
        fmt.Printf("\tExtracted Entities:\n")
        for _,entity := range *document.Entities{
            fmt.Printf("\t\tName: %s\tType: %s",*entity.Name, *entity.Type)
            if entity.SubType != nil{
                fmt.Printf("\tSub-Type: %s\n", *entity.SubType)
            }
            fmt.Println()
            for _,match := range *entity.Matches{
                fmt.Printf("\t\t\tOffset: %v\tLength: %v\tScore: %f\n", *match.Offset, *match.Length, *match.EntityTypeScore)
            }
        }
        fmt.Println()
    }

    // Printing document errors
    fmt.Println("Document Errors")
    for _,error := range *result.Errors {
        fmt.Printf("Document ID: %s Message : %s\n" ,*error.ID, *error.Message)
    }
```

Projenizin ana işlev çağrısında `ExtractEntities()`.

### <a name="output"></a>Çıktı

```console
Document ID: 0
    Extracted Entities:
        Name: Microsoft Type: Organization
            Offset: 0   Length: 9   Score: 1.000000
        Name: Bill Gates    Type: Person
            Offset: 25  Length: 10  Score: 0.999847
        Name: Paul Allen    Type: Person
            Offset: 40  Length: 10  Score: 0.998841
        Name: April 4   Type: Other
            Offset: 54  Length: 7   Score: 0.800000
        Name: April 4, 1975 Type: DateTime  Sub-Type: Date

            Offset: 54  Length: 13  Score: 0.800000
        Name: BASIC Type: Other
            Offset: 89  Length: 5   Score: 0.800000
        Name: Altair 8800   Type: Other
            Offset: 116 Length: 11  Score: 0.800000

Document ID: 1
    Extracted Entities:
        Name: Microsoft Type: Organization
            Offset: 21  Length: 9   Score: 0.999756
        Name: Redmond (Washington)  Type: Location
            Offset: 60  Length: 7   Score: 0.991128
        Name: 21 kilómetros Type: Quantity  Sub-Type: Dimension

            Offset: 71  Length: 13  Score: 0.800000
        Name: Seattle   Type: Location
            Offset: 88  Length: 7   Score: 0.999878
```

## <a name="key-phrase-extraction"></a>Anahtar tümcecik ayıklama

Adlı yeni bir işlev oluşturma `ExtractKeyPhrases()` daha önce oluşturduğunuz istemci alır. Bir liste oluşturur `MultiLanguageInput` çözümlemek istediğiniz belgeleri içeren nesne. Her nesne içerecek bir `id`, `language`ve `text` özniteliği. `text` Öznitelik depolarının Analiz edilecek metnin `language` belge dilidir ve `id` herhangi bir değer olabilir.

```golang
func ExtractKeyPhrases(textAnalyticsclient textanalytics.BaseClient) {

    ctx := context.Background()
    inputDocuments := []textanalytics.MultiLanguageInput {
        textanalytics.MultiLanguageInput {
            Language: StringPointer("ja"),
            ID:StringPointer("0"),
            Text:StringPointer("猫は幸せ"),
        },
        textanalytics.MultiLanguageInput {
            Language: StringPointer("de"),
            ID:StringPointer("1"),
            Text:StringPointer("Fahrt nach Stuttgart und dann zum Hotel zu Fu."),
        },
        textanalytics.MultiLanguageInput {
            Language: StringPointer("en"),
            ID:StringPointer("2"),
            Text:StringPointer("My cat might need to see a veterinarian."),
        },
        textanalytics.MultiLanguageInput {
            Language: StringPointer("es"),
            ID:StringPointer("3"),
            Text:StringPointer("A mi me encanta el fútbol!"),
        },
    }

    batchInput := textanalytics.MultiLanguageBatchInput{Documents:&inputDocuments}
}
```

Aynı işlevde textAnalyticsclient.KeyPhrases() çağırabilir ve sonucu alın. Ardından sonuçlarını yinelemek ve her bir belgenin kimliği ve ayıklanan anahtar ifadeleri yazdırın.

```golang
    result, _ := textAnalyticsclient.KeyPhrases(ctx, BoolPointer(false), &batchInput)

    // Printing extracted key phrases results
    for _,document := range *result.Documents {
        fmt.Printf("Document ID: %s\n" , *document.ID)
        fmt.Printf("\tExtracted Key Phrases:\n")
        for _,keyPhrase := range *document.KeyPhrases{
            fmt.Printf("\t\t%s\n",keyPhrase)
        }
        fmt.Println()
    }

    // Printing document errors
    fmt.Println("Document Errors")
    for _,error := range *result.Errors {
        fmt.Printf("Document ID: %s Message : %s\n" ,*error.ID, *error.Message)
    }
```

Projenizin ana işlev çağrısında `ExtractKeyPhrases()`.

### <a name="output"></a>Çıktı

```console
Document ID: 0
    Extracted Key Phrases:
        幸せ

Document ID: 1
    Extracted Key Phrases:
        Stuttgart
        Hotel
        Fahrt
        Fu

Document ID: 2
    Extracted Key Phrases:
        cat
        veterinarian

Document ID: 3
    Extracted Key Phrases:
        fútbol
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Power BI ile Metin Analizi](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Ayrıca bkz.

 [Metin Analizi'ne genel bakış](../overview.md) [sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)
