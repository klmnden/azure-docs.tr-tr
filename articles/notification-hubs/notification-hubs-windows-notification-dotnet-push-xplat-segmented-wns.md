---
title: "(Evrensel Windows platformu) son dakika haberleri göndermek için Azure Notification Hubs'ı kullanma"
description: "Bir evrensel Windows platformu uygulamasında son dakika haberleri göndermek için Azure Notification Hubs kayıt etiketleri kullanın."
services: notification-hubs
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
ms.assetid: 994d2eed-f62e-433c-bf65-4afebf1c0561
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: d510e7e665adec9607aeee80802c466b363d5d5b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-notification-hubs-to-send-breaking-news"></a>Son dakika haberleri göndermek için Notification Hubs kullanma
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Genel Bakış
Bu konu Azure Notification Hubs son dakika haberi bildirimleri Windows mağazası veya Windows Phone 8.1 (Silverlight olmayan) uygulamasına yayını için nasıl kullanılacağını gösterir. Windows Phone 8.1 Silverlight hedefliyorsanız, başvurmak [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) sürümü. 

Bu işlemi tamamladıktan sonra ilgilendiğiniz son dakika haberleri kategorileri için kaydedebilirsiniz ve anında iletme bildirimlerini yalnızca bu kategorilerin alırsınız. Bu senaryo, bunları ilgi bildirilen kullanıcı gruplarına bildirimler burada gönderilmelidir birçok uygulamalar (örneğin, RSS Okuyucu veya müzik fanlar için uygulamalar) için yaygın bir durumdur. 

Bir veya daha fazla dahil ederek yayın senaryolarına olanak sağlayabilirsiniz *etiketleri* bildirim hub ' bir kayıt oluşturduğunuzda. Bir etiket için bildirimler gönderildiğinde kayıtlı tüm aygıtları etiketi için bildirim alırsınız. Etiketleri yalnızca dizeleri olduğundan, bunlar önceden ayarlanmış olması gerekmez. Etiketler hakkında daha fazla bilgi için bkz: [bildirim hub'ları Yönlendirme ve etiket ifadeleri](notification-hubs-tags-segment-push-message.md).

> [!NOTE]
> Windows mağazası ve Windows Phone proje sürümlerini 8.1 ve önceki Visual Studio 2017 içinde desteklenmez. Daha fazla bilgi için bkz. [Visual Studio 2017 Platform Desteği ve Uyumluluk](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs). 

## <a name="prerequisites"></a>Ön koşullar
Bu konu, oluşturduğunuz uygulama inşa edilmiştir [Notification Hubs ile çalışmaya başlama][get-started]. Bu öğretici başlamadan önce tamamlamanız gereken [Notification Hubs ile çalışmaya başlama][get-started].

## <a name="add-category-selection-to-the-app"></a>Kategori seçimi için uygulama ekleme
İlk adım kullanıcılar kaydetmek için kategoriler seçebilmeniz için varolan ana sayfanıza kullanıcı Arabirimi öğeleri eklemektir. Seçili kategorileri cihazda depolanır. Uygulama başlatıldığında bir aygıt kaydı bildirim hub'ınıza, seçilen kategori etiketleri olarak oluşturulur.

1. MainPage.xaml proje dosyasını açın ve ardından aşağıdaki kodu kopyalayın **kılavuz** öğe:
   
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

2. Sağ tıklatın **paylaşılan** adlı yeni bir sınıf ekleyin, proje **bildirimleri**, ekleme **ortak** değiştiricisi sınıf tanımına ve aşağıdakileri ekleyin **kullanarak** deyimleri için yeni kod dosyası:
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.Storage;
        using System.Threading.Tasks;

3. Aşağıdaki kodu yeni kopyalayın **bildirimleri** sınıfı:
   
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
   
    Bu sınıf, bu cihazı almalıdır haber kategorilerini depolamak için yerel depolama kullanır. Çağırmak yerine *RegisterNativeAsync* yöntemi, çağrı *RegisterTemplateAsync* kategoriler için bir şablon kaydı kullanarak kaydetmek için. 
   
    Ayrıca (örneğin, biri bildirimleri) ve biri döşeme için birden fazla şablon kaydetmek istediğiniz çünkü bir şablon adı (örneğin, "simpleWNSTemplateExample") sağlar. Böylece güncelleştirebilir veya silebilirsiniz şablonları adı.
   
    >[!NOTE]
    >Bir aygıt ile aynı etiketi birden fazla şablon kaydederse, etiket hedefleyen gelen iletiyi aygıta (her şablon için bir tane) teslim edilecek birden fazla bildirim neden olur. Bu davranış, aynı mantıksal ileti birden çok görsel bildirimleri (örneğin, bir Windows mağazası uygulamasında bir gösterge ve bildirim gösteren) neden gerekir yararlıdır.
   
    Daha fazla bilgi için bkz: [şablonları](notification-hubs-templates-cross-platform-push-messages.md).

