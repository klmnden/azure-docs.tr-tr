---
title: Azure Active Directory v2.0 .NET yerel uygulama | Microsoft Docs
description: "Kullanıcıların hem kişisel Microsoft Account oturum ve iş veya Okul hesapları imzalar .NET yerel uygulama oluşturma."
services: active-directory
documentationcenter: 
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 46d81e09-bad0-44ce-9026-881805976e72
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/30/2016
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 7389f55ee6fef9548abb0ca4ac1bbd0399868d47
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="add-sign-in-to-a-windows-desktop-app"></a>Bir Windows masaüstü uygulamasına oturum açma ekleme
İle v2.0 uç hızla Masaüstü uygulamalarınızı hem kişisel Microsoft hesapları için destek ile kimlik doğrulaması ve iş veya Okul hesapları ekleyebilirsiniz.  Ayrıca, güvenli bir şekilde bir arka uç ile iletişim kurmak uygulamanızı sağlar web API'si, yanı [Microsoft Graph](https://graph.microsoft.io) ve bazılarını [Office 365 birleşik API'leri](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2).

> [!NOTE]
> Tüm Azure Active Directory (AD) senaryolarını ve özelliklerini v2.0 uç noktası tarafından desteklenir.  V2.0 uç kullanmanızın gerekli olup olmadığını belirlemek için okuyun [v2.0 sınırlamaları](active-directory-v2-limitations.md).
> 
> 

İçin [bir cihazda çalıştırma .NET yerel uygulamalar](active-directory-v2-flows.md#mobile-and-native-apps), Azure AD Microsoft Identity kimlik doğrulama kitaplığı veya MSAL sağlar.  Hayatta MSAL'ın tek amacı, web hizmetleri çağırmak için belirteçleri almak, uygulamanız için kolay hale getirmektir.  Ne kadar kolay olduğunu göstermek için burada size bir .NET WPF Yapılacaklar listesi uygulaması, yapı:

* Kullanıcı oturum açtığında & alır erişim belirteçleri kullanarak [OAuth 2.0 kimlik doğrulama protokolü](active-directory-v2-protocols.md).
* Arka uç OAuth 2.0 tarafından da güvenli Yapılacaklar listesi web hizmeti, güvenli bir şekilde çağırır.
* Kullanıcı oturumu kapattığında.

## <a name="download-sample-code"></a>Örnek kodu indirin
Bu öğretici için kod [GitHub'da](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet) korunur.  İzlemek için [uygulamanın çatısını bir .zip karşıdan](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/skeleton.zip) veya çatıyı kopyalayın:

    git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git

De Bu öğretici sonunda tamamlanmış uygulama sağlanır.

## <a name="register-an-app"></a>Bir uygulamayı kaydetme
En yeni bir uygulama oluşturma [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), veya adımları izleyin: [ayrıntılı adımları](active-directory-v2-app-registration.md).  Emin olun:

* Aşağı kopyalama **uygulama kimliği** uygulamanıza atanan, bunu en kısa sürede ihtiyacınız vardır.
* Ekleme **mobil** uygulamanız için platform.

## <a name="install--configure-msal"></a>Yükleme & MSAL yapılandırın
Microsoft ile kayıtlı bir uygulamaya sahip olduğunuza göre MSAL yükleyin ve kimlikle ilgili kodunuzu yazın.  V2.0 uç iletişim kurabilmesi MSAL uygulama kaydınızı hakkında bazı bilgiler ile sağlamak gerekir.

* Paket Yöneticisi konsolu kullanılarak TodoListClient projeye MSAL ekleyerek başlayın.

```
PM> Install-Package Microsoft.Identity.Client -ProjectName TodoListClient -IncludePrerelease
```

* TodoListClient projeyi açın `app.config`.  Öğeleri değerleri değiştirmek `<appSettings>` uygulama kayıt Portalı'na giriş değerleri yansıtacak şekilde bölümü.  MSAL kullandığında kodunuzu bu değerleri başvurur.
  
  * `ida:ClientId` Olan **uygulama kimliği** portaldan kopyaladığınız, uygulamanızın.
* TodoList hizmet projeyi açın `web.config` proje kökündeki.  
  
  * Değiştir `ida:Audience` aynı değerle **uygulama kimliği** portalından.

## <a name="use-msal-to-get-tokens"></a>Belirteçleri almak için MSAL kullanın
Temel MSAL arkasındaki bir erişim belirteci, uygulamanızı gereksinim duyduğunda, yalnızca çağrısı ilkesidir `app.AcquireToken(...)`, ve MSAL rest yapar.  

* İçinde `TodoListClient` proje, açık `MainWindow.xaml.cs` ve bulun `OnInitialized(...)` yöntemi.  İlk adım, uygulamanızın başlatmaktır `PublicClientApplication` -yerel uygulamalar temsil eden MSAL'ın birincil sınıfı.  Burada, Azure AD ile iletişim kurmasını ve onu nasıl belirteçleri önbelleğe söyleyin gereken koordinatları MSAL geçirmek budur.

```C#
protected override async void OnInitialized(EventArgs e)
{
        base.OnInitialized(e);

        app = new PublicClientApplication(new FileCache());
        AuthenticationResult result = null;
        ...
}
```

* Uygulama başlatıldığında denetleyin ve kullanıcı uygulamaya zaten imzalı değilse görmek istiyoruz.  Ancak, bir oturum açma kullanıcı Arabirimi henüz çağrılacak istemediğiniz - "işareti Bunu yapmak için" tıklatın kullanıcı hale getireceğiz.  Ayrıca, `OnInitialized(...)` yöntemi:

```C#
// As the app starts, we want to check to see if the user is already signed in.
// You can do so by trying to get a token from MSAL, using the method
// AcquireTokenSilent.  This forces MSAL to throw an exception if it cannot
// get a token for the user without showing a UI.
try
{
    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
    // If we got here, a valid token is in the cache - or MSAL was able to get a new oen via refresh token.
    // Proceed to fetch the user's tasks from the TodoListService via the GetTodoList() method.

    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "user_interaction_required")
    {
        // If user interaction is required, the app should take no action,
        // and simply show the user the sign in button.
    }
    else
    {
        // Here, we catch all other MsalExceptions
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }
}

```

* Kullanıcı oturum açmadı ve "Oturum Aç" düğmesini tıklatın, oturum açma kullanıcı Arabirimi çağırmak ve kimlik bilgilerini girin kullanıcının istiyoruz.  Oturum açma düğmesi işleyicisi uygulayın:

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // TODO: Sign the user out if they clicked the "Clear Cache" button

// If the user clicked the 'Sign-In' button, force
// MSAL to prompt the user for credentials by using
// AcquireTokenAsync, a method that is guaranteed to show a prompt to the user.
// MSAL will get a token for the TodoListService and cache it for you.

AuthenticationResult result = null;
try
{
    result = await app.AcquireTokenAsync(new string[] { clientId });
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    // If MSAL cannot get a token, it will throw an exception.
    // If the user canceled the login, it will result in the
    // error code 'authentication_canceled'.

    if (ex.ErrorCode == "authentication_canceled")
    {
        MessageBox.Show("Sign in was canceled by the user");
    }
    else
    {
        // An unexpected error occurred.
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }

        MessageBox.Show(message);
    }

    return;
}


}
```

* Kullanıcı başarıyla oturum açtığında, MSAL almak ve sizin için bir belirteç önbelleğe ve çağrı geçebilirsiniz `GetTodoList()` güvenle yöntemi.  Bir kullanıcının görevleri almak için sol şey uygulamak için `GetTodoList()` yöntemi.

```C#
private async void GetTodoList()
{

AuthenticationResult result = null;
try
{
    // Here, we try to get an access token to call the TodoListService
    // without invoking any UI prompt.  AcquireTokenSilentAsync forces
    // MSAL to throw an exception if it cannot get a token silently.


    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
}
catch (MsalException ex)
{
    // MSAL couldn't get a token silently, so show the user a message
    // and let them click the Sign-In button.

    if (ex.ErrorCode == "user_interaction_required")
    {
        MessageBox.Show("Please sign in first");
        SignInButton.Content = "Sign In";
    }
    else
    {
        // In any other case, an unexpected error occurred.

        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }

    return;
}

// Once the token has been returned by MSAL,
// add it to the http authorization header,
// before making the call to access the To Do list service.

httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);


        ...
...


- When the user is done managing their To-Do List, they may finally sign out of the app by clicking the "Clear Cache" button.

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // If the user clicked the 'clear cache' button,
        // clear the MSAL token cache and show the user as signed out.
        // It's also necessary to clear the cookies from the browser
        // control so the next user has a chance to sign in.

        if (SignInButton.Content.ToString() == "Clear Cache")
        {
                TodoList.ItemsSource = string.Empty;
                app.UserTokenCache.Clear(app.ClientId);
                ClearCookies();
                SignInButton.Content = "Sign In";
                return;
        }

        ...
```

## <a name="run"></a>Çalıştırın
Tebrikler! Artık kullanıcıların kimliğini doğrulamak ve güvenli bir şekilde Web OAuth 2.0 kullanan API'leri çağırmak için gönderebilen bir çalışma .NET WPF uygulaması sahipsiniz.  Her iki projelerinizi çalıştırın ve kişisel bir Microsoft hesabı veya bir iş veya Okul hesabınızla oturum açın.  Görevler kullanıcının yapılacaklar listesine ekleyin.  Oturumu kapatın ve başka bir kullanıcı olarak kendi yapılacaklar listesini görüntülemek için yeniden oturum açın.  Uygulamayı kapatın ve yeniden çalıştırın.  Nasıl kullanıcının oturumunu değişmeden kalır - uygulama bir yerel dosya belirteçleri önbelleğe alır, çünkü dikkat edin.

MSAL uygulamanıza, genel kimlik özellikleri içerecek şekilde hem kişisel hem de iş hesaplarını kullanarak kolaylaştırır.  Bu tüm dirty iş, - önbellek yönetimi, OAuth protokol desteği, kullanıcı bir oturum açma kullanıcı Arabirimi, belirteçlerin süresinin ve daha fazlasını yenileme sunmak için mvc'deki.  Gerçekten bilmeniz gereken tek şey tek bir API çağrısı `app.AcquireTokenAsync(...)`.

(Yapılandırma değerleriniz olmadan) tamamlanan örnek, başvuru için [burada bir .zip sağlanan](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip), veya Github'dan kopyalayabilirsiniz:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git```

## <a name="next-steps"></a>Sonraki adımlar
Şimdi daha gelişmiş konu başlıklarına geçebilirsiniz.  Denemek isteyebilirsiniz:

* [V2.0 uç noktası ile TodoListService Web API güvenliğini sağlama](active-directory-v2-devquickstarts-dotnet-api.md)

Ek kaynaklar için gözden geçirin:  

* [V2.0 Geliştirici Kılavuzu >>](active-directory-appmodel-v2-overview.md)
* [StackOverflow "msal" etiketi >>](http://stackoverflow.com/questions/tagged/msal)

## <a name="get-security-updates-for-our-products"></a>Ürünlerimiz için güvenlik güncelleştirmelerini alma
[Bu sayfayı](https://technet.microsoft.com/security/dd252948) ziyaret ederek ve Güvenlik Önerisi Uyarılarına abone olarak güvenlik olaylarının ne zaman ortaya çıkacağı hakkında bildirimleri almanızı öneririz.

