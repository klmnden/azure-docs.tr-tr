---
title: "Windows Evrensel uygulamaları Engagement SDK tümleştirmesi"
description: "Azure Mobile Engagement Windows Evrensel uygulamaları ile tümleştirme"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 71236b68-5ebd-44aa-8c82-c7ca8098ea05
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 56a382a348609df1d1d308aeac39f47ca82ac4c8
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="windows-universal-apps-engagement-sdk-integration"></a>Windows Evrensel uygulamaları Engagement SDK tümleştirmesi
> [!div class="op_single_selector"]
> * [Evrensel Windows](mobile-engagement-windows-store-integrate-engagement.md) 
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md) 
> * [iOS](mobile-engagement-ios-integrate-engagement.md) 
> * [Android](mobile-engagement-android-integrate-engagement.md) 
> 
> 

Bu yordam, Engagement analizi ve Windows Evrensel uygulamanız işlevlerde izlemeyi etkinleştirmek için en basit yolu açıklar.

Aşağıdaki adım kullanıcıları, oturumlar, etkinlikleri, kilitlenme ve Technicals ilgili tüm istatistikleri işlem için gereken günlükleri rapor etkinleştirmek için yeterli değildir. Olaylar, hatalar ve işleri gibi diğer istatistikleri işlem için gereken günlükleri rapor katılım API kullanarak el ile yapılması gerekir (bkz [Gelişmiş Mobile Engagement Windows Evrensel uygulamanız API etiketleme kullanmayı](mobile-engagement-windows-store-use-engagement-api.md) bu istatistikleri uygulama bağımlı olduğundan.

## <a name="supported-versions"></a>Desteklenen sürümler
Mobile Engagement SDK'sı Windows Evrensel uygulamaları için yalnızca Windows çalışma zamanı ve hedefleyen Evrensel Windows platformu uygulamaları tümleştirilebilir:

* Windows 8
* Windows 8.1
* Windows Phone 8.1
* Windows 10 (Masaüstü ve mobil aileleri)

> [!NOTE]
> Windows Phone Silverlight hedefleme sonra başvurmak [Windows Phone Silverlight tümleştirme yordamı](mobile-engagement-windows-phone-integrate-engagement.md).
> 
> 

## <a name="install-the-mobile-engagement-universal-apps-sdk"></a>Mobil katılım Evrensel uygulamaları SDK yükleme
### <a name="all-platforms"></a>Tüm platformlar
Mobile Engagement SDK'sı için Windows Evrensel uygulaması olarak adlandırılan bir Nuget paketi olarak kullanılabilir *MicrosoftAzure.MobileEngagement*. Visual Studio Nuget Paket Yöneticisi'nden yükleyebilirsiniz.

### <a name="windows-8x-and-windows-phone-81"></a>Windows 8.x ve Windows Phone 8.1
NuGet SDK kaynakları otomatik olarak dağıtan `Resources` uygulama projesi kökündeki klasör.

### <a name="windows-10-universal-windows-platform-applications"></a>Windows 10 Evrensel Windows platformu uygulamaları
NuGet UWP uygulaması Henüz SDK kaynakları otomatik olarak dağıtmaz. Bunu yapmanın el ile kaynakların dağıtımı NuGet içinde yeniden girmesini kadar vardır:

1. Dosya Gezgini'ni açın.
2. Şu konuma gidin (**x.x.x** yüklemekte katılım sürümüdür): *% USERPROFILE %\\.nuget\packages\MicrosoftAzure.MobileEngagement\\**x.x.x**\\content\win81*
3. Sürükleme ve bırakma **kaynakları** dosya Gezgini'nde projenizi Visual Studio'da kök klasörüne.
4. Visual Studio projenizi seçin ve etkinleştirme **tüm dosyaları göster** simgesinin üstüne **Çözüm Gezgini**.
5. Bazı dosyalar projede dahil edilmez. İçeri aktarmak için bunları aynı anda sağ tıklayın **kaynakları** klasörünü **projeden hariç** üzerinde başka bir sağ tıklayın ardından **kaynakları** klasörü, **Proje Ekle** tüm klasör yeniden eklemek için. Tüm dosyaları **kaynakları** klasörü projenizde eklenmiştir.

## <a name="add-the-capabilities"></a>Yetenekleri ekleme
Engagement SDK'SININ bazı özellikleri Windows SDK'ın düzgün çalışması için gerekir.

Açık, `Package.appxmanifest` dosya ve aşağıdaki özellikleri bildirilir emin olun:

* `Internet (Client)`

## <a name="initialize-the-engagement-sdk"></a>Engagement SDK'yı başlatma
### <a name="engagement-configuration"></a>Katılım yapılandırma
Katılım yapılandırma içinde Merkezi `Resources\EngagementConfiguration.xml` projenizin dosya.

Belirtmek için bu dosyayı düzenleyin:

* Uygulama bağlantı dizenizi etiketleri arasına `<connectionString>` ve `<\connectionString>`.

Bunun yerine çalışma zamanında belirtmek istiyorsanız, katılım aracı başlatmadan önce aşağıdaki yöntemini çağırabilirsiniz:

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);

