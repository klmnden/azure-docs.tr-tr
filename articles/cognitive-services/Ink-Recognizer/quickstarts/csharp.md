---
title: "Hızlı Başlangıç: Dijital Mürekkep mürekkep tanıyıcı REST API'si ile tanıması veC#"
description: Dijital mürekkep vuruşlarını algılamayı başlatmak için mürekkep tanıyıcı API'sini kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: ink-recognizer
ms.topic: article
ms.date: 05/02/2019
ms.author: aahi
ms.openlocfilehash: 9bb9c23cc1f807cae1d0d22f1652e8f4408f1f91
ms.sourcegitcommit: 17411cbf03c3fa3602e624e641099196769d718b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65518666"
---
# <a name="quickstart-recognize-digital-ink-with-the-ink-recognizer-rest-api-and-c"></a>Hızlı Başlangıç: Dijital Mürekkep mürekkep tanıyıcı REST API'si ile tanıması veC#

Dijital mürekkep vuruşlarını mürekkep tanıyıcı API'sine göndermeye başlamak için bu Hızlı Başlangıç'ı kullanın. Bu C# uygulama JSON biçimli bir mürekkep vuruşu verilerini içeren bir API isteği gönderir ve yanıtı alır.

Bu uygulamanın yazıldığı sırada C#, çoğu programlama dilleri ile uyumlu bir RESTful web hizmeti API'dir.

Genellikle bir dijital mürekkep uygulamasından API çağrısıyla sonlandırmalısınız. Bu hızlı başlangıçta, bir JSON dosyasından el yazısı aşağıdaki örnek mürekkep vuruşu verileri gönderir.

![Resimlerdeki el yazısı görüntüsü](../media/handwriting-sample.jpg)

