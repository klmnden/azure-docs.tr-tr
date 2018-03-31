---
title: Gelişmiş Raporlama ile MobileApps Engagement Windows Evrensel
description: Azure Mobile Engagement Windows Evrensel uygulamaları ile tümleştirme
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: ''
ms.assetid: ea5030bf-73ac-49b7-bc3e-c25fc10e945a
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/12/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 88d28bbc32179698147ab7516cc414fd23ed4bd6
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="advanced-reporting-with-the-windows-universal-apps-engagement-sdk"></a>Gelişmiş Windows Evrensel uygulamaları Engagement SDK'sı raporlama
> [!IMPORTANT]
> Azure Mobile Engagement 31/3/2018 üzerinde denemeler. Bu sayfa, kısa süre sonra silinir.
> 

> [!div class="op_single_selector"]
> * [Evrensel Windows](mobile-engagement-windows-store-advanced-reporting.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-advanced-reporting.md)
> 
> 

Bu konu Windows Evrensel uygulamanız ek raporlama senaryolarda açıklar. Bu senaryolar, oluşturduğunuz uygulama uygulamak için seçebileceğiniz seçenekleriniz [Başlarken](mobile-engagement-windows-store-dotnet-get-started.md) Öğreticisi.

## <a name="prerequisites"></a>Önkoşullar
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

Bu öğreticiye başlamadan önce ilk tamamlamalısınız [Başlarken](mobile-engagement-windows-store-dotnet-get-started.md) kasıtlı olarak doğrudan ve basit öğretici. Bu öğretici aralarından seçim yapabileceğiniz ek seçenekler ele alınmaktadır.

## <a name="specifying-engagement-configuration-at-runtime"></a>Çalışma zamanında katılım yapılandırmasını belirtme
Katılım yapılandırma içinde Merkezi `Resources\EngagementConfiguration.xml` Burada belirtilmiş olan projenizin dosya içinde [Başlarken](mobile-engagement-windows-store-dotnet-get-started.md) konu.

Ancak, ayrıca, çalışma zamanında belirtebilirsiniz: katılım aracı başlatmadan önce aşağıdaki yöntemini çağırabilirsiniz:

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);



## <a name="recommended-method-overload-your-page-classes"></a>Önerilen yöntem: aşırı yükleme, `Page` sınıfları
Kullanıcılar, oturumlar, etkinlikleri, kilitlenme ve teknik istatistikleri işlem katılım tarafından gerekli tüm günlükleri raporlama etkinleştirmek için tüm olun, `Page` alt sınıfları `EngagementPage` sınıfları.

Burada, uygulamanızın bir sayfa için bir örnek verilmiştir. Uygulamanızın tüm sayfalar için aynı şey yapabilirsiniz.

### <a name="c-source-file"></a>C# kaynak dosyası
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
> Sayfanız `OnNavigatedTo` yöntemini geçersiz kılıyorsa `base.OnNavigatedTo(e)` öğesini çağırdığınızdan emin olun. Aksi takdirde etkinlik olması bildirilmedi ( `EngagementPage` çağrıları `StartActivity` içinde kendi `OnNavigatedTo` yöntemi).
> 
> 

### <a name="xaml-file"></a>XAML dosyası
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

### <a name="override-the-default-behaviour"></a>Varsayılan davranışı geçersiz kılma
Varsayılan olarak, sayfa sınıf adını etkinlik adıyla hiçbir ek olarak bildirilir. Sınıf "Sayfa" soneki kullanıyorsa, katılım kaldırır.

Ad için varsayılan davranışı geçersiz kılma için bu kodu ekleyin:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

Etkinliğiniz ek bilgilerle bildirmek için bu kodu ekleyin:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

Bu yöntemler içinden adlı `OnNavigatedTo` sayfanızın yöntemi.

### <a name="alternate-method-call-startactivity-manually"></a>Alternatif yöntem: çağrı `StartActivity()` el ile
Olamaz ya da tekrar etmek istiyor musunuz, `Page` sınıfları, bunun yerine, çağırarak etkinliklerinizi başlatabilirsiniz `EngagementAgent` doğrudan yöntemleri.

Arama öneririz `StartActivity` içinde `OnNavigatedTo` sayfanızın yöntemi.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [!IMPORTANT]
> Doğru oturumunuzu sonlandırmak emin olun.
> 
> Windows Evrensel SDK'yı otomatik olarak çağırır `EndActivity` uygulama kapatıldığında yöntemi. Bu nedenle, olan **yüksek oranda** çağırmak için önerilen `StartActivity` kullanıcı etkinliği değiştirdiğinizde, yöntemi ve **hiçbir zaman** çağrısı `EndActivity` yöntemi. Bu yöntem, geçerli kullanıcının tüm uygulama günlüklerini etkiler uygulama ayrıldı katılım sunucusu bildirir.
> 
> 

## <a name="advanced-reporting"></a>Gelişmiş raporlama
İsteğe bağlı olarak, uygulamaya özgü olaylar, hatalar ve bunu yapmak için işleri, kullanın diğer yöntemleri bulunan rapor isteyebilirsiniz `EngagementAgent` sınıfı. Katılım API tüm Engagement'ın gelişmiş özelliklerini kullanılmasına izin verir.

Daha fazla bilgi için bkz: [Gelişmiş Mobile Engagement Windows Evrensel uygulamanız API etiketleme kullanmayı](mobile-engagement-windows-store-use-engagement-api.md).

