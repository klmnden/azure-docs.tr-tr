---
title: Azure Machine Learning modeli Yönetim web hizmeti tüketim | Microsoft Docs
description: Bu belgede adımları ve kavramları açıklamaktadır Azure Machine Learning modeli Yönetimi'ni kullanarak dağıtılan web hizmetlerini tüketen söz konusu.
services: machine-learning
author: raymondlaghaeian
ms.author: raymondl
manager: hjerez
ms.reviewer: jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/06/2017
ms.openlocfilehash: 0976e2dca909781ade76c742cc99746e1123307d
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="consuming-web-services"></a>Web Hizmetleri kullanma
Model bir gerçek zamanlı web hizmeti olarak dağıtma sonra veri göndermek ve çeşitli platformlar ve uygulamaları Öngörüler alın. Gerçek zamanlı web hizmetini Öngörüler almak için bir REST API gösterir. Web hizmeti aynı anda bir veya daha fazla Öngörüler almak için tek veya birden çok satır biçiminde veri gönderebilir.

İle [Azure Machine Learning Web hizmeti](model-management-service-deploy.md), bir dış uygulama zaman uyumlu olarak Tahmine dayalı modelle hizmet URL'si için HTTP POST çağrısı yaparak iletişim kurar. Bir web hizmeti çağrısı yapmak için tahmin dağıtmak ve istek verileri POST istek gövdesi yerleştirin, oluşturduğunuz API anahtarını belirtmek istemci uygulaması gerekir.

API anahtarları yalnızca küme dağıtım modunda kullanılabilir olduğunu unutmayın. Yerel web hizmetleri anahtarlara sahip değil.

## <a name="service-deployment-options"></a>Hizmet dağıtım seçenekleri
Azure Machine Learning Web Hizmetleri, üretim ve test senaryoları için bulut tabanlı kümeler ve docker altyapısını kullanarak yerel iş istasyonları için dağıtılabilir. Her iki durumda da Tahmine dayalı modelde işlevselliğini aynı kalır. Hata ayıklama için yerel dağıtım kullanılabilse de kullanıcı çözüm Azure kapsayıcı hizmetlerini esas ve küme tabanlı dağıtım ölçeklenebilir sağlar. 

API ve Azure Machine Learning CLI sağlar oluşturmak ve yönetmek için kullanışlı komutları ortamlar kullanarak hizmet dağıtımları için işlem ```az ml env``` seçeneği. 

## <a name="list-deployed-services-and-images"></a>Dağıtılan listesi Hizmetleri ve görüntüleri
Şu anda dağıtılmış hizmet ve Docker görüntüleri CLI komutu kullanarak listeleyebilirsiniz ```az ml service list realtime -o table```. Bu komut her zaman geçerli işlem ortamı bağlamında çalıştığını unutmayın. Dağıtılan Hizmetleri geçerli olması için ayarlanmamış bir ortamda göstermeniz gerekmez. Ortam kullanımı ayarlamak için ```az ml env set```. 

## <a name="get-service-information"></a>Hizmet bilgileri alma
Web hizmeti başarıyla dağıtıldıktan sonra hizmet uç noktası çağırmak için hizmet URL'sini ve diğer ayrıntıları almak için aşağıdaki komutu kullanın. 

```
az ml service usage realtime -i <web service id>
```

Bu komut, hizmet URL'si, gerekli istek üstbilgileri, swagger URL'si ve dağıtım sırasında hizmeti API şeması sağlandıysa hizmetini çağırmak için örnek veri çıkışı yazdırır.

Giriş verisi örnek CLI komutu girerek bir HTTP isteği oluşturma olmadan CLI hizmetinden doğrudan test edebilirsiniz:

```
az ml service run realtime -i <web service id> -d "Your input data"
```

## <a name="get-the-service-api-key"></a>Hizmet API anahtarını alın
Web hizmet anahtarı almak için aşağıdaki komutu kullanın:

```
az ml service keys realtime -i <web service id>
```
HTTP isteği oluştururken, yetkilendirme üstbilgisinde anahtarı kullanın: "Yetkilendirmesi": "taşıyıcı <key>"

## <a name="get-the-service-swagger-description"></a>Hizmet Swagger açıklaması alma
Hizmet API şeması sağlandıysa hizmet uç noktası Swagger belgesinin adresindeki kullanılabilmesini ```http://<ip>/api/v1/service/<service name>/swagger.json```. Swagger belgesinin otomatik olarak hizmeti istemcisi oluşturmak ve beklenen giriş verilerinin ve hizmeti ile ilgili diğer ayrıntıları keşfetmek için kullanılabilir.

