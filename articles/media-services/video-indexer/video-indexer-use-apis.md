---
title: Video Indexer API - Azure kullanın
titlesuffix: Azure Media Services
description: Bu makalede, Video Indexer API'sini kullanmaya nasıl başlayacağınız gösterilmektedir.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 03/25/2019
ms.author: juliako
ms.openlocfilehash: 41c665a2a1aec56cc07d5465742d01e41e6adfff
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58443577"
---
# <a name="tutorial-use-the-video-indexer-api"></a>Öğretici: Video Indexer API'sini kullanma

> [!Note]
> Video Indexer V1 API, 1 Ağustos 2018'de kullanım dışı bırakıldı. Artık, Video Indexer v2 API kullanılmalıdır. <br/>Video Indexer v2 API'leri ile geliştirme çalışmaları yapmak için [buradaki](https://api-portal.videoindexer.ai/) yönergelere bakın. 

Video Indexer, Microsoft tarafından sunulan ses ve videoya yönelik çeşitli yapay zeka (AI) teknolojilerini tümleşik tek bir hizmette birleştirerek geliştirme faaliyetlerini kolaylaştırır. API'ler, geliştiricilerin bulut platformunun ölçek, küresel erişim, kullanılabilirlik veya güvenilirliği üzerinde düşünmeye gerek kalmadan Medya yapay zeka teknolojilerini kullanmaya odaklanmasına olanak sağlayacak şekilde tasarlanmıştır. API'yi kullanarak dosyalarınızı karşıya yükleyebilir, ayrıntılı video içgörüleri edinebilir, uygulamanıza eklemek amacıyla içgörü ve yürütücü pencere öğelerinin URL'lerini alabilir ve başka görevleri yerine getirebilirsiniz.

Video Indexer hesabınızı oluştururken ücretsiz bir deneme hesabı (belirli sayıda ücretsiz dizin oluşturma dakikası elde edersiniz) veya ücretli bir seçenek (kota sınırlaması olmaz) arasından seçim yapabilirsiniz. Ücretsiz deneme kullanıldığında Video Indexer, web sitesi kullanıcılarına 600 dakikaya kadar ve API kullanıcılarına ise 2400 dakikaya kadar ücretsiz dizin oluşturma olanağı sunar. Bir Video Indexer hesabı oluşturduğunuz Ücretli seçeneğiyle [Azure aboneliğinizi ve Azure Media Services hesabı için bağlı](connect-to-azure.md). Dizin oluşturma faaliyeti yapılan dakika sayısının yanı sıra Azure Media Services hesabıyla ilgili ücretler için ödeme yaparsınız. 

Bu makalede geliştiricilerin [Video Indexer API’sinden](https://api-portal.videoindexer.ai/) nasıl yararlanabileceği açıklanmaktadır.

## <a name="subscribe-to-the-api"></a>API’ye abone olma

1. [Video Indexer Geliştirici Portalı](https://api-portal.videoindexer.ai/)’nda oturum açın.
    
    ![Oturum aç](./media/video-indexer-use-apis/video-indexer-api01.png)

   > [!Important]
   > * Video Indexer için kaydolurken kullandığınız sağlayıcıyı kullanmanız gerekir.
   > * Kişisel Google ve Microsoft (Outlook/Live) hesapları yalnızca deneme hesapları için kullanılabilir. Azure'a bağlı hesaplar için Azure Active Directory gerekir.
   > * E-posta başına yalnızca bir etkin hesap olabilir. Kullanıcı LinkedIn için user@gmail.com ile oturum açar ve sonra Google için user@gmail.com ile oturum açmaya çalışırsa bu ikinci denemede kullanıcının zaten mevcut olduğunu belirten bir hata sayfası görüntülenir.

2. Abone olun.

    [Ürünler](https://api-portal.videoindexer.ai/products) sekmesini seçin. Ardından Yetkilendirme’yi seçip abone olun. 
    
    ![Kaydolma](./media/video-indexer-use-apis/video-indexer-api02.png)

    > [!NOTE]
    > Yeni kullanıcılar otomatik olarak Yetkilendirme’ye abone edilir.
    
    Abone olduktan sonra aboneliğinizi, birincil ve ikincil anahtarlarınızı görebilirsiniz. Anahtarlar korunmalıdır. Anahtarlar yalnızca sunucu kodunuz tarafından kullanılmalıdır. Bu anahtarlar istemci tarafında (.js, .html vb.) mevcut olmamalıdır.

    ![Kaydolma](./media/video-indexer-use-apis/video-indexer-api03.png)

> [!TIP]
> Video Indexer kullanıcısı, tek bir abonelik anahtarını kullanarak birden çok Video Indexer hesabına bağlanabilir. Daha sonra bu Video Indexer hesaplarını farklı Media Services hesaplarına bağlayabilirsiniz.

## <a name="obtain-access-token-using-the-authorization-api"></a>Yetkilendirme API'sini kullanarak erişim belirtecini alma

Yetkilendirme API'sine abone olduktan sonra erişim belirteçleri alabilirsiniz. Bu erişim belirteçleri, İşlemler API’sinde kimlik doğrulamak için kullanılır. 

İşlemler API'sine yapılan her çağrı, çağrının yetkilendirme kapsamına uyan bir erişim belirteci ile ilişkilendirilmelidir.

- Kullanıcı düzeyi: Kullanıcı düzeyindeki erişim belirteçleri, **kullanıcı** düzeyinde işlem gerçekleştirmenize olanak sağlar. Örnek: ilişkili hesaplar alma.
- Hesap düzeyi: Hesap düzeyindeki erişim belirteçleri, **hesap** veya **video** düzeyinde işlem gerçekleştirmenize olanak sağlar. Örnek: karşıya video yükleme, tüm videoları listeleme, video içgörüsü alma vb.
- Video düzeyi: Video düzeyindeki erişim belirteçleri, belirli bir **video** üzerinde işlem gerçekleştirmenize olanak sağlar. Örnek: video içgörüsü alma, açıklamalı alt yazı indirme, pencere öğesi alma vb. 

**allowEdit=true/false** parametresini belirterek bu belirteçlerin salt okunur mu olacağını yoksa düzenlemeye izin mi verileceğini denetleyebilirsiniz.

Sunucudan sunucuya senaryoların çoğunda büyük bir olasılıkla aynı **hesap** belirtecini kullanırsınız çünkü bu belirteç hem **hesap** hem de **video** işlemlerini kapsar. Ancak, Video Indexer’a istemci tarafından çağrı yapmayı planlıyorsanız (örneğin javascript’ten) istemcilerin tüm hesaba erişmesini engellemek için bir **video** erişim belirteci kullanmanız iyi olur. Aynı nedenden dolayı, istemcinize Video Indexer istemci kodu eklerken (örneğin **İçgörüler Pencere Öğesini Al** veya **Yürütücü Pencere Öğesini Al**’ı kullanma) bir **video**erişim belirteci sağlamanız gerekir.

İşleri kolaylaştırmak için **Yetkilendirme** API’si > **GetAccounts**’u kullanarak ilk önce bir kullanıcı belirteci almaya gerek kalmadan hesaplarınızı alabilirsiniz. Ayrıca, hesapları geçerli belirteçlerle almayı da isteyebilirsiniz. Böylece hesap belirteci almak için yapılacak ek bir çağrıyı atlayabilirsiniz.

Erişim belirteçlerinin süresi 1 saatte dolar. İşlemler API'sini kullanmadan önce erişim belirtecinizin geçerli olduğundan emin olun. Belirtecin süresi dolarsa yeni bir erişim belirteci almak için Yetkilendirme API'sini tekrar çağırın.
 
API ile tümleştirmeye hazırsınız. [Her bir Video Indexer REST API’nin ayrıntılı açıklamasına](https://api-portal.videoindexer.ai/) bakabilirsiniz.

## <a name="account-id"></a>Hesap Kimliği 

Hesap Kimliği parametresi, işleme yönelik tüm API çağrıları için gereklidir. Hesap Kimliği, aşağıdaki yollardan biriyle alınabilen bir GUID'dir:

* Hesap Kimliğini almak için **Video Indexer web sitesini** kullanın:

    1. [Video Indexer](https://www.videoindexer.ai/) web sitesine gidip oturum açın.
    2. **Ayarlar** sayfasına gidin.
    3. Hesap kimliğini kopyalayın.

        ![Hesap Kimliği](./media/video-indexer-use-apis/account-id.png)

* Hesap Kimliğini programlı olarak almak için **Video Indexer Geliştirici Portalı**’nı kullanın.

    [Hesap Al](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Accounts?) API’sini kullanın.
    
    > [!TIP]
    > `generateAccessTokens=true` parametresini tanımlayarak hesaplar için erişim belirteçleri oluşturabilirsiniz.
    
* Hesap Kimliğini hesabınızdaki bir yürütücü sayfasının URL'sinden alın.

    Video izlerken kimlik, `accounts` bölümünden sonra ve `videos` bölümünden önce görünür.

    ```
    https://www.videoindexer.ai/accounts/00000000-f324-4385-b142-f77dacb0a368/videos/d45bf160b5/
    ```

## <a name="recommendations"></a>Öneriler

Bu bölümde, Video Indexer API kullanırken işinize yarayacak bazı öneriler sunulmaktadır.

- Karşıya video yüklemeyi planlıyorsanız dosyayı bir ortak ağ konumuna (OneDrive gibi) yerleştirmeniz önerilir. Videonun bağlantısını alın ve URL'yi karşıya dosya yükleme parametresi olarak kullanın. 

    Video Indexer’a sağlanan URL bir medya dosyasına (ses veya video) yönlendirmelidir. OneDrive tarafından oluşturulan bağlantıların bazıları, dosyayı içeren bir HTML sayfası içindir. URL’yi doğrulamanın kolay bir yolu bir tarayıcıya yapıştırmaktır. Dosya indirilmeye başlarsa URL büyük olasılıkla uygundur. Tarayıcı bir görselleştirme gösteriyorsa URL büyük olasılıkla dosyaya değil bir HTML sayfasına yönlendirmektedir.
    
- Belirtilen video için video içgörüleri alan API’yi çağırdığınızda yanıt içeriği olarak ayrıntılı bir JSON çıktısını alırsınız. [Döndürülen JSON hakkındaki ayrıntıları bu konuda görebilirsiniz](video-indexer-output-json-v2.md).

## <a name="code-sample"></a>Kod örneği

Aşağıdaki C# kod parçacığı, tüm Video Indexer API'lerinin kullanımını bir arada göstermektedir.

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

## <a name="see-also"></a>Ayrıca bkz.

- [Video Indexer’a genel bakış](video-indexer-overview.md)
- [Bölgeler](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)

## <a name="next-steps"></a>Sonraki adımlar

[JSON çıktısının ayrıntılarını inceleyin](video-indexer-output-json-v2.md).

