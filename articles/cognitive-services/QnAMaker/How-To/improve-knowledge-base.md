---
title: Bilgi Bankası - soru-cevap Oluşturucu geliştirin
titleSuffix: Azure Cognitive Services
description: Etkin öğrenme bilgi bankanızı alternatif sorular, soru ve yanıt çiftinizi için kullanıcı-gönderimler göre önererek kalitesini sağlar. Bu öneriler, ya da sorularınız var ya da bunları reddetmeniz ekleme inceleyin. Bilgi bankanızı otomatik olarak değişmez. Herhangi bir değişikliğin etkili olması için önerileri kabul etmeniz gerekir. Bu öneriler sorular ekleyebilir ancak yoksa değiştirebilir veya de soru kaldırabilirsiniz.
author: diberry
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 06/19/2019
ms.author: diberry
ms.openlocfilehash: b73884e544ea1b8ee76c8a891048e6a8e17d6ab3
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67204086"
---
# <a name="use-active-learning-to-improve-your-knowledge-base"></a>Etkin öğrenme bilgi bankanızı geliştirmek için kullanın

Etkin öğrenme bilgi bankanızı alternatif sorular, soru ve yanıt çiftinizi için kullanıcı-gönderimler göre önererek kalitesini sağlar. Bu öneriler, ya da sorularınız var ya da bunları reddetmeniz ekleme inceleyin. 

Bilgi bankanızı otomatik olarak değişmez. Herhangi bir değişikliğin etkili olması için önerileri kabul etmeniz gerekir. Bu öneriler sorular ekleyebilir ancak yoksa değiştirebilir veya de soru kaldırabilirsiniz.

## <a name="what-is-active-learning"></a>Etkin öğrenme nedir?

Soru-cevap Oluşturucu örtük ve açık bir geri bildirim ile soru çeşitlemesini öğrenir.
 