## <a name="get-service-logs"></a>Hizmet Günlükleri alma
Hizmet davranışlarını anlamak ve sorunları tanılamak için hizmet günlükleri almak için birkaç yolu vardır:
- CLI komutu ```az ml service logs realtime -i <service id>```. Bu komut, hem küme hem de yerel modları çalışır.
- Hizmet günlüğü dağıtımın etkinleştirilirse, hizmet günlükleri de AppInsight için gönderilir. CLI komutu ```az ml service usage realtime -i <service id>``` AppInsight URL gösterir. 2-5 dakika tarafından AppInsight günlükleri gecikebilir unutmayın.
- Küme günlükleri geçerli küme ortamı ayarladığınızda bağlanan Kubernetes Konsolu aracılığıyla görüntülenebilir ```az ml env set```
- Yerel olarak çalıştığı yerel docker günlükleri docker altyapısı günlükleri ile kullanılabilir.

## <a name="call-the-service-using-c"></a>C# kullanarak hizmet çağırın
C# konsol uygulamasından bir istek göndermek için hizmet URL'si kullanın. 

1. Visual Studio'da yeni bir konsol uygulaması oluşturun: 
    * Menüye tıklayın, Dosya -> Yeni Proje ->
    * Altında Visual Studio C# Windows sınıfı Masaüstü'nü tıklatın, sonra konsol uygulaması seçin.
2. Girin _MyFirstService_ projesinin adı ardından Tamam'ı tıklatın.
3. Proje başvuruları kümesi başvuruları _System.Net_, ve _System.Net.Http_.
4. Tıklatın Araçlar -> NuGet Paket Yöneticisi Paket Yöneticisi konsolu -> sonra Microsoft.AspNet.WebApi.Client paketi yükleyin.
5. Program.cs dosyasını açın ve kodu aşağıdaki kodla değiştirin:
6. Güncelleştirme _SERVICE_URL_ ve _apı_key_ web hizmetiniz bilgileriyle parametreleri.
7. Projeyi çalıştırın.

```csharp
using System;
using System.Collections.Generic;
using System.Net.Http;
using System.Net.Http.Headers;
using Newtonsoft.Json;

namespace MyFirstService
{
    class Program
    {
        // Web Service URL (update it with your service url)
        private const string SERVICE_URL = "http://<service ip address>:80/api/v1/service/<service name>/score";
        private const string API_KEY = "your service key";

        static void Main(string[] args)
        {
            Program.PostRequest();
            Console.ReadLine();
        }

        private static void PostRequest()
        {
            HttpClient client = new HttpClient();
            client.BaseAddress = new Uri(SERVICE_URL);
            //For local web service, comment out this line.
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", API_KEY);

            var inputJson = new List<RequestPayload>();
            RequestPayload payload = new RequestPayload();
            List<InputDf> inputDf = new List<InputDf>();
            inputDf.Add(new InputDf()
            {
                feature1 = value1,
                feature2 = value2,
            });
            payload.Input_df_list = inputDf;
            inputJson.Add(payload);

            try
            {
                var request = new HttpRequestMessage(HttpMethod.Post, string.Empty);
                request.Content = new StringContent(JsonConvert.SerializeObject(payload));
                request.Content.Headers.ContentType = new MediaTypeHeaderValue("application/json");
                var response = client.SendAsync(request).Result;

                Console.Write(response.Content.ReadAsStringAsync().Result);
            }
            catch (Exception e)
            {
                Console.Out.WriteLine(e.Message);
            }
        }
        public class InputDf
        {
            public double feature1 { get; set; }
            public double feature2 { get; set; }
        }
        public class RequestPayload
        {
            [JsonProperty("input_df")]
            public List<InputDf> Input_df_list { get; set; }
        }
    }
}
```

## <a name="call-the-web-service-using-python"></a>Python kullanarak web hizmeti çağrısı
Gerçek zamanlı web hizmetiniz için bir istek göndermek için Python kullanın. 

1. Aşağıdaki kod örneği, yeni bir Python dosyaya kopyalayın.
2. Veri, url ve apı_key parametreleri güncelleştirin. Yerel web hizmetleri için 'Authorization' üst bilgisi kaldırın.
3. Kodu çalıştırın. 

```python
import requests
import json

data = "{\"input_df\": [{\"feature1\": value1, \"feature2\": value2}]}"
body = str.encode(json.dumps(data))

url = 'http://<service ip address>:80/api/v1/service/<service name>/score'
api_key = 'your service key' 
headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}

resp = requests.post(url, body, headers=headers)
resp.text
```
