---
title: "Windows Phone Silverlight Engagement SDK tümleştirmesi"
description: "Azure Mobile Engagement Windows Phone Silverlight uygulamaları ile tümleştirme"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 447fea8d-f4e3-4ad4-8ec0-8e3cf1ad3ab0
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 29b18aecff783cebf617995e2a19f16f0b68b51b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="windows-phone-silverlight-engagement-sdk-integration"></a>Windows Phone Silverlight Engagement SDK tümleştirmesi
> [!div class="op_single_selector"]
> * [Windows Evrensel](mobile-engagement-windows-store-integrate-engagement.md) 
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md) 
> * [iOS](mobile-engagement-ios-integrate-engagement.md) 
> * [Android](mobile-engagement-android-integrate-engagement.md) 
> 
> 

Bu yordam, Azure Mobile Engagement analizi ve izleme, Windows Phone Silverlight uygulamanızda işlevlerine etkinleştirmek için en basit yolu açıklar.

Aşağıdaki adım kullanıcıları, oturumlar, etkinlikleri, kilitlenme ve Technicals ilgili tüm istatistikleri işlem için gereken günlükleri rapor etkinleştirmek için yeterli değildir. Olaylar, hatalar ve işleri gibi diğer istatistikleri işlem için gereken günlükleri rapor katılım API kullanarak el ile yapılması gerekir (bkz [API, Windows Phone Silverlight uygulaması etiketleme Gelişmiş Mobile Engagement kullanmayı](mobile-engagement-windows-phone-use-engagement-api.md) aşağıda) Bu istatistikler uygulama bağımlı olduğundan.

## <a name="supported-versions"></a>Desteklenen sürümler
Windows Silverlight için Mobile Engagement SDK'sı yalnızca hedefleme uygulamalara tümleştirilebilir:

* Windows Phone 8.0
* Windows Phone 8.1 Silverlight

> [!NOTE]
> Windows Phone 8.1 (Silverlight olmayan) hedefliyorsanız sonra başvurmak [Windows Evrensel tümleştirme yordamı](mobile-engagement-windows-store-integrate-engagement.md).
> 
> 

## <a name="install-the-mobile-engagement-silverlight-sdk"></a>Mobil katılım Silverlight SDK'sını yükleyin
Windows Silverlight için Mobile Engagement SDK'sı olarak adlandırılan bir Nuget paketi olarak kullanılabilir *MicrosoftAzure.MobileEngagement*. Visual Studio Nuget Paket Yöneticisi'nden yükleyebilirsiniz. 

## <a name="add-the-capabilities"></a>Yetenekleri ekleme
Engagement SDK'SININ bazı özellikleri Windows Phone Silverlight SDK'ın düzgün çalışması için gereklidir.

Açık, `WMAppManifest.xml` dosya ve aşağıdaki özellikleri de bildirilir emin `Capabilities` paneli:

* `ID_CAP_NETWORKING`
* `ID_CAP_IDENTITY_DEVICE`

## <a name="initialize-the-engagement-sdk"></a>Engagement SDK'yı başlatma
### <a name="engagement-configuration"></a>Katılım yapılandırma
Katılım yapılandırma içinde Merkezi `Resources\EngagementConfiguration.xml` projenizin dosya.

Belirtmek için bu dosyayı düzenleyin:

* Uygulama bağlantı dizenizi etiketleri arasına `<connectionString>` ve `<\connectionString>`.

Bunun yerine çalışma zamanında belirtmek istiyorsanız, katılım aracı başlatmadan önce aşağıdaki yöntemini çağırabilirsiniz:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

Bağlantı dizesi, uygulamanız için Azure Klasik Portalı'ndaki görüntülenir.

### <a name="engagement-initialization"></a>Katılım başlatma
Yeni bir proje oluşturduğunuzda, bir `App.xaml.cs` dosyası oluşturulur. Bu sınıf devraldığı `Application` ve birçok önemli yöntemler içerir. Engagement SDK'sı başlatmak için de kullanılır.

Değiştirme `App.xaml.cs`:

* Ekleme, `using` deyimleri:
  
      using Microsoft.Azure.Engagement;
