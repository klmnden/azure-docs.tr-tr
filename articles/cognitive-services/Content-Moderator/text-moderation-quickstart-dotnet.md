---
title: Azure Content Moderator - .NET kullanarak metin | Microsoft Docs
description: Nasıl yapılır .NET için Azure Content Moderator SDK'sını kullanarak metin
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/04/2018
ms.author: sajagtap
ms.openlocfilehash: 87ed816077d2c742223a0350851cdf2f0c5653f6
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2018
ms.locfileid: "44023047"
---
# <a name="moderate-text-using-net"></a>.NET kullanarak metin

Bu makalede bilgiler sağlanmaktadır ve yardımcı olması için kod örnekleri, kullanmaya başlama [Content Moderator SDK'sı .NET için](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) için:
- Terim tabanlı filtrelemeyle metin olası küfürleri algılamanıza
- Makine öğrenme tabanlı modeller için kullanma [metin sınıflandırma](text-moderation-api.md#classification) üç kategoride.
- ABD ve UK telefon numaraları, e-posta adreslerini ve ABD adresleriniz gibi kişisel bilgileri (PII) algılar.
- Metin ve düzeltme hatalarını normalleştirin

Bu makalede, zaten Visual Studio ve C# ile ilgili bilgi sahibi olduğunuz varsayılır.

## <a name="sign-up-for-content-moderator-services"></a>Content Moderator Hizmetleri için kaydolun

Content Moderator Hizmetleri REST API veya SDK aracılığıyla kullanabilmeniz için önce bir abonelik anahtarı gerekir.
Başvurmak [hızlı](quick-start.md) anahtarı nasıl edinebilirsiniz öğrenin.

## <a name="create-your-visual-studio-project"></a>Visual Studio projenizi oluşturun

1. Yeni bir **konsol uygulaması (.NET Framework)** çözümünüze bir proje.

   Örnek kodda, projeyi adlandırın **TextModeration**.

1. Bu proje, çözüm için tek bir başlangıç projesi olarak seçin.

1. Bir başvuru ekleyin **ModeratorHelper** proje oluşturduğunuz derleme [Content Moderator istemci Yardımcısı hızlı](content-moderator-helper-quickstart-dotnet.md).

### <a name="install-required-packages"></a>Gerekli paketleri yükleme

Aşağıdaki NuGet paketlerini yükleyin:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Deyimleri kullanarak program güncelleştirme

Değiştirme deyimleri kullanarak program.

    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using ModeratorHelper;
    using Newtonsoft.Json;
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Threading;


### <a name="initialize-application-specific-settings"></a>Uygulamaya özgü ayarları başlatmak

Aşağıdaki statik alanları ekleme **Program** Program.cs sınıfında.

    /// <summary>
    /// The name of the file that contains the text to evaluate.
    /// </summary>
    /// <remarks>You will need to create an input file and update this path
    /// accordingly. Relative paths are ralative the execution directory.</remarks>
    private static string TextFile = "TextFile.txt";

    /// <summary>
    /// The name of the file to contain the output from the evaluation.
    /// </summary>
    /// <remarks>Relative paths are ralative the execution directory.</remarks>
    private static string OutputFile = "TextModerationOutput.txt";

Bu Hızlı Başlangıç için çıktı oluşturmak için aşağıdaki metni kullanılır:

> [!NOTE]
> Aşağıdaki örnek metni geçersiz sosyal güvenlik numarası kasıtlıdır. Amacı, iletmek örnek giriş ve çıkış biçimi sağlamaktır.

    Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.
    These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 
    0800 820 3300. Also, 999-99-9999 looks like a social security number (SSN).

## <a name="add-code-to-load-and-evaluate-the-input-text"></a>Yükleme ve giriş metni değerlendirmek için kod ekleyin

Aşağıdaki kodu ekleyin **ana** yöntemi.

    // Load the input text.
    string text = File.ReadAllText(TextFile);
    text = text.Replace(System.Environment.NewLine, " ");

    // Save the moderation results to a file.
    using (StreamWriter outputWriter = new StreamWriter(OutputFile, false))
    {
        // Create a Content Moderator client and evaluate the text.
        using (var client = Clients.NewClient())
        {
            // Screen the input text: check for profanity, classify the text into three categories
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

> [!NOTE]
> Content Moderator hizmet anahtarınız bir istek başına ikinci (RP'ler) hız sınırı vardır ve sınırını aşarsanız, SDK'sı, 429 hata koduna sahip bir özel durum oluşturur.
>
> Ücretsiz katmanı anahtarı kullanılırken isteklerinin hızı saniye başına bir istek sınırlıdır.

## <a name="run-the-program-and-review-the-output"></a>Programı çalıştırın ve çıktıyı gözden geçirin

Program için günlük dosyasına yazılmış gibi çıktı örneği verilmiştir:

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
            "   CountryCode": "UK",
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

## <a name="next-steps"></a>Sonraki adımlar

Alma [Content Moderator .NET SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) ve [Visual Studio çözümü](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) bu ve diğer Content Moderator hızlı başlangıçlar, .NET için ve tümleştirmenizi üzerinde çalışmaya başlayın.
