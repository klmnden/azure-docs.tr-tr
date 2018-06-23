---
title: Görsel arama mobil uygulama Öğreticisi
description: Bilişsel hizmetler bilgisayar görme API, Bing Web arama API ve Xamarin.Forms platformlar arası framework kullanarak visual arama Uygulama kaynağı C# uygulamasını açın.
services: bing-web-search, computer-vision
author: Aristoddle
manager: bking
ms.service: cognitive-services
ms.devlang: csharp
ms.topic: article
ms.date: 06/22/2017
ms.author: t-jolan
ms.openlocfilehash: 54dabaeaad2e49ba7cc138636054bc3dfa4b8379
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352516"
---
# <a name="visual-search-mobile-app-tutorial"></a>Görsel arama mobil uygulama Öğreticisi

## <a name="introduction"></a>Giriş  
Bu öğretici araştırır [bilgisayar görme API](https://azure.microsoft.com/services/cognitive-services/computer-vision/) ve [Bing Web arama API](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/) programlarını nasıl bunlar ile temel görsel arama uygulaması derlemek için kullanılabilir ve uç noktaları [Xamarin.Forms](https://developer.xamarin.com/guides/xamarin-forms/).  Genel olarak, Bu öğretici aşağıdaki konuları içerir: 

> [!div class="checklist"]
> * Xamarin.Forms uygulamaları geliştirmek için Visual Studio'yu ayarlama
> * Kullanarak [Xamarin medya eklentisi](https://github.com/jamesmontemagno/MediaPlugin) yakalamak ve resim verileri içeri aktarmak için
> * Görüntü işleme API'leri kullanarak bir görüntüden metni ayrıştırma
> * Bing Web arama API'sine arama istekleri gönderirken
> * Her iki API'leri kullanılarak JSON yanıtlarının ayrıştırma [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) (LINQ ve model nesnesi seri durumdan çıkarma)
> * Görsel arama için platformlar arası Xamarin.Forms kullanıcı arabirimi oluşturma 

## <a name="prerequisites"></a>Önkoşullar

Bu örnek ile Xamarin.Forms kullanılarak geliştirildi [Visual Studio 2017](https://www.visualstudio.com/downloads/). Community Edition, Visual Studio 2017 kullanılabilir. Xamarin ile Visual Studio kullanma hakkında daha fazla bilgi için bkz: [Xamarin belgelerine](https://developer.xamarin.com/guides/cross-platform/getting_started/).

Bu örnek aşağıdaki NuGet paketlerini kullanır:

> [!div class="checklist"]
> * [Xamarin medya eklentisi](https://github.com/jamesmontemagno/MediaPlugin)
> * [Json.NET](https://github.com/JamesNK/Newtonsoft.Json)

Uygulama aşağıdaki Bilişsel hizmetler API'leri kullanır:

> [!div class="checklist"]
> * [Bing Web araması API](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/) 
> * [Görüntü İşleme API’si](https://azure.microsoft.com/services/cognitive-services/computer-vision/)

Bu API için 30 günlük deneme anahtarlarından edinmek için bkz: [deneyin Bilişsel Hizmetler](https://azure.microsoft.com/try/cognitive-services/). Ticari kullanım için anahtarları edinme hakkında daha fazla bilgi için bkz: [fiyatlandırma](https://azure.microsoft.com/pricing/calculator/).

## <a name="setup-for-development"></a>Geliştirme için Kurulum  

### <a name="installing-xamarin-on-windows"></a>Windows'ta Xamarin yükleme
Visual Studio yükleyicisi sahip herhangi bir sürüm yüklü Visual Studio 2017 birini açın, Visual Studio yüklemenizin ile ilişkili hamburger menüsünü seçin ve **Değiştir**.  

![Visual Studio yükleyicisi](./media/computer-vision-web-search-tutorial/visual-studio-installer.png) 

Mobil ve oyun ve etkinleştir kaydırın **mobil geliştirme .NET ile**, zaten etkin değil.

![Seçili Xamarin.Forms ile Visual Studio yükleyicisi](./media/computer-vision-web-search-tutorial/xamarin-forms-is-enabled.png)

Son olarak, tıklatın **Değiştir** sayfanın sağ üst köşesinde penceresi ve yüklemek Xamarin tamamlanmasını bekleyin.

### <a name="installing-xamarin-on-macos"></a>Xamarin macOS üzerinde yükleme
Mac için Xamarin ile Visual Studio önceden paketlenmiş gelir Yükleme yapılmasına gerek.

## <a name="building-and-running-the-app"></a>Oluşturma ve uygulama çalıştırma

Visual Studio çözüm dosyasını *(.sln)* Visual Ara uygulama yüklenebilir [Bilişsel hizmetler Visual arama uygulamayla](https://azure.microsoft.com/resources/samples/cognitive-services-xamarin-forms-computer-vision-search/). Bir Web tarayıcısı kullanarak ZIP arşivini indirin, Github'dan istasyonunuza kopyalamak veya Visual Studio kullanarak yükleyebilirsiniz.

Örnek ile çalışmaya başlamak için açık `VisualSearchApp.sln` Visual Studio.  Gerekli bileşenleri başlatılırken biraz zaman alabilir.

Uygulama için iki üçüncü taraf kitaplıklar gereklidir: **Json.NET** ve **Xamarin medya eklentisi**. Visual Studio'da NuGet Paket Yöneticisi ile bu kitaplıkları sağ yükleyebilirsiniz. Seçin **Araçlar > NuGet Paket Yöneticisi > NuGet paketleri çözümleri yönetmek**, Çözüm Gezgini'ndeki çözüme sağ tıklayın ve seçin **NuGet paketlerini Yönet** ve bağlam menüsünden.

NuGet penceresinde arayın ve yükleyin **Xamarin medya eklentisi** (Xam.Plugin.Media) sürüm 2.6.2 ve **Json.NET** (Newtonsoft.Json) 10.0.3. Tüm projeleri yüklerken seçtiğinizden emin olun.

Kullanılabilir tüm platformlar için uygulama oluşturmak için basın **Ctrl + Shift + B**, veya **yapı** Şerit menüsünde ve seçin **Çözümü Derle**.  Derleyin ve iOS Windows sisteminden için kodu test etmek için bkz: [bu kılavuzda](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/) Yardım.

Uygulamayı çalıştırmadan önce hedef yapılandırma, platforma ve proje seçmeniz gerekir.  Xamarin.Forms uygulamaları, Windows, Android ve iOS için yerel kodu derleme. Bu öğretici ekran görüntüleri örnek Windows sürümünü içerir, ancak tüm sürümleri işlevsel olarak eşdeğerdir. Bu kılavuzda kullanılan dağıtım ayarları burada gösterilir.  

![Visual Studio için Android telefonla derlemek için yapılandırılmış](./media/computer-vision-web-search-tutorial/configuration-selection.png)

Derleme işleminin başarılı olduğunu ve hedef platformu seçtikten sonra tıklayın **Başlat** basın ve araç çubuğu düğmesini **F5**. Visual Studio çözümünüzü hedef platformunuzu dağıtır ve başlatılır.

Anahtarları Ekle sayfası görüntülenir. (Bu sayfa tanımlanan `AddKeysPage.xaml`.)  

![Sayfa Ekle görüntüsü tuşları](./media/computer-vision-web-search-tutorial/add-keys-page.png)  

Burada, bilgisayar görme ve Bing Web arama API anahtarlarınızı yapıştırın. Bilgisayar görme API uç noktası barındırıldığı sunucu seçin gerektirir.

> [!TIP]
> Bu sayfayı atlamak için uygun konumlarda bu bilgileri girin. `App.xaml.cs`. 

Bir test sorgusu gerçekleştirerek anahtarlarınızı uygulama doğrular, alır, OCR seçin sayfasında (tanımlanan `OcrSelectPage.xaml`).
   
![OCR Seç sayfasının görüntüsü](./media/computer-vision-web-search-tutorial/ocr-select-page.png)  

Bu ekran üstünde tanımak için istediğiniz metin yazdırılan el yazısıyla yazılmış olup seçebilirsiniz.

> [!TIP]
> Metin olarak düzeyi algılar ve eşit olarak hiçbir yansıma aydınlatma emin olmak çalıştığınız öğeyi basılı tutun. Biz, el yazısı OCR bazen daha iyi betik yazı tiplerini veya diğer "süslü" metin çalıştığını buldunuz.

Bundan sonra öğesini **Al fotoğraf** veya **Al fotoğraf** aygıtınızın kamera kullanarak bir fotoğraf alın veya Cihazınızda depolanan bir fotoğraf seçin.

Fotoğraf gerçekleştirilecek ya da seçilen sonra görüntüyü bilgisayar görme API'sine geçirilir. Sözcük bulundu sayfa (tanımlanan `OcrResultsPage.xaml`) görüntüde tanınan herhangi bir sözcük görüntüler.

![OCR sonuçları sayfasının görüntüsü](./media/computer-vision-web-search-tutorial/ocr-results-page.png)  

> [!NOTE]
> Bu sonuçları için kullandık kaynak deposunda görüntüdür `SamplePhotos\TestImage.jpg`.

Sözcük bulundu üzerindeki bir öğeyi tıklattığında sayfası, Web sonuçlar sayfası (`WebResultsPage.xaml`), Bing arama için sonuçları üst gösteren görünür.

![Web sonuçları sayfasının görüntüsü](./media/computer-vision-web-search-tutorial/web-results-page.png)  
Son olarak, katıştırılmış bir Web görünümü içinde bağlantıyı açmak için Web sonuçları sayfasında bir öğe seçin. (Veya geri farklı bir sonuç seçin gidin.)

![Web görünümü sayfasının görüntüsü](./media/computer-vision-web-search-tutorial/web-view-page.png)  

Herhangi bir Web tarayıcısında misiniz veya Web sonuçları sayfasına gidin sayfa ile etkileşim kurabilirsiniz. 

## <a name="review-and-learn"></a>Gözden geçirin ve öğrenin

Bir döngü için görsel arama uygulaması ayırdıktan, tam olarak nasıl şu iki Microsoft Bilişsel hizmetler API'leri kullanmakta olduğunuz inceleyelim.  Bu örnek bir başlangıç noktası olarak kendi uygulamanız için veya yalnızca bir nasıl yapılır olarak API'leri için kullanmakta olduğunuz olup olmadığını, uygulama ekran tam olarak nasıl çalıştığını incelemek için ekran yürütmek için faydalıdır.

### <a name="add-keys-page"></a>Anahtarları Sayfası Ekle
UI tanımlanmış anahtarları Ekle sayfası `AddKeysPage.xaml`, ve, birincil mantığını tanımlanan `AddKeysPage.xaml.cs`. Anahtarları bir test isteğinde doğrulandığından Bilişsel hizmetler uç noktaları C# ile kullanmak nasıl kurmak hazır bir saattir.  

Bu etkileşimi temel yapısını şöyledir: 

1. Örneği *httpresponsemessage öğesini* ve *HttpClient* nesnelerin *System.Net.Http*
2. (Her uç noktanın API başvuru tanımlanan) tüm istenen üstbilgileri ekleme *HttpClient* nesnesi
3. Uç noktanız için URI gerekli bir parametre eklemek, verilerle bir GET veya POST isteği gönder
4. Yanıt başarılı olduğunu doğrulayın
5. Daha ayrıntılı işleme için yanıt geçirin

Bing arama API anahtarını geçerliliğini denetler işlevi `CheckBingSearchKey()`, burada gösterilen.

```csharp
async Task CheckBingSearchKey(object sender = null, EventArgs e = null)
{
    HttpResponseMessage response;
    HttpClient SearchApiClient = new HttpClient();

    SearchApiClient.DefaultRequestHeaders.Add(AppConstants.OcpApimSubscriptionKey, BingSearchKeyEntry.Text);

    try
    {
        response = await SearchApiClient.GetAsync(AppConstants.BingWebSearchApiUrl + "q=test");

        if (response.StatusCode != System.Net.HttpStatusCode.Unauthorized)
        {
            BingSearchKeyEntry.BackgroundColor = Color.Green;
            AppConstants.BingWebSearchApiKey = BingSearchKeyEntry.Text;
            bingSearchKeyWorks = true;
        }
        else
        {
            BingSearchKeyEntry.BackgroundColor = Color.Red;
            bingSearchKeyWorks = false;
        }
    }
    catch( Exception exception )
    {
        BingSearchKeyEntry.BackgroundColor = Color.Red;
        Console.WriteLine($"ERROR: {exception.Message}");
    }
}
```

Benzer bir işlevi `CheckComputerVisionKey()`, bilgisayar görme anahtar doğrulamak için kullanılır.

### <a name="ocr-select-page"></a>OCR Seç sayfası

OCR seçin sayfası (`OcrSelectPage.xaml`) iki önemli rol yok. İlk olarak, ne tür bir OCR hedef fotoğrafa gerçekleştirilir belirler. İkinci olarak, yakalama ya da işlemek için istedikleri görüntüyü açmak kullanıcı olanak sağlar.

Her platform farklı kod gerektirdiği için ikinci bir platformlar arası uygulamasında geleneksel sıkıcı bir görevdir. Xamarin medya eklentisi, yalnızca birkaç kod satırıyla, işleme.

Aşağıdaki işlevi fotoğraf yakalama için Xamarin medya eklentisi kullanma örneği içerir.  İçindeki, biz:

1. Geçerli aygıtta bir kamera kullanılabilir olduğundan emin olun
2. Örneği bir `StoreCameraMediaOptions` nesne yakalanan görüntüyü kaydedileceği yeri belirtin
3. Yansıma yakalama ve elde bir `MediaFile` görüntü veri içeren bir nesne
4. Paketini açma `MediaFile` bir bayt dizisi halinde
5. Başka bir işleme için bayt dizisi döndürür

Burada `TakePhoto()`, fotoğraf yakalama için Xamarin medya eklenti kullanan bir işlev.  

```csharp
async Task<byte[]> TakePhoto()
{
    MediaFile photoMediaFile = null;
    byte[] photoByteArray = null;

    if (CrossMedia.Current.IsCameraAvailable)
    {
        var mediaOptions = new StoreCameraMediaOptions
        {
            PhotoSize = PhotoSize.Medium,
            AllowCropping = true,
            SaveToAlbum = true,
            Name = $"{DateTime.UtcNow}.jpg"
        };
        photoMediaFile = await CrossMedia.Current.TakePhotoAsync(mediaOptions);
        photoByteArray = MediaFileToByteArray(photoMediaFile);
    }
    else
    {
        await DisplayAlert("Error", "No camera found", "OK");
        Console.WriteLine($"ERROR: No camera found");
    }
    return photoByteArray;
}
```
Fotoğraf yardımcı programı works benzer bir şekilde içeri aktarın. Her iki parça işlevlerin bulunabilir `OcrSelectPage.xaml.cs`. 
 
> [!NOTE]
> El yazısıyla yazılmış OCR uç nokta yalnızca 4 MB'den küçük fotoğraf işleyebilir. Dosya boyutu sorunlarından kaçınmak için biz fotoğrafı özgün boyutunun % 50'ye seçeneğini ayarlayarak azaltmak `PhotoSize = PhotoSize.Medium` üzerinde `StoreCameraMediaOptions` nesnesi.  Cihazınızı olağanüstü yüksek kaliteli fotoğraf sürüyorsa ve hatalarla karşılaşıyor deneyebilecekleriniz `PhotoSize = PhotoSize.Small` yerine.

Dönüştürmek için kullanılan yardımcı program işlevi İşte bir *MediaFile* içine bir bayt dizisi.

```csharp
byte[] MediaFileToByteArray(MediaFile photoMediaFile)
{
    using (var memStream = new MemoryStream())
    {
        photoMediaFile.GetStream().CopyTo(memStream);
        return memStream.ToArray();
    }
}
```

### <a name="ocr-results-page"></a>OCR sonuçlar sayfası

Görüntü kullanımından ayıklanan metin OCR sonuçları sayfasını görüntüler **Json.NET** [SelectToken yöntemi](http://www.newtonsoft.com/json/help/html/SelectToken.htm).  Her ikisi de tartışmak için yararlı olacak şekilde iki OCR uç nokta biraz farklı çalışır.  

Bilgisayar görme API yalnızca birkaç sunucu konumlarda barındırıldığından, kendi uç nokta çalışma zamanında dinamik olarak oluşturulması gerekir. İşlev `SetOcrLocation` (statik parçası *AppConstants* sınıfı bulunan `AppConstants.cs`) URI endpoint konumunu ayarlar. Kullanıcının seçimini Ekle anahtarları sayfasında karşılık gelen veya ayarlanan değeri kullanır `App.xaml.cs`. 

```csharp
public static void SetOcrLocation(string location)
{
    ComputerVisionApiOcrUrl = $"https://{location}.api.cognitive.microsoft.com/vision/v1.0/ocr?language=en&detectOrientation=true";
    ComputerVisionApiHandwritingUrl = $"https://{location}.api.cognitive.microsoft.com/vision/v1.0/recognizeText?handwriting=true";
}
```

Bu API istekleri daha yakın bir göz atalım. Bilgisayar görme OCR API belirlenmemiş bir dil metinden ayrıştırma yeteneğine sahiptir, ancak biz sonuçlarını iyileştirmek İngilizce metin arama söyleyin. Biz de ABD metin yönlendirmesini algılamak API olanak tanır. Sabit yönlendirme kullanarak ayrıştırma sonuçları iyileştirebilir ancak yönlendirmesini algılama bir mobil uygulama yararlı olabilir.

Bu uç noktasından ile kullanılabilen parametreleri hakkında daha fazla bilgiyi [yazdırma optik karakter tanıma API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fc).  

El yazısıyla yazılmış OCR hala önizlemede ve şu anda yalnızca İngilizce metin ile çalışır API'dir.  Bu nedenle, yalnızca şu anda hiç el yazısı metni ayrıştırılamıyor gerekip gerekmediğini belirten bir bayrak parametresidir. El yazısı OCR API her ikisi de ayrıştıramıyor makine yazdırılan ve el yazısı metin, ancak `handwriting=false` veriyor daha iyi sonuçlar yazdırılan metin. 

Bu uygulama İngilizce olduğundan, biz varsa yalnızca el yazısıyla yazılmış OCR Bu örnek için kullanılan ve ayarlamak `handwriting` kullanıcı bize tanımak için gerekenleri bildirir göre bayrağı.  Her iki uç noktaları tanımlayıcı amaçlarla kullandık.  

Bu uç noktasından ile kullanılabilen parametreleri hakkında daha fazla bilgiyi [el yazısıyla yazılmış optik karakter tanıma API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/587f2c6a154055056008f200).

Şimdi, bu API çağrısının işlevler bakalım.

`FetchPrintedWordList()` Yazdırılan metin görüntülerden ayrıştırmak için yazdırma OCR uç nokta kullanır. HTTP isteği eklemek anahtarları sayfasında anahtar doğrulamak için kullanılan çağrısı benzer bir yapıyı takip eder, ancak biz fotoğraf göndermek gerekir. Fotoğraf bir HTTP POST isteği GET isteğini yerine kullanırız gerekir böylece bir sorgu dizesi olarak geçirmek için çok büyük. (Bir bayt dizisi olarak bellekte var olan) bizim fotoğraf kodlanacak içine ihtiyacımız bir `ByteArrayContent` nesne ve verileri gönderme türünü tanımlayan bu nesne için bir üstbilgi ekleyin. 

Kabul edilebilir içerik türler hakkında bilgi edinebilirsiniz [bilgisayar görme API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/587f2c6a154055056008f200).  

```csharp
async Task<ObservableCollection<string>> FetchPrintedWordList()
{
    ObservableCollection<string> wordList = new ObservableCollection<string>();
    if (photo != null)
    {
        HttpResponseMessage response = null;
        using (var content = new ByteArrayContent(photo))
        {
            // The media type of the body sent to the API. 
            // "application/octet-stream" defines an image represented 
            // as a byte array
            content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
            response = await visionApiClient.PostAsync(AppConstants.ComputerVisionApiOcrUrl, content);
        }

        string ResponseString = await response.Content.ReadAsStringAsync();
        JObject json = JObject.Parse(ResponseString);

        // Here, we pull down each "line" of text and then join it to 
        // make a string representing the entirety of each line.  
        // In the Handwritten endpoint, you are able to extract the 
        // "line" without any further processing.  If you would like 
        // to simply get a list of all extracted words,* you can do 
        // this with:
        // json.SelectTokens("$.regions[*].lines[*].words[*].text) 
        IEnumerable<JToken> lines = json.SelectTokens("$.regions[*].lines[*]");
        if (lines != null)
        {
            foreach (JToken line in lines)
            {
                IEnumerable<JToken> words = line.SelectTokens("$.words[*].text");
                if (words != null)
                {
                    wordList.Add(string.Join(" ", words.Select(x => x.ToString())));
                }
            }
        }
    }
    return wordList;
}
```
> [!TIP]
> Biz nasıl kullanıyorsanız Not **Json.NET** [SelectToken yöntemi](http://www.newtonsoft.com/json/help/html/SelectToken.htm) metin yanıt nesnesinden ayıklamak için. `SelectToken` Biz ardından sonraki işlev geçirebilirsiniz JSON yanıt belirli bir kısmını ihtiyacımız olduğunda idealdir. Kod diğer bölümleri biz JSON yanıtları tanımlanan model nesneleri içine seri durumdan `ModelObjects.cs`.

Yazdırma OCR el yazısıyla yazılmış OCR isteği arasındaki birincil fark el yazısıyla yazılmış OCR hizmet bilgilerini almak için ek bir istek gerektirse yazdırma OCR hizmet tanınan metni HTTP yanıtı bir parçası olarak döndürmesidir. İlk el yazısıyla yazılmış OCR isteği işleme başladığını yalnızca sinyalleri bir HTTP 202 durumu döndürür. Bu yanıt, istemci kullanılabilir olduğunda tamamlanan yanıt almak için denetlemelisiniz bir uç nokta içerir. Bkz: `FetchHandwrittenWordList()` tüm nasıl çalıştığını görmek için.

```csharp
async Task<ObservableCollection<string>> FetchHandwrittenWordList()
{
    ObservableCollection<string> wordList = new ObservableCollection<string>();
    if (photo != null)
    {
        // Make the POST request to the handwriting recognition URL
        HttpResponseMessage response = null;
        using (var content = new ByteArrayContent(photo))
        {
            // The media type of the body sent to the API. 
            // "application/octet-stream" defines an image represented 
            // as a byte array
            content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
            response = await visionApiClient.PostAsync(AppConstants.ComputerVisionApiHandwritingUrl, content);
        }

        // Fetch results
        IEnumerable<string> operationLocationValues;
        string statusUri = string.Empty;
        if (response.Headers.TryGetValues("Operation-Location", out operationLocationValues))
        {
            statusUri = operationLocationValues.FirstOrDefault();

            // Ping status URL, wait for processing to finish 
            JObject obj = await FetchResultFromStatusUri(statusUri);
            IEnumerable<JToken> strings = obj.SelectTokens("$.recognitionResult.lines[*].text");
            foreach (string s in strings)
            {
                wordList.Add((string)s);
            }
        }
    }
    return wordList;
}
```

`FetchResultFromStatusUri()` İkinci el yazısı OCR işleminin parçasıdır. İlk yanıtın meta verilerini ayıklanan URI ping ya da sonuçta elde edilen veya işlev zaman aşımına uğruyor kadar.  Bu işlev, kendi iş parçacığında zaman uyumsuz olarak çağrılır önemlidir. Aksi takdirde işlem tamamlanana kadar kesilmiştir bu yöntem kullanıcı arabirimini oluşturan kilitleyin.

```csharp
// Takes in the url to check for handwritten text parsing results, and pings it per second until processing is finished
// Returns the JObject holding data for a successful parse
async Task<JObject> FetchResultFromStatusUri(string statusUri)
{
    JObject obj = null;
    int timeoutcounter = 0;
    HttpResponseMessage response = await visionApiClient.GetAsync(statusUri);
    string responseString = await response.Content.ReadAsStringAsync();
    obj = JObject.Parse(responseString);
    while ((!((string)obj.SelectToken("status")).Equals("Succeeded")) && (timeoutcounter++ < 60))
    {
        await Task.Delay(1000);
        response = await visionApiClient.GetAsync(statusUri);
        responseString = await response.Content.ReadAsStringAsync();
        obj = JObject.Parse(responseString);
    } 
    return obj;
}
```

### <a name="web-results-page"></a>Web sonuçlar sayfası
Kullanıcı OCR sonuçları sayfada görüntülenen bir arama terimi seçtikten sonra biz Web sonuçları sayfasına gitmek.  Burada size oluşturmak bir [Bing Web arama API](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/) isteği, hizmetin uç noktasına göndermek ve yanıt kullanarak JSON seri durumdan **Json.NET** [DeserializeObject](http://www.newtonsoft.com/json/help/html/DeserializeObject.htm) yöntemi.  

```csharp
async Task<WebResultsList> GetQueryResults()
{
    // URL-encode the query term
    var queryString = System.Net.WebUtility.UrlEncode(queryTerm);

    // Here we encode the URL that will be used for the GET request to 
    // find query results.  Its arguments are as follows:
    // 
    // - [count=20] This sets the number of webpage objects returned at 
    //   "$.webpages" in the JSON response.  Currently, the API asks for 
    //   20 webpages in the response
    //
    // - [mkt=en-US] This sets the market where the results come from.
    //   Currently, the API looks for english results based in the 
    //   United States.
    //
    // - [q=queryString] This sets the string queried using the Search 
    //   API.   
    //
    // - [responseFilter=Webpages] This filters the response to only 
    //   include Webpage results.  This tag can take a comma seperated 
    //   list of response types that you are looking for.  If left 
    //   blank, all responses (webPages, News, Videos, etc) are 
    //   returned.
    //
    // - [setLang=en] This sets the languge for user interface strings. 
    //   To learn more about UI strings, check the Web Search API 
    //   reference.
    //
    // - [API Reference] https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference
    string uri = AppConstants.BingWebSearchApiUrl + $"count=20&mkt=en-US&q={queryString}&responseFilter=Webpages&setLang=en";

    // Make the HTTP Request
    WebResultsList webResults = null;
    HttpResponseMessage httpResponseMessage = await searchApiClient.GetAsync(uri);
    var responseContentString = await httpResponseMessage.Content.ReadAsStringAsync();
    JObject json = JObject.Parse(responseContentString);
    JToken resultBlock = json.SelectToken("$.webPages");
    if (resultBlock != null)
    {
        webResults = JsonConvert.DeserializeObject<WebResultsList>(resultBlock.ToString());
    }
    return webResults;
}
```

Kullanıcının istiyor hakkında mümkün olduğu kadar bilgi sağladığınızda Bing Web arama API en iyi şekilde çalışır. Özellikle, neredeyse her zaman kullanmalısınız `mkt` ve `setLang` (burada ABD İngilizcesi için ayarlanan) parametre, kullanıcının konumu ve tercih edilen dili tanımlamak için.

> [!NOTE]
> Biz bizim Bing Web arama sorgusu kaynak kodunu anlaşılması kolay emin olmak için basit tutulur.  Gerçek bir uygulamada Gelişmiş sonuçlar için HTTP isteği için aşağıdaki üst bilgiler eklemeniz gerekir. 
> * Kullanıcı Aracısı  
> * X MSEdge ClientID  
> * X arama ClientIP  
> * X arama konumu  
>
> Bu üstbilgi değerleri hakkında daha fazla bilgiyi [Bing Web arama API Başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#headers)

## <a name="next-steps"></a>Sonraki Adımlar
Microsoft Bilişsel hizmetler bu uygulamaya kolayca tümleştirmek birçok ek hizmetler sağlar.  Örneğin, olabilir:

* Ekleme [Bing varlık arama](https://azure.microsoft.com/services/cognitive-services/bing-entity-search-api/) web arama sonuçlarınızı genişletmek için
* İçinde takas [Bing özel arama](https://azure.microsoft.com/services/cognitive-services/bing-custom-search/) Bing Web araması yerine
* Kullanım [Bing görüntü arama](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/) görüntü yakaladığınız görüntüyü hakkında daha fazla bilgi ve benzeri görüntüleri Web'de bulmak için Öngörüler özelliği
* İşe [Bing yazım denetimi](https://azure.microsoft.com/services/cognitive-services/spell-check/) daha fazla ayrıştırılmış metninizi kalitesini artırmak için
* Tümleştirme [Microsoft Translator](https://azure.microsoft.com/services/cognitive-services/translator-text-api/) ayıklanan metninizi farklı dillerde görmek için
* Karışık ve diğer hizmetleri eşleşmesi [Bilişsel Hizmetleri portalı](https://azure.microsoft.com/services/cognitive-services/); sky sınırı kullanıcının!