* INSERT `EngagementAgent.Instance.Init` içinde `Application_Launching` yöntemi:
  
      private void Application_Launching(object sender, LaunchingEventArgs e)
      {
        EngagementAgent.Instance.Init();
      }
* INSERT `EngagementAgent.Instance.OnActivated` içinde `Application_Activated` yöntemi:
  
      private void Application_Activated(object sender, ActivatedEventArgs e)
      {
         EngagementAgent.Instance.OnActivated(e);
      }

> [!WARNING]
> Katılım başlatma, uygulamanızın başka bir yere ekleyin kesinlikle önerilmemektedir. Ancak, farkında olması, `EngagementAgent.Instance.Init` yöntemi, adanmış bir iş parçacığı üzerinde ve kullanıcı Arabirimi iş parçacığı üzerinde çalışır.
> 
> 

## <a name="basic-reporting"></a>Temel raporlama
### <a name="recommended-method--overload-your-phoneapplicationpage-classes"></a>Önerilen yöntem: aşırı yükleme, `PhoneApplicationPage` sınıfları
Rapor kullanıcıları, oturumlar, etkinlikleri, kilitlenme ve teknik istatistikleri işlem katılım tarafından gerekli tüm günlüklerin etkinleştirmek için yalnızca tüm yapabilirsiniz, `PhoneApplicationPage` alt sınıfları `EngagementPage` sınıfları.

Bunu yapmak için uygulamanızın bir sayfa nasıl bir örneği burada verilmiştir. Uygulamanızın tüm sayfalar için aynı şey yapabilirsiniz.

#### <a name="c-source-file"></a>C# kaynak dosyası
Sayfanızı değiştirmek `.xaml.cs` dosyası:

* Ekleme, `using` deyimleri:
  
      using Microsoft.Azure.Engagement;
* Değiştir `PhoneApplicationPage` ile `EngagementPage` :

**Katılım:**

        namespace Example
        {
          public partial class ExamplePage : PhoneApplicationPage
          {
            [...]
          }
        }

**Katılım ile:**

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [!WARNING]
> Sayfanız devraldığı varsa `OnNavigatedTo` yöntemi, izin vermek dikkatli olun `base.OnNavigatedTo(e)` çağırın. Aksi takdirde etkinlik raporlanmaz. Aslında, `EngagementPage` arayan `StartActivity` içinde `OnNavigatedTo` yöntemi.
> 
> 

#### <a name="xaml-file"></a>XAML dosyası
Sayfanızı değiştirmek `.xaml` dosyası:

* Ad alanı bildirimlerinize ekleyin:
  
      xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
* Değiştir `phone:PhoneApplicationPage` ile `engagement:EngagementPage` :

**Katılım:**

        <phone:PhoneApplicationPage>
            <!-- layout -->
        </phone:PhoneApplicationPage>

**Katılım ile:**

        <engagement:EngagementPage 
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP">

            <!-- layout -->
        </engagement:EngagementPage >

#### <a name="override-the-default-behavior"></a>Varsayılan davranışı geçersiz kılma
Varsayılan olarak, sayfa sınıf adını etkinlik adıyla hiçbir ek olarak bildirilir. Sınıf "Sayfa" soneki kullanıyorsa, katılım bunu de kaldırılır.

Yalnızca ad varsayılan davranışı geçersiz kılmak istiyorsanız, bunu kodunuzu ekleyin:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
           /* your code */
           return "new name";
        }

Bazı ek bilgiler etkinliklerinizi ile rapor istiyorsanız, bu kodunuzu ekleyebilirsiniz:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
           /* your code */
           return extra;
        }

Bu yöntemler içinden adlı `OnNavigatedTo` sayfanızın yöntemi.

### <a name="alternate-method-call-startactivity-manually"></a>Alternatif yöntem: çağrı `StartActivity()` el ile
Olamaz ya da tekrar etmek istiyor musunuz, `PhoneApplicationPage` sınıfları, bunun yerine, çağırarak etkinliklerinizi başlatabilirsiniz `EngagementAgent` doğrudan yöntemleri.

Arama öneririz `StartActivity` içinde `OnNavigatedTo` , mainpage yöntemi.

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
           base.OnNavigatedTo(e);
           EngagementAgent.Instance.StartActivity("MyPage");
        }

