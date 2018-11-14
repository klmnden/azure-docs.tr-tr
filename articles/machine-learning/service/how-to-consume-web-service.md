---
title: Web hizmeti dağıtımları - Azure Machine Learning kullanma
description: Bir Azure Machine Learning modelini dağıtma tarafından oluşturulan bir web hizmetinin nasıl kullanılacağı hakkında bilgi edinin. Bir Azure Machine Learning modelini dağıtma, bir REST API'sini kullanıma sunan bir web hizmeti oluşturur. İstemciler için tercih ettiğiniz programlama dilini kullanarak bu API oluşturabilirsiniz. Bu belgede, Python kullanarak API erişmeyi öğrenin ve C#.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: raymondl
author: raymondlaghaeian
ms.reviewer: larryfr
ms.date: 10/30/2018
ms.openlocfilehash: 0ad39048a6b175a30ac7c5cdc346d0858c3719ef
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51621879"
---
# <a name="consume-an-azure-machine-learning-model-deployed-as-a-web-service"></a>Bir web hizmeti olarak bir Azure Machine Learning modeli kullanma

Bir Azure Machine Learning modeli bir web hizmeti olarak dağıtma, bir REST API oluşturur. Bu API için veri göndermek ve modeli tarafından döndürülen tahmin alırsınız. Bu belgede, web hizmeti kullanmak için istemcileri oluşturmayı öğrenin C#, Go, Java ve Python.

