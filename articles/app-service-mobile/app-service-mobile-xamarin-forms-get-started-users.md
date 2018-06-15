---
title: Mobil uygulamaları için Xamarin Forms uygulamasında kimlik doğrulamasını kullanmaya başlama | Microsoft Docs
description: Mobile Apps kimlik sağlayıcıları, AAD, Google, Facebook, Twitter ve Microsoft dahil olmak üzere çeşitli Xamarin Forms uygulamanızdaki kullanıcıların kimliklerini doğrulamak için nasıl kullanılacağını öğrenin.
services: app-service\mobile
documentationcenter: xamarin
author: panarasi
manager: crdun
editor: ''
ms.assetid: 9c55e192-c761-4ff2-8d88-72260e9f6179
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 08/07/2017
ms.author: panarasi
ms.openlocfilehash: e3e8c843437558c6d5d3a3c39bed1e647f852b18
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2018
ms.locfileid: "27593407"
---
# <a name="add-authentication-to-your-xamarin-forms-app"></a>Xamarin Forms kimlik doğrulaması ekleme
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="overview"></a>Genel Bakış
Bu konuda bir mobil uygulama hizmeti istemci uygulamanızdan kullanıcıların kimliklerini gösterilmiştir. Bu öğretici kapsamında, kimlik doğrulama App Service tarafından desteklenen bir kimlik sağlayıcısı kullanarak Xamarin Forms hızlı başlangıç projesi ekleyin. Mobil uygulamanız tarafından yetkili başarılı bir şekilde kimliği doğrulanmış ve sonra kullanıcı kimliği değeri görüntülenir ve kısıtlı tablo veri erişimi olacaktır.

## <a name="prerequisites"></a>Önkoşullar
Bu öğretici ile en iyi sonuç için önce tamamlamanızı öneririz [Xamarin Forms uygulaması oluşturma] [ 1] Öğreticisi. Bu öğreticiyi tamamladıktan sonra bir çok platformlu Yapılacaklar listesi uygulamasıyla Xamarin Forms proje sahip olur.

İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, kimlik doğrulaması uzantısı paketini projenize eklemeniz gerekir. Server uzantısı paketleri hakkında daha fazla bilgi için bkz: [.NET arka uç sunucusu SDK ile Azure Mobile Apps için iş][2].

## <a name="register-your-app-for-authentication-and-configure-app-services"></a>Kimlik doğrulaması için uygulamanızı kaydetme ve uygulama hizmetlerini yapılandırma
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>Uygulamanız için izin verilen dış yönlendirme URL'leri ekleme

Uygulamanız için yeni bir URL şemasını tanımlamak güvenli kimlik doğrulaması gerektirir. Bu kimlik doğrulama işlemi tamamlandıktan sonra uygulamanıza geri yönlendirmek bir kimlik doğrulama sistemi sağlar. Bu öğreticide, URL şemasının kullanırız _appname_ boyunca. Ancak, seçtiğiniz herhangi bir URL şeması kullanabilirsiniz. Mobil uygulamanız için benzersiz olmalıdır. Sunucu tarafında yeniden yönlendirmeyi etkinleştirmek için:

1. İçinde [Azure portal][8], uygulama hizmetinizi seçin.

2. Tıklatın **kimlik doğrulama / yetkilendirme** menü seçeneği.

3. İçinde **yeniden yönlendirme URL'lere izin**, girin `url_scheme_of_your_app://easyauth.callback`.  **Url_scheme_of_your_app** Bu dize, mobil uygulamanız için URL düzenidir.  Bir protokol (harf kullanın ve yalnızca sayı ve bir harf ile başlar) için normal URL belirtimi izlemelisiniz.  Çeşitli yerlerde URL şeması ile mobil uygulama kodunuzu ayarlamak ihtiyaç duyacağınız seçtiğiniz dizeyi Not olmanız gerekir.

4. **Tamam**’a tıklayın.

5. **Kaydet**’e tıklayın.

## <a name="restrict-permissions-to-authenticated-users"></a>Kimliği doğrulanmış kullanıcılar için izinleri kısıtla
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

## <a name="add-authentication-to-the-portable-class-library"></a>Taşınabilir Sınıf Kitaplığı'na kimlik doğrulaması ekleme
Mobile Apps kullanan [LoginAsync] [ 3] genişletme yöntemi [MobileServiceClient] [ 4] uygulama hizmeti ile bir kullanıcıyla oturum açmak için kimlik doğrulaması. Bu örnek uygulamada oturum açma sağlayıcısı'nın arabirimi görüntüleyen bir sunucu yönetilen kimlik doğrulama akışı kullanır. Daha fazla bilgi için bkz: [kimlik doğrulama sunucusu yönetilen][5]. Üretim uygulamanızda daha iyi bir kullanıcı deneyimi sağlamak için dikkate almanız gereken kullanmayı [istemci yönetilen kimlik doğrulaması][6].

