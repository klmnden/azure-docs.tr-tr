---
title: "Azure Notification Hubs kullanıcılara bildirme .NET arka ucu ile"
description: "Azure'da güvenli anında iletme bildirimleri göndermek öğrenin. .NET API kullanarak C# dilinde yazılan kod örnekleri."
documentationcenter: windows
author: ysxu
manager: erikre
services: notification-hubs
editor: 
ms.assetid: 012529f2-fdbc-43c4-8634-2698164b5880
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: c0b963ef661612b1a176dd8e5f01d56e61eb5acb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-notification-hubs-notify-users-with-net-backend"></a>Azure Notification Hubs kullanıcılara bildirme .NET arka ucu ile
[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

## <a name="overview"></a>Genel Bakış
Azure'da anında iletme bildirimi desteği, kullanımı kolay, multiplatform ve mobil platformlar için tüketici ve kurumsal uygulama için anında iletme bildirimleri uyarlamasını büyük ölçüde basitleştirir ölçeklendirilmiş gönderim altyapısı erişmenize olanak tanır. Bu öğreticide, belirli bir cihazdaki belirli bir uygulama kullanıcısına anında iletme bildirimleri göndermek için Azure Bildirim Hub'larını nasıl kullanacağınız gösterilir. Bir ASP.NET Webapı arka uç, istemcilerin kimliğini doğrulamak için kullanılır. Kimliği doğrulanmış istemci kullanıcı ve etiketi otomatik olarak eklenir arka ucu tarafından bildirimi kaydı için kullanma. Bu etiketi arka ucu tarafından belirli bir kullanıcı için bildirimler üretebilir göndermek için kullanılır. Bir uygulama arka ucu kullanarak bildirimleri için kaydediliyor daha fazla bilgi için Kılavuzu konusuna [uygulama arka ucunuzdan kaydetme](http://msdn.microsoft.com/library/dn743807.aspx). Bu öğreticide oluşturduğunuz proje ve bildirim hub'ı derlemeler [Notification Hubs ile çalışmaya başlama] Öğreticisi.

Bu öğreticinin ayrıca için ön koşuldur [güvenli anında] Öğreticisi. Bu öğreticide adımları tamamladıktan sonra geçebilirsiniz [güvenli anında] kodunun güvenli bir şekilde bir anında iletme bildirimi göndermek için Bu öğreticide nasıl değiştirileceğini gösterir öğretici.

## <a name="before-you-begin"></a>Başlamadan önce
Geri bildiriminizi ciddiye alacağız. Bu konu başlığını tamamlamakta veya bu içeriğin geliştirilmesi için önerilerde bir güçlükle karşılaşırsanız, sayfanın sonundaki geri bildiriminize değer vermekteyiz.

Bu öğreticinin tamamlanan kodu GitHub'da [buradan](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers) bulunabilir. 

## <a name="prerequisites"></a>Ön koşullar
Bu öğretici başlamadan önce zaten bu Mobile Services öğreticileri tamamlamış olmanız gerekir:

* [Notification Hubs ile çalışmaya başlama]<br/>Uygulama adı ayrılmaya bildirim hub'ınızı oluşturun ve Bu öğreticide bildirimleri almak için kaydolun. Bu öğreticide adımları önceden tamamlamış varsayar. Aksi durumda, lütfen adımları [bildirim hub'ları (Windows mağazası) ile çalışmaya başlama](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md); özellikle, bölümler [Windows mağazası uygulamanızı kaydetme](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#register-your-app-for-the-windows-store) ve [bildirim hub'ınızı yapılandırma](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#configure-your-notification-hub). Özellikle, girdiğinizden emin olun **paket SID'si** ve **gizli** değerleri Portalı'nda **yapılandırma** bildirim hub'ınız için sekmesi. Bu yapılandırma yordamı bölümünde açıklanan [bildirim hub'ınızı yapılandırma](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#configure-your-notification-hub). Bu önemli bir adımdır: Portal kimlik bilgileri için uygulama adı belirtilen eşleşmiyorsa seçerseniz seçin, anında iletme bildirimi başarılı olmaz.

> [!NOTE]
> Arka uç hizmetinizin Azure App Service'de Mobile Apps kullanıyorsanız, bkz: [Mobile Apps sürüm](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md) Bu öğreticinin.
> 
> 

&nbsp;

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="update-the-code-for-the-client-project"></a>İstemci proje kodunu güncelleştirin
Bu bölümde, için tamamlandı projesinde kodu güncelleştirme [Notification Hubs ile çalışmaya başlama] Öğreticisi. Zaten deposuyla ilişkili ve gerekir bildirim hub'ınız için yapılandırılmış. Bu bölümde, yeni Webapı arka uç çağırın ve kaydetme ve bildirimleri göndermek için kullanmak üzere kod ekleyeceksiniz.

1. Visual Studio'da açmak için oluşturduğunuz çözüm [Notification Hubs ile çalışmaya başlama] Öğreticisi.
2. Çözüm Gezgini'nde sağ **(Windows 8.1)** proje ve ardından **NuGet paketlerini Yönet**.
3. Sol taraftaki tıklatın **çevrimiçi**.
4. İçinde **arama** kutusuna **Http istemcisi**.
5. Sonuçlar listesinde tıklatın **Microsoft HTTP istemci kitaplıkları**ve ardından **yükleme**. Yüklemeyi tamamlayın.
6. NuGet edilene **arama** kutusuna **Json.net**. Yükleme **Json.NET** paketini ve sonra da NuGet Paket Yöneticisi penceresini kapatın.
7. Yukarıdaki adımları yineleyin **(Windows Phone 8.1)** yüklemek için proje **JSON.NET** Windows Phone projesi için NuGet paketi.
8. Çözüm Gezgini'nde, içinde **(Windows 8.1)** projesi, çift **MainPage.xaml** Visual Studio düzenleyicisinde açın.
9. İçinde **MainPage.xaml** XML kodunu değiştir `<Grid>` bölümü aşağıdaki kod ile. Bu kod ile kullanıcı kimliğini doğrulayacak kullanıcı adı ve parola metin kutusu ekler. Ayrıca, bildirim iletisini ve bildirimi alması gereken kullanıcı adı etiketi için metin kutuları ekler:
   
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
   
            <TextBlock Grid.Row="0" Text="Notify Users" HorizontalAlignment="Center" FontSize="48"/>
   
            <StackPanel Grid.Row="1" VerticalAlignment="Center">
                <Grid>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition></ColumnDefinition>
                        <ColumnDefinition></ColumnDefinition>
                        <ColumnDefinition></ColumnDefinition>
                    </Grid.ColumnDefinitions>
                    <TextBlock Grid.Row="0" Grid.ColumnSpan="3" Text="Username" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="UsernameTextBox" Grid.Row="1" Grid.ColumnSpan="3" Margin="20,0,20,0"/>
                    <TextBlock Grid.Row="2" Grid.ColumnSpan="3" Text="Password" FontSize="24" Margin="20,0,20,0" />
                    <PasswordBox Name="PasswordTextBox" Grid.Row="3" Grid.ColumnSpan="3" Margin="20,0,20,0"/>
   
                    <Button Grid.Row="4" Grid.ColumnSpan="3" HorizontalAlignment="Center" VerticalAlignment="Center"
                                Content="1. Login and register" Click="LoginAndRegisterClick" Margin="0,0,0,20"/>
   
                    <ToggleButton Name="toggleWNS" Grid.Row="5" Grid.Column="0" HorizontalAlignment="Right" Content="WNS" IsChecked="True" />
                    <ToggleButton Name="toggleGCM" Grid.Row="5" Grid.Column="1" HorizontalAlignment="Center" Content="GCM" />
                    <ToggleButton Name="toggleAPNS" Grid.Row="5" Grid.Column="2" HorizontalAlignment="Left" Content="APNS" />
   
                    <TextBlock Grid.Row="6" Grid.ColumnSpan="3" Text="Username Tag To Send To" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="ToUserTagTextBox" Grid.Row="7" Grid.ColumnSpan="3" Margin="20,0,20,0" TextWrapping="Wrap" />
                    <TextBlock Grid.Row="8" Grid.ColumnSpan="3" Text="Enter Notification Message" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="NotificationMessageTextBox" Grid.Row="9" Grid.ColumnSpan="3" Margin="20,0,20,0" TextWrapping="Wrap" />
                    <Button Grid.Row="10" Grid.ColumnSpan="3" HorizontalAlignment="Center" Content="2. Send push" Click="PushClick" Name="SendPushButton" />
                </Grid>
            </StackPanel>
        </Grid>
10. Çözüm Gezgini'nde, içinde **(Windows Phone 8.1)** proje, açık **MainPage.xaml** ve Windows Phone 8.1 yerine `<Grid>` yukarıdaki aynı kodu ile bölüm. Arabirimi aşağıda gösterilen nedir benzer görünmelidir.
    
    ![][13]
11. Çözüm Gezgini'nde açık **MainPage.xaml.cs** dosya **(Windows 8.1)** ve **(Windows Phone 8.1)** projeleri. Aşağıdakileri ekleyin `using` deyimlerini dosyalar:
    
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Windows.Networking.PushNotifications;
        using Windows.UI.Popups;
        using System.Threading.Tasks;
12. İçinde **MainPage.xaml.cs** için **(Windows 8.1)** ve **(Windows Phone 8.1)** projeler eklemek için aşağıdaki üye `MainPage` sınıfı. Değiştirdiğinizden emin olun `<Enter Your Backend Endpoint>` ile gerçek arka uç noktanızı elde daha önce. Örneğin, `http://mybackend.azurewebsites.net`.
    
        private static string BACKEND_ENDPOINT = "<Enter Your Backend Endpoint>";
13. MainPage sınıfında aşağıdaki kodu ekleyin **MainPage.xaml.cs** için **(Windows 8.1)** ve **(Windows Phone 8.1)** projeleri.
    
    `PushClick` Yöntemdir click işleyicisi **Gönder anında** düğmesi. Eşleşen bir kullanıcı adı etiketi ile tüm cihazlar için bir bildirim tetiklemek için arka uç çağırır `to_tag` parametresi. Bildirim iletisi, istek gövdesinde JSON içeriği olarak gönderilir.
    
    `LoginAndRegisterClick` Yöntemdir click işleyicisi **oturum açma ve kaydetme** düğmesi. Temel depolar (Bu, kimlik doğrulama şemasını kullanan herhangi bir belirteci temsil ettiğini unutmayın), Yerel Depodaki kimlik doğrulama belirteci sonra kullanır `RegisterClient` arka ucu kullanarak bildirimleri için kaydedilecek.

        private async void PushClick(object sender, RoutedEventArgs e)
        {
            if (toggleWNS.IsChecked.Value)
            {
                await sendPush("wns", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);
            }
            if (toggleGCM.IsChecked.Value)
            {
                await sendPush("gcm", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);
            }
            if (toggleAPNS.IsChecked.Value)
            {
                await sendPush("apns", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);

            }
        }

        private async Task sendPush(string pns, string userTag, string message)
        {
            var POST_URL = BACKEND_ENDPOINT + "/api/notifications?pns=" +
                pns + "&to_tag=" + userTag;

            using (var httpClient = new HttpClient())
            {
                var settings = ApplicationData.Current.LocalSettings.Values;
                httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                try
                {
                    await httpClient.PostAsync(POST_URL, new StringContent("\"" + message + "\"",
                        System.Text.Encoding.UTF8, "application/json"));
                }
                catch (Exception ex)
                {
                    MessageDialog alert = new MessageDialog(ex.Message, "Failed to send " + pns + " message");
                    alert.ShowAsync();
                }
            }
        }

        private async void LoginAndRegisterClick(object sender, RoutedEventArgs e)
        {
            SetAuthenticationTokenInLocalStorage();

            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            // The "username:<user name>" tag gets automatically added by the message handler in the backend.
            // The tag passed here can be whatever other tags you may want to use.
            try
            {
                // The device handle used will be different depending on the device and PNS. 
                // Windows devices use the channel uri as the PNS handle.
                await new RegisterClient(BACKEND_ENDPOINT).RegisterAsync(channel.Uri, new string[] { "myTag" });

                var dialog = new MessageDialog("Registered as: " + UsernameTextBox.Text);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
                SendPushButton.IsEnabled = true;
            }
            catch (Exception ex)
            {
                MessageDialog alert = new MessageDialog(ex.Message, "Failed to register with RegisterClient");
                alert.ShowAsync();
            }
        }

        private void SetAuthenticationTokenInLocalStorage()
        {
            string username = UsernameTextBox.Text;
            string password = PasswordTextBox.Password;

            var token = Convert.ToBase64String(System.Text.Encoding.UTF8.GetBytes(username + ":" + password));
            ApplicationData.Current.LocalSettings.Values["AuthenticationToken"] = token;
        }



1. Çözüm Gezgini'nde, altında **paylaşılan** proje, açık **App.xaml.cs** dosya. Çağrı Bul `InitNotificationsAsync()` içinde `OnLaunched()` olay işleyicisi. Out yorum yapmak veya çağrısı delete `InitNotificationsAsync()`. Yukarıda eklediğiniz düğme işleyicisi bildirim kayıtlar başlatacak.

        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
            //InitNotificationsAsync();


1. Çözüm Gezgini'nde sağ **paylaşılan** proje ve ardından **Ekle**ve ardından **sınıfı**. Sınıf adını **RegisterClient.cs**, ardından **Tamam** sınıfı oluşturmak için.
   
   Bu sınıf için anında iletme bildirimleri kaydetmek için uygulama arka ucu başvurmak için gereken REST çağrılarını sarmalar. Ayrıca yerel olarak depoladığı *registrationIds* bildirim hub'ı içinde ayrıntılı olarak tarafından oluşturulan [uygulama arka ucunuzdan kaydetme](http://msdn.microsoft.com/library/dn743807.aspx). Tıkladığınızda yerel depolamada depolanan bir yetki belirteci kullandığına dikkat edin **oturum açma ve kaydetme** düğmesi.
2. Aşağıdakileri ekleyin `using` RegisterClient.cs dosyanın en üstüne deyimlerini:
   
       using Windows.Storage;
       using System.Net;
       using System.Net.Http;
       using System.Net.Http.Headers;
       using Newtonsoft.Json;
       using System.Threading.Tasks;
       using System.Linq;
3. Aşağıdaki kodu `RegisterClient` sınıf tanımına ekleyin.
   
       private string POST_URL;
   
       private class DeviceRegistration
       {
           public string Platform { get; set; }
           public string Handle { get; set; }
           public string[] Tags { get; set; }
       }
   
       public RegisterClient(string backendEndpoint)
       {
           POST_URL = backendEndpoint + "/api/register";
       }
   
       public async Task RegisterAsync(string handle, IEnumerable<string> tags)
       {
           var regId = await RetrieveRegistrationIdOrRequestNewOneAsync();
   
           var deviceRegistration = new DeviceRegistration
           {
               Platform = "wns",
               Handle = handle,
               Tags = tags.ToArray<string>()
           };
   
           var statusCode = await UpdateRegistrationAsync(regId, deviceRegistration);
   
           if (statusCode == HttpStatusCode.Gone)
           {
               // regId is expired, deleting from local storage & recreating
               var settings = ApplicationData.Current.LocalSettings.Values;
               settings.Remove("__NHRegistrationId");
               regId = await RetrieveRegistrationIdOrRequestNewOneAsync();
               statusCode = await UpdateRegistrationAsync(regId, deviceRegistration);
           }
   
           if (statusCode != HttpStatusCode.Accepted)
           {
               // log or throw
               throw new System.Net.WebException(statusCode.ToString());
           }
       }
   
       private async Task<HttpStatusCode> UpdateRegistrationAsync(string regId, DeviceRegistration deviceRegistration)
       {
           using (var httpClient = new HttpClient())
           {
               var settings = ApplicationData.Current.LocalSettings.Values;
               httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string) settings["AuthenticationToken"]);
   
               var putUri = POST_URL + "/" + regId;
   
               string json = JsonConvert.SerializeObject(deviceRegistration);
                               var response = await httpClient.PutAsync(putUri, new StringContent(json, Encoding.UTF8, "application/json"));
               return response.StatusCode;
           }
       }
   
       private async Task<string> RetrieveRegistrationIdOrRequestNewOneAsync()
       {
           var settings = ApplicationData.Current.LocalSettings.Values;
           if (!settings.ContainsKey("__NHRegistrationId"))
           {
               using (var httpClient = new HttpClient())
               {
                   httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);
   
                   var response = await httpClient.PostAsync(POST_URL, new StringContent(""));
                   if (response.IsSuccessStatusCode)
                   {
                       string regId = await response.Content.ReadAsStringAsync();
                       regId = regId.Substring(1, regId.Length - 2);
                       settings.Add("__NHRegistrationId", regId);
                   }
                   else
                   {
                       throw new System.Net.WebException(response.StatusCode.ToString());
                   }
               }
           }
           return (string)settings["__NHRegistrationId"];
   
       }
