---
title: "Evrensel Windows Platformu uygulamaları için Azure Notification Hubs ile çalışmaya başlama | Microsoft Docs"
description: "Bu öğreticide, bir Windows Evrensel Platform uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını öğrenirsiniz."
services: notification-hubs
documentationcenter: windows
author: jwhitedev
manager: kpiteira
editor: 
ms.assetid: cf307cf3-8c58-4628-9c63-8751e6a0ef43
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 12/22/2017
ms.author: jawh
ms.openlocfilehash: c09621d1152aafbe15039130f6ca24082dc5bd21
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="get-started-with-notification-hubs-for-universal-windows-platform-apps"></a>Evrensel Windows Platformu uygulamaları için Notification Hubs'ı kullanmaya başlama

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Genel Bakış
Bu makale, bir Evrensel Windows Platformu (UWP) uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını size gösterir.

Bu makalede, Microsoft Anında İletme Bildirimi Hizmeti'ni (WNS) kullanarak anında iletme bildirimleri alan boş bir Windows Mağazası uygulaması oluşturursunuz. İşiniz bittiğinde, uygulamanızı çalıştıran tüm cihazlara anında iletme bildirimleri yayımlamak için bildirim hub'ınızı kullanabileceksiniz.

## <a name="before-you-begin"></a>Başlamadan önce
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Bu öğreticinin tamamlanan kodunu [GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal)'da bulabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Bu öğretici için aşağıdakiler gereklidir:

* [Microsoft Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs) veya üstü
* [UWP uygulama geliştirme araçlarının yüklü olması](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up)
* Etkin bir Azure hesabı  
    Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Daha fazla bilgi için bkz. [Azure Ücretsiz Denemesi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-store-dotnet-get-started%2F).
* Etkin bir Windows Mağazası hesabı

Bu öğreticiyi tamamlamak UWP uygulamalarına ilişkin diğer tüm Notification Hubs öğreticileri için önkoşuldur.

## <a name="register-your-app-for-the-windows-store"></a>Uygulamanızı Windows Mağazası'na kaydetme
UWP uygulamalarına anında iletme bildirimleri göndermek için uygulamanızı Windows Mağazası ile ilişkilendirin. Daha sonra, WNS ile tümleştirmek için bildirim hub'ınızı yapılandırın.