Bir web hizmeti, görüntüyü Azure Container örneği, Azure Kubernetes hizmeti veya Project Brainwave (alanda programlanabilir kapı dizileri) dağıttığınızda oluşturulur. Görüntüleri, kayıtlı bir model ve puanlama dosyaları oluşturulur. Bir web hizmetine erişmek için kullanılan URI kullanılarak alınabilir [Azure Machine Learning SDK'sı](https://docs.microsoft.com/en-us/python/api/overview/azure/ml/intro?view=azure-ml-py). Kimlik doğrulamasını etkinleştirdiyseniz, kimlik doğrulama anahtarlarını almak için SDK'yı da kullanabilirsiniz.

Bir XML web hizmeti kullanan bir istemci oluşturma, genel iş akışı şöyledir:

1. Bağlantı bilgilerini almak için SDK'yi kullanın
1. Model tarafından kullanılan istek verisi türünü belirleme
1. Web hizmetini çağıran bir uygulama oluşturun

## <a name="connection-information"></a>Bağlantı bilgileri

> [!NOTE]
> Azure Machine Learning SDK'sı, web hizmeti bilgileri almak için kullanılır. Bir Python SDK'sı budur. Web hizmetleri hakkında bilgi almak için kullanılır, ancak bir istemci hizmeti oluşturmak için herhangi bir dil kullanabilirsiniz.

Web hizmeti bağlantı bilgileri, Azure Machine Learning SDK'sı kullanılarak alınabilir. [Azureml.core.Webservice](https://docs.microsoft.com/en-us/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py) sınıfı, bir istemci oluşturmak için gereken bilgileri sağlar. Aşağıdaki `Webservice` bir istemci uygulaması oluştururken yararlı olan özellikler:

* `auth_enabled` -Kimlik doğrulama etkinse `True`; Aksi takdirde `False`.
* `scoring_uri` -REST API adresi.

Dağıtılan web hizmetleri için bu bilgileri almak için üç yol vardır:

* Bir model dağıttığınızda bir `Webservice` nesne hizmetiyle ilgili bilgi döndürülür:

    ```python
    service = Webservice.deploy_from_model(name='myservice',
                                           deployment_config=myconfig,
                                           models=[model],
                                           image_config=image_config,
                                           workspace=ws)
    print(service.scoring_uri)
    ```

* Kullanabileceğiniz `Webservice.list` bir listesini almak için çalışma alanınızdaki modelleri için web hizmetleri dağıtıldı. Döndürülen bilgileri listesini daraltmak için filtre ekleyebilirsiniz. Ne hakkında daha fazla bilgi filtrelenebilen için bkz: [Webservice.list](https://docs.microsoft.com/en-us/python/api/azureml-core/azureml.core.webservice.webservice.webservice?view=azure-ml-py#list) başvuru belgeleri.

    ```python
    services = Webservice.list(ws)
    print(services[0].scoring_uri)
    ```

* Dağıtılan hizmetin adını biliyorsanız, yeni bir örneğini oluşturabilirsiniz `Webservice` ve parametrelere çalışma ve hizmet adını girin. Yeni nesne dağıtılan hizmeti hakkında bilgi içerir.

    ```python
    service = Webservice(workspace=ws, name='myservice')
    print(service.scoring_uri)
    ```

### <a name="authentication-key"></a>Kimlik doğrulama anahtarı

Kimlik doğrulaması anahtarları, bir dağıtım için kimlik doğrulaması etkinleştirildiğinde otomatik olarak oluşturulur.

* Kimlik doğrulaması __varsayılan olarak etkin__ dağıtım yapılırken __Azure Kubernetes hizmeti__.
* Kimlik doğrulaması __varsayılan olarak devre dışı__ dağıtım yapılırken __Azure container Instances__.

Kimlik doğrulaması denetlemek için kullanın `auth_enabled` oluşturulurken veya güncelleştirilirken bir dağıtım parametresi.

Kimlik doğrulamasını etkinleştirdiyseniz, kullanabileceğiniz `get_keys` yönteminin bir birincil ve ikincil kimlik doğrulama anahtarı almak için:

```python
primary, secondary = service.get_keys()
print(primary)
```

> [!IMPORTANT]
> Bir anahtarı yeniden oluşturmak ihtiyacınız varsa [ `service.regen_key` ](https://docs.microsoft.com/en-us/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#regen-key).

## <a name="request-data"></a>İstek verileri

REST API isteği aşağıdaki yapıya sahip bir JSON belge gövdesinin bekliyor:

```json
{
    "data":
        [
            <model-specific-data-structure>
        ]
}
```

> [!IMPORTANT]
> Verilerin yapısı, hangi Puanlama betiği ve hizmet expect modelinde ile eşleşmesi gerekiyor. Puanlama betiği modele iletmeden önce verileri değiştirebilir.

Örneğin, modelde [Not Defteri içinde Train](https://github.com/Azure/MachineLearningNotebooks/tree/master/01.getting-started/01.train-within-notebook) örnek 10 sayıdan oluşan bir diziyi bekliyor. Bu örnek için Puanlama betiğine istekten Numpy dizisi oluşturur ve modele geçirir. Aşağıdaki örnek, bu hizmet bekliyor veri gösterir:

```json
{
    "data": 
        [
            [
                0.0199132141783263, 
                0.0506801187398187, 
                0.104808689473925, 
                0.0700725447072635, 
                -0.0359677812752396, 
                -0.0266789028311707, 
                -0.0249926566315915, 
                -0.00259226199818282, 
                0.00371173823343597, 
                0.0403433716478807
            ]
        ]
}
``` 

Web hizmeti, birden çok bir istekteki veri kümelerini kabul edebilir. Yanıt bir dizi içeren bir JSON belgesini döndürür.

## <a name="call-the-service-c"></a>Hizmet çağrısı (C#)

Bu örnek nasıl kullanılacağını gösterir C# oluşturulan web hizmeti çağırmak amacıyla [eğitme Not Defteri içinde](https://github.com/Azure/MachineLearningNotebooks/tree/master/01.getting-started/01.train-within-notebook) örneği:

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Net.Http;
using System.Net.Http.Headers;
using Newtonsoft.Json;

namespace MLWebServiceClient
{
    // The data structure expected by the service
    internal class InputData
    {
        [JsonProperty("data")]
        // The service used by this example expects an array containing
        //   one or more arrays of doubles
        internal double[,] data;
    }
    class Program
    {
        static void Main(string[] args)
        {
            // Set the scoring URI and authentication key
            string scoringUri = "<your web service URI>";
            string authKey = "<your key>";

            // Set the data to be sent to the service.
            // In this case, we are sending two sets of data to be scored.
            InputData payload = new InputData();
            payload.data = new double[,] {
                {
                    0.0199132141783263,
                    0.0506801187398187,
                    0.104808689473925,
                    0.0700725447072635,
                    -0.0359677812752396,
                    -0.0266789028311707,
                    -0.0249926566315915,
                    -0.00259226199818282,
                    0.00371173823343597,
                    0.0403433716478807
                },
                {
                    -0.0127796318808497, 
                    -0.044641636506989, 
                    0.0606183944448076, 
                    0.0528581912385822, 
                    0.0479653430750293, 
                    0.0293746718291555, 
                    -0.0176293810234174, 
                    0.0343088588777263, 
                    0.0702112981933102, 
                    0.00720651632920303
                }
            };

            // Create the HTTP client
            HttpClient client = new HttpClient();
            // Set the auth header. Only needed if the web service requires authentication.
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", authKey);

            // Make the request
            try {
                var request = new HttpRequestMessage(HttpMethod.Post, new Uri(scoringUri));
                request.Content = new StringContent(JsonConvert.SerializeObject(payload));
                request.Content.Headers.ContentType = new MediaTypeHeaderValue("application/json");
                var response = client.SendAsync(request).Result;
                // Display the response from the web service
                Console.WriteLine(response.Content.ReadAsStringAsync().Result);
            }
            catch (Exception e)
            {
                Console.Out.WriteLine(e.Message);
            }
        }
    }
}
```

Döndürülen sonuçlar için aşağıdaki JSON belgesini benzerdir:

```json
[217.67978776218715, 224.78937091757172]
```

## <a name="call-the-service-go"></a>Arama Hizmeti (Git)

Bu örnekte oluşturulan web hizmeti çağırmak için Git kullanmayı gösteren [Not Defteri içinde Train](https://github.com/Azure/MachineLearningNotebooks/tree/master/01.getting-started/01.train-within-notebook) örnek:

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
)

// Features for this model are an array of decimal values
type Features []float64

// The web service input can accept multiple sets of values for scoring
type InputData struct {
    Data []Features `json:"data",omitempty`
}

// Define some example data
var exampleData = []Features{
    []float64{
        0.0199132141783263, 
        0.0506801187398187, 
        0.104808689473925, 
        0.0700725447072635, 
        -0.0359677812752396, 
        -0.0266789028311707, 
        -0.0249926566315915, 
        -0.00259226199818282, 
        0.00371173823343597, 
        0.0403433716478807,
    },
    []float64{
        -0.0127796318808497, 
        -0.044641636506989, 
        0.0606183944448076, 
        0.0528581912385822, 
        0.0479653430750293, 
        0.0293746718291555, 
        -0.0176293810234174, 
        0.0343088588777263, 
        0.0702112981933102, 
        0.00720651632920303,
    },
}

// Set to the URI for your service
var serviceUri string = "<your web service URI>"
// Set to the authentication key (if any) for your service
var authKey string = "<your key>"

func main() {
    // Create the input data from example data
    jsonData := InputData{
        Data: exampleData,
    }
    // Create JSON from it and create the body for the HTTP request
    jsonValue, _ := json.Marshal(jsonData)
    body := bytes.NewBuffer(jsonValue)

    // Create the HTTP request
    client := &http.Client{}
    request, err := http.NewRequest("POST", serviceUri, body)
    request.Header.Add("Content-Type", "application/json")

    // These next two are only needed if using an authentication key
    bearer := fmt.Sprintf("Bearer %v", authKey)
    request.Header.Add("Authorization", bearer)

    // Send the request to the web service
    resp, err := client.Do(request)
    if err != nil {
        fmt.Println("Failure: ", err)
    }

    // Display the response received
    respBody, _ := ioutil.ReadAll(resp.Body)
    fmt.Println(string(respBody))
}
```

Döndürülen sonuçlar için aşağıdaki JSON belgesini benzerdir:

```json
[217.67978776218715, 224.78937091757172]
```

## <a name="call-the-service-java"></a>Arama Hizmeti (Java)

Bu örnek, Java oluşturulan web hizmetini çağırmak için nasıl kullanılacağını gösterir. [Not Defteri içinde Train](https://github.com/Azure/MachineLearningNotebooks/tree/master/01.getting-started/01.train-within-notebook) örneği:

```java
import java.io.IOException;
import org.apache.http.client.fluent.*;
import org.apache.http.entity.ContentType;
import org.json.simple.JSONArray;
import org.json.simple.JSONObject;

public class App {
    // Handle making the request
    public static void sendRequest(String data) {
        // Replace with the scoring_uri of your service
        String uri = "<your web service URI>";
        // If using authentication, replace with the auth key
        String key = "<your key>";
        try {
            // Create the request
            Content content = Request.Post(uri)
            .addHeader("Content-Type", "application/json")
            // Only needed if using authentication
            .addHeader("Authorization", "Bearer " + key)
            // Set the JSON data as the body
            .bodyString(data, ContentType.APPLICATION_JSON)
            // Make the request and display the response.
            .execute().returnContent();
            System.out.println(content);
        }
        catch (IOException e) {
            System.out.println(e);
        }
    }
    public static void main(String[] args) {
        // Create the data to send to the service
        JSONObject obj = new JSONObject();
        // In this case, it's an array of arrays
        JSONArray dataItems = new JSONArray();
        // Inner array has 10 elements
        JSONArray item1 = new JSONArray();
        item1.add(0.0199132141783263);
        item1.add(0.0506801187398187);
        item1.add(0.104808689473925);
        item1.add(0.0700725447072635);
        item1.add(-0.0359677812752396);
        item1.add(-0.0266789028311707);
        item1.add(-0.0249926566315915);
        item1.add(-0.00259226199818282);
        item1.add(0.00371173823343597);
        item1.add(0.0403433716478807);
        // Add the first set of data to be scored
        dataItems.add(item1);
        // Create and add the second set
        JSONArray item2 = new JSONArray();
        item2.add(-0.0127796318808497);
        item2.add(-0.044641636506989);
        item2.add(0.0606183944448076);
        item2.add(0.0528581912385822);
        item2.add(0.0479653430750293);
        item2.add(0.0293746718291555);
        item2.add(-0.0176293810234174);
        item2.add(0.0343088588777263);
        item2.add(0.0702112981933102);
        item2.add(0.00720651632920303);
        dataItems.add(item2);
        obj.put("data", dataItems);

        // Make the request using the JSON document string
        sendRequest(obj.toJSONString());
    }
}
```

Döndürülen sonuçlar için aşağıdaki JSON belgesini benzerdir:

```json
[217.67978776218715, 224.78937091757172]
```

## <a name="call-the-service-python"></a>Arama Hizmeti (Python)

Bu örnek Python oluşturulan web hizmetini çağırmak için nasıl kullanılacağını gösterir [Not Defteri içinde Train](https://github.com/Azure/MachineLearningNotebooks/tree/master/01.getting-started/01.train-within-notebook) örneği:

```python
import requests
import requests
import json

# URL for the web service
scoring_uri = '<your web service URI>'
# If the service is authenticated, set the key
key = '<your key>'

# Two sets of data to score, so we get two results back
data = {"data": 
            [
                [
                    0.0199132141783263, 
                    0.0506801187398187, 
                    0.104808689473925, 
                    0.0700725447072635, 
                    -0.0359677812752396, 
                    -0.0266789028311707, 
                    -0.0249926566315915, 
                    -0.00259226199818282, 
                    0.00371173823343597, 
                    0.0403433716478807
                ],
                [
                    -0.0127796318808497, 
                    -0.044641636506989, 
                    0.0606183944448076, 
                    0.0528581912385822, 
                    0.0479653430750293, 
                    0.0293746718291555, 
                    -0.0176293810234174, 
                    0.0343088588777263, 
                    0.0702112981933102, 
                    0.00720651632920303]
            ]
        }
# Convert to JSON string
input_data = json.dumps(data)

# Set the content type
headers = { 'Content-Type':'application/json' }
# If authentication is enabled, set the authorization header
headers['Authorization']=f'Bearer {key}'

# Make the request and display the response
resp = requests.post(scoring_uri, input_data, headers = headers)
print(resp.text)

```

Döndürülen sonuçlar için aşağıdaki JSON belgesini benzerdir:

```JSON
[217.67978776218715, 224.78937091757172]
```

## <a name="next-steps"></a>Sonraki adımlar

Bir istemci için Dağıtılmış bir modelinin nasıl oluşturulacağını öğrendiniz, bilgi nasıl [model IOT Edge cihazına dağıtma](how-to-deploy-to-iot.md).
