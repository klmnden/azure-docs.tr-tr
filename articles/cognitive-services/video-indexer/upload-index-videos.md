---
title: Karşıya yükleme ve videolarınızı Azure Video Indexer ile dizin | Microsoft Docs
description: Bu konuda karşıya yüklemek ve videolarınızı Azure Video Indexer ile dizin için API'leri nasıl yapılacağı açıklanır.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 08/17/2018
ms.author: juliako
ms.openlocfilehash: 8a9409c46cac8397bc449c586374729a4d864036
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "41988774"
---
# <a name="upload-and-index-your-videos"></a>Karşıya yükleme ve videolarınızı dizin  

Bu makalede nasıl kullanılacağını gösterir [videoyu karşıya yükle](https://api-portal.videoindexer.ai/docs/services/operations/operations/Upload-video?) karşıya yüklemek ve videolarınızı Azure Video Indexer ile dizin için API. Ayrıca işlem ve API çıktısını değiştirmek için API ayarlayabilirsiniz parametrelerden bazıları açıklanır.

> [!Note]
> Video Indexer hesabınız oluşturulurken (belirli sayıda boş dizin dakika nereden) ücretsiz bir deneme hesabı veya Ücretli bir seçeneğe (burada, kota tarafından sınırlı değildir) seçebilirsiniz. <br/>Ücretsiz deneme ile 2400 dakika sayısı en fazla ücretsiz API kullanıcılara dizin oluşturma ve Video Indexer, ücretsiz Web sitesi kullanıcılara dizin 600 dakika sağlar. <br/>Bir Video Indexer hesabı oluşturduğunuz Ücretli seçeneğiyle [Azure aboneliğinizi ve Azure Media Services hesabına bağlı](connect-to-azure.md). İlgili medya hesabı yanı sıra dizine dakikalar için ödeme ücretleri. 

## <a name="configurations-and-params"></a>Yapılandırmalar ve parametreler

Bu bölümde bazı isteğe bağlı parametrelerinin ve bunları istediğiniz zaman açıklanmaktadır.

### <a name="externalid"></a>externalID = 

Bu parametre, video ile ilişkilendirilecek bir kimliği belirtmenizi sağlar. Dış "Video içerik yönetimi" (VCM) sistem tümleştirme kimliği uygulanabilir. Video Indexer Portalı'nda bulunan videoları belirtilen dış kimlik kullanarak aranabilir.

### <a name="indexingpreset"></a>indexingPreset

Arka plan gürültüsü ham veya dış kayıtları içeriyorsa, bu parametreyi kullanın. Bu parametre, dizin oluşturma işlemini yapılandırmak için kullanılır. Aşağıdaki değerleri belirtebilirsiniz:

- `Default` – Dizin ve ses ve video hem kullanarak Öngörüler ayıklayın
- `AudioOnly` – Dizin ve ses yalnızca (göz ardı video) kullanarak Öngörüler ayıklayın
- `DefaultWithNoiseReduction` – Dizin ve ses akışı gürültü azaltma algoritmalar uygularken ses hem video içgörüleri ayıklayın

Fiyat, seçili dizin seçeneğe bağlıdır.  

### <a name="callbackurl"></a>callbackUrl

Dizin oluşturulurken bildirmek için bir POST URL'si tamamlanır. Video Indexer ekler iki sorgu dizesi parametreleri: kimliği ve durumu. Örneğin, geri çağırma URL'si ise 'https://test.com/notifyme?projectName=MyProject', ek parametreler ile bildirim gönderilir'https://test.com/notifyme?projectName=MyProject&id=1234abcd&state=Processed'.

Video Indexer çağrısı göndermeden önce URL'sine daha fazla parametre ekleyebilir ve bu parametreleri geri aramada dahil edilir. Sorgu dizesini ayrıştırmak ve alma kodunuzda (URL'si artı Video Indexer tarafından sağlanan bilgileri için başlangıçta eklenmiş veriler.) sorgu dizesinde belirtilen parametrelerin tümü daha sonra tekrar 

### <a name="streamingpreset"></a>streamingPreset

Videonuz karşıya yüklendikten sonra Video Indexer, isteğe bağlı olarak kodlar video. Ardından, dizin oluşturma ve video Çözümleme devam eder. Video Indexer bittiğinde çözümleme, video kimliğe sahip bir bildirim alırsınız  

