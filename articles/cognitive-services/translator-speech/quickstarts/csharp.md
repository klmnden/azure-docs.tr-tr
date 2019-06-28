---
title: "Hızlı Başlangıç: Translator konuşma çevirisi API'siC#"
titlesuffix: Azure Cognitive Services
description: Translator Konuşma Çevirisi API'sini kısa sürede kullanmaya başlamanıza yardımcı olacak bilgi ve kod örnekleri alın.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-speech
ms.topic: quickstart
ms.date: 04/26/2019
ms.author: swmachan
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 5cca3d5d02f9618e99f122bf3fc2a7d2874e1cdf
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444519"
---
# <a name="quickstart-translator-speech-api-with-c"></a>Hızlı Başlangıç: Translator konuşma tanıma API'si ileC#
<a name="HOLTop"></a>

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-translator-speech-deprecation-note.md)]

Bu makalede Translator Konuşma Çevirisi API'si’ni kullanarak bir .wav dosyasında konuşulan sözcüklerin nasıl çevrileceği gösterilir.

## <a name="prerequisites"></a>Önkoşullar

İhtiyacınız olacak [Visual Studio 2019](https://www.visualstudio.com/downloads/) Windows üzerinde bu kodu çalıştırmak için. (Ücretsiz Community Edition’ı kullanabilirsiniz.) Mac OS veya Linux kullanırsanız Metin Düzenleyicisi'ni de kullanabilirsiniz [Visual Studio Code](https://code.visualstudio.com/Download) alternatif olarak.

Aşağıdaki koddan derlediğiniz yürütülebilir dosya ile aynı klasörün içinde "speak.wav" adlı bir .wav dosyanız olmalıdır. Bu .wav dosyası standart PCM, 16 bit, 16 kHz, mono biçiminde olmalıdır.

**Microsoft Translator Konuşma Çevirisi API'sine** sahip bir [Bilişsel Hizmetler API hesabınızın](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) olması gerekir. [Azure panonuzdan](https://portal.azure.com/#create/Microsoft.CognitiveServices) ücretli bir abonelik anahtarına ihtiyacınız olacak.

## <a name="translate-speech"></a>Konuşmayı çevirme

Aşağıdaki kod, konuşmayı bir dilden diğerine çevirir.

1. Sık kullandığınız IDE'de yeni bir C# projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```csharp
using System;
using System.IO;
using System.Net.WebSockets;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

namespace TranslateSpeechQuickStart
{
    class Program
    {
        static string host = "wss://dev.microsofttranslator.com";
        static string path = "/speech/translate";

        // NOTE: Replace this example key with a valid subscription key.
        static string key = "ENTER KEY HERE";

        async static Task Send (ClientWebSocket client, string input_path)
        {
            var audio = File.ReadAllBytes(input_path);
            var audio_out_buffer = new ArraySegment<byte>(audio);
            Console.WriteLine("Sending audio.");
            await client.SendAsync(audio_out_buffer, WebSocketMessageType.Binary, true, CancellationToken.None);

            /* Make sure the audio file is followed by silence.
             * This lets the service know that the audio input is finished. */
            var silence = new byte[32000];
            var silence_buffer = new ArraySegment<byte>(silence);
            await client.SendAsync(silence_buffer, WebSocketMessageType.Binary, true, CancellationToken.None);

            Console.WriteLine("Done sending.");
            System.Threading.Thread.Sleep(3000);
            await client.CloseAsync(WebSocketCloseStatus.NormalClosure, "", CancellationToken.None);
        }

        async static Task Receive(ClientWebSocket client, string output_path)
        {
            var inbuf = new byte[102400];
            var segment = new ArraySegment<byte>(inbuf);
            var stream = new FileStream(output_path, FileMode.Create);

            Console.WriteLine("Awaiting response.");
            while (client.State == WebSocketState.Open)
            {
                var result = await client.ReceiveAsync(segment, CancellationToken.None);
                switch (result.MessageType)
                {
                    case WebSocketMessageType.Close:
                        Console.WriteLine("Received close message. Status: " + result.CloseStatus + ". Description: " + result.CloseStatusDescription);
                        await client.CloseAsync(WebSocketCloseStatus.NormalClosure, string.Empty, CancellationToken.None);
                        break;
                    case WebSocketMessageType.Text:
                        Console.WriteLine("Received text.");
                        Console.WriteLine(Encoding.UTF8.GetString(inbuf).TrimEnd('\0'));
                        break;
                    case WebSocketMessageType.Binary:
                        Console.WriteLine("Received binary data: " + result.Count + " bytes.");
                        stream.Write(inbuf, 0, result.Count);
                        break;
                }
            }

            stream.Close();
            stream.Dispose();
        }

        async static void TranslateSpeech()
        {
            var client = new ClientWebSocket();
            client.Options.SetRequestHeader ("Ocp-Apim-Subscription-Key", key);

            string from = "en-US";
            string to = "it-IT";
            string features = "texttospeech";
            string voice = "it-IT-Elsa";
            string api = "1.0";

            string input_path = "speak.wav";
            string output_path = "speak2.wav";

            string uri = host + path +
                "?from=" + from +
                "&to=" + to +
                "&api-version=" + api +
                "&features=" + features +
                "&voice=" + voice;

            Console.WriteLine("uri: " + uri);
            Console.WriteLine("Opening connection.");
            await client.ConnectAsync(new Uri(uri), CancellationToken.None);
            Console.WriteLine("Connection open.");
            Task.WhenAll(Send(client, input_path), Receive(client, output_path)).Wait();
        }

        static void Main()
        {
            TranslateSpeech();
            Console.ReadLine();
        }

    }
}
```

**Konuşma yanıtını çevirme**

"speak2.wav" adlı bir dosya oluşturulduğunda sonuç başarılıdır. Dosya, "speak.wav" içinde konuşulan sözcüklerin çevirisini içerir.

[Başa dön](#HOLTop)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Translator Konuşma Çevirisi öğreticisi](../tutorial-translator-speech-csharp.md)

## <a name="see-also"></a>Ayrıca bkz.

[Translator Konuşma Çevirisine genel bakış](../overview.md)
[API Başvurusu](https://docs.microsoft.com/azure/cognitive-services/translator-speech/reference)
