---
title: QnA Maker ve HALUK - Microsoft Bilişsel hizmetler tümleştirme | Microsoft Docs
titleSuffix: Azure
description: adım adım öğretici QnA Maker ve HALUK tümleştirme
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: 0a0eeb3815b793ed81f60b2b239bc459e5574788
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354112"
---
# <a name="integrate-qna-maker-and-luis-to-distribute-your-knowledge-base"></a>QnA Maker ve HALUK, Bilgi Bankası dağıtmak için tümleştirme
QnA Maker Bilgi Bankası büyük büyüdükçe, tek tek yapılı bir ayarlayın ve Bilgi Bankası küçük mantıksal parçalara bölmek için gerekirse olarak korumak zorlaşır.

Birden çok Bilgi Bankası QnA Maker'oluşturmak basit olsa da, uygun Bilgi Bankası'na gelen soru yönlendirmek için bazı mantığı gerekir. HALUK kullanarak bunu yapabilirsiniz.

## <a name="architecture"></a>Mimari

![QnA Maker Haluk mimarisi](../media/qnamaker-tutorials-qna-luis/qnamaker-luis-architecture.PNG)

Yukarıdaki senaryoda QnA Maker önce gelen soru amacı HALUK modelden alır ve, doğru QnA Maker Bilgi Bankası'na yönlendirmek için kullanın.

## <a name="prerequisites"></a>Önkoşullar
- Oturum [HALUK](https://www.luis.ai/) portal ve [bir uygulama oluşturmak](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/create-new-app).
- [Hedefleri Ekle](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/add-intents) senaryonuz göredir.
- [Tren](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-how-to-train) ve [yayımlama](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/publishapp) HALUK uygulamanızı.
- Oturum [QnA Maker](https://qnamaker.ai) ve [oluşturma bilgi]() senaryonuza uygun şekilde ayarlar.
- [Test]() ve [yayımlama]() Bilgi Bankası.

## <a name="qna-maker--luis-bot"></a>QnA Maker + HALUK Bot
1. Önce bir Web uygulaması bot HALUK şablonla oluşturabilir, yukarıda oluşturduğunuz HALUK uygulaması ile bağlantı ve hedefleri değiştirebilirsiniz. Ayrıntılı adımlar bkz [burada](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-csharp-tutorial-build-bot-framework-sample).

2. Bağımlılıklar diğer bağımlılıkları olan dosyanın en üstüne ekleyin:

    ```
    using RestSharp;
    using System.Collections.Generic;
    using Newtonsoft.Json;
    ```
3. Ekleme QnA Maker hizmetinizi çağırmak için sınıf altında:

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

3. Git https://qnamaker.ai -> My Bilgi Bankası, karşılık gelen Bilgi Bankası görünümü kodu ->. Aşağıdaki bilgileri alın:

    ![QnA Maker HTTP isteği](../media/qnamaker-tutorials-qna-luis/qnamaker-http-request.png)

4. QnAMakerService sınıfının her bir Bilgi Bankası için örneği:
    ```
            // Instantiate the knowledge bases
            public QnAMakerService hrQnAService = new QnAMakerService("https://hrkb.azurewebsites.net", "<HR knowledge base id>", "<HR endpoint key>");
            public QnAMakerService payrollQnAService = new QnAMakerService("https://payrollkb.azurewebsites.net", "<Payroll knowledge base id>", "<Payroll endpoint key>");
            public QnAMakerService financeQnAService = new QnAMakerService("https://financekb.azurewebsites.net", "<Finance knowledge base id>", "<Finance endpoint key>");
    ```

5. Karşılık gelen Bilgi Bankası hedefini çağırın.
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

## <a name="build-the-bot"></a>Bot derleme
1. Kod Düzenleyicisi'nde sağ tıklayın `build.cmd` seçip **konsolundan çalıştırma**.

    ![konsolundan çalıştırma](../media/qnamaker-tutorials-qna-luis/run-from-console.png)

2. Kod görünümünde ilerleme durumunu ve sonuçlarını derleme gösteren bir terminal penceresi ile değiştirilir.

    ![Konsol derleme](../media/qnamaker-tutorials-qna-luis/console-build.png)

## <a name="test-the-bot"></a>Bot test
Azure portalında seçin **Test Web sohbet** bot test etmek için. Karşılık gelen Bilgi Bankası'ndaki yanıt almak için farklı amaçlar gelen iletileri yazın.

![Web sohbeti testi](../media/qnamaker-tutorials-qna-luis/qnamaker-web-chat.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [QnA Maker için bir iş sürekliliği planı oluşturma](../How-To/business-continuity-plan.md)
