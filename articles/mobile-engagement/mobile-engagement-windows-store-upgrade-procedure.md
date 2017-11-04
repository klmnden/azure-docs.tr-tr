---
title: "Windows Evrensel uygulamaları SDK yükseltme yordamları"
description: "Azure Mobile Engagement için Windows Evrensel uygulamaları SDK yükseltme yordamları"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 4c898175-2cd6-43db-b350-bb408332f24d
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: fe85a99a92fb39082cafe7422b356de1f20f14bd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="windows-universal-apps-sdk-upgrade-procedures"></a>Windows Evrensel uygulamaları SDK yükseltme yordamları
Uygulamanıza katılım daha eski bir sürümü zaten bütünleştirdiyseniz, SDK'yı yükseltirken aşağıdaki noktaları dikkate almanız gerekir.

SDK çeşitli sürümleri eksik, birçok yordamı uygulamanız gerekebilir. Örneğin, 0.10.1 önce "Kimden 0.9.0'dan 0.10.1 için" yordamını takip etmek için sahip 0.11.0 sonra "Kimden 0.10.1 0.11.0 için" yordamı geçiş ise.

## <a name="from-330-to-340"></a>3.3.0 3.4.0 için
### <a name="test-logs"></a>Test günlükleri
SDK'sı tarafından üretilen konsol günlükleri artık etkin/devre dışı bırakılmış/filtrelenmiş olabilir. Bu özelleştirmek için özellik güncelleştirme `EngagementAgent.Instance.TestLogEnabled` bulunan değer birine `EngagementTestLogLevel` numaralandırma, örneğin:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="resources"></a>Kaynaklar
Reach katmana geliştirilmiştir. SDK'sı NuGet paketi kaynakları parçasıdır.

SDK'sının yeni sürüme yükseltme veya varolan dosyalarınızı kaynaklarınızı katmana klasöründen tutmak isteyip istemediğinizi seçebilirsiniz ancak:

