---
title: Microsoft kimlik platformu Windows UWP hızlı başlangıç | Azure
description: Nasıl bir evrensel Windows Platformu (XAML) uygulama erişim belirteci almak ve Microsoft kimlik platformu uç noktası tarafından korunan bir API'yi çağırabilen öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/12/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: d9d2e9aa5e5e805b302763f5417110cdd078eb3b
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59997606"
---
# <a name="quickstart-call-the-microsoft-graph-api-from-a-universal-windows-platform-uwp-application"></a>Hızlı Başlangıç: Evrensel Windows Platformu (UWP) uygulamasından Microsoft Graph API'sini çağırma

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

Bu hızlı başlangıçta nasıl bir evrensel Windows Platformu (UWP) uygulaması, kullanıcıların kişisel hesaplarıyla oturum açma veya iş ve Okul hesapları, bir erişim belirteci alma ve Microsoft Graph API'sini çağırmak gösteren kod örneği içerir.

![Bu Hızlı Başlangıç ile oluşturulan örnek uygulamasını nasıl çalıştığını gösterir](media/quickstart-v2-uwp/uwp-intro.svg)

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme
> [!div renderon="docs" class="sxs-lookup"]
> Hızlı başlangıç uygulamanızı başlatmak için kullanabileceğiniz iki seçenek vardır:
> * [Express] [Seçenek 1: Kaydet ve otomatik Uygulamanızı yapılandırmak ve ardından, kod örneğini indirin](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [El ile] [Seçeneği 2: Kaydetme ve uygulama ve kod örneğinizi el ile yapılandırma](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>1. seçenek: Kaydet ve otomatik Uygulamanızı yapılandırmak ve ardından, kod örneğini indirin
>
> 1. Yeni Git [Azure Portalı - Uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/UwpQuickstartPage/sourceType/docs) bölmesi.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'e tıklayın.
> 1. Yönergeleri izleyerek yeni uygulamanızı tek tıkla indirin ve otomatik olarak yapılandırın.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2. seçenek: Kaydetme ve uygulama ve kod örneğinizi el ile yapılandırma
> [!div renderon="docs"]
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
> Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze eklemek için şu adımları izleyin:
> 1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
> 1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
> 1. Geliştiriciler için Microsoft identity platformuna gidin [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfası.
> 1. Seçin **yeni kayıt**.
> 1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:
>      - **Ad** alanına uygulama kullanıcılarına gösterilecek anlamlı bir uygulama adı girin, örneğin `UWP-App-calling-MsGraph`.
>      - **Desteklenen hesap türleri** bölümünde **Herhangi bir kuruluş dizinindeki hesaplar ve kişisel Microsoft hesapları (ör. Skype, Xbox, Outlook.com)** seçeneğini belirtin.
>      - Uygulamayı kaydetmek için **Kaydet**'i seçin.
> 1. Uygulama sayfa listesinde **Kimlik doğrulaması**'nı seçin.
> 1. **Yeniden yönlendirme URL'leri** bölümünde **Ortak istemciler (mobil, masaüstü) için önerilen Yeniden Yönlendirme URI'leri** altında **"urn:ietf:wg:oauth:2.0:oob** girişini seçin.
> 1. **Kaydet**’i seçin.

> [!div renderon="portal" class="sxs-lookup alert alert-info"]
> #### <a name="step-1-configure-your-application"></a>1. Adım: Uygulamanızı yapılandırma
> Bu hızlı başlangıç kod örneğinin çalışması için **urn:ietf:wg:oauth:2.0:oob** gibi bir yeniden yönlendirme URI’si eklemeniz gerekir.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişikliği benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-uwp/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış.

#### <a name="step-2-download-your-visual-studio-project"></a>2. Adım: Visual Studio projenizi indirin

 - [Visual Studio projesini indirin](https://github.com/Azure-Samples/active-directory-dotnet-native-uwp-v2/archive/msal3x.zip)

#### <a name="step-3-configure-your-visual-studio-project"></a>3. Adım: Visual Studio projenizi yapılandırın

1. Zip dosyasını diskin köküne yakın bir yerel klasöre (örneğin **C:\Azure-Samples**) ayıklayın.
1. Projeyi Visual Studio'da açın. Bir UWP SDK'yı yüklemeyi istenebilir. Bu durumda, kabul edin.
1. Düzen **MainPage.Xaml.cs** ve değerlerini değiştirin `ClientId` alan:

    ```csharp
    private const string ClientId = "Enter_the_Application_Id_here";
    ```

> [!div renderon="docs"]
> Konumlar:
> - `Enter_the_Application_Id_here` - Kaydettiğiniz uygulamanın Uygulama Kimliği değeridir.
>
> > [!TIP]
> > Değerini bulmak için *uygulama kimliği*Git **genel bakış** portalı bölümünde

#### <a name="step-4-run-your-application"></a>4. Adım: Uygulamanızı çalıştırma

Bu hızlı başlangıçta, Windows makinenizde denemek istiyorsanız:

1. Visual Studio araç çubuğunda, doğru platformu seçin (muhtemelen **x64** veya **x86**, ARM değil).
   > Hedef cihaz gelen değişiklikleri gözlemleyin *cihaz* için *yerel makine*
1. hata ayıklama seçin | **Hata ayıklama olmadan Başlat**

## <a name="more-information"></a>Daha fazla bilgi

Bu bölümde hızlı başlangıç hakkında daha fazla bilgi verilmektedir.

### <a name="msalnet"></a>MSAL.NET

MSAL ([Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)) kullanıcılarının oturumunu ve güvenlik belirteci istemek için kullanılan bir kitaplık sunulmaktadır. Güvenlik belirteçleri, geliştiriciler için Microsoft Identity platformu tarafından korunan bir API'ye erişmek için kullanılır. MSAL kitaplığını Visual Studio'nun *Paket Yöneticisi Konsolu*'nda aşağıdaki komutu çalıştırarak yükleyebilirsiniz:

```powershell
Install-Package Microsoft.Identity.Client -IncludePrerelease
```

### <a name="msal-initialization"></a>MSAL başlatma

Şu kodu ekleyerek MSAL başvurusunu ekleyebilirsiniz:

```csharp
using Microsoft.Identity.Client;
```

Ardından, aşağıdaki kodu kullanarak MSAL başlatılır:

```csharp
public static IPublicClientApplication PublicClientApp;
PublicClientApp = new PublicClientApplicationBuilder.Create(ClientId)
                                                    .Build();
```

> |Konumlar: ||
> |---------|---------|
> | `ClientId` | **Uygulama (istemci) Kimliği**, Azure portalda kayıtlı uygulamadır. Bu değeri Azure portalda uygulamanın **Genel bakış** sayfasında bulabilirsiniz. |

### <a name="requesting-tokens"></a>Belirteç isteme

MSAL bir UWP uygulamasında belirteçlerini almak için iki yöntem vardır: `AcquireTokenInteractive` ve `AcquireTokenSilent`.

#### <a name="get-a-user-token-interactively"></a>Etkileşimli olarak kullanıcı belirteci alma

Bazı durumlarda, kullanıcıların Microsoft kimlik platformu uç noktası aracılığıyla izin verin ya da ya da kendi kimlik bilgilerini doğrulamak için bir açılan pencere ile etkileşim kurmak için zorlama gerektirir. Bazı örnekler:

- İlk kez kullanıcıların uygulamada oturum
- Parolanın süresi dolduğundan kullanıcıların kimlik bilgilerini yeniden girmesi gerektiğinde
- Uygulamanızın kullanıcı onay gerektiren bir kaynağa erişim ne zaman isteme
- İki faktörlü kimlik doğrulama gerektiğinde

```csharp
authResult = await App.PublicClientApp.AcquireTokenInteractive(scopes)
                      .ExecuteAsync();
```

> |Konumlar:||
> |---------|---------|
> | `scopes` | İstenen kapsamları (Microsoft Graph için `{ "user.read" }` veya özel Web API'leri için `{ "api://<Application ID>/access_as_user" }` gibi) barındırır. |

#### <a name="get-a-user-token-silently"></a>Kullanıcı belirtecini sessizce alma

Kullanım `AcquireTokenSilent` ilk korunan kaynaklara erişim belirteçleri elde etmek için yöntemi `AcquireTokenAsync` yöntemi. Bir kaynağa erişmek ihtiyaç duydukları her zaman kendi kimlik doğrulama isteyecek şekilde istemezsiniz. Belirteç edinme ve herhangi bir kullanıcı etkileşimi olmadan yenileme çoğu zaman, istediğiniz

```csharp
var accounts = await App.PublicClientApp.GetAccountsAsync();
var firstAccount = accounts.FirstOrDefault();
authResult = await App.PublicClientApp.AcquireTokenSilent(scopes, firstAccount)
                                      .ExecuteAsync();
```

> |Konumlar: ||
> |---------|---------|
> | `scopes` | İstenen kapsamları (Microsoft Graph için `{ "user.read" }` veya özel Web API'leri için `{ "api://<Application ID>/access_as_user" }` gibi) barındırır |
> | `firstAccount` | (MSAL birden çok kullanıcı tek bir uygulamada destekler) önbelleğinde ilk kullanıcı hesabı belirtir. |

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıcın tam bir açıklamasının da içinde olduğu yeni özellikleri ve uygulamaları oluşturma hakkında eksiksiz adım adım kılavuz için Windows masaüstü öğreticisini deneyin.

> [!div class="nextstepaction"]
> [UWP - Graph API'si çağırma öğreticisi](tutorial-v2-windows-uwp.md)
