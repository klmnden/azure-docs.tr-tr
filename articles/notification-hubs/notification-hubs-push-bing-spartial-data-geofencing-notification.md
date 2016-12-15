---
title: "Azure Notification Hubs ve Bing Uzamsal Veri ile coğrafi bölge sınırlamalı anında iletme bildirimleri | Microsoft Belgeleri"
description: "Bu öğreticide, Azure Notification Hubs ve Bing Uzamsal Veri ile konum temelli anında iletme bildirimleri göndermeyi öğreneceksiniz."
services: notification-hubs
documentationcenter: windows
keywords: "anında iletme bildirimi, anında iletme bildirimi"
author: dend
manager: yuaxu
editor: dend
ms.assetid: f41beea1-0d62-4418-9ffc-c9d70607a1b7
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/31/2016
ms.author: dendeli
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: b2a84e0479aac9ded08bb64e1ea20ddee6636cce


---
# <a name="geo-fenced-push-notifications-with-azure-notification-hubs-and-bing-spatial-data"></a>Azure Notification Hubs ve Bing Uzamsal Veri ile bölge sınırlamalı anında iletme bildirimleri
> [!NOTE]
> Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02).
> 
> 

Bu öğreticide, bir Evrensel Windows Platform uygulaması içinde kullanılan Azure Notification Hubs ve Bing Uzamsal Veri ile konum temelli anında iletme bildirimleri göndermeyi öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar
Öncelikle, tüm yazılım ve hizmet önkoşullarına sahip olduğunuzdan emin olmanız gerekir:

