<properties
    pageTitle="Windows Mağazası Uygulamaları için Azure Notification Hubs'ı kullanmaya başlama | Microsoft Azure"
    description="Bu öğreticide, bir Windows Mağazası veya Windows Phone 8.1 (Silverlight olmayan) uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını öğrenirsiniz."
    services="notification-hubs"
    documentationCenter="windows"
    authors="wesmc7777"
    manager="dwrede"
    editor="dwrede"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="03/28/2016"
    ms.author="wesmc"/>

# Windows Mağazası Uygulamaları için Notification Hubs'ı kullanmaya başlama

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##Genel Bakış

Bu öğretici, bir Windows Mağazası veya Windows Phone 8.1 (Silverlight olmayan) uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını size gösterir. Windows Phone 8.1 Silverlight'ı hedefliyorsanız lütfen [Windows Evrensel](notification-hubs-windows-phone-get-started.md) sürümüne bakın.
Bu öğreticide, Microsoft Anında İletme Bildirimi Hizmeti'ni (WNS) kullanarak anında iletme bildirimleri alan boş bir Windows Mağazası uygulaması oluşturursunuz. İşiniz bittiğinde, uygulamanızı çalıştıran tüm cihazlara anında iletme bildirimleri yayımlamak için bildirim hub'ınızı kullanabileceksiniz.


## Başlamadan önce

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Bu öğreticinin tamamlanan kodu GitHub'da [buradan](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal) bulunabilir.



##Önkoşullar

Bu öğretici için aşağıdakiler gereklidir:

+ Windows için Microsoft Visual Studio Express 2013 ile Güncelleştirme 2 veya sonrası<br/>Visual Studio'nun bu sürümü, bir evrensel uygulama projesi oluşturmak için gereklidir. Yalnızca Windows Mağazası uygulaması oluşturmak istiyorsanız Windows 8 için Visual Studio 2012 Express gerekir.

+ Etkin bir Windows Mağazası hesabı

+ Etkin bir Azure hesabı <br/>Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-store-dotnet-get-started%2F).

Bu öğreticiyi tamamlamak Windows Mağazası uygulamalarına ilişkin diğer tüm Notification Hubs öğreticileri için önkoşuldur.

##Uygulamanızı Windows Mağazası'na kaydetme

Windows Mağazası uygulamalarına anında iletme bildirimleri göndermek için uygulamanızı Windows Mağazası ile ilişkilendirmeniz gerekir. Daha sonra, WNS ile tümleştirmek için bildirim hub'ınızı yapılandırmanız gerekir.

