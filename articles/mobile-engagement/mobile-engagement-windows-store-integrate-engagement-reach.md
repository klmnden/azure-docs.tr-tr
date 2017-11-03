---
title: "Windows Evrensel uygulamaları Reach SDK tümleştirmesi"
description: "Azure Mobile Engagement Reach Windows Evrensel uygulamaları ile tümleştirme"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a31ca1d6-856f-4aec-898a-07969ae5f7ec
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 9311e998e67d8d0d56da68fc9460df32ce7ce5a9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="windows-universal-apps-reach-sdk-integration"></a>Windows Evrensel uygulamaları Reach SDK tümleştirmesi
Açıklanan tümleştirme yordamı izlemelisiniz [Windows Evrensel Engagement SDK tümleştirmesi](mobile-engagement-windows-store-integrate-engagement.md) bu kılavuz aşağıdaki önce.

## <a name="embed-the-engagement-reach-sdk-into-your-windows-universal-project"></a>Windows Evrensel projenize Engagement Reach SDK'sı ekleme
Eklemek için herhangi bir şey yok. `EngagementReach`Projenizde başvuruları ve kaynak zaten var.

> [!TIP]
> Görüntüleri bulunan özelleştirebilirsiniz `Resources` klasörü projenizin, özellikle marka simgesi (Bu varsayılan katılım simgesine). Evrensel uygulamaları, aynı zamanda taşıyabilirsiniz `Resources` tutmak klasör içeriğini uygulamalar, ancak arasında paylaşmak için paylaşılan bir proje üzerinde olacaktır `Resources\EngagementConfiguration.xml` platform bağımlı olduğu gibi varsayılan konumuna dosya.
> 
> 

## <a name="enable-the-windows-notification-service"></a>Windows bildirim Hizmeti'ni etkinleştir
### <a name="windows-8x-and-windows-phone-81-only"></a>Windows 8.x ve Windows Phone 8.1 yalnızca
Kullanmak için **Windows bildirim hizmeti** (WNS adlandırılır) içinde `Package.appxmanifest` üzerinde dosya `Application UI` tıklayın `All Image Assets` sol bot kutusunda. Kutuya sağında `Notifications`, değiştirme `toast capable` gelen `(not set)` için `(Yes)`.

### <a name="all-platforms"></a>Tüm platformlar
Uygulamanızı Microsoft hesabınız ve engagement platformu eşitlemek gerekir. Bunun için bir hesap oluşturun veya oturum açmak gereken [windows Geliştirme Merkezi](https://dev.windows.com). Bundan sonra yeni bir uygulama oluşturun ve SID'sini ve gizli anahtarını bulun. Uygulama ayarınız katılım ön uçta gidin `native push` ve kimlik bilgilerinizi yapıştırın. Bundan sonra projenize sağ üzerinde select tıklayın `store` ve `Associate App with the Store...`. Yalnızca sizin oluşturduğunuz önce eşitlemek için uygulama seçmeniz gerekir.

## <a name="initialize-the-engagement-reach-sdk"></a>Engagement Reach SDK'yı başlatma
Değiştirme `App.xaml.cs`:

* INSERT `EngagementReach.Instance.Init` hemen sonra `EngagementAgent.Instance.Init` içinde `InitEngagement` yöntemi:
  
      private void InitEngagement(IActivatedEventArgs e)
      {
        EngagementAgent.Instance.Init(e);
        EngagementReach.Instance.Init(e);
      }
  
  `EngagementReach.Instance.Init` Adanmış bir iş parçacığı çalıştırır. Kendi başınıza sahip değil.

> [!NOTE]
> Anında iletme bildirimleri, uygulamanızda başka bir yerde kullanıyorsanız sonra sayesinde, gerek [itme kanalını paylaşan](#push-channel-sharing) Engagement Reach ile.
> 
> 

## <a name="integration"></a>Tümleştirme
Katılım duyuruları ve yoklamaları için Interstitial görünümleri ve Reach uygulama başlıkları uygulamanıza eklemek için iki yol sunar: yer paylaşımlı tümleştirme ve web görünümleri el ile tümleştirme. Her iki yaklaşım aynı sayfada birleştirmelisiniz değil.

Bu şekilde iki tümleştirme arasında seçim özetlenen:

* Sayfalarınızın zaten devralıyorsa Aracıdan yer paylaşımlı tümleştirme seçebilirsiniz `EngagementPage`, onu değiştirme yalnızca bir konudur `EngagementPage` tarafından `EngagementPageOverlay` ve `xmlns:engagement="using:Microsoft.Azure.Engagement"` tarafından `xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"` sayfalarınızı içinde.
* Tam olarak sayfalarınızı içinde ulaşmak UI yerleştirmek istiyorsanız veya başka bir devralma düzeyi sayfalarınıza eklemek istemiyorsanız, el ile tümleştirme web görünümleri seçebilirsiniz. 

### <a name="overlay-integration"></a>Yer paylaşımlı tümleştirme
Katılım katmana sayfanızda Reach kampanyaları görüntülemek için kullanılan kullanıcı Arabirimi öğeleri dinamik olarak ekler. Katmana düzeninize uyacak değil, web görünümleri bunun yerine el ile tümleştirme düşünmelisiniz.

.Xaml dosya değişikliği `EngagementPage` başvuru`EngagementPageOverlay`

* Ad alanı bildirimlerinize aşağıdakini ekleyin:
  
      xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"
* Değiştir `engagement:EngagementPage` ile `engagement:EngagementPageOverlay`:

**EngagementPage ile:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">

            <!-- Your layout -->
        </engagement:EngagementPage>

**EngagementPageOverlay ile:**

        <engagement:EngagementPageOverlay 
            xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay">

            <!-- Your layout -->
        </engagement:EngagementPageOverlay>

Ardından .cs dosyanızda sayfanızda etiketi `EngagementPageOverlay` yerine `EngagementPage` ve içeri aktarma `Microsoft.Azure.Engagement.Overlay`.

            using Microsoft.Azure.Engagement.Overlay;

* Değiştir `EngagementPage` ile `EngagementPageOverlay`:

**EngagementPage ile:**

            using Microsoft.Azure.Engagement;

            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPage
              {
                [...]
              }
            }

**EngagementPageOverlay ile:**

            using Microsoft.Azure.Engagement.Overlay;

            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPageOverlay 
              {
                [...]
              }
            }


