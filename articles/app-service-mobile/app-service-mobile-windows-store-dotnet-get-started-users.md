---
title: "Evrensel Windows Platformu (UWP) uygulamanıza kimlik doğrulaması ekleme | Microsoft Docs"
description: "Evrensel Windows Platformu (UWP) uygulamanızın kimlik sağlayıcıları dahil olmak üzere, çeşitli kullanarak kullanıcıların kimlik doğrulaması için Azure App Service Mobile Apps kullanmayı öğrenin: AAD, Google, Facebook, Twitter ve Microsoft."
services: app-service\mobile
documentationcenter: windows
author: conceptdev
manager: panarasi
editor: 
ms.assetid: 6cffd951-893e-4ce5-97ac-86e3f5ad9466
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 07/05/2017
ms.author: panarasi
ms.openlocfilehash: 4cc597f8aca13445034c8a1691b41018d4d9bc4b
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2018
---
# <a name="add-authentication-to-your-windows-app"></a>Windows kimlik doğrulaması ekleme
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Bu konu, mobil uygulamanızın bulut tabanlı kimlik doğrulaması ekleme gösterir. Bu öğreticide, kimlik doğrulama için evrensel Windows Platformu (UWP) hızlı başlangıç projesi mobil Azure App Service tarafından desteklenen bir kimlik sağlayıcısı kullanarak uygulamalar için ekleyin. Başarılı bir şekilde kimliği doğrulanmış ve, mobil uygulama arka ucu tarafından yetkili sonra kullanıcı kimliği değeri görüntülenir.

Bu öğretici, Mobile Apps hızlı başlangıç üzerinde temel alır. Öğreticiyi tamamlamak [Mobile Apps'i kullanmaya başlamak](app-service-mobile-windows-store-dotnet-get-started.md).

## <a name="register"></a>Kimlik doğrulaması için uygulamanızı kaydetme ve uygulama hizmetini yapılandırma
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>Uygulamanız için izin verilen dış yönlendirme URL'leri ekleme

Uygulamanız için yeni bir URL şemasını tanımlamak güvenli kimlik doğrulaması gerektirir. Bu kimlik doğrulama işlemi tamamlandıktan sonra uygulamanıza geri yönlendirmek bir kimlik doğrulama sistemi sağlar. Bu öğreticide, URL şemasının kullanırız _appname_ boyunca. Ancak, seçtiğiniz herhangi bir URL şeması kullanabilirsiniz. Mobil uygulamanız için benzersiz olmalıdır. Sunucu tarafında yeniden yönlendirmeyi etkinleştirmek için:

1. [Azure portalında] uygulama hizmetinizi seçin.

2. Tıklatın **kimlik doğrulama / yetkilendirme** menü seçeneği.

3. İçinde **yeniden yönlendirme URL'lere izin**, girin `url_scheme_of_your_app://easyauth.callback`.  **Url_scheme_of_your_app** Bu dize, mobil uygulamanız için URL düzenidir.  Bir protokol (harf kullanın ve yalnızca sayı ve bir harf ile başlar) için normal URL belirtimi izlemelisiniz.  Çeşitli yerlerde URL şeması ile mobil uygulama kodunuzu ayarlamak ihtiyaç duyacağınız seçtiğiniz dizeyi Not olmanız gerekir.

4. **Tamam**’a tıklayın.

5. **Kaydet**’e tıklayın.

## <a name="permissions"></a>Kimliği doğrulanmış kullanıcılar için izinleri kısıtla
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Şimdi, arka adsız erişim devre dışı olduğunu doğrulayabilirsiniz. UWP uygulaması projesini başlangıç projesi olarak ayarla, dağıtmak ve uygulamayı çalıştırın; Uygulama başlatıldıktan sonra bir durum koduyla işlenmeyen bir özel durum 401 (yetkisiz) tetiklenir doğrulayın. Uygulama Kimliği doğrulanmamış bir kullanıcı olarak, mobil uygulama kodu erişmeye çünkü bu gerçekleşir ancak *Todoıtem* tablo artık kimlik doğrulaması gerektirir.

Ardından, uygulama hizmetinizden kaynakları istemeden önce kullanıcıların kimliklerini doğrulamak için uygulamayı güncelleştirir.

## <a name="add-authentication"></a>Kimlik doğrulaması için uygulama ekleme
1. UWP uygulaması proje dosyası MainPage.xaml.cs ve aşağıdaki kod parçacığını ekleyin:
   
        // Define a member variable for storing the signed-in user. 
        private MobileServiceUser user;
   
        // Define a method that performs the authentication process
        // using a Facebook sign-in. 
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
            try
            {
                // Change 'MobileService' to the name of your MobileServiceClient instance.
                // Sign-in using Facebook authentication.
                user = await App.MobileService
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                message =
                    string.Format("You are now signed in - {0}", user.UserId);
   
                success = true;
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }
   
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
            return success;
        }
   
    Bu kod bir Facebook oturum açma ile kullanıcı kimliğini doğrular. Facebook dışında bir kimlik sağlayıcısı kullanıyorsanız, değerini değiştirme **MobileServiceAuthenticationProvider** yukarıda sağlayıcınız için değer.
