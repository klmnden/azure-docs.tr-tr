---
title: Windows Phone Silverlight Reach SDK tümleştirmesi
description: Azure Mobile Engagement Reach Windows Phone Silverlight uygulamaları ile tümleştirme
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: ''
ms.assetid: d3516a6b-db9f-4cdb-a475-4148edf81af1
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 34e5ce414ebf72fbecef6c921e57128e2658c921
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="windows-phone-silverlight-reach-sdk-integration"></a>Windows Phone Silverlight Reach SDK tümleştirmesi
> [!IMPORTANT]
> Azure Mobile Engagement 31/3/2018 üzerinde denemeler. Bu sayfa, kısa süre sonra silinir.
> 

Açıklanan tümleştirme yordamı izlemelisiniz [Windows Phone Silverlight Engagement SDK tümleştirmesi](mobile-engagement-windows-phone-integrate-engagement.md) bu kılavuz aşağıdaki önce.

## <a name="embed-the-engagement-reach-sdk-into-your-windows-phone-silverlight-project"></a>Windows Phone Silverlight projenize Engagement Reach SDK'sı ekleme
Eklemek için herhangi bir şey yok. `EngagementReach` Projenizde başvuruları ve kaynak zaten var.

> [!TIP]
> Görüntüleri bulunan özelleştirebilirsiniz `Resources` klasörü projenizin, özellikle marka simgesi (Bu varsayılan katılım simgesine).
> 
> 

## <a name="add-the-capabilities"></a>Yetenekleri ekleme
Engagement Reach SDK'SININ bazı ek yetenekler gerekir.

Açık, `WMAppManifest.xml` dosya ve aşağıdaki özellikleri bildirilir emin olun:

* `ID_CAP_PUSH_NOTIFICATION`
* `ID_CAP_WEBBROWSERCOMPONENT`

İlk bildirim görüntüsünü izin vermek için MPNS hizmeti tarafından kullanılır. İkincisi, bir tarayıcı görev SDK katıştırmak için kullanılır.

Düzen `WMAppManifest.xml` dosya ve içine ekleyin `<Capabilities />` etiketi:

    <Capability Name="ID_CAP_PUSH_NOTIFICATION" />
    <Capability Name="ID_CAP_WEBBROWSERCOMPONENT" />

## <a name="enable-the-microsoft-push-notification-service"></a>Microsoft anında bildirim hizmeti etkinleştirme
Kullanmak için **Microsoft anında bildirim hizmeti** (MPNS adlandırılır), `WMAppManifest.xml` dosya olmalıdır bir `<App />` ile etiketi bir `Publisher` özniteliğini projenizin adına ayarlayın.

## <a name="initialize-the-engagement-reach-sdk"></a>Engagement Reach SDK'yı başlatma
### <a name="engagement-configuration"></a>Katılım yapılandırma
Katılım yapılandırma içinde Merkezi `Resources\EngagementConfiguration.xml` projenizin dosya.

Reach yapılandırmasını belirtmek için bu dosyayı düzenleyin:

* *İsteğe bağlı*, yerel gönderim (MPNS) etkin olup olmadığını veya arasında değil belirtmek `<enableNativePush>` ve `</enableNativePush>` etiketleri (`true` varsayılan olarak).
* *İsteğe bağlı*, arasında itme kanal adını belirtmek `<channelName>` ve `</channelName>` etiketleri, aynı uygulamanız şu anda kullanın veya bu alanı boş bırakın, sağlar.

Bunun yerine çalışma zamanında belirtmek istiyorsanız, katılım aracı başlatmadan önce aşağıdaki yöntemini çağırabilirsiniz:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    engagementConfiguration.Reach.EnableNativePush = true;                  
    /* [Optional] whether the native push (MPNS) is activated or not. */

    engagementConfiguration.Reach.ChannelName = "YOUR_PUSH_CHANNEL_NAME";   
    /* [Optional] Provide the same channel name that your application may currently use. */

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

> [!TIP]
> MPNS anında iletme kanal, uygulamanızın adı belirtebilirsiniz. Varsayılan olarak, katılım üzerinde AppID tabanlı bir ad oluşturur. Katılım dışında itme kanal kullanmayı planlıyorsanız dışında adı kendiniz belirtmenize gerek yoktur var.
> 
> 

### <a name="engagement-initialization"></a>Katılım başlatma
Değiştirme `App.xaml.cs`:

* Ekleme, `using` deyimleri:
  
      using Microsoft.Azure.Engagement;