Katılım katmana ekler bir `Grid` öğesi, sayfanın en üstünde oluşan düzeninizi ve iki `WebView` başlık ve diğeri Interstitial görünümü için bir öğeleri.

Doğrudan katmana öğeleri özelleştirebilirsiniz `EngagementPageOverlay.cs` dosya.

### <a name="web-views-manual-integration"></a>Web görünümleri el ile tümleştirme
Reach sayfalarınızın iki arama `WebView` öğeleri başlık ve Interstitial görünümü görüntülemek için sorumlu. Bu iki eklemek için yapmanız gereken tek şey. `WebView` öğeleri yere sayfalarınızda, örnek aşağıda verilmiştir:

    <Grid x:Name="engagementGrid">

      <!-- Your layout -->

      <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Stretch" VerticalAlignment="Top"/>
      <WebView x:Name="engagement_announcement_content" Visibility="Collapsed"  HorizontalAlignment="Stretch"  VerticalAlignment="Stretch"/>
    </Grid>


Bu örnekte `WebView` öğeler otomatik olarak onları ekran döndürme veya pencere boyutu değişikliği durumunda yeniden boyutlandırır kendi kapsayıcısına uyacak şekilde uzatılır.

> [!WARNING]
> Aynı adlandırma tutmanız önemlidir `engagement_notification_content` ve `engagement_announcement_content` için `WebView` öğeleri. Reach bunları adına göre tanımlamak için kullanılır. 
> 
> 

## <a name="handle-datapush-optional"></a>Tanıtıcı datapush (isteğe bağlı)
Reach veri gönderimleri almak, uygulamanızın isterseniz, iki olay EngagementReach sınıfının uygulamak vardır:

App.xaml.cs App() oluşturucuda ekleyin:

            EngagementReach.Instance.DataPushStringReceived += (body) =>
            {
              Debug.WriteLine("String data push message received: " + body);
              return true;
            };

            EngagementReach.Instance.DataPushBase64Received += (decodedBody, encodedBody) =>
            {
              Debug.WriteLine("Base64 data push message received: " + encodedBody);
              // Do something useful with decodedBody like updating an image view
              return true;
            };

Geri arama yönteminin her bir Boole değeri döndürür görebilirsiniz. Katılım, veri gönderimi gönderdikten sonra kendi arka uç için bir geri bildirim gönderir. Geri çağırma false değerini döndürürse `exit` geri bildirim gönder olacaktır. Aksi durumda olur `action`. Olaylar için bir geri çağırma ayarlarsanız `drop` katılım için geri döndürülecek.

