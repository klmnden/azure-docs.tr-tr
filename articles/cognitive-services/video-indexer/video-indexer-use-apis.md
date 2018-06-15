---
title: Azure Video dizin oluşturucu API kullanın | Microsoft Docs
description: Bu makalede, Video dizin oluşturucu API kullanmaya başlamak gösterilmiştir.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 06/04/2018
ms.author: juliako
ms.openlocfilehash: d378934a0c085910475c366f4bdb538f09efc12b
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35356477"
---
# <a name="use-azure-video-indexer-api"></a>Azure Video dizin oluşturucu API kullanın

Video dizin oluşturucu, ses ve video yapay Intelligence (AI) teknolojiler bir tümleşik hizmet Microsoft tarafından sunulan geliştirme daha basit hale birleştirir. API'ler hakkında ölçek, genel endişelenmeden medya AI teknolojileri kullanan odaklanmasına geliştiricilerine ulaşmak etkinleştir, kullanılabilirliği ve güvenilirliği bulut platformu için tasarlanmıştır. API'ı, dosyalarınızı karşıya yükleme, ayrıntılı video Öngörüler alın, uygulamanızı ve diğer görevleri katıştırmak için Insight ve oyuncu pencere öğeleri URL'lerini almak için kullanabilirsiniz.

> [!Note]
> Ücretsiz deneme bir günlük 100 dosya karşıya yükleme sınırı vardır. Ayrıntılar için döndürülen hata iletisi kontrol edebilirsiniz. Günlük sınır değişebileceğini unutmayın.