2. Değiştir **OnNavigatedTo()** MainPage.xaml.cs yöntemi. Sonra ekleyeceksiniz bir **oturum** kimlik doğrulamasını tetikler App düğmesi.

        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            if (e.Parameter is Uri)
            {
                App.MobileService.ResumeWithURL(e.Parameter as Uri);
            }
        }

3. Aşağıdaki kod parçacığını MainPage.xaml.cs ekleyin:
   
        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Switch the buttons and load items from the mobile app.
                ButtonLogin.Visibility = Visibility.Collapsed;
                ButtonSave.Visibility = Visibility.Visible;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
4. MainPage.xaml proje dosyasını açın, tanımlar öğesini bulun **kaydetmek** düğmesine tıklayın ve aşağıdaki kod ile değiştirin:
   
        <Button Name="ButtonSave" Visibility="Collapsed" Margin="0,8,8,0" 
                Click="ButtonSave_Click">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Add"/>
                <TextBlock Margin="5">Save</TextBlock>
            </StackPanel>
        </Button>
        <Button Name="ButtonLogin" Visibility="Visible" Margin="0,8,8,0" 
                Click="ButtonLogin_Click" TabIndex="0">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Permissions"/>
                <TextBlock Margin="5">Sign in</TextBlock> 
            </StackPanel>
        </Button>
5. Aşağıdaki kod parçacığını App.xaml.cs ekleyin:

        protected override void OnActivated(IActivatedEventArgs args)
        {
            if (args.Kind == ActivationKind.Protocol)
            {
                ProtocolActivatedEventArgs protocolArgs = args as ProtocolActivatedEventArgs;
                Frame content = Window.Current.Content as Frame;
                if (content.Content.GetType() == typeof(MainPage))
                {
                    content.Navigate(typeof(MainPage), protocolArgs.Uri);
                }
            }
            Window.Current.Activate();
            base.OnActivated(args);
        }
6. Package.appxmanifest dosyasını açın ve gidin **bildirimleri**, **kullanılabilir bildirimleri** açılır listesinden, **Protokolü** tıklatıp **Ekle** düğmesi. Şimdi yapılandırmak **özellikleri** , **Protokolü** bildirimi. İçinde **görünen adı**, uygulamanızın kullanıcılara görüntülenmesini istediğiniz adı ekleyin. İçinde **adı**, {url_scheme_of_your_app} ekleyin.
7. Uygulamayı çalıştırmak için F5 tuşuna basarak **oturum** düğmesi ve seçilen kimlik sağlayıcınızı uygulamada oturum. Oturum açma işleminiz başarılı olduktan sonra uygulama hatasız çalışır ve arka sorgu ve veri güncelleştirmeleri yapabilirler.

## <a name="tokens"></a>Mağaza istemci kimlik doğrulama belirteci
Önceki örnekte oturum açma, kimlik sağlayıcısı ve App Service uygulama başladıktan her zaman başvurun istemciye gerektiren standart gösterdi. Bu yöntem yalnızca çalıştırabilirsiniz verimsiz olduğu halinde kullanım-sorunları aynı anda uygulamasını başlatmak birçok müşteri deneyin ilişkilendirir. Daha iyi bir yaklaşım olduğu App Service tarafından döndürülen yetkilendirme belirtecini önbelleğe ve bir sağlayıcı tabanlı oturum açma kullanmadan önce bu ilk kullanmayı deneyin.

> [!NOTE]
> Uygulama Hizmetleri tarafından yönetilen istemci veya hizmet tarafından yönetilen kimlik doğrulaması kullanıp bağımsız olarak verilen belirteç önbelleğe alabilir. Bu öğretici, kimlik doğrulama hizmeti tarafından yönetilen kullanır.
> 
> 

[!INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bu temel kimlik doğrulaması öğreticisini tamamladığınıza göre aşağıdaki öğreticiler birini açın etmeden göz önünde bulundurun:

* [Uygulamanıza anında iletme bildirimleri ekleme](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Uygulamanıza anında iletme bildirimleri desteği eklemeyi ve anında iletme bildirimleri göndermek için Azure Notification Hubs’ı kullanmak üzere Mobile App arka ucunuzu yapılandırmayı öğrenin.
* [Uygulamanız için çevrimdışı eşitlemeyi etkinleştirme](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Mobil Uygulama arka ucu kullanarak uygulamanıza çevrimdışı destek eklemeyi öğrenin. Çevrimdışı eşitleme son kullanıcıların, ağ bağlantısı yokken dahi, mobil uygulama ile etkileşim kurmalarına &mdash;veri görüntüleme, ekleme ya da değiştirme&mdash; olanak tanır.

<!-- URLs. -->
[Get started with your mobile app]: app-service-mobile-windows-store-dotnet-get-started.md
