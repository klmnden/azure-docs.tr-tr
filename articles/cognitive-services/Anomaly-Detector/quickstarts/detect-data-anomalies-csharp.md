---
title: 'Hızlı Başlangıç: Anomali algılayıcısı REST API kullanarak, zaman serisi verilerinde görülen anomalileri algılayın veC#'
titleSuffix: Azure Cognitive Services
description: Toplu olarak ya da akış verileri, veri serisinde prosesler algılamak için Anomali algılayıcısı API'sini kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: quickstart
ms.date: 03/26/2019
ms.author: aahi
ms.openlocfilehash: 1b3ed38e7ce8738a0e92775915e894c6e0bbd52a
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67721554"
---
# <a name="quickstart-detect-anomalies-in-your-time-series-data-using-the-anomaly-detector-rest-api-and-c"></a>Hızlı Başlangıç: Anomali algılayıcısı REST API kullanarak, zaman serisi verilerinde görülen anomalileri algılayın veC# 

Zaman serisi verilerinizdeki anormallikleri algılamak için Anomali algılayıcısı API'SİNİN iki algılama modunu kullanmaya başlamak için bu Hızlı Başlangıç'ı kullanın. Bu C# uygulama JSON biçimli zaman serisi verilerini içeren iki API isteği gönderir ve yanıtları alır.

| API isteği                                        | Uygulama çıktısı                                                                                                                         |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Toplu iş olarak anomalileri algılayın                        | Zaman serisi verileri ve algılanan anomalileri konumlarını her veri noktası için anomali durumu (ve diğer verileri) içeren bir JSON yanıtı. |
| En son veri noktası durumunu anomali algılama | Zaman serisi verilerinde en son veri noktası için anomali durumu (ve diğer verileri) içeren bir JSON yanıtı.                                                                                                                                         |

 Bu uygulamanın yazıldığı sırada C#, çoğu programlama dilleri ile uyumlu bir RESTful web hizmeti API'dir.

## <a name="prerequisites"></a>Önkoşullar