1. Uygulamanızı henüz kaydetmediyseniz [Windows Geliştirme Merkezi](https://dev.windows.com/overview)'ne gidin, Microsoft hesabınızla oturum açın ve ardından **Yeni uygulama oluştur**'u seçin.

2. Uygulamanız için bir ad yazın ve ardından **Uygulama adını ayır**'ı seçin. Bunu yaptığınızda uygulamanız için yeni bir Windows Mağazası kaydı oluşturulur.

3. Visual Studio'da, UWP **Boş Uygulama** şablonunu kullanarak yeni bir Visual C# Mağaza uygulamaları projesi oluşturun ve **Tamam**'ı seçin.

4. Hedef ve en düşük platform sürümleri için varsayılan değerleri kabul edin.

5. Çözüm Gezgini'nde, Windows Mağazası uygulama projesine sağ tıklayın, **Mağaza**'yı ve ardından **Uygulamayı Mağaza ile ilişkilendir**'i seçin.  
    **Uygulamanızı Windows Mağazası ile ilişkilendirin** sihirbazı görüntülenir.

6. Sihirbazda Microsoft hesabınızla oturum açın.

7. 2. adımda kaydettiğiniz uygulamayı seçin, **İleri**'yi ve ardından **İlişkilendir**'i seçin. Bunu yaptığınızda uygulama bildirimine gerekli Windows Mağazası kayıt bilgileri eklenir.

8. Yeni uygulamanızın [Windows Geliştirme Merkezi](http://dev.windows.com/overview) sayfasına geri dönerek **Hizmetler**, **Anında iletme bildirimleri** ve ardından **WNS/MPNS**'i seçin.

9. **Yeni Bildirim**'i seçin.

10. **Boş (Bildirim)** şablonunu ve ardından **Tamam**'ı seçin.

11. Bildirim için bir **Ad** ve Görsel **Bağlam** iletisi girin, ardından **Taslak olarak kaydet**'i seçin.

12. [Uygulama Kayıt Portalı](http://apps.dev.microsoft.com)'na gidin ve oturum açın.

13. Uygulamanızın adını seçin. **Windows Mağazası** platformu ayarlarında **Uygulama Gizli Anahtarı** parolasını ve **Paket güvenlik tanımlayıcısı (SID)** değerini not edin.

    >[!WARNING]
    >Uygulama gizli anahtarı ve paket SID'si önemli güvenlik kimlik bilgileridir. Bu değerleri kimseyle paylaşmayın veya uygulamanızla birlikte dağıtmayın.

## <a name="configure-your-notification-hub"></a>Bildirim hub'ınızı yapılandırma
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p><b>Bildirim Hizmetleri</b> bölümünde <b>Windows (WNS)</b> seçeneğini belirleyin ve <b>Güvenlik Anahtarı</b> kutusuna uygulama gizli parolasını girin. Önceki bölümde WNS'den edindiğiniz değeri <b>Paket SID'si</b> kutusuna girin ve ardından <b>Kaydet</b>'i seçin.</p>
</li>
</ol>

![Paket SID'si ve Güvenlik Anahtarı kutuları](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

Bildirim hub'ınız WNS ile birlikte çalışacak şekilde yapılandırıldı. Uygulamanızı kaydetmek ve bildirim göndermek için gerekli bağlantı dizelerine sahipsiniz.

## <a name="connect-your-app-to-the-notification-hub"></a>Uygulamanızı bildirim hub'ına bağlama
1. Visual Studio'da çözüme sağ tıklayın ve ardından **NuGet Paketlerini Yönet**'i seçin.  
    **NuGet Paketlerini Yönet** penceresi açılır.

2. Arama kutusuna **WindowsAzure.Messaging.Managed** yazın, **Yükle**'yi seçin ve kullanım koşullarını kabul edin.
   
    ![NuGet Paketlerini Yönet penceresi][20]
   
    Bu eylem [WindowsAzure.Messaging.Managed NuGet paketini](http://nuget.org/packages/WindowsAzure.Messaging) kullanarak Windows için Azure Mesajlaşma kitaplığına bir başvuruyu indirir, ekler ve yükler.

3. App.xaml.cs proje dosyasını açın ve aşağıdaki `using` deyimleri ekleyin: 
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.UI.Popups;

4. App.xaml.cs dosyasında ayrıca, **App** sınıfına aşağıdaki **InitNotificationsAsync** yöntem tanımını ekleyin:
   
        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            var hub = new NotificationHub("<your hub name>", "<Your DefaultListenSharedAccessSignature connection string>");
            var result = await hub.RegisterNativeAsync(channel.Uri);
   
            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
   
        }
   
    Bu kod, WNS'den uygulamanın kanal URI'sini alır ve ardından bu kanal URI'sini bildirim hub'ınıza kaydeder.
   
    >[!NOTE]
    >* **hub name** yer tutucusunu Azure portalında görünen bildirim hub'ının adıyla değiştirin. 
    >* Ayrıca, bağlantı dizesi yer tutucusunu önceki bölümde yer alan bildirim hub'ı **Erişim İlkeleri** sayfasında edindiğiniz **DefaultListenSharedAccessSignature** bağlantı dizesi ile değiştirin.
   > 
   > 
5. App.xaml.cs dosyasındaki **OnLaunched** olay işleyicisinin üst kısmında, yeni **InitNotificationsAsync** yöntemine aşağıdaki çağrıyı ekleyin:
   
        InitNotificationsAsync();
   
    Bu eylem uygulama her başlatıldığında kanal URI'sinin bildirim hub'ınıza kaydedilmesini garanti eder.

6. Uygulamayı çalıştırmak için **F5** tuşuna basın. Kayıt anahtarını içeren bir iletişim kutusu görüntülenir.

Uygulamanız şimdi bildirim almaya hazırdır.

## <a name="send-notifications"></a>Bildirim gönderme
[Azure portalından](https://portal.azure.com/) bildirim göndererek uygulamanızda bildirim alma testi gerçekleştirebilirsiniz. Bildirim hub'ındaki **Test Gönderimi** düğmesini aşağıdaki resimde gösterilen şekilde kullanın:

![Test Gönderimi bölmesi](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)

Anında iletme bildirimleri normalde, uyumlu bir kitaplık kullanılarak Mobile Services veya ASP.NET gibi bir arka uç hizmetinde gönderilir. Arka ucunuz için uygun bir kitaplık yoksa bildirim iletilerini göndermek için doğrudan REST API de kullanabilirsiniz. 

Bu öğreticide yalnızca bir arka uç hizmeti yerine bir konsol uygulamasındaki bildirim hub'ları için .NET SDK ile bildirim göndererek istemci uygulamanızı test etmeyi göstereceğiz. Bir ASP.NET arka ucundan bildirim göndermek için sonraki adım olarak [Kullanıcılara anında iletme bildirimleri göndermek için Notification Hubs'ı kullanma] öğreticisini öneririz. Ancak, aşağıdaki yaklaşımlardan kullanarak bildirim gönderebilirsiniz:

* **REST arabirimi**: [REST arabirimini](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx) kullanarak herhangi bir arka uç platformunda bildirimleri destekleyebilirsiniz.

* **Microsoft Azure Notification Hubs .NET SDK'sı**: Visual Studio için NuGet Paket Yöneticisi'nde [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) komutunu çalıştırın.

* **Node.js**: Bkz. [Node.js'den Notification Hubs'ı kullanma](notification-hubs-nodejs-push-notification-tutorial.md).
* **Azure Mobile Apps**: Notification Hubs ile tümleştirilmiş bir Azure mobil uygulaması arka ucundan nasıl bildirim gönderildiğinin bir örneği için bkz. [Mobile Apps için anında iletme bildirimleri ekleme](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).

* **Java veya PHP**: REST API'lerini kullanarak bildirim gönderme örnekleri için bkz:
    * [Java](notification-hubs-java-push-notification-tutorial.md)
    * [PHP](notification-hubs-php-push-notification-tutorial.md)

## <a name="next-steps"></a>Sonraki adımlar
Bu basit örnekte, portalı veya konsol uygulamasını kullanarak tüm Windows cihazlarınıza yayın bildirimleri gönderdiniz. Sonraki adım için [Kullanıcılara anında iletme bildirimleri göndermek için Notification Hubs'ı kullanma] öğreticisini öneririz. Bu, belirli kullanıcıları hedeflemek için etiketler kullanarak ASP.NET arka ucundan nasıl bildirim göndereceğinizi gösterir.

Kullanıcılarınızı ilgi alanı gruplarına göre segmentlere ayırmak istiyorsanız bkz. [Son dakika haberleri göndermek için Notification Hubs kullanma]. 

Notification Hubs hakkında daha fazla genel bilgi için bkz. [Notification Hubs kılavuzu](notification-hubs-push-notification-overview.md).

<!-- Images. -->
[13]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-console-app.png
[14]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-toast.png
[19]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-reg.png
[20]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-universal-app-install-package.png

<!-- URLs. -->

[Kullanıcılara anında iletme bildirimleri göndermek için Notification Hubs'ı kullanma]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Son dakika haberleri göndermek için Notification Hubs kullanma]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md

[toast catalog]: http://msdn.microsoft.com/library/windows/apps/hh761494.aspx
[tile catalog]: http://msdn.microsoft.com/library/windows/apps/hh761491.aspx
[badge overview]: http://msdn.microsoft.com/library/windows/apps/hh779719.aspx
 
