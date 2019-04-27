---
title: Web hizmetini kullanma
titleSuffix: Azure Machine Learning Studio
description: Bir machine learning hizmeti Azure Machine Learning Studio dağıtıldıktan sonra RESTFul Web hizmeti gerçek zamanlı istek-yanıt hizmeti ya da bir toplu yürütme hizmeti olarak kullanılabilir.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 06/02/2017
ms.openlocfilehash: a537227a7003391122e10f7f39233040cef49db3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60751306"
---
# <a name="how-to-consume-an-azure-machine-learning-studio-web-service"></a>Bir Azure Machine Learning Studio web hizmetini kullanma

Bir Azure Machine Learning Studio'da Tahmine dayalı modeli bir Web hizmeti olarak dağıtma sonra veri göndermek ve Öngörüler almak için bir REST API kullanabilirsiniz. Gerçek zamanlı veya toplu iş modunda veriler gönderebilir.

Burada, Machine Learning Studio'yu kullanarak bir Machine Learning Web hizmeti oluşturma ve dağıtma hakkında daha fazla bilgi bulabilirsiniz:

* Machine Learning Studio'da bir deneme oluşturma hakkında bir öğretici için bkz. [ilk denemenizi oluşturma](create-experiment.md).
* Bir Web hizmeti dağıtma hakkında daha fazla ayrıntı için bkz. [Machine Learning Web hizmetini dağıtma](publish-a-machine-learning-web-service.md).
* Genel olarak, Machine Learning hakkında daha fazla bilgi için ziyaret [Machine Learning Belge Merkezi](https://azure.microsoft.com/documentation/services/machine-learning/).



## <a name="overview"></a>Genel Bakış
Azure Machine Learning Web hizmeti ile bir dış uygulama, gerçek zamanlı Machine Learning iş akışı Puanlama modeli ile iletişim kurar. Bir Machine Learning Web hizmeti çağrısı bir dış uygulamaya tahmin sonuçlarını döndürür. Machine Learning Web hizmeti çağrısı yapmak için tahmin dağıttığınızda oluşturulan bir API anahtarı geçirirsiniz. Machine Learning Web hizmeti bir web programlama projeleri için popüler bir mimari seçimi olan REST'i temel alır.

Azure Machine Learning Studio iki tür hizmet içerir:

* İstek-yanıt hizmeti (RRS) – düşük gecikme süresi, Machine Learning Studio'da oluşturulan ve dağıtılan durum bilgisiz modeller için arabirim sağlar yüksek oranda ölçeklenebilir bir hizmettir.
* Toplu yürütme hizmeti (BES) – bir zaman uyumsuz yapan veri kayıtları için toplu iş hizmettir.

Machine Learning Web Hizmetleri hakkında daha fazla bilgi için bkz. [Machine Learning Web hizmetini dağıtma](publish-a-machine-learning-web-service.md).

## <a name="get-an-azure-machine-learning-studio-authorization-key"></a>Bir Azure Machine Learning Studio yetkilendirme anahtarını alma
API anahtarları, denemenizi dağıttığınızda, Web hizmeti oluşturulur. Çeşitli konumlardan anahtarları alabilirsiniz.

### <a name="from-the-microsoft-azure-machine-learning-web-services-portal"></a>Microsoft Azure Machine Learning Web Hizmetleri portalından
Oturum [Microsoft Azure Machine Learning Web Hizmetleri](https://services.azureml.net) portalı.

Yeni Machine Learning Web hizmeti için API anahtarını almak için:

1. Azure Machine Learning Web Hizmetleri portalında **Web Hizmetleri** üst menü.
2. Anahtar almak istediğiniz Web hizmeti tıklayın.
3. Üst menüsünde **Tüket**.
4. Kopyalayıp kaydedin **birincil anahtar**.

Klasik Machine Learning Web hizmeti için API anahtarını almak için:

1. Azure Machine Learning Web Hizmetleri portalında **Klasik Web Hizmetleri** üst menü.
2. Web hizmeti ile çalıştığınız tıklayın.
3. Anahtar almak istediğiniz uç noktaya tıklayın.
4. Üst menüsünde **Tüket**.
5. Kopyalayıp kaydedin **birincil anahtar**.

### <a name="classic-web-service"></a>Klasik Web hizmeti
 Ayrıca, Machine Learning Studio'dan bir Klasik Web hizmeti için bir anahtar alabilirsiniz.

#### <a name="machine-learning-studio"></a>Machine Learning Studio
1. Machine Learning Studio'da tıklayın **WEB Hizmetleri** soldaki.
2. Bir Web hizmetine tıklayın. **API anahtarı** açıktır **PANO** sekmesi.

## <a id="connect"></a>Bir Machine Learning Web hizmetine bağlanma
HTTP istek ve yanıt destekleyen herhangi bir programlama dilini kullanarak bir Machine Learning Web hizmetine bağlanabilirsiniz. Örneklerde görüntüleyebileceğiniz C#, Python ve R bir Machine Learning Web hizmeti Yardım sayfası.

**Machine Learning API Yardım** Machine Learning API Yardım, bir Web hizmetini dağıttığınızda oluşturulur. Bkz: [öğretici 3: Kredi riski model dağıtma](tutorial-part3-credit-risk-deploy.md).
Machine Learning API Yardım bir tahmin Web hizmeti hakkındaki ayrıntıları içerir.

1. Web hizmeti ile çalıştığınız tıklayın.
2. API Yardım sayfasında görüntülemek istediğiniz uç noktaya tıklayın.
3. Üst menüsünde **Tüket**.
4. Tıklayın **API Yardım sayfası** istek-yanıt ya da toplu iş yürütme bitiş noktaları altında.

**Machine Learning API görünümüne yeni Web hizmeti için Yardım**

İçinde [Azure Machine Learning Web Hizmetleri portalını](https://services.azureml.net/):

1. Tıklayın **WEB Hizmetleri** en üstteki menüde.
2. Anahtar almak istediğiniz Web hizmeti tıklayın.

Tıklayın **kullanım Web hizmeti** istek-yanıt, toplu iş yürütme hizmetler ve örnek kodu bir URI'leri almak için C#, R ve Python.

Tıklayın **Swagger API'si** Swagger almak için sağlanan bir URI'leri API çağrısı için Belge tabanlı.

### <a name="c-sample"></a>C# örneği
Bir Machine Learning Web hizmetine bağlanmak için bir **HttpClient** ScoreData geçirme. ScoreData bir FeatureVector ScoreData temsil eden bir n-boyutlu vektör sayısal özelliklerini içerir. Machine Learning hizmeti için bir API anahtarı ile kimlik doğrulaması.

Machine Learning Web hizmetine bağlanmak için **System.NET.http.Formatting** NuGet paketi yüklü olmalıdır.

**Visual Studio'da System.NET.http.Formatting NuGet yükleyin**

1. UCI indirme kümesinden yayımlama: Yetişkin 2 sınıfı veri kümesi Web hizmeti.
2. **Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**’na tıklayın.
3. Seçin **Install-Package System.NET.http.Formatting**.

**Kod örneği çalıştırmak için**

1. Yayımlama "Örnek 1: Veri kümesi UCI ' indirin: Yetişkin 2 sınıfı veri kümesi"deneme, Machine Learning örnek koleksiyonun parçası.
2. ApiKey bir Web hizmetinden anahtarla atayın. Bkz: **bir Azure Machine Learning Studio yetkilendirme anahtarını alma** yukarıda.
3. İstek URI'si ile serviceUri atayın.

**İşte tam bir istek aşağıdaki gibi görünür.**
```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Net.Http;
using System.Net.Http.Formatting;
using System.Net.Http.Headers;
using System.Text;
using System.Threading.Tasks;

namespace CallRequestResponseService
{
    class Program
    {
        static void Main(string[] args)
        {
            InvokeRequestResponseService().Wait();
        }

        static async Task InvokeRequestResponseService()
        {
            using (var client = new HttpClient())
            {
                var scoreRequest = new
                {
                    Inputs = new Dictionary<string, List<Dictionary<string, string>>> () {
                        {
                            "input1",
                            // Replace columns labels with those used in your dataset
                            new List<Dictionary<string, string>>(){new Dictionary<string, string>(){
                                    {
                                        "column1", "value1"
                                    },
                                    {
                                        "column2", "value2"
                                    },
                                    {
                                        "column3", "value3"
                                    }
                                }
                            }
                        },
                    },
                    GlobalParameters = new Dictionary<string, string>() {}
                };

                // Replace these values with your API key and URI found on https://services.azureml.net/
                const string apiKey = "<your-api-key>"; 
                const string apiUri = "<your-api-uri>";
                
                client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue( "Bearer", apiKey);
                client.BaseAddress = new Uri(apiUri);

                // WARNING: The 'await' statement below can result in a deadlock
                // if you are calling this code from the UI thread of an ASP.NET application.
                // One way to address this would be to call ConfigureAwait(false)
                // so that the execution does not attempt to resume on the original context.
                // For instance, replace code such as:
                //      result = await DoSomeTask()
                // with the following:
                //      result = await DoSomeTask().ConfigureAwait(false)

                HttpResponseMessage response = await client.PostAsJsonAsync("", scoreRequest);

                if (response.IsSuccessStatusCode)
                {
                    string result = await response.Content.ReadAsStringAsync();
                    Console.WriteLine("Result: {0}", result);
                }
                else
                {
                    Console.WriteLine(string.Format("The request failed with status code: {0}", response.StatusCode));

                    // Print the headers - they include the request ID and the timestamp,
                    // which are useful for debugging the failure
                    Console.WriteLine(response.Headers.ToString());

                    string responseContent = await response.Content.ReadAsStringAsync();
                    Console.WriteLine(responseContent);
                }
            }
        }
    }
}
```

### <a name="python-sample"></a>Python örneği
Bir Machine Learning Web hizmetine bağlanmak için kullanmak **urllib2** kitaplığı için Python 2.X ve **urllib.request** kitaplığı için Python 3.X. Bir FeatureVector ScoreData temsil eden bir n-boyutlu vektör sayısal özellikleri içeren ScoreData geçer. Machine Learning hizmeti için bir API anahtarı ile kimlik doğrulaması.

**Kod örneği çalıştırmak için**

1. Dağıtma "Örnek 1: Veri kümesi UCI ' indirin: Yetişkin 2 sınıfı veri kümesi"deneme, Machine Learning örnek koleksiyonun parçası.
2. ApiKey bir Web hizmetinden anahtarla atayın. Bkz: **bir Azure Machine Learning Studio yetkilendirme anahtarını alma** başlangıcı yakınında bu makalenin.
3. İstek URI'si ile serviceUri atayın.

**İşte tam bir istek aşağıdaki gibi görünür.**
```python
import urllib2 # urllib.request for Python 3.X
import json

data = {
    "Inputs": {
        "input1":
        [
            {
                'column1': "value1",   
                'column2': "value2",   
                'column3': "value3"
            }
        ],
    },
    "GlobalParameters":  {}
}

body = str.encode(json.dumps(data))

# Replace this with the URI and API Key for your web service
url = '<your-api-uri>'
api_key = '<your-api-key>'
headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}

# "urllib.request.Request(uri, body, headers)" for Python 3.X
req = urllib2.Request(url, body, headers)

try:
    # "urllib.request.urlopen(req)" for Python 3.X
    response = urllib2.urlopen(req)

    result = response.read()
    print(result)
# "urllib.error.HTTPError as error" for Python 3.X
except urllib2.HTTPError, error: 
    print("The request failed with status code: " + str(error.code))

    # Print the headers - they include the request ID and the timestamp, which are useful for debugging the failure
    print(error.info())
    print(json.loads(error.read())) 
```

### <a name="r-sample"></a>Örnek R

Machine Learning Web hizmetine bağlanmak için **RCurl** ve **rjson** döndürülen JSON yanıtı istekte bulunmak ve kitaplıkları. Bir FeatureVector ScoreData temsil eden bir n-boyutlu vektör sayısal özellikleri içeren ScoreData geçer. Machine Learning hizmeti için bir API anahtarı ile kimlik doğrulaması.

**İşte tam bir istek aşağıdaki gibi görünür.**
```r
library("RCurl")
library("rjson")

# Accept SSL certificates issued by public Certificate Authorities
options(RCurlOptions = list(cainfo = system.file("CurlSSL", "cacert.pem", package = "RCurl")))

h = basicTextGatherer()
hdr = basicHeaderGatherer()

req = list(
    Inputs = list(
            "input1" = list(
                list(
                        'column1' = "value1",
                        'column2' = "value2",
                        'column3' = "value3"
                    )
            )
        ),
        GlobalParameters = setNames(fromJSON('{}'), character(0))
)

body = enc2utf8(toJSON(req))
api_key = "<your-api-key>" # Replace this with the API key for the web service
authz_hdr = paste('Bearer', api_key, sep=' ')

h$reset()
curlPerform(url = "<your-api-uri>",
httpheader=c('Content-Type' = "application/json", 'Authorization' = authz_hdr),
postfields=body,
writefunction = h$update,
headerfunction = hdr$update,
verbose = TRUE
)

headers = hdr$value()
httpStatus = headers["status"]
if (httpStatus >= 400)
{
print(paste("The request failed with status code:", httpStatus, sep=" "))

# Print the headers - they include the request ID and the timestamp, which are useful for debugging the failure
print(headers)
}

print("Result:")
result = h$value()
print(fromJSON(result))
```

### <a name="javascript-sample"></a>JavaScript örneği

Machine Learning Web hizmetine bağlanmak için **isteği** npm paketini projenize. Ayrıca kullanacağınız `JSON` Biçimlendir ve istemcinin sonucu ayrıştırması için nesne. Kullanarak yükleme `npm install request --save`, veya ekleme `"request": "*"` Package.json'ınızdaki altında için `dependencies` çalıştırıp `npm install`.

**İşte tam bir istek aşağıdaki gibi görünür.**
```js
let req = require("request");

const uri = "<your-api-uri>";
const apiKey = "<your-api-key>";

let data = {
    "Inputs": {
        "input1":
        [
            {
                'column1': "value1",
                'column2': "value2",
                'column3': "value3"
            }
        ],
    },
    "GlobalParameters": {}
}

const options = {
    uri: uri,
    method: "POST",
    headers: {
        "Content-Type": "application/json",
        "Authorization": "Bearer " + apiKey,
    },
    body: JSON.stringify(data)
}

req(options, (err, res, body) => {
    if (!err && res.statusCode == 200) {
        console.log(body);
    } else {
        console.log("The request failed with status code: " + res.statusCode);
    }
});
```
