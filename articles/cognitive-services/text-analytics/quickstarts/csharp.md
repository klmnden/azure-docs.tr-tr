---
title: "Hızlı Başlangıç: Kullanarak C# metin analizi API'sini çağırmak için"
titleSuffix: Azure Cognitive Services
description: Metin Analizi API'sini kısa sürede kullanmaya başlamanıza yardımcı olacak bilgi ve kod örnekleri alın.
services: cognitive-services
author: raymondl
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 04/29/2019
ms.author: assafi
ms.openlocfilehash: e7b07472623cc459c31906aeaa6ccfb4388b4b50
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65146098"
---
# <a name="quickstart-using-c-to-call-the-text-analytics-cognitive-service"></a>Hızlı Başlangıç: Kullanarak C# metin analizi Bilişsel hizmetini çağırmak için
<a name="HOLTop"></a>

Metin analizi için SDK'sı ile dil incelemeye başlamak için bu hızlı başlangıçta kullanmak C#. Sırada [metin analizi](//go.microsoft.com/fwlink/?LinkID=759711) çoğu programlama dilleri ile uyumlu REST API, SDK hizmeti uygulamalarınızla tümleştirmek için kolay bir yol sağlar. Bu örnek için kaynak kodu bulunabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/samples/TextAnalytics).

API'lerle ilgili teknik bilgiler için [API tanımları](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7) sayfasını inceleyin.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

Ayrıca kayıt sırasında oluşturulan [uç nokta ve erişim anahtarı](../How-tos/text-analytics-how-to-access-key.md) değerlerine de sahip olmanız gerekir.

> [!Tip]
>  [HTTP uç noktalarını](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9) doğrudan C# ile de çağırabilirsiniz ancak Microsoft.Azure.CognitiveServices.Language SDK'sı hizmeti çağırmayı çok daha kolay hale getirir ve JSON yanıtını seri duruma getirme ve seri durumdan çıkarma konusunda endişelenmenize gerek kalmaz.
>
> Birkaç faydalı bağlantı:
> - [SDK Nuget sayfası](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics)
> - [SDK kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/src/SDKs/CognitiveServices/dataPlane/Language/TextAnalytics)

## <a name="create-the-visual-studio-solution-and-install-the-sdk"></a>Visual Studio çözümü oluşturun ve SDK'sını yükleyin