> [!WARNING]
> Katılım katları geribildirimler veri gönderimi için almak mümkün değil. Bir olaya birkaç işleyicilerini ayarlama planlıyorsanız geri bildirim son karşılık gelir unutmayın gönderdi. Bu durumda, her zaman döndürür için kafa karıştırıcı geri bildirim ön uç üzerinde zorunda kalmamak için aynı değeri öneririz.
> 
> 

## <a name="customize-ui-optional"></a>(İsteğe bağlı) kullanıcı arabirimini özelleştirme
### <a name="first-step"></a>İlk adım
Biz UI ulaşma özelleştirmenizi sağlar.

Öğesinin bir alt kümesi oluşturmak zorunda Bunu yapmak için `EngagementReachHandler` sınıfı.

**Örnek kod:**

            using Microsoft.Azure.Engagement;

            namespace Example
            {
              internal class ExampleReachHandler : EngagementReachHandler
              {
               // Override EngagementReachHandler methods depending on your needs
              }
            }

Ardından, içeriği ayarlayın `EngagementReach.Instance.Handler` özel nesnesinde ile alan, `App.xaml.cs` içinde sınıf `App()` yöntemi.

**Örnek kod:**

            protected override void OnLaunched(LaunchActivatedEventArgs args)
            {
              // your app initialization 
              EngagementReach.Instance.Handler = new ExampleReachHandler();
              // Engagement Agent and Reach initialization
            }

> [!NOTE]
> Varsayılan olarak, kendi uyarlamasını katılım kullanır `EngagementReachHandler`.
> Kendi oluşturmanız gerekmez ve bunu yaparsanız, her yöntemi geçersiz kılma yok. Katılım temel nesneyi seçmek için varsayılan davranıştır.
> 
> 

### <a name="web-view"></a>Web görünümü
Varsayılan olarak, bildirimler ve sayfaları görüntülemek için dll katıştırılmış kaynakları ulaşma kullanır.

Tam özelleştirme olanakları sağlamak için yalnızca web görünümü kullanırız. Doğrudan düzenleri özelleştirmek istiyorsanız, kaynak dosyaları geçersiz kılma `EngagementAnnouncement.html` ve `EngagementNotification.html`. Katılım gereken tüm kodda `<body></body>` düzgün çalışması için. Ancak, dış etiket ekleyebilirsiniz `engagement_webview_area`.

Ancak, kendi kaynaklarını kullanmaya karar verebilirsiniz.

Geçersiz kılabilirsiniz `EngagementReachHandler` , düzenleri kullanır, ancak ilgilenebilmek için Engagement bildirmek için bir alt yöntemlerinde katıştırılmış katılım mekanizması:

**Örnek kod:**

            // In your subclass of EngagementReachHandler

            public override string GetAnnouncementHTML()
            {
              return base.GetAnnouncementHTML();
            }
            public override string GetAnnouncementName()
            {
              return base.GetAnnouncementName();
            }
            public override string GetNotfificationHTML()
            {
              return base.GetNotfificationHTML();
            }
            public override string GetNotfificationName()
            {
              return base.GetNotfificationName();
            }


Varsayılan olarak, AnnouncementHTML olan `ms-appx-web:///Resources/EngagementAnnouncement.html`. Anında iletme iletisi (metin Duyurusu, Web anoucement ve yoklama duyuru) içeriğini tasarım html dosyası temsil eder. AnnouncementName olan `engagement_announcement_content`. Xaml sayfası webview tasarımında adıdır.

NotfificationHTML olan `ms-appx-web:///Resources/EngagementNotification.html`. Anında iletme iletisi bildirimini tasarlamaya html dosyası temsil eder. NotfificationName olan `engagement_notification_content`. Xaml sayfası webview tasarımında adıdır.

### <a name="customization"></a>Özelleştirme
Bildirim ve web görünümü, katılım nesne korumak istiyorsanız sahip duyuru özelleştirebilirsiniz. Bu Web görünümü özen nesnesi üç kez - xaml ilk kez, ikinci kez .cs dosyanızdaki "setwebview()" yöntemi, üçüncü saat ve html dosyasındaki açıklanmaktadır.

* Xaml içinde geçerli grafik düzenini webview bileşeni açıklanmaktadır.
* .Cs dosyanızda "setwebview() (bildirim, duyuru) iki webview boyutunu Ayarla" tanımlayabilirsiniz. Uygulama yeniden boyutlandırılırken çok etkili olur.
* Katılım html dosyasında webview içerik, tasarım ve birbirleri arasındaki öğeleri konum açıklanmaktadır.

### <a name="launch-message"></a>İleti Başlatma
Bir kullanıcı bir sistem bildirimi (bildirim) tıkladığında, katılım uygulamasını başlatır, anında iletileri içeriği yüklemek ve ilgili kampanya sayfasını görüntüleyin.

