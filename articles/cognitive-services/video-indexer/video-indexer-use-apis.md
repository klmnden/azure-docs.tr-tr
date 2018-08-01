---
title: Azure Video dizinleyici API'sini kullanma | Microsoft Docs
description: Bu makalede, Video Indexer API kullanmaya başlayacağınız gösterilmektedir.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 07/25/2018
ms.author: juliako
ms.openlocfilehash: 82416c7c653438fcd8b8f4a4ead7591bad0ac022
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39391522"
---
# <a name="use-azure-video-indexer-api"></a>Azure Video dizinleyici API'sini kullanma

Video Indexer, tümleşik bir hizmet Microsoft tarafından sunulan çeşitli ses ve video yapay zeka (AI) teknolojileri geliştirmeyi birleştirir. API'ler hakkında ölçek, global endişelenmeden medya AI teknolojilerini kullanan odaklanmasına geliştiricilerine ulaşmak etkinleştir, kullanılabilirlik ve güvenilirlik bulut platformunun için tasarlanmıştır. Dosyalarınızı karşıya yüklemek, ayrıntılı video öngörüleri almak için içgörü ve oynatıcı pencere öğeleri URL'lerini alın. uygulamanız ve diğer görevleri katıştırmak için API'yi kullanabilirsiniz.

Video Indexer hesabınız oluşturulurken (belirli sayıda boş dizin dakika nereden) ücretsiz bir deneme hesabı veya Ücretli bir seçeneğe (burada, kota tarafından sınırlı değildir) seçebilirsiniz. Ücretsiz deneme ile 2400 dakika sayısı en fazla ücretsiz API kullanıcılara dizin oluşturma ve Video Indexer, ücretsiz Web sitesi kullanıcılara dizin 600 dakika sağlar. Bir Video Indexer hesabı oluşturduğunuz Ücretli seçeneğiyle [Azure aboneliğinizi ve Azure Media Services hesabına bağlı](connect-to-azure.md). İlgili medya hesabı yanı sıra dizine dakikalar için ödeme ücretleri. 

