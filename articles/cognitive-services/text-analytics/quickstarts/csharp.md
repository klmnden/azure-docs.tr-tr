---
title: "Hızlı Başlangıç: .NET için Azure SDK'sını kullanarak metin analizi hizmeti çağırmak veC#"
titleSuffix: Azure Cognitive Services
description: Metin analizi hizmeti kullanmaya başlamanıza yardımcı olacak bilgiler ve kod örnekleri ve C#.
services: cognitive-services
author: raymondl
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 05/24/2019
ms.author: assafi
ms.openlocfilehash: 4b0f4c4768c68e2fa58fb0577d0586095c6fb716
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66256327"
---
# <a name="quickstart-use-the-net-sdk-and-c-to-call-the-text-analytics-service"></a>Hızlı Başlangıç: .NET SDK'yı kullanın ve C# metin analizi hizmeti çağırmak için
<a name="HOLTop"></a>

Bu hızlı başlangıçta, .NET için Azure SDK'sını kullanmaya başlamak yardımcı olur ve C# dil analiz etmek için. Ancak [metin analizi](//go.microsoft.com/fwlink/?LinkID=759711) çoğu programlama dilleri ile uyumlu REST API, SDK hizmeti uygulamalarınızla tümleştirmek için kolay bir yol sağlar.

> [!NOTE]
> Bu örneğin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/samples/TextAnalytics)’da mevcuttur.

Teknik Ayrıntılar için .NET için SDK'sına başvurun [metin analizi başvuru](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/textanalytics?view=azure-dotnet).

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

Ayrıca gerekir [uç noktası ve erişim anahtarı](../How-tos/text-analytics-how-to-access-key.md) oluşturulan, kayıt sırasında.

> [!Tip]
>  Çağırabilirsiniz sırada [HTTP uç noktalarını](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9) doğrudan C#, Microsoft.Azure.CognitiveServices.Language SDK'sı, JSON seri hale getrime ve gerek kalmadan hizmet çağrısı çok daha kolay kılar.
>
> Birkaç faydalı bağlantı:
> - [SDK'sı NuGet sayfası](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics)
> - [SDK kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/src/SDKs/CognitiveServices/dataPlane/Language/TextAnalytics)

## <a name="create-the-visual-studio-solution-and-install-the-sdk"></a>Visual Studio çözümü oluşturun ve SDK'sını yükleyin

1. Yeni bir konsol uygulaması (.NET Core) projesi oluşturun. [Visual Studio erişim](https://visualstudio.microsoft.com/vs/).
1. Çözüme sağ tıklayıp **çözüm için NuGet paketlerini Yönet**.
1. **Gözat** sekmesini seçin. Arama **Microsoft.Azure.CognitiveServices.Language.TextAnalytics**.

## <a name="authenticate-your-credentials"></a>Kimlik bilgileriniz kimlik doğrulaması

1. Aşağıdaki `using` deyimleri ana sınıfı dosyasına (Program.cs varsayılan olarak olan).

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
    /// Allows authentication to the API by using a basic apiKey mechanism
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

3. Güncelleştirme `Program` sınıfı. Metin analizi abonelik anahtarınız için sabit bir üye ve başka bir hizmet uç noktası ekleyin. Metin analizi aboneliğiniz için doğru Azure bölgesi kullanmanız gerektiğini unutmayın.

    ```csharp
    private const string SubscriptionKey = "enter-your-key-here";

    private const string Endpoint = "enter-your-service-endpoint-here"; // For example: "https://westus.api.cognitive.microsoft.com";
    ```
> [!Tip]
> Üretim sistemleri gizli dizileri güvenliğini artırmak için kullanmanızı öneririz [Azure anahtar kasası](https://docs.microsoft.com/azure/key-vault/quick-create-net).
>

## <a name="create-a-text-analytics-client"></a>Metin analizi istemcisi oluşturma

İçinde `Main` işlevi projenizi çağırmak istediğiniz örnek yöntemi çağırın. Geçirmek `Endpoint` ve `SubscriptionKey` tanımladığınız parametreleri.

```csharp
    public static void Main(string[] args)
    {
        var credentials = new ApiKeyServiceClientCredentials(SubscriptionKey);
        var client = new TextAnalyticsClient(credentials)
        {
            Endpoint = Endpoint
        };

        // Change the console encoding to display non-ASCII characters.
        Console.OutputEncoding = System.Text.Encoding.UTF8;
        SentimentAnalysisExample(client).Wait();
        // DetectLanguageExample(client).Wait();
        // RecognizeEntitiesExample(client).Wait();
        // KeyPhraseExtractionExample(client).Wait();
        Console.ReadLine();
    }
```

Aşağıdaki bölümlerde her hizmeti özelliği çağırmak nasıl açıklanmaktadır.

## <a name="perform-sentiment-analysis"></a>Yaklaşım analizi gerçekleştirme

1. Yeni bir işlev oluşturma `SentimentAnalysisExample()` daha önce oluşturduğunuz istemci alır.
2. Listesini oluşturmak `MultiLanguageInput` çözümlemek istediğiniz belge içeren nesne.

    ```csharp
    public static async Task SentimentAnalysisExample(TextAnalyticsClient client)
    {
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

3. Aynı işlev çağrısında `client.SentimentAsync()` ve sonucu alın. Ardından sonuçlarını yinelemek. Her bir belgenin kimliği ve yaklaşım puanı yazdırın. 1'e yakın bir puan pozitif yaklaşımı çağrılırken 0 yakın bir puan bir negatif yaklaşımı gösterir.

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

## <a name="perform-language-detection"></a>Dil algılama

1. Yeni bir işlev oluşturma `DetectLanguageExample()` daha önce oluşturduğunuz istemci alır.
2. Listesini oluşturmak `LanguageInput` belgelerinizi içeren nesne.

    ```csharp
    public static async Task DetectLanguageExample(TextAnalyticsClient client)
    {

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

3. Aynı işlev çağrısında `client.DetectLanguageAsync()` ve sonucu alın. Ardından sonuçlarını yinelemek. Her bir belgenin kimliği ve ilk döndürülen dil yazdırın.

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

## <a name="perform-entity-recognition"></a>Varlık tanıma gerçekleştirin

1. Yeni bir işlev oluşturma `RecognizeEntitiesExample()` daha önce oluşturduğunuz istemci alır.
2. Listesini oluşturmak `MultiLanguageBatchInput` belgelerinizi içeren nesne.

    ```csharp
    public static async Task RecognizeEntitiesExample(TextAnalyticsClient client)
    {

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

3. Aynı işlev çağrısında `client.EntitiesAsync()` ve sonucu alın. Ardından sonuçlarını yinelemek. Her bir belgenin kimliği yazdırma (Varsa) için algılanan her varlık, orijinal metni konumlarda yanı sıra Wikipedia adını ve türünü ve subtypes yazdırın.

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

## <a name="perform-key-phrase-extraction"></a>Anahtar ifade ayıklama gerçekleştirin

1. Yeni bir işlev oluşturma `KeyPhraseExtractionExample()` daha önce oluşturduğunuz istemci alır.
2. Listesini oluşturmak `MultiLanguageBatchInput` belgelerinizi içeren nesne.

    ```csharp
    public static async Task KeyPhraseExtractionExample(TextAnalyticsClient client)
    {
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

3. Aynı işlev çağrısında `client.KeyPhrasesAsync()` ve sonucu alın. Ardından sonuçlarını yinelemek. Her bir belgenin kimliği ve algılanan tüm anahtar ifadeleri yazdırın.

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
