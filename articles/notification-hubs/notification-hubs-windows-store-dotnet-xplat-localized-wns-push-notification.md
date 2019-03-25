---
title: Azure Notification Hubs kullanarak Windows uygulamalarına yerelleştirilmiş bildirimler gönderme | Microsoft Docs
description: Azure Notification Hubs kullanarak yerelleştirilmiş son dakika haber bildirimleri göndermeyi öğrenin.
services: notification-hubs
documentationcenter: windows
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: c454f5a3-a06b-45ac-91c7-f91210889b25
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/22/2019
ms.author: jowargo
ms.openlocfilehash: f3880db813072ca0bcecf073a8db24b21c87189f
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58402722"
---
# <a name="tutorial-push-localized-notifications-to-windows-apps-by-using-azure-notification-hubs"></a>Öğretici: Azure Notification hubs'ı kullanarak Windows uygulamaları için yerelleştirilmiş anında iletme bildirimleri

> [!div class="op_single_selector"]
> * [Windows Mağazası C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)

## <a name="overview"></a>Genel Bakış

Bu öğreticide, Notification Hubs hizmetine kaydolmuş mobil cihazlara yerelleştirilmiş anında iletme bildirimleri gönderme işlemi gösterilmektedir. Öğreticide, oluşturulan uygulamaları güncelleştirme [Öğreticisi: Belirli cihazlara (Evrensel Windows platformu) bildirimleri gönderme](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) aşağıdaki senaryoları desteklemek için:

- Windows Mağazası uygulaması, istemci cihazların bir dil belirtmesine ve farklı son dakika haber kategorilerine abone olmasına olanak tanır.
- Arka uç uygulaması kullanarak bildirim yayınlar **etiketi** ve **şablon** Azure Notification Hubs'ın özellikleri.

Öğreticiyi tamamladığınızda, mobil uygulamayı kullanarak ilgi duyduğunuz kategorilere kaydolabilir ve ayrıca bildirimlerin alınacağı dili belirtebilirsiniz. Arka uç uygulaması, dil ve cihaza göre yerelleştirilmiş bildirimler gönderir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Windows uygulamasını yerel ayar bilgilerini destekleyecek şekilde güncelleştirme
> * Arka uç uygulamasını yerelleştirilmiş bildirimler gönderecek şekilde güncelleştirme
> * Uygulamayı test edin

## <a name="prerequisites"></a>Önkoşullar

Tamamlamak [Öğreticisi: Belirli cihazlara (Evrensel Windows platformu) bildirimleri gönderme](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md).

İçinde [Öğreticisi: Belirli cihazlara (Evrensel Windows platformu) bildirimleri gönderme](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md), kullanılan uygulama yerleşik **etiketleri** farklı bir haberler için Bildirimlere abone için **kategorileri**. Bu öğreticide, Notification Hubs’ın **şablon** özelliğini kullanarak **yerelleştirilmiş** son dakika haber bildirimlerini kolayca iletebilirsiniz.

Yüksek bir düzeyde, şablonlar belirli bir cihazın bir bildirimi hangi biçimde alacağını belirtmenin yoludur. Şablon, uygulama arka ucunuz tarafından gönderilen iletinin parçası olan özelliklere başvurarak tam yük biçimini belirtir. Bu öğreticide, arka uç uygulaması tüm desteklenen dilleri içeren, yerel ayardan bağımsız bir ileti gönderir:

```json
{
    "News_English": "...",
    "News_French": "...",
    "News_Mandarin": "..."
}
```

Cihaz, doğru özelliğe başvuran bir şablonla kaydolur. Örneğin, İngilizce bildirim almak isteyen bir Windows Mağazası uygulaması, karşılık gelen herhangi bir etiketle aşağıdaki şablona kaydolur:

```xml
<toast>
    <visual>
    <binding template=\"ToastText01\">
        <text id=\"1\">$(News_English)</text>
    </binding>
    </visual>
</toast>
```

Şablonlar hakkında daha fazla bilgi için bkz: [anında iletme şablonları](notification-hubs-templates-cross-platform-push-messages.md).

## <a name="update-windows-app-to-support-locale-information"></a>Windows uygulamasını yerel ayar bilgilerini destekleyecek şekilde güncelleştirme

