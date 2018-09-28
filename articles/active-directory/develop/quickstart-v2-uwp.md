---
title: Azure AD v2 Windows UWP hızlı başlangıcı | Microsoft Docs
description: Evrensel Windows Platformu (XAML) uygulamasının nasıl erişim belirteci alabileceğini ve Azure Active Directory v2.0 uç noktasıyla korunan bir API'yi nasıl çağırabileceğini öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/25/2018
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 5241df87297fb2e293e2cc828821e66d6f2837b0
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46970837"
---
# <a name="call-the-microsoft-graph-api-from-a-universal-windows-platform-uwp-application"></a>Evrensel Windows Platformu (UWP) uygulamasından Microsoft Graph API'sini çağırma

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

Bu hızlı başlangıç, bir Evrensel Windows Platformu (UWP) uygulaması ile kişisel, iş ve okul hesaplarıyla kullanıcıların oturumunu açmayı, erişim belirteci almayı ve Microsoft Graph API’sini çağırmayı gösteren bir kod örneği içerir.

![Bu Hızlı Başlangıcın oluşturduğu örnek uygulama nasıl çalışır?](media/quickstart-v2-uwp/uwp-intro.png)

> [!div renderon="docs"]
> ## <a name="register-and-download"></a>Kaydetme ve indirme
> ### <a name="register-and-configure-your-application-and-code-sample"></a>Uygulamanızı ve kod örneğinizi kaydetme ve yapılandırma
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
> Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze eklemek için aşağıdakileri yapın:
> 1. Uygulamayı kaydetmek için [Microsoft Uygulama Kayıt Portalı](https://apps.dev.microsoft.com/portal/register-app)’na gidin.
> 1. **Uygulama Adı** kutusuna uygulamanız için bir ad girin.
> 1. **Destekli Kurulum** onay kutusunun işaretli olmadığından emin olun ve **Oluştur**’u seçin.
> 1. **Platform Ekle**’yi, **Yerel Uygulama**’yı ve **Kaydet**’i seçin.

> [!div renderon="portal" class="sxs-lookup alert alert-info"]
> #### <a name="step-1-configure-your-application"></a>1. Adım: Uygulamanızı yapılandırma
> Bu hızlı başlangıç kod örneğinin çalışması için **urn:ietf:wg:oauth:2.0:oob** gibi bir yeniden yönlendirme URL’si eklemeniz gerekir.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişikliği benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-uwp/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış

#### <a name="step-2-download-your-visual-studio-project"></a>2. Adım: Visual Studio projenizi indirme

 - [Visual Studio 2017 projesini indirin](https://github.com/Azure-Samples/active-directory-dotnet-native-uwp-v2/archive/master.zip)

#### <a name="step-3-configure-your-visual-studio-project"></a>3. Adım: Visual Studio projenizi yapılandırma

1. Zip dosyasını yerel bir klasöre (örneğin **C:\Azure-Samples**) açın
1. Projeyi Visual Studio'da açın
1. **App.Xaml.cs** dosyasını düzenleyin ve `private static string ClientId` ile başlayan satırın yerine aşağıdakini koyun:

    ```csharp
    private static string ClientId = "Enter_the_Application_Id_here";
    ```

## <a name="more-information"></a>Daha Fazla Bilgi

Aşağıda bu Hızlı Başlangıca genel bir bakış yer alır:

### <a name="msalnet"></a>MSAL.NET

MSAL ([Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)) kullanıcı oturumlarını açmak ve Microsoft Azure Active Directory tarafından korunan bir API'ye erişmek için kullanılan belirteçler istemek için kullanılan kitaplıktır. Kitaplığı Visual Studio'nun *Paket Yöneticisi Konsolu*'nda aşağıdaki komutu çalıştırarak yükleyebilirsiniz:

```powershell
Install-Package Microsoft.Identity.Client -Pre
```

### <a name="msal-initialization"></a>MSAL başlatma

Aşağıdaki satırı ekleyerek MSAL başvurusunu ekleyebilirsiniz:

```csharp
using Microsoft.Identity.Client;
```

Sonra aşağıdaki satırı kullanarak MSAL'yi başlatın:

```csharp
public static PublicClientApplication PublicClientApp = new PublicClientApplication(ClientId);
```

> |Konumlar: ||
> |---------|---------|
> |ClientId | *portal.microsoft.com*’da kaydedilen uygulamanın Uygulama Kimliği |

### <a name="requesting-tokens"></a>Belirteç isteme

MSAL'nin belirteç almak için kullanılan iki yöntemi vardır - `AcquireTokenAsync` ve `AcquireTokenSilentAsync`:

#### <a name="get-a-user-token-interactively"></a>Etkileşimli olarak kullanıcı belirteci alma

 Bazı durumlar, kimlik bilgilerini doğrulamaları veya onay vermeleri için, açılan bir pencere yoluyla kullanıcıların Azure Active Directory v2 uç noktasıyla etkileşim kurmaya zorlanmasını gerektirir. Bazı örnekler:

- Kullanıcılar uygulamada ilk kez oturum açtığında
- Parolanın süresi dolduğundan kullanıcıların kimlik bilgilerini yeniden girmesi gerektiğinde
- Uygulamanız kullanıcının onaylaması gereken bir kaynağa erişim istediğinde
- İki faktörlü kimlik doğrulama gerektiğinde

```csharp
authResult = await App.PublicClientApp.AcquireTokenAsync(scopes);
```

> |Konumlar:||
> |---------|---------|
> |scopes | İstenen kapsamları (yani Microsoft Graph için `{ "user.read" }` veya özel Web API'leri için `{ "api://<Application ID>/access_as_user" }`) barındırır |

#### <a name="get-a-user-token-silently"></a>Kullanıcı belirtecini sessizce alma

Kullanıcının bir kaynağa her erişmesi gerektiğinde kimlik bilgilerini doğrulamasını istemiyorsunuz. Çoğunlukla, belirteç alımlarının ve yenilemesinin hiç kullanıcı etkileşimi olmadan yapılmasını istiyorsunuz. İlk `AcquireTokenAsync` sonrasında korumalı kaynaklara erişim için kullanılan belirteçleri alırken en yaygın kullanılan yöntem `AcquireTokenSilentAsync` yöntemidir:

```csharp
var accounts = await App.PublicClientApp.GetAccountsAsync();
authResult = await App.PublicClientApp.AcquireTokenSilentAsync(scopes, accounts.FirstOrDefault());
```

> |Konumlar: ||
> |---------|---------|
> |scopes | İstenen kapsamları (yani Microsoft Graph için `{ "user.read" }` veya özel Web API'leri için `{ "api://<Application ID>/access_as_user" }`) barındırır |
> |accounts.FirstOrDefault() | İlk kullanıcı önbelleğinde (MSAL destekleyen birden çok kullanıcı tek bir uygulama olarak) |

## <a name="next-steps"></a>Sonraki adımlar

Bu Hızlı Başlangıcın tam bir açıklamasının da içinde olduğu yeni özellikleri ve uygulamaları oluşturma hakkında eksiksiz adım adım kılavuz için Windows Masaüstü öğreticisini deneyin:

### <a name="learn-the-steps-to-create-the-application-used-in-this-quickstart"></a>Bu Hızlı Başlangıçta kullanılan uygulamayı oluşturma adımlarını öğrenin

> [!div class="nextstepaction"]
> [UWP - Graph API'si çağırma öğreticisi](tutorial-v2-windows-uwp.md)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]