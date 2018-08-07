---
title: Azure AD .NET Masaüstü (başlama WPF) | Microsoft Docs
description: Oturum açma için Azure AD ile tümleştirilir ve Azure AD çağıran bir .NET Windows masaüstü uygulaması oluşturmayı öğrenin, OAuth kullanarak API'leri korumalı.
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
ms.openlocfilehash: c4bc777c36608ff9aeeec3428af0103fa3ae29c4
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39578971"
---
# <a name="azure-ad-net-desktop-wpf-getting-started"></a>Azure AD .NET Masaüstü (başlama WPF)
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Bir masaüstü uygulaması geliştiriyorsanız, Azure AD, kullanıcılarınızın kimliğini Active Directory hesaplarını ile yapabileceğiniz sade ve basit kılar. Ayrıca güvenli bir şekilde herhangi bir web API, Office 365 API'lerini veya Azure API gibi Azure AD tarafından korunan tüketebilmesini sağlar.

Korunan kaynaklara erişim gerektiren .NET yerel istemciler için Azure AD Active Directory Authentication Library veya ADAL sağlar. ADAL'ın tek amacı hayatta erişim belirteçlerini almak uygulamanızı kolaylaştırır sağlamaktır. Ne kadar kolay olduğunu göstermek için buraya bir .NET WPF Yapılacaklar listesi uygulaması, oluşturacağız:

* Alır erişim belirteçlerini kullanarak Azure AD Graph API'sini çağırmak için [OAuth 2.0 kimlik doğrulama protokolü](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Belirli bir takma ad ile kullanıcılar için bir dizini arar.
* Kullanıcıların imzalar.

Tam çalışma uygulamayı yapılandırmak için yapmanız gerekir:

1. Uygulamanızı Azure AD'ye kaydedin.
2. Yükleme ve ADAL'ı yapılandırın.
3. Azure AD belirteçlerini almak için ADAL'ı kullanın.

Başlamak için [uygulama çatıyı indirmeniz](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) veya [tamamlanan örnek indirme](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip). Ayrıca, kullanıcılar oluşturur ve bir uygulamayı kaydetme Azure AD kiracısı gerekir. Bir kiracı yoksa [edinebileceğinizi öğrenin](quickstart-create-new-tenant.md).

## <a name="1-register-the-directorysearcher-application"></a>1. DirectorySearcher uygulamayı kaydetme
Belirteçlerini almak uygulamanızı etkinleştirmek için ilk Azure AD kiracınıza kaydetme ve Azure AD Graph API'sine erişim izni vermek gerekir:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üst çubuğunda, hesabınızda ve altında tıklayın **dizin** listesinde, uygulamanızı kaydetmek istediğiniz Active Directory kiracısı seçin.
3. Tıklayarak **tüm hizmetleri** , sol taraftaki gezinti ve **Azure Active Directory**.
4. Tıklayarak **uygulama kayıtları** ve **Ekle**.
5. Komut istemlerini izleyin ve yeni bir **yerel istemci uygulaması**.
  * **Adı** uygulamanız son kullanıcılara uygulamayı açıklanmıştır
  * **Yeniden yönlendirme URI'si** Azure AD'nin belirteç yanıtlarını döndürmek için kullanacağı bir şema ve dize birleşiminden oluşur. Uygulamanıza özgü bir değer girin, örneğin `http://DirectorySearcher`.
6. Kayıt tamamlandıktan sonra AAD uygulamanızın benzersiz bir uygulama kimliği atar. Bu değer gerekir sonraki bölümlerde, bu nedenle uygulama sayfasından kopyalayın.
7. Gelen **ayarları** sayfasında **gerekli izinler** ve **Ekle**. Seçin **Microsoft Graph** API olarak ve ekleme **dizin verilerini okuma** altında izin **Temsilcili izinler**. Bu, kullanıcılar için Graph API sorgu uygulamanıza olanak sağlar.

## <a name="2-install--configure-adal"></a>2. Yükleme ve ADAL'ı yapılandırma
Azure AD'de bir uygulamanız olduğuna göre ADAL'ı yükleyebilir ve kimlikle ilgili kodunuzu yazın. Azure AD ile iletişim kurabilmesi ADAL uygulama kaydınızı hakkında bazı bilgiler vermeniz gerekir.

* Paket Yöneticisi konsolu kullanarak DirectorySearcher projeye ADAL ekleyerek başlayın.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

* DirectorySearcher projeyi `app.config`. Öğe değerlerini değiştirin `<appSettings>` bölümü Azure Portalı'na giriş değerleri yansıtır. ADAL kullandığında, kodunuzun bu değerleri başvurur.
  * `ida:Tenant` Örn. contoso.onmicrosoft.com, Azure AD kiracınızın etki alanı
  * `ida:ClientId` Portaldan kopyaladığınız uygulamanızın istemci kimliğidir.
  * `ida:RedirectUri` Olan yeniden yönlendirme URL'si Portalı'nda kayıtlı.

