---
title: 'Hızlı Başlangıç: Konuşma tanıma, çeviri C# (.NET Framework Windows) - konuşma Hizmetleri'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, komut satırına metin çıktısı kullanıcı konuşma yakalamak ve başka bir dile çevirmek için basit bir .NET Framework uygulaması oluşturacaksınız. Bu kılavuz, Windows kullanıcıları için tasarlanmıştır.
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 12/13/2018
ms.author: erhopf
ms.openlocfilehash: 948565bc3266b2835a8948c391442c132d93471b
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55227684"
---
# <a name="quickstart-translate-speech-with-the-speech-sdk-for-net-framework"></a>Hızlı Başlangıç: .NET Framework için konuşma Speech SDK'sı ile Çevir

Bu hızlı başlangıçta, bilgisayarınızın mikrofondan gelen kullanıcı konuşma yakalar, konuşma çevirir ve gerçek zamanlı olarak komut satırında çevrilmiş metne dönüştürür basit bir .NET Framework uygulaması oluşturacaksınız. Bu uygulama, 64 bit Windows üzerinde çalışacak şekilde tasarlanmıştır ve ile oluşturulmuş [konuşma SDK'sı NuGet paketi](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2017.

Konuşma çevirisi için kullanılabilen dilleri tam bir listesi için bkz. [dil desteği](language-support.md).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Konuşma hizmeti için bir Azure aboneliği anahtarı. [Ücretsiz edinin](get-started.md).

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

[!INCLUDE [Create project ](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Örnek kod ekleme

1. `Program.cs` dosyasını açın ve tüm kodu aşağıdakiyle değiştirin.

    ```csharp
    using System;
    using System.Threading.Tasks;
    using Microsoft.CognitiveServices.Speech;
    using Microsoft.CognitiveServices.Speech.Translation;

    namespace helloworld
    {
        class Program
        {
            public static async Task TranslationContinuousRecognitionAsync()
            {
                // Creates an instance of a speech translation config with specified subscription key and service region.
                // Replace with your own subscription key and service region (e.g., "westus").
                var config = SpeechTranslationConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");

                // Sets source and target languages.
                string fromLanguage = "en-US";
                config.SpeechRecognitionLanguage = fromLanguage;
                config.AddTargetLanguage("de");

                // Sets voice name of synthesis output.
                const string GermanVoice = "de-DE-Hedda";
                config.VoiceName = GermanVoice;
                // Creates a translation recognizer using microphone as audio input.
                using (var recognizer = new TranslationRecognizer(config))
                {
                    // Subscribes to events.
                    recognizer.Recognizing += (s, e) =>
                    {
                        Console.WriteLine($"RECOGNIZING in '{fromLanguage}': Text={e.Result.Text}");
                        foreach (var element in e.Result.Translations)
                        {
                            Console.WriteLine($"    TRANSLATING into '{element.Key}': {element.Value}");
                        }
                    };

                    recognizer.Recognized += (s, e) =>
                    {
                        if (e.Result.Reason == ResultReason.TranslatedSpeech)
                        {
                            Console.WriteLine($"\nFinal result: Reason: {e.Result.Reason.ToString()}, recognized text in {fromLanguage}: {e.Result.Text}.");
                            foreach (var element in e.Result.Translations)
                            {
                                Console.WriteLine($"    TRANSLATING into '{element.Key}': {element.Value}");
                            }
                        }
                    };

                    recognizer.Synthesizing += (s, e) =>
                    {
                        var audio = e.Result.GetAudio();
                        Console.WriteLine(audio.Length != 0
                            ? $"AudioSize: {audio.Length}"
                            : $"AudioSize: {audio.Length} (end of synthesis data)");
                    };

                    recognizer.Canceled += (s, e) =>
                    {
                        Console.WriteLine($"\nRecognition canceled. Reason: {e.Reason}; ErrorDetails: {e.ErrorDetails}");
                    };

                    recognizer.SessionStarted += (s, e) =>
                    {
                        Console.WriteLine("\nSession started event.");
                    };

                    recognizer.SessionStopped += (s, e) =>
                    {
                        Console.WriteLine("\nSession stopped event.");
                    };

                    // Starts continuous recognition. Uses StopContinuousRecognitionAsync() to stop recognition.
                    Console.WriteLine("Say something...");
                    await recognizer.StartContinuousRecognitionAsync().ConfigureAwait(false);

                    do
                    {
                        Console.WriteLine("Press Enter to stop");
                    } while (Console.ReadKey().Key != ConsoleKey.Enter);

                    // Stops continuous recognition.
                    await recognizer.StopContinuousRecognitionAsync().ConfigureAwait(false);
                }
            }

            static void Main(string[] args)
            {
                TranslationContinuousRecognitionAsync().Wait();
            }
        }
    }
    ```

1. Aynı dosyada `YourSubscriptionKey` dizesini abonelik anahtarınız ile değiştirin.

1. Ayrıca `YourServiceRegion` dizesini de aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin ücretsiz deneme aboneliğinde `westus`) değiştirin.

1. Proje üzerindeki değişiklikleri kaydedin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Uygulamayı derleyin. Menü çubuğundan **Derle** > **Çözümü Derle**'yi seçin. Kodun hatasız derlenmesi gerekir.

    ![Visual Studio uygulamasının, Çözümü Derle seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-dotnetcore-windows-05-build.png "Başarılı derleme")

1. Uygulamayı başlatın. Menü çubuğundan **Hata Ayıklama** > **Hata Ayıklamayı Başlat**'ı seçin veya **F5** tuşuna basın.

    ![Visual Studio uygulamasının, Hata Ayıklamayı Başlat seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-dotnetcore-windows-06-start-debugging.png "Uygulamayı hata ayıklamada başlatma")

1. Bir şey söylemenizi isteyen bir konsol penceresi görünür. İngilizce bir deyim ya da cümle söyleyin. Konuşma konuşma tanıma Hizmeti'ne aktarılan çevrilmiş ve transcribed aynı pencerede görünen metin.

    ![Başarılı çeviri sonra konsol çıktısı ekran görüntüsü](media/sdk/qs-translate-csharp-dotnetcore-windows-output.png "başarılı çeviri sonra konsol çıktısı")

## <a name="next-steps"></a>Sonraki adımlar

Konuşma bir ses dosyasını okuma ve Sentezlenen konuşma olarak çevrilmiş metin çıktısı gibi ek örnekler Github'da kullanılabilir.

> [!div class="nextstepaction"]
> [Keşfedin C# github'da örnekleri](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
