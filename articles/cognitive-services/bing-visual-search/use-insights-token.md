---
title: Bing Visual arama API'si ile Öngörüler belirteci kullanarak | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Görüntünün Insight kullanmayı bir görüntü ile ilgili bilgileri elde etmek için Visual arama API ile belirteci gösterir.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 6470bb5b7b5b8404683d1ae6fd4f16f0436982ef
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355084"
---
# <a name="using-an-insights-token-to-get-insights-about-an-image"></a>Bir görüntü ile ilgili bilgileri elde etmek için bir Öngörüler belirteci kullanma

Bing Visual arama API sağlayan bir görüntü ile ilgili bilgileri döndürür. Belirteç, ya da görüntüyü karşıya yükleyerek bir Öngörüler resmin URL'sini kullanarak görüntü sağlayabilir. Bu seçenekler hakkında daha fazla bilgi için bkz: [Bing Visual arama API nedir?](overview.md). Bu makalede, bir Öngörüler belirteci kullanarak gösterilmektedir. Bilgileri, elde etmek için görüntüyü karşıya gösteren örnekleri görmek için hızlı başlangıç ipuçları ([C#](quickstarts\csharp.md) | [Java](quickstarts\java.md) | [Node.js](quickstarts\nodejs.md)  |  [Python](quickstarts\python.md)).


