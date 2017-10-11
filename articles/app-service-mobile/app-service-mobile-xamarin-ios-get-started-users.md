---
title: "Xamarin iOS mobil uygulamalar için kimlik doğrulaması kullanmaya başlama"
description: "Xamarin iOS uygulamanızın kimlik sağlayıcıları, AAD, Google, Facebook, Twitter ve Microsoft dahil olmak üzere çeşitli kullanıcıların kimliklerini doğrulamak için Mobile Apps kullanmayı öğrenin."
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 180cc61b-19c5-48bf-a16c-7181aef3eacc
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 07/05/2017
ms.author: glenga
ms.openlocfilehash: 454b2df5a9bf8cfba93befea54370957ab044d95
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="add-authentication-to-your-xamarinios-app"></a>Xamarin.iOS uygulamanıza kimlik doğrulaması ekleyin
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Bu konuda bir mobil uygulama hizmeti istemci uygulamanızdan kullanıcıların kimliklerini gösterilmiştir. Bu öğretici kapsamında, kimlik doğrulama App Service tarafından desteklenen bir kimlik sağlayıcısı kullanarak Xamarin.iOS hızlı başlangıç projesi ekleyin. Mobil uygulamanız tarafından yetkili başarılı bir şekilde kimliği doğrulanmış ve sonra kullanıcı kimliği değeri görüntülenir ve kısıtlı tablo veri erişimi olacaktır.

Öğreticiyi tamamlamak [bir Xamarin.iOS uygulaması oluşturma]. İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, kimlik doğrulaması uzantısı paketini projenize eklemeniz gerekir. Server uzantısı paketleri hakkında daha fazla bilgi için bkz: [.NET arka uç sunucusu SDK ile Azure Mobile Apps için iş](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="register-your-app-for-authentication-and-configure-app-services"></a>Kimlik doğrulaması için uygulamanızı kaydetme ve uygulama hizmetlerini yapılandırma
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="add-your-app-to-the-allowed-external-redirect-urls"></a>Uygulamanız için izin verilen dış yönlendirme URL'leri ekleme

Uygulamanız için yeni bir URL şemasını tanımlamak güvenli kimlik doğrulaması gerektirir. Bu kimlik doğrulama işlemi tamamlandıktan sonra uygulamanıza geri yönlendirmek bir kimlik doğrulama sistemi sağlar. Bu öğreticide, URL şemasının kullanırız _appname_ boyunca. Ancak, seçtiğiniz herhangi bir URL şeması kullanabilirsiniz. Mobil uygulamanız için benzersiz olmalıdır. Sunucu tarafında yeniden yönlendirmeyi etkinleştirmek için:

1. [Azure portalında] uygulama hizmetinizi seçin.

2. Tıklatın **kimlik doğrulama / yetkilendirme** menü seçeneği.

3. İçinde **yeniden yönlendirme URL'lere izin**, girin `url_scheme_of_your_app://easyauth.callback`.  **Url_scheme_of_your_app** Bu dize, mobil uygulamanız için URL düzenidir.  Bir protokol (harf kullanın ve yalnızca sayı ve bir harf ile başlar) için normal URL belirtimi izlemelisiniz.  Çeşitli yerlerde URL şeması ile mobil uygulama kodunuzu ayarlamak ihtiyaç duyacağınız seçtiğiniz dizeyi Not olmanız gerekir.

4. **Tamam** düğmesine tıklayın.

5. **Kaydet** düğmesine tıklayın.

## <a name="restrict-permissions-to-authenticated-users"></a>Kimliği doğrulanmış kullanıcılar için izinleri kısıtla
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

&nbsp;&nbsp;4. Visual Studio veya Xamarin Studio'da istemci projesi bir cihaz veya öykünücü çalıştırın. Uygulama başlatıldıktan sonra bir durum koduyla işlenmeyen bir özel durum 401 (yetkisiz) tetiklenir doğrulayın. Hata ayıklayıcı konsola hata günlüğe kaydedilir. Bu nedenle Visual Studio'da hata çıktı penceresinde görmeniz gerekir.

&nbsp;&nbsp;Uygulama Kimliği doğrulanmamış bir kullanıcı olarak, mobil uygulamanızın arka ucuna erişmeye çünkü bu yetkisiz hatası gerçekleşir. *Todoıtem* tablo artık kimlik doğrulaması gerektirir.

Ardından, istemci uygulaması isteği kaynaklarına mobil uygulama arka ucundan kimliği doğrulanmış bir kullanıcı ile güncelleştirir.

## <a name="add-authentication-to-the-app"></a>Kimlik doğrulaması için uygulama ekleme
Bu bölümde, uygulama veri görüntülenmeden önce bir oturum açma ekranı değiştirecektir. Uygulama başlatıldığında değil, App Service'e bağlanmaz ve herhangi bir veri görüntülenmez. İlk kez sonra kullanıcı oturum açma ekranı görünür yenileme hareketi gerçekleştirir; başarılı oturum açma işleminden sonra Yapılacaklar öğelerini listesi görüntülenir.

1. İstemci proje dosyasını açın **QSTodoService.cs** ve aşağıdakileri ekleyin deyimiyle ve `MobileServiceUser` QSTodoService sınıfına erişimcisi ile:
 
        using UIKit;
       
        // Logged in user
        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }
2. Adlı yeni bir yöntem ekleyin **kimlik doğrulama** için **QSTodoService** aşağıdaki tanımıyla:

        public async Task Authenticate(UIViewController view)
        {
            try
            {
                AppDelegate.ResumeWithURL = url => url.Scheme == "zumoe2etestapp" && client.ResumeWithURL(url);
                user = await client.LoginAsync(view, MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine (@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    >[AZURE.NOTE] Bir Facebook dışında bir kimlik sağlayıcısı kullanıyorsanız, geçirilen değerini değiştirmek **LoginAsync** yukarıda aşağıdakilerden birine: _MicrosoftAccount_, _Twitter_,  _Google_, veya _WindowsAzureActiveDirectory_.

3. Açık **QSTodoListViewController.cs**. Yöntem tanımını değiştirme **ViewDidLoad** çağrısı kaldırma **RefreshAsync()** yakınında bitiş:
   
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
4. Yöntemi Değiştir **RefreshAsync** , kimlik doğrulaması için **kullanıcı** özelliği null. Yöntem tanımı üstünde aşağıdaki kodu ekleyin:
   
        // start of RefreshAsync method
        if (todoService.User == null) {
            await QSTodoService.DefaultService.Authenticate(this);
            if (todoService.User == null) {
                Console.WriteLine("couldn't login!!");
                return;
            }
        }
        // rest of RefreshAsync method
5. Açık **AppDelegate.cs**, aşağıdaki yöntemi ekleyin:

        public static Func<NSUrl, bool> ResumeWithURL;

        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            return ResumeWithURL != null && ResumeWithURL(url);
        }
6. Açık **Info.plist** dosya, gitmek **URL türleri** içinde **Gelişmiş** bölümü. Şimdi yapılandırmak **tanımlayıcısı** ve **URL şemalarını** URL türü ve tıklatın **URL türü Ekle**. **URL şemalarını** , {url_scheme_of_your_app} aynı olmalıdır.
7. Visual Studio veya Xamarin Studio'da, Xamarin derleme konaktaki bir cihaz veya öykünücü hedefleme istemci projesi çalıştırma Mac'iniz bağlı. Uygulama veri görüntülendiğini doğrulayın.
   
    Görüntülenecek oturum açma ekranı neden olacak öğeleri listede aşağı çekerek yenileme hareketi gerçekleştirin. Geçerli kimlik bilgileri başarıyla girdikten sonra uygulama Yapılacaklar öğelerini listesi görüntülenir ve veri güncelleştirmeleri yapabilirsiniz.

<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[bir Xamarin.iOS uygulaması oluşturma]: app-service-mobile-xamarin-ios-get-started.md