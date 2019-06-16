---
title: Xamarin Android Mobile Apps için kimlik doğrulamasını kullanmaya başlama
description: Kimlik sağlayıcıları, AAD, Google, Facebook, Twitter ve Microsoft gibi çeşitli Xamarin Android uygulamanızdaki kullanıcıların kimliğini doğrulamak için Mobile Apps'ı kullanmayı öğrenin.
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: panarasi
editor: ''
ms.assetid: 570fc12b-46a9-4722-b2e0-0d1c45fb2152
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 09/24/2018
ms.author: panarasi
ms.openlocfilehash: 0a2d964d60d13f0e71de5776112a4edbe3cdcc45
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62127922"
---
# <a name="add-authentication-to-your-xamarinandroid-app"></a>Xamarin.Android uygulamanıza kimlik doğrulaması ekleme
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Bu konu, bir mobil uygulamadan alınan istemci uygulamanızın kullanıcılarının kimlik doğrulaması yapmayı gösterir. Bu öğreticide, Azure Mobile Apps tarafından desteklenen bir kimlik sağlayıcısı kullanarak hızlı başlangıç projesine kimlik doğrulama ekleyin. Başarıyla kimlik doğrulaması ve mobil uygulamada yetkili sonra kullanıcı kimliği değeri görüntülenir.

