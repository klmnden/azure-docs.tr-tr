---
title: "İOS Azure Mobile Apps ile kimlik doğrulaması ekleme"
description: "İOS uygulamanızın kimlik sağlayıcıları, AAD, Google, Facebook, Twitter ve Microsoft dahil olmak üzere çeşitli kullanıcıların kimliklerini doğrulamak için Azure Mobile Apps kullanmayı öğrenin."
services: app-service\mobile
documentationcenter: ios
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: ef3d3cbe-e7ca-45f9-987f-80c44209dc06
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: glenga
ms.openlocfilehash: 21a2cc6c1eaf4b34cbe8c2d7c4dbb69c8730cf32
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="add-authentication-to-your-ios-app"></a>İOS uygulamanıza kimlik doğrulaması ekleme
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Bu öğretici kapsamında, kimlik doğrulaması ekleme [iOS Hızlı Başlat] desteklenen kimlik sağlayıcısı kullanarak proje. Bu öğretici dayanır [iOS Hızlı Başlat] önce tamamlamanız gereken öğretici.

## <a name="register"></a>Kimlik doğrulaması için uygulamanızı kaydetme ve uygulama hizmetini yapılandırma
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>Uygulamanız için izin verilen dış yönlendirme URL'leri ekleme

Uygulamanız için yeni bir URL şemasını tanımlamak güvenli kimlik doğrulaması gerektirir.  Bu kimlik doğrulama işlemi tamamlandıktan sonra uygulamanıza geri yönlendirmek bir kimlik doğrulama sistemi sağlar.  Bu öğreticide, URL şemasının kullanırız _appname_ boyunca.  Ancak, seçtiğiniz herhangi bir URL şeması kullanabilirsiniz.  Mobil uygulamanız için benzersiz olmalıdır.  Th sunucu tarafında yeniden yönlendirmeyi etkinleştirmek için:

1. İçinde [Azure portal], uygulama hizmetinizi seçin.

2. Tıklatın **kimlik doğrulama / yetkilendirme** menü seçeneği.

3. Tıklatın **Azure Active Directory** altında **kimlik doğrulama sağlayıcıları** bölümü.

4. Ayarlama **yönetim modu** için **Gelişmiş**.

5. İçinde **yeniden yönlendirme URL'lere izin**, girin `appname://easyauth.callback`.  _Appname_ Bu dize, mobil uygulamanız için URL düzenidir.  Bir protokol (harf kullanın ve yalnızca sayı ve bir harf ile başlar) için normal URL belirtimi izlemelisiniz.  Çeşitli yerlerde URL şeması ile mobil uygulama kodunuzu ayarlamak ihtiyaç duyacağınız seçtiğiniz dizeyi Not olmanız gerekir.

6. **Tamam** düğmesine tıklayın.

7. **Kaydet** düğmesine tıklayın.

## <a name="permissions"></a>Kimliği doğrulanmış kullanıcılar için izinleri kısıtla
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Xcode'da, basın **çalıştırmak** uygulamayı başlatmak için. Uygulama arka ucu kimliği doğrulanmamış bir kullanıcı olarak erişmeye çalıştığı bir özel durum oluşturulur ancak *Todoıtem* tablo artık kimlik doğrulaması gerektirir.

## <a name="add-authentication"></a>Kimlik doğrulaması için uygulama ekleme
**Objective-C**:

1. Mac'inizde açmak *QSTodoListViewController.m* xcode'da ve aşağıdaki yöntemi ekleyin:

    ```Objective-C
    - (void)loginAndGetData
    {
        QSAppDelegate *appDelegate = (QSAppDelegate *)[UIApplication sharedApplication].delegate;
        appDelegate.qsTodoService = self.todoService;

        [self.todoService.client loginWithProvider:@"google" urlScheme:@"appname" controller:self animated:YES completion:^(MSUser * _Nullable user, NSError * _Nullable error) {
            if (error) {
                NSLog(@"Login failed with error: %@, %@", error, [error userInfo]);
            }
            else {
                self.todoService.client.currentUser = user;
                NSLog(@"User logged in: %@", user.userId);

                [self refresh];
            }
        }];
    }
    ```

    Değişiklik *google* için *microsoftaccount*, *twitter*, *facebook*, veya *windowsazureactivedirectory* kimlik sağlayıcınız olarak Google kullanmıyorsanız. Facebook kullanmak istiyorsanız, gerekir [beyaz liste Facebook etki alanları] [ 1] uygulamanızda.

    Değiştir **urlScheme** uygulamanız için benzersiz bir ada sahip.  UrlScheme belirtilen URL şeması protokolü ile aynı olmalıdır **yeniden yönlendirme URL'lere izin** Azure portalında alan. UrlScheme kimlik doğrulama geri çağırmasının tarafından kimlik doğrulama isteği tamamlandıktan sonra geri uygulamanıza geçiş yapmak için kullanılır.

2. Değiştir `[self refresh]` içinde `viewDidLoad` içinde *QSTodoListViewController.m* aşağıdaki kod ile:

    ```Objective-C
    [self loginAndGetData];
    ```

3. Açık `QSAppDelegate.h` dosya ve aşağıdaki kodu ekleyin:

    ```Objective-C
    #import "QSTodoService.h"

    @property (strong, nonatomic) QSTodoService *qsTodoService;
    ```