Bu Hızlı Başlangıç için kaynak kodu bulunabilir [GitHub](https://go.microsoft.com/fwlink/?linkid=2089502).

## <a name="prerequisites"></a>Önkoşullar

- [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)’nin herhangi bir sürümü.
- [Newtonsoft.Json](https://www.newtonsoft.com/json)
    - Visual Studio'da bir NuGet paketi olarak Newtonsoft.Json yüklemek için:
        1. Sağ tıklayın **çözüm Yöneticisi**
        2. Tıklayın **NuGet paketlerini Yönet...**
        3. Arama `Newtonsoft.Json` paketini ve yükleme
- Linux/MacOS kullanıyorsanız, bu uygulama olması çalıştırırsa kullanarak [Mono](https://www.mono-project.com/).

- Bu hızlı başlangıç örnek mürekkep vuruşu verilerini bulunabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/InkRecognition/quickstart/example-ink-strokes.json).

[!INCLUDE [cognitive-services-ink-recognizer-signup-requirements](../../../../includes/cognitive-services-ink-recognizer-signup-requirements.md)]


## <a name="create-a-new-application"></a>Yeni uygulama oluşturma

1. Visual Studio'da yeni bir konsol çözümü oluşturun ve aşağıdaki paketleri ekleyin. 

    ```csharp
    using System;
    using System.IO;
    using System.Net;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using System.Threading.Tasks;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    ```

2. Abonelik anahtarınız ve uç noktanız için değişkenler oluşturun. Mürekkep tanıma için kullanabileceğiniz URI aşağıdadır. Daha sonra API oluşturmak için hizmet uç noktanıza eklenir istek URL'si.

    ```csharp
    // Replace the subscriptionKey string with your valid subscription key.
    const string subscriptionKey = "YOUR_SUBSCRIPTION_KEY";

    // Replace the dataPath string with a path to the JSON formatted ink stroke data.
    const string dataPath = @"PATH-TO-INK-STROKE-DATA"; 

    // URI information for ink recognition:
    const string endpoint = "https://api.cognitive.microsoft.com";
    const string inkRecognitionUrl = "/inkrecognizer/v1.0-preview/recognize";
    ```

## <a name="create-a-function-to-send-requests"></a>İstekleri göndermek için bir işlev oluşturma

1. Adlı yeni bir zaman uyumsuz işlev oluşturma `Request` yukarıda oluşturulan değişkenleri alır.

2. İstemcinin güvenlik protokolü ve üst bilgi bilgileri kullanarak bir `HttpClient` nesne. Abonelik anahtarınızı eklediğinizden emin olun `Ocp-Apim-Subscription-Key` başlığı. Oluşturup bir `StringContent` istek nesnesi.
 
3. İsteği Gönder `PutAsync()`. İstek başarılı olursa, yanıtı döndürür.  
    
    ```csharp
    static async Task<string> Request(string apiAddress, string endpoint, string subscriptionKey, string requestData){
        
        using (HttpClient client = new HttpClient { BaseAddress = new Uri(apiAddress) }){
            System.Net.ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls12 | SecurityProtocolType.Tls11 | SecurityProtocolType.Tls;
            client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

            var content = new StringContent(requestData, Encoding.UTF8, "application/json");
            var res = await client.PutAsync(endpoint, content);
            if (res.IsSuccessStatusCode){
                return await res.Content.ReadAsStringAsync();
            }
            else{
                return $"ErrorCode: {res.StatusCode}";
            }
        }
    }
    ```

## <a name="send-an-ink-recognition-request"></a>Bir mürekkep tanıma İsteği Gönder

1. Adlı yeni bir işlev oluşturma `recognizeInk()`. Bir isteği oluşturmak ve çağırarak göndermek `Request()` işlevi, uç nokta, abonelik anahtarı, API ve dijital mürekkep vuruşu verileri için URL.

2. JSON nesnesi seri durumdan ve konsola yazar. 
    
    ```csharp
    static void recognizeInk(string requestData){

        //construct the request
        var result = Request(
            endpoint,
            inkRecognitionUrl,
            subscriptionKey,
            requestData).Result;

        dynamic jsonObj = Newtonsoft.Json.JsonConvert.DeserializeObject(result);
        System.Console.WriteLine(jsonObj);
    }
    ```

## <a name="load-your-digital-ink-data"></a>Dijital Mürekkep verilerinizi yüklemek

Çağrılan bir işlev oluşturma `LoadJson()` mürekkep verileri JSON dosyası yüklenemedi. Kullanım bir `StreamReader` ve `JsonTextReader` oluşturmak için bir `JObject` ve döndürün.
    
```csharp
public static JObject LoadJson(string fileLocation){

    var jsonObj = new JObject();

    using (StreamReader file = File.OpenText(fileLocation))
    using (JsonTextReader reader = new JsonTextReader(file)){
        jsonObj = (JObject)JToken.ReadFrom(reader);
    }
    return jsonObj;
}
```

## <a name="send-the-api-request"></a>API isteği gönder

1. Uygulamanızın ana yöntemde işlevini kullanarak yukarıda oluşturulan JSON verilerinizi yükleyin. 

2. Çağrı `recognizeInk()` yukarıda oluşturduğunuz işlev. Kullanım `System.Console.ReadKey()` için uygulamayı çalıştırdıktan sonra konsol penceresini açık tutun.
    
    ```csharp
    static void Main(string[] args){

        var requestData = LoadJson(dataPath);
        string requestString = requestData.ToString(Newtonsoft.Json.Formatting.None);
        recognizeInk(requestString);
        System.Console.WriteLine("\nPress any key to exit ");
        System.Console.ReadKey();
        }
    ```

## <a name="run-the-application-and-view-the-response"></a>Uygulamayı çalıştırmak ve yanıtı görüntüleyin

Uygulamayı çalıştırın. Başarılı bir yanıt JSON biçiminde döndürülür. Üzerinde JSON yanıt bulabilirsiniz [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/InkRecognition/quickstart/example-response.json).


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [REST API başvurusu](https://go.microsoft.com/fwlink/?linkid=2089907)


Mürekkep tanıma API'si dijital mürekkep bir uygulamada nasıl çalıştığını görmek için Github'da aşağıdaki örnek uygulamaları göz atın:
* [C# Evrensel Windows Platformu (UWP)](https://go.microsoft.com/fwlink/?linkid=2089803)  
* [C# Windows Presentation Foundation (WPF)](https://go.microsoft.com/fwlink/?linkid=2089804)
* [JavaScript web tarayıcı uygulaması](https://go.microsoft.com/fwlink/?linkid=2089908)       
* [Java ve Android mobil uygulaması](https://go.microsoft.com/fwlink/?linkid=2089906)
* [Swift ve iOS mobil uygulaması](https://go.microsoft.com/fwlink/?linkid=2089805)