* INSERT `EngagementReach.Instance.Init` hemen sonra `EngagementAgent.Instance.Init` içinde `Application_Launching` :
  
      private void Application_Launching(object sender, LaunchingEventArgs e)
      {
         EngagementAgent.Instance.Init();
         EngagementReach.Instance.Init();
      }
* INSERT `EngagementReach.Instance.OnActivated` içinde `Application_Activated` yöntemi:
  
      private void Application_Activated(object sender, ActivatedEventArgs e)
      {
         EngagementAgent.Instance.OnActivated(e);
         EngagementReach.Instance.OnActivated(e);
      }

> [!IMPORTANT]
> `EngagementReach.Instance.Init` Adanmış bir iş parçacığı çalıştırır. Kendi başınıza sahip değil.
> 
> 

## <a name="app-store-submission-considerations"></a>Uygulama mağazası gönderimi dikkat edilecek noktalar
Microsoft anında iletme bildirimleri kullanırken bazı kuralları uygular:

Microsoft'tan [uygulama ilkeleri] belgeleri, bölüm 2.9:

1) Anında iletme bildirimleri almayı kabul edecek kullanıcı kaldırmasını isteyebilirsiniz. Ardından, ayarlarınızda, anında iletme bildirimleri devre dışı bırakmak için bir yol ekleyin.

EngagementReach nesne içinde/opt-vazgeçme, yönetmek için iki yöntem sunar `EnableNativePush()` ve `DisableNativePush()`. Örneğin, bir seçenek MPNS etkinleştirmek veya devre dışı bırakmak ayarlarında geçiş ile oluşturabilirsiniz.

MPNS katılım yapılandırma aracılığıyla devre dışı bırakmak karar verebilirsiniz\<windows phone-sdk-reach-yapılandırma\>.

> 2.9.1) uygulamayı gerekir ilk tanımlayan sağlanacak bildirimleri ve **(katılımı) kullanıcının express iznini elde**, ve **kullanıcı üzerinden kabul anında iletme bildirimleri alma dışında bir mekanizma sağlamanız gerekir**. Microsoft anında bildirim Hizmeti'ni kullanarak sağlanan tüm bildirimleri kullanıcı tarafından sağlanan açıklama ile tutarlı olmalıdır ve tüm ile uymalıdır geçerli [uygulama ilkeleri] [ Content Policies] ve [belirli uygulama türleri için ek gereksinimler].
> 
> 

2) Çok fazla anında iletme bildirimleri kullanmamanız gerekir. Katılım bildirimleri sizin için işler.

> 2.9.2) uygulama ve Microsoft anında iletme bildirimi hizmeti kullanımı değil aşırı ağ kapasitesi veya bant genişliği Microsoft anında bildirim hizmeti veya kullanmanız gerekir veya aksi halde unduly bir Windows Phone veya diğer Microsoft cihaz veya hizmeti makul kendi takdirine bağlı olarak Microsoft tarafından belirlenen aşırı anında iletme bildirimleri ile yük ve gerekir değil zarar herhangi Microsoft ağları veya sunucuları veya tüm üçüncü taraf sunucular veya Microsoft anında iletme bildirimi hizmeti bağlı ağlara kesintiye uğratabilir.
> 
> 

3) Criticals bilgi gönder MPNS kullanmayın. Katılım MPNS, kullanır, bu nedenle bu kural içinde katılım ön uç oluşturulan kampanyalar için de geçerlidir.

> 2.9.3) Microsoft anında bildirim hizmeti olmayabilir, kritik veya aksi takdirde görev bildirimleri yaşam veya ölüm önemlidir etkileyen göndermek için kullanılan, sınırlaması kritik bildirimleri da dahil olmak üzere ilgili tıbbi cihaz ya da koşul. MICROSOFT, HİÇBİR GARANTİ AÇIKÇA REDDEDER MICROSOFT ANINDA BİLDİRİM HİZMETİ KULLANIMINI YA DA MICROSOFT ANINDA BİLDİRİM HİZMETİ BİLDİRİMLERİNİ TESLİMİNİ OLACAKTIR KESİNTİSİZ, HATASIZ VEYA AKSİ GARANTİ BİR GERÇEK ZAMANLI OLARAK GERÇEKLEŞECEK.
> 
> 

**Bu öneriler dikkate almaz, uygulamanızın doğrulama işlemi geçer garanti edemez.**

## <a name="handle-data-push-optional"></a>Veri gönderimi (isteğe bağlı) işleme
Reach veri gönderimleri almak, uygulamanızın isterseniz, iki olay EngagementReach sınıfının uygulamak vardır:

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

