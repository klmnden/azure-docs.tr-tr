<properties
    pageTitle="Windows Evrensel Platform Uygulamaları için Azure Notification Hubs ile çalışmaya başlama | Microsoft Azure"
    description="Bu öğreticide, bir Windows Evrensel Platform uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını öğrenirsiniz."
    services="notification-hubs"
    documentationCenter="windows"
    authors="wesmc7777"
    manager="erikre"
    editor="erikre"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="07/21/2016"
    ms.author="wesmc"/>


# Windows Evrensel Platform Uygulamaları için Notification Hubs'ı kullanmaya başlama

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##Genel Bakış

Bu öğretici, bir Windows Evrensel Platform (UWP) uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını size gösterir.

Bu öğreticide, Microsoft Anında İletme Bildirimi Hizmeti'ni (WNS) kullanarak anında iletme bildirimleri alan boş bir Windows Mağazası uygulaması oluşturursunuz. İşiniz bittiğinde, uygulamanızı çalıştıran tüm cihazlara anında iletme bildirimleri yayımlamak için bildirim hub'ınızı kullanabileceksiniz.


## Başlamadan önce

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Bu öğreticinin tamamlanan kodu GitHub'da [buradan](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal) bulunabilir.



##Önkoşullar

Bu öğretici için aşağıdakiler gereklidir:

+ [Microsoft Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs) veya üstü

+ [Evrensel Windows Uygulama Geliştirme Araçları’nın yüklü olması](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up)

+ Etkin bir Azure hesabı <br/>Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-store-dotnet-get-started%2F).

+ Etkin bir Windows Mağazası hesabı



Bu öğreticinin tamamlanması Windows Evrensel Platform uygulamalarına ilişkin diğer tüm Notification Hubs öğreticileri için önkoşuldur.

##Uygulamanızı Windows Mağazası'na kaydetme

UWP uygulamalarına anında iletme bildirimleri göndermek için uygulamanızı Windows Mağazası ile ilişkilendirmeniz gerekir. Daha sonra, WNS ile tümleştirmek için bildirim hub'ınızı yapılandırmanız gerekir.

1. Uygulamanızı henüz kaydetmediyseniz [Windows Geliştirme Merkezi](https://dev.windows.com/overview)'ne gidin, Microsoft hesabınızla oturum açın ve ardından **Yeni uygulama oluştur**'a tıklayın.


2. Uygulamanız için bir ad yazın ve **Uygulama adını ayır**'a tıklayın.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hubs-win8-app-name.png)

    Bu, uygulamanız için yeni bir Windows Mağazası kaydı oluşturur.

3. Visual Studio'da, **Boş Uygulama** şablonunu kullanarak yeni bir Visual C# Mağaza Uygulamaları projesi oluşturun ve **Tamam**’a tıklayın.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-windows-universal-app.png)

4. Hedef ve en düşük platform sürümleri için varsayılan değerleri kabul edin.


5. Çözüm Gezgini'nde, Windows Mağazası uygulama projesine sağ tıklayın, **Mağaza**'ya ve ardından **Uygulamayı Mağaza ile ilişkilendir...** seçeneğine tıklayın.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-associate-win8-app.png)


    **Uygulamanızı Windows Mağazası ile ilişkilendirin** sihirbazı görüntülenir.

6. Sihirbazda **Oturum aç**'a tıklayın ve ardından Microsoft hesabınızla oturum açın.

7. 2 adımda kaydettiğiniz uygulamaya tıklayın, **İleri**'ye ve ardından **İlişkilendir**'e tıklayın.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-associate-app-name.png)

    Bu, uygulama bildirimine gerekli Windows Mağazası kayıt bilgilerini ekler.

8. Yeni uygulamanız için [Windows Geliştirme Merkezi](http://go.microsoft.com/fwlink/p/?LinkID=266582) sayfasına geri dönün, **Hizmetler**'e ve **Anında iletme bildirimleri**'ne tıklayın. Ardından, **Windows Anında Bildirim Hizmetleri (WNS) ve Microsoft Azure Mobile Apps** altındaki **Live Services** sitesine tıklayın.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hubs-uwp-app-live-services.png)

9. Uygulamanızın kayıt sayfasında **Uygulama Gizli Anahtarı** parolasını ve **Windows Mağazası** platform ayarları altındaki **Paket güvenliği tanımlayıcısı (SID)** değerini not edin.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hubs-uwp-app-push-auth.png)


    > [AZURE.WARNING]
    Uygulama gizli anahtarı ve paket SID'si önemli güvenlik kimlik bilgileridir. Bu değerleri kimseyle paylaşmayın veya uygulamanızla birlikte dağıtmayın.

##Bildirim hub'ınızı yapılandırma

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p><b>Bildirim Hizmetleri</b> seçeneğini <b>Windows (WNS)</b> seçeneğini belirleyin. Ardından <b>Güvenlik Anahtarı</b> alanına <b>Uygulama gizli anahtarı</b> parolasını girin. Önceki bölümde WNS’den edindiğiniz <b>Paket SID’si</b> değerini girin ve ardından <b>Kaydet</b>’e tıklayın.</p>
</li>
</ol>

&emsp;&emsp;![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)


Bildirim hub'ınız şimdi WNS ile birlikte çalışmak üzere yapılandırıldı. Ayrıca, uygulamanızı kaydetmenizi ve bildirim göndermenizi sağlayan bağlantı dizelerine sahipsiniz.

##Uygulamanızı bildirim hub'ına bağlama