1. Yeni bir konsol uygulaması (.NET Core) projesi oluşturmak [Visual Studio](https://visualstudio.microsoft.com/vs/).
1. Çözüme sağ tıklayın ve ardından **Çözüm için NuGet Paketlerini Yönet**'e tıklayın.
1. **Gözat** sekmesini seçin ve **Microsoft.Azure.CognitiveServices.Language.TextAnalytics** aratın.

## <a name="authenticate-your-credentials"></a>Kimlik bilgileriniz kimlik doğrulaması

1. Aşağıdaki `using` ana sınıf dosyası deyimleriyle (`Program.cs` varsayılan olarak).

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Net.Http;
    using System.Threading;
    using System.Threading.Tasks;

    using Microsoft.Azure.CognitiveServices.Language.TextAnalytics;
    using Microsoft.Azure.CognitiveServices.Language.TextAnalytics.Models;
    using Microsoft.Rest;
    ```

2. Yeni bir `ApiKeyServiceClientCredentials` kimlik bilgilerini depolamak ve bunları her istek için eklemek için sınıfı.

    ```csharp
    /// <summary>
    /// Allows authentication to the API using a basic apiKey mechanism
    /// </summary>
    class ApiKeyServiceClientCredentials : ServiceClientCredentials
    {
        private readonly string subscriptionKey;

        /// <summary>
        /// Creates a new instance of the ApiKeyServiceClientCredentails class
        /// </summary>
        /// <param name="subscriptionKey">The subscription key to authenticate and authorize as</param>
        public ApiKeyServiceClientCredentials(string subscriptionKey)
        {
            this.subscriptionKey = subscriptionKey;
        }

        /// <summary>
        /// Add the Basic Authentication Header to each outgoing request
        /// </summary>
        /// <param name="request">The outgoing request</param>
        /// <param name="cancellationToken">A token to cancel the operation</param>
        public override Task ProcessHttpRequestAsync(HttpRequestMessage request, CancellationToken cancellationToken)
        {
            if (request == null)
            {
                throw new ArgumentNullException("request");
            }

            request.Headers.Add("Ocp-Apim-Subscription-Key", this.subscriptionKey);
            return base.ProcessHttpRequestAsync(request, cancellationToken);
        }
    }
    ```

3. Güncelleştirme `Program` sınıfı ve metin analizi abonelik anahtarınız için sabit bir üye, diğeri için hizmet uç noktası ekleyin. Metin analizi aboneliğiniz için doğru Azure bölgesi kullanmanız gerektiğini unutmayın.

    ```csharp
    private const string SubscriptionKey = "enter-your-key-here";

    private const string Endpoint = "enter-your-service-endpoint-here"; // For example: "https://westus.api.cognitive.microsoft.com";
    ```
> [!Tip]
> Üretim sistemlerine gizli dizileri güvenli dağıtımı için kullanmanızı öneririz [Azure anahtar kasası](https://docs.microsoft.com/azure/key-vault/quick-create-net)
>

## <a name="create-a-text-analytics-client"></a>Metin analizi istemcisi oluşturma

İçinde `Main` işlevi, projenin çağırmak ve iletmek istediğiniz örnek yöntemi çağırın `Endpoint` ve `SubscriptionKey` tanımladığınız parametreleri.

```csharp
    public static void Main(string[] args)
    {
        var credentials = new ApiKeyServiceClientCredentials(SubscriptionKey);
        var client = new TextAnalyticsClient(credentials)
        {
            //Replace 'westus' with the correct region for your Text Analytics subscription
            Endpoint = "https://westus.api.cognitive.microsoft.com"
        };

        // Change console encoding to display non-ASCII characters
        Console.OutputEncoding = System.Text.Encoding.UTF8;
        SentimentAnalysisExample(client).Wait();
        // DetectLanguageExample(client).Wait();
        // RecognizeEntitiesExample(client).Wait();
        // KeyPhraseExtractionExample(client).Wait();
        Console.ReadLine();
    }
```

Sonraki bölümlerde, her API çağırmak nasıl açıklar.

## <a name="sentiment-analysis"></a>Yaklaşım analizi

1. Adlı yeni bir işlev oluşturma `SentimentAnalysisExample()` daha önce oluşturduğunuz istemci alır.
2. Yeni bir `TextAnalyticsClient` nesnesi ile `ApiKeyServiceClientCredentials` bir parametre olarak ve ardından listesi oluşturmak `MultiLanguageInput` çözümlemek istediğiniz belgeleri içeren nesne.

    ```csharp
    public static async Task SentimentAnalysisExample(string endpoint, string key)
    {
        var credentials = new ApiKeyServiceClientCredentials(key);
        var client = new TextAnalyticsClient(credentials)
        {
            Endpoint = endpoint
        };

        // The documents to be analyzed. Add the language of the document. The ID can be any value.
        var inputDocuments = new MultiLanguageBatchInput(
            new List<MultiLanguageInput>
            {
                new MultiLanguageInput("en", "1", "I had the best day of my life."),
                new MultiLanguageInput("en", "2", "This was a waste of my time. The speaker put me to sleep."),
                new MultiLanguageInput("es", "3", "No tengo dinero ni nada que dar..."),
                new MultiLanguageInput("it", "4", "L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."),
            });
        //...
    }
    ```

3. Aynı işlev çağrısında `client.SentimentAsync()` ve sonucu alın. Ardından sonuçlarını yinelemek ve her bir belgenin kimliği ve yaklaşım puanını yazdırın. Bir puan yakın 1 pozitif yaklaşımı çağrılırken bir puan yakın 0 bir negatif yaklaşımı gösterir.

    ```csharp
    var result = await client.SentimentAsync(false, inputDocuments);

    // Printing sentiment results
    foreach (var document in result.Documents)
    {
        Console.WriteLine($"Document ID: {document.Id} , Sentiment Score: {document.Score:0.00}");
    }
    ```

### <a name="output"></a>Çıktı

```console
Document ID: 1 , Sentiment Score: 0.87
Document ID: 2 , Sentiment Score: 0.11
Document ID: 3 , Sentiment Score: 0.44
Document ID: 4 , Sentiment Score: 1.00
```

## <a name="language-detection"></a>Dil algılama

1. Adlı yeni bir işlev oluşturma `DetectLanguageExample()` daha önce oluşturduğunuz istemci alır.
2. Yeni bir `TextAnalyticsClient` nesnesi ile `ApiKeyServiceClientCredentials` bir parametre olarak ve ardından listesi oluşturmak `LanguageInput` belgelerinizi içeren nesne.

    ```csharp
    public static async Task DetectLanguageExample(string endpoint, string key)
    {
        var credentials = new ApiKeyServiceClientCredentials(key);
        var client = new TextAnalyticsClient(credentials)
        {
            Endpoint = endpoint
        };

        // The documents to be submitted for language detection. The ID can be any value.
        var inputDocuments = new LanguageBatchInput(
                new List<LanguageInput>
                    {
                        new LanguageInput(id: "1", text: "This is a document written in English."),
                        new LanguageInput(id: "2", text: "Este es un document escrito en Español."),
                        new LanguageInput(id: "3", text: "这是一个用中文写的文件")
                    });
        //...
    }
    ```

3. Aynı işlev çağrısında `client.DetectLanguageAsync()` ve sonucu alın. Ardından sonuçlarını yinelemek ve her bir belgenin kimliği ve ilk döndürülen dil yazdırın.

    ```csharp
    var langResults = await client.DetectLanguageAsync(false, inputDocuments);

    // Printing detected languages
    foreach (var document in langResults.Documents)
    {
        Console.WriteLine($"Document ID: {document.Id} , Language: {document.DetectedLanguages[0].Name}");
    }
    ```

### <a name="output"></a>Çıktı

```console
===== LANGUAGE EXTRACTION ======
Document ID: 1 , Language: English
Document ID: 2 , Language: Spanish
Document ID: 3 , Language: Chinese_Simplified
```

## <a name="entity-recognition"></a>Varlık tanıma

1. Adlı yeni bir işlev oluşturma `RecognizeEntitiesExample()` daha önce oluşturduğunuz istemci alır.
2. Yeni bir `TextAnalyticsClient` nesnesi ile `ApiKeyServiceClientCredentials` bir parametre olarak ve ardından listesi oluşturmak `MultiLanguageBatchInput` belgelerinizi içeren nesne.

    ```csharp
    public static async Task RecognizeEntitiesExample(string endpoint, string key)
    {
        var credentials = new ApiKeyServiceClientCredentials(key);
        var client = new TextAnalyticsClient(credentials)
        {
            Endpoint = endpoint
        };

        // The documents to be submitted for entity recognition. The ID can be any value.
        var inputDocuments = new MultiLanguageBatchInput(
            new List<MultiLanguageInput>
            {
                new MultiLanguageInput("en", "1", "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800."),
                new MultiLanguageInput("es", "2", "La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle.")
            });
        //...
    }
    ```

3. Aynı işlev çağrısında `client.EntitiesAsync()` ve sonucu alın. Ardından sonuçlarını yinelemek ve her bir belgenin kimliği yazdırma Her varlık algılanan için onun wikipedia adını, türünü ve alt türleri yazdırma (varsa var) orijinal metni konumlarda yanı sıra.

    ```csharp
    var entitiesResult = await client.EntitiesAsync(false, inputDocuments);

    // Printing recognized entities
    foreach (var document in entitiesResult.Documents)
    {
        Console.WriteLine($"Document ID: {document.Id} ");

        Console.WriteLine("\t Entities:");
        foreach (var entity in document.Entities)
        {
            Console.WriteLine($"\t\tName: {entity.Name},\tType: {entity.Type ?? "N/A"},\tSub-Type: {entity.SubType ?? "N/A"}");
            foreach (var match in entity.Matches)
            {
                Console.WriteLine($"\t\t\tOffset: {match.Offset},\tLength: {match.Length},\tScore: {match.EntityTypeScore:F3}");
            }
        }
    }
    ```

### <a name="output"></a>Çıktı

```console
Document ID: 1
         Entities:
                Name: Microsoft,        Type: Organization,     Sub-Type: N/A
                        Offset: 0,      Length: 9,      Score: 1.000
                Name: Bill Gates,       Type: Person,   Sub-Type: N/A
                        Offset: 25,     Length: 10,     Score: 1.000
                Name: Paul Allen,       Type: Person,   Sub-Type: N/A
                        Offset: 40,     Length: 10,     Score: 0.999
                Name: April 4,  Type: Other,    Sub-Type: N/A
                        Offset: 54,     Length: 7,      Score: 0.800
                Name: April 4, 1975,    Type: DateTime, Sub-Type: Date
                        Offset: 54,     Length: 13,     Score: 0.800
                Name: BASIC,    Type: Other,    Sub-Type: N/A
                        Offset: 89,     Length: 5,      Score: 0.800
                Name: Altair 8800,      Type: Other,    Sub-Type: N/A
                        Offset: 116,    Length: 11,     Score: 0.800
Document ID: 2
         Entities:
                Name: Microsoft,        Type: Organization,     Sub-Type: N/A
                        Offset: 21,     Length: 9,      Score: 1.000
                Name: Redmond (Washington),     Type: Location, Sub-Type: N/A
                        Offset: 60,     Length: 7,      Score: 0.991
                Name: 21 kilómetros,    Type: Quantity, Sub-Type: Dimension
                        Offset: 71,     Length: 13,     Score: 0.800
                Name: Seattle,  Type: Location, Sub-Type: N/A
                        Offset: 88,     Length: 7,      Score: 1.000
```

## <a name="key-phrase-extraction"></a>Anahtar tümcecik ayıklama

1. Adlı yeni bir işlev oluşturma `KeyPhraseExtractionExample()` daha önce oluşturduğunuz istemci alır.
2. Yeni bir `TextAnalyticsClient` nesnesi ile `ApiKeyServiceClientCredentials` bir parametre olarak ve ardından listesi oluşturmak `MultiLanguageBatchInput` belgelerinizi içeren nesne.

    ```csharp
    public static async Task KeyPhraseExtractionExample(string endpoint, string key)
    {
        var credentials = new ApiKeyServiceClientCredentials(key);
        var client = new TextAnalyticsClient(credentials)
        {
            Endpoint = endpoint
        };

        var inputDocuments = new MultiLanguageBatchInput(
                    new List<MultiLanguageInput>
                    {
                        new MultiLanguageInput("ja", "1", "猫は幸せ"),
                        new MultiLanguageInput("de", "2", "Fahrt nach Stuttgart und dann zum Hotel zu Fu."),
                        new MultiLanguageInput("en", "3", "My cat might need to see a veterinarian."),
                        new MultiLanguageInput("es", "4", "A mi me encanta el fútbol!")
                    });
        //...
    }
    ```

3. Aynı işlev çağrısında `client.KeyPhrasesAsync()` ve sonucu alın. Ardından sonuçlarını yinelemek ve her bir belgenin kimliği ve algılanan tüm anahtar ifadeleri yazdırın.

    ```csharp
    var kpResults = await client.KeyPhrasesAsync(false, inputDocuments);

    // Printing keyphrases
    foreach (var document in kpResults.Documents)
    {
        Console.WriteLine($"Document ID: {document.Id} ");

        Console.WriteLine("\t Key phrases:");

        foreach (string keyphrase in document.KeyPhrases)
        {
            Console.WriteLine($"\t\t{keyphrase}");
        }
    }
    ```

### <a name="output"></a>Çıktı

```console
Document ID: 1
         Key phrases:
                幸せ
Document ID: 2
         Key phrases:
                Stuttgart
                Hotel
                Fahrt
                Fu
Document ID: 3
         Key phrases:
                cat
                veterinarian
Document ID: 4
         Key phrases:
                fútbol
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Power BI ile Metin Analizi](../tutorials/tutorial-power-bi-key-phrases.md)


* [Metin Analizine genel bakış](../overview.md)
* [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)

