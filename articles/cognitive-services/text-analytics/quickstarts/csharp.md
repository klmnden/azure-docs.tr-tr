---
title: Azure Bilişsel hizmetler, metin analizi API için C# hızlı başlangıç | Microsoft Docs
description: Hızlı bir şekilde yardımcı olmak için bilgi ve kod örnekleri get metin Analytics API Azure üzerinde Microsoft Bilişsel Hizmetleri'ndeki kullanmaya başlayın.
services: cognitive-services
documentationcenter: ''
author: luiscabrer
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 09/20/2017
ms.author: ashmaka
ms.openlocfilehash: d9c61a83450844461f621ff16354881a029f7ad6
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266303"
---
# <a name="quickstart-for-text-analytics-api-with-c"></a>Metin Analizi ile C# API'si için hızlı başlangıç 
<a name="HOLTop"></a>

Bu makalede dili algıla, düşünceleri çözümlemek ve anahtar tümcecikleri kullanarak ayıklamak gösterilmiştir [metin Analytics API'leri](//go.microsoft.com/fwlink/?LinkID=759711) C# ile. Kodu, ayrıca, Linux veya MacOS çalıştırabilir şekilde dış kitaplıkları, en az başvuruları çekirdek uygulama .net üzerinde çalışmak için yazılmıştır.

Başvurmak [API tanımlarını](//go.microsoft.com/fwlink/?LinkID=759346) API için teknik belgeler için.

## <a name="prerequisites"></a>Önkoşullar

Sahip olmanız gerekir bir [Bilişsel Hizmetleri API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ile **metin Analytics API**. Kullanabileceğiniz **5.000 işlemleri/ay ücretsiz katmanına** Bu hızlı başlangıç tamamlamak için.

Ayrıca olmalıdır [endpoint ve erişim anahtarı](../How-tos/text-analytics-how-to-access-key.md) üretilen sizin için oturum açma sırasında ayarlama. 


## <a name="install-the-nuget-sdk-package"></a>Nuget SDK paketini yükle
1. Visual Studio'da yeni bir konsol çözümü oluşturun.
1. Çözüm ve üzerinde sağ tıklatın **çözüm için NuGet paketlerini Yönet**
1. İşareti **dahil yayın öncesi** onay kutusu.
1. Seçin **Gözat** arayın ve sekmesinde **Microsoft.Azure.CognitiveServices.Language**
1. Nuget paketini seçin ve yükleyin.

> [!Tip]
>  Çağrı ancak [HTTP uç noktaları](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) doğrudan C# öğesinden Microsoft.Azure.CognitiveServices.Language SDK'sı kolaylaştırır çok seri hale getirme ve JSON seri durumdan hakkında endişelenmeye gerek kalmadan hizmeti çağırmak.
>
> Birkaç faydalı bağlantılar:
> - [SDK'sı Nuget sayfası](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics)
> - [SDK kod ](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/CognitiveServices/dataPlane/Language/TextAnalytics)


## <a name="call-the-text-analytics-api-using-the-sdk"></a>Metin analizi SDK'sını kullanarak API çağrısı
1. Program.cs aşağıda sağlanan kod ile değiştirin. Bu program (dil ayıklama, anahtar tümcecik ayıklama ve düşünceleri analiz) 3 bölümlerdeki metni Analytics API özelliklerini gösterir.
1. Değiştir `Ocp-Apim-Subscription-Key` aboneliğiniz için geçerli bir erişim anahtarı ile üstbilgi değeri.
1. Konumu değiştirmek `client.AzureRegion` (şu anda `AzureRegions.Westus`) oturumunuz için bölge.
1. Programını çalıştırın.

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
            ITextAnalyticsAPI client = new TextAnalyticsAPI(new ApiKeyServiceClientCredentials());
            client.AzureRegion = AzureRegions.Westus;

            Console.OutputEncoding = System.Text.Encoding.UTF8;

            // Extracting language
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

            // Getting key-phrases
            Console.WriteLine("\n\n===== KEY-PHRASE EXTRACTION ======");

            KeyPhraseBatchResult result2 = client.KeyPhrasesAsync(new MultiLanguageBatchInput(
                        new List<MultiLanguageInput>()
                        {
                          new MultiLanguageInput("ja", "1", "猫は幸せ"),
                          new MultiLanguageInput("de", "2", "Fahrt nach Stuttgart und dann zum Hotel zu Fu."),
                          new MultiLanguageInput("en", "3", "My cat is stiff as a rock."),
                          new MultiLanguageInput("es", "4", "A mi me encanta el fútbol!")
                        })).Result;

            // Printing keyphrases
            foreach (var document in result2.Documents)
            {
                Console.WriteLine("Document ID: {0} ", document.Id);

                Console.WriteLine("\t Key phrases:");

                foreach (string keyphrase in document.KeyPhrases)
                {
                    Console.WriteLine("\t\t" + keyphrase);
                }
            }

            // Extracting sentiment
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


            // Printing sentiment results
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
> [Power BI ile metin analizi](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Ayrıca bkz. 

 [Metin analizi genel bakış](../overview.md)  
 [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)