1. Uygulamanızı henüz kaydetmediyseniz [Windows Geliştirme Merkezi](http://go.microsoft.com/fwlink/p/?LinkID=266582)'ne gidin, Microsoft hesabınızla oturum açın ve ardından **Yeni uygulama oluştur**'a tıklayın.


2. Uygulamanız için bir ad yazın ve **Uygulama adını ayır**'a tıklayın.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hubs-win8-app-name.png)

    Bu, uygulamanız için yeni bir Windows Mağazası kaydı oluşturur.

3. Visual Studio'da, **Boş Uygulama** şablonunu kullanarak yeni bir Visual C# Mağaza Uygulamaları projesi oluşturun.

    ![][2]

4. Çözüm Gezgini'nde, Windows Mağazası uygulama projesine sağ tıklayın, **Mağaza**'ya ve ardından **Uygulamayı Mağaza ile ilişkilendir...** seçeneğine tıklayın.

    ![][3]

    **Uygulamanızı Windows Mağazası ile ilişkilendirin** sihirbazı görüntülenir.

5. Sihirbazda **Oturum aç**'a tıklayın ve ardından Microsoft hesabınızla oturum açın.

6. 2. adımda kaydettiğiniz uygulamaya tıklayın, **İleri**'ye ve ardından **İlişkilendir**'e tıklayın.

    ![][4]

    Bu, uygulama bildirimine gerekli Windows Mağazası kayıt bilgilerini ekler.

7. (İsteğe bağlı) Windows Phone mağazası uygulama projesi için 4-6 adımlarını yineleyin.  

8. Yeni uygulamanız için [Windows Geliştirme Merkezi](http://go.microsoft.com/fwlink/p/?LinkID=266582) sayfasına geri dönün, **Hizmetler**'e ve **Anında iletme bildirimleri**'ne tıklayın. Ardından, **Windows Anında Bildirim Hizmetleri (WNS) ve Microsoft Azure Mobile Services** altındaki **Live Services** sitesine tıklayın.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hubs-win8-app-live-services.png)

9. **Uygulama Ayarları** sekmesinde **Gizli anahtar** ve **Paket güvenlik tanımlayıcısı (SID)** değerlerini not edin.

    ![][6]

    > [AZURE.WARNING]
    Gizli anahtar ve paket SID'si önemli güvenlik kimlik bilgileridir. Bu değerleri kimseyle paylaşmayın veya uygulamanızla birlikte dağıtmayın.

##Bildirim hub'ınızı yapılandırma

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="7">
<li><p>Üst kısımdaki <b>Yapılandır</b> sekmesini seçin, önceki bölümde WNS'den aldığınız <b>Gizli anahtar</b> ve <b>Paket SID'si</b> değerlerini girin ve ardından <b>Kaydet</b>'e tıklayın.</p>
</li>
</ol>

&emsp;&emsp;![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)


Bildirim hub'ınız şimdi WNS ile birlikte çalışmak üzere yapılandırıldı. Ayrıca, uygulamanızı kaydetmenizi ve bildirim göndermenizi sağlayan bağlantı dizelerine sahipsiniz.

##Uygulamanızı bildirim hub'ına bağlama

1. Visual Studio'da çözüme sağ tıklayın ve ardından **NuGet Paketlerini Yönet**'e tıklayın.

    Bu, **NuGet Paketlerini Yönet** iletişim kutusunu görüntüler.

2. `WindowsAzure.Messaging.Managed` için arama yapın ve **Yükle**'ye tıklayın, çözümdeki tüm projeleri seçin ve kullanım koşullarını kabul edin.

    ![][20]

    Bu, <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">WindowsAzure.Messaging.Managed NuGet paketini</a> kullanarak tüm projelerde Windows için Azure Mesajlaşma kitaplığına bir başvuruyu indirir, ekler ve yükler.

3. App.xaml.cs dosyasını açın ve aşağıdaki `using` deyimleri ekleyin. Evrensel bir projede bu dosya `<project_name>.Shared` klasöründe bulunur.

        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.UI.Popups;



4. Ayrıca App.xaml.cs dosyasında, **Uygulama** sınıfına aşağıdaki **InitNotificationsAsync** yöntem tanımını ekleyin:

        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            var hub = new NotificationHub("<hub name>", "<connection string with listen access>");
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

    >[AZURE.NOTE]"Hub name" yer tutucusunu, **Notification Hubs** sekmesindeki [Klasik Azure Portalı]'nda görünen anında bildirim hub'ı adı ile değiştirdiğinizden emin olun (örneğin, önceki örnekte **mynotificationhub2**). Ayrıca bağlantı dizesi yer tutucusunu, önceki bölümde edindiğiniz **DefaultListenSharedAccessSignature ** adlı bağlantı dizesi ile değiştirin.

5. App.xaml.cs dosyasındaki **OnLaunched** olay işleyicisinin üst kısmında, yeni **InitNotificationsAsync** yöntemine aşağıdaki çağrıyı ekleyin:

        InitNotificationsAsync();

    Bu, uygulama her başlatıldığında kanal URI'sinin bildirim hub'ınıza kaydedilmesini garanti eder.

6. Çözüm Gezgini'nde, Windows Mağazası uygulamasının **Package.appxmanifest** öğesine çift tıklayın ve ardından **Bildirimler**'de **Bildirim özellikli** seçeneğini **Evet** olarak ayarlayın:

    ![][18]

    **Dosya** menüsünden **Tümünü Kaydet**'e tıklayın.

7. (İsteğe bağlı) Windows Phone Mağazası uygulama projesinde önceki adımı yineleyin.

8. Uygulamayı çalıştırmak için **F5** tuşuna basın. Kayıt anahtarını içeren bir açılır iletişim kutusu görüntülenir.

    ![][19]

9. (İsteğe bağlı) Uygulamayı bir Windows Phone cihazına kaydetmek üzere Windows Phone projesini çalıştırmak için önceki adımı yineleyin.



Uygulamanız şimdi bildirim almaya hazırdır.

##Bildirim gönderme 

Aşağıdaki ekranda gösterildiği gibi, bildirim hub'ındaki hata ayıklama sekmesi aracılığıyla [Klasik Azure Portalı]'nda bildirim göndererek uygulamanızda bildirim almayı test edebilirsiniz.

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-debug.png)

Anında iletme bildirimleri normalde, uyumlu bir kitaplık kullanılarak Mobile Services veya ASP.NET gibi bir arka uç hizmetinde gönderilir. Arka ucunuz için uygun bir kitaplık yoksa bildirim iletilerini göndermek için doğrudan REST API de kullanabilirsiniz. 

Bu öğreticide konuyu basit bir şekilde işleyeceğiz ve yalnızca bir arka uç hizmeti yerine bir konsol uygulamasındaki bildirim hub'ları için .NET SDK ile bildirim göndererek istemci uygulamanızı test etmeyi göstereceğiz. Bir ASP.NET arka ucundan bildirim göndermek için sonraki adım olarak [Kullanıcılara anında iletme bildirimleri göndermek için Notification Hubs’ı kullanma] öğreticisini öneririz. Bununla birlikte, bildirim göndermek için aşağıdaki yaklaşımlar kullanılabilir:

* **REST Arabirimi**: [REST arabirimini](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx) kullanarak herhangi bir arka uç platformunda bildirimi destekleyebilirsiniz.

* **Microsoft Azure Notification Hubs .NET SDK'sı**: Visual Studio için Nuget Paket Yöneticisi'nde [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) komutunu çalıştırın.

* **Node.js**: [Node.js'den Notification Hubs'ı kullanma](notification-hubs-nodejs-how-to-use-notification-hubs.md)

* [Azure Mobile Apps](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md): Notification Hubs ile tümleştirilmiş Azure Mobile Apps arka ucundan nasıl bildirim gönderildiğinin bir örneği için bkz. **Mobile Apps için anında iletme bildirimleri ekleme**.

* **Java/PHP**: REST API'ler kullanarak nasıl bildirim gönderildiğinin bir örneği için bkz. "Java/PHP'den Notification Hubs'ı kullanma"([Java](notification-hubs-java-backend-how-to.md) | [PHP](notification-hubs-php-backend-how-to.md)).


## (İsteğe bağlı) Konsol uygulamasından bildirim gönderme


.NET konsol uygulaması kullanarak bildirim göndermek için aşağıdaki adımları izleyin. 

1. Çözüme sağ tıklayın, **Ekle**'yi ve **Yeni Proje...** seçeneğini belirleyin. Sonra **Visual C#** altında **Windows**'a ve **Konsol Uygulaması**'na tıklayın ve ardından **Tamam**'a tıklayın.

    ![][13]

    Bu, çözüme yeni bir Visual C# konsol uygulaması ekler. Bunu ayrı bir çözümde de yapabilirsiniz.

2. Visual Studio'da **Araçlar**'a, **NuGet Paket Yöneticisi**'ne ve ardından **Paket Yöneticisi Konsolu**'na tıklayın.

    Bu, Visual Studio'da Paket Yöneticisi Konsolu'nu görüntüler.

3. Paket Yöneticisi Konsolu penceresinde, **Varsayılan projeyi** yeni konsol uygulaması projeniz olarak ayarlayın ve ardından konsol penceresinde aşağıdaki komutu yürütün:

        Install-Package Microsoft.Azure.NotificationHubs

    Bu, <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet paketini</a> kullanarak Azure Notification Hubs SDK'sına bir başvuru ekler.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)


4. Program.cs dosyasını açın ve aşağıdaki `using` deyimini ekleyin:

        using Microsoft.Azure.NotificationHubs;

5. **Program** sınıfına şu yöntemi ekleyin:

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">Hello from a .NET App!</text></binding></visual></toast>";
            await hub.SendWindowsNativeNotificationAsync(toast);
        }

    "Hub name" yer tutucusunu, **Notification Hubs** sekmesindeki [Klasik Azure Portalı]'nda görünen bildirim hub'ı adı ile değiştirdiğinizden emin olun. Ayrıca, bağlantı dizesi yer tutucusunu, "Bildirim hub'ınızı yapılandırma" bölümünde edindiğiniz **DefaultFullSharedAccessSignature** adlı bağlantı dizesi ile değiştirin.

    >[AZURE.NOTE]**Dinleme** erişimi olan değil, **Tam** erişimi olan bağlantı dizesini kullandığınızdan emin olun. Dinleme erişimi dizesinin bildirim gönderme izinleri yoktur.

