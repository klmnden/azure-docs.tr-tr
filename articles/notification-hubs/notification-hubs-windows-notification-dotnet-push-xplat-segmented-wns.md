---
title: Belirli cihazlara bildirim gönderme (Evrensel Windows Platformu) | Microsoft Docs
description: Bir Evrensel Windows Platformu uygulamasına son dakika haberleri göndermek için kayıtta etiketlerle birlikte Azure Notification Hubs kullanın.
services: notification-hubs
documentationcenter: windows
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: 994d2eed-f62e-433c-bf65-4afebf1c0561
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: 9b9e3b910162653c14c398e2c3392709abcd5fd8
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-push-notifications-to-specific-windows-devices-running-universal-windows-platform-applications"></a>Öğretici: Evrensel Windows Platformu uygulamaları çalıştıran belirli Windows cihazlarına anında iletme bildirimleri gönderme
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

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce [Öğretici: Azure Notification Hubs kullanarak Evrensel Windows Platformu uygulamalarına bildirimler gönderme][get-started] öğreticisini tamamlayın.  

## <a name="add-category-selection-to-the-app"></a>Uygulamaya kategori seçimi ekleme
İlk adım, mevcut ana sayfanıza kullanıcının kaydolmak için kategorileri seçebileceği UI öğeleri eklemektir. Seçilen kategoriler cihazda depolanır. Uygulama başlatıldığında, etiketler olarak seçilen kategorilerle bildirim hub’ınızda bir cihaz kaydı oluşturulur.

1. MainPage.xaml proje dosyasını açın ve sonra aşağıdaki kodu **Kılavuz** öğesine kopyalayın:
   
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

2. **Çözüm Gezgini**’nde projeye sağ tıklayın ve **Notifications** adlı yeni bir sınıf oluşturun. Sınıf tanımına **public** değiştiricisini ekleyin ve sonra aşağıdaki **using** deyimlerini yeni kod dosyasına ekleyin:

    ```csharp   
    using Windows.Networking.PushNotifications;
    using Microsoft.WindowsAzure.Messaging;
    using Windows.Storage;
    using System.Threading.Tasks;
    ```

3. Yeni **Notifications** sınıfına aşağıdaki kodu kopyalayın:
   
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
   
    Bu sınıf, bu cihazın alması gereken haber kategorilerini depolamak için yerel depolamayı kullanır. *RegisterNativeAsync* yöntemini çağırmak yerine, bir şablon kaydı kullanarak kategorilere kaydolmak için *RegisterTemplateAsync* yöntemini çağırın. 
   
    Birden fazla şablon (örneğin, bir tane bildirimler, bir tane de kutucuklar için) kaydetmek istiyorsanız bir şablon adı belirtin (örneğin, "simpleWNSTemplateExample"). Şablonları güncelleştirebilmeniz veya silebilmeniz için adlandırmanız gerekir.
   
    >[!NOTE]
    >Bir cihaz aynı etiketle birden fazla şablonu kaydederse, etiketi hedefleyen bir gelen ileti, cihaza birden fazla bildirimin (her şablon için bir tane) iletilmesine neden olur. Bu davranış, aynı mantıksal iletinin birden fazla görsel bildirimle sonuçlanması gereken durumlarda (örneğin, Windows Mağazası uygulamasında hem bir gösterge hem de bir bildirim gösterme) yararlıdır.
   
    Daha fazla bilgi için bkz. [Şablonlar](notification-hubs-templates-cross-platform-push-messages.md).

4. App.xaml.cs proje dosyasında **App** sınıfına aşağıdaki özelliği ekleyin:

    ```csharp   
    public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");
    ```
   
    Bu özelliği kullanarak bir **Notifications** örneği oluşturup erişebilirsiniz.
   
    Kod içindeki `<hub name>` ve `<connection string with listen access>` yer tutucularını, daha önce edindiğiniz bildirim hub'ı adınız ve *DefaultListenSharedAccessSignature* bağlantı dizeniz ile değiştirin.
   
   > [!NOTE]
   > Bir istemci uygulaması ile dağıtılmış kimlik bilgileri genellikle güvenli olmadığından yalnızca istemci uygulamanızla *dinleme* erişimi için anahtarı dağıtın. Dinleme erişimi ile uygulamanızın bildirimlere kaydolmasını sağlar, ancak mevcut kayıtlar değiştirilemez ve bildirimler gönderilemez. Tam erişim anahtarı, güvenli bir arka uç hizmetinde bildirimler göndermek ve mevcut kayıtları değiştirmek için kullanılır.
   > 
   > 