Bir görüntü belirteç veya URL Visual arama gönderirseniz, aşağıdaki form verilerini POST gövdesinde içermelidir gösterir. Form verileri içerik düzeni üstbilgisini içermelidir ve kendi `name` "knowledgeRequest" parametresini ayarlayın. Hakkındaki ayrıntılar için `imageInfo` nesne için bkz: [isteği](#the-request).

```json
{
    "imageInfo" : {
        "url" : "",
        "imageInsightsToken" : "",
        "cropArea" : {
            "top" : 0.1,
            "left" : 0.5,
            "right" : 0.9,
            "bottom" : 0.9
        }
    },
    "knowledgeRequest" : {
      "filters" : {
        "site" : ""
      }
    }
}
```

Bu makaledeki örneklerde Öngörüler belirteci kullanmayı gösterir. Bir /images/search API yanıt görüntü nesneden Öngörüler belirteci alın. Öngörüler belirteci alma hakkında daha fazla bilgi için bkz: [Bing görüntü arama API](../Bing-Image-Search/overview.md).

```
--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
    "imageInfo" : {
        "imageInsightsToken" : "ccid_tmaGQ2eU*mid_D12339146CFEDF3D40...",
    }
}

--boundary_1234-abcd--
```


Öngörüler belirteci kullanın örnekler için bkz: [C#](#using-csharp) | [Java](#using-java) | [Node.js](#using-nodejs) | [Python](#using-python).

<a name="csharp" />
## <a name="using-c"></a>C# kullanın

### <a name="prerequisites"></a>Önkoşullar

İhtiyacınız olacak [Visual Studio 2017](https://www.visualstudio.com/downloads/) Windows üzerinde çalışan bu kodu alın. (Ücretsiz Community sürümü çalışır.)

Bu Hızlı Başlangıç için kullandığınız bir [ücretsiz deneme sürümü](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) abonelik anahtar veya Ücretli abonelik anahtarı.

## <a name="running-the-application"></a>Uygulamayı çalıştırma

Bu uygulamayı çalıştırmak için aşağıdaki adımları izleyin:

1. Visual Studio'da yeni bir konsol çözümü oluşturun.
1. Değiştir `Program.cs` Bu hızlı başlangıcı gösterilen koduna sahip.
2. Değiştir `accessKey` abonelik anahtarınızı değerle.
2. Değiştir `insightsToken` bir/görüntüleri/arama yanıt Öngörüler belirtecinden değerle.
3. Programını çalıştırın.

```csharp
using System;
using System.Text;
using System.Net;
using System.IO;
using System.Collections.Generic;
using System.Net.Http;
using System.Threading.Tasks;
using System.Threading;

namespace VisualSearchInsightsToken
{

    class Program
    {
        // Replace the accessKey string value with your valid subscription key.
        const string accessKey = "<yoursubscriptionkeygoeshere>";

        const string uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch";

        // Update with an insights token from an image object that the /images/search API endpoint returns.
        static string insightsToken = @"ccid_tmaGQ2eU*mid_D12339146CFEDF3D409CC7A66D2C98D0D71904D4*simid_608022145667564759*thid_OIP.tmaGQ2eUI1yq3yll!_jn9kwHaFZ";

        static string BoundaryTemplate = "batch_{0}";

        static void Main(string[] args)
        {
            try { 
                Console.WriteLine("Getting image insights using insights token: " + insightsToken);

                var knowledgeRequest = "{\"imageInfo\" : {\"imageInsightsToken\" : \"" + insightsToken + "\"}}";
                var boundary = string.Format(BoundaryTemplate, Guid.NewGuid());

                var json = BingVisualSearch(knowledgeRequest, boundary, uriBase, accessKey);

                Console.WriteLine("\nJSON Response:\n");
                Console.WriteLine(JsonPrettyPrint(json));

                Console.Write("\nPress Enter to exit ");
                Console.ReadLine();
            }
            catch (Exception e)
            {
                Console.WriteLine(e.Message);
            }

        }


        /// <summary>
        /// Calls the Bing visual search endpoint and returns the JSON response with insights.
        /// </summary>
        static string BingVisualSearch(string knowledgeRequest, string boundary, string uri, string subscriptionKey)
        {
            var requestMessage = new HttpRequestMessage(HttpMethod.Post, uri);
            requestMessage.Headers.Add("Ocp-Apim-Subscription-Key", accessKey);

            var content = new MultipartFormDataContent(boundary);
            content.Add(new StringContent(knowledgeRequest, Encoding.UTF8, "application/json"), "knowledgeRequest");
            requestMessage.Content = content;

            var httpClient = new HttpClient();

            Task<HttpResponseMessage> httpRequest = httpClient.SendAsync(requestMessage, HttpCompletionOption.ResponseContentRead, CancellationToken.None);
            HttpResponseMessage httpResponse = httpRequest.Result;
            HttpStatusCode statusCode = httpResponse.StatusCode;
            HttpContent responseContent = httpResponse.Content;

            string json = null;

            if (responseContent != null)
            {
                Task<String> stringContentsTask = responseContent.ReadAsStringAsync();
                json = stringContentsTask.Result;
            }


            return json;
        }


        /// <summary>
        /// Formats the given JSON string by adding line breaks and indents.
        /// </summary>
        /// <param name="json">The raw JSON string to format.</param>
        /// <returns>The formatted JSON string.</returns>
        static string JsonPrettyPrint(string json)
        {
            if (string.IsNullOrEmpty(json))
                return string.Empty;

            json = json.Replace(Environment.NewLine, "").Replace("\t", "");

            StringBuilder sb = new StringBuilder();
            bool quote = false;
            bool ignore = false;
            char last = ' ';
            int offset = 0;
            int indentLength = 2;

            foreach (char ch in json)
            {
                switch (ch)
                {
                    case '"':
                        if (!ignore) quote = !quote;
                        break;
                    case '\\':
                        if (quote && last != '\\') ignore = true;
                        break;
                }

                if (quote)
                {
                    sb.Append(ch);
                    if (last == '\\' && ignore) ignore = false;
                }
                else
                {
                    switch (ch)
                    {
                        case '{':
                        case '[':
                            sb.Append(ch);
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', ++offset * indentLength));
                            break;
                        case '}':
                        case ']':
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', --offset * indentLength));
                            sb.Append(ch);
                            break;
                        case ',':
                            sb.Append(ch);
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', offset * indentLength));
                            break;
                        case ':':
                            sb.Append(ch);
                            sb.Append(' ');
                            break;
                        default:
                            if (quote || ch != ' ') sb.Append(ch);
                            break;
                    }
                }
                last = ch;
            }

            return sb.ToString().Trim();
        }
    }
}
```




## <a name="using-java"></a>Java kullanarak

### <a name="prerequisites"></a>Önkoşullar

İhtiyacınız olacak [JDK 7 veya 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) derlemek ve bu kodu çalıştırmak için. Sık kullanılan varsa, ancak bir metin düzenleyicisi yeterli bir Java IDE kullanabilir.

Bu Hızlı Başlangıç için kullandığınız bir [ücretsiz deneme sürümü](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) abonelik anahtar veya Ücretli abonelik anahtarı.

## <a name="running-the-application"></a>Uygulamayı çalıştırma

Bu uygulamayı çalıştırmak için aşağıdaki adımları izleyin:

1. İndirme veya yükleme [gson Kitaplığı](https://github.com/google/gson). Ayrıca Maven alabilirsiniz.
2. Sık kullanılan IDE veya Düzenleyicisi yeni bir Java projesi oluşturun.
3. Sağlanan kod adındaki bir dosyada eklemek `VisualSearch.java`.
4. Değiştir `subscriptionKey` abonelik anahtarınızı değerle.
5. Programını çalıştırın.

```java
package insightstoken;

import java.util.*;
import java.io.*;



/*
 * Gson: https://github.com/google/gson
 * Maven info:
 *     groupId: com.google.code.gson
 *     artifactId: gson
 *     version: 2.8.2
 *
 * Once you have compiled or downloaded gson-2.8.2.jar, assuming you have placed it in the
 * same folder as this file (BingImageSearch.java), you can compile and run this program at
 * the command line as follows.
 *
 * javac BingImageSearch.java -classpath .;gson-2.8.2.jar -encoding UTF-8
 * java -cp .;gson-2.8.2.jar BingImageSearch
 */

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

// http://hc.apache.org/downloads.cgi (HttpComponents Downloads) HttpClient 4.5.5

import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.ContentType;
import org.apache.http.entity.mime.MultipartEntityBuilder;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClientBuilder;


public class InsightsToken {

    
    static String endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch";
    static String subscriptionKey = "<yoursubscriptionkeygoeshere>";

    // To get an insights, call the /images/search endpoint. Get the token from
    // the imageInsightsToken field in the Image object.
    static String insightsToken = "ccid_tmaGQ2eU*mid_D12339146CFEDF3D409CC7A66D2C98D0D71904D4*simid_608022145667564759*thid_OIP.tmaGQ2eUI1yq3yll!_jn9kwHaFZ";

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        CloseableHttpClient httpClient = HttpClientBuilder.create().build();
        
        String knowledgeRequest = "{\"imageInfo\" : {\"imageInsightsToken\" : \"" + insightsToken + "\"}}";

        try {
            HttpEntity entity = MultipartEntityBuilder
                .create()
                .addTextBody("knowledgeRequest", knowledgeRequest, ContentType.create("application/json"))
                .build();

            HttpPost httpPost = new HttpPost(endpoint);
            httpPost.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
            httpPost.setEntity(entity);
            HttpResponse response = httpClient.execute(httpPost);

            InputStream stream = response.getEntity().getContent();
            String json = new Scanner(stream).useDelimiter("\\A").next();

            System.out.println("\nJSON Response:\n");
            System.out.println(prettify(json));
        }
        catch (IOException e)
        {
            e.printStackTrace(System.out);
            System.exit(1);
        }
        catch (Exception e) {
            e.printStackTrace(System.out);
            System.exit(1);
        }
    }
    
    // pretty-printer for JSON; uses GSON parser to parse and re-serialize
    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonObject json = parser.parse(json_text).getAsJsonObject();
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }

    
}
```


<a name="using-nodejs" />
## <a name="using-nodejs"></a>Node.js'yi kullanma

### <a name="prerequisites"></a>Önkoşullar

Gereksinim duyduğunuz [Node.js 6](https://nodejs.org/en/download/) bu kodu çalıştırmak için.

Bu Hızlı Başlangıç için kullandığınız bir [ücretsiz deneme sürümü](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) abonelik anahtar veya Ücretli abonelik anahtarı.

## <a name="running-the-application"></a>Uygulamayı çalıştırma

Bu uygulamayı çalıştırmak için aşağıdaki adımları izleyin:

1. Projeniz için bir klasör oluşturun (veya sık kullanılan IDE veya düzenleyicisi kullanın).
2. Bir komut istemi veya terminal, az önce oluşturduğunuz klasöre gidin.
3. İstek modüllerini yükleyin:  
  ```  
  npm install request  
  ```  
3. Form verileri modüllerini yükleyin:  
  ```  
  npm install form-data  
  ```  
4. GetVisualInsights.js adlı bir dosya oluşturun ve aşağıdaki kodu ekleyin.
5. Değiştir `subscriptionKey` abonelik anahtarınızı değerle.
7. Programını çalıştırın.  
  ```
  node GetVisualInsights.js
  ```

```javascript
var request = require('request');
var FormData = require('form-data');

var baseUri = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch';
var subscriptionKey = '<yoursubscriptionkeygoeshere>';

// To get an insights, call the /images/search endpoint. Get the token from
// the imageInsightsToken field in the Image object.
var insightsToken = "ccid_tmaGQ2eU*mid_D12339146CFEDF3D409CC7A66D2C98D0D71904D4*simid_608022145667564759*thid_OIP.tmaGQ2eUI1yq3yll!_jn9kwHaFZ";

var knowledgeRequest = {
    "imageInfo" : {
        "imageInsightsToken" : insightsToken
    }
};

var form = new FormData();
form.append('knowledgeRequest', JSON.stringify(knowledgeRequest));

form.getLength(function(err, length){
  if (err) {
    return requestCallback(err);
  }

  var r = request.post(baseUri, requestCallback);
  r._form = form; 
  r.setHeader('Ocp-Apim-Subscription-Key', subscriptionKey);
});

function requestCallback(err, res, body) {
    console.log(JSON.stringify(JSON.parse(body), null, '  '))
}
```




## <a name="using-python"></a>Python kullanarak


### <a name="prerequisites"></a>Önkoşullar

Gereksinim duyduğunuz [Python 3](https://www.python.org/) bu kodu çalıştırmak için.

Bu Hızlı Başlangıç için kullandığınız bir [ücretsiz deneme sürümü](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) abonelik anahtar veya Ücretli abonelik anahtarı.

## <a name="running-the-walkthrough"></a>İzlenecek yol çalıştırma

Bu uygulamayı çalıştırmak için aşağıdaki adımları izleyin:

1. Sık kullanılan IDE veya Düzenleyicisi içinde yeni bir Python projesi oluşturun.
2. Visualsearch.PY adlı bir dosya oluşturun ve bu hızlı başlangıcı gösterilen kodu ekleyin.
3. Değiştir `SUBSCRIPTION_KEY` abonelik anahtarınızı değerle.
4. Programını çalıştırın.


```python
"""Bing Visual Search example"""

# Download and install Python at https://www.python.org/
# Run the following in a command console window
# pip3 install requests

import requests, json

BASE_URI = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch'

SUBSCRIPTION_KEY = '<yoursubscriptionkeygoeshere>'

HEADERS = {'Ocp-Apim-Subscription-Key': SUBSCRIPTION_KEY}

# To get an insights, call the /images/search endpoint. Get the token from
# the imageInsightsToken field in the Image object.
insightsToken = 'ccid_tmaGQ2eU*mid_D12339146CFEDF3D409CC7A66D2C98D0D71904D4*simid_608022145667564759*thid_OIP.tmaGQ2eUI1yq3yll!_jn9kwHaFZ'

formData = '{"imageInfo":{"imageInsightsToken":"' + insightsToken + '"}}'


file = {'knowledgeRequest' : (None, formData)}

def main():
    
    try:
        response = requests.post(BASE_URI, headers=HEADERS, files=file)
        response.raise_for_status()
        print_json(response.json())

    except Exception as ex:
        raise ex


def print_json(obj):
    """Print the object as json"""
    print(json.dumps(obj, sort_keys=True, indent=2, separators=(',', ': ')))



# Main execution
if __name__ == '__main__':
    main()
```

## <a name="next-steps"></a>Sonraki adımlar

[Bing Visual arama tek sayfa uygulaması Öğreticisi](tutorial-bing-visual-search-single-page-app.md)  
[Bing Visual arama genel bakış](overview.md)  
[Deneyin](https://aka.ms/bingvisualsearchtryforfree)  
[Ücretsiz deneme erişim anahtarı alma](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Bing Visual arama API Başvurusu](https://aka.ms/bingvisualsearchreferencedoc)
