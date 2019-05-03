---
title: Konuşmaların çok katılımcılı sohbet Speech SDK'sı - konuşma Hizmetleri
titleSuffix: Azure Cognitive Services
description: Konuşma tanıma hizmeti Speech SDK'sı ile kullanmayı öğrenin. Kullanılabilir C++, C#ve Java.
services: cognitive-services
author: jhakulin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: jhakulin
ms.openlocfilehash: 73ab4cfa92a1efc49dea16ba2941cf16b7a1cf3e
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025799"
---
# <a name="transcribe-multi-participant-conversations-with-the-speech-sdk"></a>Çok katılımcılı sohbet Speech SDK'sı özelliği

Konuşma SDK'ın **ConversationTranscriber** API toplantıları/konuşmalar eklemek, kaldırmak ve konuşma Hizmetleri kullanarak ses akışı olarak katılımcıları tanımlama yeteneği özelliği sağlar `PullStream` veya `PushStream`.

## <a name="limitations"></a>Sınırlamalar

* Konuşma uyarlayıcı için desteklenen C++, C#ve Windows, Linux ve Android üzerinde Java.
* ROOBO DevKit sağlayan verimli bir şekilde Konuşmacı tanıma için konuşma tanıma hizmeti tarafından yararlanılabilir döngüsel çok mikrofon dizisi olarak konuşmalar oluşturma için desteklenen donanım ortamdır. [Daha fazla bilgi için bkz: konuşma cihazları SDK](speech-devices-sdk.md). 
* Ses çekme'si ve anında iletilen PCM ses sekiz kanalları modu akışlarla konuşma SDK desteği sınırlıdır.

## <a name="prerequisites"></a>Önkoşullar

* [Konuşmayı metne Speech SDK'sı ile kullanmayı öğrenin.](quickstart-csharp-dotnet-windows.md)
* [Konuşma deneme aboneliğinizi alın.](https://azure.microsoft.com/try/cognitive-services/)

## <a name="create-voice-signatures-for-participants"></a>Katılımcıların ses imzalar oluşturma

İlk adım, konuşma katılımcılarını ses imzaları oluşturmaktır. Ses imzalarını oluşturmak, etkili Konuşmacı tanıma için gereklidir.
Aşağıdaki örnekte, geçeriz [ses imza almak için REST API'yi kullanın.](https://aka.ms/cts/signaturegenservice)

Aşağıdaki örnekte, sesli imzaları oluşturmanın iki farklı yolu gösterilmektedir:
```csharp
class Program
{
    static async Task CreateVoiceSignatureByUsingFormData()
    {
        byte[] fileBytes = File.ReadAllBytes(@"speakerVoice.wav");
        var form = new MultipartFormDataContent();
        var content = new ByteArrayContent(fileBytes);
        form.Add(content, "file", "file");
        var client = new HttpClient();
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "YourSubscriptionKey");
        var response = await client.PostAsync($"https://{region}.signature.speech.microsoft.com/api/v1/Signature/GenerateVoiceSignatureFromFormData", form);
        // A voice signature can be extracted from the jsonData
        var jsonData = await response.Content.ReadAsStringAsync();
    }

    static async Task CreateVoiceSignatureByUsingBody()
    {
        byte[] fileBytes = File.ReadAllBytes(@"speakerVoice.wav");
        var content = new ByteArrayContent(fileBytes);

        var client = new HttpClient();
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "YourSubscriptionKey");
        var response = await client.PostAsync($"https://{region}.cts.speech.microsoft.com/api/v1/Signature/GenerateVoiceSignatureFromByteArray", content);
        // A voice signature can be extracted from the jsonData
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
    private static string endpoint = "YourOwnEndpoint";

    public static async Task ConversationWithPullAudioStreamAsync()
    {
        // Creates an instance of a speech config with specified subscription key and service region.
        // Replace with your own endpoint and subscription key.
        var config = SpeechConfig.FromEndpoint(new Uri(endpoint), "YourSubScriptionKey");
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
                        Console.WriteLine($"RECOGNIZED: Text={e.Result.Text}, SpeakerID={e.Result.SpeakerId}");
                    }
                    else if (e.Result.Reason == ResultReason.NoMatch)
                    {
                        Console.WriteLine($"NOMATCH: Speech could not be recognized.");
                    }
                };

                // Sets a conversation Id.
                transcriber.ConversationId = "AConversationFromTeams";

                // Add participants to the conversation.
                // Create data for voice signatures using REST API described in the earlier section in this document.
                // How to create voice signatureA, signatureB & signatureC variables, please check the SDK API samples.

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