Uygulamayı başlatın ve (bağlı olarak, ağ hızı) sayfa görünümünü arasında bir gecikme yoktur.

Kullanıcıya bir şey yüklüyor belirtmek için bir ilerleme çubuğu veya bir İlerleme göstergesi gibi görsel bir bilgi sağlamalıdır. Katılım kendisi, ancak sağladığı birkaç işleyicileri sizin için işleyemez.

Geri çağırma uygulamak için "Ortak App() {}" App.xaml.cs ekleyin:

            /* The application has launched and the content is loading.
             * You should display an indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageStarted += () => { [...] };

            /* The application has finished loading the content and the page
             * is about to be displayed.
             * You should hide the indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageCompleted += () => { [...] };

            /* The content has been loaded, but an error has occurred.
             * You can provide an information to the user.
             * You should hide the indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageFailed += () => { [...] };

Geri arama, "Ortak App() {}" yönteminde ayarlayabilirsiniz, `App.xaml.cs` dosya, tercihen önce `EngagementReach.Instance.Init()` çağırın.

> [!TIP]
> Her işleyicisi kullanıcı Arabirimi iş parçacığı tarafından çağrılır. MessageBox veya bir şey UI ilgili kullanırken endişelenmeniz gerekmez.
> 
> 

## <a id="push-channel-sharing"></a>Kanal paylaşımı gönderme
Ardından, uygulamanızda anında iletme bildirimleri başka bir amaç için kullanıyorsanız, paylaşımı özelliğini Engagement SDK'ın itme kanal kullanmak zorunda. Bu, kaçırılan itme önlemek için gerekir.

* Engagement Reach başlatma kendi itme kanala sağlayabilir. SDK'ın yeni bir tane isteyen yerine kullanır.

Engagement Reach başlatma itme kanalda güncelleştirmek `InitEngagement` yönteminden `App.xaml.cs` dosyası:

    /* Your own push channel logic... */
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    /*...Engagement initialization */
    EngagementAgent.Instance.Init(e);
    EngagementReach.Instance.Init(e,pushChannel);

* Alternatif olarak, ulaşma başlatma sonra bir geri çağırma itme kanal kez almak için Engagement Reach ayarlayabilirsiniz sonra itme kanal kullanmak istiyorsanız, SDK'sı tarafından oluşturulur.

Herhangi bir noktada, geri aramasını ayarlamasını **sonra** ulaşma başlatma:

    /* Set action on the SDK push channel. */
    EngagementReach.Instance.SetActionOnPushChannel((PushNotificationChannel channel) => 
    {
      /* The forwarded channel can be null if its creation fails for any reason. */
      if (channel != null)
      {
        /* Your own push channel logic... */
      });
    }

## <a name="custom-scheme-tip"></a>Özel şema İpucu
Özel şema kullanım sunuyoruz. Katılım uygulamanızda kullanılacak katılım ön uç öğesinden farklı türde bir URI gönderebilirsiniz. Varsayılan şema gibi `http, ftp, ...` olan yönetmek penceresi olacak Windows komut isteminde bir varsayılan uygulama cihazda yüklü olmaları durumunda. Ayrıca, uygulamanız için kendi özel şema oluşturabilirsiniz.

Özel bir şema, uygulamanızda ayarlamak için basit bir yolla açmaktır, `Package.appxmanifest` içinde gidin `Declarations` paneli. Seçin `Protocol` kullanılabilir bildirimlerinde Kaydırma kutusu ve ekleyin. Düzen `Name` yeni Protokolü alanıyla istenen adı.

Şimdi bu protokolünü kullanmak için düzenlemek, `App.xaml.cs` ile `OnActivated` yöntemi ve burada katılım ayrıca başlatmak unutmayın:

            /// <summary>
            /// Enter point when app his called by another way than user click
            /// </summary>
            /// <param name="args">Activation args</param>
            protected override void OnActivated(IActivatedEventArgs args)
            {
              /* Init engagement like it was launch by a custom uri scheme */
              EngagementAgent.Instance.Init(args);
              EngagementReach.Instance.Init(args);

              //TODO design action to do when app is launch

              #region Custom scheme use
              if (args.Kind == ActivationKind.Protocol)
              {
                ProtocolActivatedEventArgs myProtocol = (ProtocolActivatedEventArgs)args;

                if (myProtocol.Uri.Scheme.Equals("protocolName"))
                {
                  string path = myProtocol.Uri.AbsolutePath;
                }
              }
              #endregion