Nasıl geliştiriciler yararlanabilir Bu makale [Video Indexer API](https://api-portal.videoindexer.ai/). Video Indexer hizmeti daha ayrıntılı bir genel bakış için bkz [genel bakış](video-indexer-overview.md) makalesi.

## <a name="subscribe-to-the-api"></a>API için abone olun

1. Oturum Aç.

    Video Indexer ile geliştirmeye başlamak için ilk oturum açma için gereken [Video Indexer](https://api-portal.videoindexer.ai/) portalı. 
    
    ![Kaydolma](./media/video-indexer-use-apis/video-indexer-api01.png)

    > [!Important]
    > * Video Indexer için kaydolurken kullandığınız aynı sağlayıcı kullanmanız gerekir.
    > * Google ve Microsoft (outlook/Canlı) kişisel hesaplar yalnızca deneme hesapları için kullanılabilir. Azure'a bağlı hesaplar, Azure AD gerektirir.
    > * E-posta başına yalnızca bir etkin hesap olabilir. Bir kullanıcı ile oturum açmanız çalışırsa user@gmail.com ve sonrası ile LinkedIn için user@gmail.com kullanıcı zaten belirten Google daha sonra bir hata sayfası görüntüler için mevcut.


2. Abone olun.

    Seçin [ürünleri](https://api-portal.videoindexer.ai/products) sekmesi. Ardından, yetkilendirme seçin ve abone olun. 
    
    ![Kaydolma](./media/video-indexer-use-apis/video-indexer-api02.png)

    > [!NOTE]
    > Yeni kullanıcıların otomatik olarak yetkilendirme için abone oldu.
    
    Abone olduktan sonra aboneliğinizi ve, birincil ve ikincil anahtarlarını görmek mümkün olacaktır. Korumalı anahtarlar. Anahtarlar yalnızca sunucu kodunuz tarafından kullanılmalıdır. Bunlar mevcut istemci tarafında (.js, .html, vb.) olmamalıdır.

    ![Kaydolma](./media/video-indexer-use-apis/video-indexer-api03.png)

## <a name="obtain-access-token-using-the-authorization-api"></a>Yetkilendirme API'si kullanılarak erişim belirteci alın

Yetkilendirme API'ye abone sonra erişim belirteçleri almak mümkün olacaktır. Bu erişim belirteçleri, işlemleri API'ye kimlik doğrulaması için kullanılır. 

İşlemleri API'sine yapılan her çağrı çağrı yetkilendirme kapsamı eşleşen bir erişim belirteci ile ilişkili olması gerekir.

- Kullanıcı düzeyinde - kullanıcı düzeyinde erişim belirteçleri üzerinde işlemler gerçekleştirmenize izin **kullanıcı** düzeyi. Örneğin, ilişkili hesapları alın.
- Hesap düzeyi – hesap düzeyinde erişim belirteçleri üzerinde işlemler gerçekleştirmenize izin **hesabı** düzeyi veya **video** düzeyi. Örneğin, karşıya video yükleme, tüm videoları liste, vb. video öngörüleri alın.
- Video düzeyindeki – video düzeyinde erişim belirteçleri izin, belirli bir işlemleri **video**. Örneğin, video öngörüleri almak, açıklamalı alt yazıları indir, vb. pencere öğeleri alın. 

Salt okunur Bu belirteçler, ya da belirterek düzenlemeye izin denetleyebilirsiniz **allowEdit = true/false**.

Sunucudan sunucuya senaryoları için büyük olasılıkla aynı kullanacağınız **hesabı** hem de kapsayan bu yana belirteç **hesabı** işlemleri ve **video** operations. Ancak, istemci tarafı olun planlıyorsanız (örneğin, gelen javascript) çağrıları Video ındexer'a kullanmak istediğiniz bir **video** istemcilerin tüm hesap erişimi engellemek için erişim belirteci. Ayrıca neden olan olduğunda istemcinizde Videoındexer istemci kodu ekleme (örnek olarak, **İçgörüler pencere öğesi alma** veya **oynatıcı pencere öğesi alma**) sağlamanız gerekir bir **video**erişim belirteci.

İşleri kolaylaştırmak için kullanabileceğiniz **yetkilendirme** API > **GetAccounts** hesaplarınızı kullanıcı sağlanmadan önce belirteci alma işlemi. Aynı zamanda geçerli belirteçleriyle hesaplarını almak bir hesap belirteci almak için ek bir arama atlamanızı etkinleştirme sorabilirsiniz.

Erişim belirteci 1 saat sonra sona erer. Erişim belirtecinizi Operations API'si kullanmadan önce geçerli olduğundan emin olun. Varsa süresi, yeniden yeni bir erişim belirteci almak için yetkilendirme API çağrısı.
 
API ile tümleştirmek için hazır olursunuz. Bulma [her Video Indexer REST API ayrıntılı açıklamasını](http://api-portal.videoindexer.ai/).

## <a name="location"></a>Konum

Tüm işlem API çağrısı yönlendirileceğini ve hesabın oluşturulduğu bölgeyi gösteren bir konum parametresi gerektirir.

Aşağıdaki tabloda açıklanan değerlere uygulanır. **Parametre değerine** ne zaman geçirdiğiniz değer API kullanıyor.

|**Ad**|**Parametre değeri**|**Açıklama**|
|---|---|---|
|Deneme|deneme|Deneme hesapları için kullanılır.|
|Batı ABD|westus2|Azure Batı ABD 2 bölgesinde için kullanılır.|
|Kuzey Avrupa |northeurope|Azure Kuzey Avrupa bölgesinde için kullanılır.|
|Doğu Asya|eastasia|Azure Doğu Asya bölgesi kullanılır.|

## <a name="account-id"></a>Hesap Kimliği 

Hesap kimliği parametresi tüm işletimsel API çağrılarında gereklidir. Hesap Kimliği aşağıdaki yollardan biriyle alınabilir bir GUID'dir:

* Video Indexer portal, hesabı Kimliğini almak için kullanın:

    1. Oturum [videoındexer](https://www.videoindexer.ai/).
    2. Gözat **ayarları** sayfası.
    3. Hesap Kimliği kopyalayın.

        ![Hesap Kimliği](./media/video-indexer-use-apis/account-id.png)

* Program aracılığıyla hesap kimliği almak için API'yi kullanın.

    Kullanım [alma hesapları](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Accounts?) API.
    
    > [!TIP]
    > Tanımlayarak hesapları için erişim belirteçleri oluşturabilirsiniz `generateAccessTokens=true`.
    
* Hesap Kimliği hesabınızdaki bir oynatıcı sayfasının URL'sini alın.

    Bir video izleyin, sonra kimliği görünen `accounts` bölümü ve önce `videos` bölümü.

    ```
    https://www.videoindexer.ai/accounts/00000000-f324-4385-b142-f77dacb0a368/videos/d45bf160b5/
    ```

## <a name="recommendations"></a>Öneriler

Bu bölümde, Video Indexer API kullanırken bazı öneriler listelenir.

- Karşıya video yükleme planlıyorsanız, bazı ortak ağ konumlarına (OneDrive gibi) dosya yerleştirmeniz önerilir. Videonun bağlantısını alın ve karşıya yükleme dosyası param URL'sini sağlayın. 

    Video Indexer için sağlanan URL bir medya (ses veya video) dosyası göstermesi gerekir. OneDrive tarafından oluşturulan bağlantıları dosyasını içeren bir HTML sayfası bazılarıdır. Bir kolayca doğrulama URL'sini dosyası indirilmeye başlar, bir tarayıcıya – yapıştırın uygulanacak için bu büyük olasılıkla bir iyi URL'dir. Tarayıcı, bazı görselleştirme işleme, büyük olasılıkla bir dosya ancak bir HTML sayfasına bağlantı değildir.
    
- Video içgörüleri belirtilen videoyu alır API çağırdığınızda, yanıt içeriği olarak ayrıntılı bir JSON çıktısını alın. [Bu konudaki döndürülen JSON ayrıntılarını görmek](video-indexer-output-json.md).

## <a name="code-sample"></a>Kod örneği

Aşağıdaki C# kod parçacığı birlikte tüm Video Indexer API'lerin kullanımını gösterir.

```csharp
var apiUrl = "https://api.videoindexer.ai";
var accountId = "..."; 
var location = "westus2";
var apiKey = "..."; 

System.Net.ServicePointManager.SecurityProtocol = System.Net.ServicePointManager.SecurityProtocol | System.Net.SecurityProtocolType.Tls12;

// create the http client
var handler = new HttpClientHandler(); 
handler.AllowAutoRedirect = false; 
var client = new HttpClient(handler);
client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey); 

// obtain account access token
var accountAccessTokenRequestResult = client.GetAsync($"{apiUrl}/auth/{location}/Accounts/{accountId}/AccessToken?allowEdit=true").Result;
var accountAccessToken = accountAccessTokenRequestResult.Content.ReadAsStringAsync().Result.Replace("\"", "");

client.DefaultRequestHeaders.Remove("Ocp-Apim-Subscription-Key");

// upload a video
var content = new MultipartFormDataContent();
Debug.WriteLine("Uploading...");
// get the video from URL
var videoUrl = "VIDEO_URL"; // replace with the video URL

// as an alternative to specifying video URL, you can upload a file.
// remove the videoUrl parameter from the query string below and add the following lines:
  //FileStream video =File.OpenRead(Globals.VIDEOFILE_PATH);
  //byte[] buffer =newbyte[video.Length];
  //video.Read(buffer, 0, buffer.Length);
  //content.Add(newByteArrayContent(buffer));

var uploadRequestResult = client.PostAsync($"{apiUrl}/{location}/Accounts/{accountId}/Videos?accessToken={accountAccessToken}&name=some_name&description=some_description&privacy=private&partition=some_partition&videoUrl={videoUrl}", content).Result;
var uploadResult = uploadRequestResult.Content.ReadAsStringAsync().Result;

// get the video id from the upload result
var videoId = JsonConvert.DeserializeObject<dynamic>(uploadResult)["id"];
Debug.WriteLine("Uploaded");
Debug.WriteLine("Video ID: " + videoId);           

// obtain video access token            
client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
var videoTokenRequestResult = client.GetAsync($"{apiUrl}/auth/{location}/Accounts/{accountId}/Videos/{videoId}/AccessToken?allowEdit=true").Result;
var videoAccessToken = videoTokenRequestResult.Content.ReadAsStringAsync().Result.Replace("\"", "");

client.DefaultRequestHeaders.Remove("Ocp-Apim-Subscription-Key");

// wait for the video index to finish
while (true)
{
  Thread.Sleep(10000);

  var videoGetIndexRequestResult = client.GetAsync($"{apiUrl}/{location}/Accounts/{accountId}/Videos/{videoId}/Index?accessToken={videoAccessToken}&language=English").Result;
  var videoGetIndexResult = videoGetIndexRequestResult.Content.ReadAsStringAsync().Result;

  var processingState = JsonConvert.DeserializeObject<dynamic>(videoGetIndexResult)["state"];

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
var searchRequestResult = client.GetAsync($"{apiUrl}/{location}/Accounts/{accountId}/Videos/Search?accessToken={accountAccessToken}&id={videoId}").Result;
var searchResult = searchRequestResult.Content.ReadAsStringAsync().Result;
Debug.WriteLine("");
Debug.WriteLine("Search:");
Debug.WriteLine(searchResult);

// get insights widget url
var insightsWidgetRequestResult = client.GetAsync($"{apiUrl}/{location}/Accounts/{accountId}/Videos/{videoId}/InsightsWidget?accessToken={videoAccessToken}&widgetType=Keywords&allowEdit=true").Result;
var insightsWidgetLink = insightsWidgetRequestResult.Headers.Location;
Debug.WriteLine("Insights Widget url:");
Debug.WriteLine(insightsWidgetLink);

// get player widget url
var playerWidgetRequestResult = client.GetAsync($"{apiUrl}/{location}/Accounts/{accountId}/Videos/{videoId}/PlayerWidget?accessToken={videoAccessToken}").Result;
var playerWidgetLink = playerWidgetRequestResult.Headers.Location;
Debug.WriteLine("");
Debug.WriteLine("Player Widget url:");
Debug.WriteLine(playerWidgetLink);

```

## <a name="next-steps"></a>Sonraki adımlar

[Çıkış JSON ayrıntılarını incelemek](video-indexer-output-json.md).

## <a name="see-also"></a>Ayrıca bkz.

[Video Indexer genel bakış](video-indexer-overview.md)
