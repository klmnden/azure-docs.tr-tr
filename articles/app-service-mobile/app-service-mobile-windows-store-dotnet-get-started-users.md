---
title: Evrensel Windows Platformu (UWP) uygulamanıza kimlik doğrulaması ekleme | Microsoft Docs
description: 'Azure App Service Mobile Apps, çeşitli kimlik sağlayıcılar dahil olmak üzere, kullanarak evrensel Windows Platformu (UWP) uygulamanızdaki kullanıcıların kimliğini doğrulamak için kullanmayı öğrenin: AAD, Google, Facebook, Twitter ve Microsoft.'
services: app-service\mobile
documentationcenter: windows
author: conceptdev
manager: panarasi
editor: ''
ms.assetid: 6cffd951-893e-4ce5-97ac-86e3f5ad9466
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 07/05/2017
ms.author: panarasi
ms.openlocfilehash: 7caaa1ca4cdaf7290b7ce05d17c07e565e7b51d1
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59698691"
---
# <a name="add-authentication-to-your-windows-app"></a>Windows kimlik doğrulaması ekleme
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Bu konuda, mobil uygulamanıza bulut tabanlı kimlik doğrulaması ekleme işlemini göstermektedir. Bu öğreticide, kimlik doğrulaması için evrensel Windows Platformu (UWP) hızlı başlangıç projesi için Mobile Apps, Azure App Service tarafından desteklenen bir kimlik sağlayıcısı kullanarak ekleyin. Başarıyla kimlik doğrulaması ve yetkili tarafından Mobile App arka ucunuzu sonra kullanıcı kimliği değeri görüntülenir.

