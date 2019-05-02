---
title: Microsoft kimlik platformu iOS hızlı başlangıç | Azure
description: Oturum açma öğrenin kullanıcıların ve Microsoft Graph sorguda bir iOS uygulaması.
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/18/2019
ms.author: dadobali
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 32cc373421e6b04737f40dc987b10c6ace858e17
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64937365"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-ios-app"></a>Hızlı Başlangıç: Kullanıcılar oturum ve bir iOS uygulamasından Microsoft Graph API çağırma

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

Bu hızlı başlangıç, yerel bir iOS uygulaması ile kişisel, iş ve okul hesaplarının oturumunu açmayı, erişim belirteci almayı ve Microsoft Graph API’sini çağırmayı gösteren bir kod örneği içerir.

![Bu Hızlı Başlangıç ile oluşturulan örnek uygulamasını nasıl çalıştığını gösterir](media/quickstart-v2-ios/ios-intro.svg)

> [!NOTE]
> **Önkoşullar**
> * XCode 10+
> * iOS 10+ 

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme
> Hızlı başlangıç uygulamanızı başlatmak için kullanabileceğiniz iki seçenek vardır:
> * [Express] [Seçenek 1: Kaydet ve otomatik Uygulamanızı yapılandırmak ve ardından, kod örneğini indirin](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [El ile] [Seçeneği 2: Kaydetme ve uygulama ve kod örneğinizi el ile yapılandırma](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>1. seçenek: Kaydet ve otomatik Uygulamanızı yapılandırmak ve ardından, kod örneğini indirin
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
> Uygulamanızı kaydetmek için
> 1. Yeni Git [Azure Portalı - Uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/IosQuickstartPage/sourceType/docs) bölmesi.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
> 1. Yönergeleri izleyerek yeni uygulamanızı yalnızca tek tıklamayla indirin ve otomatik olarak yapılandırın.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2. seçenek: Kaydetme ve uygulama ve kod örneğinizi el ile yapılandırma
>
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
> Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze el ile eklemek için şu adımları izleyin:
>
> 1. Geliştiriciler için Microsoft identity platformuna gidin [uygulama kayıtları](https://aka.ms/MobileAppReg) sayfası.
> 1. Seçin **yeni kayıt**.
> 1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:
>      - İçinde **adı** bölümünde, oturum açın veya örneğin uygulamanıza onay, uygulamayı kullanıcılara görüntülenecek bir anlamlı uygulama adı girin `iOSQuickstart`.
>      - Bu sayfada diğer yapılandırmalar atlayın. 
>      - İsabet `Register` düğmesi.
> 1. Yeni uygulama tıklayın > Git `Authentication`  >  `Add Platform`  >  `iOS`.    
>      - Girin ***paket grubu tanımlayıcısı*** uygulamanız için. 
> 1. Seçin `Configure` kaydedip ***MSAL yapılandırma*** için daha sonra ayrıntıları. 

> [!div renderon="portal" class="sxs-lookup"]
>
> #### <a name="step-1-configure-your-application"></a>1. Adım: Uygulamanızı yapılandırma
> Çalışmak bu hızlı başlangıç için kod örneği için bir yeniden yönlendirme URI'si ile kimlik doğrulama Aracısı uyumlu eklemeniz gerekir. 
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişikliği benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-ios/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış

#### <a name="step-2-download-your-web-server-or-project"></a>2. Adım: Web sunucunuzda veya proje indirme

- [Kod örneğini indirin](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip)

#### <a name="step-3-configure-your-project"></a>3. Adım: Projenizi yapılandırın

> [!div renderon="docs"]
> Yukarıdaki seçeneği 1'i seçtiyseniz, şu adımları atlayabilirsiniz. 

> [!div renderon="portal" class="sxs-lookup"]
> 1. Zip dosyasını ayıklayın ve projeyi XCode’da açın.
> 1. **ViewController.swift**’i düzenleyin ve 'let kClientID' ile başlayan satırı aşağıdaki kod parçacığı ile değiştirin:
>    ```swift
>    let kClientID = "Enter_the_Application_Id_here"
>    let kAuthority = "https://login.microsoftonline.com/Enter_the_Tenant_Info_Here"
>
>    ```
> 1. Sağ tıklayın **Info.plist** seçip **açık olarak** > **kaynak kodu**.
> 1. Değiştirin dict kök düğümü altında ***paket kimliği***:
>
>    ```xml
>    <key>CFBundleURLTypes</key>
>    <array>
>       <dict>
>          <key>CFBundleURLSchemes</key>
>          <array>
>             <string>msauth.Enter_the_Bundle_Id_Here</string>
>          </array>
>       </dict>
>    </array>
> 
>    ```
> 1. Derleme ve uygulamayı çalıştırın! 

> [!div renderon="docs"]
>
> 1. Zip dosyasını ayıklayın ve projeyi XCode’da açın.
> 1. Düzen **ViewController.swift** 'let kClientID' ile aşağıdaki kod parçacığı ile başlayan satırı değiştirin:
>
>    ```swift
>    let kClientID = "<ENTER_YOUR_APPLICATION/CLIENT_ID>"
> 
>    ```
> 1. Sağ tıklayın **Info.plist** seçip **açık olarak** > **kaynak kodu**.
> 1. Değiştirin dict kök düğümü altında ***paket kimliği***:
>
>    ```xml
>    <key>CFBundleURLTypes</key>
>    <array>
>       <dict>
>          <key>CFBundleURLSchemes</key>
>          <array>
>             <string>msauth.<ENTER_YOUR_BUNDLE_ID></string>
>          </array>
>       </dict>
>    </array>
>
>    ```
> 1. Derleme ve uygulamayı çalıştırın! 

## <a name="more-information"></a>Daha Fazla Bilgi

Bu hızlı başlangıç hakkında daha fazla bilgi almak için şu bölümleri okuyun.

### <a name="getting-msal"></a>MSAL alma

MSAL ([MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)) kullanıcılarının oturumunu ve Microsoft kimlik platformu tarafından korunan bir API'ye erişmek için kullanılan belirteci istemek için kullanılan bir kitaplık sunulmaktadır. Aşağıdaki işlemi uygulayarak uygulamanıza MSAL ekleyebilirsiniz:

```
$ vi Podfile

```
Aşağıdakileri bu podfile (ile projenizin hedef) ekleyin:

```
use_frameworks!

target 'MSALiOS' do
   pod 'MSAL', '~> 0.4.0'
end

```

### <a name="msal-initialization"></a>MSAL başlatma

Şu kodu ekleyerek MSAL başvurusunu ekleyebilirsiniz:

```swift
import MSAL
```

Sonra şu kodu kullanarak MSAL'yi başlatın:

```swift
let authority = try MSALAADAuthority(url: URL(string: kAuthority)!)
            
let msalConfiguration = MSALPublicClientApplicationConfig(clientId: kClientID, redirectUri: nil, authority: authority)
self.applicationContext = try MSALPublicClientApplication(configuration: msalConfiguration)

```

> |Konumlar: ||
> |---------|---------|
> | `clientId` | *portal.azure.com* adresinde kayıtlı uygulamaya ait Uygulama Kimliği |
> | `authority` | Microsoft kimlik platformu uç noktası. Çoğu durumda bu *https<span/>://login.microsoftonline.com/common* olur |
> | `redirectUri` | Yeniden yönlendirme URI'sini uygulama. Özel yeniden yönlendirme URI'si veya varsayılan değeri kullanmak için ' nil' geçirebilirsiniz. |

### <a name="additional-app-requirements"></a>Ek uygulama gereksinimleri  

Uygulamanız aynı zamanda aşağıdaki olmalı, `AppDelegate`. Bu MSAL SDK'sı, kimlik doğrulaması gerçekleştirdiğinizde, kimlik doğrulama Aracısı uygulamasından belirteç yanıtı işlemek sağlar.

 ```swift
 func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
         guard let sourceApplication = options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String else {
             return false
         }
         
         return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: sourceApplication)
     }

```

Son olarak, uygulamanız gereken sahip bir `LSApplicationQueriesSchemes` girişinde, ***Info.plist*** yanı sıra `CFBundleURLTypes`. Örnek dahil bu ile birlikte gelir. 

   ```xml 
   <key>LSApplicationQueriesSchemes</key>
   <array>
      <string>msauth</string>
      <string>msauthv2</string>
   </array>
   ```

### <a name="sign-in-users--request-tokens"></a>Belirteçleri istek & kullanıcılarının oturumunu

Belirteç almak için MSAL’in iki yöntemi vardır: `acquireToken` ve `acquireTokenSilent`.

#### <a name="acquiretoken-getting-a-token-interactively"></a>acquireToken: Etkileşimli bir belirteci alma

Bazı durumlarda kullanıcıların Microsoft kimlik platformu ile etkileşime geçmesini gerektirir. Bu durumlarda, son kullanıcı hesabını seçin, kimlik bilgilerini girin veya uygulamanızın izinleri onayı gerekli olabilir. Örneğin, 

* Kullanıcılar uygulamada ilk kez oturum açtığında
* Bir kullanıcının parolasını sıfırlar, bunlar kimlik bilgilerini girmeniz gerekir 
* Ne zaman, uygulama bir kaynağa erişim ilk kez istiyor
* Mfa'yı veya diğer koşullu erişim ilkelerini gerekli olduğunda

```swift
let parameters = MSALInteractiveTokenParameters(scopes: kScopes)
applicationContext.acquireToken(with: parameters) { (result, error) in /* Add your handling logic */}
```

> |Konumlar:||
> |---------|---------|
> | `scopes` | İstenen kapsam içerir (diğer bir deyişle, `[ "user.read" ]` Microsoft Graph için veya `[ "<Application ID URL>/scope" ]` özel Web API'leri için (`api://<Application ID>/access_as_user`) |

#### <a name="acquiretokensilent-getting-an-access-token-silently"></a>acquireTokenSilent: Erişim belirtecini sessiz bir şekilde alma

Uygulamaları bir belirteç istediklerinde her zaman oturum açmak kullanıcıları gerekmez. Kullanıcı zaten açtıysa, bu yöntem sessizce belirteçler istemek uygulamalar sağlar. 

```swift
let parameters = MSALSilentTokenParameters(scopes: kScopes, account: applicationContext.allAccounts().first)
applicationContext.acquireTokenSilent(with: parameters) { (result, error) in /* Add your handling logic */}
```

> |Konumlar: ||
> |---------|---------|
> | `scopes` | İstenen kapsam içerir (diğer bir deyişle, `[ "user.read" ]` Microsoft Graph için veya `[ "<Application ID URL>/scope" ]` özel Web API'leri için (`api://<Application ID>/access_as_user`) |
> | `account` | Bir belirteç hesabı için istenen. Belirteç istekleri için kullanılacak hesabı tanımlamak için mantığı tanımlamak için ihtiyacınız olacak bir çok hesabı uygulaması oluşturmak istiyorsanız bu hızlı başlangıçta bir tek hesap, uygulamasıdır `applicationContext.account(forHomeAccountId: self.homeAccountId)` |

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta tam bir açıklaması gibi uygulamaları oluşturmaya tam bir adım adım kılavuz için iOS Eğitim programını deneyin.

### <a name="learn-the-steps-to-create-the-application-used-in-this-quickstart"></a>Bu hızlı başlangıçta kullanılan uygulamayı oluşturma adımlarını öğrenin

> [!div class="nextstepaction"]
> [Graph API’si çağırma iOS öğreticisi](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-ios)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]