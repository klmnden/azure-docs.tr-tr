---
title: Bir Azure Machine Learning Web hizmetini kullanma | Microsoft Docs
description: Machine learning hizmeti dağıtıldıktan sonra gerçek zamanlı istek-yanıt hizmeti veya toplu yürütme hizmeti olarak kullanılabilir hale getirileceğini RESTFul Web hizmeti tüketilebilir.
services: machine-learning
documentationcenter: ''
author: YasinMSFT
ms.author: yahajiza
manager: hjerez
editor: cgronlun
ms.assetid: 804f8211-9437-4982-98e9-ca841b7edf56
ms.service: machine-learning
ms.component: studio
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 06/02/2017
ms.openlocfilehash: b89fb0fbb499fa06c9e56f02937b1c586efde9b6
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34833405"
---
# <a name="how-to-consume-an-azure-machine-learning-web-service"></a>Bir Azure Machine Learning Web hizmetini kullanma

Bir Azure Machine Learning Tahmine dayalı modeli bir Web hizmeti olarak dağıttığınızda, veri göndermek ve Öngörüler elde etmek için bir REST API'si kullanabilirsiniz. Gerçek zamanlı veya toplu iş modunda veriler gönderebilir.

Oluştur ve buraya Machine Learning Studio kullanarak bir Machine Learning Web hizmetini dağıtma hakkında daha fazla bilgi bulabilirsiniz:

* Machine Learning Studio'da bir deneme oluşturma konusunda bir öğretici için bkz: [ilk denemenizi oluşturma](create-experiment.md).
* Web hizmetinin nasıl dağıtılacağı hakkında daha fazla bilgi için bkz: [Machine Learning Web hizmetini dağıtma](publish-a-machine-learning-web-service.md).
* Machine Learning hakkında daha fazla bilgi için genel olarak, ziyaret [Machine Learning Belge Merkezi](https://azure.microsoft.com/documentation/services/machine-learning/).

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="overview"></a>Genel Bakış
Azure Machine Learning Web hizmeti ile bir dış uygulama Machine Learning bir iş akışı Puanlama modeli ile gerçek zamanlı iletişim kurar. Bir Machine Learning Web hizmeti çağrısı bir dış uygulamaya tahmin sonuçlarını döndürür. Bir Machine Learning Web hizmeti çağrısı yapmak için bir tahmini dağıttığınızda oluşturduğunuz bir API anahtarı geçirirsiniz. Machine Learning Web hizmeti web programlama projeleri için popüler bir mimari seçimi olan REST'i temel alır.

Azure Machine Learning iki tür hizmet içerir:

* İstek-yanıt hizmeti (RRS) – düşük gecikme, Machine Learning Studio'dan oluşturulan ve dağıtılan durum bilgisi olmayan modellere bir arabirim sağlayan yüksek düzeyde ölçeklenebilir hizmet.
* Toplu yürütme hizmeti (BES) – zaman uyumsuz bir hizmet veri kayıtları için toplu iş yapan.

Machine Learning Web Hizmetleri hakkında daha fazla bilgi için bkz: [Machine Learning Web hizmetini dağıtma](publish-a-machine-learning-web-service.md).

## <a name="get-an-azure-machine-learning-authorization-key"></a>Bir Azure Machine Learning yetkilendirme anahtarı alma
API anahtarları denemenizi dağıttığınızda, Web hizmeti için oluşturulur. Anahtarları çeşitli konumlardan alabilirsiniz.

### <a name="from-the-microsoft-azure-machine-learning-web-services-portal"></a>Microsoft Azure Machine Learning Web Hizmetleri Portalı'ndan
Oturum [Microsoft Azure Machine Learning Web Hizmetleri](https://services.azureml.net) portal.

Yeni Machine Learning Web hizmeti için API anahtarı almak için:

1. Azure Machine Learning Web Hizmetleri Portalı'nda tıklatın **Web Hizmetleri** üst menü.
2. Anahtar almak istediğiniz Web hizmeti tıklatın.
3. Üst menüsünde **Tüket**.
4. Kopyalayıp kaydedin **birincil anahtar**.

Klasik Machine Learning Web hizmeti için API anahtarı almak için:

1. Azure Machine Learning Web Hizmetleri Portalı'nda tıklatın **Klasik Web Hizmetleri** üst menü.
2. Çalıştığınız Web hizmeti tıklatın.
3. Anahtar almak istediğiniz uç noktasına tıklayın.
4. Üst menüsünde **Tüket**.
5. Kopyalayıp kaydedin **birincil anahtar**.

### <a name="classic-web-service"></a>Klasik Web hizmeti
 Bu gibi durumlarda, Klasik Web hizmeti için bir anahtar da Machine Learning Studio'dan alabilirsiniz.

#### <a name="machine-learning-studio"></a>Machine Learning Studio
1. Machine Learning Studio'da tıklatın **WEB Hizmetleri** soldaki.
2. Bir Web hizmeti tıklatın. **API anahtarı** açık **PANO** sekmesi.

## <a id="connect"></a>Bir Machine Learning Web hizmetine bağlanma
HTTP istek ve yanıt destekleyen herhangi bir programlama dili kullanılarak bir Machine Learning Web hizmetine bağlanabilir. C#, Python ve R örnekler bir Machine Learning Web hizmeti Yardım sayfasından görüntüleyebilirsiniz.

**Machine Learning API Yardım** Machine Learning API'si Yardım Web hizmetini dağıttığınızda oluşturulur. Bkz: [Azure Machine Learning izlenecek dağıtımı Web hizmeti](walkthrough-5-publish-web-service.md).
Machine Learning API'si Yardım tahmin Web hizmeti hakkındaki ayrıntıları içerir.

1. Çalıştığınız Web hizmeti tıklatın.
2. API Yardım sayfasında görüntülemek istediğiniz uç noktasına tıklayın.
3. Üst menüsünde **Tüket**.
4. Tıklatın **API Yardım sayfası** istek-yanıt ya da toplu iş yürütme bitiş noktaları altında.

**Machine Learning API görünümüne yeni Web hizmeti için Yardım**

İçinde [Azure Machine Learning Web Hizmetleri portalı](https://services.azureml.net/):

1. Tıklatın **WEB Hizmetleri** üst menüsünde.
2. Anahtar almak istediğiniz Web hizmeti tıklatın.

Tıklatın **kullanım Web hizmeti** URI'ler isteği Reposonse ve toplu yürütme Hizmetleri ve örnek kod için C#, R ve Python almak için.

Tıklatın **Swagger API'si** Swagger almak için sağlanan URI'ler API çağrısı için belgelerine bağlı.

### <a name="c-sample"></a>C# örnek
Bir Machine Learning Web hizmetine bağlanmak için bir **HttpClient** ScoreData geçirme. ScoreData bir FeatureVector ScoreData temsil eden bir n boyutlu vektörü sayısal özellik içerir. Machine Learning hizmetine bir API anahtarı ile kimlik doğrulaması.

Machine Learning Web hizmetine bağlanmak için **Microsoft.AspNet.WebApi.Client** NuGet paketi yüklü olmalıdır.

**Microsoft.AspNet.WebApi.Client NuGet Visual Studio yükleme**

1. UCI indirme kümesinden yayımlama: yetişkin 2 sınıfı dataset Web hizmeti.
2. **Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**’na tıklayın.
3. Seçin **Install-Package Microsoft.AspNet.WebApi.Client**.

**Kod örneği çalıştırmak için**

1. Yayımla "Örnek 1: dataset UCI indirin: yetişkin 2 sınıfı dataset" deneme, Machine Learning örnek koleksiyonunun bir parçası.
2. Apikey ile yapılan bir Web hizmetinden anahtarla atayın. Bkz: **bir Azure Machine Learning yetkilendirme anahtarı alma** üstünde.
3. İstek URI'si ile serviceUri atayın.

**Tam bir istek nasıl görüneceğini aşağıda verilmiştir.**
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
                // if you are calling this code from the UI thread of an ASP.Net application.
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

                    // Print the headers - they include the requert ID and the timestamp,
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
Bir Machine Learning Web hizmetine bağlanmak için kullanmak **urllib2** Python kitaplığı 2.X ve **urllib.request** kitaplığı için Python 3.X. Bir FeatureVector ScoreData temsil eden bir n boyutlu vektörü sayısal özellikleri içeren ScoreData geçer. Machine Learning hizmetine bir API anahtarı ile kimlik doğrulaması.

**Kod örneği çalıştırmak için**

1. Dağıtma "Örnek 1: dataset UCI indirin: yetişkin 2 sınıfı dataset" deneme, Machine Learning örnek koleksiyonunun bir parçası.
2. Apikey ile yapılan bir Web hizmetinden anahtarla atayın. Bkz: **bir Azure Machine Learning yetkilendirme anahtarı alma** başına bir kısmına bu makalenin.
3. İstek URI'si ile serviceUri atayın.

**Tam bir istek nasıl görüneceğini aşağıda verilmiştir.**
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

    # Print the headers - they include the requert ID and the timestamp, which are useful for debugging the failure
    print(error.info())
    print(json.loads(error.read())) 
```

### <a name="r-sample"></a>R örnek

Bir Machine Learning Web hizmetine bağlanmak için **RCurl** ve **rjson** istekte ve döndürülen JSON yanıtı işlemek için kitaplıkları. Bir FeatureVector ScoreData temsil eden bir n boyutlu vektörü sayısal özellikleri içeren ScoreData geçer. Machine Learning hizmetine bir API anahtarı ile kimlik doğrulaması.

**Tam bir istek nasıl görüneceğini aşağıda verilmiştir.**
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

# Print the headers - they include the requert ID and the timestamp, which are useful for debugging the failure
print(headers)
}

print("Result:")
result = h$value()
print(fromJSON(result))
```

### <a name="javascript-sample"></a>JavaScript örneği

Bir Machine Learning Web hizmetine bağlanmak için **isteği** projenizdeki npm paket. Ayrıca kullanacağınız `JSON` Biçimlendir ve sonucu ayrıştırmak için nesne. Kullanarak yükleyin `npm install request --save`, veya ekleme `"request": "*"` Package.json'ınızdaki altında için `dependencies` çalıştırıp `npm install`.

**Tam bir istek nasıl görüneceğini aşağıda verilmiştir.**
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