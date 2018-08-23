---
title: Bilişsel hizmetler metin analizi API'si için C# hızlı başlangıç | Microsoft Docs
description: Bilgi ve kod örnekleri, Azure üzerinde Microsoft Bilişsel hizmetler metin analizi API'sini kullanarak ile hızlıca çalışmaya başlamanıza yardımcı olmak için alın.
services: cognitive-services
documentationcenter: ''
author: luiscabrer
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 09/20/2017
ms.author: ashmaka
ms.openlocfilehash: 4bf5179ade6f49b847b8b674d33652071e19a769
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2018
ms.locfileid: "41987628"
---
# <a name="quickstart-for-the-text-analytics-api-with-c"></a>Metin analizi API'si ile C# için hızlı başlangıç 
<a name="HOLTop"></a>

Bu makalede, dili algılayın, duyguları çözümleyin ve kullanarak, anahtar tümcecikleri ayıklayın işlemini göstermektedir [metin analizi API'lerini](//go.microsoft.com/fwlink/?LinkID=759711) C# ile. Kodu en az başvurular dış kitaplıkları, bir .NET Core uygulaması ayrıca Linux veya Macos'ta çalıştırabilirsiniz şekilde çalışmak için yazılmıştır.

Başvurmak [API tanımlarını](//go.microsoft.com/fwlink/?LinkID=759346) API'leri için teknik belgeler için.

## <a name="prerequisites"></a>Önkoşullar

Olmalıdır bir [Bilişsel hizmetler API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) metin analizi API'si ile. Kullanabileceğiniz *5.000 işlem/ay için ücretsiz katman* Bu hızlı başlangıcı tamamlamak için.

Sahip olmalısınız [uç noktası ve erişim anahtarı](../How-tos/text-analytics-how-to-access-key.md) , oluşturuldu, kayıt sırasında. 


## <a name="install-the-nuget-sdk-package"></a>SDK'sı NuGet paketini yükle
1. Visual Studio'da yeni bir konsol çözümü oluşturun.
1. Çözüme sağ tıklayıp **çözüm için NuGet paketlerini Yönet**.
1. Seçin **öncesini** onay kutusu.
1. Seçin **Gözat** sekmesinde ve arama **Microsoft.Azure.CognitiveServices.Language**.
1. NuGet paketini seçin ve yükleyin.

> [!Tip]
> Çağırabilirsiniz rağmen [HTTP uç noktalarını](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) doğrudan C# ' tan, Microsoft.Azure.CognitiveServices.Language SDK'sı sağlar serileştirme ve JSON seri kaldırma hakkında endişelenmenize gerek kalmadan hizmet çağrısı çok daha kolay.
>
> Faydalı bağlantılar aşağıda verilmiştir:
> - [SDK'sı NuGet sayfası](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics)
> - [SDK kodu](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/CognitiveServices/dataPlane/Language/TextAnalytics)


## <a name="call-the-text-analytics-api-by-using-the-sdk"></a>SDK'sını kullanarak metin analizi API'sini çağırma
1. Program.cs aşağıdaki kodla değiştirin. Bu program, metin analizi API'si üç bölümlük (dil ayıklama, anahtar ifade ayıklama ve yaklaşım analizi) özellikleri gösterilmektedir.
1. Değiştirin `Ocp-Apim-Subscription-Key` aboneliğiniz için geçerli olan bir erişim anahtarı ile üst bilgi değeri.
1. Konumu değiştirmek `Endpoint` kaydolup uç noktası. Uç nokta, Azure portal kaynağında bulabilirsiniz. Uç nokta, genellikle "https://[region].api.cognitive.microsoft.com" ile başlar Yalnızca protokolü ve ana bilgisayar adını içerir.
1. Programı çalıştırın.

```csharp
using System;
using Microsoft.Azure.CognitiveServices.Language.TextAnalytics;
using Microsoft.Azure.CognitiveServices.Language.TextAnalytics.Models;
using System.Collections.Generic;
using Microsoft.Rest;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;

namespace ConsoleApp1
{
    class Program
    {
        /// <summary>
        /// Container for subscription credentials. Make sure to enter your valid key.
        /// </summary>
        class ApiKeyServiceClientCredentials : ServiceClientCredentials
        {
            public override Task ProcessHttpRequestAsync(HttpRequestMessage request, CancellationToken cancellationToken)
            {
                request.Headers.Add("Ocp-Apim-Subscription-Key", "ENTER KEY HERE");
                return base.ProcessHttpRequestAsync(request, cancellationToken);
            }
        }

        static void Main(string[] args)
        {

            // Create a client.
            ITextAnalyticsClient client = new TextAnalyticsClient(new ApiKeyServiceClientCredentials())
            {
                Endpoint = "https://westus.api.cognitive.microsoft.com"
            };

            Console.OutputEncoding = System.Text.Encoding.UTF8;

            // Extracting language.
            Console.WriteLine("===== LANGUAGE EXTRACTION ======");

            var result =  client.DetectLanguageAsync(new BatchInput(
                    new List<Input>()
                        {
                          new Input("1", "This is a document written in English."),
                          new Input("2", "Este es un document escrito en Español."),
                          new Input("3", "这是一个用中文写的文件")
                    })).Result;

            // Printing language results.
            foreach (var document in result.Documents)
            {
                Console.WriteLine("Document ID: {0} , Language: {1}", document.Id, document.DetectedLanguages[0].Name);
            }

            // Getting key phrases.
            Console.WriteLine("\n\n===== KEY-PHRASE EXTRACTION ======");

            KeyPhraseBatchResult result2 = client.KeyPhrasesAsync(new MultiLanguageBatchInput(
                        new List<MultiLanguageInput>()
                        {
                          new MultiLanguageInput("ja", "1", "猫は幸せ"),
                          new MultiLanguageInput("de", "2", "Fahrt nach Stuttgart und dann zum Hotel zu Fu."),
                          new MultiLanguageInput("en", "3", "My cat is stiff as a rock."),
                          new MultiLanguageInput("es", "4", "A mi me encanta el fútbol!")
                        })).Result;

            // Printing key phrases.
            foreach (var document in result2.Documents)
            {
                Console.WriteLine("Document ID: {0} ", document.Id);

                Console.WriteLine("\t Key phrases:");

                foreach (string keyphrase in document.KeyPhrases)
                {
                    Console.WriteLine("\t\t" + keyphrase);
                }
            }

            // Extracting sentiment.
            Console.WriteLine("\n\n===== SENTIMENT ANALYSIS ======");

            SentimentBatchResult result3 = client.SentimentAsync(
                    new MultiLanguageBatchInput(
                        new List<MultiLanguageInput>()
                        {
                          new MultiLanguageInput("en", "0", "I had the best day of my life."),
                          new MultiLanguageInput("en", "1", "This was a waste of my time. The speaker put me to sleep."),
                          new MultiLanguageInput("es", "2", "No tengo dinero ni nada que dar..."),
                          new MultiLanguageInput("it", "3", "L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."),
                        })).Result;


            // Printing sentiment results.
            foreach (var document in result3.Documents)
            {
                Console.WriteLine("Document ID: {0} , Sentiment Score: {1:0.00}", document.Id, document.Score);
            }
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Power BI ile Metin Analizi](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Ayrıca bkz. 

 [Metin Analizi'ne genel bakış](../overview.md)  
 [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)

