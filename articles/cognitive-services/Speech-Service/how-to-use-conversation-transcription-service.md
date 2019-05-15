---
title: Konuşmaların çok katılımcılı sohbet Speech SDK'sı - konuşma Hizmetleri
titleSuffix: Azure Cognitive Services
description: Konuşma Transkripsiyonu Speech SDK'sı ile kullanmayı öğrenin. Kullanılabilir C++, C#ve Java.
services: cognitive-services
author: jhakulin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: jhakulin
ms.openlocfilehash: 80ec606fee30c239d47bca94188d3b9cbb7c82d5
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65604421"
---
# <a name="transcribe-multi-participant-conversations-with-the-speech-sdk"></a>Çok katılımcılı sohbet Speech SDK'sı özelliği

Konuşma SDK'ın **ConversationTranscriber** API toplantıları/konuşmalar eklemek, kaldırmak ve konuşma Hizmetleri kullanarak ses akışı olarak katılımcıları tanımlama yeteneği özelliği sağlar `PullStream` veya `PushStream`.

## <a name="limitations"></a>Sınırlamalar

* Konuşma uyarlayıcı için desteklenen C++, C#ve Windows, Linux ve Android üzerinde Java.
* ROOBO DevKit yararlanılabilir döngüsel çok mikrofon dizisi Konuşmacı tanıma için verimli bir şekilde sağlayan olarak konuşma döküm oluşturma için desteklenen donanımlar ortamıdır. [Daha fazla bilgi için bkz: konuşma cihazları SDK](speech-devices-sdk.md).
* Konuşma SDK desteği için konuşma tanıma ses çekme kullanın ve 16-bit 16 kHz PCM ses sekiz kanallar modu akışları göndermek için sınırlıdır.
* Konuşma Transkripsiyonu şu anda aşağıdaki bölgelerde "en-US" ve "zh-CN" dillerde: centralus ve ping'in ekran.

## <a name="prerequisites"></a>Önkoşullar