Bu öğretici, Mobile Apps hızlı başlangıcı temel alır. Öğreticiyi tamamlamak [Mobile Apps'i kullanmaya başlama](app-service-mobile-windows-store-dotnet-get-started.md).

## <a name="register"></a>Kimlik doğrulaması için uygulamanızı kaydetme ve App Service'ı yapılandırma
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>İzin verilen dış yönlendirme URL'leri uygulamanıza ekleyin

Uygulamanız için yeni bir URL şemasını tanımlamak güvenli kimlik doğrulaması gerektirir. Bu kimlik doğrulama işlemi tamamlandıktan sonra uygulamanıza geri yönlendirmek bir kimlik doğrulama sistemi sağlar. Bu öğreticide, kullandığımız URL şeması _appname_ boyunca. Ancak, seçtiğiniz herhangi bir URL şeması kullanabilirsiniz. Mobil uygulamanız için benzersiz olmalıdır. Sunucu tarafında yeniden yönlendirmeyi etkinleştirmek için:

1. İçinde [Azure portalında](https://ms.portal.azure.com), App Service'ı seçin.

2. Tıklayın **kimlik doğrulama / yetkilendirme** menü seçeneği.

3. İçinde **izin verilen dış yönlendirme URL'leri**, girin `url_scheme_of_your_app://easyauth.callback`.  **Url_scheme_of_your_app** bu dizesinde mobil uygulamanız için URL şeması aşağıdaki gibidir.  Bu, bir protokol (kullanım harf ve yalnızca sayı ve bir harfle) için normal URL belirtimi izlemeniz gerekir.  Çeşitli yerlerde URL şeması ile mobil uygulama kodunuzu ayarlamak kullanmanız gerektiğinden, seçtiğiniz dizenin Not.

4. **Kaydet**’e tıklayın.

## <a name="permissions"></a>Kimliği doğrulanmış kullanıcılar için izinleri kısıtla
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Şimdi arka ucunuzu anonim erişim devre dışı olduğunu doğrulayabilirsiniz. UWP uygulaması projesini başlangıç projesi olarak ayarla, dağıtım ve uygulamayı çalıştırın; uygulama başladıktan sonra işlenmeyen bir özel durum ile bir durum kodu 401 (yetkisiz) tetiklenir doğrulayın. Uygulama Kimliği doğrulanmamış bir kullanıcı olarak mobil uygulama kodunuzu erişmeye çünkü böyle ancak *Todoıtem* tablo artık kimlik doğrulaması gerektirir.

Ardından, kaynaklar, App Service'ten istemeden önce kullanıcıların kimliklerini doğrulamak için uygulamayı güncelleştirir.

## <a name="add-authentication"></a>Uygulamaya kimlik doğrulaması ekleme
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
   
    Bu kod bir Facebook oturum açma ile kullanıcı kimliğini doğrular. Facebook dışında bir kimlik sağlayıcısı kullanıyorsanız, değiştirin **MobileServiceAuthenticationProvider** yukarıda sağlayıcınız için değer.
2. Değiştirin **OnNavigatedTo()** MainPage.xaml.cs yöntemi. Sonra ekleyeceksiniz bir **oturum** kimlik doğrulamasını tetikler uygulamaya düğmesi.

        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            if (e.Parameter is Uri)
            {
                App.MobileService.ResumeWithURL(e.Parameter as Uri);
            }
        }

3. MainPage.xaml.cs için aşağıdaki kod parçacığını ekleyin:
   
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
4. MainPage.xaml proje dosyasını açın, tanımlayan bir öğe bulun **Kaydet** düğmesine tıklayın ve aşağıdaki kodla değiştirin:
   
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
5. App.xaml.cs için aşağıdaki kod parçacığını ekleyin:

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
6. Package.appxmanifest dosyasını açın ve gidin **bildirimleri**, **kullanılabilir bildirimler** açılan listesinden **Protokolü** tıklatıp **Ekle** düğmesi. Şimdi Yapılandır **özellikleri** , **Protokolü** bildirimi. İçinde **görünen ad**, uygulamanızın kullanıcılara görüntülenmesini istediğiniz adı ekleyin. İçinde **adı**, {url_scheme_of_your_app} ekleyin.
7. Uygulamayı çalıştırmak için F5 tuşuna **oturum** düğmesi ve uygulamayı, seçtiğiniz kimlik sağlayıcısı ile oturum açın. Oturum açma işleminiz başarılı olduktan sonra uygulama hatasız çalışır ve arka uç sorgu ve veri güncelleştirmelerini yapın.

## <a name="tokens"></a>Store istemci kimlik doğrulama belirteci
Önceki örnekte oturum açma, kimlik sağlayıcısı hem de App Service uygulama başladıktan her seferinde başvurmak istemci gerektiren standart gösterdi. Bu yöntem yalnızca çalıştırabilirsiniz verimsiz bir durumdur içine kullanımı-sorunları uygulamasını aynı anda başlatmak birçok müşteri deneyin ilişkilendirir. Daha iyi bir yaklaşım, önbellek, App Service tarafından döndürülen yetkilendirme belirtecini ve bir sağlayıcı tabanlı oturum açma kullanmadan önce bu ilk kullanmaya çalıştığınızda oluşturmaktır.

> [!NOTE]
> Uygulama Hizmetleri tarafından yönetilen ya da hizmet tarafından yönetilen bir kimlik doğrulama kullanmanıza bakılmaksızın verilen belirtecin önbelleğe alabilir. Bu öğretici, hizmet tarafından yönetilen kimlik doğrulaması kullanır.
> 
> 

[!INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bu temel kimlik doğrulama öğreticisini tamamladığınıza göre aşağıdaki öğreticilerden birine açın etmeden göz önünde bulundurun:

* [Uygulamanıza anında iletme bildirimleri ekleme](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Uygulamanıza anında iletme bildirimleri desteği eklemeyi ve anında iletme bildirimleri göndermek için Azure Notification Hubs’ı kullanmak üzere Mobile App arka ucunuzu yapılandırmayı öğrenin.
* [Uygulamanız için çevrimdışı eşitlemeyi etkinleştirme](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Mobil Uygulama arka ucu kullanarak uygulamanıza çevrimdışı destek eklemeyi öğrenin. Çevrimdışı eşitleme son kullanıcıların, ağ bağlantısı yokken dahi, mobil uygulama ile etkileşim kurmalarına &mdash;veri görüntüleme, ekleme ya da değiştirme&mdash; olanak tanır.

<!-- URLs. -->
[Get started with your mobile app]: app-service-mobile-windows-store-dotnet-get-started.md
