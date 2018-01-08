---
title: "Windows Phone uygulamaları için Azure Notification Hubs'ı kullanmaya başlama | Microsoft Docs"
description: "Bu öğreticide, bir Windows Phone 8 veya Windows Phone 8.1 Silverlight uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını öğrenirsiniz."
services: notification-hubs
documentationcenter: windows
keywords: "anında iletme bildirimi,anında iletme bildirimi,windows phone anında iletme"
author: jwhitedev
manager: kpiteira
editor: 
ms.assetid: d872d8dc-4658-4d65-9e71-fa8e34fae96e
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 12/22/2017
ms.author: jawh
ms.openlocfilehash: 7d44d0a0f8683ad6ad55136ad17879e98e26498b
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="get-started-with-azure-notification-hubs-for-windows-phone-apps"></a>Windows Phone uygulamaları için Azure Notification Hubs'ı kullanmaya başlama
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Genel Bakış
> [!NOTE]
> Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F).
> 
> 

Bu öğretici, bir Windows Phone 8 veya Windows Phone 8.1 Silverlight uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını size gösterir. Windows Phone 8.1'i (Silverlight olmayan) hedefliyorsanız [Windows Evrensel](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) sürümüne bakın.
Bu öğreticide, Microsoft Anında İletme Bildirimi Hizmeti'ni (MPNS) kullanarak anında iletme bildirimleri alan boş bir Windows Phone 8 uygulaması oluşturursunuz. İşiniz bittiğinde, uygulamanızı çalıştıran tüm cihazlara anında iletme bildirimleri yayımlamak için bildirim hub'ınızı kullanabileceksiniz.

> [!NOTE]
> Notification Hubs Windows Phone SDK'sı, Windows Anında Bildirim Hizmeti'ni (WNS) Windows Phone 8.1 Silverlight uygulamaları ile kullanmayı desteklemez. WNS'yi (MPNS yerine) Windows Phone 8.1 Silverlight uygulamaları ile kullanmak için REST API'ler kullanan [Notification Hubs - Windows Phone Silverlight öğreticisi]'ni izleyin.
> 
> 

Bu öğretici, Notification Hubs kullanımında basit yayın senaryosunu gösterir.

## <a name="prerequisites"></a>Ön koşullar
Bu öğretici için aşağıdakiler gereklidir:

* [Windows Phone için Visual Studio 2012 Express] veya sonraki bir sürümü.

Bu öğreticiyi tamamlamak Windows Phone 8 uygulamalarına ilişkin diğer tüm Notification Hubs öğreticileri için önkoşuldur.

## <a name="create-your-notification-hub"></a>Bildirim hub'ınızı oluşturma
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p><b>Bildirim Hizmetleri</b> bölümünde <b>Windows Phone (MPNS)</b> öğesini seçip <b>Kimliği doğrulanmamış anında iletmeleri etkinleştir</b> onay kutusuna tıklayın.</p>
</li>
</ol>