4. Açık `QSAppDelegate.m` dosya ve aşağıdaki kodu ekleyin:

    ```Objective-C
    - (BOOL)application:(UIApplication *)application openURL:(NSURL *)url options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options
    {
        if ([[url.scheme lowercaseString] isEqualToString:@"appname"]) {
            // Resume login flow
            return [self.qsTodoService.client resumeWithURL:url];
        }
        else {
            return NO;
        }
    }
    ```

   Bu kod satırı okuma hemen öncesine eklemek `#pragma mark - Core Data stack`.  Değiştir _appname_ olan 1. adımda kullanılan urlScheme değeri.

5. Açık `AppName-Info.plist` (uygulamanızın adıyla değiştirerek AppName) dosyasını bulun ve aşağıdaki kodu ekleyin:

    ```XML
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLName</key>
            <string>com.microsoft.azure.zumo</string>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>appname</string>
            </array>
        </dict>
    </array>
    ```

    Bu kod içinde yerleştirilmelidir `<dict>` öğesi.  Değiştir _appname_ dize (dizi için içinde **CFBundleURLSchemes**) 1. adımda seçtiğiniz uygulama adı ile.  Ayrıca bu plist Düzenleyicisi - tıklayarak değişiklik yapabilirsiniz `AppName-Info.plist` plist Düzenleyicisi'ni açmak için XCode dosyasında.

    Değiştir `com.microsoft.azure.zumo` dize **CFBundleURLName** , Apple ile paket tanımlayıcı.

6. Tuşuna *çalıştırmak* uygulamayı başlatın ve sonra oturum açın. Oturum açtıktan sonra Yapılacaklar listesi görebilir ve güncelleştirme yapmak olmalıdır.

**SWIFT**:

1. Mac'inizde açmak *ToDoTableViewController.swift* xcode'da ve aşağıdaki yöntemi ekleyin:

    ```swift
    func loginAndGetData() {

        guard let client = self.table?.client, client.currentUser == nil else {
            return
        }

        let appDelegate = UIApplication.shared.delegate as! AppDelegate
        appDelegate.todoTableViewController = self

        let loginBlock: MSClientLoginBlock = {(user, error) -> Void in
            if (error != nil) {
                print("Error: \(error?.localizedDescription)")
            }
            else {
                client.currentUser = user
                print("User logged in: \(user?.userId)")
            }
        }

        client.login(withProvider:"google", urlScheme: "appname", controller: self, animated: true, completion: loginBlock)

    }
    ```

    Değişiklik *google* için *microsoftaccount*, *twitter*, *facebook*, veya *windowsazureactivedirectory* kimlik sağlayıcınız olarak Google kullanmıyorsanız. Facebook kullanmak istiyorsanız, gerekir [beyaz liste Facebook etki alanları] [ 1] uygulamanızda.

    Değiştir **urlScheme** uygulamanız için benzersiz bir ada sahip.  UrlScheme belirtilen URL şeması protokolü ile aynı olmalıdır **yeniden yönlendirme URL'lere izin** Azure portalında alan. UrlScheme kimlik doğrulama geri çağırmasının tarafından kimlik doğrulama isteği tamamlandıktan sonra geri uygulamanıza geçiş yapmak için kullanılır.

2. Satırları kaldırmak `self.refreshControl?.beginRefreshing()` ve `self.onRefresh(self.refreshControl)` sonunda `viewDidLoad()` içinde *ToDoTableViewController.swift*. Bir çağrı ekleyin `loginAndGetData()` kendi yerinde:

    ```swift
    loginAndGetData()
    ```

3. Açık `AppDelegate.swift` dosya ve aşağıdaki satırı ekleyin `AppDelegate` sınıfı:

    ```swift
    var todoTableViewController: ToDoTableViewController?

    func application(_ application: UIApplication, openURL url: NSURL, options: [UIApplicationOpenURLOptionsKey : Any] = [:]) -> Bool {
        if url.scheme?.lowercased() == "appname" {
            return (todoTableViewController!.table?.client.resume(with: url as URL))!
        }
        else {
            return false
        }
    }
    ```

    Değiştir _appname_ olan 1. adımda kullanılan urlScheme değeri.

4. Açık `AppName-Info.plist` (uygulamanızın adıyla değiştirerek AppName) dosyasını bulun ve aşağıdaki kodu ekleyin:

    ```xml
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLName</key>
            <string>com.microsoft.azure.zumo</string>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>appname</string>
            </array>
        </dict>
    </array>
    ```

    Bu kod içinde yerleştirilmelidir `<dict>` öğesi.  Değiştir _appname_ dize (dizi için içinde **CFBundleURLSchemes**) 1. adımda seçtiğiniz uygulama adı ile.  Ayrıca bu plist Düzenleyicisi - tıklayarak değişiklik yapabilirsiniz `AppName-Info.plist` plist Düzenleyicisi'ni açmak için XCode dosyasında.

    Değiştir `com.microsoft.azure.zumo` dize **CFBundleURLName** , Apple ile paket tanımlayıcı.

5. Tuşuna *çalıştırmak* uygulamayı başlatın ve sonra oturum açın. Oturum açtıktan sonra Yapılacaklar listesi görebilir ve güncelleştirme yapmak olmalıdır.

Uygulama hizmeti kimlik doğrulama elmalar Inter uygulama iletişimi kullanır.  Bu konu hakkında daha fazla ayrıntı için başvurmak [Apple belgeleri][2]
<!-- URLs. -->

[1]: https://developers.facebook.com/docs/ios/ios9#whitelist
[2]: https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Inter-AppCommunication/Inter-AppCommunication.html
[Azure portal]: https://portal.azure.com

[iOS Hızlı Başlat]: app-service-mobile-ios-get-started.md