## <a name="3-use-adal-to-get-tokens-from-aad"></a>3. AAD'den belirteçlerini almak için ADAL'ı kullanın
Uygulamanızı bir erişim belirteci gerektiğinde, yalnızca çağırır, ADAL ardındaki temel buradaki prensip şudur: `authContext.AcquireTokenAsync(...)`, ve ADAL geri kalanını yapar. 

* İçinde `DirectorySearcher` projesini açarsanız `MainWindow.xaml.cs` bulun `MainWindow()` yöntemi. İlk adım, uygulamanızın başlatma olarak `AuthenticationContext` -birincil sınıf ADAL. ADAL burada geçirdiğiniz budur Azure AD ile iletişim kurmak ve belirteçleri önbelleğe alınacağını bildirmek için ihtiyaç duyduğu koordinatlar.

```csharp
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());

    CheckForCachedToken();
}
```

* Artık bulun `Search(...)` kullanıcı uygulamanın kullanıcı arabiriminde "Ara" düğmesine tıkladığında çağrılacak yöntem. Bu yöntem, UPN verilen arama terimiyle başlayan kullanıcıları için Azure AD Graph API için sorgu için bir GET isteği yapar. Graph API'sini sorgulamak üzere bir access_token eklemeniz gerekir, ancak `Authorization` üst bilgi isteği - bu ADAL burada devreye girer.

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
* Uygulamanız istediğinde bir belirteç çağırarak `AcquireTokenAsync(...)`, ADAL kullanıcı kimlik bilgilerinin sorulmasına gerek kalmadan bir belirteç döndürecek şekilde çalışır. ADAL bir belirteç almak üzere oturum açmak kullanıcının gerektiğini belirlerse, bir oturum açma iletişim kutusu görüntüler, kullanıcının kimlik bilgilerini toplamak ve başarılı kimlik doğrulamadan sonra bir belirteç döndürecek. ADAL belirteç herhangi bir nedenle döndüremedi ise, throw bir `AdalException`.
* Dikkat `AuthenticationResult` nesne içeren bir `UserInfo` uygulamanız gerekebilir bilgilerini toplamak için kullanılan nesne. İçinde DirectorySearcher `UserInfo` kullanıcının kimliği ile uygulamanın kullanıcı arabirimini özelleştirmek için kullanılır.
* Kullanıcı "Oturumu Kapat" düğmesini tıkladığında, sonraki çağrı emin olmak istiyoruz `AcquireTokenAsync(...)` kullanıcının oturum açmasını ister. ADAL ile bu belirteç önbelleği temizleme olarak oldukça kolaydır:

```csharp
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear the token cache
    authContext.TokenCache.Clear();

    ...
}
```

* Ancak, kullanıcı, "Oturumu Kapat" düğmesini tıklatın değil, kullanıcının oturumu bir sonraki kez DirectorySearcher çalıştırdığı korumak isteyeceksiniz. Uygulamayı başlattığında, ADAL'ın mevcut bir belirteç için belirteç önbelleği denetleyin ve kullanıcı Arabirimi gerektiği gibi güncelleştirin. İçinde `CheckForCachedToken()` yöntemi, başka bir çağrı yapmak `AcquireTokenAsync(...)`, bu süre içinde geçirme `PromptBehavior.Never` parametresi. `PromptBehavior.Never` ADAL, kullanıcı oturum açma için sizden ve ADAL belirteç döndüremedi ise bunun yerine özel bir durum oluşturmamalıdır bildirir.

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

Tebrikler! Artık sahip çalışan bir .NET WPF uygulaması, kullanıcıların kimlik doğrulaması, OAuth 2.0 kullanarak Web API'leri güvenli bir şekilde çağırma özelliğine sahiptir ve kullanıcı ile ilgili temel bilgileri alın. Henüz yapmadıysanız, kiracınızın bazı kullanıcılar ile doldurmak için zaman sunulmuştur. DirectorySearcher uygulamanızı çalıştırın ve oturum kullanıcılarla birini açın. Kendi UPN'e bağlı diğer kullanıcılar için arama yapın. Uygulamayı kapatın ve tekrar çalıştırın. Kullanıcının oturumunu nasıl değişmeden kalır dikkat edin. Oturumu kapatın ve başka bir kullanıcı olarak yeniden oturum açın.

ADAL, bu ortak kimlik özelliklerin tümü, uygulamanıza eklemenize kolaylaştırır. Bu tüm kirli iş, - önbellek yönetimi, OAuth protokol desteği, kullanıcı bir oturum açma kullanıcı Arabirimi, süresi dolan ve daha fazlasını yenileme sunmak için üstlenir. Gerçekten bilmeniz gereken her şey tek bir API çağrısı `authContext.AcquireTokenAsync(...)`.

Başvuru için tamamlanmış bir örnek (olmadan yapılandırma değerlerinize) sağlanan [burada](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip). Artık için ilave senaryolar da taşıyabilirsiniz. Denemek isteyebilirsiniz:

[.NET Web API'si Azure AD ile güvenli >>](quickstart-v1-dotnet-webapi.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