1. İçin oluşturulan Visual Studio çözümünü açmak [Öğreticisi: Belirli cihazlara (Evrensel Windows platformu) bildirimleri gönderme](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md).
2. Çözümün güncelleştirme `MainPage.xaml` dosyasını bir yerel ayar combobox içerecek şekilde:

    ```xml
    <Grid Margin="120, 58, 120, 80"  
            Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top"/>
        <ComboBox Name="Locale" HorizontalAlignment="Left" VerticalAlignment="Center" Width="200" Grid.Row="1" Grid.Column="0">
            <x:String>English</x:String>
            <x:String>French</x:String>
            <x:String>Mandarin</x:String>
        </ComboBox>
        <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="2" Grid.Column="0"/>
        <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="3" Grid.Column="0"/>
        <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="4" Grid.Column="0"/>
        <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="2" Grid.Column="1"/>
        <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="3" Grid.Column="1"/>
        <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="4" Grid.Column="1"/>
        <Button Content="Subscribe" HorizontalAlignment="Center" Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
    </Grid>
    ```
3. İçinde `Notifications` sınıfı, bir yerel ayar parametresini Ekle `StoreCategoriesAndSubscribe` ve `SubscribeToCategories` yöntemleri.

    ```csharp
    public async Task<Registration> StoreCategoriesAndSubscribe(string locale, IEnumerable<string> categories)
    {
        ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
        ApplicationData.Current.LocalSettings.Values["locale"] = locale;
        return await SubscribeToCategories(locale, categories);
    }

    public async Task<Registration> SubscribeToCategories(string locale, IEnumerable<string> categories = null)
    {
        var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

        if (categories == null)
        {
            categories = RetrieveCategories();
        }

        // Using a template registration. This makes supporting notifications across other platforms much easier.
        // Using the localized tags based on locale selected.
        string templateBodyWNS = String.Format("<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(News_{0})</text></binding></visual></toast>", locale);

        return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "localizedWNSTemplateExample", categories);
    }
    ```

    Çağırmak yerine `RegisterNativeAsync` yöntemi, çağrı `RegisterTemplateAsync`. Şablonun yerel ayara bağlı olduğu belirli bir bildirim biçimi kaydedin. Ayrıca, birden fazla şablon (örneğin, bir tane bildirimler, bir tane de kutucuklar için) kaydetmek istiyorsanız şablon için bir ad ("localizedWNSTemplateExample") belirtin. Şablonları güncelleştirmek veya silmek için de adlandırmanız gerekir.

    Bir cihaz aynı etiketle birden fazla şablonu kaydederse, o etiketi hedefleyen bir gelen ileti, cihaza birden fazla bildirimin (her şablon için bir tane) iletilmesiyle sonuçlanır. Bu davranış, aynı mantıksal iletinin birden fazla görsel bildirimle sonuçlanması gereken durumlarda (örneğin, Windows Mağazası uygulamasında hem bir gösterge hem de bir bildirim gösterme) yararlıdır.
4. Depolanan yerel ayarı almak için aşağıdaki yöntemi ekleyin:

    ```csharp
    public string RetrieveLocale()
    {
        var locale = (string) ApplicationData.Current.LocalSettings.Values["locale"];
        return locale != null ? locale : "English";
    }
    ```

5. İçinde `MainPage.xaml.cs` dosyası, güncelleştirme işleyicisi, geçerli yerel ayar birleşik giriş kutusu değerini almak ve çağrı sağlamak için düğmeye tıklayın `Notifications` sınıfı:

    ```csharp
    private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
    {
        var locale = (string)Locale.SelectedItem;

        var categories = new HashSet<string>();
        if (WorldToggle.IsOn) categories.Add("World");
        if (PoliticsToggle.IsOn) categories.Add("Politics");
        if (BusinessToggle.IsOn) categories.Add("Business");
        if (TechnologyToggle.IsOn) categories.Add("Technology");
        if (ScienceToggle.IsOn) categories.Add("Science");
        if (SportsToggle.IsOn) categories.Add("Sports");

        var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(locale,
                categories);

        var dialog = new MessageDialog("Locale: " + locale + " Subscribed to: " + 
            string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
    ```
6. Son olarak `App.xaml.cs` dosyası, güncelleştirme `InitNotificationsAsync` yöntemi yerel alıp abone olurken kullanın:

    ```csharp
    private async void InitNotificationsAsync()
    {
        var result = await notifications.SubscribeToCategories(notifications.RetrieveLocale());

        // Displays the registration ID so you know it was successful
        if (result.RegistrationId != null)
        {
            var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }
    ```



## <a name="run-the-uwp-application"></a>UWP uygulama çalıştırma