* [Visual Studio 2015 Güncelleştirmesi 1](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) veya üzeri ([Community Edition](https://go.microsoft.com/fwlink/?LinkId=691978&clcid=0x409) da geçerli olur). 
* [Azure SDK](https://azure.microsoft.com/downloads/)'nın en son sürümü. 
* [Bing Haritalar Geliştirme Merkezi hesabı](https://www.bingmapsportal.com/) (ücretsiz bir tane oluşturabilir ve Microsoft hesabınızla ilişkilendirebilirsiniz). 

## <a name="getting-started"></a>Başlarken
Projeyi oluşturarak başlayalım. Visual Studio'da **Boş Uygulama (Evrensel Windows)** türü yeni bir proje başlatın.

![](./media/notification-hubs-geofence/notification-hubs-create-blank-app.png)

Proje oluşturmayı tamamladıktan sonra, uygulamanın kendisi için donanımınız olması gerekir. Şimdi, bölge sınırlama altyapısı için her şeyi ayarlayalım. Bunun için Bing hizmetlerini kullanacağımızdan, belirli konum çerçevelerini sorgulamamıza izin veren bir ortak REST API uç noktası vardır:

    http://spatial.virtualearth.net/REST/v1/data/

Bunu çalıştırmak için aşağıdaki parametreleri belirtmeniz gerekir:

* **Veri Kaynağı Kimliği** ve **Veri Kaynağı Adı**: Veri kaynakları, Bing Haritalar API'sinde konumlar ve çalışma saatleri gibi kümelenmiş çeşitli meta veriler içerir. Burada, bunlar hakkında daha fazla bilgi edinebilirsiniz. 
* **Varlık Adı**: Bildirim için bir başvuru noktası olarak kullanmak istediğiniz varlık. 
* **Bing Haritalar API'si Anahtarı**: Bu, daha önce Bing Geliştirme Merkezi hesabı oluşturduğunuzda edindiğiniz anahtardır.

Yukarıdaki öğelerin her biri için kurulumu derinlemesine inceleyelim.

## <a name="setting-up-the-data-source"></a>Veri kaynağını ayarlama
Bunu, Bing Haritalar Geliştirme Merkezi'nde yapabilirsiniz. Üst gezinti çubuğunda **Veri kaynakları**'na tıklayın ve **Veri Kaynaklarını Yönet**'i seçin.

![](./media/notification-hubs-geofence/bing-maps-manage-data.png)

Daha önce Bing Haritalar API'si ile çalışmadıysanız büyük olasılıkla herhangi bir veri kaynağı mevcut olmayacaktır. Bu nedenle, bir veri kaynağına veri Yükle seçeneğine tıklayarak yeni bir tane oluşturabilirsiniz. Tüm gerekli alanları doldurduğunuzdan emin olun:

![](./media/notification-hubs-geofence/bing-maps-create-data.png)

Merak ediyor olabilirsiniz, veri dosyası nedir ve neleri karşıya yüklemeniz gerekir? Bu testin amaçları doğrultusunda, San Francisco rıhtımının belirli bir alanını çevreleyen kanal tabanlı örneği kullanabiliriz.

    Bing Spatial Data Services, 1.0, TestBoundaries
    EntityID(Edm.String,primaryKey)|Name(Edm.String)|Longitude(Edm.Double)|Latitude(Edm.Double)|Boundary(Edm.Geography)
    1|SanFranciscoPier|||POLYGON ((-122.389825 37.776598,-122.389438 37.773087,-122.381885 37.771849,-122.382186 37.777022,-122.389825 37.776598))

Yukarıdaki dize bu varlığı temsil eder:

![](./media/notification-hubs-geofence/bing-maps-geofence.png)

Yukarıdaki dizeyi kopyalayıp yeni bir dosyaya yapıştırın, dosyayı **NotificationHubsGeofence.pipe** olarak kaydedin ve Bing Geliştirme Merkezi'ne yükleyin.

> [!NOTE]
> **Sorgu Anahtarı**'ndan farklı olan **Ana Anahtar** için yeni bir anahtar belirtmeniz istenebilir. Pano aracılığıyla yeni bir anahtar oluşturun ve veri kaynağını karşıya yükleme sayfasını yenileyin.
> 
> 

Veri dosyasını karşıya yükledikten sonra, veri kaynağını yayımladığınızdan emin olmanız gerekir. 

Daha önce yaptığımız gibi **Veri Kaynaklarını Yönet**'e gidin, listede veri kaynağınızı bulun ve **Eylemler** sütununda **Yayımla**'ya tıklayın. Kısa bir süre içinde, **Yayımlanan Veri Kaynakları** sekmesinde veri kaynağınızı görmeniz gerekir:

![](./media/notification-hubs-geofence/bing-maps-published-data.png)

**Düzenle**'ye tıklarsanız bunun içinde hangi konumları gösterdiğimizi bir bakışta görebilirsiniz:

![](./media/notification-hubs-geofence/bing-maps-data-details.png)

Bu noktada portal, oluşturduğumuz bölge sınırına ait sınırları göstermez. İhtiyacımız olan tek şey, belirtilen konumun doğru yakın çevrede olduğunun onaylanmasıdır.

Şimdi veri kaynağı için tüm gereksinimlere sahipsiniz. API çağrısının istek URL'si hakkındaki ayrıntıları almak için, Bing Haritalar Geliştirme Merkezi'nde **Veri kaynakları**'na tıklayın ve **Veri Kaynağı Bilgisi**'ni seçin.

![](./media/notification-hubs-geofence/bing-maps-data-info.png)

Burada aradığımız şey **Sorgu URL**'sidir. Bu, cihazın şu anda bir konum sınırları içinde olup olmadığını denetlemek için sorgular çalıştırabileceğimiz uç noktadır. Bu denetimi gerçekleştirmek için, aşağıdaki parametrelerin eklendiği sorgu URL'sinde bir GET çağrısı yapmamız gerekir:

    ?spatialFilter=intersects(%27POINT%20LONGITUDE%20LATITUDE)%27)&$format=json&key=QUERY_KEY

Bu yolla cihazdan aldığımız bir hedef noktasını belirtirsiniz ve Bing Haritalar bu hedef noktasının bölge sınırı içinde olup olmadığını görmek için otomatik olarak hesaplamalar yapar. Bir tarayıcı (veya cURL) aracılığıyla isteği yürüttükten sonra, standart bir JSON yanıtı alırsınız:

![](./media/notification-hubs-geofence/bing-maps-json.png)

Bu yanıt yalnızca, söz konusu nokta belirlenen sınırlar içinde olduğunda gönderilir. Değilse boş bir **sonuçlar** demeti alırsınız:

![](./media/notification-hubs-geofence/bing-maps-nores.png)

## <a name="setting-up-the-uwp-application"></a>UWP uygulamasını ayarlama
Şimdi veri kaynağımız hazır olduğuna göre, daha önce önyüklemesini yaptığımız UWP uygulaması üzerinde çalışmaya başlayabiliriz.

Öncelikle, uygulamamız için konum hizmetlerini etkinleştirmemiz gerekir. Bunu yapmak için **Çözüm Gezgini**'nde `Package.appxmanifest` dosyasına çift tıklayın.

![](./media/notification-hubs-geofence/vs-package-manifest.png)

Açılan paket özellikleri sekmesinde **Özellikler**'e tıklayın ve **Konum**'u seçtiğinizden emin olun:

![](./media/notification-hubs-geofence/vs-package-location.png)

Konum özelliği bildirildiğine göre, çözümünüz içinde `Core` adlı yeni bir klasör oluşturun ve içine `LocationHelper.cs` adlı yeni bir dosya ekleyin:

![](./media/notification-hubs-geofence/vs-location-helper.png)

Bu noktada, `LocationHelper` sınıfı oldukça temel bir işleve sahiptir. Tek yaptığı, sistem API'si aracılığıyla kullanıcı konumunu almamızı sağlamasıdır:

    using System;
    using System.Threading.Tasks;
    using Windows.Devices.Geolocation;

    namespace NotificationHubs.Geofence.Core
    {
        public class LocationHelper
        {
            private static readonly uint AppDesiredAccuracyInMeters = 10;

            public async static Task<Geoposition> GetCurrentLocation()
            {
                var accessStatus = await Geolocator.RequestAccessAsync();
                switch (accessStatus)
                {
                    case GeolocationAccessStatus.Allowed:
                        {
                            Geolocator geolocator = new Geolocator { DesiredAccuracyInMeters = AppDesiredAccuracyInMeters };

                            return await geolocator.GetGeopositionAsync();
                        }
                    default:
                        {
                            return null;
                        }
                }
            }

        }
    }

UWP uygulamalarında kullanıcının konumunu alma hakkında daha fazla bilgi edinmek için resmi [MSDN belgesi](https://msdn.microsoft.com/library/windows/apps/mt219698.aspx)'ne başvurabilirsiniz.

Konum almanın çalışıp çalışmadığını denetlemek için ana sayfanızın kod tarafını açın (`MainPage.xaml.cs`). `MainPage` oluşturucusunda `Loaded`olayı için yeni bir olay işleyicisi oluşturun:

    public MainPage()
    {
        this.InitializeComponent();
        this.Loaded += MainPage_Loaded;
    }

Olay işleyicisinin uygulaması aşağıdaki gibidir:

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        var location = await LocationHelper.GetCurrentLocation();

        if (location != null)
        {
            Debug.WriteLine(string.Concat(location.Coordinate.Longitude,
                " ", location.Coordinate.Latitude));
        }
    }

`GetCurrentLocation` beklenebilir olduğu ve bu nedenle zaman uyumsuz bir içerik içinde yürütülmesi gerektiğinden, işleyiciyi zaman uyumsuz olarak bildirdiğimize dikkat edin. Ayrıca, belirli koşullar altında null bir konum alma olasılığımız nedeniyle (örneğin, konum hizmetleri devre dışı bırakıldığında ya da uygulamanın konuma erişim izinleri reddedildiğinde), null denetimi ile isteğin düzgün şekilde işlendiğinden emin olmamız gerekir.

Uygulamayı çalıştırın. Konum erişimine izin verdiğinizden emin olun:

![](./media/notification-hubs-geofence/notification-hubs-location-access.png)

Uygulama başlatıldıktan sonra, **Çıkış** penceresinde koordinatları görebilmeniz gerekir:

![](./media/notification-hubs-geofence/notification-hubs-location-output.png)

Şimdi konum almanın çalıştığını biliyorsunuz. Loaded test olay işleyicisini artık kullanmayacağımız için işleyiciyi kaldırabilirsiniz.

Sonraki adım konum değişikliklerini yakalamaktır. Bunun için `LocationHelper` sınıfına geri dönelim ve `PositionChanged` için olay işleyicisini ekleyelim:

    geolocator.PositionChanged += Geolocator_PositionChanged;

Uygulama, konum koordinatlarını **Çıkış** penceresinde gösterecektir:

    private static async void Geolocator_PositionChanged(Geolocator sender, PositionChangedEventArgs args)
    {
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            Debug.WriteLine(string.Concat(args.Position.Coordinate.Longitude, " ", args.Position.Coordinate.Latitude));
        });
    }

## <a name="setting-up-the-backend"></a>Arka ucu ayarlama
[GitHub'dan .NET Arka Ucu Örneği](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers)'ni indirin. İndirme işlemi tamamlandıktan sonra, `NotifyUsers` klasörünü ve ardından `NotifyUsers.sln` dosyasını açın.

`AppBackend` projesini **Başlangıç Projesi** olarak ayarlayın ve başlatın.

![](./media/notification-hubs-geofence/vs-startup-project.png)

Proje hedef cihazlara anında iletme bildirimleri göndermek için zaten yapılandırıldı. Bu nedenle yapmamız gereken yalnızca iki şey var: Bildirim hub'ı için doğru bağlantı dizesini değiştirin ve sadece kullanıcı bölge sınırı içinde olduğunda bildirim göndermek için sınır kimliği ekleyin.

Bağlantı dizesini yapılandırmak için `Models` klasöründe `Notifications.cs` öğesini açın. `NotificationHubClient.CreateClientFromConnectionString` işlevi, [Azure Portal](https://portal.azure.com)'dan alabileceğiniz bildirim hub'ınız hakkındaki bilgileri içermelidir (**Ayarlar**'da, **Erişim İlkeleri** dikey penceresinin içine bakın). Güncelleştirilen yapılandırma dosyasını kaydedin.

Şimdi, Bing Haritalar API'si sonucu için bir model oluşturmamız gerekir. Bunu yapmanın en kolay yolu, `Models` klasörüne sağ tıklayıp **Ekle** > **Sınıf**'ı seçmektir. Bunu, `GeofenceBoundary.cs` olarak adlandırın. Bunu yaptıktan sonra, ilk bölümde bahsettiğimiz API yanıtından JSON'ı kopyalayın ve Visual Studio'da **Düzenle** > **Özel Yapıştır** > **JSON'ı Sınıflar olarak Yapıştır**'ı kullanın. 

Böylece, nesnenin istendiği gibi tamamen seri durumdan çıkarılacağından emin oluruz. Elde edilen sınıf kümeniz şuna benzemelidir:

    namespace AppBackend.Models
    {
        public class Rootobject
        {
            public D d { get; set; }
        }

        public class D
        {
            public string __copyright { get; set; }
            public Result[] results { get; set; }
        }

        public class Result
        {
            public __Metadata __metadata { get; set; }
            public string EntityID { get; set; }
            public string Name { get; set; }
            public float Longitude { get; set; }
            public float Latitude { get; set; }
            public string Boundary { get; set; }
            public string Confidence { get; set; }
            public string Locality { get; set; }
            public string AddressLine { get; set; }
            public string AdminDistrict { get; set; }
            public string CountryRegion { get; set; }
            public string PostalCode { get; set; }
        }

        public class __Metadata
        {
            public string uri { get; set; }
        }
    }

Ardından `Controllers` > `NotificationsController.cs` öğesini açın. Hedef enlemini ve boylamını hesaba katmak için Post çağrısına ince ayar yapmamız gerekir. Bunun için işlev imzasına iki dize eklemeniz yeterlidir: `latitude` ve `longitude`.

    public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag, string latitude, string longitude)

Proje içinde `ApiHelper.cs` adlı yeni bir sınıf oluşturun. Bunu, nokta sınır kesişimlerini denetlemek amacıyla Bing'e bağlanmak için kullanacağız. Aşağıdaki şekilde bir `IsPointWithinBounds` işlevini uygulayın:

    public class ApiHelper
    {
        public static readonly string ApiEndpoint = "{YOUR_QUERY_ENDPOINT}?spatialFilter=intersects(%27POINT%20({0}%20{1})%27)&$format=json&key={2}";
        public static readonly string ApiKey = "{YOUR_API_KEY}";

        public static bool IsPointWithinBounds(string longitude,string latitude)
        {
            var json = new WebClient().DownloadString(string.Format(ApiEndpoint, longitude, latitude, ApiKey));
            var result = JsonConvert.DeserializeObject<Rootobject>(json);
            if (result.d.results != null && result.d.results.Count() > 0)
            {
                return true;
            }
            else
            {
                return false;
            }
        }
    }

> [!NOTE]
> API uç noktasını, daha önce Bing Geliştirme Merkezi'nden edindiğiniz sorgu URL'si ile değiştirdiğinizden emin olun (aynısı API anahtarı için de geçerlidir). 
> 
> 

Sorgunun sonuçları getirmesi belirtilen noktanın bölge sınırının içinde olduğu anlamına gelir, bu durumda `true` döndürürüz. Sonuç yoksa Bing bize noktanın arama çerçevesi dışında olduğunu belirtiyor demektir. Bu durumda, `false` döndürürüz.

`NotificationsController.cs` öğesine geri dönün, switch deyiminden hemen önce bir denetim oluşturun:

    if (ApiHelper.IsPointWithinBounds(longitude, latitude))
    {
        switch (pns.ToLower())
        {
            case "wns":
                //// Windows 8.1 / Windows Phone 8.1
                var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                // Windows 10 specific Action Center support
                toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                break;
        }
    }

Bu şekilde, bildirim yalnızca nokta sınırlar içinde olduğunda gönderilir.

## <a name="testing-push-notifications-in-the-uwp-app"></a>UWP uygulamasında anında iletme bildirimlerini test etme
UWP uygulamasında artık bildirimleri test edebilmemiz gerekir. `LocationHelper` sınıfı içinde yeni bir işlev oluşturun (`SendLocationToBackend`):

    public static async Task SendLocationToBackend(string pns, string userTag, string message, string latitude, string longitude)
    {
        var POST_URL = "http://localhost:8741/api/notifications?pns=" +
            pns + "&to_tag=" + userTag + "&latitude=" + latitude + "&longitude=" + longitude;

        using (var httpClient = new HttpClient())
        {
            try
            {
                await httpClient.PostAsync(POST_URL, new StringContent("\"" + message + "\"",
                    System.Text.Encoding.UTF8, "application/json"));
            }
            catch (Exception ex)
            {
                Debug.WriteLine(ex.Message);
            }
        }
    }

> [!NOTE]
> `POST_URL` öğesini önceki bölümde oluşturduğumuz dağıtılmış web uygulamanızın konumuna götürün. Şu an için bunu yerel olarak çalıştırabilirsiniz. Ancak genel bir sürümü dağıtma üzerine çalıştığınızdan, bunu dış sağlayıcı ile barındırmanız gerekir.
> 
> 

Şimdi, UWP uygulamasını anında iletme bildirimleri için kaydettiğimizden emin olalım. Visual Studio'da, **Proje** > **Mağaza** > **Uygulamayı mağaza ile ilişkilendir**'e tıklayın.

![](./media/notification-hubs-geofence/vs-associate-with-store.png)

Geliştirici hesabınızda oturum açtıktan sonra, mevcut bir uygulamayı seçtiğinizden emin olun veya yeni bir tane oluşturun ve paketi bununla ilişkilendirin. 

Geliştirme Merkezi'ne gidin ve yeni oluşturduğunuz uygulamayı açın. **Hizmetler** > **Anında İletme Bildirimleri** > **Live Services sitesine** tıklayın.

![](./media/notification-hubs-geofence/ms-live-services.png)

Sitede, **Uygulama Anahtarı** ve **Paket SID**'sini not edin. Azure Portal'da her ikisine de ihtiyacınız olacak: Bildirim hub'ınızı açın, **Ayarlar** > **Bildirim Hizmetleri** > **Windows'a (WNS)** tıklayın ve gerekli alanlara bu bilgileri girin.

![](./media/notification-hubs-geofence/notification-hubs-wns.png)

**Kaydet**'e tıklayın.

**Çözüm Gezgini**'nde **Başvurular**'a sağ tıklayın ve **NuGet Paketlerini Yönet**'i seçin. **Microsoft Azure Service Bus yönetilen kitaplık**'a bir başvuru eklememiz gerekir. `WindowsAzure.Messaging.Managed` için arama yapın ve bunu projenize ekleyin.

![](./media/notification-hubs-geofence/vs-nuget.png)

Test amacıyla, `MainPage_Loaded` olay işleyicisini yeniden oluşturabilir ve buna şu kod parçacığını ekleyebiliriz:

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    var hub = new NotificationHub("HUB_NAME", "HUB_LISTEN_CONNECTION_STRING");
    var result = await hub.RegisterNativeAsync(channel.Uri);

    // Displays the registration ID so you know it was successful
    if (result.RegistrationId != null)
    {
        Debug.WriteLine("Reg successful.");
    }

Yukarıdaki kod parçacığı uygulamayı bildirim hub'ına kaydeder. Artık başlamaya hazırsınız! 

`LocationHelper` sınıfındaki `Geolocator_PositionChanged` işleyicisi içine, konumu bölge sınırının içine zorla yerleştirecek bir test kodu parçası ekleyebilirsiniz:

    await LocationHelper.SendLocationToBackend("wns", "TEST_USER", "TEST", "37.7746", "-122.3858");

Gerçek koordinatları geçirmediğimiz (şu anda sınırlar içinde olmayabilirler) ve önceden tanımlanmış test değerleri kullandığımız için güncelleştirme üzerinde bir bildirim belirdiğini görürüz:

![](./media/notification-hubs-geofence/notification-hubs-test-notification.png)

## <a name="whats-next"></a>Sırada ne var?
Çözümün üretime hazır olduğundan emin olmak için, yukarıdakilere ek olarak izlemeniz gerekebilecek birkaç adım vardır.

Öncelikle, bölge sınırlarının dinamik olduğundan emin olmanız gerekebilir. Bu, yeni sınırları mevcut veri kaynağı içinde karşıya yükleyebilmek için Bing API'si ile bazı ek çalışmalar yapmayı gerektirir. Bu konu hakkında daha fazla bilgi edinmek için [Bing Uzamsal Veri Hizmetleri API belgelerine](https://msdn.microsoft.com/library/ff701734.aspx) başvurun.

İkinci olarak, teslimin doğru katılımcılara yapıldığından emin olmaya çalıştığınızdan, onları [etiketleme](notification-hubs-tags-segment-push-message.md) yoluyla hedeflemek isteyebilirsiniz.

Yukarıda gösterilen çözümde, çok çeşitli hedef platformlara sahip olabileceğiniz bir senaryo açıklanmaktadır. Bu nedenle, bölge sınırlamasını sisteme özgü özelliklerle sınırlamadık. Bununla birlikte, Evrensel Windows Platformu [bölge sınırlarını hemen algılayan](https://msdn.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence) özellikler sunar.

Notification Hubs özellikleri hakkında daha ayrıntılı bilgi için [belge portalımıza](https://azure.microsoft.com/documentation/services/notification-hubs/) göz atın.




<!--HONumber=Dec16_HO1-->