4. Yaptığınız tüm değişiklikleri kaydedin.

## <a name="testing-the-application"></a>Uygulamayı test etme
1. Windows 8.1 ve Windows Phone 8.1 Uygulamayı başlatın. Windows Phone 8.1 için öykünücüyü veya gerçek bir cihaza örneği çalıştırabilirsiniz.
2. Uygulamayı Windows 8.1 örneğinde girin bir **kullanıcıadı** ve **parola** aşağıdaki ekranda gösterildiği gibi. Kullanıcı adı ve parola Windows Phone üzerinde farklı olmalıdır.
3. Tıklatın **oturum açma ve kaydetme** ve bir iletişim kutusu gösterir oturum olduğunu doğrulayın. Bu da etkinleştirir **Gönder anında** düğmesi.
   
    ![][14]
4. Windows Phone 8.1 örneği, bir kullanıcı adı dizesi girmeniz **kullanıcıadı** ve **parola** alanları ardından **oturum açma ve kaydetme**.
5. Ardından **alıcı kullanıcı adı etiketi** alan, Windows 8.1 kayıtlı kullanıcı adını girin. Bir bildirim iletisi girin ve tıklayın **Gönder anında**.
   
    ![][16]
6. Eşleşen kullanıcı adı etiketi ile kaydettiğiniz cihazları bildirim iletisini alırsınız.
   
    ![][15]

## <a name="next-steps"></a>Sonraki Adımlar
* Kullanıcılarınızı ilgi alanı gruplarına göre segmentlere ayırmak istiyorsanız bkz. [Son dakika haberleri göndermek için Notification Hubs kullanma].
* Notification Hubs kullanma hakkında daha fazla bilgi için bkz: [Notification Hubs Kılavuzu].

[9]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push9.png
[10]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push10.png
[11]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push11.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-ui.png
[14]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-windows-instance-username.png
[15]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-notification-received.png
[16]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-send-message.png



<!-- URLs. -->
[Notification Hubs ile çalışmaya başlama]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[güvenli anında]: notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md
[Son dakika haberleri göndermek için Notification Hubs kullanma]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Notification Hubs Kılavuzu]: http://msdn.microsoft.com/library/jj927170.aspx
