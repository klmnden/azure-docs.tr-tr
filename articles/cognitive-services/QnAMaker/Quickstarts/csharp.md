---
title: REST API (V4) - C# - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Alma C# hızlı bir şekilde yardımcı olması için REST tabanlı bilgi ve kod örnekleri, azure'da Microsoft Bilişsel Hizmetler Microsoft Translator metin tanıma API'sini kullanmaya başlama.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 02/13/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: cf801b8b6b458104a469932487860285102a641b
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65777633"
---
# <a name="how-to-use-the-qna-maker-rest-api-with-c"></a>Soru-cevap Oluşturucu REST API ile kullanmaC# 
<a name="HOLTop"></a>

Bu makalede nasıl kullanılacağını gösterir [Microsoft soru-cevap Oluşturucu API'si](../Overview/overview.md) ile C# aşağıdakileri yapmak için.

- [Yeni bilgi bankası oluşturma.](#Create)
- [Var olan bilgi bankasını güncelleştirme.](#Update)
- [Bilgi bankası oluşturma veya güncelleştirme isteğinin durumunu alma.](#Status)
- [Var olan bilgi bankasını yayımlama.](#Publish)
- [Var olan bilgi bankasının içeriğini değiştirme.](#Replace)
- [Bilgi bankasının içeriğini indirme.](#GetQnA)
- [Bilgi bankasını kullanarak bir soruya yanıt bulma.](#GetAnswers)
- [Bir bilgi bankası hakkında bilgi alma.](#GetKB)
- [Belirli bir kullanıcıya ait olan tüm bilgi bankaları hakkında bilgi alma.](#GetKBsByUser)
- [Bilgi bankasını silme.](#Delete)
- [Geçerli uç nokta anahtarlarını alma.](#GetKeys)
- [Geçerli uç nokta anahtarlarını yeniden oluşturma.](#PutKeys)
- [Büyük küçük harf duyarsız word değişiklikleri geçerli kümesini alın.](#GetAlterations)
- [Büyük küçük harf duyarsız word değişiklikleri geçerli kümesini değiştirin.](#PutAlterations)

[!INCLUDE [Code is available in Azure-Samples GitHub repo](../../../../includes/cognitive-services-qnamaker-csharp-repo-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu kodu Windows’da çalıştırmak için [Visual Studio 2017](https://www.visualstudio.com/downloads/) gerekir. (Ücretsiz Community Edition’ı kullanabilirsiniz.)

**Microsoft Soru-Cevap Oluşturma API'sine** sahip bir [Bilişsel Hizmetler API hesabınızın](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) olması gerekir. [Azure panonuzdan](https://portal.azure.com/#create/Microsoft.CognitiveServices) ücretli bir abonelik anahtarına ihtiyacınız olacak.

<a name="Create"></a>

## <a name="create-knowledge-base"></a>Bilgi bankası oluşturma

Aşağıdaki kod, [Oluşturma](https://go.microsoft.com/fwlink/?linkid=2092179) yöntemini kullanarak yeni bir bilgi bankası oluşturur.

1. Sık kullandığınız IDE'de yeni bir C# projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

// NOTE: Install the Newtonsoft.Json NuGet package.
using Newtonsoft.Json;

namespace QnAMaker
{
    class Program
    {
        static string host = "https://westus.api.cognitive.microsoft.com";
        static string service = "/qnamaker/v4.0";
        static string method = "/knowledgebases/create";

        // NOTE: Replace this with a valid subscription key.
        static string key = "ENTER KEY HERE";

        static string kb = @"
{
  'name': 'QnA Maker FAQ',
  'qnaList': [
    {
      'id': 0,
      'answer': 'You can use our REST APIs to manage your Knowledge Base. See here for details: https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update',
      'source': 'Custom Editorial',
      'questions': [
        'How do I programmatically update my Knowledge Base?'
      ],
      'metadata': [
        {
          'name': 'category',
          'value': 'api'
        }
      ]
    }
  ],
  'urls': [
    'https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs',
    'https://docs.microsoft.com/bot-framework/resources-bot-framework-faq'
  ],
  'files': []
}
";

        public struct Response
        {
            public HttpResponseHeaders headers;
            public string response;

            public Response(HttpResponseHeaders headers, string response)
            {
                this.headers = headers;
                this.response = response;
            }
        }

        static string PrettyPrint(string s)
        {
            return JsonConvert.SerializeObject(JsonConvert.DeserializeObject(s), Formatting.Indented);
        }

        async static Task<Response> Post(string uri, string body)
        {
            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = HttpMethod.Post;
                request.RequestUri = new Uri(uri);
                request.Content = new StringContent(body, Encoding.UTF8, "application/json");
                request.Headers.Add("Ocp-Apim-Subscription-Key", key);

                var response = await client.SendAsync(request);
                var responseBody = await response.Content.ReadAsStringAsync();
                return new Response(response.Headers, responseBody);
            }
        }

        async static Task<Response> Get(string uri)
        {
            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = HttpMethod.Get;
                request.RequestUri = new Uri(uri);
                request.Headers.Add("Ocp-Apim-Subscription-Key", key);

                var response = await client.SendAsync(request);
                var responseBody = await response.Content.ReadAsStringAsync();
                return new Response(response.Headers, responseBody);
            }
        }

        async static Task<Response> PostCreateKB(string kb)
        {
            string uri = host + service + method;
            Console.WriteLine("Calling " + uri + ".");
            return await Post(uri, kb);
        }

        async static Task<Response> GetStatus(string operation)
        {
            string uri = host + service + operation;
            Console.WriteLine("Calling " + uri + ".");
            return await Get(uri);
        }

        async static void CreateKB()
        {
            
            var response = await PostCreateKB(kb);
            var operation = response.headers.GetValues("Location").First();
            Console.WriteLine(PrettyPrint(response.response));

            var done = false;
            while (true != done)
            {
                response = await GetStatus(operation);
                Console.WriteLine(PrettyPrint(response.response));

                var fields = JsonConvert.DeserializeObject<Dictionary<string, string>>(response.response);

                String state = fields["operationState"];
                if (state.CompareTo("Running") == 0 || state.CompareTo("NotStarted") == 0)
                {
                    var wait = response.headers.GetValues("Retry-After").First();
                    Console.WriteLine("Waiting " + wait + " seconds...");
                    Thread.Sleep(Int32.Parse(wait) * 1000);
                }
                else
                {
                    Console.WriteLine("Press any key to continue.");
                    done = true;
                }
            }
        }

        static void Main(string[] args)
        {
            CreateKB();
            Console.ReadLine();
        }
    }
}

```

**Bilgi bankası yanıtı oluşturma**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-04-13T01:52:30Z",
  "lastActionTimestamp": "2018-04-13T01:52:30Z",
  "userId": "2280ef5917bb4ebfa1aae41fb1cebb4a",
  "operationId": "e88b5b23-e9ab-47fe-87dd-3affc2fb10f3"
}
...
{
  "operationState": "Running",
  "createdTimestamp": "2018-04-13T01:52:30Z",
  "lastActionTimestamp": "2018-04-13T01:52:30Z",
  "userId": "2280ef5917bb4ebfa1aae41fb1cebb4a",
  "operationId": "e88b5b23-e9ab-47fe-87dd-3affc2fb10f3"
}
...
{
  "operationState": "Succeeded",
  "createdTimestamp": "2018-04-13T01:52:30Z",
  "lastActionTimestamp": "2018-04-13T01:52:46Z",
  "resourceLocation": "/knowledgebases/b0288f33-27b9-4258-a304-8b9f63427dad",
  "userId": "2280ef5917bb4ebfa1aae41fb1cebb4a",
  "operationId": "e88b5b23-e9ab-47fe-87dd-3affc2fb10f3"
}
```

[Başa dön](#HOLTop)

<a name="Update"></a>

## <a name="update-knowledge-base"></a>Bilgi bankası güncelleştirme

Aşağıdaki kod, [Güncelleştirme](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update) yöntemini kullanarak mevcut bir bilgi bankasını güncelleştirir.

1. Sık kullandığınız IDE'de yeni bir C# projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

// NOTE: Install the Newtonsoft.Json NuGet package.
using Newtonsoft.Json;

namespace QnAMaker
{
    class Program
    {
        static string host = "https://westus.api.cognitive.microsoft.com";
        static string service = "/qnamaker/v4.0";
        static string method = "/knowledgebases/";

        // NOTE: Replace this with a valid subscription key.
        static string key = "ENTER KEY HERE";

        // NOTE: Replace this with a valid knowledge base ID.
        static string kb = "ENTER ID HERE";

        static string new_kb = @"
{
  'add': {
    'qnaList': [
      {
        'id': 1,
        'answer': 'You can change the default message if you use the QnAMakerDialog. See this for details: https://docs.botframework.com/azure-bot-service/templates/qnamaker/#navtitle',
        'source': 'Custom Editorial',
        'questions': [
          'How can I change the default message from QnA Maker?'
        ],
        'metadata': []
      }
    ],
    'urls': [
      'https://docs.microsoft.com/azure/cognitive-services/Emotion/FAQ'
    ]
  },
  'update' : {
    'name' : 'New KB Name'
  },
  'delete': {
    'ids': [
      0
    ]
  }
}
";

        public struct Response
        {
            public HttpResponseHeaders headers;
            public string response;

            public Response(HttpResponseHeaders headers, string response)
            {
                this.headers = headers;
                this.response = response;
            }
        }

        static string PrettyPrint(string s)
        {
            return JsonConvert.SerializeObject(JsonConvert.DeserializeObject(s), Formatting.Indented);
        }

        async static Task<Response> Patch(string uri, string body)
        {
            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = new HttpMethod("PATCH");
                request.RequestUri = new Uri(uri);
                request.Content = new StringContent(body, Encoding.UTF8, "application/json");
                request.Headers.Add("Ocp-Apim-Subscription-Key", key);

                var response = await client.SendAsync(request);
                var responseBody = await response.Content.ReadAsStringAsync();
                return new Response(response.Headers, responseBody);
            }
        }

        async static Task<Response> Get(string uri)
        {
            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = HttpMethod.Get;
                request.RequestUri = new Uri(uri);
                request.Headers.Add("Ocp-Apim-Subscription-Key", key);

                var response = await client.SendAsync(request);
                var responseBody = await response.Content.ReadAsStringAsync();
                return new Response(response.Headers, responseBody);
            }
        }

        async static Task<Response> PostUpdateKB(string kb, string new_kb)
        {
            string uri = host + service + method + kb;
            Console.WriteLine("Calling " + uri + ".");
            return await Patch(uri, new_kb);
        }

        async static Task<Response> GetStatus(string operation)
        {
            string uri = host + service + operation;
            Console.WriteLine("Calling " + uri + ".");
            return await Get(uri);
        }

        async static void UpdateKB(string kb, string new_kb)
        {
            var response = await PostUpdateKB(kb, new_kb);
            var operation = response.headers.GetValues("Location").First();
            Console.WriteLine(PrettyPrint(response.response));

            var done = false;
            while (true != done)
            {
                response = await GetStatus(operation);
                Console.WriteLine(PrettyPrint(response.response));

                var fields = JsonConvert.DeserializeObject<Dictionary<string, string>>(response.response);

                String state = fields["operationState"];
                if (state.CompareTo("Running") == 0 || state.CompareTo("NotStarted") == 0)
                {
                    var wait = response.headers.GetValues("Retry-After").First();
                    Console.WriteLine("Waiting " + wait + " seconds...");
                    Thread.Sleep(Int32.Parse(wait) * 1000);
                }
                else
                {
                    Console.WriteLine("Press any key to continue.");
                    done = true;
                }
            }
        }

        static void Main(string[] args)
        {
            UpdateKB(kb, new_kb);
            Console.ReadLine();
        }
    }
}

```

**Bilgi bankası güncelleştirme yanıtı**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-04-13T01:49:48Z",
  "lastActionTimestamp": "2018-04-13T01:49:48Z",
  "userId": "2280ef5917bb4ebfa1aae41fb1cebb4a",
  "operationId": "5156f64e-e31d-4638-ad7c-a2bdd7f41658"
}
...
{
  "operationState": "Succeeded",
  "createdTimestamp": "2018-04-13T01:49:48Z",
  "lastActionTimestamp": "2018-04-13T01:49:50Z",
  "resourceLocation": "/knowledgebases/140a46f3-b248-4f1b-9349-614bfd6e5563",
  "userId": "2280ef5917bb4ebfa1aae41fb1cebb4a",
  "operationId": "5156f64e-e31d-4638-ad7c-a2bdd7f41658"
}
Press any key to continue.
```

[Başa dön](#HOLTop)

<a name="Status"></a>

## <a name="get-request-status"></a>İstek alma durumu

Bilgi bankası oluşturma veya güncelleştirme isteğinin durumunu denetlemek için [İşlem](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/operations/getdetails) yöntemini çağırabilirsiniz. Bu yöntemin nasıl kullanıldığını görmek için lütfen [Oluşturma](#Create) veya [Güncelleştirme](#Update) yönteminin örnek koduna bakın.

[Başa dön](#HOLTop)

<a name="Publish"></a>

## <a name="publish-knowledge-base"></a>Bilgi bankası yayımlama

Aşağıdaki kod, [Yayımla](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/publish) yöntemini kullanarak mevcut bir bilgi bankasını yayımlar.

1. Sık kullandığınız IDE'de yeni bir C# projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

// NOTE: Install the Newtonsoft.Json NuGet package.
using Newtonsoft.Json;

namespace QnAMaker
{
    class Program
    {
        static string host = "https://westus.api.cognitive.microsoft.com";
        static string service = "/qnamaker/v4.0";
        static string method = "/knowledgebases/";

        // NOTE: Replace this with a valid subscription key.
        static string key = "ENTER KEY HERE";

        // NOTE: Replace this with a valid knowledge base ID.
        static string kb = "ENTER ID HERE";

        static string PrettyPrint(string s)
        {
            return JsonConvert.SerializeObject(JsonConvert.DeserializeObject(s), Formatting.Indented);
        }

        async static Task<string> Post(string uri)
        {
            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = HttpMethod.Post;
                request.RequestUri = new Uri(uri);
                request.Headers.Add("Ocp-Apim-Subscription-Key", key);

                var response = await client.SendAsync(request);
                if (response.IsSuccessStatusCode)
                {
                    return "{'result' : 'Success.'}";
                }
                else
                {
                    return await response.Content.ReadAsStringAsync();
                }
            }
        }

        async static void PublishKB()
        {
            var uri = host + service + method + kb;
            Console.WriteLine("Calling " + uri + ".");
            var response = await Post(uri);
            Console.WriteLine(PrettyPrint(response));
            Console.WriteLine("Press any key to continue.");
        }

        static void Main(string[] args)
        {
            PublishKB();
            Console.ReadLine();
        }
    }
}

```

**Bilgi bankası yanıtını yayımlama**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "result": "Success."
}
```

[Başa dön](#HOLTop)

<a name="Replace"></a>

## <a name="replace-knowledge-base"></a>Bilgi bankasını değiştirme

Aşağıdaki kod [Replace](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/replace) metodunu kullanarak belirtilen bilgi bankasının içeriğini değiştirir.

1. Sık kullandığınız IDE'de yeni bir C# projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

// NOTE: Install the Newtonsoft.Json NuGet package.
using Newtonsoft.Json;

namespace QnAMaker
{
    class Program
    {
        static string host = "https://westus.api.cognitive.microsoft.com";
        static string service = "/qnamaker/v4.0";
        static string method = "/knowledgebases/";

        // NOTE: Replace this with a valid subscription key.
        static string key = "ENTER KEY HERE";

        // NOTE: Replace this with a valid knowledge base ID.
        static string kb = "ENTER ID HERE";

        static string new_kb = @"
{
  'qnaList': [
    {
      'id': 0,
      'answer': 'You can use our REST APIs to manage your Knowledge Base. See here for details: https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update',
      'source': 'Custom Editorial',
      'questions': [
        'How do I programmatically update my Knowledge Base?'
      ],
      'metadata': [
        {
          'name': 'category',
          'value': 'api'
        }
      ]
    }
  ]
}
";

        static string PrettyPrint(string s)
        {
            return JsonConvert.SerializeObject(JsonConvert.DeserializeObject(s), Formatting.Indented);
        }

        async static Task<string> Put(string uri, String body)
        {
            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = HttpMethod.Put;
                request.RequestUri = new Uri(uri);
                request.Content = new StringContent(body, Encoding.UTF8, "application/json");
                request.Headers.Add("Ocp-Apim-Subscription-Key", key);

                var response = await client.SendAsync(request);
                if (response.IsSuccessStatusCode)
                {
                    return "{'result' : 'Success.'}";
                }
                else
                {
                    return await response.Content.ReadAsStringAsync();
                }
            }
        }

        async static void ReplaceKB()
        {
            var uri = host + service + method + kb;
            Console.WriteLine("Calling " + uri + ".");
            var response = await Put(uri, new_kb);
            Console.WriteLine(PrettyPrint(response));
            Console.WriteLine("Press any key to continue.");
        }

        static void Main(string[] args)
        {
            ReplaceKB();
            Console.ReadLine();
        }
    }
}

```

**Bilgi bankası yanıtını değiştirme**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
    "result": "Success."
}
```

[Başa dön](#HOLTop)

<a name="GetQnA"></a>

## <a name="download-the-contents-of-a-knowledge-base"></a>Bilgi bankasının içeriğini indirme

Aşağıdaki kod [Download knowledge base](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/download) metodunu kullanarak belirtilen bilgi bankasının içeriğini indirir.

1. Sık kullandığınız IDE'de yeni bir C# projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

// NOTE: Install the Newtonsoft.Json NuGet package.
using Newtonsoft.Json;

namespace QnAMaker
{
    class Program
    {
        static string host = "https://westus.api.cognitive.microsoft.com";
        static string service = "/qnamaker/v4.0";
        static string method = "/knowledgebases/{0}/{1}/qna/";

        // NOTE: Replace this with a valid subscription key.
        static string key = "ENTER KEY HERE";

        // NOTE: Replace this with a valid knowledge base ID.
        static string kb = "ENTER ID HERE";

        // NOTE: Replace this with "test" or "prod".
        static string env = "test";

        static string PrettyPrint(string s)
        {
            return JsonConvert.SerializeObject(JsonConvert.DeserializeObject(s), Formatting.Indented);
        }

        async static Task<string> Get(string uri)
        {
            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = HttpMethod.Get;
                request.RequestUri = new Uri(uri);
                request.Headers.Add("Ocp-Apim-Subscription-Key", key);

                var response = await client.SendAsync(request);
                return await response.Content.ReadAsStringAsync();
            }
        }

        async static void GetQnA()
        {
            var method_with_id = String.Format(method, kb, env);
            var uri = host + service + method_with_id;
            Console.WriteLine("Calling " + uri + ".");
            var response = await Get(uri);
            Console.WriteLine(PrettyPrint(response));
            Console.WriteLine("Press any key to continue.");
        }

        static void Main(string[] args)
        {
            GetQnA();
            Console.ReadLine();
        }
    }
}

```

**Bilgi bankası yanıtını indirme**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "qnaDocuments": [
    {
      "id": 1,
      "answer": "You can use our REST APIs to manage your Knowledge Base. See here for details: https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update",
      "source": "Custom Editorial",
      "questions": [
        "How do I programmatically update my Knowledge Base?"
      ],
      "metadata": [
        {
          "name": "category",
          "value": "api"
        }
      ]
    },
    {
      "id": 2,
      "answer": "QnA Maker provides an FAQ data source that you can query from your bot or application. Although developers will find this useful, content owners will especially benefit from this tool. QnA Maker is a completely no-code way of managing the content that powers your bot or application.",
      "source": "https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs",
      "questions": [
        "Who is the target audience for the QnA Maker tool?"
      ],
      "metadata": []
    },
...
  ]
}
```

[Başa dön](#HOLTop)

<a name="GetAnswers"></a>

## <a name="get-answers-to-a-question-by-using-a-knowledge-base"></a>Bilgi bankasını kullanarak bir soruya yanıt bulma

Aşağıdaki kod **Generate answers** metodunu kullanarak belirli bir bilgi bankasından bir soruya yanıt bulur.

1. Sık kullandığınız IDE'de yeni bir C# projesi oluşturun.
1. Aşağıda sağlanan kodu ekleyin.
1. `host` değerinin yerine Soru-Cevap Oluşturma aboneliğinizin web sitesi adını yazın. Daha fazla bilgi için bkz. [Soru-Cevap Oluşturma hizmeti oluşturma](../How-To/set-up-qnamaker-service-azure.md).
1. `endpoint_key` değerini, aboneliğiniz için geçerli olan bir uç nokta anahtarı ile değiştirin. Bu anahtarın abonelik anahtarınızdan farklı olduğunu unutmayın. Uç nokta anahtarlarınızı [Get endpoint keys](#GetKeys) metoduyla alabilirsiniz.
1. `kb` değerini yanıt için sorgulamak istediğiniz bilgi bankasının kimliğiyle değiştirin. Bu bilgi bankasının [Publish](#Publish) metodu kullanılarak önceden yayımlanmış olması gerektiğini unutmayın.
1. Programı çalıştırın.

```csharp
using System;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;

namespace QnAMaker
{
    class Program
    {
        // NOTE: Replace this with a valid host name.
        static string host = "ENTER HOST HERE";

        // NOTE: Replace this with a valid endpoint key.
        // This is not your subscription key.
        // To get your endpoint keys, call the GET /endpointkeys method.
        static string endpoint_key = "ENTER KEY HERE";

        // NOTE: Replace this with a valid knowledge base ID.
        // Make sure you have published the knowledge base with the
        // POST /knowledgebases/{knowledge base ID} method.
        static string kb = "ENTER KB ID HERE";

        static string service = "/qnamaker";
        static string method = "/knowledgebases/" + kb + "/generateAnswer/";

        static string question = @"
{
    'question': 'Is the QnA Maker Service free?',
    'top': 3
}
";

        async static Task<string> Post(string uri, string body)
        {
            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = HttpMethod.Post;
                request.RequestUri = new Uri(uri);
                request.Content = new StringContent(body, Encoding.UTF8, "application/json");
                request.Headers.Add("Authorization", "EndpointKey " + endpoint_key);

                var response = await client.SendAsync(request);
                return await response.Content.ReadAsStringAsync();
            }
        }

        async static void GetAnswers()
        {
            var uri = host + service + method;
            Console.WriteLine("Calling " + uri + ".");
            var response = await Post(uri, question);
            Console.WriteLine(response);
            Console.WriteLine("Press any key to continue.");
        }

        static void Main(string[] args)
        {
            GetAnswers();
            Console.ReadLine();
        }
    }
}
```

**Yanıt alma**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "answers": [
    {
      "questions": [
        "Can I use BitLocker with the Volume Shadow Copy Service?"
      ],
      "answer": "Yes. However, shadow copies made prior to enabling BitLocker will be automatically deleted when BitLocker is enabled on software-encrypted drives. If you are using a hardware encrypted drive, the shadow copies are retained.",
      "score": 17.3,
      "id": 62,
      "source": "https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-frequently-asked-questions",
      "metadata": []
    },
...
  ]
}
```

[Başa dön](#HOLTop)

<a name="GetKB"></a>

## <a name="get-information-about-a-knowledge-base"></a>Bir bilgi bankası hakkında bilgi alma

Aşağıdaki kod [Get knowledge base details](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/getdetails) metodunu kullanarak belirtilen bilgi bankasıyla ilgili bilgileri alır.

1. Sık kullandığınız IDE'de yeni bir C# projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

// NOTE: Install the Newtonsoft.Json NuGet package.
using Newtonsoft.Json;

namespace QnAMaker
{
    class Program
    {
        static string host = "https://westus.api.cognitive.microsoft.com";
        static string service = "/qnamaker/v4.0";
        static string method = "/knowledgebases/";

        // NOTE: Replace this with a valid subscription key.
        static string key = "ENTER KEY HERE";

        // NOTE: Replace this with a valid knowledge base ID.
        static string kb = "ENTER ID HERE";

        static string PrettyPrint(string s)
        {
            return JsonConvert.SerializeObject(JsonConvert.DeserializeObject(s), Formatting.Indented);
        }

        async static Task<string> Get(string uri)
        {
            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = HttpMethod.Get;
                request.RequestUri = new Uri(uri);
                request.Headers.Add("Ocp-Apim-Subscription-Key", key);

                var response = await client.SendAsync(request);
                return await response.Content.ReadAsStringAsync();
            }
        }

        async static void GetKB()
        {
            var uri = host + service + method + kb;
            Console.WriteLine("Calling " + uri + ".");
            var response = await Get(uri);
            Console.WriteLine(PrettyPrint(response));
            Console.WriteLine("Press any key to continue.");
        }

        static void Main(string[] args)
        {
            GetKB();
            Console.ReadLine();
        }
    }
}

```

**Bilgi bankası ayrıntılarını alma yanıtı**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "id": "140a46f3-b248-4f1b-9349-614bfd6e5563",
  "hostName": "https://qna-docs.azurewebsites.net",
  "lastAccessedTimestamp": "2018-04-12T22:58:01Z",
  "lastChangedTimestamp": "2018-04-12T22:58:01Z",
  "name": "QnA Maker FAQ",
  "userId": "2280ef5917bb4ebfa1aae41fb1cebb4a",
  "urls": [
    "https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs",
    "https://docs.microsoft.com/bot-framework/resources-bot-framework-faq"
  ],
  "sources": [
    "Custom Editorial"
  ]
}
```

[Başa dön](#HOLTop)

<a name="GetKBsByUser"></a>

## <a name="get-all-knowledge-bases-for-a-user"></a>Bir kullanıcının tüm bilgi bankalarını alma

Aşağıdaki kod [Get knowledge bases for user](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/listall) metodunu kullanarak belirtilen bir kullanıcının tüm bilgi bankalarıyla ilgili bilgileri alır.

1. Sık kullandığınız IDE'de yeni bir C# projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

// NOTE: Install the Newtonsoft.Json NuGet package.
using Newtonsoft.Json;

namespace QnAMaker
{
    class Program
    {
        static string host = "https://westus.api.cognitive.microsoft.com";
        static string service = "/qnamaker/v4.0";
        static string method = "/knowledgebases";

        // NOTE: Replace this with a valid subscription key.
        static string key = "ENTER KEY HERE";

        static string PrettyPrint(string s)
        {
            return JsonConvert.SerializeObject(JsonConvert.DeserializeObject(s), Formatting.Indented);
        }

        async static Task<string> Get(string uri)
        {
            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = HttpMethod.Get;
                request.RequestUri = new Uri(uri);
                request.Headers.Add("Ocp-Apim-Subscription-Key", key);

                var response = await client.SendAsync(request);
                return await response.Content.ReadAsStringAsync();
            }
        }

        async static void GetKBsByUser()
        {
            var uri = host + service + method;
            Console.WriteLine("Calling " + uri + ".");
            var response = await Get(uri);
            Console.WriteLine(PrettyPrint(response));
            Console.WriteLine("Press any key to continue.");
        }

        static void Main(string[] args)
        {
            GetKBsByUser();
            Console.ReadLine();
        }
    }
}

```

**Kullanıcının bilgi bankalarını alma yanıtı**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "knowledgebases": [
    {
      "id": "081c32a7-bd05-4982-9d74-07ac736df0ac",
      "hostName": "https://qna-docs.azurewebsites.net",
      "lastAccessedTimestamp": "2018-04-12T11:51:58Z",
      "lastChangedTimestamp": "2018-04-12T11:51:58Z",
      "name": "QnA Maker FAQ",
      "userId": "2280ef5917bb4ebfa1aae41fb1cebb4a",
      "urls": [],
      "sources": []
    },
    {
      "id": "140a46f3-b248-4f1b-9349-614bfd6e5563",
      "hostName": "https://qna-docs.azurewebsites.net",
      "lastAccessedTimestamp": "2018-04-12T22:58:01Z",
      "lastChangedTimestamp": "2018-04-12T22:58:01Z",
      "name": "QnA Maker FAQ",
      "userId": "2280ef5917bb4ebfa1aae41fb1cebb4a",
      "urls": [
        "https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs",
        "https://docs.microsoft.com/bot-framework/resources-bot-framework-faq"
      ],
      "sources": [
        "Custom Editorial"
      ]
    },
...
  ]
}
Press any key to continue.
```

[Başa dön](#HOLTop)

<a name="Delete"></a>

## <a name="delete-a-knowledge-base"></a>Bilgi bankasını silme

Aşağıdaki kod [Delete knowledge base](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/delete) metodunu kullanarak belirtilen bilgi bankasını siler.

1. Sık kullandığınız IDE'de yeni bir C# projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

// NOTE: Install the Newtonsoft.Json NuGet package.
using Newtonsoft.Json;

namespace QnAMaker
{
    class Program
    {
        static string host = "https://westus.api.cognitive.microsoft.com";
        static string service = "/qnamaker/v4.0";
        static string method = "/knowledgebases/";

        // NOTE: Replace this with a valid subscription key.
        static string key = "ENTER KEY HERE";

        // NOTE: Replace this with a valid knowledge base ID.
        static string kb = "ENTER ID HERE";

        static string PrettyPrint(string s)
        {
            return JsonConvert.SerializeObject(JsonConvert.DeserializeObject(s), Formatting.Indented);
        }

        async static Task<string> Delete(string uri)
        {
            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = HttpMethod.Delete;
                request.RequestUri = new Uri(uri);
                request.Headers.Add("Ocp-Apim-Subscription-Key", key);

                var response = await client.SendAsync(request);
                if (response.IsSuccessStatusCode)
                {
                    return "{'result' : 'Success.'}";
                }
                else
                {
                    return await response.Content.ReadAsStringAsync();
                }
            }
        }

        async static void DeleteKB()
        {
            var uri = host + service + method + kb;
            Console.WriteLine("Calling " + uri + ".");
            var response = await Delete(uri);
            Console.WriteLine(PrettyPrint(response));
            Console.WriteLine("Press any key to continue.");
        }

        static void Main(string[] args)
        {
            DeleteKB();
            Console.ReadLine();
        }
    }
}
```

**Bilgi bankasını silme yanıtı**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "result": "Success."
}
```

[Başa dön](#HOLTop)

<a name="GetKeys"></a>

## <a name="get-endpoint-keys"></a>Uç nokta anahtarlarını alma

Aşağıdaki kod [Get endpoint keys](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/endpointkeys/getkeys) metodunu kullanarak geçerli uç nokta anahtarlarını alır.

1. Sık kullandığınız IDE'de yeni bir C# projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

// NOTE: Install the Newtonsoft.Json NuGet package.
using Newtonsoft.Json;

namespace QnAMaker
{
    class Program
    {
        static string host = "https://westus.api.cognitive.microsoft.com";
        static string service = "/qnamaker/v4.0";
        static string method = "/endpointkeys/";

        // NOTE: Replace this with a valid subscription key.
        static string key = "ENTER KEY HERE";

        static string PrettyPrint(string s)
        {
            return JsonConvert.SerializeObject(JsonConvert.DeserializeObject(s), Formatting.Indented);
        }

        async static Task<string> Get(string uri)
        {
            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = HttpMethod.Get;
                request.RequestUri = new Uri(uri);
                request.Headers.Add("Ocp-Apim-Subscription-Key", key);

                var response = await client.SendAsync(request);
                return await response.Content.ReadAsStringAsync();
            }
        }

        async static void GetEndpointKeys()
        {
            var uri = host + service + method;
            Console.WriteLine("Calling " + uri + ".");
            var response = await Get(uri);
            Console.WriteLine(PrettyPrint(response));
            Console.WriteLine("Press any key to continue.");
        }

        static void Main(string[] args)
        {
            GetEndpointKeys();
            Console.ReadLine();
        }
    }
}
```

**Uç nokta anahtarlarını alma yanıtı**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "primaryEndpointKey": "ac110bdc-34b7-4b1c-b9cd-b05f9a6001f3",
  "secondaryEndpointKey": "1b4ed14e-614f-444a-9f3d-9347f45a9206"
}
```

[Başa dön](#HOLTop)

<a name="PutKeys"></a>

## <a name="refresh-endpoint-keys"></a>Uç nokta anahtarlarını yenileme

Aşağıdaki kod [Refresh endpoint keys](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/endpointkeys/refreshkeys) metodunu kullanarak geçerli uç nokta anahtarlarını yeniden üretir.

1. Sık kullandığınız IDE'de yeni bir C# projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

// NOTE: Install the Newtonsoft.Json NuGet package.
using Newtonsoft.Json;

namespace QnAMaker
{
    class Program
    {
        static string host = "https://westus.api.cognitive.microsoft.com";
        static string service = "/qnamaker/v4.0";
        static string method = "/endpointkeys/";

        // NOTE: Replace this with a valid subscription key.
        static string key = "ENTER KEY HERE";

        // NOTE: Replace this with "PrimaryKey" or "SecondaryKey."
        static string key_type = "PrimaryKey";

        static string PrettyPrint(string s)
        {
            return JsonConvert.SerializeObject(JsonConvert.DeserializeObject(s), Formatting.Indented);
        }

        async static Task<string> Patch(string uri)
        {
            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = new HttpMethod("PATCH");
                request.RequestUri = new Uri(uri);
                request.Headers.Add("Ocp-Apim-Subscription-Key", key);

                var response = await client.SendAsync(request);
                return await response.Content.ReadAsStringAsync();
            }
        }

        async static void RefreshEndpoints()
        {
            var uri = host + service + method + key_type;
            Console.WriteLine("Calling " + uri + ".");
            var response = await Patch(uri);
            Console.WriteLine(PrettyPrint(response));
            Console.WriteLine("Press any key to continue.");
        }

        static void Main(string[] args)
        {
            RefreshEndpoints();
            Console.ReadLine();
        }
    }
}
```

**Uç nokta anahtarlarını yenileme yanıtı**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "primaryEndpointKey": "ac110bdc-34b7-4b1c-b9cd-b05f9a6001f3",
  "secondaryEndpointKey": "1b4ed14e-614f-444a-9f3d-9347f45a9206"
}
```

[Başa dön](#HOLTop)

<a name="GetAlterations"></a>

## <a name="get-word-alterations"></a>Sözcük değişikliklerini alma

Aşağıdaki kod [Download alterations](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/alterations/get) metodunu kullanarak geçerli sözcük değişikliklerini alır.

1. Sık kullandığınız IDE'de yeni bir C# projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

// NOTE: Install the Newtonsoft.Json NuGet package.
using Newtonsoft.Json;

namespace QnAMaker
{
    class Program
    {
        static string host = "https://westus.api.cognitive.microsoft.com";
        static string service = "/qnamaker/v4.0";
        static string method = "/alterations/";

        // NOTE: Replace this with a valid subscription key.
        static string key = "ENTER KEY HERE";

        static string PrettyPrint(string s)
        {
            return JsonConvert.SerializeObject(JsonConvert.DeserializeObject(s), Formatting.Indented);
        }

        async static Task<string> Get(string uri)
        {
            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = HttpMethod.Get;
                request.RequestUri = new Uri(uri);
                request.Headers.Add("Ocp-Apim-Subscription-Key", key);

                var response = await client.SendAsync(request);
                return await response.Content.ReadAsStringAsync();
            }
        }

        async static void GetAlterations()
        {
            var uri = host + service + method;
            Console.WriteLine("Calling " + uri + ".");
            var response = await Get(uri);
            Console.WriteLine(PrettyPrint(response));
            Console.WriteLine("Press any key to continue.");
        }

        static void Main(string[] args)
        {
            GetAlterations();
            Console.ReadLine();
        }
    }
}
```

**Sözcük değişikliklerini alma yanıtı**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "wordAlterations": [
    {
      "alterations": [
        "botframework",
        "bot frame work"
      ]
    }
  ]
}
```

[Başa dön](#HOLTop)

<a name="PutAlterations"></a>

## <a name="replace-word-alterations"></a>Sözcük değişikliklerini değiştirme

Aşağıdaki kod [Replace alterations](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/alterations/replace) metodunu kullanarak geçerli sözcük değişikliklerini değiştirir.

1. Sık kullandığınız IDE'de yeni bir C# projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

// NOTE: Install the Newtonsoft.Json NuGet package.
using Newtonsoft.Json;

namespace QnAMaker
{
    class Program
    {
        static string host = "https://westus.api.cognitive.microsoft.com";
        static string service = "/qnamaker/v4.0";
        static string method = "/alterations/";

        // NOTE: Replace this with a valid subscription key.
        static string key = "ENTER KEY HERE";

        static string alterations = @"
{
  'wordAlterations': [
    {
      'alterations': [
        'botframework',
        'bot frame work'
      ]
    }
  ]
}
";

        public struct Response
        {
            public HttpResponseHeaders headers;
            public string response;

            public Response(HttpResponseHeaders headers, string response)
            {
                this.headers = headers;
                this.response = response;
            }
        }

        static string PrettyPrint(string s)
        {
            return JsonConvert.SerializeObject(JsonConvert.DeserializeObject(s), Formatting.Indented);
        }

        async static Task<string> Put(string uri, string body)
        {
            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = new HttpMethod("PUT");
                request.RequestUri = new Uri(uri);
                request.Content = new StringContent(body, Encoding.UTF8, "application/json");
                request.Headers.Add("Ocp-Apim-Subscription-Key", key);

                var response = await client.SendAsync(request);
                if (response.IsSuccessStatusCode)
                {
                    return "{'result' : 'Success.'}";
                }
                else
                {
                    return await response.Content.ReadAsStringAsync();
                }
            }
        }

        async static void ReplaceAlterations(string alterations)
        {
            var uri = host + service + method;
            Console.WriteLine("Calling " + uri + ".");
            var response = await Put(uri, alterations);
            Console.WriteLine(PrettyPrint(response));
            Console.WriteLine("Press any key to continue.");
        }

        static void Main(string[] args)
        {
            ReplaceAlterations(alterations);
            Console.ReadLine();
        }
    }
}
```

**Sözcük değişikliklerini değiştirme yanıtı**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "result": "Success."
}
```

[Başa dön](#HOLTop)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://go.microsoft.com/fwlink/?linkid=2092179)

## <a name="see-also"></a>Ayrıca bkz. 

[Soru-Cevap Oluşturma’ya genel bakış](../Overview/overview.md)
