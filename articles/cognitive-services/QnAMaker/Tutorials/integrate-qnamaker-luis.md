---
title: Soru-cevap oluşturucu ve LUIS - Microsoft Bilişsel hizmetler | Microsoft Docs
titleSuffix: Azure
description: soru-cevap oluşturucu ve LUIS tümleştirme ilişkin adım adım öğretici
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: 18eae69867dc9774f63b11c762b22df4595bdce6
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2018
ms.locfileid: "43781756"
---
# <a name="integrate-qna-maker-and-luis-to-distribute-your-knowledge-base"></a>Soru-cevap oluşturucu ve LUIS bilgi bankanızı dağıtmak için tümleştirin
Soru-cevap Oluşturucu bankanızı büyük büyüdükçe, tek tek parça bir ayarlayın ve Bilgi Bankası daha küçük mantıksal parçalara bölmek için gerekir bakımını yapmak zor hale gelir.

Soru-cevap oluşturucu içinde birden çok bilgi bankaları oluşturmak kolay olsa da, uygun Bilgi Bankası'na gelen soru yönlendirmek için mantığa ihtiyacınız olacak. LUIS kullanarak bunu yapabilirsiniz.

## <a name="architecture"></a>Mimari

![Soru-cevap Oluşturucu luıs mimarisi](../media/qnamaker-tutorials-qna-luis/qnamaker-luis-architecture.PNG)

Yukarıdaki senaryoda soru-cevap Oluşturucu önce gelen soru amacı LUIS modelden alır ve doğru soru-cevap Oluşturucu Bilgi Bankası'na yönlendirmek için bunu kullanın.

