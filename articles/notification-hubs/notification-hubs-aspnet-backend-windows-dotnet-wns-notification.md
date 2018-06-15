---
title: Azure Notification Hubs kullanarak belirli kullanıcılara bildirim gönderme | Microsoft Docs
description: Evrensel Windows Platformu (UWP) uygulamalarını kullanarak belirli kullanıcılara bildirim göndermeyi öğrenin.
documentationcenter: windows
author: dimazaid
manager: kpiteira
editor: spelluru
services: notification-hubs
ms.assetid: 012529f2-fdbc-43c4-8634-2698164b5880
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/04/2018
ms.author: dimazaid
ms.openlocfilehash: ee0e78402515a733731e6faf21b09886674cda51
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33778251"
---
# <a name="tutorial-send-notifications-to-specific-users-by-using-azure-notification-hubs"></a>Öğretici: Azure Notification Hubs kullanarak belirli kullanıcılara bildirim gönderme
[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

## <a name="overview"></a>Genel Bakış
Bu öğreticide, belirli bir cihazdaki belirli bir uygulama kullanıcısına anında iletme bildirimleri göndermek için Azure Notification Hubs’ı nasıl kullanacağınız gösterilmektedir. İstemcilerin kimliğini doğrulamak için bir ASP.NET WebAPI arka ucu kullanılır. Arka uç bir istemci uygulama kullanıcısının kimliğini doğruladığında, bildirimi kaydına otomatik olarak bir etiket ekler. Arka uç, bu etiketi kullanarak belirli bir kullanıcıya bildirimler gönderir. 

> [!NOTE]
> Bu öğreticinin tamamlanan kodu [GitHub'da](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers) bulunabilir. 

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * WebAPI projesi oluşturma
> * WebAPI arka ucunda istemcilerin kimliğini doğrulama
> * WebAPI arka ucunu kullanarak bildirimlere kaydolma
> * WebAPI arka ucundan bildirim gönderme
> * Yeni WebAPI arka ucunu yayımlama
> * İstemci projesi için kodu güncelleştirme
> * Uygulamayı test etme


## <a name="prerequisites"></a>Ön koşullar
Bu öğretici, [Öğretici: Azure Notification Hubs kullanarak Evrensel Windows Platformu uygulamalarına bildirimler gönderme](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) öğreticisinde oluşturduğunuz bildirim hub’ını ve Visual Studio projesini temel alır. Bu nedenle, bu öğreticiye başlamadan önce onu tamamlayın. 

> [!NOTE]
> Azure App Service’te arka uç hizmetiniz olarak Mobile Apps kullanıyorsanız, bu öğreticinin [Mobile Apps sürümüne](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md) bakın.


&nbsp;

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="update-the-code-for-the-client-project"></a>İstemci projesi için kodu güncelleştirme
Bu bölümde, [Öğretici: Azure Notification Hubs kullanarak Evrensel Windows Platformu uygulamalarına bildirimler gönderme](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) öğreticisi için tamamladığınız projedeki kodu güncelleştireceksiniz. Proje, Windows mağazası ile zaten ilişkilendirilmiş olmalıdır. Ayrıca, bildirim hub'ınızı kullanacak şekilde yapılandırılmış olmalıdır. Bu bölümde, yeni WebAPI arka ucunu çağırmak için kod ekleyecek ve bildirimlere kaydolup bildirim göndermek için kullanacaksınız.

1. Visual Studio’da, [Öğretici: Azure Notification Hubs kullanarak Evrensel Windows Platformu uygulamalarına bildirimler gönderme](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) öğreticisi için oluşturduğunuz çözümü açın.
2. Çözüm Gezgini'nde, **WindowsApp** projesine sağ tıklayın ve ardından **NuGet Paketlerini Yönet**'e tıklayın.
3. Sol taraftaki **Çevrimiçi** öğesine tıklayın.
4. **Arama** kutusuna **Http İstemcisi** yazın.
5. Sonuçlar listesinde **System.Net.Http** ve sonra **Yükle**’ye tıklayın. Yüklemeyi tamamlayın.
6. NuGet **Arama** kutusuna **Json.net** yazın. **Newtonsoft.json** paketini yükleyin ve sonra NuGet Paket Yöneticisi penceresini kapatın.
8. Çözüm Gezgini'nde, **WindowsApp** projesinde bulunan **MainPage.xaml** dosyasına çift tıklayarak dosyayı Visual Studio düzenleyicisinde açın.
9. **MainPage.xaml** XML kodunda `<Grid>` bölümünü aşağıdaki kodla değiştirin: Bu kod, kullanıcının kimlik doğrulaması yaptığı kullanıcı adı ve parola metin kutusunu ekler. Ayrıca, bildirim iletisi için metin kutularıni ve bildirimi alması gereken kullanıcı adı etiketini ekler:

    ```xml   
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
    ```
11. Çözüm Gezgini’nde **(Windows 8.1)** ve **(Windows Phone 8.1)** projelerinin **MainPage.xaml.cs** dosyasını açın. Her iki dosyanın üst kısmına aşağıdaki `using` deyimlerini ekleyin:

    ```csharp    
    using System.Net.Http;
    using Windows.Storage;
    using System.Net.Http.Headers;
    using Windows.Networking.PushNotifications;
    using Windows.UI.Popups;
    using System.Threading.Tasks;
    ```
12. **WindowsApp** projesinin **MainPage.xaml.cs** dosyasında, `MainPage` sınıfına aşağıdaki üyeyi ekleyin. `<Enter Your Backend Endpoint>` değerini, önceden aldığınız gerçek arka ucun uç noktasıyla değiştirdiğinizden emin olun. Örneğin, `http://mybackend.azurewebsites.net`.
    
    ```csharp
    private static string BACKEND_ENDPOINT = "<Enter Your Backend Endpoint>";
    ```
13. Aşağıdaki kodu **(Windows 8.1)** ve **(Windows Phone 8.1)** projelerinin **MainPage.xaml.cs** dosyasındaki MainPage sınıfına ekleyin.
    
    `PushClick` yöntemi, **Anında İletme Bildirimi Gönder** düğmesinin tıklama işleyicisidir. `to_tag` parametresiyle eşleşen bir kullanıcı adı etiketine sahip tüm cihazlarda bir bildirimi tetiklemek için arka ucu çağırır. Bildirim iletisi, istek gövdesinde JSON içeriği olarak gönderilir.
    
    `LoginAndRegisterClick` yöntemi, **Oturum aç ve kaydet** düğmesinin tıklama işleyicisidir. Temel kimlik doğrulama belirtecini (kimlik doğrulama düzeninizin kullandığı herhangi bir belirteci temsil eder) yerel depolama alanında depolar, sonra arka ucu kullanarak bildirimlere kaydolmak için `RegisterClient` kullanır.

    ```csharp
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
            // The device handle used is different depending on the device and PNS. 
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
    ```
1. **App.xaml.cs** dosyasını açın. `OnLaunched()` olay işleyicisinde `InitNotificationsAsync()` çağrısını bulun. `InitNotificationsAsync()` çağrısına açıklama ekleyin veya çağrıyı silin. Düğme işleyicisi, bildirim kayıtlarını başlatır.

    ```csharp
    protected override void OnLaunched(LaunchActivatedEventArgs e)
    {
        //InitNotificationsAsync();
    ```
1. **WindowsApp** projesine sağ tıklayın, **Ekle**’ye ve sonra **Sınıf**’a tıklayın. **RegisterClient.cs** sınıfını adlandırın, ardından **Tamam**’a tıklayarak sınıfı oluşturun.
   
   Bu sınıf, anında iletme bildirimlerine kaydolmak için uygulama arka ucuyla iletişim kurmak üzere gereken REST çağrılarını sarmalar. [Uygulama arka ucunuzdan kaydetme](http://msdn.microsoft.com/library/dn743807.aspx) bölümünde açıklandığı gibi Bildirim Hub’ı tarafından oluşturulan *registrationId*’leri de yerel olarak depolar. **Oturum aç ve kaydet** düğmesine tıkladığınızda yerel depolama alanında depolanan bir yetkilendirme belirtecini kullanır.
2. Aşağıdaki `using` deyimlerini RegisterClient.cs dosyasının en üst kısmına ekleyin:

    ```csharp   
    using Windows.Storage;
    using System.Net;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using Newtonsoft.Json;
    using System.Threading.Tasks;
    using System.Linq;
    ```
3. Aşağıdaki kodu `RegisterClient` sınıf tanımına ekleyin.
   
    ```csharp
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

        if (statusCode != HttpStatusCode.Accepted && statusCode != HttpStatusCode.OK)
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
    ```
4. Yaptığınız tüm değişiklikleri kaydedin.

## <a name="test-the-application"></a>Uygulamayı test etme
1. Uygulamayı Windows üzerinde başlatın.
2. Aşağıdaki ekranda gösterildiği gibi **Kullanıcı Adı** ve **Parola** girin. Girdiğiniz değerler, Windows Phone üzerinde girdiğiniz kullanıcı adı ve paroladan farklı olmalıdır.
3. **Oturum aç ve kaydet**’e tıklayın ve bir iletişim kutusunun oturum açtığınızı gösterdiğini onaylayın. Bu kod ayrıca **Anında İletme Bildirimi Gönder** düğmesini etkinleştirir.
   
    ![][14]
5. Sonra **Alıcı Kullanıcı Adı Etiketi** alanına, kayıtlı kullanıcı adını girin. Bir bildirim iletisi girin ve **Anında İletme Bildirimi Gönder**’e tıklayın.
6. Yalnızca eşleşen kullanıcı adı etiketi ile kaydedilmiş cihazlar bildirim iletisini alır.
   
    ![][15]

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, kayıtlarıyla ilişkili etiketleri olan belirli kullanıcılara anında iletme bildirimleri göndermeyi öğrendiniz. Konum tabanlı anında iletme bildirimleri göndermeyi öğrenmek için aşağıdaki öğreticiye ilerleyin: 

> [!div class="nextstepaction"]
>[Konum temelli anında iletme bildirimleri gönderme](notification-hubs-push-bing-spartial-data-geofencing-notification.md)

[9]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push9.png
[10]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push10.png
[11]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push11.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-ui.png
[14]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-windows-instance-username.png
[15]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-notification-received.png
[16]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-send-message.png



<!-- URLs. -->
[Get started with Notification Hubs]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Secure Push]: notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md
[Use Notification Hubs to send breaking news]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