1. Evrensel Windows platformu uygulaması çalıştırın. **Kayıt başarılı** iletisini görene kadar bekleyin.

    ![Mobil uygulama ve kayıt](./media/notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification/registration-successful.png)
2. **Kategoriler** ve **yerel ayar**’ı seçip **Abone ol**’a tıklayın. Uygulama, seçilen kategorileri etiketlere dönüştürür ve bildirim hub’ından seçilen etiketler için yeni bir cihaz kaydı ister.

    ![Mobil uygulama](./media/notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification/mobile-app.png)
3. **Abonelikler** hakkında bir **onay** iletisi görürsünüz.

    ![Abonelik iletisi](./media/notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification/subscription-message.png)

## <a name="update-console-app-to-send-localized-notifications"></a>Yerelleştirilmiş bildirimler göndermek için güncelleştirme konsol uygulaması

Şablon bildirimleri gönderirken yalnızca bir dizi özellik sağlamanız gerekir. Bu öğreticide, arka uç uygulaması geçerli haberin yerelleştirilmiş sürümünü içeren bir dizi özellik gönderir, örneğin:

```json
{
    "News_English": "World News in English!",
    "News_French": "World News in French!",
    "News_Mandarin": "World News in Mandarin!"
}
```

Bu bölümde, çözümdeki konsol uygulaması projesini güncelleştireceksiniz. Daha önce oluşturduğunuz konsol uygulamasındaki `SendTemplateNotificationAsync` yöntemini aşağıdaki kodla değiştirin:

> [!IMPORTANT]
> Kodda bildirim hub’ınıza tam erişimi olan adı ve bağlantı dizesini belirtin.

```csharp
private static async void SendTemplateNotificationAsync()
{
    // Define the notification hub.
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(
        "<connection string with full access>", "<hub name>");

    // Sending the notification as a template notification. All template registrations that contain
    // "messageParam" or "News_<local selected>" and the proper tags will receive the notifications.
    // This includes APNS, FCM, WNS, and MPNS template registrations.
    Dictionary<string, string> templateParams = new Dictionary<string, string>();

    // Create an array of breaking news categories.
    var categories = new string[] { "World", "Politics", "Business", "Technology", "Science", "Sports"};
    var locales = new string[] { "English", "French", "Mandarin" };

    foreach (var category in categories)
    {
        templateParams["messageParam"] = "Breaking " + category + " News!";

        // Sending localized News for each tag too...
        foreach( var locale in locales)
        {
            string key = "News_" + locale;

            // Your real localized news content would go here.
            templateParams[key] = "Breaking " + category + " News in " + locale + "!";
        }

        await hub.SendTemplateNotificationAsync(templateParams, category);
    }
}
```

Bu basit aramayı haberlerinden yerelleştirilmiş parçasını sunar **tüm** cihazlar, bildirim Hub'ınıza oluşturur ve tüm cihazlar, belirli bir etikete abone doğru yerel yük teslim platformu, belirtilmediğine.

## <a name="run-console-app-to-send-localized-notification"></a>Yerelleştirilmiş bildirim göndermek için konsol uygulaması
Çalıştırma **konsol uygulaması** her kategori için ve her bildirimleri göndermek için desteklenen dil. Yalnızca abone olduğunuz kategoriler için bildirim aldığınızı ve iletinin seçtiğiniz yerel ayara uygun olduğunu doğrulayın.

![Bildirim iletileri](./media/notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification/notifications.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, kayıtlarıyla ilişkili etiketleri olan belirli cihazlara yerelleştirilmiş anında iletme bildirimleri göndermeyi öğrendiniz. Birden fazla cihaz kullanıyor olabilecek belirli kullanıcılara nasıl anında iletme bildirimleri gönderileceğini öğrenmek için aşağıdaki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
>[Belirli kullanıcılara anında iletme bildirimleri gönderme](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md)

<!-- Anchors. -->
[Template concepts]: #concepts
[The app user interface]: #ui
[Building the Windows Store client app]: #building-client
[Send notifications from your back-end]: #send
[Next Steps]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Notify users with Notification Hubs: Mobile Services]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Use Notification Hubs to send breaking news]: notification-hubs/notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Submit an app page]: https://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: https://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: https://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-dotnet
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-dotnet
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-app-users-dotnet
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-dotnet
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js
[wns object]: https://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: https://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: https://msdn.microsoft.com/library/jj927168.aspx
[Notification Hubs How-To for Windows Store]: https://msdn.microsoft.com/library/jj927172.aspx
