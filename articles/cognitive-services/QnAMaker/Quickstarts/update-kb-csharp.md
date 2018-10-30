---
title: 'Hızlı başlangıç: Bilgi bankası güncelleştirme - REST, C# - Soru-Cevap Oluşturma'
titleSuffix: Azure Cognitive Services
description: Bu REST tabanlı hızlı başlangıçta program aracılığıyla örnek bir Soru-Cevap Oluşturma bilgi bankası (KB) güncelleştirme adımları gösterilmektedir. KB güncelleştirme amacıyla kullandığınız JSON tanımı soru-cevap çiftlerinde ekleme, silme ve değiştirme işlemlerini gerçekleştirmenizi sağlar.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: quickstart
ms.date: 10/19/2018
ms.author: diberry
ms.openlocfilehash: 463fb4bc1cfd6ba7a7fc4df2fcde53bda7767488
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49648250"
---
# <a name="quickstart-update-a-knowledge-base-in-qna-maker-using-c"></a>Hızlı başlangıç: C# kullanarak Soru-Cevap Oluşturma’da bilgi bankası güncelleştirme

Bu hızlı başlangıçta var olan bir Soru-Cevap Oluşturma bilgi bankasını (KB) program aracılığıyla güncelleştirme adımları gösterilmektedir.  Bu JSON kodu yeni veri kaynağı ekleyerek, veri kaynaklarını değiştirerek veya veri kaynaklarını silerek bir KB'yi güncelleştirmenizi sağlar.

Bu API Soru-Cevap Oluşturma portalında düzenleme yapıp **Kaydet ve eğit** düğmesini kullanmakla eşdeğerdir.

Bu hızlı başlangıç şu Soru-Cevap Oluşturma API'lerini çağırır:
* [Update](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600): JSON ile tanımlanan bilgi bankası modeli API isteğinin gövdesinde gönderilir. 
* [İşlem Ayrıntılarını Alma](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/operations_getoperationdetails)

## <a name="prerequisites"></a>Ön koşullar