## <a name="prerequisites"></a>Önkoşullar
- Oturum [LUIS](https://www.luis.ai/) portalı ve [uygulama oluşturma](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/create-new-app).
- [Hedef ekleme](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/add-intents) senaryonuza göre.
- [Train](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-how-to-train) ve [yayımlama](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/publishapp) LUIS uygulamanızı.
- Oturum [soru-cevap Oluşturucu](https://qnamaker.ai) ve [oluşturma](https://www.qnamaker.ai/Create) bilgi bankalarından senaryonuza göre.
- Test edin ve bilgi bankalarından yayımlayın.

## <a name="qna-maker--luis-bot"></a>Soru-cevap Oluşturucu + LUIS Robotu
1. İlk LUIS şablonu ile bir Web App botu oluşturun, yukarıda oluşturduğunuz LUIS uygulaması ile bağlantı ve hedefleri değiştirin. Ayrıntılı adımlar için bkz: [burada](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-csharp-tutorial-build-bot-framework-sample).

2. Bağımlılıklar, diğer bağımlılıkları olan dosyanın üstüne ekleyin:

    ```
    using RestSharp;
    using System.Collections.Generic;
    using Newtonsoft.Json;
    ```
3. Ekleme, soru-cevap Oluşturucu hizmetini çağırmak için sınıf aşağıda:

    ```
        /// <summary>
        /// QnAMakerService is a wrapper over the QnA Maker REST endpoint
        /// </summary>
        [Serializable]
        public class QnAMakerService
        {
            private string qnaServiceHostName;
            private string knowledgeBaseId;
            private string endpointKey;
    
            /// <summary>
            /// Initialize a particular endpoint with it's details
            /// </summary>
            /// <param name="hostName">Hostname of the endpoint</param>
            /// <param name="kbId">Knowledge base ID</param>
            /// <param name="ek">Endpoint Key</param>
            public QnAMakerService(string hostName, string kbId, string ek)
            {
                qnaServiceHostName = hostName;
                knowledgeBaseId = kbId;
                endpointKey = ek;
            }
    
            /// <summary>
            /// Call the QnA Maker endpoint and get a response
            /// </summary>
            /// <param name="query">User question</param>
            /// <returns></returns>
            public string GetAnswer(string query)
            {
                var client = new RestClient( qnaServiceHostName + "/qnamaker/knowledgebases/" + knowledgeBaseId + "/generateAnswer");
                var request = new RestRequest(Method.POST);
                request.AddHeader("authorization", "EndpointKey " + endpointKey);
                request.AddHeader("content-type", "application/json");
                request.AddParameter("application/json", "{\"question\": \"" + query + "\"}", ParameterType.RequestBody);
                IRestResponse response = client.Execute(request);
    
                // Deserialize the response JSON
                QnAAnswer answer = JsonConvert.DeserializeObject<QnAAnswer>(response.Content);
    
                // Return the answer if present
                if (answer.answers.Count > 0)
                    return answer.answers[0].answer;
                else
                    return "No good match found.";
            }
        }
    
        /* START - QnA Maker Response Class */
        public class Metadata
        {
            public string name { get; set; }
            public string value { get; set; }
        }
    
        public class Answer
        {
            public IList<string> questions { get; set; }
            public string answer { get; set; }
            public double score { get; set; }
            public int id { get; set; }
            public string source { get; set; }
            public IList<object> keywords { get; set; }
            public IList<Metadata> metadata { get; set; }
        }
    
        public class QnAAnswer
        {
            public IList<Answer> answers { get; set; }
        }
        /* END - QnA Maker Response Class */
    ```

3. Git https://qnamaker.ai -> My bilgi bankalarından karşılık gelen Bilgi Bankası tabanınızın Kodu Görüntüle ->. Aşağıdaki bilgileri edinin:

    ![Soru-cevap Oluşturucu HTTP isteği](../media/qnamaker-tutorials-qna-luis/qnamaker-http-request.png)

4. QnAMakerService sınıfının her biri, bilgi bankaları için örneği:
    ```
            // Instantiate the knowledge bases
            public QnAMakerService hrQnAService = new QnAMakerService("https://hrkb.azurewebsites.net", "<HR knowledge base id>", "<HR endpoint key>");
            public QnAMakerService payrollQnAService = new QnAMakerService("https://payrollkb.azurewebsites.net", "<Payroll knowledge base id>", "<Payroll endpoint key>");
            public QnAMakerService financeQnAService = new QnAMakerService("https://financekb.azurewebsites.net", "<Finance knowledge base id>", "<Finance endpoint key>");
    ```

5. Karşılık gelen Bilgi Bankası için hedefi arayın.
    ```
            // HR Intent
            [LuisIntent("HR")]
            public async Task CancelIntent(IDialogContext context, LuisResult result)
            {
                // Ask the HR knowledge base
                await context.PostAsync(hrQnAService.GetAnswer(result.Query));
            }
    
            // Payroll intent
            [LuisIntent("Payroll")]
            public async Task GreetingIntent(IDialogContext context, LuisResult result)
            {
                // Ask the payroll knowledge base
                await context.PostAsync(payrollQnAService.GetAnswer(result.Query));
            }
    
            // Finance intent
            [LuisIntent("Finance")]
            public async Task HelpIntent(IDialogContext context, LuisResult result)
            {
                // Ask the finance knowledge base
                await context.PostAsync(financeQnAService.GetAnswer(result.Query));
            }
    ```

## <a name="build-the-bot"></a>Bot oluşturma
1. Kod Düzenleyicisi'nde sağ `build.cmd` seçip **konsolundan çalıştırın**.

    ![konsolundan çalıştırın](../media/qnamaker-tutorials-qna-luis/run-from-console.png)

2. Kod görünümü, bir terminal penceresi yapının sonuçlarını ve ilerleme durumunu gösteren değiştirilir.

    ![Konsol derleme](../media/qnamaker-tutorials-qna-luis/console-build.png)

## <a name="test-the-bot"></a>Bot test
Azure portalında **Test Web sohbeti içinde** bot test etmek için. Karşılık gelen Bilgi Bankası'ndaki yanıt almak için farklı amaçlarla gelen iletileri yazın.

![Web sohbeti test](../media/qnamaker-tutorials-qna-luis/qnamaker-web-chat.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-cevap Oluşturucu için bir iş süreklilik planı oluşturma](../How-To/business-continuity-plan.md)
