---
title: Belirli cihazlara bildirim gönderme (Evrensel Windows Platformu) | Microsoft Docs
description: Bir Evrensel Windows Platformu uygulamasına son dakika haberleri göndermek için kayıtta etiketlerle birlikte Azure Notification Hubs kullanın.
services: notification-hubs
documentationcenter: windows
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 994d2eed-f62e-433c-bf65-4afebf1c0561
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/22/2019
ms.author: jowargo
ms.openlocfilehash: 9cfe5f490ef4063e02d9407f23130c1a216961ed
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60872411"
---
# <a name="tutorial-push-notifications-to-specific-windows-devices-running-universal-windows-platform-applications"></a>Öğretici: Evrensel Windows platformu uygulamaları çalıştıran belirli Windows cihazlara anında iletme bildirimleri gönderme

[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Genel Bakış

Bu öğreticide, bir Windows Mağazası veya Windows Phone 8.1 (Silverlight olmayan) uygulamasında son dakika haber bildirimleri yayınlamak için Azure Notification Hubs'ın nasıl kullanılacağı gösterilmektedir. Windows Phone 8.1 Silverlight'ı hedefliyorsanız [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) sürümüne bakın.

Bu öğreticide, Evrensel Windows Platformu uygulaması çalıştıran belirli Windows cihazlara anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını öğreneceksiniz. Öğreticiyi tamamladıktan sonra, ilginizi çeken son dakika haberi kategorilerine kaydolabilir ve yalnızca bu kategoriler için anında iletme bildirimleri alırsınız.

Yayın senaryoları, bildirim hub’ında bir kayıt oluştururken bir veya daha fazla *etiket* dahil edilerek etkinleştirilir. Bir etikete bildirimler gönderildiğinde, etikete kaydolan tüm cihazlar bildirimi alır. Etiketler hakkında daha fazla bilgi için bkz. [Kayıtlardaki Etiketler](notification-hubs-tags-segment-push-message.md).

> [!NOTE]
> Windows Mağazası ve Windows Phone 8.1 ve önceki proje sürümleri, Visual Studio 2017’de desteklenmez. Daha fazla bilgi için bkz. [Visual Studio 2017 Platform Desteği ve Uyumluluk](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Mobil uygulamaya kategori seçimi ekleme
> * Bildirimlere kaydolma
> * Etiketli bildirim gönderme
> * Uygulamayı çalıştırma ve bildirimler oluşturma

## <a name="prerequisites"></a>Önkoşullar

Tamamlamak [Öğreticisi: Azure Notification Hubs'ı kullanarak, Evrensel Windows platformu uygulamaları için bildirimleri gönderecek] [ get-started] bu öğreticiye başlamadan önce.  

## <a name="add-category-selection-to-the-app"></a>Uygulamaya kategori seçimi ekleme

İlk adım, mevcut ana sayfanıza kullanıcının kaydolmak için kategorileri seçebileceği UI öğeleri eklemektir. Seçilen kategoriler cihazda depolanır. Uygulama başlatıldığında, etiketler olarak seçilen kategorilerle bildirim hub’ınızda bir cihaz kaydı oluşturulur.

1. MainPage.xaml proje dosyasını açın ve ardından aşağıdaki kodu kopyalayın `Grid` öğesi:

    ```xml
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition/>
            <RowDefinition/>
            <RowDefinition/>
            <RowDefinition/>
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>
        <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top" HorizontalAlignment="Center"/>
        <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="1" Grid.Column="0" HorizontalAlignment="Center"/>
        <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="2" Grid.Column="0" HorizontalAlignment="Center"/>
        <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="3" Grid.Column="0" HorizontalAlignment="Center"/>
        <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="1" Grid.Column="1" HorizontalAlignment="Center"/>
        <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="2" Grid.Column="1" HorizontalAlignment="Center"/>
        <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="3" Grid.Column="1" HorizontalAlignment="Center"/>
        <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click"/>
    </Grid>
    ```

2. İçinde **Çözüm Gezgini**, projeyi sağ tıklatın, yeni bir sınıf ekleyin: **Bildirimleri**. Ekleme **genel** değiştiricisi sınıf tanımına ve ardından aşağıdakileri ekleyin `using` deyimleri için yeni bir kod dosyası:

    ```csharp
    using Windows.Networking.PushNotifications;
    using Microsoft.WindowsAzure.Messaging;
    using Windows.Storage;
    using System.Threading.Tasks;
    ```

3. Aşağıdaki kodu yeni kopyalayın `Notifications` sınıfı:

    ```csharp
    private NotificationHub hub;

    public Notifications(string hubName, string listenConnectionString)
    {
        hub = new NotificationHub(hubName, listenConnectionString);
    }

    public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
    {
        ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
        return await SubscribeToCategories(categories);
    }

    public IEnumerable<string> RetrieveCategories()
    {
        var categories = (string) ApplicationData.Current.LocalSettings.Values["categories"];
        return categories != null ? categories.Split(','): new string[0];
    }

    public async Task<Registration> SubscribeToCategories(IEnumerable<string> categories = null)
    {
        var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

        if (categories == null)
        {
            categories = RetrieveCategories();
        }

        // Using a template registration to support notifications across platforms.
        // Any template notifications that contain messageParam and a corresponding tag expression
        // will be delivered for this registration.

        const string templateBodyWNS = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

        return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "simpleWNSTemplateExample",
                categories);
    }
    ```

    Bu sınıf, bu cihazın alması gereken haber kategorilerini depolamak için yerel depolamayı kullanır. Çağırmak yerine `RegisterNativeAsync` yöntemi, çağrı `RegisterTemplateAsync` kategoriler için bir şablon kaydı kullanarak kaydetmek için.

    Birden fazla şablon (örneğin, bir tane bildirimler, bir tane de kutucuklar için) kaydetmek istiyorsanız bir şablon adı belirtin (örneğin, "simpleWNSTemplateExample"). Şablonları güncelleştirebilmeniz veya silebilmeniz için adlandırmanız gerekir.

    >[!NOTE]
    >Bir cihaz aynı etiketle birden fazla şablonu kaydederse, etiketi hedefleyen bir gelen ileti, cihaza birden fazla bildirimin (her şablon için bir tane) iletilmesine neden olur. Bu davranış, aynı mantıksal iletinin birden fazla görsel bildirimle sonuçlanması gereken durumlarda (örneğin, Windows Mağazası uygulamasında hem bir gösterge hem de bir bildirim gösterme) yararlıdır.

    Daha fazla bilgi için bkz. [Şablonlar](notification-hubs-templates-cross-platform-push-messages.md).

4. App.xaml.cs proje dosyasında aşağıdaki özelliği ekleyin `App` sınıfı:

    ```csharp
    public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");
    ```

    Bu özellik oluşturmak ve erişmek için kullandığınız bir `Notifications` örneği.

    Kod içindeki `<hub name>` ve `<connection string with listen access>` yer tutucularını, daha önce edindiğiniz bildirim hub'ı adınız ve *DefaultListenSharedAccessSignature* bağlantı dizeniz ile değiştirin.

   > [!NOTE]
   > Bir istemci uygulaması ile dağıtılmış kimlik bilgileri genellikle güvenli olmadığından yalnızca istemci uygulamanızla *dinleme* erişimi için anahtarı dağıtın. Dinleme erişimi ile uygulamanızın bildirimlere kaydolmasını sağlar, ancak mevcut kayıtlar değiştirilemez ve bildirimler gönderilemez. Tam erişim anahtarı, güvenli bir arka uç hizmetinde bildirimler göndermek ve mevcut kayıtları değiştirmek için kullanılır.

5. İçinde `MainPage.xaml.cs` dosyasında, aşağıdaki satırı ekleyin:

    ```csharp
    using Windows.UI.Popups;
    ```

6. İçinde `MainPage.xaml.cs` dosyasında, aşağıdaki yöntemi ekleyin:

    ```csharp
    private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
    {
        var categories = new HashSet<string>();
        if (WorldToggle.IsOn) categories.Add("World");
        if (PoliticsToggle.IsOn) categories.Add("Politics");
        if (BusinessToggle.IsOn) categories.Add("Business");
        if (TechnologyToggle.IsOn) categories.Add("Technology");
        if (ScienceToggle.IsOn) categories.Add("Science");
        if (SportsToggle.IsOn) categories.Add("Sports");

        var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);

        var dialog = new MessageDialog("Subscribed to: " + string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
    ```

    Bu yöntem kategoriler kullanır ve bir liste oluşturur `Notifications` liste yerel depolamada depolamak için sınıf. Ayrıca ilgili etiketleri bildirim hub’ınıza kaydeder. Kategoriler değiştirildiğinde kayıt yeni kategorilerle yeniden oluşturulur.

Uygulamanız artık cihazın yerel depolama alanında bir kategori kümesini depolayabilir. Uygulama, kullanıcılar kategori seçimini her değiştirdiğinde bildirim hub’ına kaydolur.

## <a name="register-for-notifications"></a>Bildirimlere kaydolma

Bu bölümde, yerel depolama alanında depoladığınız kategorileri kullanarak başlatma sırasında bildirim hub’ına kaydolursunuz.

> [!NOTE]
> Windows Bildirim Hizmeti (WNS) tarafından atanan kanal URI’si her zaman değişebileceğinden, bildirim hatalarını önlemek için sık sık bildirimlere kaydolmanız gerekir. Bu örnek, uygulama her başlatıldığında bildirimlere kaydolur. Sık çalıştırdığınız uygulamalar için, önceki kayıttan bu yana bir günden az zaman geçtiyse bant genişliğini korumak için günde birkaç kere kaydı atlayabilirsiniz.

1. Kullanılacak `notifications` abone olmak için sınıf tabanlı kategorileri, App.xaml.cs dosyasını açın ve ardından güncelleştirme `InitNotificationsAsync` yöntemi.

    ```csharp
    // *** Remove or comment out these lines ***
    //var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
    //var hub = new NotificationHub("your hub name", "your listen connection string");
    //var result = await hub.RegisterNativeAsync(channel.Uri);

    var result = await notifications.SubscribeToCategories();
    ```

    Bu işlem, uygulama her başlatıldığında uygulamanın yerel depolama alanından kategorileri almasını ve bu kategorilere kayıt isteğinde bulunmasını sağlar. Oluşturduğunuz `InitNotificationsAsync` yöntemi bir parçası olarak [Notification Hubs ile çalışmaya başlama] [ get-started] öğretici.
2. İçinde `MainPage.xaml.cs` proje dosyası, aşağıdaki kodu ekleyin `OnNavigatedTo` yöntemi:

    ```csharp
    protected override void OnNavigatedTo(NavigationEventArgs e)
    {
        var categories = ((App)Application.Current).notifications.RetrieveCategories();

        if (categories.Contains("World")) WorldToggle.IsOn = true;
        if (categories.Contains("Politics")) PoliticsToggle.IsOn = true;
        if (categories.Contains("Business")) BusinessToggle.IsOn = true;
        if (categories.Contains("Technology")) TechnologyToggle.IsOn = true;
        if (categories.Contains("Science")) ScienceToggle.IsOn = true;
        if (categories.Contains("Sports")) SportsToggle.IsOn = true;
    }
    ```

    Bu kod, önceden kaydedilmiş kategorilerin durumuna göre ana sayfayı güncelleştirir.

Uygulamanız artık tamamlandı. Uygulama, kullanıcılar kategori seçimini değiştirdiğinde cihazın yerel depolama alanında bildirim hub’ını kaydetmek için kullanılan bir kategori kümesi depolayabilir. Sonraki bölümde, bu uygulamaya kategori bildirimleri gönderebilen bir arka uç tanımlayacaksınız.

## <a name="run-the-uwp-app"></a>UWP uygulama çalıştırma 
1. Visual Studio’da **F5** tuşunu seçerek uygulamayı derleyin ve başlatın. Uygulama kullanıcı arabirimi, abone olunacak kategorileri seçmenize olanak sağlayan iki durumlu düğmeler sağlar.

    ![Son Dakika Haberleri uygulaması](./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-breakingnews-win1.png)

2. Bir veya daha fazla kategori iki durumlu düğmesini etkinleştirin ve sonra **Abone ol**’a tıklayın.

    Uygulama, seçilen kategorileri etiketlere dönüştürür ve bildirim hub’ından seçilen etiketler için yeni bir cihaz kaydı ister. Kayıtlı kategoriler döndürülür ve bir iletişim kutusunda görüntülenir.

    ![Kategori iki durumlu düğmeleri ve Abone ol düğmesi](./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-toast-2.png)

## <a name="create-a-console-app-to-send-tagged-notifications"></a>Etiketli bildirimleri göndermek için bir konsol uygulaması oluşturma

[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-the-console-app-to-send-tagged-notifications"></a>Etiketli bildirimleri göndermek için konsol uygulamasını çalıştırma

1. Önceki bölümde oluşturduğunuz uygulamayı çalıştırın.
2. Seçili kategorilere ait bildirimler, bildirim olarak görünür. Bildirim seçtiğiniz ilk UWP uygulaması penceresini görürsünüz. 

     ![Bildirimler](./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-reg-2.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, son dakika haberlerini kategoriye göre yayınlamayı öğrendiniz. Arka uç uygulaması, bir etikete ait bildirimleri almak için kaydolmuş cihazlara o etiketi taşıyan bildirimleri gönderir. Hangi cihazı kullandıklarından bağımsız olarak belirli kullanıcılara anında iletme bildirimleri gönderme hakkında bilgi almak için sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Yerelleştirilmiş anında iletme bildirimleri gönderme](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)

<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- URLs.-->
[get-started]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Use Notification Hubs to broadcast localized breaking news]: notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: https://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: https://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: https://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: https://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: https://go.microsoft.com/fwlink/p/?LinkId=262253
[wns object]: https://go.microsoft.com/fwlink/p/?LinkId=260591
