---
title: 'Hızlı Başlangıç: C# - Content Moderator metin içeriği denetleyin'
titlesuffix: Azure Cognitive Services
description: İçerik Moderator SDK'sı için C# kullanarak metin içerik denetleme
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: sajagtap
ms.openlocfilehash: ae795ad823c32bc83669d5e98e3fd922500741d4
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2018
ms.locfileid: "49309221"
---
# <a name="quickstart-check-text-content-in-c"></a>Hızlı Başlangıç: C# metin içeriği denetleyin 

Bu makalede, aşağıdaki amaçlarla [.NET için Content Moderator SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/)'nı kullanmaya başlamanıza yardımcı olacak bilgi ve kod örnekleri sağlanmaktadır:

- Terim tabanlı filtrelemeyle metin olası küfürleri algılamanıza
- Makine öğrenme tabanlı modeller için kullanma [metin sınıflandırma](text-moderation-api.md#classification) üç kategoride.
- ABD ve UK telefon numaraları, e-posta adreslerini ve ABD adresleriniz gibi kişisel bilgileri (PII) algılar.
- Metin ve düzeltme hatalarını normalleştirin

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="sign-up-for-content-moderator-services"></a>Content Moderator hizmetleri için kaydolma

Content Moderator Hizmetleri REST API veya SDK aracılığıyla kullanabilmeniz için önce bir abonelik anahtarı gerekir. Content Moderator hizmete abone [Azure portalında](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator) biri elde edilir.

## <a name="create-your-visual-studio-project"></a>Visual Studio projenizi oluşturma

1. Çözümünüze yeni bir **Console uygulaması (.NET Framework)** projesi ekleyin.

   Örnek kodda, projeyi adlandırın **TextModeration**.

1. Bu projeyi çözümün tek başlatma projesi olarak seçin.

### <a name="install-required-packages"></a>Gerekli paketleri yükleme

Aşağıdaki NuGet paketlerini yükleyin:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Programı deyimler kullanarak güncelleştirme

Aşağıdaki `using` deyimleri. 

```csharp
using Microsoft.Azure.CognitiveServices.ContentModerator;
using Microsoft.CognitiveServices.ContentModerator;
using Microsoft.CognitiveServices.ContentModerator.Models;
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using System.IO;
using System.Threading;
```

### <a name="create-the-content-moderator-client"></a>Content Moderator istemcisini oluşturma

Aboneliğiniz için bir Content Moderator istemcisi oluşturmak üzere aşağıdaki kodu ekleyin.

> [!IMPORTANT]
> **AzureRegion** ve **CMSubscriptionKey** alanlarını bölge tanımlayıcınız ve abonelik anahtarınızın değerleri ile güncelleştirin.

```csharp
/// <summary>
/// Wraps the creation and configuration of a Content Moderator client.
/// </summary>
/// <remarks>This class library contains insecure code. If you adapt this 
/// code for use in production, use a secure method of storing and using
/// your Content Moderator subscription key.</remarks>
public static class Clients
{
    /// <summary>
    /// The region/location for your Content Moderator account, 
    /// for example, westus.
    /// </summary>
    private static readonly string AzureRegion = "YOUR API REGION";

    /// <summary>
    /// The base URL fragment for Content Moderator calls.
    /// </summary>
    private static readonly string AzureBaseURL =
        $"https://{AzureRegion}.api.cognitive.microsoft.com";

    /// <summary>
    /// Your Content Moderator subscription key.
    /// </summary>
    private static readonly string CMSubscriptionKey = "YOUR API KEY";

    /// <summary>
    /// Returns a new Content Moderator client for your subscription.
    /// </summary>
    /// <returns>The new client.</returns>
    /// <remarks>The <see cref="ContentModeratorClient"/> is disposable.
    /// When you have finished using the client,
    /// you should dispose of it either directly or indirectly. </remarks>
    public static ContentModeratorClient NewClient()
    {
        // Create and initialize an instance of the Content Moderator API wrapper.
        ContentModeratorClient client = new ContentModeratorClient(new ApiKeyServiceClientCredentials(CMSubscriptionKey));

        client.Endpoint = AzureBaseURL;
        return client;
    }
}
```

### <a name="initialize-application-specific-settings"></a>Uygulamaya özgü ayarları başlatma

Aşağıdaki statik alanları Program.cs dosyasındaki **Program** sınıfına ekleyin.

```csharp
/// <summary>
/// The name of the file that contains the text to evaluate.
/// </summary>
/// <remarks>You will need to create an input file and update this path
/// accordingly. Relative paths are relative to the execution directory.</remarks>
private static string TextFile = "TextFile.txt";

/// <summary>
/// The name of the file to contain the output from the evaluation.
/// </summary>
/// <remarks>Relative paths are relative to the execution directory.</remarks>
private static string OutputFile = "TextModerationOutput.txt";
```

Aşağıdaki metni girdi olarak bu hızlı başlangıç için kullandık.

> [!NOTE]
> Aşağıdaki örnek metni geçersiz sosyal güvenlik numarası kasıtlıdır. Amacı, iletmek örnek giriş ve çıkış biçimi sağlamaktır.

```
Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.
These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 
0800 820 3300. Also, 999-99-9999 looks like a social security number (SSN).
```

## <a name="add-code-to-load-and-evaluate-the-input-text"></a>Yükleme ve giriş metni değerlendirmek için kod ekleyin

Aşağıdaki kodu **Main** yöntemine ekleyin.

```csharp
// Load the input text.
string text = File.ReadAllText(TextFile);
Console.WriteLine("Screening {0}", TextFile);

text = text.Replace(System.Environment.NewLine, " ");

// Save the moderation results to a file.
using (StreamWriter outputWriter = new StreamWriter(OutputFile, false))
{
    // Create a Content Moderator client and evaluate the text.
    using (var client = Clients.NewClient())
    {
        // Screen the input text: check for profanity, classify the text into three categories,
        // do autocorrect text, and check for personally identifying
        // information (PII)
        outputWriter.WriteLine("Autocorrect typos, check for matching terms, PII, and classify.");
        var screenResult =
        client.TextModeration.ScreenText("eng", "text/plain", text, true, true, null, true);
        outputWriter.WriteLine(
                JsonConvert.SerializeObject(screenResult, Formatting.Indented));
    }
    outputWriter.Flush();
    outputWriter.Close();
}
```

> [!NOTE]
> Content Moderator hizmet anahtarınızın saniye başına istek (RPS) hız sınırı vardır ve bu sınırı aşarsanız SDK 429 hata kodu ile bir özel durum oluşturur. Ücretsiz katmanı anahtarı kullanılırken isteklerinin hızı saniye başına bir istek sınırlıdır.

## <a name="run-the-program-and-review-the-output"></a>Programı çalıştırma ve çıktıyı gözden geçirme

Program için günlük dosyasına yazılmış gibi çıktı örneği verilmiştir:

```json
Autocorrect typos, check for matching terms, PII, and classify.
{
"OriginalText": "\"Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052. These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. Also, 999-99-9999 looks like a social security number (SSN).\"",
"NormalizedText": "\" Is this a garbage or crap email abide@ abed. com, phone: 6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, Redmond, WA 98052. These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. Also, 999-99-9999 looks like a social security number ( SSN) . \"",
"AutoCorrectedText": "\" Is this a garbage or crap email abide@ abed. com, phone: 6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, Redmond, WA 98052. These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. Also, 999-99-9999 looks like a social security number ( SSN) . \"",
"Misrepresentation": null,
    
"Classification": {
        "Category1": {
        "Score": 1.5113095059859916E-06
        },
        "Category2": {
        "Score": 0.12747249007225037
        },
        "Category3": {
        "Score": 0.98799997568130493
        },
        "ReviewRecommended": true
  },
  "Status": {
    "Code": 3000,
    "Description": "OK",
    "Exception": null
  },
  "PII": {
        "Email": [
            {
            "Detected": "abcdef@abcd.com",
            "SubType": "Regular",
            "Text": "abcdef@abcd.com",
            "Index": 33
            }
            ],
        "IPA": [
            {
            "SubType": "IPV4",
            "Text": "255.255.255.255",
            "Index": 73
            }
            ],
        "Phone": [
            {
            "CountryCode": "US",
            "Text": "6657789887",
            "Index": 57
            },
            {
            "CountryCode": "US",
            "Text": "870 608 4000",
            "Index": 211
            },
            {
            "CountryCode": "UK",
            "Text": "+44 870 608 4000",
            "Index": 207
            },
            {
            "CountryCode": "UK",
            "Text": "0344 800 2400",
            "Index": 227
            },
            {
        "CountryCode": "UK",
            "Text": "0800 820 3300",
            "Index": 244
            }
            ],
         "Address": [{
             "Text": "1 Microsoft Way, Redmond, WA 98052",
             "Index": 89
                }]
    },
  "Language": "eng",
  "Terms": [
    {
        "Index": 22,
        "OriginalIndex": 22,
        "ListId": 0,
        "Term": "crap"
    }
  ],
  "TrackingId": "9392c53c-d11a-441d-a874-eb2b93d978d3"
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu ve diğer .NET için Content Moderator hızlı başlangıçları için [Content Moderator .NET SDK'sını](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) ve [Visual Studio çözümünü](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) alın ve tümleştirmeniz üzerinde çalışmaya başlayın.