> [!IMPORTANT]
> Doğru oturumunuzu sonlandırmak emin olun.
> 
> SDK'yı otomatik olarak çağırır `EndActivity` uygulama kapatıldığında yöntemi. Bu nedenle, olan **yüksek oranda** çağırmak için önerilen `StartActivity` kullanıcı etkinliği değiştirdiğinizde, yöntemi ve **hiçbir zaman** çağrısı `EndActivity` yöntemi. Bu yöntem geçerli kullanıcının uygulama ayrıldı ve bu tüm uygulama günlüklerini etkiler katılım sunucusuna bir ileti gönderir.
> 
> 

## <a name="advanced-reporting"></a>Gelişmiş raporlama
İsteğe bağlı olarak, rapor uygulama belirli olaylar, hatalar ve işleri isteyebilirsiniz Bunu yapmak için diğer yöntemleri bulunan kullanın `EngagementAgent` sınıfı. Tüm Engagement'ın gelişmiş özelliklerinden kullanacak şekilde katılım API sağlar.

Daha fazla bilgi için bkz: [API, Windows Phone Silverlight uygulaması etiketleme Gelişmiş Mobile Engagement kullanmayı](mobile-engagement-windows-phone-use-engagement-api.md).

## <a name="advanced-configuration"></a>Gelişmiş yapılandırma
### <a name="disable-automatic-crash-reporting"></a>Otomatik Kilitlenme bildirimini devre dışı bırak
Otomatik Kilitlenme raporlama katılım özelliği devre dışı bırakabilirsiniz. Ardından, işlenmeyen bir özel durum meydana gelir, katılım hiçbir şey olmaz.

> [!WARNING]
> Bu özelliği devre dışı planlıyorsanız, uygulamanızda işlenmeyen bir kilitlenme meydana gelir, katılım kilitlenme göndermez unutmayın **ve** oturum ve işleri kapanmaz.
> 
> 

Otomatik Kilitlenme bildirimini devre dışı bırakmak için yalnızca yapılandırmanıza bağlı olarak, bildirilen şekilde özelleştirin:

#### <a name="from-engagementconfigurationxml-file"></a>Gelen `EngagementConfiguration.xml` dosyası
Kümesine rapor kilitlenme `false` arasında `<reportCrash>` ve `</reportCrash>` etiketler.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Gelen `EngagementConfiguration` çalışma zamanında nesne
Rapor kilitlenme EngagementConfiguration nesnesini kullanarak false olarak ayarlayın.

        /* Engagement configuration. */

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration(); engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        /\* Disable Engagement crash reporting. \*/ engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Veri bloğu modu
Varsayılan olarak, katılım hizmet raporları gerçek zamanlı olarak günlüğe kaydeder. Uygulamanızı günlükleri çok sık bildirirse, günlükleri arabellek ve (Buna "veri bloğu modu" denir) tümünü bir defada bir normal zaman üzerinde temel bildirmek için daha iyi olur.

Bunu yapmak için yöntemi çağırın:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Bağımsız değişkeni olarak bir değerdir **milisaniye**. Gerçek zamanlı günlük yeniden etkinleştirmek isterseniz, herhangi bir zamanda yalnızca herhangi bir parametre olmadan veya 0 değeri yöntemini çağırın.

Veri bloğu modu biraz pil ömrünün artırabilirsiniz ancak Engagement İzleyicisi üzerinde bir etkisi vardır: tüm oturumları ve işleri süre (dolayısıyla, oturumlar ve işleri veri bloğu eşik görünmeyebilir daha kısa) veri bloğu eşik yuvarlanır. Bir veri bloğu eşikten artık 30000 (30s) kullanılması önerilir. Günlükleri kaydedilen 300 öğelerle sınırlandırılır farkında olması gerekir. Gönderme çok uzunsa, bazı günlükleri kaybedebilir.

> [!WARNING]
> Veri bloğu eşiği daha düşük bir süre yapılandırılamaz bir saniye daha. SDK'yı otomatik olarak varsayılan değer olarak sıfırlamak olacaktır ve hata izleme gösterecektir bunun çalışırsanız, diğer bir deyişle, sıfır saniye. Bu günlükler gerçek zamanlı rapor için SDK tetikler.
> 
> 

