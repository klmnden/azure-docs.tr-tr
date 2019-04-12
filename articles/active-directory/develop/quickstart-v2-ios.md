---
title: Microsoft kimlik platformu iOS hızlı başlangıç | Azure
description: Yerel bir iOS uygulamasında kullanıcılara oturum açmayı ve Microsoft Graph sorgulamayı öğrenin.
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
ms.date: 03/20/2019
ms.author: dadobali
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: e6340e0f349d66ecf6baaca481722396a6d786c5
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59496138"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-ios-native-app"></a>Hızlı Başlangıç: Kullanıcılar oturum ve bir iOS yerel uygulamadan Microsoft Graph API çağırma

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

Bu hızlı başlangıç, yerel bir iOS uygulaması ile kişisel, iş ve okul hesaplarının oturumunu açmayı, erişim belirteci almayı ve Microsoft Graph API’sini çağırmayı gösteren bir kod örneği içerir.

![Bu Hızlı Başlangıç ile oluşturulan örnek uygulamasını nasıl çalıştığını gösterir](media/quickstart-v2-ios/ios-intro.svg)

> [!div renderon="docs"]
> ## <a name="register-and-download"></a>Kaydolma ve indirme
> ### <a name="register-and-configure-your-application-and-code-sample"></a>Uygulamanızı ve kod örneğinizi kaydetme ve yapılandırma
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
> Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze eklemek için aşağıdakileri yapın:
> 1. Uygulamayı kaydetmek için [Microsoft Uygulama Kayıt Portalı](https://apps.dev.microsoft.com/portal/register-app)’na gidin.
> 1. **Uygulama Adı** kutusuna uygulamanız için bir ad girin.
> 1. **Destekli Kurulum** onay kutusunun işaretli olmadığından emin olun ve **Oluştur**’u seçin.
> 1. **Platform Ekle**’yi, **Yerel Uygulama**’yı ve **Kaydet**’i seçin.

> [!div renderon="portal" class="sxs-lookup"]
> #### <a name="step-1-configure-your-application"></a>1. Adım: Uygulamanızı yapılandırma
> Çalışmak bu hızlı başlangıç için kod örneği için bir yanıt URL'si olarak eklemek istediğiniz `msal<AppId>://auth` (burada msal\<AppID > Bu uygulama kimliği).
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Benim için bu değişiklik yapın]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Önceden yapılandırılmış](media/quickstart-v2-ios/green-check.png) uygulamanız bu öznitelikle yapılandırılana

#### <a name="step-2-download-your-web-server-or-project"></a>2. Adım: Web sunucunuzda veya proje indirme

- [XCode projesi indirme](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip)

#### <a name="step-3-configure-your-project"></a>3. Adım: Projenizi yapılandırın

1. Zip dosyasını ayıklayın ve projeyi XCode’da açın.
1. **ViewController.swift**’i düzenleyin ve 'let kClientID' ile başlayan satırı aşağıdaki kod parçacığı ile değiştirin:

    > [!div renderon="portal" class="sxs-lookup"]
    > ```swift
    > let kClientID = "Enter_the_Application_Id_here"
    > ```

    > [!div renderon="docs"]
    > ```swift
    > let kClientID = "<ENTER_THE_APPLICATION_ID_HERE>"
    > ```   
1. Control tuşunu basılı tutarken **Info.plist**’e tıklayarak bağlamsal menüyü getirin ve **Kaynak Kod** **Olarak Aç** > ’ı seçin.
1. Dict root düğümü altında aşağıdaki kodu ekleyin:

    > [!div renderon="portal" class="sxs-lookup"]
    > ```xml
    > <key>CFBundleURLTypes</key>
    > <array>
    >     <dict>
    >         <key>CFBundleTypeRole</key>
    >         <string>Editor</string>
    >         <key>CFBundleURLName</key>
    >         <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
    >         <key>CFBundleURLSchemes</key>
    >         <array>
    >             <string>msalEnter_the_Application_Id_here</string>
    >         </array>
    >     </dict>
    > </array>
    > ```

    > [!div renderon="docs"]
    > ```xml
    > <key>CFBundleURLTypes</key>
    > <array>
    >     <dict>
    >         <key>CFBundleTypeRole</key>
    >         <string>Editor</string>
    >         <key>CFBundleURLName</key>
    >         <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
    >         <key>CFBundleURLSchemes</key>
    >         <array>
    >             <string>msal<ENTER_THE_APPLICATION_ID_HERE></string>
    >         </array>
    >     </dict>
    > </array>
    > ```
    
> [!div renderon="docs"]
> <span>5.</span> `<ENTER_THE_APPLICATION_ID_HERE>` kısmını uygulamanız için olan *Uygulama Kimliği* ile değiştirin. *Uygulama Kimliği*’ni bulmanız gerekiyorsa *Genel Bakış* sayfasına gidin.