6. Aşağıdaki satırları **Main** yöntemine ekleyin:

         SendNotificationAsync();
         Console.ReadLine();

7. Visual Studio'da konsol uygulaması projesine sağ tıklayın ve bunu başlangıç projesi olarak ayarlamak için **Başlangıç Projesi Olarak Ayarla**'ya tıklayın. Ardından uygulamayı çalıştırmak için **F5** tuşuna basın.

    ![][14]

    Tüm kayıtlı cihazlarda bildirim alırsınız. Bildirim başlığına tıklamak veya dokunmak uygulamayı yükler.

MSDN'deki [bildirim kataloğu], [kutucuk kataloğu] ve [göstergeye genel bakış] konu başlıklarında tüm desteklenen yükleri bulabilirsiniz.

##Sonraki adımlar

Bu basit örnekte, portalı veya konsol uygulamasını kullanarak tüm Windows cihazlarınıza yayın bildirimleri gönderdiniz. Sonraki adım olarak [Kullanıcılara anında iletme bildirimleri göndermek için Notification Hubs’ı kullanma] öğreticisini öneririz. Bu, belirli kullanıcıları hedeflemek için etiketler kullanarak ASP.NET arka ucundan nasıl bildirim göndereceğinizi gösterir.

Kullanıcılarınızı ilgi alanı gruplarına göre segmentlere ayırmak istiyorsanız bkz. [Son dakika haberleri göndermek için Notification Hubs kullanma]. 