5. MainPage.xaml.cs proje dosyasına aşağıdaki satırı ekleyin:
   
    ```csharp
    using Windows.UI.Popups;
    ```

6. MainPage.xaml.cs proje dosyasına aşağıdaki yöntemi ekleyin:
   
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

    Bu yöntem, kategorilerin bir listesini oluşturur ve **Notifications** sınıfını kullanarak listeyi yerel depolama alanında depolar. Ayrıca ilgili etiketleri bildirim hub’ınıza kaydeder. Kategoriler değiştirildiğinde kayıt yeni kategorilerle yeniden oluşturulur.

Uygulamanız artık cihazın yerel depolama alanında bir kategori kümesini depolayabilir. Uygulama, kullanıcılar kategori seçimini her değiştirdiğinde bildirim hub’ına kaydolur.

## <a name="register-for-notifications"></a>Bildirimlere kaydolma
Bu bölümde, yerel depolama alanında depoladığınız kategorileri kullanarak başlatma sırasında bildirim hub’ına kaydolursunuz.

> [!NOTE]
> Windows Bildirim Hizmeti (WNS) tarafından atanan kanal URI’si her zaman değişebileceğinden, bildirim hatalarını önlemek için sık sık bildirimlere kaydolmanız gerekir. Bu örnek, uygulama her başlatıldığında bildirimlere kaydolur. Sık çalıştırdığınız uygulamalar için, önceki kayıttan bu yana bir günden az zaman geçtiyse bant genişliğini korumak için günde birkaç kere kaydı atlayabilirsiniz.
> 
> 

1. `notifications` sınıfını kullanarak kategorilere göre abone olmak için App.xaml.cs dosyasını açın ve sonra **InitNotificationsAsync** yöntemini güncelleştirin.
   
    ```csharp
    // *** Remove or comment out these lines *** 
    //var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
    //var hub = new NotificationHub("your hub name", "your listen connection string");
    //var result = await hub.RegisterNativeAsync(channel.Uri);

    var result = await notifications.SubscribeToCategories();
   ```
    Bu işlem, uygulama her başlatıldığında uygulamanın yerel depolama alanından kategorileri almasını ve bu kategorilere kayıt isteğinde bulunmasını sağlar. [Notification Hubs kullanmaya başlama][get-started] öğreticisinin bir parçası olarak **InitNotificationsAsync** yöntemini oluşturdunuz.
2. MainPage.xaml.cs proje dosyasına, *OnNavigatedTo* yöntemine aşağıdaki kodu ekleyin:
   
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

## <a name="send-tagged-notifications"></a>Etiketli bildirimler gönderme
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-the-app-and-generate-notifications"></a>Uygulamayı çalıştırma ve bildirimler oluşturma
1. Visual Studio’da **F5** tuşunu seçerek uygulamayı derleyin ve başlatın. Uygulama kullanıcı arabirimi, abone olunacak kategorileri seçmenize olanak sağlayan iki durumlu düğmeler sağlar. 
   
    ![Son Dakika Haberleri uygulaması][1]

2. Bir veya daha fazla kategori iki durumlu düğmesini etkinleştirin ve sonra **Abone ol**’a tıklayın.
   
    Uygulama, seçilen kategorileri etiketlere dönüştürür ve bildirim hub’ından seçilen etiketler için yeni bir cihaz kaydı ister. Kayıtlı kategoriler döndürülür ve bir iletişim kutusunda görüntülenir.
   
    ![Kategori iki durumlu düğmeleri ve Abone ol düğmesi][19]

3. Aşağıdaki yöntemlerden birini kullanarak arka uçtan yeni bir bildirim gönderin:

   * **Konsol uygulaması**: Konsol uygulamasını başlatın.
   * **Java/PHP**: Uygulamanızı veya betiğinizi çalıştırın.
     
     Seçili kategorilere ait bildirimler, bildirim olarak görünür.
     
     ![Bildirimler][14]

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

<!-- Images. -->
[1]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-breakingnews-win1.png

[14]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-toast-2.png


[19]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-reg-2.png

<!-- URLs.-->
[get-started]: /azure/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification
[Use Notification Hubs to broadcast localized breaking news]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