Bağlantı dizesi, uygulamanız için Azure portalda görüntülenir.

### <a name="engagement-initialization"></a>Katılım başlatma
Yeni bir proje oluşturduğunuzda, bir `App.xaml.cs` dosyası oluşturulur. Bu sınıf devraldığı `Application` ve birçok önemli yöntemler içerir. Engagement SDK'sı başlatmak için de kullanılır.

Değiştirme `App.xaml.cs`:

* Ekleme, `using` deyimleri:
  
      using Microsoft.Azure.Engagement;
* Tüm çağrıları için bir kez Engagement başlatma paylaşmak için bir yöntem tanımlayın:
  
      private void InitEngagement(IActivatedEventArgs e)
      {
        EngagementAgent.Instance.Init(e);
  
        // or
  
        EngagementAgent.Instance.Init(e, engagementConfiguration);
      }
* Çağrı `InitEngagement` içinde `OnLaunched` yöntemi:
  
      protected override void OnLaunched(LaunchActivatedEventArgs e)
      {
        InitEngagement(e);
      }
* Özel bir şema, başka bir uygulama veya komut satırını kullanarak uygulamanızı başlatıldığında sonra `OnActivated` yöntemi çağrılır. Ayrıca, uygulamanızın etkinleştirildiğinde Engagement SDK'sı başlatmak gerekir. Bunu yapmak için geçersiz kılma `OnActivated` yöntemi:
  
      protected override void OnActivated(IActivatedEventArgs args)
      {
        InitEngagement(args);
      }

> [!IMPORTANT]
> Katılım başlatma, uygulamanızın başka bir yere ekleyin kesinlikle önerilmemektedir.
> 
> 

## <a name="basic-reporting"></a>Temel raporlama
### <a name="recommended-method-overload-your-page-classes"></a>Önerilen yöntem: aşırı yükleme, `Page` sınıfları
Rapor kullanıcıları, oturumlar, etkinlikleri, kilitlenme ve teknik istatistikleri işlem katılım tarafından gerekli tüm günlüklerin etkinleştirmek için yalnızca tüm yapabilirsiniz, `Page` alt sınıfları `EngagementPage` sınıfları.

Bunu yapmak için uygulamanızın bir sayfa nasıl bir örneği burada verilmiştir. Uygulamanızın tüm sayfalar için aynı şey yapabilirsiniz.

#### <a name="c-source-file"></a>C# kaynak dosyası
Sayfanızı değiştirmek `.xaml.cs` dosyası:

* Ekleme, `using` deyimleri:
  
      using Microsoft.Azure.Engagement;
* Değiştir `Page` ile `EngagementPage`:

**Katılım:**

        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

**Katılım ile:**

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [!IMPORTANT]
> Sayfanız `OnNavigatedTo` yöntemini geçersiz kılıyorsa `base.OnNavigatedTo(e)` öğesini çağırdığınızdan emin olun. Aksi takdirde etkinlik bildirilmez (`EngagementPage`, `OnNavigatedTo` yönteminin içinde `StartActivity` öğesini çağırır).
> 
> 