## <a name="more-information"></a>Daha Fazla Bilgi

Bu hızlı başlangıç hakkında daha fazla bilgi almak için şu bölümleri okuyun.

### <a name="msal"></a>MSAL

MSAL ([MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)) kullanıcıların oturumlarını açmak için kullanılan kitaplığı ve Microsoft Azure Active Directory tarafından korunan bir API’ye erişmek için kullanılan istek belirteçlerini içerir. Aşağıdaki işlemi uygulayarak uygulamanıza MSAL ekleyebilirsiniz:

```
$ vi Podfile
```
Aşağıdakileri bu podfile dosyaya ekleyin:

```
 target 'QuickStart' do
   use_frameworks!
 pod 'MSAL'
 end
```

### <a name="msal-initialization"></a>MSAL başlatma

Şu kodu ekleyerek MSAL başvurusunu ekleyebilirsiniz:

```swift
import MSAL
```

Sonra şu kodu kullanarak MSAL'yi başlatın:

```swift
let authority = MSALAuthority(url: URL(string: kAuthority)!)
self.applicationContext = try MSALPublicClientApplication(clientId: kClientID, authority: authority)
```

> |Konumlar: ||
> |---------|---------|
> | `clientId` | *portal.azure.com* adresinde kayıtlı uygulamaya ait Uygulama Kimliği |
> | `authority` | Microsoft kimlik platformu uç noktası. Çoğu durumda bu *https<span/>://login.microsoftonline.com/common* olur |

### <a name="requesting-tokens"></a>Belirteç isteme

Belirteç almak için MSAL’in iki yöntemi vardır: `acquireToken` ve `acquireTokenSilent`.

#### <a name="getting-an-access-token-interactively"></a>Etkileşimli bir şekilde erişim belirteci alma

Bazı durumlarda, kullanıcılar ya da kullanıcı kimlik bilgilerini doğrulamak için sistemi tarayıcıya ya da onay için bir içerik anahtarı sonuçlanacak Microsoft kimlik platformu uç ile etkileşim kurmak için zorlama gerektirir. Bazı örnekler:

* Kullanıcılar uygulamada ilk kez oturum açtığında
* Parolanın süresi dolduğundan kullanıcıların kimlik bilgilerini yeniden girmesi gerektiğinde
* Uygulamanız kullanıcının onaylaması gereken bir kaynağa erişim istediğinde
* İki faktörlü kimlik doğrulama gerektiğinde

```swift
applicationContext.acquireToken(forScopes: self.kScopes) { (result, error) in /* Add your handling logic */}
```

> |Konumlar:||
> |---------|---------|
> | `forScopes` | İstenilen kapsamları içerir (yani Microsoft Graph için `[ "user.read" ]` veya özel Web API’leri için `[ "<Application ID URL>/scope" ]` (örn. `api://<Application ID>/access_as_user`)) |

#### <a name="getting-an-access-token-silently"></a>Erişim belirtecini sessiz bir şekilde alma

Kullanıcının bir kaynağa erişmesi gerektiği her seferde kimlik bilgilerini doğrulamasının gerekmesini istemezsiniz. Çoğu zaman belirteç alımları ve yenilemelerinin kullanıcı etkileşimi olmadan gerçekleşmesini istersiniz. İlk `acquireToken` yönteminden sonra korunan kaynaklara erişmek üzere belirteçleri almak için `acquireTokenSilent` yöntemini kullanabilirsiniz:

```swift
applicationContext.acquireTokenSilent(forScopes: self.kScopes, account: applicationContext.allAccounts().first) { (result, error) in /* Add your handling logic */}
```

> |Konumlar: ||
> |---------|---------|
> | `forScopes` | İstenilen kapsamları içerir (yani Microsoft Graph için `[ "user.read" ]` veya özel Web API’leri için `[ "<Application ID URL>/scope" ]` (örn. `api://<Application ID>/access_as_user`)) |
> | `account` | Belirteci isteyen hesap (MSAL tek bir uygulamada birden çok hesabı destekler). Bu Hızlı Başlangıç örneğinde, değer önbellekteki (`applicationContext.allAccounts().first`) ilk hesaba işaret eder. |

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıcın tam açıklaması dahil olmak üzere uygulama geliştirme ve yeni özelliklere ilişkin tam bir adım adım kılavuzu için iOS öğreticisini deneyin.

### <a name="learn-the-steps-to-create-the-application-used-in-this-quickstart"></a>Bu hızlı başlangıçta kullanılan uygulamayı oluşturma adımlarını öğrenin

> [!div class="nextstepaction"]
> [Graph API'si iOS öğreticisini çağırın](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-ios)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