Xamarin Forms projesi ile kimlik doğrulaması için tanımlama bir **IAuthenticate** uygulama için taşınabilir Sınıf Kitaplığı'nda arabirimi. Ardından ekleyin bir **oturum açma** taşınabilir sınıf hangi kimlik doğrulama başlatmak için tıklatın Kitaplığı'nda tanımlanan kullanıcı arabirimi düğme. Veriler mobil uygulama arka ucundan başarılı kimlik doğrulamasından sonra yüklenir.

Uygulama **IAuthenticate** uygulamanız tarafından desteklenen her platform için arabirim.

1. Visual Studio veya Xamarin Studio'da ile projenizden App.cs açın **taşınabilir** taşınabilir sınıf kitaplığı proje olan adlarında sonra aşağıdakileri ekleyin `using` deyimi:

        using System.Threading.Tasks;
2. App.cs içinde aşağıdaki ekleme `IAuthenticate` tanımı hemen önce arabirim `App` sınıf tanımının.

        public interface IAuthenticate
        {
            Task<bool> Authenticate();
        }
3. Platforma özgü bir uygulama arabirimi başlatmak için aşağıdaki statik üyeleri Ekle **uygulama** sınıfı.

        public static IAuthenticate Authenticator { get; private set; }

        public static void Init(IAuthenticate authenticator)
        {
            Authenticator = authenticator;
        }
4. Taşınabilir sınıf kitaplığı projesinden TodoList.xaml açın, aşağıdakileri ekleyin **düğmesini** öğesinde *buttonsPanel* varolan bir düğmeyi sonra Düzen öğesi:

          <Button x:Name="loginButton" Text="Sign-in" MinimumHeightRequest="30"
            Clicked="loginButton_Clicked"/>

    Bu düğme, mobil uygulama arka ucu ile kimlik doğrulama sunucusu yönetilen tetikler.
5. Taşınabilir sınıf kitaplığı projesinden TodoList.xaml.cs açın, ardından aşağıdaki alana ekleyin `TodoList` sınıfı:

        // Track whether the user has authenticated.
        bool authenticated = false;
6. Değiştir **OnAppearing** aşağıdaki kod ile yöntemi:

        protected override async void OnAppearing()
        {
            base.OnAppearing();

            // Refresh items only when authenticated.
            if (authenticated == true)
            {
                // Set syncItems to true in order to synchronize the data
                // on startup when running in offline mode.
                await RefreshItems(true, syncItems: false);

                // Hide the Sign-in button.
                this.loginButton.IsVisible = false;
            }
        }

    Bu kod, doğrulanan sonra veriler yalnızca hizmetinden yenilenir emin olur.
7. Aşağıdaki işleyicisi ekleme **tıklama** olaya **TodoList** sınıfı:

        async void loginButton_Clicked(object sender, EventArgs e)
        {
            if (App.Authenticator != null)
                authenticated = await App.Authenticator.Authenticate();

            // Set syncItems to true to synchronize the data on startup when offline is enabled.
            if (authenticated == true)
                await RefreshItems(true, syncItems: false);
        }
8. Değişikliklerinizi kaydetmek ve hiçbir hata doğrulama taşınabilir sınıf kitaplığı proje derleyin.

## <a name="add-authentication-to-the-android-app"></a>Android uygulaması için kimlik doğrulaması ekleme
Bu bölümde nasıl uygulandığını gösterir **IAuthenticate** Android uygulama projesi arabiriminde. Android cihazları destekleme değil, bu bölümü atlayabilirsiniz.

1. Visual Studio veya Xamarin Studio'da sağ **droid** , sonra da proje **başlangıç projesi olarak ayarla**.
2. Projeyi Hata Ayıklayıcısı'ndaki başlayın, ardından uygulama başladıktan sonra durum koduyla işlenmeyen bir özel durum 401 (yetkisiz) tetiklenir doğrulamak için F5 tuşuna basın. Arka uç erişimi yalnızca yetkili kullanıcılar ile sınırlı olduğundan 401 kodu oluşturulur.
3. MainActivity.cs Android projeyi açın ve aşağıdakileri ekleyin `using` deyimleri:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
4. Güncelleştirme **MainActivity** uygulamak için sınıf **IAuthenticate** arabirimi, aşağıdaki gibi:

        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity, IAuthenticate
5. Güncelleştirme **MainActivity** ekleyerek sınıfı bir **MobileServiceUser** alan ve bir **kimlik doğrulama** gerekli yöntemi **IAuthenticate** arabirimi, aşağıdaki gibi:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(this, 
                    MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                if (user != null)
                {
                    message = string.Format("you are now signed-in as {0}.",
                        user.UserId);
                    success = true;
                }
            }
            catch (Exception ex)
            {
                message = ex.Message;
            }

            // Display the success or failure message.
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.SetMessage(message);
            builder.SetTitle("Sign-in result");
            builder.Create().Show();

            return success;
        }

    Facebook dışında bir kimlik sağlayıcısı kullanıyorsanız, için farklı bir değer seçin [MobileServiceAuthenticationProvider][7].

6. Güncelleştirme **AndroidManifest.xml** dosyasını aşağıdaki XML içine ekleyerek `<application>` öğe:

    ```xml
    <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity" android:launchMode="singleTop" android:noHistory="true">
      <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="{url_scheme_of_your_app}" android:host="easyauth.callback" />
      </intent-filter>
    </activity>
    ```
    Değiştir `{url_scheme_of_your_app}` URL şemasına sahip.
7. Aşağıdaki kodu ekleyin **OnCreate** yöntemi **MainActivity** çağırmadan önce sınıfın `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        App.Init((IAuthenticate)this);

    Bu kod, Doğrulayıcı uygulama yükleri önce başlatılmış sağlar.
8. Uygulama yeniden, çalıştırın, ardından seçtiğiniz ve kimliği doğrulanmış bir kullanıcı olarak verilerine erişebilir doğrulayın kimlik doğrulama sağlayıcısının oturum açın.

## <a name="add-authentication-to-the-ios-app"></a>İOS uygulaması için kimlik doğrulaması ekleme
Bu bölümde nasıl uygulandığını gösterir **IAuthenticate** iOS uygulaması projesi arabiriminde. İOS cihazları destekleme değil, bu bölümü atlayabilirsiniz.

1. Visual Studio veya Xamarin Studio'da sağ **iOS** , sonra da proje **başlangıç projesi olarak ayarla**.
2. Projeyi Hata Ayıklayıcısı'ndaki başlayın, ardından uygulama başladıktan sonra durum koduyla işlenmeyen bir özel durum 401 (yetkisiz) tetiklenir doğrulamak için F5 tuşuna basın. Arka uç erişimi yalnızca yetkili kullanıcılar ile sınırlı olduğundan 401 yanıtı oluşturulur.
3. AppDelegate.cs iOS projeyi açın ve aşağıdakileri ekleyin `using` deyimleri:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
4. Güncelleştirme **AppDelegate** uygulamak için sınıf **IAuthenticate** arabirimi, aşağıdaki gibi:

        public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate, IAuthenticate
5. Güncelleştirme **AppDelegate** ekleyerek sınıfı bir **MobileServiceUser** alan ve bir **kimlik doğrulama** gerekli yöntemi **IAuthenticate** arabirimi, aşağıdaki gibi:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(UIApplication.SharedApplication.KeyWindow.RootViewController,
                        MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                    if (user != null)
                    {
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                        success = true;
                    }
                }
            }
            catch (Exception ex)
            {
               message = ex.Message;
            }

            // Display the success or failure message.
            UIAlertView avAlert = new UIAlertView("Sign-in result", message, null, "OK", null);
            avAlert.Show();

            return success;
        }

    Facebook dışında bir kimlik sağlayıcısı kullanıyorsanız, [MobileServiceAuthenticationProvider] için farklı bir değer seçin.
    
6. Güncelleştirme **AppDelegate** ekleyerek sınıfı **OpenUrl** yöntemi aşırı yükleme, aşağıdaki gibi:

        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            return TodoItemManager.DefaultManager.CurrentClient.ResumeWithURL(url);
        }
   
7. Aşağıdaki kod satırını ekleyin **FinishedLaunching** yöntemi çağırmadan önce `LoadApplication()`:

        App.Init(this);

    Bu kod, Doğrulayıcı uygulama yüklenmeden önce başlatılmış sağlar.

8. Info.plist açın ve eklemek bir **URL türü**. Ayarlama **tanımlayıcısı** seçtiğiniz, bir adla **URL şemalarını** , uygulamanız için URL şeması için ve **rol** yok.

9. Uygulama yeniden, çalıştırın, ardından seçtiğiniz ve kimliği doğrulanmış bir kullanıcı olarak verilerine erişebilir doğrulayın kimlik doğrulama sağlayıcısının oturum açın.

## <a name="add-authentication-to-windows-10-including-phone-app-projects"></a>Windows 10 (Phone dahil) uygulaması projeleri için kimlik doğrulaması ekleme
Bu bölümde nasıl uygulandığını gösterir **IAuthenticate** Windows 10 uygulaması projeleri arabiriminde. Evrensel Windows Platformu (UWP) projeleri, ancak kullanarak için aynı adımları uygulamak **UWP** projeyle (not ettiğiniz değişir). Windows cihazları destekleme değil, bu bölümü atlayabilirsiniz.

1. Visual Studio'da sağ **UWP** , sonra da proje **başlangıç projesi olarak ayarla**.
2. Projeyi Hata Ayıklayıcısı'ndaki başlayın, ardından uygulama başladıktan sonra durum koduyla işlenmeyen bir özel durum 401 (yetkisiz) tetiklenir doğrulamak için F5 tuşuna basın. Arka uç erişimi yalnızca yetkili kullanıcılar ile sınırlı olduğundan 401 yanıt olur.
3. Windows uygulama projesi için MainPage.xaml.cs açın ve aşağıdakileri ekleyin `using` deyimleri:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.UI.Popups;
        using <your_Portable_Class_Library_namespace>;

    Değiştir `<your_Portable_Class_Library_namespace>` ile taşınabilir sınıf kitaplığı için ad alanı.
4. Güncelleştirme **MainPage** uygulamak için sınıf **IAuthenticate** arabirimi, aşağıdaki gibi:

        public sealed partial class MainPage : IAuthenticate
5. Güncelleştirme **MainPage** ekleyerek sınıfı bir **MobileServiceUser** alan ve bir **kimlik doğrulama** gerekli yöntemi **IAuthenticate**arabirimi, aşağıdaki gibi:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            string message = string.Empty;
            var success = false;

            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                    if (user != null)
                    {
                        success = true;
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                    }
                }

            }
            catch (Exception ex)
            {
                message = string.Format("Authentication Failed: {0}", ex.Message);
            }

            // Display the success or failure message.
            await new MessageDialog(message, "Sign-in result").ShowAsync();

            return success;
        }

    Facebook dışında bir kimlik sağlayıcısı kullanıyorsanız, için farklı bir değer seçin [MobileServiceAuthenticationProvider][7].

1. Aşağıdaki kod satırını Oluşturucusu eklemek **MainPage** çağırmadan önce sınıfın `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        <your_Portable_Class_Library_namespace>.App.Init(this);

    Değiştir `<your_Portable_Class_Library_namespace>` ile taşınabilir sınıf kitaplığı için ad alanı.

