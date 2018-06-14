---
title: Xamarin Android mobil uygulamalar için kimlik doğrulaması kullanmaya başlama
description: Xamarin Android uygulamanızın kimlik sağlayıcıları, AAD, Google, Facebook, Twitter ve Microsoft dahil olmak üzere çeşitli kullanıcıların kimliklerini doğrulamak için Mobile Apps kullanmayı öğrenin.
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
ms.date: 07/05/2017
ms.author: panarasi
ms.openlocfilehash: 97207b722b65ccf98c57304cd559b0927aacd5a4
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2018
ms.locfileid: "27595304"
---
# <a name="add-authentication-to-your-xamarinandroid-app"></a>Xamarin.Android uygulamanıza kimlik doğrulaması ekleme
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Bu konuda istemci uygulamanız mobil uygulamasından kullanıcıların kimliklerini gösterilmiştir. Bu öğreticide, Azure Mobile Apps tarafından desteklenen bir kimlik sağlayıcısı kullanarak hızlı başlangıç projesi için kimlik doğrulaması ekleyin. Başarılı bir şekilde kimliği doğrulanmış ve mobil uygulamada yetkili sonra kullanıcı kimliği değeri görüntülenir.

Bu öğretici, mobil uygulama hızlı başlangıç üzerinde temel alır. Ayrıca ilk öğreticiyi tamamlamanız gereken [Xamarin.Android uygulaması oluşturma]. İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, kimlik doğrulaması uzantısı paketini projenize eklemeniz gerekir. Server uzantısı paketleri hakkında daha fazla bilgi için bkz: [.NET arka uç sunucusu SDK ile Azure Mobile Apps için iş](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="register"></a>Kimlik doğrulaması için uygulamanızı kaydetme ve uygulama hizmetlerini yapılandırma
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

Visual Studio veya Xamarin Studio'da istemci projesi bir cihaz veya öykünücü çalıştırın. Uygulama başlatıldıktan sonra bir durum koduyla işlenmeyen bir özel durum 401 (yetkisiz) tetiklenir doğrulayın. Uygulama Kimliği doğrulanmamış bir kullanıcı olarak, mobil uygulamanızın arka ucuna erişmeye çünkü bu gerçekleşir. *Todoıtem* tablo artık kimlik doğrulaması gerektirir.

Ardından, istemci uygulaması isteği kaynaklarına mobil uygulama arka ucundan kimliği doğrulanmış bir kullanıcı ile güncelleştirir.

## <a name="add-authentication"></a>Kimlik doğrulaması için uygulama ekleme
Uygulama dokunun gerektirmek güncelleştirildi **oturum** düğmesine tıklayın ve veri görüntülenmeden önce kimlik doğrulaması.

1. Aşağıdaki kodu ekleyin **TodoActivity** sınıfı:
   
        // Define a authenticated user.
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
   
    Bu kullanıcı ve yöntemi işleyici yeni bir kimlik doğrulaması için yeni bir yöntem oluşturur **oturum** düğmesi. Yukarıdaki örnek kodda kullanıcı Facebook oturum açma kullanılarak doğrulanır. Bir iletişim kutusu, bir kez kimliği doğrulanmış kullanıcı kimliği görüntülemek için kullanılır.
   
   > [!NOTE]
   > Facebook dışında bir kimlik sağlayıcısı kullanıyorsanız, geçirilen değerini değiştirmek **LoginAsync** yukarıda aşağıdakilerden birine: *MicrosoftAccount*, *Twitter*, *Google*, veya *WindowsAzureActiveDirectory*.
   > 
   > 
2. İçinde **OnCreate** yöntemi, silmek veya yorum kullanıma aşağıdaki kod satırını:
   
        OnRefreshItemsSelected ();
3. Aşağıdaki Activity_To_Do.axml dosyasına ekleyin *LoginUser* düğmesini varolan önce tanımını *addItem* düğmesi:
   
          <Button
            android:id="@+id/buttonLoginUser"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="LoginUser"
            android:text="@string/login_button_text" />
4. Aşağıdaki öğe Strings.xml kaynaklar dosyasına ekleyin:
   
        <string name="login_button_text">Sign in</string>
5. AndroidManifest.xml dosyasını açın, aşağıdaki kodu ekleyin `<application>` XML öğesi:

        <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity" android:launchMode="singleTop" android:noHistory="true">
          <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="{url_scheme_of_your_app}" android:host="easyauth.callback" />
          </intent-filter>
        </activity>

6. Visual Studio veya Xamarin Studio, bir cihaz veya öykünücü istemci projesi çalıştırın ve seçilen kimlik sağlayıcınız ile oturum açın. Başarıyla oturum açma, uygulama oturum açma Kimliğiniz ve yapılacaklar öğelerini listesi görüntülenir ve veri güncelleştirmeleri yapabilirsiniz.

<!-- URLs. -->
[Xamarin.Android uygulaması oluşturma]: app-service-mobile-xamarin-android-get-started.md