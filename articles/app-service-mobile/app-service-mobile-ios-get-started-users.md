---
title: Azure Mobile Apps ile iOS kimlik doğrulaması ekleme
description: Kimlik sağlayıcıları, AAD, Google, Facebook, Twitter ve Microsoft gibi çeşitli iOS uygulamanızdaki kullanıcıların kimliğini doğrulamak için Azure Mobile Apps'ı kullanmayı öğrenin.
services: app-service\mobile
documentationcenter: ios
author: conceptdev
manager: crdun
editor: ''
ms.assetid: ef3d3cbe-e7ca-45f9-987f-80c44209dc06
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: crdun
ms.openlocfilehash: 8c1c52790065015977add7e32a06063057b24dad
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62128158"
---
# <a name="add-authentication-to-your-ios-app"></a>İOS uygulamanıza kimlik doğrulaması ekleme
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Bu öğreticide, kimlik doğrulaması ekleme [iOS Hızlı Başlangıç] desteklenen kimlik sağlayıcısı kullanarak proje. Bu öğreticide dayanır [iOS Hızlı Başlangıç] Öğreticisi, öncelikle tamamlamanız gerekir.

## <a name="register"></a>Kimlik doğrulaması için uygulamanızı kaydetme ve App Service'ı yapılandırma
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>İzin verilen dış yönlendirme URL'leri uygulamanıza ekleyin

Uygulamanız için yeni bir URL şemasını tanımlamak güvenli kimlik doğrulaması gerektirir.  Bu kimlik doğrulama işlemi tamamlandıktan sonra uygulamanıza geri yönlendirmek bir kimlik doğrulama sistemi sağlar.  Bu öğreticide, kullandığımız URL şeması _appname_ boyunca.  Ancak, seçtiğiniz herhangi bir URL şeması kullanabilirsiniz.  Mobil uygulamanız için benzersiz olmalıdır.  Th sunucu tarafında yeniden yönlendirmeyi etkinleştirmek için:

1. İçinde [Azure portal], App Service'ı seçin.

2. Tıklayın **kimlik doğrulama / yetkilendirme** menü seçeneği.

3. Tıklayın **Azure Active Directory** altında **kimlik doğrulama sağlayıcıları** bölümü.

4. Ayarlama **yönetim modu** için **Gelişmiş**.

5. İçinde **izin verilen dış yönlendirme URL'leri**, girin `appname://easyauth.callback`.  _Appname_ bu dizesinde mobil uygulamanız için URL şeması aşağıdaki gibidir.  Bu, bir protokol (kullanım harf ve yalnızca sayı ve bir harfle) için normal URL belirtimi izlemeniz gerekir.  Çeşitli yerlerde URL şeması ile mobil uygulama kodunuzu ayarlamak kullanmanız gerektiğinden, seçtiğiniz dizenin Not.

6. **Tamam** düğmesine tıklayın.

7. **Kaydet**’e tıklayın.

## <a name="permissions"></a>Kimliği doğrulanmış kullanıcılar için izinleri kısıtla
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Xcode içindeki basın **çalıştırma** uygulamasını başlatmak için. Uygulama Kimliği doğrulanmamış bir kullanıcı olarak arka uç erişmeye çalıştığı bir özel durum oluşturulur ancak *Todoıtem* tablo artık kimlik doğrulaması gerektirir.

## <a name="add-authentication"></a>Kimlik doğrulaması için uygulama ekleme
**Objective-C**:

1. Mac'inizde açın *QSTodoListViewController.m* xcode'da ve aşağıdaki yöntemi ekleyin:

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

    Değişiklik *google* için *microsoftaccount*, *twitter*, *facebook*, veya *windowsazureactivedirectory* kimlik sağlayıcınız olarak Google kullanmıyorsanız. Facebook kullanıyorsanız yapmanız gerekenler [beyaz liste Facebook etki alanları] [ 1] uygulamanızda.

    Değiştirin **urlScheme** uygulamanız için benzersiz bir ada sahip.  UrlScheme belirtilen URL şeması protokolü ile aynı olmalıdır **izin verilen dış yönlendirme URL'leri** Azure portalında alan. UrlScheme doğrulama geri çağırmasının tarafından kimlik doğrulama isteği tamamlandıktan sonra uygulamanıza geri geçiş yapmak için kullanılır.

2. Değiştirin `[self refresh]` içinde `viewDidLoad` içinde *QSTodoListViewController.m* aşağıdaki kod ile:

    ```Objective-C
    [self loginAndGetData];
    ```