* En son [**Visual Studio Community sürümü**](https://www.visualstudio.com/downloads/).
* [Soru-Cevap Oluşturma hizmetine](../How-To/set-up-qnamaker-service-azure.md) sahip olmanız gerekir. Anahtarınızı almak için, panonuzda **Kaynak Yönetimi** altında **Anahtarlar** öğesini seçin.
* Soru-Cevap Oluşturma bilgi bankası (KB) kimliği aşağıda gösterildiği gibi URL'nin kbid sorgu dizesi bölümünde bulunur.

    ![Soru-Cevap Oluşturma bilgi bankası kimliği](../media/qnamaker-quickstart-kb/qna-maker-id.png)

Henüz bir bilgi bankanız yoksa, bu hızlı başlangıçta kullanmak için bir örneğini oluşturabilirsiniz: [Yeni bilgi bankası oluşturma](create-new-kb-csharp.md).

[!INCLUDE [Code is available in Azure-Samples Github repo](../../../../includes/cognitive-services-qnamaker-csharp-repo-note.md)]

## <a name="create-knowledge-base-project"></a>Bilgi bankası projesi oluşturma

[!INCLUDE [Create Visual Studio Project](../../../../includes/cognitive-services-qnamaker-quickstart-csharp-create-project.md)] 

## <a name="add-required-dependencies"></a>Gerekli bağımlılıkları ekleme

[!INCLUDE [Add required dependencies to code file](../../../../includes/cognitive-services-qnamaker-quickstart-csharp-required-dependencies.md)] 

## <a name="add-required-constants"></a>Gerekli sabitleri ekleme

[!INCLUDE [Add required constants to code file](../../../../includes/cognitive-services-qnamaker-quickstart-csharp-required-constants.md)] 

## <a name="add-knowledge-base-id"></a>Bilgi bankası kimliği ekleme

[!INCLUDE [Add knowledge base ID as constant](../../../../includes/cognitive-services-qnamaker-quickstart-csharp-kb-id.md)] 

## <a name="add-the-kb-update-definition"></a>KB güncelleştirme tanımını ekleme

Sabitlerden sonra aşağıdaki KB güncelleştirme tanımını ekleyin. Güncelleştirme tanımı üç bölümden oluşur:

* add
* update
* delete

Tüm bölümler API'ye gönderilen tek bir istekte kullanılabilir. 

```csharp
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
        'name' : 'QnA Maker FAQ from quickstart - updated'
    },
    'delete': {
        'ids': [
            0
        ]
    }
}
";
```

## <a name="add-supporting-functions-and-structures"></a>Destekleyici işlevleri ve yapıları ekleme

[!INCLUDE [Add supporting functions and structures](../../../../includes/cognitive-services-qnamaker-quickstart-csharp-support-functions.md)] 

## <a name="add-patch-request-to-update-kb"></a>KB güncelleştirmesi için PATCH isteğini ekleme

Aşağıdaki kod KB içindeki soru-cevap gruplarını güncelleştirmek için Soru-Cevap Oluşturma API'sine bir HTTPS isteği gönderir ve yanıtı alır:

```csharp
async static Task<Response> PatchUpdateKB(string kb, string new_kb)
{
    string uri = host + service + method + kb;
    Console.WriteLine("Calling " + uri + ".");

    using (var client = new HttpClient())
    using (var request = new HttpRequestMessage())
    {
        request.Method = new HttpMethod("PATCH");
        request.RequestUri = new Uri(uri);

        request.Content = new StringContent(new_kb, Encoding.UTF8, "application/json");
        request.Headers.Add("Ocp-Apim-Subscription-Key", key);

        var response = await client.SendAsync(request);
        var responseBody = await response.Content.ReadAsStringAsync();
        return new Response(response.Headers, responseBody);
    }
}
```

## <a name="add-get-request-to-determine-creation-status"></a>Oluşturma durumunu belirlemek için GET isteği ekleme

KB güncelleştirmesi soru-cevap çiftlerini ekler, güncelleştirir ve siler. 

```csharp
async static Task<Response> GetStatus(string operation)
{
    string uri = host + service + operation;
    Console.WriteLine("Calling " + uri + ".");

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
```

Bu API çağrısı, işlem durumunu içeren bir JSON yanıtı döndürür: 

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:22:53Z",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "177e12ff-5d04-4b73-b594-8575f9787963"
}
```

Başarılı veya başarısız bir sonuç alana kadar çağrıyı tekrarlayın: 

```JSON
{
  "operationState": "Succeeded",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:23:08Z",
  "resourceLocation": "/knowledgebases/XXX7892b-10cf-47e2-a3ae-e40683adb714",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "177e12ff-5d04-4b73-b594-8575f9787963"
}
```

## <a name="add-updatekb-method"></a>UpdateKB metodunu ekleme
Aşağıdaki metot, KB'yi güncelleştirir ve durum denetimini tekrarlar. KB oluşturma işlemi zaman alabileceğinden başarılı veya başarısız bir sonuç alana kadar durum denetimi çağrılarını tekrarlamanız gerekir.

```csharp
async static void UpdateKB(string kb, string new_kb)
{
    try
    {
        // Starts the QnA Maker operation to update the knowledge base.
        var response = await PatchUpdateKB(kb, new_kb);

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
    catch(Exception ex)
    {
        // An error occurred while updating the knowledge base. Ensure that
        // you included your QnA Maker subscription key and knowledge base ID
        // where directed in the sample.
        Console.WriteLine("An error occurred while updating the knowledge base." + ex.InnerException);
    }
    finally
    {
        Console.WriteLine("Press any key to continue.");
    }
}
```

## <a name="add-the-updatekb-method-to-main"></a>UpdateKB metodunu Main metoduna ekleme

Main metodunu UpdateKB metodunu çağıracak şekilde değiştirin:

```csharp
static void Main(string[] args)
{
    // Invoke the UpdateKB() method to update a knowledge base, periodically
    // checking the status of the QnA Maker operation until the
    // knowledge base is updated.
    UpdateKB(kb, new_kb);

    // The console waits for a key to be pressed before closing.
    Console.ReadLine();
}
```


## <a name="build-and-run-the-program"></a>Programı derleme ve çalıştırma

Programı derleyin ve çalıştırın. Soru-Cevap Oluşturma API'sine otomatik olarak KB güncelleştirme isteği gönderir ve 30 saniyede bir sonucu yoklar. Her yanıt konsol penceresine yazdırılır.

Bilgi bankanız güncelleştirildikten sonra Soru-Cevap Oluşturma Portalı’nızdaki [Bilgi bankalarım](https://www.qnamaker.ai/Home/MyServices) sayfasından görüntüleyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)