Bu öğretici, mobil uygulama hızlı başlangıcı temel alır. Ayrıca ilk öğreticiyi tamamlamanız gerekir [bir Xamarin.Android uygulaması oluşturma]. İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, kimlik doğrulaması uzantı paketi projenize eklemeniz gerekir. Server uzantısı paketleri hakkında daha fazla bilgi için bkz. [Azure Mobile Apps için .NET arka uç sunucu SDK'sı ile çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="register"></a>Kimlik doğrulaması için uygulamanızı kaydetme ve uygulama Hizmetleri'ı yapılandırma
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>İzin verilen dış yönlendirme URL'leri uygulamanıza ekleyin

Uygulamanız için yeni bir URL şemasını tanımlamak güvenli kimlik doğrulaması gerektirir. Bu kimlik doğrulama işlemi tamamlandıktan sonra uygulamanıza geri yönlendirmek bir kimlik doğrulama sistemi sağlar. Bu öğreticide, kullandığımız URL şeması _appname_ boyunca. Ancak, seçtiğiniz herhangi bir URL şeması kullanabilirsiniz. Mobil uygulamanız için benzersiz olmalıdır. Sunucu tarafında yeniden yönlendirmeyi etkinleştirmek için:

1. [Azure portalı] uygulama hizmetinizi seçin.

2. Tıklayın **kimlik doğrulama / yetkilendirme** menü seçeneği.

3. İçinde **izin verilen dış yönlendirme URL'leri**, girin `url_scheme_of_your_app://easyauth.callback`.  **Url_scheme_of_your_app** bu dizesinde mobil uygulamanız için URL şeması aşağıdaki gibidir.  Bu, bir protokol (kullanım harf ve yalnızca sayı ve bir harfle) için normal URL belirtimi izlemeniz gerekir.  Çeşitli yerlerde URL şeması ile mobil uygulama kodunuzu ayarlamak kullanmanız gerektiğinden, seçtiğiniz dizenin Not.

4. **Tamam** düğmesine tıklayın.

5. **Kaydet**’e tıklayın.

## <a name="permissions"></a>Kimliği doğrulanmış kullanıcılar için izinleri kısıtla
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Visual Studio veya Xamarin Studio, bir cihaz veya öykünücü üzerinde istemci projesini çalıştırın. Uygulama başladıktan sonra işlenmeyen bir özel durum ile bir durum kodu 401 (yetkisiz) tetiklenir doğrulayın. Bunun nedeni, uygulama kimliği doğrulanmamış bir kullanıcı olarak mobil uygulama arka ucunuzu erişmeye kullanmasıdır. *Todoıtem* tablo artık kimlik doğrulaması gerektirir.

Ardından, istemci uygulaması isteği kaynaklarına mobil uygulama arka ucundan ile kimliği doğrulanmış bir kullanıcıyı güncelleştirir.

## <a name="add-authentication"></a>Uygulamaya kimlik doğrulaması ekleme
Uygulama, kullanıcıların dokunarak gerektirecek şekilde güncelleştirilir **oturum** düğmesine tıklayın ve veri görüntülemeden önce kimlik doğrulaması.

1. Aşağıdaki kodu ekleyin **TodoActivity** sınıfı:
   
        // Define an authenticated user.
        private MobileServiceUser user;
        private async Task<bool> Authenticate()
        {
                var success = false;
                try
                {
                    // Sign in with Facebook login using a server-managed flow.
                    user = await client.LoginAsync(this,
                        MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                    CreateAndShowDialog(string.Format("you are now logged in - {0}",
                        user.UserId), "Logged in!");
   
                    success = true;
                }
                catch (Exception ex)
                {
                    CreateAndShowDialog(ex, "Authentication failed");
                }
                return success;
        }
   
        [Java.Interop.Export()]
        public async void LoginUser(View view)
        {
            // Load data only after authentication succeeds.
            if (await Authenticate())
            {
                //Hide the button after authentication succeeds.
                FindViewById<Button>(Resource.Id.buttonLoginUser).Visibility = ViewStates.Gone;
   
                // Load the data.
                OnRefreshItemsSelected();
            }
        }
   
    Bu, bir kullanıcı ve bir yöntemin işleyicisi için yeni bir kimlik doğrulaması için yeni bir yöntem oluşturur **oturum** düğmesi. Yukarıdaki örnek kod kullanıcı bir Facebook oturum açma kullanılarak doğrulanır. Bir iletişim kutusu, bir kez kimliği doğrulanmış kullanıcı kimliği görüntülemek için kullanılır.
   
   > [!NOTE]
   > Facebook dışında bir kimlik sağlayıcısı kullanıyorsanız, geçirilen değeri değiştirmek **LoginAsync** yukarıda için aşağıdakilerden biri: *MicrosoftAccount*, *Twitter*, *Google*, veya *WindowsAzureActiveDirectory*.
   > 
   > 
2. İçinde **OnCreate** yöntemi, silin veya açıklama çıkış aşağıdaki kod satırını:
   
        OnRefreshItemsSelected ();
3. Activity_To_Do.axml dosyasına aşağıdakileri ekleyin *LoginUser* düğme var olan önce tanımını *addItem* düğmesi:
   
          <Button
            android:id="@+id/buttonLoginUser"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="LoginUser"
            android:text="@string/login_button_text" />
4. Strings.xml kaynak dosyasına şu öğeyi ekleyin:
   
        <string name="login_button_text">Sign in</string>
5. AndroidManifest.xml dosyasını açın, içine aşağıdaki kodu ekleyin `<application>` XML öğesi:

        <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity" android:launchMode="singleTop" android:noHistory="true">
          <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="{url_scheme_of_your_app}" android:host="easyauth.callback" />
          </intent-filter>
        </activity>

6. Visual Studio veya Xamarin Studio, bir cihaz veya öykünücü üzerinde istemci projesini çalıştırın ve, seçilen kimlik sağlayıcısı ile oturum açın. Başarıyla oturum açma, uygulamanın, oturum açma kimliği ve yapılacaklar öğelerinin listesi görüntülenir ve veri güncelleştirmeleri yapabilirsiniz.

## <a name="troubleshooting"></a>Sorun giderme

**Uygulama ile kilitlendi `Java.Lang.NoSuchMethodError: No static method startActivity`**

Bazı durumlarda, Visual studio, ancak çalışma zamanında özel durum uygulama kilitlenmesi yalnızca bir uyarı olarak görüntülenen Destek paketlerinde çakışıyor. Bu durumda, projenizde başvurulan tüm destek paketleri aynı sürümü kullandığınızdan emin olmanız gerekir. [Azure Mobile Apps NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/), Android platformu için `Xamarin.Android.Support.CustomTabs` bağımlılığına sahiptir, yani projeniz daha yeni destek paketleri kullanıyorsa, çakışmaları önlemek için doğrudan gerekli sürüme sahip bu paketi yüklemeniz gerekir.

<!-- URLs. -->
[Bir Xamarin.Android uygulaması oluşturma]: app-service-mobile-xamarin-android-get-started.md