* [Örtük geri bildirim](#how-qna-makers-implicit-feedback-works) – derecelendiricisini kullanıcı soru çok yakın puanlar ile birden çok yanıtlar olduğunda anlar ve bu geri bildirim göz önünde bulundurur. Bunun gerçekleşmesi için herhangi bir şey yapmanız gerekmez.
* [Açık bir geri bildirim](#how-you-give-explicit-feedback-with-the-train-api) – bilgi bankasından puanları çok az varyasyonu ile birden çok yanıt döndürüldüğünde istemci uygulamayı doğru soruyu hangi soru ise kullanıcıya sorar. Soru-cevap Oluşturucu ile kullanıcının açık geri bildirim gönderilmesini [eğitme API](#train-api). 

Her iki yöntem de derecelendiricisini kümelenir benzer sorgularla sağlar.

## <a name="how-active-learning-works"></a>Nasıl etkin works öğrenme

Etkin öğrenme, soru-cevap Oluşturucu tarafından döndürülen ilk birkaç yanıtların puanları temel tetiklenir. Puan farklılıkları küçük bir aralık içinde yer alan, ardından sorgu (gibi alternatif bir soru) bir olası öneri her olası soru-cevap çiftlerini kabul edilir. Belirli bir soru-cevap çifti için önerilen soru kabul ettikten sonra diğer çiftleri için reddedilir. Kaydet ve önerileri kabul ettikten sonra eğitmek hatırlamanız gerekir.

Etkin öğrenme makul bir miktar ve çeşitli sorgular kullanım uç noktalar burada alma durumlarda en iyi olası öneriler sağlar. 5 veya daha fazla benzer sorguları kümelenir, 30 dakikada kabul etmek veya reddetmek için Bilgi Bankası Tasarımcısı kullanıcı tabanlı soruların soru-cevap Oluşturucu önerir. Tüm öneriler birlikte benzerliğe göre kümelenir ve diğer sorular için en çok istenen önerilerden belirli sorgularının sıklığını üzerinde son kullanıcılar tarafından bağlı görüntülenir.

Sorular soru-cevap Oluşturucu Portalı'nda önerilen sonra gözden geçirip kabul edin ya da bu önerileri Reddet gerekir. Öneriler yönetmek için bir API yoktur.

## <a name="how-qna-makers-implicit-feedback-works"></a>Soru-cevap Oluşturucu'nın örtük geri bildirim nasıl çalışır?

Soru-cevap Oluşturucu'nın örtük geri bildirim puanı yakınlık belirlemek sonra etkin olarak öğrenmeye önerilerde bulunmak için bir algoritma kullanır. Algoritma yakınlık belirlemek için basit bir hesaplama değil. Aralıkları aşağıdaki örnekte sabit beklenmez, ancak algoritma yalnızca etkisini anlamak için kılavuz olarak kullanılmalıdır.

Bir sorunun puan % gibi 80, yüksek oranda emin olduğu durumlarda, etkin öğrenim için kabul edilen puanları aralığını yaklaşık %10 içinde geniş. Güvenilirlik puanı azaldıkça %40 gibi puanları aralığını yaklaşık %4 içinde de azaltır. 

## <a name="how-you-give-explicit-feedback-with-the-train-api"></a>Train API'si ile açık bir geri bildirimde nasıl

Soru-cevap Oluşturucu'açık geri bildirim yanıtları hakkında en iyi cevabı olduğu alır önemlidir. En iyi cevabı nasıl belirlendiğini size bağlıdır ve dahil edebilirsiniz:

* Yanıtları birini seçerek Kullanıcı bildirim.
* Kabul edilebilir bir belirleme gibi iş mantığı, puan aralığı.  
* Her iki kullanıcı geri bildirim ve iş mantığı birleşimi.

## <a name="upgrade-your-runtime-version-to-use-active-learning"></a>Etkin öğrenme kullanmak için çalışma zamanı sürümü yükseltme

Etkin öğrenme, çalışma zamanı sürümü 4.4.0 ve sonraki sürümlerde desteklenir. Bilgi bankanızı bir önceki sürümünde oluşturulduysa [, çalışma zamanını yükseltme](troubleshooting-runtime.md#how-to-get-latest-qnamaker-runtime-updates) bu özelliği kullanmak için. 

## <a name="turn-on-active-learning-to-see-suggestions"></a>Etkin önerileri görmek öğrenme üzerinde Aç

Varsayılan olarak etkin olarak öğrenmeye kapalıdır. Bu önerilen bu soruları görmek için etkinleştirin. Etkin öğrenmeyi etkinleştirdikten sonra soru-cevap Oluşturucu için istemci uygulamasından bilgi göndermesine izin gerekir. Daha fazla bilgi için [bir bot GenerateAnswer ve eğitme API'lerini kullanarak mimari akışı](#architectural-flow-for-using-generateanswer-and-train-apis-from-a-bot).

1. Seçin **Yayımla** Bilgi Bankası yayımlama. Etkin öğrenme sorguları GenerateAnswer API tahmin uç noktasından yalnızca toplanır. Soru-cevap Oluşturucu Portalı'nda Test bölmesine sorgular, etkin öğrenim etkilemez.

1. Üzerinde soru-cevap Oluşturucu Portalı'nda öğrenme etkin açmak için sağ üst köşeye select gidin, **adı**Git [ **hizmet ayarları**](https://www.qnamaker.ai/UserSettings).  

    ![Etkin öğrenme'nin önerilen soru alternatifleri hizmet ayarları sayfasından etkinleştirin. Sağ üst menüsünde kullanıcı adınızı seçin, sonra hizmet ayarlarını seçin.](../media/improve-knowledge-base/Endpoint-Keys.png)


1. Soru-cevap Oluşturucu hizmetini bulun, sonra geçiş **etkin olarak öğrenmeye**. 

    [![Hizmet Ayarları sayfasında, etkin öğrenim özelliğini Değiştir. Özellik geçiş yapmak mümkün değilse, hizmetinizi yükseltmeniz gerekebilir.](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png)](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png#lightbox)

    Bir kez **etkin olarak öğrenmeye** olan etkin, Bilgi Bankası yeni sorular kullanıcı tarafından gönderilen sorular temel alınarak düzenli aralıklarla önerir. Devre dışı bırakabilirsiniz **etkin olarak öğrenmeye** ayarın açılıp tarafından yeniden.

## <a name="accept-an-active-learning-suggestion-in-the-knowledge-base"></a>Bilgi Bankası'nda bir etkin olarak öğrenmeye öneriyi kabul edin

1. Önerilen sorular görmek için **Düzenle** Bilgi Bankası sayfasında **görüntüleme seçenekleri**, ardından **etkin olarak öğrenmeye önerileri göster**. 

    [![Portal Düzenle bölümüne etkin olarak öğrenmeye ait yeni soru seçenekleri görmek için öneriler Göster'i seçin.](../media/improve-knowledge-base/show-suggestions-button.png)](../media/improve-knowledge-base/show-suggestions-button.png#lightbox)

1. Bilgi Bankası ile soru ve yanıt çiftleri seçerek yalnızca önerileri göstermek için filtre **önerileri filtreyle**.

    [![Filtreyi önerileri geçiş tarafından etkin olarak öğrenmeye ait önerilen soru seçenekleri görüntülemek için kullanın.](../media/improve-knowledge-base/filter-by-suggestions.png)](../media/improve-knowledge-base/filter-by-suggestions.png#lightbox)

1. Bir onay işareti ile yeni soru alternatifleri her soru-cevap çifti önerir `✔` , soru kabul etmek için veya bir `x` öneriler reddetmek için. Soru eklemek için onay işaretini seçin. 

    [![Etkin öğrenme'nin önerilen soru alternatifleri yeşil onay işareti veya kırmızı Sil işareti'ı seçerek reddetmemize veya seçin.](../media/improve-knowledge-base/accept-active-learning-suggestions.png)](../media/improve-knowledge-base/accept-active-learning-suggestions.png#lightbox)

    Ekleme veya silme _tüm öneriler_ seçerek **tüm eklemek** veya **tümünü Reddet** bağlamsal araç.

1. Seçin **kaydedin ve eğitme** Bilgi Bankası'na değişiklikleri kaydedin.

1. Seçin **Yayımla** listesinden kullanılabilir olması değişikliklere izin vermek üzere [GenerateAnswer API](metadata-generateanswer-usage.md#generateanswer-request-configuration).

    5 veya daha fazla benzer sorguları kümelenir, 30 dakikada bir, soru-cevap Oluşturucu, kabul etme veya reddetme alternatif soruları önerir.


<a name="#score-proximity-between-knowledge-base-questions"></a>

### <a name="architectural-flow-for-using-generateanswer-and-train-apis-from-a-bot"></a>Bir bot GenerateAnswer ve eğitme API'lerini kullanarak mimari akışı

Bir bot veya başka bir istemci uygulama aşağıdaki mimari akış etkin öğrenmeyi kullanma kullanmanız gerekir:

* Bot [Bilgi Bankası'ndaki yanıtı alır](#use-the-top-property-in-the-generateanswer-request-to-get-several-matching-answers) GenerateAnswer API'siyle kullanarak `top` yanıt sayısı alınacağı özellik.
* Bot, açık bir geri bildirim belirler:
    * Kullanarak kendi [özel iş mantığı](#use-the-score-property-along-with-business-logic-to-get-list-of-answers-to-show-user), düşük puanlar filtre.
    * Bot veya istemci uygulaması, kullanıcı için olası yanıtların listesini görüntülemek ve kullanıcının seçili yanıt alın.
* Bot [soru-cevap Oluşturucu seçili yanıtı geri gönderir](#bot-framework-sample-code) ile [eğitme API](#train-api).


### <a name="use-the-top-property-in-the-generateanswer-request-to-get-several-matching-answers"></a>Üst özellik GenerateAnswer istekte birden fazla eşleşen yanıtlar almak için kullanın.

Soru-cevap Oluşturucu soru, yanıt gönderirken `top` JSON gövdesi özelliğini ayarlar döndürülecek yanıt sayısı. 

```json
{
    "question": "wi-fi",
    "isTest": false,
    "top": 3
}
```

### <a name="use-the-score-property-along-with-business-logic-to-get-list-of-answers-to-show-user"></a>Kullanıcıya göstermek için yanıtların listesini almak için iş mantığı ile birlikte puanı özelliğini kullanın

İstemci uygulaması (örneğin, bir sohbet Robotu) yanıtı aldığında, en üst 3 soruyu döndürülür. Kullanım `score` yakınlık puanları arasında analiz etmek için özellik. Bu yakınlık aralığı kendi iş mantığı tarafından belirlenir. 

```json
{
    "answers": [
        {
            "questions": [
                "Wi-Fi Direct Status Indicator"
            ],
            "answer": "**Wi-Fi Direct Status Indicator**\n\nStatus bar icons indicate your current Wi-Fi Direct connection status:  \n\nWhen your device is connected to another device using Wi-Fi Direct, '$  \n\n+ •+ ' Wi-Fi Direct is displayed in the Status bar.",
            "score": 74.21,
            "id": 607,
            "source": "Bugbash KB.pdf",
            "metadata": []
        },
        {
            "questions": [
                "Wi-Fi - Connections"
            ],
            "answer": "**Wi-Fi**\n\nWi-Fi is a term used for certain types of Wireless Local Area Networks (WLAN). Wi-Fi communication requires access to a wireless Access Point (AP).",
            "score": 74.15,
            "id": 599,
            "source": "Bugbash KB.pdf",
            "metadata": []
        },
        {
            "questions": [
                "Turn Wi-Fi On or Off"
            ],
            "answer": "**Turn Wi-Fi On or Off**\n\nTurning Wi-Fi on makes your device able to discover and connect to compatible in-range wireless APs.  \n\n1.  From a Home screen, tap ::: Apps > e Settings .\n2.  Tap Connections > Wi-Fi , and then tap On/Off to turn Wi-Fi on or off.",
            "score": 69.99,
            "id": 600,
            "source": "Bugbash KB.pdf",
            "metadata": []
        }
    ]
}
```

## <a name="client-application-follow-up-when-questions-have-similar-scores"></a>Benzer puanları sorularınız olduğunda istemci uygulaması izleme

İstemci uygulamanızı seçmek bir kullanıcı için bir seçenek ile soruları görüntüler _tek soru_ çoğu kendi amacınıza temsil eder. 

Kullanıcı mevcut sorulardan birini seçtikten sonra istemci uygulama, kullanıcının seçimini soru-cevap Oluşturucu'nın Train API'sini kullanarak geri bildirim gönderir. Bu geri bildirim tamamlandıktan etkin geri bildirim döngüsü öğrenme. 

## <a name="train-api"></a>Train API

Etkin öğrenme geri bildirim için soru-cevap Oluşturucu ile eğitme API POST isteği gönderilir. API imzası verilmiştir:

```http
POST https://<QnA-Maker-resource-name>.azurewebsites.net/qnamaker/knowledgebases/<knowledge-base-ID>/train
Authorization: EndpointKey <endpoint-key>
Content-Type: application/json
{"feedbackRecords": [{"userId": "1","userQuestion": "<question-text>","qnaId": 1}]}
```

|HTTP isteği özelliği|Ad|Tür|Amaç|
|--|--|--|--|
|URL rota parametresi|Bilgi Bankası kimliği|string|Bilgi bankanızı GUİD'i.|
|Ana bilgisayar alt etki alanı|QnAMaker kaynak adı|string|Azure aboneliğinizdeki, soru-cevap Oluşturucu için konak adı. Bilgi Bankası yayımladıktan sonra bu ayarlar sayfasında kullanılabilir. |
|Üstbilgi|İçerik türü|string|API'ye gönderilen gövdenin medya türü. Varsayılan değerdir: `application/json`|
|Üstbilgi|Yetkilendirme|string|Uç nokta anahtarınızı (EndpointKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx).|
|POST gövdesini|JSON nesnesi|JSON|Eğitim geri bildirim|

JSON gövdesi birkaç ayar vardır:

|JSON gövdesi özelliği|Tür|Amaç|
|--|--|--|--|
|`feedbackRecords`|array|Geri bildirim listesi.|
|`userId`|string|Önerilen sorular kabul eden kişinin kullanıcı kimliği. Kullanıcı Kimliği biçimi size bağlıdır. Örneğin, bir e-posta adresi geçerli bir kullanıcı kimliği mimarinizdeki olabilir. İsteğe bağlı.|
|`userQuestion`|string|Kullanıcının sorgu tam metni. Gereklidir.|
|`qnaID`|number|Bulunan bir soru kimliği [GenerateAnswer yanıt](metadata-generateanswer-usage.md#generateanswer-response-properties). |

Örnek JSON gövdesi aşağıdaki gibi görünür:

```json
{
    "feedbackRecords": [
        {
            "userId": "1",
            "userQuestion": "<question-text>",
            "qnaId": 1
        }
    ]
}
```

Başarılı bir yanıt 204 ve JSON yanıt gövdesine durumunu döndürür. 

### <a name="batch-many-feedback-records-into-a-single-call"></a>Tek bir çağrı birçok geri bildirim kayıtları toplu

İstemci tarafı uygulamada, bir robot gibi verileri depolamak sonra tek bir JSON gövdesi içinde çok sayıda kayıt göndermek `feedbackRecords` dizisi. 

Örnek JSON gövdesi aşağıdaki gibi görünür:

```json
{
    "feedbackRecords": [
        {
            "userId": "1",
            "userQuestion": "How do I ...",
            "qnaId": 1
        },
        {
            "userId": "2",
            "userQuestion": "Where is ...",
            "qnaId": 40
        },
        {
            "userId": "3",
            "userQuestion": "When do I ...",
            "qnaId": 33
        }
    ]
}
```



<a name="active-learning-is-saved-in-the-exported-apps-tsv-file"></a>

## <a name="bot-framework-sample-code"></a>Bot framework örnek kod

Kullanıcının sorgu için etkin olarak öğrenmeye kullanılacaksa Train API'sini çağırmak bot framework kodunuzu gerekir. Kod yazmak için iki parçası vardır:

* Sorgu için etkin olarak öğrenmeye kullanılması gerekip gerekmediğini belirleme
* Etkin öğrenme için soru-cevap Oluşturucu'nın Train API sorgu geri gönder

İçinde [Azure Bot örnek](https://aka.ms/activelearningsamplebot), bu etkinliklerin her ikisi de programlanmıştır. 

### <a name="example-c-code-for-train-api-with-bot-framework-4x"></a>Örnek C# kod eğitimi API ile Bot Framework 4.x

Aşağıdaki kod eğitimi API'si ile soru-cevap Oluşturucu bilgilerini yeniden göndermek nasıl gösterir. Bu [tam kod örneğini](https://github.com/microsoft/BotBuilder-Samples/tree/master/experimental/qnamaker-activelearning/csharp_dotnetcore) Github'da kullanılabilir.

```csharp
public class FeedbackRecords
{
    // <summary>
    /// List of feedback records
    /// </summary>
    [JsonProperty("feedbackRecords")]
    public FeedbackRecord[] Records { get; set; }
}

/// <summary>
/// Active learning feedback record
/// </summary>
public class FeedbackRecord
{
    /// <summary>
    /// User id
    /// </summary>
    public string UserId { get; set; }

    /// <summary>
    /// User question
    /// </summary>
    public string UserQuestion { get; set; }

    /// <summary>
    /// QnA Id
    /// </summary>
    public int QnaId { get; set; }
}

/// <summary>
/// Method to call REST-based QnAMaker Train API for Active Learning
/// </summary>
/// <param name="host">Endpoint host of the runtime</param>
/// <param name="FeedbackRecords">Feedback records train API</param>
/// <param name="kbId">Knowledgebase Id</param>
/// <param name="key">Endpoint key</param>
/// <param name="cancellationToken"> Cancellation token</param>
public async static void CallTrain(string host, FeedbackRecords feedbackRecords, string kbId, string key, CancellationToken cancellationToken)
{
    var uri = host + "/knowledgebases/" + kbId + "/train/";

    using (var client = new HttpClient())
    {
        using (var request = new HttpRequestMessage())
        {
            request.Method = HttpMethod.Post;
            request.RequestUri = new Uri(uri);
            request.Content = new StringContent(JsonConvert.SerializeObject(feedbackRecords), Encoding.UTF8, "application/json");
            request.Headers.Add("Authorization", "EndpointKey " + key);

            var response = await client.SendAsync(request, cancellationToken);
            await response.Content.ReadAsStringAsync();
        }
    }
}
```

### <a name="example-nodejs-code-for-train-api-with-bot-framework-4x"></a>Örnek Node.js kodunu Train API ile Bot Framework 4.x 

Aşağıdaki kod eğitimi API'si ile soru-cevap Oluşturucu bilgilerini yeniden göndermek nasıl gösterir. Bu [tam kod örneğini](https://github.com/microsoft/BotBuilder-Samples/blob/master/experimental/qnamaker-activelearning/javascript_nodejs) Github'da kullanılabilir.

```javascript
async callTrain(stepContext){

    var trainResponses = stepContext.values[this.qnaData];
    var currentQuery = stepContext.values[this.currentQuery];

    if(trainResponses.length > 1){
        var reply = stepContext.context.activity.text;
        var qnaResults = trainResponses.filter(r => r.questions[0] == reply);

        if(qnaResults.length > 0){

            stepContext.values[this.qnaData] = qnaResults;

            var feedbackRecords = {
                FeedbackRecords:[
                    {
                        UserId:stepContext.context.activity.id,
                        UserQuestion: currentQuery,
                        QnaId: qnaResults[0].id
                    }
                ]
            };

            // Call Active Learning Train API
            this.activeLearningHelper.callTrain(this.qnaMaker.endpoint.host, feedbackRecords, this.qnaMaker.endpoint.knowledgeBaseId, this.qnaMaker.endpoint.endpointKey);
            
            return await stepContext.next(qnaResults);
        }
        else{

            return await stepContext.endDialog();
        }
    }

    return await stepContext.next(stepContext.result);
}
```

## <a name="active-learning-is-saved-in-the-exported-knowledge-base"></a>Etkin öğrenme dışarı aktarılan Bilgi Bankası'ndaki kaydedilir

Uygulamanız etkin olarak öğrenmeye etkin olan ve uygulamayı dışarı aktarma `SuggestedQuestions` tsv dosyası sütununda etkin olarak öğrenmeye verilerini korur. 

`SuggestedQuestions` Sütundur örtük, bilgileri bir JSON nesnesi `autosuggested`ve açık, `usersuggested` geri bildirim. Bu JSON nesnesinin tek bir kullanıcı tarafından gönderilen soru için örneği `help` olan:

```JSON
[
    {
        "clusterHead": "help",
        "totalAutoSuggestedCount": 1,
        "totalUserSuggestedCount": 0,
        "alternateQuestionList": [
            {
                "question": "help",
                "autoSuggestedCount": 1,
                "userSuggestedCount": 0
            }
        ]
    }
]
```

Bu uygulamayı yeniden içeri aktarın, bilgi toplamak ve öneriler, Bilgi Bankası için önermek etkin olarak öğrenmeye devam eder. 

## <a name="best-practices"></a>En iyi uygulamalar

Etkin öğrenme kullanırken en iyi yöntemler için bkz: [en iyi uygulamalar](../Concepts/best-practices.md#active-learning).

## <a name="next-steps"></a>Sonraki adımlar
 
> [!div class="nextstepaction"]
> [Meta veri GenerateAnswer API ile kullanma](metadata-generateanswer-usage.md)