1. Visual Studio'da çözüme sağ tıklayın ve ardından **NuGet Paketlerini Yönet**'e tıklayın.

    Bu, **NuGet Paketlerini Yönet** iletişim kutusunu görüntüler.

2. `WindowsAzure.Messaging.Managed` için arama yapın ve **Yükle**'ye tıklayın, ardından kullanım koşullarını kabul edin.

    ![][20]

    Bu işlem <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">WindowsAzure.Messaging.Managed NuGet paketini</a> kullanarak Windows için Azure Mesajlaşma kitaplığına bir başvuruyu indirir, ekler ve yükler.

3. App.xaml.cs dosyasını açın ve aşağıdaki `using` deyimleri ekleyin. 

        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.UI.Popups;



4. Ayrıca App.xaml.cs dosyasında, **Uygulama** sınıfına aşağıdaki **InitNotificationsAsync** yöntem tanımını ekleyin:

        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            var hub = new NotificationHub("< your hub name>", "<Your DefaultListenSharedAccessSignature connection string>");
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

    >[AZURE.NOTE] “Hub adınız” yer tutucusunu Azure Portal’da görünen anında bildirim hub'ının adı ile değiştirdiğinizden emin olun. Ayrıca, bağlantı dizesi yer tutucusunu önceki bölümde yer alan Notification Hub **Erişim İlkeleri** sayfasında edindiğiniz **DefaultListenSharedAccessSignature** bağlantı dizesi ile değiştirin.

5. App.xaml.cs dosyasındaki **OnLaunched** olay işleyicisinin üst kısmında, yeni **InitNotificationsAsync** yöntemine aşağıdaki çağrıyı ekleyin:

        InitNotificationsAsync();

    Bu, uygulama her başlatıldığında kanal URI'sinin bildirim hub'ınıza kaydedilmesini garanti eder.

6. Uygulamayı çalıştırmak için **F5** tuşuna basın. Kayıt anahtarını içeren bir açılır iletişim kutusu görüntülenir.

    ![][19]




Uygulamanız şimdi bildirim almaya hazırdır.

##Bildirim gönderme 

Aşağıdaki ekranda gösterildiği [Azure Portal](https://portal.azure.com/)'da bildirim hub’ındaki **Test Gönderimi** düğmesini kullanarak uygulamanızda bildirim almayı hızlıca test edebilirsiniz.

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)

Anında iletme bildirimleri normalde, uyumlu bir kitaplık kullanılarak Mobile Services veya ASP.NET gibi bir arka uç hizmetinde gönderilir. Arka ucunuz için uygun bir kitaplık yoksa bildirim iletilerini göndermek için doğrudan REST API de kullanabilirsiniz. 

Bu öğreticide konuyu basit bir şekilde işleyeceğiz ve yalnızca bir arka uç hizmeti yerine bir konsol uygulamasındaki bildirim hub'ları için .NET SDK ile bildirim göndererek istemci uygulamanızı test etmeyi göstereceğiz. Bir ASP.NET arka ucundan bildirim göndermek için sonraki adım olarak [Kullanıcılara anında iletme bildirimleri göndermek için Notification Hubs’ı kullanma] öğreticisini öneririz. Bununla birlikte, bildirim göndermek için aşağıdaki yaklaşımlar kullanılabilir:

* **REST Arabirimi**: [REST arabirimini](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx) kullanarak herhangi bir arka uç platformunda bildirimi destekleyebilirsiniz.

* **Microsoft Azure Notification Hubs .NET SDK'sı**: Visual Studio için Nuget Paket Yöneticisi'nde [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) komutunu çalıştırın.

* **Node.js**: [Node.js'den Notification Hubs'ı kullanma](notification-hubs-nodejs-push-notification-tutorial.md).

* [Azure Mobile Apps](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md): Notification Hubs ile tümleştirilmiş Azure Mobile Apps arka ucundan nasıl bildirim gönderildiğinin bir örneği için bkz. **Mobile Apps için anında iletme bildirimleri ekleme**.

* **Java/PHP**: REST API'ler kullanarak nasıl bildirim gönderildiğinin bir örneği için bkz. "Java/PHP'den Notification Hubs'ı kullanma"([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).


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

    “Hub adı” yer tutucusunu Azure Portal’da görünen anında bildirim hub'ının adı ile değiştirdiğinizden emin olun. Ayrıca, bağlantı dizesi yer tutucusunu “Bildirim hub’ınızı yapılandırma” adlı bölümde yer alan Notification Hub **Erişim İlkeleri** sayfasında edindiğiniz **DefaultFullSharedAccessSignature** bağlantı dizesiyle değiştirin.

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

Notification Hubs hakkında daha fazla genel bilgi için bkz. [Notification Hubs Kılavuzu](notification-hubs-push-notification-overview.md).






<!-- Images. -->
[13]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-console-app.png
[14]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-toast.png
[19]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-reg.png
[20]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-universal-app-install-package.png

<!-- URLs. -->

[Kullanıcılara anında iletme bildirimleri göndermek için Notification Hubs’ı kullanma]: notification-hubs-aspnet-backend-windows-dotnet-notify-users.md
[Son dakika haberleri göndermek için Notification Hubs kullanma]: notification-hubs-windows-store-dotnet-send-breaking-news.md

[bildirim kataloğu]: http://msdn.microsoft.com/library/windows/apps/hh761494.aspx
[kutucuk kataloğu]: http://msdn.microsoft.com/library/windows/apps/hh761491.aspx
[göstergeye genel bakış]: http://msdn.microsoft.com/library/windows/apps/hh779719.aspx



<!--HONumber=Sep16_HO3-->