4. App.xaml.cs proje dosyasında aşağıdaki özellik eklemek **uygulama** sınıfı:
   
        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");
   
    Bu özellik oluşturmak ve erişmek için kullandığınız bir **bildirimleri** örneği.
   
    Kodla `<hub name>` ve `<connection string with listen access>` , bildirim hub'ı adı ve bağlantı dizesinde yer tutucularını *DefaultListenSharedAccessSignature*, daha önce alınan.
   
   > [!NOTE]
   > Bir istemci uygulaması ile dağıtılmış kimlik bilgileri genellikle güvenli olmadığından, yalnızca için anahtar dağıtmak *dinleme* istemci uygulamanızı ile erişim. Dinleme erişimi olan bildirimleri için uygulamanızı kaydedebilirsiniz, ancak var olan kayıtlar değiştirilemez ve bildirim gönderilemiyor. Tam erişim tuşu bildirimleri gönderme ve var olan kayıtlar değiştirmek için güvenli bir arka uç hizmeti kullanılır.
   > 
   > 
5. MainPage.xaml.cs proje dosyasında aşağıdaki satırı ekleyin:
   
        using Windows.UI.Popups;

6. MainPage.xaml.cs proje dosyasında aşağıdaki yöntemi ekleyin:
   
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
   
    Bu yöntem kategorileri ve kullandığı bir liste oluşturur **bildirimleri** sınıf listesi yerel depolama alanına depolar. Ayrıca karşılık gelen etiketleri bildirim hub'ınıza kaydeder. Kategoriler değiştirildiğinde kaydı yeni kategoriler yeniden oluşturulur.

Uygulamanızı şimdi kategorileri kümesi cihazın yerel depolama depolayabilirsiniz. Kullanıcı kategorisi seçimine değiştirdiğinizde uygulamanın bildirim hub'ı ile kaydeder.

## <a name="register-for-notifications"></a>Bildirimler için kaydolun
Bu bölümde, başlangıçta bildirim hub'ı ile yerel depolama depoladığınız kategorileri kullanarak kaydedin.

> [!NOTE]
> Windows bildirim Hizmeti'ni (WNS) tarafından atanan URI kanal herhangi bir zamanda değiştiğinden bildirimlerinin sık bildirim hataları önlemek için kayıt olmalıdır. Bu örnek uygulama başladıktan her zaman bir bildirime kaydolur. Sık (birden fazla günde bir kez) çalışan uygulamalar için büyük olasılıkla bir günden az itibaren önceki kayıt aktarılırsa bant genişliğinden tasarruf etmek kayıt atlayabilirsiniz.
> 
> 

1. Kullanılacak `notifications` abone olmak için sınıf alarak kategorilere göre App.xaml.cs dosyasını açın ve ardından güncelleştirme **Initnotificationsasync** yöntemi.
   
        // *** Remove or comment out these lines *** 
        //var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
        //var hub = new NotificationHub("your hub name", "your listen connection string");
        //var result = await hub.RegisterNativeAsync(channel.Uri);
   
        var result = await notifications.SubscribeToCategories();
   
    Bu işlem, uygulama başlatıldığında kategorileri yerel depodan alır ve bu kategorilerin kayıt isteklerini sağlar. Oluşturduğunuz **Initnotificationsasync** yöntemi bir parçası olarak [Notification Hubs ile çalışmaya başlama] [ get-started] Öğreticisi.

2. MainPage.xaml.cs proje dosyasına aşağıdaki kodu ekleyin *OnNavigatedTo* yöntemi:
   
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
   
    Bu kod, ana sayfanın üzerinde önceden kaydedilmiş kategorileri durumunu güncelleştirir.

Uygulama tamamlanmıştır. Bu kullanıcı kategorisi seçimine değiştiğinde bildirim hub'ınızla kaydolmak için kullanılan aygıt yerel depolama alanında kategorileri kümesi saklayabilirsiniz. Sonraki bölümde, bu uygulama için kategori bildirimleri gönderebilir bir arka uç tanımlayın.

## <a name="send-tagged-notifications"></a>Etiketli bildirimleri gönderme
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-the-app-and-generate-notifications"></a>Uygulamayı çalıştırın ve bildirimler oluşturma
1. Visual Studio'da seçin **F5** derlemek ve uygulamayı başlatmak için.  
    Kullanıcı arabirimini uygulaması abone olmak için kategoriler seçmenize olanak sağlayan bir dizi değiştirir sağlar. 
   
    ![Yeni haber uygulama][1]

2. Bir veya daha fazla kategoriye değiştirir etkinleştirin ve ardından **abone ol**.
   
    Uygulama seçili kategorileri etiketlerine dönüştürür ve bildirim hub'ından seçili etiketleri için yeni bir cihaz kaydı ister. Kayıtlı kategorileri döndürülen ve bir iletişim kutusu görüntülenir.
   
    ![Kategori değiştirir ve abone ol düğmesi][19]

3. Yeni bir bildirim arka uçtan aşağıdaki yollardan biriyle gönder:

   * **Konsol uygulaması**: konsol uygulaması başlatın.
   * **Java/PHP**: uygulamanızı veya komut dosyasını çalıştırın.
     
     Seçili kategorileri için bildirimleri bildirimleri görünür.
     
     ![Bildirimleri][14]

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, son dakika haberleri kategoriye göre yayın öğrendiniz. Başka bir Gelişmiş bildirim hub'ları senaryo vurgular aşağıdaki öğreticiyi tamamlamak göz önünde bulundurun:

* [Yerelleştirilmiş son dakika haberleri yayınlamak için Notification Hubs'ı kullanma] Bu öğretici yerelleştirilmiş bildirimleri gönderme etkinleştirmek için en son haberleri uygulama nasıl ayıklanacağı açıklanır.

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
[Yerelleştirilmiş son dakika haberleri yayınlamak için Notification Hubs'ı kullanma]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