#### <a name="xaml-file"></a>XAML dosyası
Sayfanızı değiştirmek `.xaml` dosyası:

* Ad alanı bildirimlerinize aşağıdakini ekleyin:
  
      xmlns:engagement="using:Microsoft.Azure.Engagement"
* Değiştir `Page` ile `engagement:EngagementPage`:

**Katılım:**

        <Page>
            <!-- layout -->
            ...
        </Page>

**Katılım ile:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

#### <a name="override-the-default-behaviour"></a>Varsayılan davranışı geçersiz kılma
Varsayılan olarak, sayfa sınıf adını etkinlik adıyla hiçbir ek olarak bildirilir. Sınıf "Sayfa" soneki kullanıyorsa, katılım bunu de kaldırılır.

Varsayılan davranışa adı için geçersiz kılmak istiyorsanız, bu kodunuzu eklemeniz yeterlidir:

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
Olamaz ya da tekrar etmek istiyor musunuz, `Page` sınıfları, bunun yerine, çağırarak etkinliklerinizi başlatabilirsiniz `EngagementAgent` doğrudan yöntemleri.

Arama için önerdiğimiz `StartActivity` içinde `OnNavigatedTo` sayfanızın yöntemi.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [!IMPORTANT]
> Doğru oturumunuzu sonlandırmak emin olun.
> 
> Windows Evrensel SDK'yı otomatik olarak çağırır `EndActivity` uygulama kapatıldığında yöntemi. Bu nedenle, olan **yüksek oranda** çağırmak için önerilen `StartActivity` kullanıcı etkinliği değiştirdiğinizde, yöntemi ve **hiçbir zaman** çağrısı `EndActivity` yöntemi, bu yöntem gönderir katılım sunucuya geçerli kullanıcı, uygulamayı bırakın sahiptir, bu işlem tüm uygulama günlüklerini etkiler.
> 
> 

## <a name="advanced-reporting"></a>Gelişmiş raporlama
İsteğe bağlı olarak, rapor uygulama belirli olaylar, hatalar ve işleri isteyebilirsiniz Bunu yapmak için diğer yöntemleri bulunan kullanın `EngagementAgent` sınıfı. Tüm Engagement'ın gelişmiş özelliklerinden kullanacak şekilde katılım API sağlar.

Daha fazla bilgi için bkz: [Gelişmiş Mobile Engagement Windows Evrensel uygulamanız API etiketleme kullanmayı](mobile-engagement-windows-store-use-engagement-api.md).

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
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Veri bloğu modu
Varsayılan olarak, katılım hizmet raporları gerçek zamanlı olarak günlüğe kaydeder. Uygulamanızı günlükleri çok sık bildirirse, günlükleri arabellek ve (Buna "veri bloğu modu" denir) tümünü bir defada bir normal zaman üzerinde temel bildirmek için daha iyi olur.

Bunu yapmak için yöntemi çağırın:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Bağımsız değişkeni olarak bir değerdir **milisaniye**. Gerçek zamanlı günlük yeniden etkinleştirmek isterseniz, herhangi bir zamanda yalnızca herhangi bir parametre olmadan veya 0 değeri yöntemini çağırın.

Veri bloğu modu biraz pil ömrünün artırabilirsiniz ancak Engagement İzleyicisi üzerinde bir etkisi vardır: tüm oturumları ve işleri süre (dolayısıyla, oturumlar ve işleri veri bloğu eşik görünmeyebilir daha kısa) veri bloğu eşik yuvarlanır. Bir veri bloğu eşikten artık 30000 (30s) kullanılması önerilir. Günlükleri kaydedilen 300 öğelerle sınırlandırılır farkında olması gerekir. Gönderme çok uzunsa, bazı günlükleri kaybedebilir.

> [!WARNING]
> Veri bloğu eşik değeri 1'ler daha küçük bir süre için yapılandırılamaz. Bunu yapmaya çalışırsanız, SDK'sı bir izleme şu hata ile gösterir ve başka bir deyişle, 0s varsayılan değer otomatik olarak sıfırlar. Bu günlükler gerçek zamanlı rapor için SDK tetikler.
> 
> 

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview

