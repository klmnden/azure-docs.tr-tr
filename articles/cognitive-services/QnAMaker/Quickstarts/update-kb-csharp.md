---
title: 'Hızlı Başlangıç: C# bilgi bankası güncelleştirme - Soru-Cevap Oluşturucu'
titleSuffix: Azure Cognitive Services
description: C#’de Soru-Cevap Oluşturma için bilgi bankası nasıl güncelleştirilir?
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: quickstart
ms.date: 09/12/2018
ms.author: diberry
ms.openlocfilehash: c07f2272ed370060fc7334dfe3c52514a726dc33
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47035836"
---
# <a name="update-a-knowledge-base-in-c"></a>C#’de bilgi bankası güncelleştirme

Aşağıdaki kod, [Güncelleştirme](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600) yöntemini kullanarak mevcut bir bilgi bankasını güncelleştirir.

[!INCLUDE [Code is available in Azure-Samples Github repo](../../../../includes/cognitive-services-qnamaker-csharp-repo-note.md)]

Henüz bir bilgi bankanız yoksa, bu hızlı başlangıçta kullanmak için bir örneğini oluşturabilirsiniz: [Yeni bilgi bankası oluşturma](create-new-kb-csharp.md).

1. Sık kullandığınız IDE’de yeni bir .NET Framework C# konsol uygulaması oluşturun.
1. Aşağıda sağlanan kodu ekleyin.
1. `key` değerini geçerli bir abonelik anahtarı ile değiştirin.
1. `kb` değerini geçerli bir bilgi bankası kimliği ile değiştirin. [Soru-Cevap Oluşturma bilgi bankalarınızdan](https://www.qnamaker.ai/Home/MyServices) birine giderek bu değeri bulun. Güncelleştirmek istediğiniz bilgi bankasını seçin. Söz konusu sayfaya geldiğinizde, aşağıda gösterildiği gibi URL’de 'kdid=' öğesini bulun. Kod örneğiniz için değerini kullanın.

    ![Soru-Cevap Oluşturma bilgi bankası kimliği](../media/qnamaker-quickstart-kb/qna-maker-id.png)

1. Programı çalıştırın.

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
        // Represents the various elements used to create HTTP request URIs
        // for QnA Maker operations.
        static string host = "https://westus.api.cognitive.microsoft.com";
        static string service = "/qnamaker/v4.0";
        static string method = "/knowledgebases/";

        // NOTE: Replace this with a valid subscription key.
        static string key = "ADD KEY HERE";

        // NOTE: Replace this with a valid knowledge base ID.
        static string kb = "ADD ID HERE";

        /// <summary>
        /// Defines the data source used to update the knowledge base.
        /// This JSON schema is based on your existing knowledge base.
        /// In the 'update' object, the existing name is changed.
        /// </summary>
        static string new_kb = @"
        {
          'add': {
            'qnaList': [
              {
                'id': 1,
                'answer': 'You can change the default message if you use the QnAMakerDialog. See this for details: https://docs.botframework.com/en-us/azure-bot-service/templates/qnamaker/#navtitle',
                'source': 'Custom Editorial',
                'questions': [
                  'How can I change the default message from QnA Maker?'
                ],
                'metadata': []
              }
            ],
            'urls': []
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
        /// <summary>
        /// Represents the HTTP response returned by an HTTP request.
        /// </summary>
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

        /// <summary>
        /// Formats and indents JSON for display.
        /// </summary>
        /// <param name="s">The JSON to format and indent.</param>
        /// <returns>A string containing formatted and indented JSON.</returns>
        static string PrettyPrint(string s)
        {
            return JsonConvert.SerializeObject(JsonConvert.DeserializeObject(s), Formatting.Indented);
        }

        /// <summary>
        /// Asynchronously sends a PATCH HTTP request.
        /// </summary>
        /// <param name="uri">The URI of the HTTP request.</param>
        /// <param name="body">The body of the HTTP request.</param>
        /// <returns>A <see cref="System.Threading.Tasks.Task{TResult}(QnAMaker.Program.Response)"/> 
        /// object that represents the HTTP response."</returns>
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

        /// <summary>
        /// Asynchronously sends a GET HTTP request.
        /// </summary>
        /// <param name="uri">The URI of the HTTP request.</param>
        /// <returns>A <see cref="System.Threading.Tasks.Task{TResult}(QnAMaker.Program.Response)"/> 
        /// object that represents the HTTP response."</returns>
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

        /// <summary>
        /// Updates a knowledge base.
        /// </summary>
        /// <param name="kb">The ID for the existing knowledge base.</param>
        /// <param name="new_kb">The new data source for the updated knowledge base.</param>
        /// <returns>A <see cref="System.Threading.Tasks.Task{TResult}(QnAMaker.Program.Response)"/>
        /// object that represents the HTTP response."</returns>
        /// <remarks>Constructs the URI to update a knowledge base in QnA Maker,
        /// then asynchronously invokes the <see cref="QnAMaker.Program.Patch(string, string)"/>
        /// method to send the HTTP request.</remarks>
        async static Task<Response> PostUpdateKB(string kb, string new_kb)
        {
            string uri = host + service + method + kb;
            Console.WriteLine("Calling " + uri + ".");
            return await Patch(uri, new_kb);
        }

        /// <summary>
        /// Gets the status of the specified QnA Maker operation.
        /// </summary>
        /// <param name="operation">The QnA Maker operation to check.</param>
        /// <returns>A <see cref="System.Threading.Tasks.Task{TResult}(QnAMaker.Program.Response)"/>
        /// object that represents the HTTP response."</returns>
        /// <remarks>Constructs the URI to get the status of a QnA Maker
        /// operation, then asynchronously invokes the <see cref="QnAMaker.Program.Get(string)"/>
        /// method to send the HTTP request.</remarks>
        async static Task<Response> GetStatus(string operation)
        {
            string uri = host + service + operation;
            Console.WriteLine("Calling " + uri + ".");
            return await Get(uri);
        }

        /// <summary>
        /// Updates a knowledge base, periodically checking status
        /// until the knowledge base is updated.
        /// </summary>
        async static void UpdateKB(string kb, string new_kb)
        {
            try
            {
                // Starts the QnA Maker operation to update the knowledge base.
                var response = await PostUpdateKB(kb, new_kb);

                // Retrieves the operation ID, so the operation's status can be
                // checked periodically.
                var operation = response.headers.GetValues("Location").First();

                // Displays the JSON in the HTTP response returned by the 
                // PostUpdateKB(string, string) method.
                Console.WriteLine(PrettyPrint(response.response));

                // Iteratively gets the state of the operation updating the
                // knowledge base. Once the operation state is something other
                // than "Running" or "NotStarted", the loop ends.
                var done = false;
                while (true != done)
                {
                    // Gets the status of the operation.
                    response = await GetStatus(operation);
                    // Displays the JSON in the HTTP response returned by the
                    // GetStatus(string) method.
                    Console.WriteLine(PrettyPrint(response.response));

                    // Deserialize the JSON into key-value pairs, to retrieve the
                    // state of the operation.
                    var fields = JsonConvert.DeserializeObject<Dictionary<string, string>>(response.response);

                    // Gets and checks the state of the operation.
                    String state = fields["operationState"];
                    if (state.CompareTo("Running") == 0 || state.CompareTo("NotStarted") == 0)
                    {
                        // QnA Maker is still updating the knowledge base. The thread is
                        // paused for a number of seconds equal to the Retry-After
                        // header value, and then the loop continues.
                        var wait = response.headers.GetValues("Retry-After").First();
                        Console.WriteLine("Waiting " + wait + " seconds...");
                        Thread.Sleep(Int32.Parse(wait) * 1000);
                    }
                    else
                    {
                        // QnA Maker has completed updating the knowledge base.
                        done = true;
                    }
                }
            }
            catch
            {
                // An error occurred while updating the knowledge base. Ensure that
                // you included your QnA Maker subscription key and knowledge base ID
                // where directed in the sample.
                Console.WriteLine("An error occurred while updating the knowledge base.");
            }
            finally
            {
                Console.WriteLine("Press any key to continue.");
            }
        }

        static void Main(string[] args)
        {
            // Invoke the UpdateKB() method to update a knowledge base, periodically
            // checking the status of the QnA Maker operation until the
            // knowledge base is updated.
            UpdateKB(kb, new_kb);

            // The console waits for a key to be pressed before closing.
            Console.ReadLine();
        }
    }
}

```

## <a name="understand-what-qna-maker-returns"></a>Soru-Cevap Oluşturma’nın ne döndürdüğünü anlama

Başarılı bir yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür. Sonuçlarınız biraz farklılık gösterebilir. Son çağrı "Başarılı" durumunu döndürürse, bilgi bankanız başarıyla güncelleştirilmiştir. Sorun gidermek için Soru-Cevap Oluşturma API'sine ait [Bilgi Bankasını Güncelleştir](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600) yanıt kodlarına başvurun.

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

Bilgi bankanız güncelleştirildikten sonra Soru-Cevap Oluşturma Portalı’nızdaki [Bilgi bankalarım](https://www.qnamaker.ai/Home/MyServices) sayfasından görüntüleyebilirsiniz. Bilgi bankanızın adının değiştiğine dikkat edin, örneğin Soru-Cevap Oluşturma SSS (veya önceden mevcut olan bilgi bankası) artık Yeni bilgi Bankası Adı olarak adlandırılır.

Bilgi bankanızın diğer öğelerini değiştirmek için Soru-Cevap Oluşturma [JSON şemasına](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600) başvurun ve `new_kb` dizesini değiştirin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)