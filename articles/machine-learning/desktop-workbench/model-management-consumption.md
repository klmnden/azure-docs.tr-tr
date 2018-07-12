---
title: Azure Machine Learning Model Yönetimi web hizmet tüketimi | Microsoft Docs
description: Bu belgede adımlar ve kavramlar açıklanır. Azure Machine Learning'de model Yönetimi'ni kullanarak dağıtılan web hizmetleri tüketen söz konusu.
services: machine-learning
author: raymondlaghaeian
ms.author: raymondl
manager: hjerez
ms.reviewer: jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/06/2017
ms.openlocfilehash: 4a49ccff68003cf7b81a7d945176992a2893d1ac
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38972638"
---
# <a name="consuming-web-services"></a>Web hizmetlerini kullanma
Model bir gerçek zamanlı web hizmeti olarak dağıtma sonra veri göndermek ve çeşitli Platform ve uygulama arasından Öngörüler edinin. Gerçek zamanlı web hizmetini, Öngörüler almak için bir REST API gösterir. Aynı anda bir veya daha fazla Öngörüler almak için tek veya çok satırlı biçimde web hizmetine veri gönderebilir.

İle [Azure Machine Learning Web hizmetini](model-management-service-deploy.md), bir dış uygulamanın zaman uyumlu olarak Tahmine dayalı modelle hizmet URL'sine HTTP POST çağrı yaparak iletişim kurar. Web hizmeti çağrısı yapmak için istemci uygulama bir tahmin dağıtıp POST isteğinin gövdesi içinde istek verilerinizden oluşturulan API anahtarını belirtmesi gerekir.

> [!NOTE]
> API anahtarları yalnızca küme dağıtım modunda kullanılabilir olduğunu unutmayın. Yerel web hizmetlerinin anahtarları yoktur.

## <a name="service-deployment-options"></a>Hizmet dağıtım seçenekleri
Azure Machine Learning Web Hizmetleri, hem üretim hem de test senaryoları için bulut tabanlı kümeler için ve yerel iş istasyonlarına docker altyapısı kullanılarak dağıtılabilir. Her iki durumda Tahmine dayalı modelde işlevselliğini aynı kalır. Küme tabanlı dağıtım ölçeklenebilir sağlar ve hata ayıklama için yerel dağıtımı kullanılabilse tabanlı Azure Container Service üzerinde yüksek performanslı çözümüdür. 

API ve Azure Machine Learning CLI sağlar oluşturmak ve yönetmek için kullanışlı komutları işlem ortamları kullanarak hizmet dağıtımları için ```az ml env``` seçeneği. 

## <a name="list-deployed-services-and-images"></a>Liste dağıtılan hizmetler ve görüntüleri
Şu anda dağıtılmış hizmet ve CLI komutunu kullanarak Docker görüntüleri listeleyebilirsiniz ```az ml service list realtime -o table```. Bu komut her zaman geçerli işlem ortamını bağlamında çalıştığını unutmayın. Dağıtılan Hizmetleri geçerli olması için ayarlanmamış bir ortamda göstermeniz gerekmez. Ortam kullanımı ayarlanacak ```az ml env set```. 

## <a name="get-service-information"></a>Hizmet bilgilerini alma
Web hizmeti başarıyla dağıtıldıktan sonra hizmet uç noktasını çağırmak için hizmet URL'sini ve diğer ayrıntıları almak için aşağıdaki komutu kullanın. 

```
az ml service usage realtime -i <web service id>
```

Bu komut, hizmet URL'si, gerekli istek üst bilgileri, swagger URL'si ve dağıtım sırasında hizmet API şeması sağlandıysa hizmetini çağırmak için örnek verileri yazdırır.

Bir HTTP isteği oluşturma gerek kalmadan giriş verileriyle örnek CLI komutunu girerek hizmeti doğrudan CLI test edebilirsiniz:

```
az ml service run realtime -i <web service id> -d "Your input data"
```

## <a name="get-the-service-api-key"></a>Hizmet API anahtarı alma
Web hizmeti anahtarı almak için aşağıdaki komutu kullanın:

```
az ml service keys realtime -i <web service id>
```
HTTP isteği oluştururken, anahtar yetkilendirme üst bilgisinde kullanılır: "Yetkilendirme": "taşıyıcı <key>"

## <a name="get-the-service-swagger-description"></a>Hizmet Swagger tanımı Al
Hizmet API şeması sağlandıysa, hizmet uç noktası, bir Swagger belgesi kullanılabilmesini ```http://<ip>/api/v1/service/<service name>/swagger.json```. Swagger belgesinin otomatik olarak hizmeti istemcisi oluşturur ve beklenen giriş verileri ve hizmetle ilgili diğer ayrıntıları keşfetmek için kullanılabilir.

## <a name="get-service-logs"></a>Hizmet günlüklerini alma
Hizmet davranışı anlayın ve sorunları tanılamak için hizmet günlüklerini almak için birkaç yol vardır:
- CLI komutunu ```az ml service logs realtime -i <service id>```. Bu komut, hem küme hem de yerel modları çalışır.
- Hizmet günlüğü dağıtımda etkinleştirilirse, hizmet günlükleri için bir Appınsight de gönderilir. CLI komutunu ```az ml service usage realtime -i <service id>``` Appınsight URL'sini gösterir. 2-5 dakikaya Appınsight günlükleri gecikebilir unutmayın.
- Küme günlükleri ile geçerli küme ortamında ayarladığınızda bağlanan Kubernetes Konsolu aracılığıyla görüntülenebilir ```az ml env set```
- Hizmet yerel olarak çalışırken, yerel docker günlükleri docker altyapısı günlükleri kullanılabilir.

## <a name="call-the-service-using-c"></a>C# ' ı kullanarak hizmeti çağırma
Bir C# konsol uygulaması istek göndermek için hizmet URL'sini kullanın. 

1. Visual Studio'da yeni bir konsol uygulaması oluşturun: 
    * Menüde, Dosya -> Yeni Proje ->
    * Visual Studio C# altında Windows sınıfı Masaüstü'nü tıklatın ve ardından konsol uygulaması seçin.
2. Girin `MyFirstService` projenin adı Tamam'a tıklayın.
3. Proje başvuruları başvuruları kümesine `System.Net`, ve `System.Net.Http`.
4. Tıklayın Araçlar -> NuGet Paket Yöneticisi -> Paket Yöneticisi konsolu ve sonra Yükle **System.NET.http.Formatting** paket.
5. Açık **Program.cs** dosya ve kodu aşağıdaki kodla değiştirin:
6. Güncelleştirme `SERVICE_URL` ve `API_KEY` bilgilerle web hizmeti parametreleri.
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

## <a name="call-the-web-service-using-python"></a>Python kullanarak web hizmetini çağırın
Gerçek zamanlı bir web hizmetiniz için bir istek göndermek için Python'ı kullanın. 

1. Aşağıdaki kod örneği, yeni bir Python dosyasına kopyalayın.
2. Verileri, url ve apı_key parametreleri güncelleştirin. Yerel web hizmetleri için 'Authorization' üst bilgisi kaldırın.
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
