---
title: 'Hızlı Başlangıç: Metin okuma, .NET Core - konuşma Hizmetleri Dönüştür'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, metin okuma metin okuma REST API'si ile nasıl dönüştürme yapılacağını öğreneceksiniz. Bu kılavuzda yer örnek metni konuşma sentezi işaretleme dili (SSML'yi) olarak yapılandırılmıştır. Bu, ses ve konuşma yanıtın dili seçmenize olanak sağlar.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: erhopf
ms.openlocfilehash: 5ae63b1738824095073ac6b9e1071f6b4a3e5ae1
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58518856"
---
# <a name="quickstart-convert-text-to-speech-using-net-core"></a>Hızlı Başlangıç: Dönüştürme metin okuma .NET Core kullanma

Bu hızlı başlangıçta nasıl dönüştürme yapılacağını öğreneceksiniz metin okuma .NET Core ve metin okuma REST API'sini kullanarak. Bu kılavuzda yer örnek metin olarak yapılandırılmış [konuşma sentezi işaretleme dili (SSML'yi)](speech-synthesis-markup.md), ses ve yanıtın dili seçmenize olanak tanıyan.

Bu hızlı başlangıç bir [Azure Bilişsel Hizmetler hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) konuşma Hizmetleri kaynağa sahip. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](https://azure.microsoft.com/try/cognitive-services/) kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* [.NET Core SDK](https://dotnet.microsoft.com/download)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download), veya en sevdiğiniz metin düzenleyiciyi
* Konuşma hizmeti için bir Azure aboneliği anahtarı

## <a name="create-a-net-core-project"></a>.NET Core projesi oluşturma

Yeni bir komut istemi (veya terminal oturumu) açın ve şu komutları çalıştırın:

```console
dotnet new console -o tts-sample
cd tts-sample
```

İlk komut şu iki işlemi yapar. Yeni bir .NET konsol uygulaması oluşturur ve adlı bir dizin oluşturur `tts-sample`. İkinci komut, proje dizinine değiştirir.

## <a name="select-the-c-language-version"></a>Seçin C# dil sürümü

Bu Hızlı Başlangıç C# 7.1 veya sonraki sürümleri. Değiştirmek için birkaç yol vardır C# projeniz için bir sürüm. Bu kılavuzda, nasıl ayarlanacağını göstereceğiz `tts-sample.csproj` dosya. Visual Studio'da dilini değiştirme gibi tüm kullanılabilir seçenekleri görmek için [seçin C# dil sürümü](https://docs.microsoft.com/dotnet/csharp/language-reference/configure-language-version).

Projenizi açın ve ardından `tts-sample.csproj`. Emin olun `LangVersion` 7.1 veya sonraki bir sürüme ayarlayın. Özellik grubu dil sürümü için değilse, bu satırları ekleyin:

```csharp
<PropertyGroup>
   <LangVersion>7.1</LangVersion>
</PropertyGroup>
```

Değişikliklerinizi kaydettiğinizden emin olun.

## <a name="add-required-namespaces-to-your-project"></a>Gerekli ad alanları projenize ekleyin.

`dotnet new console` Daha önce çalıştırdığınız komutu tarafından oluşturulan bir projeyi dahil olmak üzere `Program.cs`. Burada uygulama kodunuza giriyorum bu dosyasıdır. Açık `Program.cs`, mevcut using deyimlerinin değiştirin. Bu deyimler, derlemek ve örnek uygulamayı çalıştırmak için gerekli tüm türlerine erişimi olduğundan emin olun.

```csharp
using System;
using System.Net.Http;
using System.Text;
using System.IO;
using System.Threading.Tasks;
```

## <a name="create-a-class-for-token-exchange"></a>Belirteç değişimi için bir sınıf oluşturma

Metin okuma REST API, kimlik doğrulaması için bir erişim belirteci gerektirir. Erişim belirteci almak için bir exchange gereklidir. Bu örnek konuşma Hizmetleri abonelik anahtarınızı kullanarak bir erişim belirteci için birbiriyle değiştirir `issueToken` uç noktası.

Bu örnek, konuşma Hizmetleri aboneliğinize Batı ABD bölgesinde olduğunu varsayar. Farklı bir bölgeye kullanıyorsanız, değerini güncelleştirin `FetchTokenUri`. Tam bir listesi için bkz [bölgeleri](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#rest-apis).

```csharp
public class Authentication
{
    private string subscriptionKey;
    private string tokenFetchUri;

    public Authentication(string tokenFetchUri, string subscriptionKey)
    {
        if (string.IsNullOrWhiteSpace(tokenFetchUri))
        {
            throw new ArgumentNullException(nameof(tokenFetchUri));
        }
        if (string.IsNullOrWhiteSpace(subscriptionKey))
        {
            throw new ArgumentNullException(nameof(subscriptionKey));
        }
        this.tokenFetchUri = tokenFetchUri;
        this.subscriptionKey = subscriptionKey;
    }

    public async Task<string> FetchTokenAsync()
    {
        using (var client = new HttpClient())
        {
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", this.subscriptionKey);
            UriBuilder uriBuilder = new UriBuilder(this.tokenFetchUri);

            var result = await client.PostAsync(uriBuilder.Uri.AbsoluteUri, null).ConfigureAwait(false);
            return await result.Content.ReadAsStringAsync().ConfigureAwait(false);
        }
    }
}
```

> [!NOTE]
> Kimlik doğrulaması hakkında daha fazla bilgi için bkz. [bir erişim belirteci ile kimlik doğrulama](https://docs.microsoft.com/azure/cognitive-services/authentication#authenticate-with-an-authentication-token).

## <a name="get-an-access-token-and-set-the-host-url"></a>Erişim belirteci almak ve ana bilgisayar URL'si ayarlayın

Bulun `static void Main(string[] args)` değiştirin `static async Task Main(string[] args)`.

Ardından, bu kodu ana yönteme kopyalayın. Birkaç şeyi yapar, ancak en önemlisi, metin giriş ve çağrıları alır `Authentication` bir erişim belirteci için abonelik anahtarınızı alışverişi işlevi. Bir sorun yaşanırsa hata konsola yazdırılır.

Uygulamayı çalıştırmadan önce abonelik anahtarınızı eklediğinizden emin olun.

```csharp
// Prompts the user to input text for TTS conversion
Console.Write("What would you like to convert to speech? ");
string text = Console.ReadLine();

// Gets an access token
string accessToken;
Console.WriteLine("Attempting token exchange. Please wait...\n");

// Add your subscription key here
// If your resource isn't in WEST US, change the endpoint
Authentication auth = new Authentication("https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken", "YOUR_SUBSCRIPTION_KEY");
try
{
    accessToken = await auth.FetchTokenAsync().ConfigureAwait(false);
    Console.WriteLine("Successfully obtained an access token. \n");
}
catch (Exception ex)
{
    Console.WriteLine("Failed to obtain an access token.");
    Console.WriteLine(ex.ToString());
    Console.WriteLine(ex.Message);
    return;
}
```

Ardından konak ve yol metin okuma için ayarlayın:

```csharp
string host = "https://westus.tts.speech.microsoft.com/cognitiveservices/v1";
```

## <a name="build-the-ssml-request"></a>Derleme SSML'yi isteği

Metin gövdesi olarak gönderilen bir `POST` istek. SSML'yi ile dil ve ses belirtebilirsiniz. Bu hızlı başlangıçta, SSML'yi kümesine diliyle kullanacağız `en-US` ve ses yap `ZiraRUS`. İsteğiniz için SSML'yi şimdi oluşturun:

```csharp
string body = @"<speak version='1.0' xmlns='https://www.w3.org/2001/10/synthesis' xml:lang='en-US'>
              <voice name='Microsoft Server Speech Text to Speech Voice (en-US, ZiraRUS)'>" +
              text + "</voice></speak>";
```

> [!NOTE]
> Bu örnekte `ZiraRUS` ses tipi. Ses/dillerde Microsoft tam bir listesi sağlanmaktadır için bkz: [dil desteği](language-support.md). Markanız için benzersiz, tanınan bir ses oluşturmak istiyorsanız bkz [özel ses tipi oluşturma](how-to-customize-voice-font.md).

## <a name="instantiate-the-client-make-a-request-and-save-synthesized-audio-to-a-file"></a>İstemci örneği, bir istek oluşturun ve Sentezlenen ses bir dosyaya kaydedin.

Bu kod örneğinde geçmeden birçok farklı kaynak mevcuttur. Hızlı bir şekilde neler olduğunu gözden geçirelim:

* İstek ve istemci örneği oluşturulur.
* HTTP yöntemi olarak ayarlandığından `POST`.
* Gerekli üst bilgileri isteğine eklenir.
* İstek gönderdi ve durum kodunu denetlenir.
* Yanıt, zaman uyumsuz olarak okuma ve sample.wav adlı bir dosyaya yazılır.

Bu kod projenize kopyalayın. Değerini değiştirdiğinizden emin olun `User-Agent` Azure portalındaki kaynağınızın adıyla başlığı.

```csharp
using (var client = new HttpClient())
{
    using (var request = new HttpRequestMessage())
    {
        // Set the HTTP method
        request.Method = HttpMethod.Post;
        // Construct the URI
        request.RequestUri = new Uri(host);
        // Set the content type header
        request.Content = new StringContent(body, Encoding.UTF8, "application/ssml+xml");
        // Set additional header, such as Authorization and User-Agent
        request.Headers.Add("Authorization", "Bearer " + accessToken);
        request.Headers.Add("Connection", "Keep-Alive");
        // Update your resource name
        request.Headers.Add("User-Agent", "YOUR_RESOURCE_NAME");
        request.Headers.Add("X-Microsoft-OutputFormat", "riff-24khz-16bit-mono-pcm");
        // Create a request
        Console.WriteLine("Calling the TTS service. Please wait... \n");
        using (var response = await client.SendAsync(request).ConfigureAwait(false))
        {
            response.EnsureSuccessStatusCode();
            // Asynchronously read the response
            using (var dataStream = await response.Content.ReadAsStreamAsync().ConfigureAwait(false))
            {
                Console.WriteLine("Your speech file is being written to file...");
                using (var fileStream = new FileStream(@"sample.wav", FileMode.Create, FileAccess.Write, FileShare.Write))
                {
                    await dataStream.CopyToAsync(fileStream).ConfigureAwait(false);
                    fileStream.Close();
                }
                Console.WriteLine("\nYour file is ready. Press any key to exit.");
                Console.ReadLine();
            }
        }
    }
}
```

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

İşte bu kadar metin okuma uygulamanızı çalıştırmak hazır olursunuz. Komut satırını (veya terminal oturumu), proje dizinine gidin ve çalıştırın:

```console
dotnet run
```

Başarılı olursa, konuşma dosyanın proje klasörünüze kaydedilir. Yürütün, sık kullanılan media player kullanarak.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Abonelik anahtarınızı programınıza sabit kodladıysanız, bu hızlı başlangıcı tamamladıktan sonra abonelik anahtarını kaldırdığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Github'da .NET örneklerini keşfedin](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/Samples-Http/NETCore)

## <a name="see-also"></a>Ayrıca bkz.

* [Metin Okuma API başvurusu](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis)
* [Özel ses tipi olarak oluşturma](how-to-customize-voice-font.md)
* [Özel ses oluşturma kayıt ses örnekleri](record-custom-voice-samples.md)
