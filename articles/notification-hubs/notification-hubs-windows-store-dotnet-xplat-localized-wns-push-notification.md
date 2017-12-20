---
title: "Notification Hubs son dakika haberleri öğretici yerelleştirilmiş"
description: "Yerelleştirilmiş son dakika haberi bildirimleri göndermek için Azure Notification Hubs kullanmayı öğrenin."
services: notification-hubs
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
ms.assetid: c454f5a3-a06b-45ac-91c7-f91210889b25
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 8f205188bd68e53b187b71981ed36dcf9129ec62
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="use-notification-hubs-to-send-localized-breaking-news"></a>Yerelleştirilmiş son dakika haberleri göndermek için Notification Hubs kullanma
> [!div class="op_single_selector"]
> * [Windows mağazası C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)
> 
> 

## <a name="overview"></a>Genel Bakış
Bu konuda nasıl kullanılacağını gösterir **şablonu** dil ve cihaz tarafından yerelleştirilmiş son dakika haberi bildirimleri yayınlamak için Azure Notification Hubs özelliğidir. Bu öğreticide oluşturduğunuz Windows mağazası uygulaması ile başlayın [son dakika haberleri göndermek için Notification Hubs kullanma]. Tamamlandığında, ilgilendiğiniz kategorileri için kaydetme, hangi bildirimleri almak ve o dilde yalnızca seçili kategorileri için anında iletme bildirimleri almak bir dil belirtin mümkün olacaktır.

Bu senaryo iki bölümü vardır:

* Windows mağazası uygulaması istemci aygıtlar bir dil belirtin ve farklı son dakika haberleri kategorilere abone olmak için sağlar;
* arka uç kullanarak bildirimleri yayınlar **etiketi** ve **şablonu** Azure bildirim hub'larını feautres.

## <a name="prerequisites"></a>Ön koşullar
Önceden tamamlamış olmalıdır [son dakika haberleri göndermek için Notification Hubs kullanma] öğretici ve bu öğreticinin bu kodu doğrudan derlemeler için kullanılabilir, koda sahip.

Ayrıca Visual Studio 2012 veya üzeri gerekir.

## <a name="template-concepts"></a>Şablon kavramları
İçinde [son dakika haberleri göndermek için Notification Hubs kullanma] kullanılan bir uygulama yerleşik **etiketleri** farklı haber kategorileri için Bildirimlere abone olma.
Birçok uygulama, ancak, birden çok pazarda hedef ve yerelleştirme gerektirir. Bildirimler içeriğe sahip yerelleştirilmiş ve aygıtların doğru kümesine teslim anlamına gelir.
Bu konudaki nasıl kullanılacağını göstereceğiz **şablonu** kolayca yerelleştirilmiş son dakika haberi bildirimleri göndermek için Notification Hubs özelliğidir.

Not: yerelleştirilmiş bildirimleri göndermek için bir yol her etiket birden fazla sürümünü oluşturmaktır. Örneğin, İngilizce, Fransızca ve Mandarin desteklemek için üç farklı etiketler world haberler için ihtiyacımız: "world_en", "world_fr" ve "world_ch". Biz sonra world haber yerelleştirilmiş bir sürümünde bu etiketlerin her biri için göndermesi gerekir. Bu konudaki etiketleri artışı ve birden fazla ileti gönderme gereksinimi önlemek için şablonlar kullanın.

Yüksek bir düzeyde şablonları belirli bir aygıt bir bildirim nasıl alacağını belirtmek için bir yoldur. Şablon, uygulama arka ucu tarafından gönderilen ileti parçası olan özellikler için bakarak tam yük biçimi belirtir. Örneğimizde, biz tüm desteklenen diller içeren bir yerel ayar belirsiz ileti gönderir:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Ardından aygıtları doğru özelliğine başvuran bir şablon ile kaydetmeye sağlayacaktır. Örneği için bir basit bildirim iletisi almak istediği bir Windows mağazası uygulaması, aşağıdaki şablonu ile ilgili herhangi bir etiket için kaydeder:

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">$(News_English)</text>
        </binding>
      </visual>
    </toast>



Şablonlar, daha fazla bilgi bulabilir hakkında içinde çok güçlü bir özellik olan bizim [şablonları](notification-hubs-templates-cross-platform-push-messages.md) makalesi. 

## <a name="the-app-user-interface"></a>Uygulama kullanıcı arabirimi
Biz şimdi konu başlığı altında oluşturulan yeni haber uygulama değiştirecek [son dakika haberleri göndermek için Notification Hubs kullanma] son dakika haberleri şablonları kullanarak göndermek için yerelleştirilmiş.

Windows mağazası uygulamanızı:

Yerel ayar combobox dahil etmek için MainPage.xaml değiştirin:

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

## <a name="building-the-windows-store-client-app"></a>Windows mağazası istemci uygulaması oluşturma
1. Yerel ayar parametresi bildirimleri sınıfınıza ekleyin, *StoreCategoriesAndSubscribe* ve *SubscribeToCateories* yöntemleri.
   
        public async Task<Registration> StoreCategoriesAndSubscribe(string locale, IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            ApplicationData.Current.LocalSettings.Values["locale"] = locale;
            return await SubscribeToCategories(categories);
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
   
    Arama yerine unutmayın *RegisterNativeAsync* diyoruz yöntemi *RegisterTemplateAsync*: biz şablon bölgesel ayarına bağlıdır belirli bildirim biçimi kaydediliyor. Ayrıca ("localizedWNSTemplateExample"), şablon için bir ad (örneğin bir bildirimleri için) ve biri döşeme için birden fazla şablon kaydetmek istiyoruz ve bunları güncelleştirmek veya bunları silmek için ad ihtiyacımız çünkü sunuyoruz.
   
    Bir aygıt ile aynı etiketi birden fazla şablon kaydederse, bir gelen ileti etiketi içinde birden fazla bildirim sonuçlanacak hedefleme aygıt (her şablon için bir tane) teslim olduğunu unutmayın. Bu davranış, aynı mantıksal ileti örneği için bir Windows mağazası uygulamasında bir gösterge ve bildirim gösteren birden çok görsel bildirimler neden olduğunda yararlıdır.
2. Depolanan yerel almak için aşağıdaki yöntemi ekleyin:
   
        public string RetrieveLocale()
        {
            var locale = (string) ApplicationData.Current.LocalSettings.Values["locale"];
            return locale != null ? locale : "English";
        }
3. MainPage.xaml.cs dosyasında Güncelleştir, düğmesi gösterildiği gibi yerel açılan kutu geçerli değerini almak ve bildirimleri sınıfı çağrısına sağlayarak işleyici tıklayın:
   
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
4. Son olarak, App.xaml.cs dosyasında güncelleştirdiğinizden emin olun, `InitNotificationsAsync` yöntemi yerel almak ve abone olurken kullanın:
   
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

## <a name="send-localized-notifications-from-your-back-end"></a>Arka ucunuz yerelleştirilmiş bildirimleri gönderme
[!INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]

<!-- Anchors. -->
[Template concepts]: #concepts
[The app user interface]: #ui
[Building the Windows Store client app]: #building-client
[Send notifications from your back-end]: #send
[Next Steps]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Notify users with Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users
[son dakika haberleri göndermek için Notification Hubs kullanma]: /notification-hubs/notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns

[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-dotnet
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-dotnet
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-app-users-dotnet
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-dotnet
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