3. Açık `QSAppDelegate.h` dosyasını açıp aşağıdaki kodu ekleyin:

    ```Objective-C
    #import "QSTodoService.h"

    @property (strong, nonatomic) QSTodoService *qsTodoService;
    ```

4. Açık `QSAppDelegate.m` dosyasını açıp aşağıdaki kodu ekleyin:

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

   Bu kod satırı okunmadan doğrudan ekleme `#pragma mark - Core Data stack`.  Değiştirin _appname_ 1. adımda kullanılan urlScheme değerine sahip.

5. Açık `AppName-Info.plist` dosya (uygulama adıyla değiştirmeyi AppName) ve aşağıdaki kodu ekleyin:

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

    Bu kod içine yerleştirilmesi gereken `<dict>` öğesi.  Değiştirin _appname_ dize (için dizi içinde **CFBundleURLSchemes**) 1. adımda seçtiğiniz uygulama adıyla.  Ayrıca bu plist Düzenleyicisi - tıklayarak değişiklik yapabilirsiniz `AppName-Info.plist` plist Düzenleyicisi'ni açmak için XCode dosyasında.

    Değiştirin `com.microsoft.azure.zumo` dize **CFBundleURLName** , Apple ile paket grubu tanımlayıcısı.

6. Tuşuna *çalıştırma* uygulamayı başlatın ve sonra oturum açın. Oturum açtıktan sonra Yapılacaklar listesi görüntüleyin ve güncelleştirme yapmak mümkün olmayacaktır.

**Swift**:

1. Mac'inizde açın *ToDoTableViewController.swift* xcode'da ve aşağıdaki yöntemi ekleyin:

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

    Değişiklik *google* için *microsoftaccount*, *twitter*, *facebook*, veya *windowsazureactivedirectory* kimlik sağlayıcınız olarak Google kullanmıyorsanız. Facebook kullanıyorsanız yapmanız gerekenler [beyaz liste Facebook etki alanları] [ 1] uygulamanızda.

    Değiştirin **urlScheme** uygulamanız için benzersiz bir ada sahip.  UrlScheme belirtilen URL şeması protokolü ile aynı olmalıdır **izin verilen dış yönlendirme URL'leri** Azure portalında alan. UrlScheme doğrulama geri çağırmasının tarafından kimlik doğrulama isteği tamamlandıktan sonra uygulamanıza geri geçiş yapmak için kullanılır.

2. Satırları kaldırmak `self.refreshControl?.beginRefreshing()` ve `self.onRefresh(self.refreshControl)` sonunda `viewDidLoad()` içinde *ToDoTableViewController.swift*. Bir çağrı ekleyin `loginAndGetData()` kendi yerinde:

    ```swift
    loginAndGetData()
    ```

3. Açık `AppDelegate.swift` dosyasını açıp aşağıdaki satırı ekleyin `AppDelegate` sınıfı:

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

    Değiştirin _appname_ 1. adımda kullanılan urlScheme değerine sahip.

4. Açık `AppName-Info.plist` dosya (uygulama adıyla değiştirmeyi AppName) ve aşağıdaki kodu ekleyin:

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

    Bu kod içine yerleştirilmesi gereken `<dict>` öğesi.  Değiştirin _appname_ dize (için dizi içinde **CFBundleURLSchemes**) 1. adımda seçtiğiniz uygulama adıyla.  Ayrıca bu plist Düzenleyicisi - tıklayarak değişiklik yapabilirsiniz `AppName-Info.plist` plist Düzenleyicisi'ni açmak için XCode dosyasında.

    Değiştirin `com.microsoft.azure.zumo` dize **CFBundleURLName** , Apple ile paket grubu tanımlayıcısı.

5. Tuşuna *çalıştırma* uygulamayı başlatın ve sonra oturum açın. Oturum açtıktan sonra Yapılacaklar listesi görüntüleyin ve güncelleştirme yapmak mümkün olmayacaktır.

App Service kimlik doğrulaması elma uygulamalar arası iletişimi kullanır.  Bu konu hakkında daha fazla ayrıntı için başvurmak [Apple belgeleri][2]
<!-- URLs. -->

[1]: https://developers.facebook.com/docs/ios/ios9#whitelist
[2]: https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Inter-AppCommunication/Inter-AppCommunication.html
[Azure portal]: https://portal.azure.com

[iOS hızlı başlangıç]: app-service-mobile-ios-get-started.md