Notification Hubs hakkında daha fazla genel bilgi için bkz. [Notification Hubs Kılavuzu].






<!-- Images. -->
[2]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-windows-universal-app.png
[3]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-associate-win8-app.png
[4]: ./media/notification-hubs-windows-store-dotnet-get-started/mobile-services-select-app-name.png
[6]: ./media/notification-hubs-windows-store-dotnet-get-started/mobile-services-win8-app-push-auth.png
[11]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png
[13]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-console-app.png
[14]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-toast.png
[15]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-scheduler1.png
[16]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-scheduler2.png
[18]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-win8-app-toast.png
[19]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-reg.png
[20]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-universal-app-install-package.png

<!-- URLs. -->
[Klasik Azure Portalı]: https://manage.windowsazure.com/
[Notification Hubs Kılavuzu]: http://msdn.microsoft.com/library/jj927170.aspx

[Kullanıcılara anında iletme bildirimleri göndermek için Notification Hubs’ı kullanma]: notification-hubs-aspnet-backend-windows-dotnet-notify-users.md
[on dakika haberleri göndermek için Notification Hubs kullanma]: notification-hubs-windows-store-dotnet-send-breaking-news.md

[bildirim kataloğu]: http://msdn.microsoft.com/library/windows/apps/hh761494.aspx
[kutucuk kataloğu]: http://msdn.microsoft.com/library/windows/apps/hh761491.aspx
[göstergeye genel bakış]: http://msdn.microsoft.com/library/windows/apps/hh779719.aspx



<!----HONumber=Jun16_HO2-->