3. Kullanıyorsanız **UWP**, aşağıdakileri ekleyin **OnActivated** yöntemi geçersiz kılma **uygulama** sınıfı:

       protected override void OnActivated(IActivatedEventArgs args)
       {
           base.OnActivated(args);

            if (args.Kind == ActivationKind.Protocol)
            {
                ProtocolActivatedEventArgs protocolArgs = args as ProtocolActivatedEventArgs;
                TodoItemManager.DefaultManager.CurrentClient.ResumeWithURL(protocolArgs.Uri);
            }
       }

3. Package.appxmanifest açın ve eklemek bir **Protokolü** bildirimi. Ayarlama **görünen adı** için seçtiğiniz, bir ad ve **adı** , uygulama için URL şeması için.

4. Uygulama yeniden, çalıştırın, ardından seçtiğiniz ve kimliği doğrulanmış bir kullanıcı olarak verilerine erişebilir doğrulayın kimlik doğrulama sağlayıcısının oturum açın.

## <a name="next-steps"></a>Sonraki adımlar
Bu temel kimlik doğrulaması öğreticisini tamamladığınıza göre aşağıdaki öğreticiler birini açın etmeden göz önünde bulundurun:

* [Uygulamanıza anında iletme bildirimleri ekleme](app-service-mobile-xamarin-forms-get-started-push.md)

  Uygulamanıza anında iletme bildirimleri desteği eklemeyi ve anında iletme bildirimleri göndermek için Azure Notification Hubs’ı kullanmak üzere Mobile App arka ucunuzu yapılandırmayı öğrenin.
* [Uygulamanız için çevrimdışı eşitlemeyi etkinleştirme](app-service-mobile-xamarin-forms-get-started-offline-data.md)

  Bir mobil uygulama arka ucu kullanarak uygulamanıza çevrimdışı destek eklemeyi öğrenin. Çevrimdışı eşitleme son kullanıcıların görüntüleme, ekleme veya ağ bağlantısı olduğunda bile veri - değiştirme bir mobil uygulamayla - etkileşime olanak sağlar.

<!-- Images. -->

<!-- URLs. -->
[1]: app-service-mobile-xamarin-forms-get-started.md
[2]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[3]: https://msdn.microsoft.com/library/azure/dn268341(v=azure.10).aspx
[4]: https://msdn.microsoft.com/library/azure/JJ553674(v=azure.10).aspx
[5]: app-service-mobile-dotnet-how-to-use-client-library.md#serverflow
[6]: app-service-mobile-dotnet-how-to-use-client-library.md#clientflow
[7]: https://msdn.microsoft.com/library/azure/jj730936(v=azure.10).aspx
[8]: https://portal.azure.com