&emsp;&emsp;![Azure portalı - Kimliği doğrulanmamış anında iletme bildirimlerini etkinleştirme](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

Hub'ınız şimdi oluşturuldu ve Windows Phone için kimliği doğrulanmamış bildirim göndermek üzere yapılandırıldı.

> [!NOTE]
> Bu öğretici, MPNS'yi kimlik doğrulamasız modda kullanır. MPNS kimlik doğrulamasız mod, her bir kanala gönderebileceğiniz bildirimlerle ilgili kısıtlamalara sahiptir. Notification Hubs, sertifikanızı karşıya yüklemenize olanak sağlayarak [MPNS kimlik doğrulamalı mod](http://msdn.microsoft.com/library/windowsphone/develop/ff941099.aspx)'u destekler.
> 
> 

## <a name="connecting-your-app-to-the-notification-hub"></a>Uygulamanızı bildirim hub'ına bağlama
1. Visual Studio'da yeni bir Windows Phone 8 uygulaması oluşturun.
   
       ![Visual Studio - New Project - Windows Phone App][13]
   
    Visual Studio 2013 Güncelleştirme 2 veya sonrasında, bunun yerine Windows Phone Silverlight uygulaması oluşturursunuz.
   
    ![Visual Studio - Yeni Proje - Boş Uygulama - Windows Phone Silverlight][11]
2. Visual Studio'da çözüme sağ tıklayın ve ardından **NuGet Paketlerini Yönet**'e tıklayın.
   
    Bu, **NuGet Paketlerini Yönet** iletişim kutusunu görüntüler.
3. `WindowsAzure.Messaging.Managed` için arama yapın ve **Yükle**'ye tıklayın, ardından kullanım koşullarını kabul edin.
   
    ![Visual Studio - NuGet Paket Yöneticisi][20]
   
    Bu işlem <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">WindowsAzure.Messaging.Managed NuGet paketini</a> kullanarak Windows için Azure Mesajlaşma kitaplığına bir başvuruyu indirir, ekler ve yükler.
4. App.xaml.cs dosyasını açın ve aşağıdaki `using` deyimlerini ekleyin:
   
        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
5. App.xaml.cs dosyasında **Application_Launching** yönteminin üzerine aşağıdaki kodu ekleyin:
   
        var channel = HttpNotificationChannel.Find("MyPushChannel");
        if (channel == null)
        {
            channel = new HttpNotificationChannel("MyPushChannel");
            channel.Open();
            channel.BindToShellToast();
        }
   
        channel.ChannelUriUpdated += new EventHandler<NotificationChannelUriEventArgs>(async (o, args) =>
        {
            var hub = new NotificationHub("<hub name>", "<connection string>");
            var result = await hub.RegisterNativeAsync(args.ChannelUri.ToString());
   
            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
            {
                MessageBox.Show("Registration :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
            });
        });
   
   > [!NOTE]
   > **MyPushChannel** değeri, [HttpNotificationChannel](https://msdn.microsoft.com/library/windows/apps/microsoft.phone.notification.httpnotificationchannel.aspx) koleksiyonunda var olan bir kanalı aramak için kullanılan bir dizindir. Bu koleksiyonda bir tane bulunmuyorsa bu adla yeni bir giriş oluşturun.
   > 
   > 
   
    Hub'ınızın adını ve önceki bölümde edindiğiniz **DefaultListenSharedAccessSignature** adlı bağlantı dizesini eklediğinizden emin olun.
    Bu kod, MPNS'den uygulamanın kanal URI'sini alır ve ardından bu kanal URI'sini bildirim hub'ınıza kaydeder. Bu kod ayrıca uygulama her başlatıldığında kanal URI'sinin bildirim hub'ınıza kaydedilmesini garanti eder.
   
   > [!NOTE]
   > Bu öğretici cihaza bir bildirim gönderir. Bir kutucuk bildirimi gönderdiğinizde, kanalda **BindToShellTile** yöntemini çağırmayı tercih etmeniz gerekir. Bildirim ve kutucuk bildirimlerinin her ikisini de desteklemek için **BindToShellTile** ve **BindToShellToast** yöntemlerini çağırın.
   > 
   > 
6. Çözüm Gezgini'nde **Özellikler**'i genişletin, `WMAppManifest.xml` dosyasını açın, **Özellikler** sekmesine tıklayın ve **ID_CAP_PUSH_NOTIFICATION** özelliğinin işaretlendiğinden emin olun.
   
       ![Visual Studio - Windows Phone App Capabilities][14]
   
       This ensures that your app can receive push notifications. Without it, any attempt to send a push notification to the app will fail.
7. Uygulamayı çalıştırmak için `F5` tuşuna basın.
   
    Uygulamada bir kayıt iletisi görüntülenir.
8. Uygulamayı kapatın.  
   
   > [!NOTE]
   > Bildirim biçiminde bir anında iletme bildirimi almak için uygulamanın ön planda çalışmıyor olması gerekir.
   >

## <a name="next-steps"></a>Sonraki adımlar
Bu basit örnekte, tüm Windows Phone 8 cihazlarınıza anında iletme bildirimleri yayımladınız. 

Belirli kullanıcıları hedeflemek için, [Kullanıcılara anında iletme bildirimleri göndermek için Notification Hubs’ı kullanma] öğreticisine bakın. 

Kullanıcılarınızı ilgi alanı gruplarına göre segmentlere ayırmak istiyorsanız [Son dakika haberleri göndermek için Notification Hubs kullanma]'yı okuyabilirsiniz. 

[Notification Hubs Kılavuzu]'nda Notification Hubs'ı nasıl kullanacağınız hakkında daha fazla bilgi edinin.

<!-- Images. -->
[6]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png
[7]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal.png
[8]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal2.png
[9]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal.png
[10]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal2.png
[11]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-silverlight-app.png
[12]: ./media/notification-hubs-windows-phone-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-app.png
[14]: ./media/notification-hubs-windows-phone-get-started/mobile-app-enable-push-wp8.png
[15]: ./media/notification-hubs-windows-phone-get-started/notification-hub-pushauth.png
[20]: ./media/notification-hubs-windows-phone-get-started/notification-hub-windows-universal-app-install-package.png
[213]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png





<!-- URLs. -->
[Windows Phone için Visual Studio 2012 Express]: https://go.microsoft.com/fwLink/p/?LinkID=268374
[Notification Hubs Kılavuzu]: http://msdn.microsoft.com/library/jj927170.aspx
[MPNS authenticated mode]: http://msdn.microsoft.com/library/windowsphone/develop/ff941099(v=vs.105).aspx
[Kullanıcılara anında iletme bildirimleri göndermek için Notification Hubs’ı kullanma]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Son dakika haberleri göndermek için Notification Hubs kullanma]: notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md
[toast catalog]: http://msdn.microsoft.com/library/windowsphone/develop/jj662938(v=vs.105).aspx
[tile catalog]: http://msdn.microsoft.com/library/windowsphone/develop/hh202948(v=vs.105).aspx
[Notification Hubs - Windows Phone Silverlight öğreticisi]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSLPhoneApp

