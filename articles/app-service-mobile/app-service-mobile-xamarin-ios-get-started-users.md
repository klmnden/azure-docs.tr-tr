---
title: Mobile Apps için kimlik doğrulaması, Xamarin iOS kullanmaya başlayın
description: Kimlik sağlayıcıları, AAD, Google, Facebook, Twitter ve Microsoft gibi çeşitli Xamarin iOS uygulamanızdaki kullanıcıların kimliğini doğrulamak için Mobile Apps'ı kullanmayı öğrenin.
services: app-service\mobile
documentationcenter: xamarin
author: elamalani
manager: crdun
editor: ''
ms.assetid: 180cc61b-19c5-48bf-a16c-7181aef3eacc
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 06/25/2019
ms.author: emalani
ms.openlocfilehash: fa1f4bae314025a71568e1e04cbf950ebbe26dbe
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446235"
---
# <a name="add-authentication-to-your-xamarinios-app"></a>Xamarin.iOS uygulamanıza kimlik doğrulaması ekleyin
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

> [!NOTE]
> Visual Studio App Center, mobil uygulama geliştirme merkezi hizmetlerinde yeni ve tümleşik yatırım yapıyor. Geliştiriciler **derleme**, **Test** ve **Dağıt** hizmetlerinin sürekli tümleştirme ve teslim işlem hattı ayarlayın. Uygulama dağıtıldığında, geliştiriciler kendi uygulamasını kullanarak kullanımı ve durumu izleyebilirsiniz **Analytics** ve **tanılama** kullanarak kullanıcılarla etkileşim kurun ve hizmetlerini **anında iletme** hizmeti. Geliştiriciler de yararlanabilir **Auth** , kullanıcıların kimliğini doğrulamak ve **veri** kalıcı hale getirmek ve uygulama verilerini bulutta eşitleme hizmeti. Kullanıma [App Center](https://appcenter.ms/?utm_source=zumo&utm_campaign=app-service-mobile-xamarin-ios-get-started-users) bugün.
>

## <a name="overview"></a>Genel Bakış

Bu konu bir App Service mobil uygulama istemci uygulamanızın kullanıcılarının kimlik doğrulaması yapmayı gösterir. Bu öğreticide, App Service tarafından desteklenen bir kimlik sağlayıcısı kullanarak Xamarin.iOS hızlı başlangıç projesi için kimlik doğrulaması ekleyin. Mobil uygulamanız tarafından yetkili başarıyla yapıldığını ve sonra kullanıcı kimliği değeri görüntülenir ve kısıtlı tablo verilerine erişmek mümkün olacaktır.

Öğreticiyi tamamlamak [bir Xamarin.iOS uygulaması oluşturma]. İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, kimlik doğrulaması uzantı paketi projenize eklemeniz gerekir. Server uzantısı paketleri hakkında daha fazla bilgi için bkz. [Azure Mobile Apps için .NET arka uç sunucu SDK'sı ile çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="register-your-app-for-authentication-and-configure-app-services"></a>Kimlik doğrulaması için uygulamanızı kaydetme ve uygulama Hizmetleri'ı yapılandırma
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="add-your-app-to-the-allowed-external-redirect-urls"></a>İzin verilen dış yönlendirme URL'leri uygulamanıza ekleyin

Uygulamanız için yeni bir URL şemasını tanımlamak güvenli kimlik doğrulaması gerektirir. Bu kimlik doğrulama işlemi tamamlandıktan sonra uygulamanıza geri yönlendirmek bir kimlik doğrulama sistemi sağlar. Bu öğreticide, kullandığımız URL şeması _appname_ boyunca. Ancak, seçtiğiniz herhangi bir URL şeması kullanabilirsiniz. Mobil uygulamanız için benzersiz olmalıdır. Sunucu tarafında yeniden yönlendirmeyi etkinleştirmek için:

1. İçinde [Azure portalında](https://portal.azure.com/), App Service'ı seçin.

2. Tıklayın **kimlik doğrulama / yetkilendirme** menü seçeneği.

3. İçinde **izin verilen dış yönlendirme URL'leri**, girin `url_scheme_of_your_app://easyauth.callback`.  **Url_scheme_of_your_app** bu dizesinde mobil uygulamanız için URL şeması aşağıdaki gibidir.  Bu, bir protokol (kullanım harf ve yalnızca sayı ve bir harfle) için normal URL belirtimi izlemeniz gerekir.  Çeşitli yerlerde URL şeması ile mobil uygulama kodunuzu ayarlamak kullanmanız gerektiğinden, seçtiğiniz dizenin Not.

4. **Tamam**'ı tıklatın.

5. **Kaydet**’e tıklayın.

## <a name="restrict-permissions-to-authenticated-users"></a>Kimliği doğrulanmış kullanıcılar için izinleri kısıtla
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

* Visual Studio veya Xamarin Studio, bir cihaz veya öykünücü üzerinde istemci projesini çalıştırın. Uygulama başladıktan sonra işlenmeyen bir özel durum ile bir durum kodu 401 (yetkisiz) tetiklenir doğrulayın. Hata, hata ayıklayıcı konsoluna kaydedilir. Bu nedenle Visual Studio'da çıktı penceresinde bir hata görürsünüz.

    Uygulama Kimliği doğrulanmamış bir kullanıcı olarak mobil uygulama arka ucunuzu erişmeye çünkü bu yetkisiz hatası gerçekleşir. *Todoıtem* tablo artık kimlik doğrulaması gerektirir.

Ardından, istemci uygulaması isteği kaynaklarına mobil uygulama arka ucundan ile kimliği doğrulanmış bir kullanıcıyı güncelleştirir.

## <a name="add-authentication-to-the-app"></a>Uygulamaya kimlik doğrulaması ekleme
Bu bölümde, veri görüntülemeden önce oturum açma ekranında görüntülenmek üzere bir uygulama değiştirir. Uygulama başlatıldığında uygulama hizmetinize bağlanmak değildir ve herhangi bir veri görüntülenmez. İlk kez sonra kullanıcı oturum açma ekranı görünür yenileme hareketini gerçekleştirir; başarılı oturum açma işleminden sonra Yapılacaklar öğelerinin listesi görüntülenir.

1. İstemci proje dosyasını açın **QSTodoService.cs** ve aşağıdaki using deyimi ve `MobileServiceUser` QSTodoService sınıfa erişimcisi ile:

    ```csharp
    using UIKit;

    // Logged in user
    private MobileServiceUser user;
    public MobileServiceUser User { get { return user; } }
    ```

2. Adlı yeni yöntemi ekleyin **doğrulaması** için **QSTodoService** aşağıdaki tanımıyla:

    ```csharp
    public async Task Authenticate(UIViewController view)
    {
        try
        {
            AppDelegate.ResumeWithURL = url => url.Scheme == "{url_scheme_of_your_app}" && client.ResumeWithURL(url);
            user = await client.LoginAsync(view, MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
        }
        catch (Exception ex)
        {
            Console.Error.WriteLine (@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
        }
    }
    ```

    > [!NOTE]
    > Bir Facebook dışında bir kimlik sağlayıcısı kullanıyorsanız, geçirilen değeri değiştirmek **LoginAsync** yukarıda için aşağıdakilerden biri: _MicrosoftAccount_, _Twitter_, _Google_, veya _WindowsAzureActiveDirectory_.

3. Açık **QSTodoListViewController.cs**. Yöntem tanımını değiştirme **ViewDidLoad** çağrısını kaldırma **RefreshAsync()** sonlarında:

    ```csharp
    public override async void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        todoService = QSTodoService.DefaultService;
        await todoService.InitializeStoreAsync();

        RefreshControl.ValueChanged += async (sender, e) => {
            await RefreshAsync();
        }

        // Comment out the call to RefreshAsync
        // await RefreshAsync();
    }
    ```

4. Yöntemi Değiştir **RefreshAsync** , kimlik doğrulaması için **kullanıcı** özelliği null. Yöntem tanımını üstüne aşağıdaki kodu ekleyin:

    ```csharp
    // start of RefreshAsync method
    if (todoService.User == null) {
        await QSTodoService.DefaultService.Authenticate(this);
        if (todoService.User == null) {
            Console.WriteLine("couldn't login!!");
            return;
        }
    }
    // rest of RefreshAsync method
    ```

5. Açık **AppDelegate.cs**, aşağıdaki yöntemi ekleyin:

    ```csharp
    public static Func<NSUrl, bool> ResumeWithURL;

    public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
    {
        return ResumeWithURL != null && ResumeWithURL(url);
    }
    ```

6. Açık **Info.plist** gidin, dosya **URL türleri** içinde **Gelişmiş** bölümü. Şimdi Yapılandır **tanımlayıcı** ve **URL şemalarını** URL türünü ve tıklatın **URL türü Ekle**. **URL şemaları** , {url_scheme_of_your_app} ile aynı olması gerekir.
7. Visual Studio, Mac ana ya da Visual Studio Mac için bağlı bir cihaz veya öykünücü hedefleyen istemci projesi çalıştırın. Uygulama veri görüntülendiğini doğrulayın.

    Görüntülenecek oturum açma ekranı açacak öğeleri listede aşağı çekerek yenileme hareketi gerçekleştirin. Geçerli kimlik bilgileri başarıyla girdikten sonra uygulama Yapılacaklar öğelerinin listesi görüntülenir ve veri güncelleştirmeleri yapabilirsiniz.

<!-- URLs. -->
[Submit an app page]: https://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: https://go.microsoft.com/fwlink/p/?LinkId=262039
[Bir Xamarin.iOS uygulaması oluşturma]: app-service-mobile-xamarin-ios-get-started.md