* [Konuşmayı metne Speech SDK'sı ile kullanmayı öğrenin.](quickstart-csharp-dotnet-windows.md)
* [Konuşma deneme aboneliğinizi alın.](https://azure.microsoft.com/try/cognitive-services/)
* Konuşma SDK'sı sürüm 1.5.1 veya üstü gereklidir.

## <a name="create-voice-signatures-for-participants"></a>Katılımcıların ses imzalar oluşturma

İlk adım, konuşma katılımcılarını ses imzaları oluşturmaktır. Ses imzalarını oluşturmak, etkili Konuşmacı tanıma için gereklidir.

### <a name="requirements-for-input-wave-file"></a>Giriş ses dosyası için gereksinimler

* Ses imzalarını oluşturmak için giriş ses wave dosyasını 16-bit örnekleri, 16 kHz örnekleme hızı ve tek bir kanal (tekli) biçiminde olmalıdır.
* Önerilen ses her örnek için iki dakika 30 saniye arasında uzunluğudur.

Aşağıdaki örnek ses imzası [REST API'sini kullanarak.] oluşturmak için iki farklı yol gösterir. (https://aka.ms/cts/signaturegenservice) gelen C#:

```csharp
class Program
{
    static async Task CreateVoiceSignatureByUsingFormData()
    {
        var region = "YourServiceRegion";
        byte[] fileBytes = File.ReadAllBytes(@"speakerVoice.wav");
        var form = new MultipartFormDataContent();
        var content = new ByteArrayContent(fileBytes);
        form.Add(content, "file", "file");
        var client = new HttpClient();
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "YourSubscriptionKey");
        var response = await client.PostAsync($"https://signature.{region}.cts.speech.microsoft.com/api/v1/Signature/GenerateVoiceSignatureFromFormData", form);
        // A voice signature contains Version, Tag and Data key values from the Signature json structure from the Response body.
        // Voice signature format example: { "Version": <Numeric value>, "Tag": "string", "Data": "string" }
        var jsonData = await response.Content.ReadAsStringAsync();
    }

    static async Task CreateVoiceSignatureByUsingBody()
    {
        var region = "YourServiceRegion";
        byte[] fileBytes = File.ReadAllBytes(@"speakerVoice.wav");
        var content = new ByteArrayContent(fileBytes);

        var client = new HttpClient();
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "YourSubscriptionKey");
        var response = await client.PostAsync($"https://signature.{region}.cts.speech.microsoft.com/api/v1/Signature/GenerateVoiceSignatureFromByteArray", content);
        // A voice signature contains Version, Tag and Data key values from the Signature json structure from the Response body.
        // Voice signature format example: { "Version": <Numeric value>, "Tag": "string", "Data": "string" }
        var jsonData = await response.Content.ReadAsStringAsync();
    }

    static void Main(string[] args)
    {
        CreateVoiceSignatureByUsingFormData().Wait();
        CreateVoiceSignatureByUsingBody().Wait();
    }
}
```

## <a name="transcribing-conversations"></a>Fotoğrafını çekme konuşmaları

Birden fazla katılımcıları ile görüşmeler konuşmaların için oluşturma `ConversationTranscriber` ile ilişkili nesne `AudioConfig` konuşma oturumu ve akış ses kullanmak için oluşturulan nesne `PullAudioInputStream` veya `PushAudioInputStream`.

Adlı bir ConversationTranscriber sınıf olduğunu varsayalım `MyConversationTranscriber`. Kodunuz şöyle görünebilir:

```csharp
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;
using Microsoft.CognitiveServices.Speech.Conversation;

public class MyConversationTranscriber
{
    public static async Task ConversationWithPullAudioStreamAsync()
    {
        // Creates an instance of a speech config with specified subscription key and service region.
        // Replace with your own subscription key and region.
        var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
        var stopTranscription = new TaskCompletionSource<int>();

        // Create an audio stream from a wav file.
        // Replace with your own audio file name and Helper class which implements AudioConfig using PullAudioInputStreamCallback
        using (var audioInput = Helper.OpenWavFile(@"8channelsOfRecordedPCMAudio.wav"))
        {
            // Creates a conversation transcriber using audio stream input.
            using (var transcriber = new ConversationTranscriber(config, audioInput))
            {
                // Subscribes to events.
                transcriber.Recognizing += (s, e) =>
                {
                    Console.WriteLine($"RECOGNIZING: Text={e.Result.Text}");
                };

                transcriber.Recognized += (s, e) =>
                {
                    if (e.Result.Reason == ResultReason.RecognizedSpeech)
                    {
                        Console.WriteLine($"RECOGNIZED: Text={e.Result.Text}, UserID={e.Result.UserId}");
                    }
                    else if (e.Result.Reason == ResultReason.NoMatch)
                    {
                        Console.WriteLine($"NOMATCH: Speech could not be recognized.");
                    }
                };

                transcriber.Canceled += (s, e) =>
                {
                    Console.WriteLine($"CANCELED: Reason={e.Reason}");

                    if (e.Reason == CancellationReason.Error)
                    {
                        Console.WriteLine($"CANCELED: ErrorCode={e.ErrorCode}");
                        Console.WriteLine($"CANCELED: ErrorDetails={e.ErrorDetails}");
                        Console.WriteLine($"CANCELED: Did you update the subscription info?");
                        stopTranscription.TrySetResult(0);
                    }
                };

                transcriber.SessionStarted += (s, e) =>
                {
                    Console.WriteLine("\nSession started event.");
                };

                transcriber.SessionStopped += (s, e) =>
                {
                    Console.WriteLine("\nSession stopped event.");
                    Console.WriteLine("\nStop recognition.");
                    stopTranscription.TrySetResult(0);
                };

                // Sets a conversation Id.
                transcriber.ConversationId = "AConversationFromTeams";

                // Add participants to the conversation.
                // Create voice signatures using REST API described in the earlier section in this document. 
                // Voice signature needs to be in the following format:
                // { "Version": <Numeric value>, "Tag": "string", "Data": "string" }

                var speakerA = Participant.From("Speaker_A", "en-us", signatureA);
                var speakerB = Participant.From("Speaker_B", "en-us", signatureB);
                var speakerC = Participant.From("SPeaker_C", "en-us", signatureC);
                transcriber.AddParticipant(speakerA);
                transcriber.AddParticipant(speakerB);
                transcriber.AddParticipant(speakerC);

                // Starts transcribing of the conversation. Uses StopTranscribingAsync() to stop transcribing when all participants leave.
                await transcriber.StartTranscribingAsync().ConfigureAwait(false);

                // Waits for completion.
                // Use Task.WaitAny to keep the task rooted.
                Task.WaitAny(new[] { stopTranscription.Task });

                // Stop transcribing the conversation.
                await transcriber.StopTranscribingAsync().ConfigureAwait(false);

                // Ends the conversation.
                await transcriber.EndConversationAsync().ConfigureAwait(false);
            }
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Github'da örneklerimizi keşfedin](https://aka.ms/csspeech/samples)