Ardından, içeriği ayarlayın `EngagementReach.Instance.Handler` özel nesnesinde ile alan, `App.xaml.cs` içinde sınıf `Application_Launching` yöntemi.

**Örnek kod:**

    private void Application_Launching(object sender, LaunchingEventArgs e)
    {
       // your app initialization 
       EngagementReach.Instance.Handler = new ExampleReachHandler();
       // Engagement Agent and Reach initialization
    }

> [!NOTE]
> Varsayılan olarak, kendi uyarlamasını katılım kullanır `EngagementReachHandler`. Kendi oluşturmanız gerekmez ve bunu yaparsanız, her yöntemi geçersiz kılma yok. Katılım temel nesneyi seçmek için varsayılan davranıştır.
> 
> 

### <a name="layouts"></a>Düzenleri
Varsayılan olarak, bildirimler ve sayfaları görüntülemek için dll katıştırılmış kaynakları ulaşma kullanır.

Ancak, bu bileşenler, markasını yansıtmak için kendi kaynaklarını kullanmayı karar verebilirsiniz.

Geçersiz kılabilirsiniz `EngagementReachHandler` , düzenleri kullanmak için katılım bildirmek için bir alt yöntemleri:

**Örnek kod:**

    // In your subclass of EngagementReachHandler

    public override string GetTextViewAnnouncementUri()
    {
       // return the path of your own xaml
    }

    public override string GetWebViewAnnouncementUri()
    {
       // return the path of your own xaml
    }

    public override string GetPollUri()
    {
       // return the path of your own xaml
    }

    public override EngagementNotificationView CreateNotification(EngagementNotificationViewModel viewModel)
    {
       // return a new instance of your own notification
    }

> [!TIP]
> `CreateNotification` Yöntemi null dönebilirsiniz. Bildirim görüntülenmez ve reach kampanya bırakılacak.
> 
> 

Düzen uygulamanızı basitleştirmek için ayrıca kodunuz için temel olarak hizmet verebilir kendi xaml sunuyoruz. Engagement SDK'sı arşivindeki bulundukları (/ src/reach /).

> [!WARNING]
> Sağladığımız kaynakları kullandığımız tam aynı olanlardır. Doğrudan değiştirmek istiyorsanız, bu nedenle ad alanına ve ada değiştirmeyi unutmayın.
> 
> 

### <a name="notification-position"></a>Bildirim konumu
Varsayılan olarak, bir uygulama bildirimi uygulamanın tarafı sol alt kısmında görüntülenir. Geçersiz kılarak bu davranışı değiştirebilirsiniz `GetNotificationPosition` yöntemi `EngagementReachHandler` nesnesi.

    // In your subclass of EngagementReachHandler

    public override EngagementReachHandler.NotificationPosition GetNotificationPosition(EngagementNotificationViewModel viewModel)
    {
       // return a value of the EngagementReachHandler.NotificationPosition enum (TOP or BOTTOM)
    }

Şu anda arasından seçim yapabilirsiniz `BOTTOM` (varsayılan) ve `TOP` konumlar.

### <a name="launch-message"></a>İleti Başlatma
Bir kullanıcı bir sistem bildirimi (bildirim) tıkladığında, katılım uygulamasını başlatır, anında iletileri içeriğini yüklemek ve ilgili kampanya sayfasını görüntüleyin.

Uygulamayı başlatın ve (bağlı olarak, ağ hızı) sayfa görünümünü arasında bir gecikme yoktur.

Kullanıcıya bir şey yüklüyor belirtmek için bir ilerleme çubuğu veya bir İlerleme göstergesi gibi görsel bir bilgi sağlamalıdır. Katılım kendisi, ancak sağladığı birkaç işleyicileri sizin için işleyemez.

Geri çağırma uygulamak için aşağıdakileri yapın:

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

Geri çağırma ayarlayabileceğiniz, `Application_Launching` yöntemi, `App.xaml.cs` dosya, tercihen önce `EngagementReach.Instance.Init()` çağırın.

> [!TIP]
> Her işleyicisi kullanıcı Arabirimi iş parçacığı tarafından çağrılır. MessageBox veya bir şey UI ilgili kullanırken endişelenmeniz gerekmez.
> 
> 

[uygulama ilkeleri]:http://msdn.microsoft.com/library/windows/apps/hh184841(v=vs.105).aspx
[Content Policies]:http://msdn.microsoft.com/library/windows/apps/hh184842(v=vs.105).aspx
[belirli uygulama türleri için ek gereksinimler]:http://msdn.microsoft.com/library/windows/apps/hh184838(v=vs.105).aspx

