---
title: LUIS ve QnAMaker - Bot tümleştirme
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu bankanızı büyük büyüdükçe, tek tek parça bir ayarlayın ve Bilgi Bankası daha küçük mantıksal parçalara bölmek için gerekir bakımını yapmak zor hale gelir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 06/11/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 1792cf2359caef3211b4ce1ac86928eeb85d682b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67053169"
---
# <a name="use-bot-with-qna-maker-and-luis-to-distribute-your-knowledge-base"></a>Bilgi bankanızı dağıtmak için soru-cevap oluşturucu ve LUIS ile bot kullanın
Soru-cevap Oluşturucu bankanızı büyük büyüdükçe, tek tek parça bir ayarlayın ve Bilgi Bankası daha küçük mantıksal parçalara bölmek için gerekir bakımını yapmak zor hale gelir.

Soru-cevap oluşturucu içinde birden çok bilgi bankaları oluşturmak kolay olsa da, uygun Bilgi Bankası'na gelen soru yönlendirmek için mantığa ihtiyacınız olacak. LUIS kullanarak bunu yapabilirsiniz.

Bu makalede, Bot Framework v3 SDK'sını kullanır. Lütfen bu makaleye [Bot Framework makale](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-dispatch?view=azure-bot-service-4.0&tabs=csharp), Bot Framework v4 SDK sürümünde bu bilgilerin ilgileniyorsanız.

## <a name="architecture"></a>Mimari

![Language Understanding mimarisi ile soru-cevap Oluşturucu](../media/qnamaker-tutorials-qna-luis/qnamaker-luis-architecture.PNG)

Yukarıdaki senaryoda soru-cevap Oluşturucu önce gelen soru amacı LUIS modelden alır ve doğru soru-cevap Oluşturucu Bilgi Bankası'na yönlendirmek için bunu kullanın.

## <a name="create-a-luis-app"></a>Bir LUIS uygulaması oluşturma