- Herhangi bir sürümünü [Visual Studio 2017 veya üstü](https://visualstudio.microsoft.com/downloads/),

- NuGet paketi olarak kullanılabilen [Json.NET](https://www.newtonsoft.com/json) çerçevesi. Visual Studio'da bir NuGet paketi olarak Newtonsoft.Json yüklemek için:
    
    1. Projenize sağ tıklayın **Çözüm Gezgini**.
    2. Seçin **NuGet paketlerini Yönet**.
    3. Arama *Newtonsoft.Json* paketini ve yükleme.

- Linux/MacOS kullanıyorsanız, bu uygulamayı kullanarak çalıştırılabilir [Mono](https://www.mono-project.com/).

- Bir JSON dosyası içeren zaman serisi verilerini işaret eder. Bu Hızlı Başlangıç için örnek veri çubuğunda bulunabilir [GitHub](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/request-data.json).

[!INCLUDE [cognitive-services-anomaly-detector-data-requirements](../../../../includes/cognitive-services-anomaly-detector-data-requirements.md)]

[!INCLUDE [cognitive-services-anomaly-detector-signup-requirements](../../../../includes/cognitive-services-anomaly-detector-signup-requirements.md)]

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
    ```

2. Abonelik anahtarınız ve uç noktanız için değişkenler oluşturun. Anomali algılama için kullanabileceğiniz bir URI'leri aşağıdadır. Bunlar daha sonra API oluşturmak için hizmet uç noktanıza eklenir istek URL'lerini.

    |Algılama yöntemi  |URI  |
    |---------|---------|
    |Batch algılama    | `/anomalydetector/v1.0/timeseries/entire/detect`        |
    |En son veri noktasına algılama     | `/anomalydetector/v1.0/timeseries/last/detect`        |
    
    ```csharp
    // Replace the subscriptionKey string value with your valid subscription key.
    const string subscriptionKey = "[YOUR_SUBSCRIPTION_KEY]";
    // Replace the endpoint URL with the correct one for your subscription. 
    // Your endpoint can be found in the Azure portal. For example: https://westus2.api.cognitive.microsoft.com
    const string endpoint = "[YOUR_ENDPOINT_URL]";
    // Replace the dataPath string with a path to the JSON formatted time series data.
    const string dataPath = "[PATH_TO_TIME_SERIES_DATA]";
    const string latestPointDetectionUrl = "/anomalydetector/v1.0/timeseries/last/detect";
    const string batchDetectionUrl = "/anomalydetector/v1.0/timeseries/entire/detect";
    ```

## <a name="create-a-function-to-send-requests"></a>İstekleri göndermek için bir işlev oluşturma

1. Adlı yeni bir zaman uyumsuz işlev oluşturma `Request` yukarıda oluşturulan değişkenleri alır.

2. İstemcinin güvenlik protokolü ve üst bilgi bilgileri kullanarak bir `HttpClient` nesne. Abonelik anahtarınızı eklediğinizden emin olun `Ocp-Apim-Subscription-Key` başlığı. Oluşturup bir `StringContent` istek nesnesi.

3. İsteği Gönder `PostAsync()`ve ardından yanıtı döndürür.

```csharp
static async Task<string> Request(string apiAddress, string endpoint, string subscriptionKey, string requestData){
    using (HttpClient client = new HttpClient { BaseAddress = new Uri(apiAddress) }){
        System.Net.ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls12 | SecurityProtocolType.Tls11 | SecurityProtocolType.Tls;
        client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

        var content = new StringContent(requestData, Encoding.UTF8, "application/json");
        var res = await client.PostAsync(endpoint, content);
        return await res.Content.ReadAsStringAsync();
    }
}
```

## <a name="detect-anomalies-as-a-batch"></a>Toplu iş olarak anomalileri algılayın

1. Adlı yeni bir işlev oluşturma `detectAnomaliesBatch()`. Bir isteği oluşturmak ve çağırarak göndermek `Request()` uç noktanızı, abonelik anahtarı, batch anomali algılama için URL ve zaman serisi verileri ile işlevi.

2. JSON nesnesi seri durumdan ve konsola yazar.

3. Yanıt içeriyorsa `code` alan, hata kodu ve hata iletisi yazdırın. 

4. Aksi takdirde, anomalileri konumları, veri kümesinde bulun. Yanıtın `isAnomaly` alan bir veri noktasına bir anomali olup olmadığını her biri gösterir Boole değerleri dizisi içerir. Bu bir dize dizisi ile yanıt nesnenin dönüştürmek `ToObject<bool[]>()` işlevi. Dizi aracılığıyla yineleme ve herhangi bir dizin yazdırma `true` değerleri. Karşılanmadığı, bu değerler, anormal veri noktası dizini karşılık gelir.

```csharp
static void detectAnomaliesBatch(string requestData){
    System.Console.WriteLine("Detecting anomalies as a batch");

    var result = Request(
        endpoint,
        batchDetectionUrl,
        subscriptionKey,
        requestData).Result;

    dynamic jsonObj = Newtonsoft.Json.JsonConvert.DeserializeObject(result);
    System.Console.WriteLine(jsonObj);

    if (jsonObj["code"] != null){
        System.Console.WriteLine($"Detection failed. ErrorCode:{jsonObj["code"]}, ErrorMessage:{jsonObj["message"]}");
    }
    else{
        bool[] anomalies = jsonObj["isAnomaly"].ToObject<bool[]>();
        System.Console.WriteLine("\nAnomalies detected in the following data positions:");
        for (var i = 0; i < anomalies.Length; i++){
            if (anomalies[i])
            {
                System.Console.Write(i + ", ");
            }
        }
    }
}
```

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>En son veri noktası durumunu anomali algılama

1. Adlı yeni bir işlev oluşturma `detectAnomaliesLatest()`. Bir isteği oluşturmak ve çağırarak göndermek `Request()` uç noktanızı, abonelik anahtarı, en son noktası anomali algılama için URL ve zaman serisi verileri ile işlevi.

2. JSON nesnesi seri durumdan ve konsola yazar.

```csharp
static void detectAnomaliesLatest(string requestData){
    System.Console.WriteLine("\n\nDetermining if latest data point is an anomaly");
    var result = Request(
        endpoint,
        latestPointDetectionUrl,
        subscriptionKey,
        requestData).Result;

    dynamic jsonObj = Newtonsoft.Json.JsonConvert.DeserializeObject(result);
    System.Console.WriteLine(jsonObj);
}
```

## <a name="load-your-time-series-data-and-send-the-request"></a>Zaman serisi verilerinizi yüklemek ve isteği gönder

1. Uygulamanızın ana yöntemde JSON zaman serisi verilerinizle yük `File.ReadAllText()`. 

2. Yukarıda oluşturulan anomali algılama işlevlerini çağırın. Kullanım `System.Console.ReadKey()` için uygulamayı çalıştırdıktan sonra konsol penceresini açık tutun.

```csharp
static void Main(string[] args){

    var requestData = File.ReadAllText(dataPath);

    detectAnomaliesBatch(requestData);
    detectAnomaliesLatest(requestData);

    System.Console.ReadKey();
}
```

### <a name="example-response"></a>Örnek yanıt

Başarılı bir yanıt JSON biçiminde döndürülür. GitHub üzerinde JSON yanıtı görüntülemek için aşağıdaki bağlantılara tıklayın:
* [Örnek batch algılama yanıt](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/batch-response.json)
* [En son noktası algılama yanıt örneği](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/latest-point-response.json)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [REST API başvurusu](https://westus2.dev.cognitive.microsoft.com/docs/services/AnomalyDetector/operations/post-timeseries-entire-detect)