Kullanırken [videoyu karşıya yükle](https://api-portal.videoindexer.ai/docs/services/operations/operations/Upload-video?) veya [yeniden dizin Video](https://api-portal.videoindexer.ai/docs/services/operations/operations/Re-index-video?) API'si, isteğe bağlı parametrelerden biri, `streamingPreset`. Ayarlarsanız `streamingPreset` için `Default`, `SingleBitrate`, veya `AdaptiveBitrate`, kodlama işlemi tetiklenir. Dizin oluşturma ve işleri kodlama tamamladıktan sonra videonuzu akışla, böylece videonun yayımlanır. Videonuzun akışını yapmak istediğiniz akış uç olmalıdır **çalıştıran** durumu.

Dizin oluşturma ve kodlama işleri çalıştırmak için [Azure Media Services hesabına bağlı Video Indexer hesabınız](connect-to-azure.md), ayrılmış birim gerektirir. Daha fazla bilgi için [medya işlemeyi ölçeklendirme](https://docs.microsoft.com/azure/media-services/previous/media-services-scale-media-processing-overview). Bu işlem gücü kullanımlı işleri olduğundan, S3 birimi türü önemle tavsiye edilir. RU sayısı, paralel olarak çalıştırılabilir işler en fazla sayısını tanımlar. 10 S3 RU'ları temel önerilir. 

Yalnızca videonuzu dizinlemeyi istiyoruz, ancak bunu kodlamamayı verilirse `streamingPreset`için `NoStreaming`.

## <a name="code-sample"></a>Kod örneği

Aşağıdaki C# kod parçacığı birlikte tüm Video Indexer API'lerin kullanımını gösterir.

```csharp
public async Task Sample()
{
    var apiUrl = "https://api.videoindexer.ai";
    var location = "westus2";
    var apiKey = "...";

    System.Net.ServicePointManager.SecurityProtocol =
        System.Net.ServicePointManager.SecurityProtocol | System.Net.SecurityProtocolType.Tls12;

    // create the http client
    var handler = new HttpClientHandler();
    handler.AllowAutoRedirect = false;
    var client = new HttpClient(handler);
    client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);

    // obtain account information and access token
    string queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"generateAccessTokens", "true"},
            {"allowEdit", "true"},
        });
    HttpResponseMessage result = await client.GetAsync($"{apiUrl}/auth/trial/Accounts?{queryParams}");
    var json = await result.Content.ReadAsStringAsync();
    var accounts = JsonConvert.DeserializeObject<AccountContractSlim[]>(json);
    // take the relevant account, here we simply take the first
    var accountInfo = accounts.First();

    // we will use the access token from here on, no need for the apim key
    client.DefaultRequestHeaders.Remove("Ocp-Apim-Subscription-Key");

    // upload a video
    var content = new MultipartFormDataContent();
    Debug.WriteLine("Uploading...");
    // get the video from URL
    var videoUrl = "VIDEO_URL"; // replace with the video URL

    // as an alternative to specifying video URL, you can upload a file.
    // remove the videoUrl parameter from the query params below and add the following lines:
    //FileStream video =File.OpenRead(Globals.VIDEOFILE_PATH);
    //byte[] buffer =newbyte[video.Length];
    //video.Read(buffer, 0, buffer.Length);
    //content.Add(newByteArrayContent(buffer));

    queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"accessToken", accountInfo.AccessToken},
            {"name", "video_name"},
            {"description", "video_description"},
            {"privacy", "private"},
            {"partition", "partition"},
            {"videoUrl", videoUrl},
        });
    var uploadRequestResult = await client.PostAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos?{queryParams}", content);
    var uploadResult = await uploadRequestResult.Content.ReadAsStringAsync();

    // get the video id from the upload result
    string videoId = JsonConvert.DeserializeObject<dynamic>(uploadResult)["id"];
    Debug.WriteLine("Uploaded");
    Debug.WriteLine("Video ID:");
    Debug.WriteLine(videoId);

    // wait for the video index to finish
    while (true)
    {
        await Task.Delay(10000);

        queryParams = CreateQueryString(
            new Dictionary<string, string>()
            {
                {"accessToken", accountInfo.AccessToken},
                {"language", "English"},
            });

        var videoGetIndexRequestResult = await client.GetAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/{videoId}/Index?{queryParams}");
        var videoGetIndexResult = await videoGetIndexRequestResult.Content.ReadAsStringAsync();

        string processingState = JsonConvert.DeserializeObject<dynamic>(videoGetIndexResult)["state"];

        Debug.WriteLine("");
        Debug.WriteLine("State:");
        Debug.WriteLine(processingState);

        // job is finished
        if (processingState != "Uploaded" && processingState != "Processing")
        {
            Debug.WriteLine("");
            Debug.WriteLine("Full JSON:");
            Debug.WriteLine(videoGetIndexResult);
            break;
        }
    }

    // search for the video
    queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"accessToken", accountInfo.AccessToken},
            {"id", videoId},
        });

    var searchRequestResult = await client.GetAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/Search?{queryParams}");
    var searchResult = await searchRequestResult.Content.ReadAsStringAsync();
    Debug.WriteLine("");
    Debug.WriteLine("Search:");
    Debug.WriteLine(searchResult);

    // Generate video access token (used for get widget calls)
    client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
    var videoTokenRequestResult = await client.GetAsync($"{apiUrl}/auth/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/{videoId}/AccessToken?allowEdit=true");
    var videoAccessToken = (await videoTokenRequestResult.Content.ReadAsStringAsync()).Replace("\"", "");
    client.DefaultRequestHeaders.Remove("Ocp-Apim-Subscription-Key");

    // get insights widget url
    queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"accessToken", videoAccessToken},
            {"widgetType", "Keywords"},
            {"allowEdit", "true"},
        });
    var insightsWidgetRequestResult = await client.GetAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/{videoId}/InsightsWidget?{queryParams}");
    var insightsWidgetLink = insightsWidgetRequestResult.Headers.Location;
    Debug.WriteLine("Insights Widget url:");
    Debug.WriteLine(insightsWidgetLink);

    // get player widget url
    queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"accessToken", videoAccessToken},
        });
    var playerWidgetRequestResult = await client.GetAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/{videoId}/PlayerWidget?{queryParams}");
    var playerWidgetLink = playerWidgetRequestResult.Headers.Location;
    Debug.WriteLine("");
    Debug.WriteLine("Player Widget url:");
    Debug.WriteLine(playerWidgetLink);
}

private string CreateQueryString(IDictionary<string, string> parameters)
{
    var queryParameters = HttpUtility.ParseQueryString(string.Empty);
    foreach (var parameter in parameters)
    {
        queryParameters[parameter.Key] = parameter.Value;
    }

    return queryParameters.ToString();
}

public class AccountContractSlim
{
    public Guid Id { get; set; }
    public string Name { get; set; }
    public string Location { get; set; }
    public string AccountType { get; set; }
    public string Url { get; set; }
    public string AccessToken { get; set; }
}
```



## <a name="next-steps"></a>Sonraki adımlar

[V2 API'si tarafından üretilen Azure Video dizinleyici çıktısını İnceleme](video-indexer-output-json-v2.md)
