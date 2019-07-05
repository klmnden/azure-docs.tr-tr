---
title: Xamarin Forms uygulaması, Mobile Apps için kimlik doğrulamasını kullanmaya başlama | Microsoft Docs
description: Kimlik sağlayıcıları, AAD, Google, Facebook, Twitter ve Microsoft gibi çeşitli Xamarin Forms uygulamanızdaki kullanıcıların kimliğini doğrulamak için Mobile Apps'ı kullanmayı öğrenin.
services: app-service\mobile
documentationcenter: xamarin
author: elamalani
manager: crdun
editor: ''
ms.assetid: 9c55e192-c761-4ff2-8d88-72260e9f6179
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 06/25/2019
ms.author: emalani
ms.openlocfilehash: f1777fcb5a4e7899da982bd9d1d35905cb408ad2
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446298"
---
# <a name="add-authentication-to-your-xamarin-forms-app"></a>Xamarin Forms kimlik doğrulaması ekleme
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

> [!NOTE]
> Visual Studio App Center, mobil uygulama geliştirme merkezi hizmetlerinde yeni ve tümleşik yatırım yapıyor. Geliştiriciler **derleme**, **Test** ve **Dağıt** hizmetlerinin sürekli tümleştirme ve teslim işlem hattı ayarlayın. Uygulama dağıtıldığında, geliştiriciler kendi uygulamasını kullanarak kullanımı ve durumu izleyebilirsiniz **Analytics** ve **tanılama** kullanarak kullanıcılarla etkileşim kurun ve hizmetlerini **anında iletme** hizmeti. Geliştiriciler de yararlanabilir **Auth** , kullanıcıların kimliğini doğrulamak ve **veri** kalıcı hale getirmek ve uygulama verilerini bulutta eşitleme hizmeti. Kullanıma [App Center](https://appcenter.ms/?utm_source=zumo&utm_campaign=app-service-mobile-xamarin-forms-get-started-users) bugün.
>

## <a name="overview"></a>Genel Bakış
Bu konu bir App Service mobil uygulama istemci uygulamanızın kullanıcılarının kimlik doğrulaması yapmayı gösterir. Bu öğreticide, App Service tarafından desteklenen bir kimlik sağlayıcısı kullanarak Xamarin Forms hızlı başlangıç projesi için kimlik doğrulaması ekleyin. Mobil uygulamanız tarafından yetkili başarıyla yapıldığını ve sonra kullanıcı kimliği değeri görüntülenir ve kısıtlı tablo verilerine erişmek mümkün olacaktır.

## <a name="prerequisites"></a>Önkoşullar
Bu öğretici ile en iyi sonuç için önce tamamlamanızı öneririz [bir Xamarin Forms uygulaması oluşturma][1] öğretici. Bu öğreticiyi tamamladıktan sonra bir çoklu platform Yapılacaklar listesi uygulaması bir Xamarin.Forms projesi olacaktır.

İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, kimlik doğrulaması uzantı paketi projenize eklemeniz gerekir. Server uzantısı paketleri hakkında daha fazla bilgi için bkz. [Azure Mobile Apps için .NET arka uç sunucu SDK'sı ile çalışma][2].

## <a name="register-your-app-for-authentication-and-configure-app-services"></a>Kimlik doğrulaması için uygulamanızı kaydetme ve uygulama Hizmetleri'ı yapılandırma
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>İzin verilen dış yönlendirme URL'leri uygulamanıza ekleyin

Uygulamanız için yeni bir URL şemasını tanımlamak güvenli kimlik doğrulaması gerektirir. Bu kimlik doğrulama işlemi tamamlandıktan sonra uygulamanıza geri yönlendirmek bir kimlik doğrulama sistemi sağlar. Bu öğreticide, kullandığımız URL şeması _appname_ boyunca. Ancak, seçtiğiniz herhangi bir URL şeması kullanabilirsiniz. Mobil uygulamanız için benzersiz olmalıdır. Sunucu tarafında yeniden yönlendirmeyi etkinleştirmek için:

1. İçinde [Azure portalında][8], App Service'ı seçin.

2. Tıklayın **kimlik doğrulama / yetkilendirme** menü seçeneği.

3. İçinde **izin verilen dış yönlendirme URL'leri**, girin `url_scheme_of_your_app://easyauth.callback`.  **Url_scheme_of_your_app** bu dizesinde mobil uygulamanız için URL şeması aşağıdaki gibidir.  Bu, bir protokol (kullanım harf ve yalnızca sayı ve bir harfle) için normal URL belirtimi izlemeniz gerekir.  Çeşitli yerlerde URL şeması ile mobil uygulama kodunuzu ayarlamak kullanmanız gerektiğinden, seçtiğiniz dizenin Not.

4. **Tamam**'ı tıklatın.

5. **Kaydet**’e tıklayın.

## <a name="restrict-permissions-to-authenticated-users"></a>Kimliği doğrulanmış kullanıcılar için izinleri kısıtla
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

## <a name="add-authentication-to-the-portable-class-library"></a>Taşınabilir sınıf kitaplığı için kimlik doğrulaması ekleme
Mobile Apps kullanan [LoginAsync][3] extension method on the [MobileServiceClient][4] to sign in a user with App Service authentication. This sample
uses a server-managed authentication flow that displays the provider's sign-in interface in the app. For more information, see [Server-managed authentication][5]. Üretim uygulamanızdaki daha iyi bir kullanıcı deneyimi sağlamak için dikkate almanız gereken kullanmayı [yönetilen kimlik doğrulaması][6].

Bir Xamarin.Forms projesi ile kimlik doğrulaması için tanımladığınız bir **IAuthenticate** arabirimi uygulaması için taşınabilir Sınıf Kitaplığı'nda. Ardından Ekle bir **oturum** tanımlanan taşınabilir sınıf hangi kimlik doğrulaması'nı başlatmak için tıklatın kitaplığında, düğme kullanıcı arabirimi. Veriler mobil uygulama arka ucundan başarılı kimlik doğrulamasından sonra yüklenir.

Uygulama **IAuthenticate** uygulamanız tarafından desteklenen her platform için arabirim.

1. Visual Studio veya Xamarin Studio'da, projeyle App.cs açık **taşınabilir** , taşınabilir sınıf kitaplığı proje adı, ardından aşağıdakileri ekleyin `using` deyimi:

        using System.Threading.Tasks;
2. App.cs içinde aşağıdaki ekleme `IAuthenticate` tanımı hemen önce arabirim `App` sınıf tanımını.

        public interface IAuthenticate
        {
            Task<bool> Authenticate();
        }
3. Platforma özgü uygulama arabirimi başlatmak için aşağıdaki statik üyeleri ekleme **uygulama** sınıfı.

        public static IAuthenticate Authenticator { get; private set; }

        public static void Init(IAuthenticate authenticator)
        {
            Authenticator = authenticator;
        }
4. Taşınabilir sınıf kitaplığı projeden TodoList.xaml açın, aşağıdakileri ekleyin **düğmesi** öğesinde *buttonsPanel* varolan bir düğmeyi sonra Düzen öğesi:

          <Button x:Name="loginButton" Text="Sign-in" MinimumHeightRequest="30"
            Clicked="loginButton_Clicked"/>

    Bu düğme, kimlik doğrulama sunucusu tarafından yönetilen mobil uygulamanızın arka ucu ile tetikler.
5. Taşınabilir sınıf kitaplığı projeden TodoList.xaml.cs açın ve aşağıdaki alana ekleyin `TodoList` sınıfı:

        // Track whether the user has authenticated.
        bool authenticated = false;
6. Değiştirin **OnAppearing** yöntemini aşağıdaki kod ile:

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

    Bu kod, doğrulandıktan sonra verileri yalnızca hizmetten yenileneceğini emin olur.
7. Aşağıdaki işleyicisi eklemek **tıklama** olaya **TodoList** sınıfı:

        async void loginButton_Clicked(object sender, EventArgs e)
        {
            if (App.Authenticator != null)
                authenticated = await App.Authenticator.Authenticate();

            // Set syncItems to true to synchronize the data on startup when offline is enabled.
            if (authenticated == true)
                await RefreshItems(true, syncItems: false);
        }
8. Değişikliklerinizi kaydetmek ve hatasız doğrulama taşınabilir sınıf kitaplığı projeyi yeniden derleyin.

## <a name="add-authentication-to-the-android-app"></a>Android uygulamasında kimlik doğrulaması ekleme
Bu bölümde nasıl uygulayacağınızı gösteren **IAuthenticate** arabiriminde Android uygulama projesi. Android cihazlar destekleniyorsa değil, bu bölümü atlayın.

1. Visual Studio veya Xamarin Studio'da, sağ **droid** ardından Proje **başlangıç projesi olarak ayarla**.
2. Hata ayıklayıcıda proje başlatın, ardından uygulama başladıktan sonra işlenmeyen bir özel durum ile bir durum kodu 401 (yetkisiz) tetiklenir doğrulamak için F5 tuşuna basın. 401 kodunu arka uç erişimi yalnızca yetkili kullanıcılar ile sınırlı olduğundan oluşturulur.
3. MainActivity.cs içinde Android projesi açın ve aşağıdakileri ekleyin `using` ifadeleri:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
4. Güncelleştirme **MainActivity** uygulamak için sınıfı **IAuthenticate** arabirimi, şu şekilde:

        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity, IAuthenticate
5. Güncelleştirme **MainActivity** ekleyerek sınıfı bir **MobileServiceUser** alan ve bir **doğrulaması** gereken yöntemini **IAuthenticate** arabirimi, şu şekilde:

        // Define an authenticated user.
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

6. Güncelleştirme **AndroidManifest.xml** içinde aşağıdaki XML ekleyerek dosya `<application>` öğesi:

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
    Değiştirin `{url_scheme_of_your_app}` , URL düzeni.
7. Aşağıdaki kodu ekleyin **OnCreate** yöntemi **MainActivity** sınıfı çağırmadan önce `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        App.Init((IAuthenticate)this);

    Bu kod, kimlik doğrulayıcı uygulama yükleri önce başlatılır sağlar.
8. Uygulamayı yeniden oluşturmanız, çalıştırın ve ardından seçtiğiniz ve kimliği doğrulanmış bir kullanıcı olarak verilere erişimini doğrulayın kimlik doğrulama sağlayıcısının oturum açın.

### <a name="troubleshooting"></a>Sorun giderme

**Uygulama ile kilitlendi `Java.Lang.NoSuchMethodError: No static method startActivity`**

Bazı durumlarda, Visual studio, ancak çalışma zamanında özel durum uygulama kilitlenmesi yalnızca bir uyarı olarak görüntülenen Destek paketlerinde çakışıyor. Bu durumda, projenizde başvurulan tüm destek paketleri aynı sürümü kullandığınızdan emin olmanız gerekir. [Azure Mobile Apps NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/), Android platformu için `Xamarin.Android.Support.CustomTabs` bağımlılığına sahiptir, yani projeniz daha yeni destek paketleri kullanıyorsa, çakışmaları önlemek için doğrudan gerekli sürüme sahip bu paketi yüklemeniz gerekir.

## <a name="add-authentication-to-the-ios-app"></a>İOS uygulaması için kimlik doğrulaması ekleme
Bu bölümde nasıl uygulayacağınızı gösteren **IAuthenticate** iOS uygulama projesinde arabirimi. İOS cihazları destekleyen değil, bu bölümü atlayabilirsiniz.

1. Visual Studio veya Xamarin Studio'da, sağ **iOS** ardından Proje **başlangıç projesi olarak ayarla**.
2. Hata ayıklayıcıda proje başlatın, ardından uygulama başladıktan sonra işlenmeyen bir özel durum ile bir durum kodu 401 (yetkisiz) tetiklenir doğrulamak için F5 tuşuna basın. Arka uç erişimi yalnızca yetkili kullanıcılar ile sınırlı olduğundan 401 yanıtı oluşturulur.
3. AppDelegate.cs iOS projesi içinde açın ve aşağıdakileri ekleyin `using` ifadeleri:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
4. Güncelleştirme **AppDelegate** uygulamak için sınıfı **IAuthenticate** arabirimi, şu şekilde:

        public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate, IAuthenticate
5. Güncelleştirme **AppDelegate** ekleyerek sınıfı bir **MobileServiceUser** alan ve bir **doğrulaması** gereken yöntemini **IAuthenticate** arabirimi, şu şekilde:

        // Define an authenticated user.
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
            UIAlertController avAlert = UIAlertController.Create("Sign-in result", message, UIAlertControllerStyle.Alert);
            avAlert.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
            UIApplication.SharedApplication.KeyWindow.RootViewController.PresentViewController(avAlert, true, null);

            return success;
        }

    Facebook dışında bir kimlik sağlayıcısı kullanıyorsanız, [MobileServiceAuthenticationProvider] için farklı bir değer seçin.
    
6. Güncelleştirme **AppDelegate** ekleyerek sınıfı **OpenUrl** yöntemi aşırı yüklemek, şu şekilde:

        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            return TodoItemManager.DefaultManager.CurrentClient.ResumeWithURL(url);
        }
   
7. Aşağıdaki kod satırını ekleyin **FinishedLaunching** yöntemi çağırmadan önce `LoadApplication()`:

        App.Init(this);

    Bu kod, kimlik doğrulayıcı uygulama yüklenmeden önce başlatılır sağlar.

8. Info.plist açın ve eklemek bir **URL türünü**. Ayarlama **tanımlayıcısı** seçtiğiniz, adına **URL şemalarını** , uygulamanız için URL şeması için ve **rol** yok.

9. Uygulamayı yeniden oluşturmanız, çalıştırın ve ardından seçtiğiniz ve kimliği doğrulanmış bir kullanıcı olarak verilere erişimini doğrulayın kimlik doğrulama sağlayıcısının oturum açın.

## <a name="add-authentication-to-windows-10-including-phone-app-projects"></a>Windows 10 (telefon dahil) uygulaması projeleri için kimlik doğrulaması ekleme
Bu bölümde nasıl uygulayacağınızı gösteren **IAuthenticate** arabiriminde Windows 10 uygulaması projeleri. Evrensel Windows Platformu (UWP) projeleri, ancak kullanmak için aynı adımları uygulamak **UWP** projeyle (not ettiğiniz değişir). Windows cihazları destekleyen değil, bu bölümü atlayabilirsiniz.

1. Visual Studio'da sağ **UWP** ardından Proje **başlangıç projesi olarak ayarla**.
2. Hata ayıklayıcıda proje başlatın, ardından uygulama başladıktan sonra işlenmeyen bir özel durum ile bir durum kodu 401 (yetkisiz) tetiklenir doğrulamak için F5 tuşuna basın. Arka uç erişimi yalnızca yetkili kullanıcılar ile sınırlı olduğundan 401 yanıt gerçekleşir.
3. MainPage.xaml.cs için Windows uygulaması projesi açın ve aşağıdakileri ekleyin `using` ifadeleri:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.UI.Popups;
        using <your_Portable_Class_Library_namespace>;

    Değiştirin `<your_Portable_Class_Library_namespace>` , taşınabilir sınıf kitaplığı için bir ad alanı.
4. Güncelleştirme **MainPage** uygulamak için sınıfı **IAuthenticate** arabirimi, şu şekilde:

        public sealed partial class MainPage : IAuthenticate
5. Güncelleştirme **MainPage** ekleyerek sınıfı bir **MobileServiceUser** alan ve bir **doğrulaması** gereken yöntemini **IAuthenticate**arabirimi, şu şekilde:

        // Define an authenticated user.
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

1. Aşağıdaki kod satırını Oluşturucusu eklemek **MainPage** sınıfı çağırmadan önce `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        <your_Portable_Class_Library_namespace>.App.Init(this);

    Değiştirin `<your_Portable_Class_Library_namespace>` , taşınabilir sınıf kitaplığı için bir ad alanı.

3. Kullanıyorsanız **UWP**, aşağıdaki **OnActivated** yöntemi geçersiz kılma **uygulama** sınıfı:

       protected override void OnActivated(IActivatedEventArgs args)
       {
           base.OnActivated(args);

            if (args.Kind == ActivationKind.Protocol)
            {
                ProtocolActivatedEventArgs protocolArgs = args as ProtocolActivatedEventArgs;
                MobileServiceClientExtensions.ResumeWithURL(TodoItemManager.DefaultManager.CurrentClient,protocolArgs.Uri);
            }
       }

3. Package.appxmanifest açın ve eklemek bir **Protokolü** bildirimi. Ayarlama **görünen ad** seçtiğiniz, bir ad ve **adı** , uygulama için URL şeması için.

4. Uygulamayı yeniden oluşturmanız, çalıştırın ve ardından seçtiğiniz ve kimliği doğrulanmış bir kullanıcı olarak verilere erişimini doğrulayın kimlik doğrulama sağlayıcısının oturum açın.

## <a name="next-steps"></a>Sonraki adımlar
Bu temel kimlik doğrulama öğreticisini tamamladığınıza göre aşağıdaki öğreticilerden birine açın etmeden göz önünde bulundurun:

* [Uygulamanıza anında iletme bildirimleri ekleme](app-service-mobile-xamarin-forms-get-started-push.md)

  Uygulamanıza anında iletme bildirimleri desteği eklemeyi ve anında iletme bildirimleri göndermek için Azure Notification Hubs’ı kullanmak üzere Mobile App arka ucunuzu yapılandırmayı öğrenin.
* [Uygulamanız için çevrimdışı eşitlemeyi etkinleştirme](app-service-mobile-xamarin-forms-get-started-offline-data.md)

  Mobil Uygulama arka ucu kullanarak uygulamanıza çevrimdışı destek eklemeyi öğrenin. Çevrimdışı eşitleme son kullanıcıların görüntüleme, ekleme veya ağ bağlantısı olduğunda bile verileri - değiştirme ile mobil uygulama - etkileşime olanak tanır.

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
