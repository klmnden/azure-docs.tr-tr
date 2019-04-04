---
title: Video Indexer ile videoları karşıya yükleme ve dizinleme
titlesuffix: Azure Media Services
description: Bu konuda, API'lerı kullanarak Video Indexer ile videolarınızı karşıya yükleme ve dizinleme gösterilmektedir.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 03/05/2019
ms.author: juliako
ms.openlocfilehash: e6dead0f08f50b32dd963832824d9166ff2467c0
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58893461"
---
# <a name="upload-and-index-your-videos"></a>Videolarınızı karşıya yükleme ve dizinleme  

Video Indexer API ile videoları karşıya yüklerken aşağıdaki karşıya yükleme seçenekleri vardır: 

* Videonuzu bir URL'den karşıya yükleyin (tercih edilir).
* İstek gövdesinde bir bayt dizisi olarak video dosyasını gönderin,
* Var olan Azure Media Services varlığını sağlayarak kullanan [varlık kimliği](https://docs.microsoft.com/azure/media-services/latest/assets-concept) (yalnızca ücretli hesaplarında desteklenir).

Bu makalede, videolarınızı bir URL’ye dayalı olarak karşıya yüklemek ve dizinlemek için [Karşıya video yükleme](https://api-portal.videoindexer.ai/docs/services/operations/operations/Upload-video?) API’sinin nasıl kullanılacağı açıklanmaktadır. Makaledeki kod örneği, bayt dizisinin nasıl yükleneceğini gösteren, açıklama satırı haline getirilmiş kod içerir. <br/>Makalede ayrıca API’ye ait süreci ve çıktıyı değiştirmek için API’de ayarlayabileceğiniz parametrelerin bazılarından bahsedilmektedir.

Videonuz karşıya yüklendikten sonra Video Indexer, isteğe bağlı olarak kodlar videonun (makalesinde açıklanmıştır). Video Indexer hesabınızı oluştururken ücretsiz bir deneme hesabı (belirli sayıda ücretsiz dizin oluşturma dakikası elde edersiniz) veya ücretli bir seçenek (kota sınırlaması olmaz) arasından seçim yapabilirsiniz. Ücretsiz deneme kullanıldığında Video Indexer, web sitesi kullanıcılarına 600 dakikaya kadar ve API kullanıcılarına ise 2400 dakikaya kadar ücretsiz dizin oluşturma olanağı sunar. Bir Video Indexer hesabı oluşturduğunuz Ücretli seçeneğiyle [Azure aboneliğinizi ve Azure Media Services hesabı için bağlı](connect-to-azure.md). Dizin oluşturma faaliyeti yapılan dakika sayısının yanı sıra Medya Hesabı ile ilgili ücretler için ödeme yaparsınız. 

## <a name="uploading-considerations"></a>Karşıya yükleme konusunda dikkat edilmesi gerekenler

- Videonuzu URL’ye dayalı olarak karşıya yüklerken (tercih edilir) uç noktanın güvenliği TLS 1.2 (veya üzeri) ile sağlanmalıdır
- URL seçeneği ile karşıya yükleme boyutu 30 GB ile sınırlıdır
- Çoğu tarayıcıda URL uzunluğu 2000 karakter ile sınırlıdır
- Bayt dizisi seçeneği ile karşıya yükleme boyutu 2 GB ile sınırlıdır
- Bayt dizisi seçeneği 30 dakika sonra zaman aşımına uğruyor
- `videoURL` parametresinde sağlanan URL kodlanmış olmalıdır
- URL'den dizin olarak da aynı sınırlama sahip varlıklar Media Services dizin oluşturma
- Video Indexer'ı tek bir dosya için 4 saat maksimum süre sınırı vardır

> [!Tip]
> .NET Framework 4.6.2 veya üzeri bir sürümünü kullanmanız önerilir. Eski .NET Framework sürümlerinde varsayılan olarak TLS 1.2 ayarı kullanılmaz.
>
> Eski .NET Framework sürümlerini kullanmanız gerekirse kodunuzda REST API çağrısı öncesine bir satır ekleyin:  <br/> System.Net.ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls | SecurityProtocolType.Tls11 | SecurityProtocolType.Tls12;

## <a name="configurations-and-params"></a>Yapılandırmalar ve parametreler

Bu bölümde, isteğe bağlı parametrelerin bazıları ve ayarlanmalarının ne zaman gerekeceği açıklanmaktadır.

### <a name="externalid"></a>externalID 

Bu parametre, video ile ilişkilendirilecek bir kimlik belirtmenize olanak sağlar. Bu kimlik, dış "Video İçerik Yönetimi" (VCM) sistemiyle tümleştirmede kullanılabilir. Video Indexer portalında bulunan videolar, belirtilen dış kimlik kullanılarak aranabilir.

### <a name="callbackurl"></a>callbackUrl

Müşterinin (bir POST isteği kullanılarak) aşağıdaki olaylar hakkında bilgilendirmek için kullanılan URL:

- Dizin oluşturma durumu değişikliği: 
    - Özellikler:    
    
        |Ad|Açıklama|
        |---|---|
        |id|Video kimliği|
        |durum|Video durumu|  
    - Örnek: https://test.com/notifyme?projectName=MyProject&id=1234abcd&state=Processed
- Videoda tanımlanan kişi:
  - Özellikler
    
      |Ad|Açıklama|
      |---|---|
      |id| Video kimliği|
      |Faceıd|Video dizinde görünür face ID|
      |knownPersonId|Yüz tanıma model içinde benzersiz olan kişinin kimliği|
      |PersonName|Kişinin adı|
        
    - Örnek: https://test.com/notifyme?projectName=MyProject&id=1234abcd&faceid=12&knownPersonId=CCA84350-89B7-4262-861C-3CAC796542A5&personName=Inigo_Montoya 

#### <a name="notes"></a>Notlar

- Video Indexer özgün URL'de sağlanan herhangi bir mevcut parametre döndürür.
- Sağlanan URL kodlanmış olması gerekir.

### <a name="indexingpreset"></a>indexingPreset

Ham veya dış kayıtlar arka plan gürültüsü içeriyorsa bu parametreyi kullanın. Bu parametre, dizinleme işlemini yapılandırmak için kullanılır. Aşağıdaki değerleri belirtebilirsiniz:

- `Default` – Dizin ve ses ve video hem kullanarak Öngörüler ayıklayın
- `AudioOnly` – Dizin ve ses yalnızca (göz ardı video) kullanarak Öngörüler ayıklayın
- `DefaultWithNoiseReduction` – Dizin ve ses akışı gürültü azaltma algoritmalar uygularken ses hem video içgörüleri ayıklayın

Fiyat, seçilen dizinleme seçeneğine bağlıdır.  

### <a name="priority"></a>öncelik

Videolar, önceliklerine göre Video Indexer tarafından dizine eklenir. Kullanım **öncelik** dizin önceliğini belirtmek için parametre. Aşağıdaki değerler geçerlidir: **Düşük**, **Normal** (varsayılan), ve **yüksek**.

**Öncelik** parametresi yalnızca ücretli hesapları için desteklenir.

### <a name="streamingpreset"></a>streamingPreset

Videonuz karşıya yüklendikten sonra Video Indexer, isteğe bağlı olarak videoyu kodlar. Video Indexer, ardından dizinlemeye ve videoyu analiz etmeye geçer. Video Indexer analizi tamamladığında video kimliğini içeren bir bildirim alırsınız.  

[Videoyu karşıya yükleme](https://api-portal.videoindexer.ai/docs/services/operations/operations/Upload-video?) veya [Videoyu Yeniden Dizinleme](https://api-portal.videoindexer.ai/docs/services/operations/operations/Re-index-video?) API'sini kullanırken isteğe bağlı parametrelerden biri de `streamingPreset` parametresidir. `streamingPreset` parametresini `Default`, `SingleBitrate` veya `AdaptiveBitrate` olarak ayarlarsanız kodlama işlemi tetiklenir. Dizinleme ve kodlama işleri tamamlandıktan sonra video yayımlanır. Böylece videonuzun akışını da yapabilirsiniz. Video akışı yapmak istediğiniz Akış Uç Noktası **Çalışıyor** durumunda olmalıdır.

Dizinleme ve kodlama işlerini çalıştırmak için [Video Indexer hesabınıza bağlı Azure Media Services hesabı](connect-to-azure.md) Ayrılmış Birimler gerektirir. Daha fazla bilgi için bkz. [Medya İşlemeyi Ölçeklendirme](https://docs.microsoft.com/azure/media-services/previous/media-services-scale-media-processing-overview). Bunlar işlem gücü kullanımı yoğun işler olduğundan S3 türü birimlerin kullanılması önemle tavsiye edilir. Ayrılmış birim sayısı, paralel olarak çalıştırılabilecek en fazla iş sayısını tanımlar. Önerilen temel kullanım 10 S3 ayrılmış birimdir. 

Videonuzu yalnızca dizinlemek istiyorsanız ve kodlamayacaksanız `streamingPreset` parametresini `NoStreaming` olarak ayarlayın.

### <a name="videourl"></a>videoUrl

Dizinlenecek video/ses dosyasının URL'si. URL bir medya dosyasına yönlendirmelidir (HTML sayfaları desteklenmez). Dosya, URI'nin parçası olarak sunulan bir erişim belirteci tarafından korunabilir ve dosyayı sunan uç noktanın güvenliği TLS 1.2 veya üzeri bir sürümle sağlanmalıdır. URL’nin kodlanması gerekir. 

`videoUrl` belirtilmezse Video Indexer dosyayı çok parçalı/form gövde içeriği olarak geçirmenizi bekler.

## <a name="code-sample"></a>Kod örneği

Aşağıdaki C# kod parçacığı, tüm Video Indexer API'lerinin kullanımını bir arada göstermektedir.

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

    // get the video ID from the upload result
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
## <a name="common-errors"></a>Sık karşılaşılan hatalar

Upload işlemi aşağıdaki tabloda listelenen durum kodlarını döndürebilir.

|Durum kodu|ErrorType (yanıt gövdesinde)|Açıklama|
|---|---|---|
|400|VIDEO_ALREADY_IN_PROGRESS|Bu video zaten aynı hesapta işleniyor.|
|400|VIDEO_ALREADY_FAILED|Bu videonun işlenmesi 2 saatten daha kısa bir süre önce aynı hesapta başarısız oldu. API istemcilerin videoyu yeniden yüklemek için en az 2 saat beklemesi gerekir.|

## <a name="next-steps"></a>Sonraki adımlar

[API tarafından üretilen Azure Video dizinleyici çıktısını İnceleme](video-indexer-output-json-v2.md)