Bu makalede nasıl geliştiriciler yararlanabilir gösterilmektedir [Video dizin oluşturucu API](https://api-portal.videoindexer.ai/). Video dizin oluşturucu hizmeti daha ayrıntılı bir genel bakış edinmek için bkz [genel bakış](video-indexer-overview.md) makalesi.

## <a name="subscribe-to-the-api"></a>API için abone olma

1. Oturum Aç.

    Video oluşturucusuyla geliştirmeye başlamak için ilk oturum açma için gereken [Video dizin oluşturucu](https://api-portal.videoindexer.ai/) portal. 
    
    ![Kaydolma](./media/video-indexer-use-apis/video-indexer-api01.png)

    > [!Important]
    > 1. Video dizin oluşturucu için kaydolurken kullandığınız aynı sağlayıcı kullanmanız gerekir.
    > 2. Azure AD etki alanındaki kullanıcıları bir oturum açabilmeniz için önce AAD etki alanı yöneticisi bu etki alanı kaydı etkinleştirmeniz gerekir [burada](https://api-portal.videoindexer.ai/aadadminconsent).
    > 3. Kişisel Google ve Microsoft (outlook/Canlı) hesapları yalnızca deneme hesapları için kullanılabilir. Azure'a bağlı hesapları AAD gerektirir.

2. Abone olun.

    Seçin [ürünleri](https://api-portal.videoindexer.ai/products) sekmesi. Ardından, yetkilendirme seçin ve abone olma. 
    
    ![Kaydolma](./media/video-indexer-use-apis/video-indexer-api02.png)
    
    Abone olduktan sonra aboneliğiniz ve birincil ve ikincil anahtarları görmeye olacaktır. Anahtarları korunmalıdır. Anahtarlar yalnızca sunucu kodunuz tarafından kullanılmalıdır. Bunlar istemci tarafında (.js, .html, vb.) kullanılabilir olmamalıdır.

    ![Kaydolma](./media/video-indexer-use-apis/video-indexer-api03.png)

## <a name="obtain-access-token-using-the-authorization-api"></a>Yetkilendirme API kullanarak erişim belirteci alın

Yetkilendirme API'sine abone sonra erişim belirteçleri elde erişemezsiniz. Bu erişim belirteçleri Operations API'si karşı kimlik doğrulaması için kullanılır. 

Her Operations API çağrısı çağrı yetkilendirme kapsamını eşleşen bir erişim belirteci ile ilişkili olmalıdır.

- Kullanıcı düzeyinde - kullanıcı düzeyinde erişim belirteçleri, üzerinde işlem gerçekleştirebileceğiniz izin **kullanıcı** düzeyi. Örneğin, ilişkili hesapları alın.
- Hesap düzeyi – hesap düzeyinde erişim belirteçleri, üzerinde işlem gerçekleştirebileceğiniz izin **hesap** düzeyi veya **video** düzeyi. Örneğin, video karşıya yükleme, tüm videoları liste, vb. video Öngörüler alın.
- Video düzeyi – video düzeyi erişim belirteçleri, belirli bir işlemleri izin **video**. Örneğin, video Öngörüler alın, resim yazıları indirin, pencere öğeleri, vb. alın. 

Bu belirteçler salt okunur olan ya da belirterek düzenleme izin denetleyebilirsiniz **allowEdit = true/false**.

Çoğu server sunucusu senaryoları için muhtemelen aynı kullanacağınız **hesap** her ikisi de kapsayan bu yana belirteci **hesap** işlemleri ve **video** işlemleri. Ancak, istemci tarafı olun planlıyorsanız (örneğin gelen javascript) çağrıları Video dizin oluşturucu için kullanmak istediğiniz bir **video** tüm hesabına erişim izni alma istemcilerin önlemek için erişim belirteci. Ayrıca neden olan olduğunda VideoIndexer istemci kodu, istemci katıştırma (örneğin, kullanarak **alma Öngörüler pencere öğesi** veya **alma Player pencere öğesi**) sağlamanız gerekir bir **video**erişim belirteci.

İşleri kolaylaştırmak için kullanabileceğiniz **yetkilendirme** API > **GetAccounts** hesaplarınızı kullanıcı sağlanmadan önce belirtecini almak için. Aynı zamanda geçerli belirteçlerini hesaplarıyla almak bir hesap belirteç almak üzere ek bir arama atlamak etkinleştirme isteyebilir.

Erişim belirteçleri, 1 saat sonra süresi dolacak. Operations API'si kullanmadan önce erişim belirteci geçerli olduğundan emin olun. Varsa süresi, yeniden yeni bir erişim belirteci almak için yetkilendirme API çağrısı.
 
API ile tümleştirme başlatmaya hazırsınız demektir. Bul [her Video dizin oluşturucu REST API ayrıntılı açıklaması](http://api-portal.videoindexer.ai/).

## <a name="location"></a>Konum

Tüm işlem API çağrısı yönlendirileceğini ve hesabın oluşturulduğu bölgeyi gösteren bir konum parametresi gerektirir.

Aşağıdaki tabloda açıklanan değerleri uygulayın. **Parametre değerine** ne zaman geçirdiğiniz değeri API'yi kullanarak.

|**Ad**|**Parametre değeri**|**Açıklama**|
|---|---|---|
|Deneme|deneme|Deneme hesapları için kullanılır.|
|Batı ABD|westus2|Azure Batı ABD 2 bölge için kullanılır.|
|Kuzey Avrupa |northeurope|Azure Kuzey Avrupa bölge için kullanılır.|
|Doğu Asya|eastasia|Azure Doğu Asya bölge için kullanılır.|

## <a name="account-id"></a>Hesap Kimliği 

Hesap kimliği parametresi tüm işletimsel API çağrılarında gereklidir. Hesap Kimliği aşağıdaki yollardan biriyle elde edilebilir bir GUID değeridir:

* Hesap Kimliği almak için Video dizin oluşturucu Portalı'nı kullanın:

    1. Oturum [videoindexer](https://www.videoindexer.ai/).
    2. Gözat **ayarları** sayfası.
    3. Hesap Kimliği kopyalayın.

        ![Hesap Kimliği](./media/video-indexer-use-apis/account-id.png)

* Program aracılığıyla hesap kimliği almak için API kullanın

    Kullanım [alma hesapları](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Accounts?) API.
    
    > [!TIP]
    > Tanımlayarak hesapları için erişim belirteçleri üretebilir `generateAccessTokens=true`.
    
* Hesap Kimliği hesabınızda player sayfasının URL'sini alırlar.

    Bir videoyu izleyin, sonra kimliği görünür `accounts` bölüm ve önce `videos` bölümü.

    ```
    https://www.videoindexer.ai/accounts/00000000-f324-4385-b142-f77dacb0a368/videos/d45bf160b5/
    ```

## <a name="recommendations"></a>Öneriler

Bu bölümde, Video dizin oluşturucu API'si kullanılırken bazı öneriler listelenir.

- Bir video karşıya planlıyorsanız, bazı ortak ağ konumlarına (örneğin, OneDrive) dosyayı yerleştirmek için önerilir. Video bağlantısını alma ve karşıya yükleme dosyası param olarak URL'sini sağlayın. 

    Video dizin oluşturucu için sağlanan URL bir ortam (ses veya video) dosyasına işaret etmelidir. OneDrive tarafından oluşturulan bağlantılar dosyasını içeren bir HTML sayfası bazılarıdır. Kolay bir doğrulama URL dosya indirme başlatılırsa bir tarayıcıya – yapıştırın uygulanacak için bu büyük olasılıkla bir iyi URL'dir. Tarayıcı bazı görselleştirme işliyorsa, bu büyük olasılıkla bir bağlantı bir dosya ancak HTML sayfası için değil.
    
- Belirtilen video için video Öngörüler alır API çağırdığınızda, ayrıntılı bir JSON çıkış yanıt içeriği olarak alın. [Bu konudaki döndürülen JSON ayrıntılarını görmek](video-indexer-output-json.md).

## <a name="code-sample"></a>Kod örneği

Aşağıdaki C# kod parçacığını birlikte tüm Video dizin oluşturucu API'lerin kullanımını gösterir.

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

[Çıkış JSON ayrıntılarını inceleyin](video-indexer-output-json.md).

## <a name="see-also"></a>Ayrıca bkz.

[Video dizin oluşturucu genel bakış](video-indexer-overview.md)