* Önceki katmana sizin yerinize çalıştığından veya tümleştirme `WebView` öğeleri el ile sonra karar mevcut dosyalarınızı korumak için bunu hala çalışır. 
* Yeni katmana güncelleştirmek istiyorsanız, yalnızca tam değiştirme `overlay` kaynaklarınızı klasöründen SDK paketinden yeni bir (UWP uygulamaları: yükseltmeden sonra yeni katmana klasör % USERPROFILE % alabilirsiniz\\.nuget\packages\ MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [!WARNING]
> Yeni katmana kullanarak önceki bir sürümünde yapılan tüm özelleştirmeler üzerine yazar.
> 
> 

## <a name="from-320-to-330"></a>3.2.0 3.3.0 için
### <a name="resources"></a>Kaynaklar
Bu adım yalnızca özelleştirilmiş kaynaklar ile ilgilidir. Ardından (html, görüntüler, katmana) SDK tarafından sağlanan kaynakları özelleştirdiyseniz, yükseltmeden önce yedekleme ve özelleştirmeyi yükseltilmiş kaynaklardaki yeniden gerekir.

## <a name="from-310-to-320"></a>3.1.0 3.2.0 için
### <a name="resources"></a>Kaynaklar
Bu adım yalnızca özelleştirilmiş kaynaklar ile ilgilidir. Ardından (html, görüntüler, katmana) SDK tarafından sağlanan kaynakları özelleştirdiyseniz, yükseltmeden önce yedekleme ve özelleştirmeyi yükseltilmiş kaynaklardaki yeniden gerekir.

### <a name="webview-integration"></a>Web görünümü tümleştirme
Farklı cihaz form faktörleri eşleştirmek için bazı geliştirmeler'ın bu sürümünde sunulan. Web görünümü, tümleştirilmesi aşağıdaki eşleştiğinden emin olun:

XAML sayfası () içinde:

            <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Right" VerticalAlignment="Top"/>
            <WebView x:Name="engagement_announcement_content" Visibility="Collapsed" HorizontalAlignment="Right" VerticalAlignment="Top"/> 

Ve ilişkili .cs dosyanızda:

    using Microsoft.Azure.Engagement;
    using System;
    using Windows.ApplicationModel.Core;
    using Windows.UI.ViewManagement;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Navigation;

    namespace My.Namespace.Example
    {
            /// <summary>
            /// An empty page that can be used on its own or navigated to within a Frame.
            /// </summary>
            public sealed partial class ExampleEngagementReachPage : EngagementPage
            {
              public ExampleEngagementReachPage()
              {
                this.InitializeComponent();

                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }

              #region to implement
              /* Attach events when page is navigated. */
              protected override void OnNavigatedTo(NavigationEventArgs e)
              {
                /* Update the webview when the app window is resized. */
                Window.Current.SizeChanged += DisplayProperties_OrientationChanged;

                /* Update the webview when the app/status bar is resized. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged += DisplayProperties_VisibleBoundsChanged; 
    #endif
                base.OnNavigatedTo(e);
              }

              /* When page is left ensure to detach SizeChanged handler. */
              protected override void OnNavigatedFrom(NavigationEventArgs e)
              {
                Window.Current.SizeChanged -= DisplayProperties_OrientationChanged;
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged -= DisplayProperties_VisibleBoundsChanged;
    #endif
                base.OnNavigatedFrom(e);
              }

              /* "width" and "height" are the current size of your application display. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
              double width = ApplicationView.GetForCurrentView().VisibleBounds.Width;
              double height = ApplicationView.GetForCurrentView().VisibleBounds.Height;
    #else
              double width =  Window.Current.Bounds.Width;
              double height =  Window.Current.Bounds.Height;
    #endif

              /// <summary>
              /// Set your webview elements to the correct size.
              /// </summary>
              /// <param name="width">The width of your current display.</param>
              /// <param name="height">The height of your current display.</param>
              private void SetWebView(double width, double height)
              {
                #pragma warning disable 4014
                CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                        () =>
                        {
                          this.engagement_notification_content.Width = width;
                          this.engagement_announcement_content.Width = width;
                          this.engagement_announcement_content.Height = height;
                        });
              }

              /// <summary>
              /// Handler that takes the Windows.Current.SizeChanged and indicates that webviews have to be resized.
              /// </summary>
              /// <param name="sender">Original event trigger.</param>
              /// <param name="e">Window Size Changed Event arguments.</param>
              private void DisplayProperties_OrientationChanged(object sender, Windows.UI.Core.WindowSizeChangedEventArgs e)
              {
                double width = e.Size.Width;
                double height = e.Size.Height;

                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }

    #if WINDOWS_PHONE_APP || WINDOWS_UWP              
              /// <summary>
              /// Handler that takes the ApplicationView.VisibleBoundsChanged and indicates that webviews have to be resized
              /// </summary>
              /// <param name="sender">The related application view.</param>
              /// <param name="e">Related event arguments.</param>
              private void DisplayProperties_VisibleBoundsChanged(ApplicationView sender, Object e)
              {
                double width = sender.VisibleBounds.Width;
                double height = sender.VisibleBounds.Height;

                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }
    #endif
              #endregion
            }
    }

## <a name="from-200-to-300"></a>2.0.0 3.0.0 için
### <a name="resources"></a>Kaynaklar
Bu adım yalnızca özelleştirilmiş kaynaklar ile ilgilidir. Ardından (html, görüntüler, katmana) SDK tarafından sağlanan kaynakları özelleştirdiyseniz, yükseltmeden önce yedekleme ve özelleştirmeyi yükseltilmiş kaynaklardaki yeniden gerekir.

## <a name="from-111-to-200"></a>1.1.1 2.0.0 için
Aşağıdaki nasıl Azure Mobile Engagement tarafından desteklenen bir uygulamaya Capptain SAS tarafından sunulan Capptain hizmetinden bir SDK tümleştirmesi geçirileceğini açıklar. 

> [!IMPORTANT]
> Capptain ve Mobile Engagement aynı Hizmetleri değildir ve aşağıda verilen yordamı yalnızca istemci uygulaması geçirmek nasıl vurgular. Uygulama SDK'yı geçirme verilerinizi Capptain sunucularından Mobile Engagement sunucuya geçişi YAPILMAZ
> 
> 

Önceki bir sürümünden geçiş yapıyorsanız, Lütfen 1.1.1 için önce geçirmenizi sonra aşağıdaki yordamı uygulamak için Capptain web sitesine bakın

### <a name="nuget-package"></a>Nuget paketi
Değiştir **Capptain.WindowsPhone** tarafından **MicrosoftAzure.MobileEngagement** Nuget paketi.

### <a name="applying-mobile-engagement"></a>Mobil katılım uygulama
Terimi SDK kullanan `Engagement`. Projenizi bu değişikliği eşleşecek şekilde güncelleştirmeniz gerekir.

Geçerli Capptain nuget paketini kaldırmanız gerekir. Tüm değişikliklerinizi Capptain Kaynaklar klasörünü kaldırılacak göz önünde bulundurun. Bu dosyaları saklamak isterseniz bir kopyasını oluşturun.

Bundan sonra projenizde yeni Microsoft Azure katılım nuget paketini yükleyin. Doğrudan [nuget sitesinde] bulabilirsiniz. veya burada dizini. Bu eylem, katılım tarafından kullanılan tüm kaynakları dosyalarının yerini alır ve yeni katılım DLL, proje başvuruları ekler.

Proje başvuruları Capptain DLL başvurularını silerek temizlemeniz gerekir. Bunu yapmazsanız Capptain sürümü çakışır ve hataları gerçekleşir.

Capptain kaynakları özelleştirdiyseniz, eski dosyaları içeriğinizi kopyalayın ve bunları yeni katılım dosyalar yapıştırın. Xaml ve cs dosyaları güncelleştirilmesi gerektiğini unutmayın.

Bu adımlar tamamlandığında yeni katılım başvurular tarafından eski Capptain başvuruları değiştirin yeterlidir.

1. Tüm Capptain ad alanlarını güncelleştirilmesi gerekiyor.
   
    Geçişten önce:
   
        using Capptain.Agent;
        using Capptain.Reach;
   
    Geçişten sonra:
   
        using Microsoft.Azure.Engagement;
2. "Capptain" içeren tüm Capptain sınıfları "Katılım" içermelidir.
   
    Geçişten önce:
   
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
   
    Geçişten sonra:
   
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }
3. XAML dosyaları için de Capptain ad alanı ve özniteliklerini değiştirir.
   
    Geçişten önce:
   
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
   
    Geçişten sonra:
   
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>
4. Sayfa değiştirir
   
   > [!IMPORTANT]
   > Ayrıca değiştirir. Yeni ad `Microsoft.Azure.Engagement.Overlay`. Xaml ve cs dosyalarında kullanılacak sahiptir. Üstelik `CapptainGrid` adlandırılması için `EngagementGrid`, `capptain_notification_content` ve `capptain_announcement_content` adlandırıldığı `engagement_notification_content` ve `engagement_announcement_content`.
   > 
   > 
   
    Katmana için:
   
        <capptain:CapptainPageOverlay
          xmlns:capptain="using:Capptain.Overlay"
          ...
        </capptain:CapptainPageOverlay>
   
    Bu olur:
   
        <EngagementPageOverlay
          engagement="using:Microsoft.Azure.Engagement.Overlay"
          ...
        </engagement:EngagementPageOverlay>
5. İçin diğer kaynaklar Capptain resimler ve HTML dosyaları gibi bunlar ayrıca "Katılım" kullanacak şekilde yeniden adlandırıldığı gerektiğini lütfen unutmayın.

### <a name="project-declaration"></a>Proje bildirimi
Package.appxmanifest üzerinde `File Type Associations` gelen güncelleştirildi:

* capptain\_ulaşmak\_katılım için içerik\_ulaşmak\_içeriği
* capptain\_günlük\_katılım dosyasına\_günlük\_dosyası

### <a name="application-id--sdk-key"></a>Uygulama Kimliği / SDK anahtarı
Katılım bağlantı dizesini kullanır. Mobile Engagement ile bir uygulama kimliği ve bir SDK anahtarı belirtmeniz gerekmez, yalnızca bir bağlantı dizesi belirtmeniz gerekir. Bunu EngagementConfiguration dosyanızı ayarlayabilirsiniz.

Katılım yapılandırma ayarlanabilir, `Resources\EngagementConfiguration.xml` projenizin dosya.

Belirtmek için bu dosyayı düzenleyin:

* Uygulama bağlantı dizenizi etiketleri arasına `<connectionString>` ve `<\connectionString>`.

Bunun yerine çalışma zamanında belirtmek istiyorsanız, katılım aracı başlatmadan önce aşağıdaki yöntemini çağırabilirsiniz:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(args, engagementConfiguration);

Bağlantı dizesi, uygulamanız için Azure Klasik Portalı'ndaki görüntülenir.

### <a name="items-name-change"></a>Öğe adı değişikliği
Tüm öğeleri adlı *capptain* adlandırılmış *engagement*. Benzer şekilde *Capptain* için *Engagement*.

Yaygın olarak kullanılan Capptain öğeleri örnekleri:

* Şimdi EngagementConfiguration adlı CapptainConfiguration
* Şimdi EngagementAgent adlı CapptainAgent
* Şimdi EngagementReach adlı CapptainReach
* Şimdi EngagementHttpConfig adlı CapptainHttpConfig
* Şimdi GetEngagementPageName adlı GetCapptainPageName

Ayrıca etkiler yeniden adlandırmak Not yöntemleri geçersiz kılındı.

