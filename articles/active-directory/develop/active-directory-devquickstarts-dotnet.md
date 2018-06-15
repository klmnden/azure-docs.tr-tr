---
title: Azure AD .NET Masaüstü (Başlarken WPF) | Microsoft Docs
description: Oturum açmak için Azure AD ile tümleşir ve Azure AD çağrıları .NET Windows Masaüstü uygulamasının nasıl oluşturulacağını OAuth kullanan API'ler korumalı.
services: active-directory
documentationcenter: .net
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: ed33574f-6fa3-402c-b030-fae76fba84e1
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 11/30/2017
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 1097d4b1f7d8c4feae500bf46b40e61029781d68
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34156271"
---
# <a name="azure-ad-net-desktop-wpf-getting-started"></a>Azure AD .NET Masaüstü (Başlarken WPF)
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Bir masaüstü uygulaması geliştiriyorsanız, Azure AD Basit ve kolay, kullanıcılarınız kendi Active Directory hesapları ile kimlik doğrulaması kolaylaştırır. Ayrıca, tüm web API Office 365 API'leri veya Azure API'sini gibi Azure AD tarafından korunan güvenli bir şekilde kullanmak uygulamanızı sağlar.

Korunan kaynaklara erişim için gereken .NET yerel istemciler için Azure AD Active Directory kimlik doğrulama kitaplığı veya ADAL sağlar. ADAL'ın tek amacı hayatta erişim belirteçleri almak uygulamanızı daha kolay hale getirmektir. Ne kadar kolay olduğunu göstermek için burada size bir .NET WPF Yapılacaklar listesi uygulaması, yapı:

* Alır erişim belirteçleri kullanarak Azure AD Graph API çağırma [OAuth 2.0 kimlik doğrulama protokolü](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Belirtilen diğer ada sahip kullanıcılar için bir dizin arar.
* İşaretlerini kullanıcıları.

Tam çalışan uygulama oluşturmak için ihtiyacınız:

1. Uygulamanızı Azure AD ile kaydedin.
2. Yükleme & ADAL yapılandırın.
3. ADAL, Azure AD'den belirteçleri almak için kullanın.

Başlamak için [uygulama çatıyı indirmeniz](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) veya [tamamlanan örnek indirme](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip). Ayrıca, kullanıcı oluşturma ve bir uygulamayı kaydetme Azure AD kiracısı gerekir. Bir kiracı yoksa [edinebileceğinizi öğrenin](active-directory-howto-tenant.md).

## <a name="1-register-the-directorysearcher-application"></a>1. DirectorySearcher uygulamayı Kaydet
Belirteçleri almak uygulamanızı etkinleştirmek için ilk Azure AD kiracınızda kaydetmek ve Azure AD grafik API'sine erişim izni vermek gerekir:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üst çubuğunda hesabınızda altında tıklatıp **Directory** listesinde, Active Directory Kiracı uygulamanızı kaydetmek için istediğiniz yeri seçin.
3. Tıklayın **tüm hizmetleri** sol taraftaki gezinti içinde ve **Azure Active Directory**.
4. Tıklayın **uygulama kayıtlar** ve **Ekle**.
5. Komut istemlerini izleyin ve yeni bir **yerel istemci uygulaması**.
  * **Adı** uygulamayı son kullanıcılar uygulamanıza anlatmaktadır
  * **Yeniden yönlendirme URI'si** Azure AD belirteci yanıtları döndürmek için kullanacağı düzeni ve dize bir birleşimidir. Uygulamanıza, örneğin belirli bir değer girin `http://DirectorySearcher`.
6. Kayıt tamamladıktan sonra AAD uygulamanızı benzersiz bir uygulama kimliği atar. Bu değer gerekir sonraki bölümlerde, bu nedenle uygulama sayfasından kopyalayın.
7. Gelen **ayarları** sayfasında, **gerekli izinler** ve **Ekle**. Seçin **Microsoft Graph** API olarak ve ekleme **dizin verilerini okuma** altında izni **izinlere temsilci**. Bu kullanıcılar için grafik API'si sorgulamak için uygulamanıza olanak sağlar.

## <a name="2-install--configure-adal"></a>2. Yükleme & ADAL yapılandırın
Azure AD'de bir uygulamanız varsa, ADAL yükleyin ve kimlikle ilgili kodunuzu yazın. Azure AD ile iletişim kurabilmesi adal sırada uygulama kaydınızı hakkında bazı bilgiler ile sağlamanız gerekir.

* Paket Yöneticisi konsolu kullanılarak DirectorySearcher projeye ADAL ekleyerek başlayın.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

* DirectorySearcher projeyi açın `app.config`. Öğeleri değerleri değiştirmek `<appSettings>` Azure Portalı'na giriş değerleri yansıtacak şekilde bölümü. ADAL kullandığında kodunuzu bu değerleri başvurur.
  * `ida:Tenant` Azure AD kiracınız, ör. contoso.onmicrosoft.com etki alanıdır
  * `ida:ClientId` Portaldan kopyaladığınız uygulamanızın istemci kimliği değil.
  * `ida:RedirectUri` Yeniden yönlendirme olan Portalı'nda kayıtlı url.

## <a name="3-use-adal-to-get-tokens-from-aad"></a>3. AAD belirteçleri almak için ADAL'ı kullanın
Temel ADAL arkasında uygulamanızı bir erişim belirteci her gerektiğinde, onu yalnızca çağırdığı ilkesidir `authContext.AcquireTokenAsync(...)`, ve ADAL rest yapar. 

* İçinde `DirectorySearcher` proje, açık `MainWindow.xaml.cs` ve bulun `MainWindow()` yöntemi. İlk adım, uygulamanızın başlatmaktır `AuthenticationContext` -birincil sınıfı ADAL. ADAL burada geçirdiğiniz budur gereken Azure AD ile iletişim kurmasını ve onu nasıl belirteçleri önbelleğe söyleyin koordinatları.

```csharp
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());

    CheckForCachedToken();
}
```

* Şimdi bulun `Search(...)` kullanıcının uygulamanın kullanıcı arabiriminde "Ara" düğmesini tıklattığında çağrılacak yöntem. Bu yöntem, UPN verilen arama terimiyle kullanıcıları için Azure AD Graph API sorgu için bir GET isteği yapar. Ancak bir access_token ile dahil etmeniz grafik API'si sorgulamak için `Authorization` üstbilgi ve istek - ADAL nereden geldiğini olan budur.

```csharp
private async void Search(object sender, RoutedEventArgs e)
{
    // Validate the Input String
    if (string.IsNullOrEmpty(SearchText.Text))
    {
        MessageBox.Show("Please enter a value for the To Do item name");
        return;
    }

    // Get an Access Token for the Graph API
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
        UserNameLabel.Content = result.UserInfo.DisplayableId;
        SignOutButton.Visibility = Visibility.Visible;
    }
    catch (AdalException ex)
    {
        // An unexpected error occurred, or user canceled the sign in.
        if (ex.ErrorCode != "access_denied")
            MessageBox.Show(ex.Message);

        return;
    }

    ...
}
```
* Uygulamanızı isteğinde bulunduğunda bir belirteç çağırarak `AcquireTokenAsync(...)`, ADAL kullanıcı kimlik bilgilerinin sorulmasına olmadan bir simge döndürür deneyecek. ADAL kullanıcı bir belirteç almak üzere oturum açmak gerektiğini belirlerse, bir oturum açma iletişim kutusu görüntüler, kullanıcının kimlik bilgilerini toplamak ve başarılı bir kimlik doğrulaması sırasında bir simge döndürür. ADAL herhangi bir nedenle bir belirteç döndüremedi ise, throw bir `AdalException`.
* Dikkat `AuthenticationResult` nesnesini içeren bir `UserInfo` uygulamanız gerekebilir bilgi toplamak için kullanılan nesne. DirectorySearcher içinde `UserInfo` kullanıcının kimliği uygulamanın kullanıcı arabirimini özelleştirmek için kullanılır.
* Kullanıcı "Oturumu Kapat" düğmesini tıklattığında sonraki çağrısı emin olmak istiyoruz `AcquireTokenAsync(...)` oturum açmak için kullanıcıdan ister. ADAL ile bu belirteç önbelleği temizleme olarak kadar kolaydır:

```csharp
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear the token cache
    authContext.TokenCache.Clear();

    ...
}
```

* Ancak, kullanıcı "Oturumu Kapat" düğmesini tıklatın değil, gelecek sefer DirectorySearcher çalıştırdığınızda için kullanıcının oturumu korumak isteyeceksiniz. Uygulama başlattığında, varolan bir belirteci için ADAL'ın belirteç önbelleği denetleyin ve kullanıcı Arabirimi gerektiği gibi güncelleştirin. İçinde `CheckForCachedToken()` yöntemi, başka bir çağırmaya `AcquireTokenAsync(...)`, bu süre içinde geçirme `PromptBehavior.Never` parametresi. `PromptBehavior.Never` ADAL, kullanıcı oturum açma için sorulması değil ve bir belirteç döndüremedi ise ADAL bunun yerine bir özel durum söyler.

```csharp
public async void CheckForCachedToken() 
{
    // As the application starts, try to get an access token without prompting the user. If one exists, show the user as signed in.
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Never));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "user_interaction_required")
        {
            // An unexpected error occurred.
            MessageBox.Show(ex.Message);
        }

        // If user interaction is required, proceed to main page without singing the user in.
        return;
    }

    // A valid token is in the cache
    SignOutButton.Visibility = Visibility.Visible;
    UserNameLabel.Content = result.UserInfo.DisplayableId;
}
```

Tebrikler! Artık çalışma kullanıcıların kimliğini doğrulamak için güvenli bir şekilde Web OAuth 2.0 kullanan API'leri çağırmak gönderebilen .NET WPF uygulaması sahip ve kullanıcı hakkındaki temel bilgileri alın. Henüz yapmadıysanız, bazı kullanıcılar ile Kiracı doldurmak için zaman sunulmuştur. DirectorySearcher uygulamanızı çalıştırın ve bu kullanıcıların biri oturum oturum. Diğer kullanıcıların UPN Değerlerinin üzerinde göre arayın. Uygulamayı kapatın ve yeniden çalıştırın. Kullanıcının oturumunu nasıl olduğu gibi kalır dikkat edin. Oturumu kapatın ve başka bir kullanıcı olarak yeniden oturum açın.

ADAL, bu ortak kimlik özelliklerin tümü, uygulamanıza eklemenizi kolaylaştırır. Bu tüm dirty iş, - önbellek yönetimi, OAuth protokol desteği, kullanıcı bir oturum açma kullanıcı Arabirimi, belirteçlerin süresinin ve daha fazlasını yenileme sunmak için mvc'deki. Gerçekten bilmeniz gereken tek şey tek bir API çağrısı `authContext.AcquireTokenAsync(...)`.

Başvuru için tamamlanan örnek (yapılandırma değerleriniz olmadan) sağlanır [burada](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip). Şimdi ilave senaryolar da taşıyabilirsiniz. Denemek isteyebilirsiniz:

[.NET Web API'si Azure AD ile güvenli >>](active-directory-devquickstarts-webapi-dotnet.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