1. Oturum [LUIS](https://www.luis.ai/) portalı.
1. [Uygulama oluşturma](https://docs.microsoft.com/azure/cognitive-services/luis/create-new-app).
1. [Bir ekleme](https://docs.microsoft.com/azure/cognitive-services/luis/add-intents) her soru-cevap Oluşturucu Bilgi Bankası için. Örnek konuşma soru-cevap Oluşturucu bilgi bankalarından karşılık gelmelidir.
1. [LUIS uygulaması eğitme](https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-train) ve [LUIS uygulaması yayımlama](https://docs.microsoft.com/azure/cognitive-services/luis/publishapp) LUIS uygulamanızı.
1. İçinde **Yönet** bölümünde, LUIS uygulama kimliği, LUIS uç noktası anahtarı Not ve ana bölge. Bu değerler daha sonra gerekecektir. 

## <a name="create-qna-maker-knowledge-bases"></a>Soru-cevap Oluşturucu bilgi bankaları oluşturmak

1. Oturum [soru-cevap Oluşturucu](https://qnamaker.ai).
1. [Oluşturma](https://www.qnamaker.ai/Create) bir bilgi bankaları için her amaca LUIS uygulaması.
1. Test edin ve bilgi bankalarından yayımlayın. Her KB Yayımla'nı bb Kimliğini not edin, ana bilgisayar (alt etki alanı önce _.azurewebsites.net/qnamaker_) ve yetkilendirme uç noktası anahtarı. Bu değerler daha sonra gerekecektir. 

    Bu makalede, tüm Azure soru-cevap Oluşturucu aynı abonelikte oluşturulan KB'leri varsayılır.

    ![Soru-cevap Oluşturucu HTTP isteği](../media/qnamaker-tutorials-qna-luis/qnamaker-http-request.png)

## <a name="web-app-bot"></a>Web app Botu

1. [Bir "Temel" Web App botu oluşturun](https://docs.microsoft.com/azure/bot-service/bot-service-quickstart?view=azure-bot-service-4.0) LUIS uygulaması otomatik olarak içerir. ' % S ' 4.x SDK'yı seçin ve C# programlama dilidir.

1. Web app botu oluşturulduğunda, Azure portalında web app botu seçin.
1. Seçin **uygulama ayarları** Web app botu hizmeti Gezinti ardından aşağı kaydırarak **uygulama ayarları** kullanılabilir Ayarlar bölümünde.
1. Değişiklik **LuisAppId** ve önceki bölümde ardından select oluşturulan LUIS uygulaması değerine **Kaydet**.


## <a name="change-code-in-basicluisdialogcs"></a>BasicLuisDialog.cs kodda değişiklik
1. Gelen **Bot Yönetim** select Azure portalında web app botu Gezinti bölümde **yapı**.
2. Seçin **açık çevrimiçi Kod Düzenleyicisi**. İle çevrimiçi düzenleme ortamınızda yeni bir tarayıcı sekmesi açılır. 
3. İçinde **WWWROOT** bölümünden **iletişim kutuları** dizinine sonra da açık **BasicLuisDialog.cs**.
4. Bağımlılıkları üst kısmına ekleyin **BasicLuisDialog.cs** dosyası:

    ```csharp
    using System;
    using System.Net.Http;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    using Microsoft.Bot.Builder.Dialogs;
    using Microsoft.Bot.Builder.Luis;
    using Microsoft.Bot.Builder.Luis.Models;
    using Newtonsoft.Json;
    using System.Text;
    ```

5. Ekleme aşağıdaki soru-cevap Oluşturucu yanıt seri durumdan çıkarmak için sınıflar:

    ```csharp
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
    ```


6. Soru-cevap Oluşturucu hizmeti için bir HTTP isteği yapmak için aşağıdaki sınıf ekleyin. Dikkat **yetkilendirme** üst bilginin değeri içeren bir word `EndpointKey` sözcüğünü izleyen bir alana sahip. JSON sonuç önceki sınıflara seri durumdan ve ilk yanıt döndürülür.

    ```csharp
    [Serializable]
    public class QnAMakerService
    {
        private string qnaServiceHostName;
        private string knowledgeBaseId;
        private string endpointKey;

        public QnAMakerService(string hostName, string kbId, string endpointkey)
        {
            qnaServiceHostName = hostName;
            knowledgeBaseId = kbId;
            endpointKey = endpointkey;

        }
        async Task<string> Post(string uri, string body)
        {
            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = HttpMethod.Post;
                request.RequestUri = new Uri(uri);
                request.Content = new StringContent(body, Encoding.UTF8, "application/json");
                request.Headers.Add("Authorization", "EndpointKey " + endpointKey);

                var response = await client.SendAsync(request);
                return  await response.Content.ReadAsStringAsync();
            }
        }
        public async Task<string> GetAnswer(string question)
        {
            string uri = qnaServiceHostName + "/qnamaker/knowledgebases/" + knowledgeBaseId + "/generateAnswer";
            string questionJSON = "{\"question\": \"" + question.Replace("\"","'") +  "\"}";

            var response = await Post(uri, questionJSON);

            var answers = JsonConvert.DeserializeObject<QnAAnswer>(response);
            if (answers.answers.Count > 0)
            {
                return answers.answers[0].answer;
            }
            else
            {
                return "No good match found.";
            }
        }
    }
    ```


7. BasicLuisDialog sınıfı değiştirin. Her LUIS hedefi bir yöntem ile donatılmış olmalıdır **LuisIntent**. Düzenleme parametresi gerçek LUIS hedefi addır. Donatılmış yöntem adı _gereken_ LUIS hedefi ad okunabilirliği ve bakım için ancak Tasarım aynı olması veya çalışma zamanı gerekli değildir.  

    ```csharp
    [Serializable]
    public class BasicLuisDialog : LuisDialog<object>
    {
        // LUIS Settings
        static string LUIS_appId = "<LUIS APP ID>";
        static string LUIS_apiKey = "<LUIS API KEY>";
        static string LUIS_hostRegion = "westus.api.cognitive.microsoft.com";

        // QnA Maker global settings
        // assumes all KBs are created with same Azure service
        static string qnamaker_endpointKey = "<QnA Maker endpoint KEY>";
        static string qnamaker_endpointDomain = "my-qnamaker-s0-s";
        
        // QnA Maker Human Resources Knowledge base
        static string HR_kbID = "<QnA Maker KNOWLEDGE BASE ID>";

        // QnA Maker Finance Knowledge base
        static string Finance_kbID = "<QnA Maker KNOWLEDGE BASE ID>";

        // Instantiate the knowledge bases
        public QnAMakerService hrQnAService = new QnAMakerService("https://" + qnamaker_endpointDomain + ".azurewebsites.net", HR_kbID, qnamaker_endpointKey);
        public QnAMakerService financeQnAService = new QnAMakerService("https://" + qnamaker_endpointDomain + ".azurewebsites.net", Finance_kbID, qnamaker_endpointKey);

        public BasicLuisDialog() : base(new LuisService(new LuisModelAttribute(
            LUIS_appId,
            LUIS_apiKey,
            domain: LUIS_hostRegion)))
        {
        }

        [LuisIntent("None")]
        public async Task NoneIntent(IDialogContext context, LuisResult result)
        {
            HttpClient client = new HttpClient();
            await this.ShowLuisResult(context, result);
        }

        // HR Intent
        [LuisIntent("HR")]
        public async Task HumanResourcesIntent(IDialogContext context, LuisResult result)
        {
            // Ask the HR knowledge base
            var qnaMakerAnswer = await hrQnAService.GetAnswer(result.Query);
            await context.PostAsync($"{qnaMakerAnswer}");
            context.Wait(MessageReceived);
        }

        // Finance intent
        [LuisIntent("Finance")]
        public async Task FinanceIntent(IDialogContext context, LuisResult result)
        {
            // Ask the finance knowledge base
            var qnaMakerAnswer = await financeQnAService.GetAnswer(result.Query);
            await context.PostAsync($"{qnaMakerAnswer}");
            context.Wait(MessageReceived);
        }
        private async Task ShowLuisResult(IDialogContext context, LuisResult result)
        {
            await context.PostAsync($"You have reached {result.Intents[0].Intent}. You said: {result.Query}");
            context.Wait(MessageReceived);
        }
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
