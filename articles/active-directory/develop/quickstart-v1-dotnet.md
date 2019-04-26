---
title: Kullanıcılar oturum ve bir .NET Masaüstü (WPF) uygulaması Microsoft Graph API'sini çağırmak | Microsoft Docs
description: Oturum açma için Azure AD ile tümleşen bir .NET Windows masaüstü uygulaması oluşturmayı öğrenin ve OAuth 2.0 kullanarak API çağrıları Azure AD'ye korumalı.
services: active-directory
documentationcenter: .net
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: ed33574f-6fa3-402c-b030-fae76fba84e1
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2b55f7e615f2c2edb604d5b9433db6cc48d9f36f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60298987"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-a-net-desktop-wpf-app"></a>Hızlı Başlangıç: Kullanıcılar oturum ve bir .NET Masaüstü (WPF) uygulamasından Microsoft Graph API çağırma

[!INCLUDE [active-directory-develop-applies-v1-adal](../../../includes/active-directory-develop-applies-v1-adal.md)]

Korunan kaynaklara erişim gerektiren .NET yerel istemciler için Azure Active Directory (Azure AD), Active Directory Authentication Library (ADAL) sağlar. ADAL erişim belirteçlerini almak uygulamanızı kolaylaştırır. 

Bu hızlı başlangıçta, bir .NET WPF Yapılacaklar listesi uygulamasının nasıl oluşturulacağını öğreneceksiniz. Bu:

* OAuth 2.0 kimlik doğrulama protokollerini kullanarak Azure AD Graph API'sini çağırmak için belirteçlerini alır erişin.
* Belirli bir takma ad ile kullanıcılar için bir dizini arar.
* Kullanıcıların imzalar.

Eksiksiz, çalışan bir uygulama oluşturmak için şunları yapmalısınız:

1. Uygulamanızı Azure AD'ye kaydedin.
2. ADAL'ı yükleyin ve yapılandırın.
3. ADAL'ı kullanarak Azure AD'den belirteçleri alın.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için şu önkoşulları tamamlayın:

* [Uygulama çatıyı indirmeniz](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) veya [tamamlanmış örneği indirin](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip)
* Burada kullanıcıları oluşturun ve bir uygulamayı kaydetme Azure AD kiracısı vardır. Henüz bir kiracınız yoksa [nasıl kiracı alınabileceğini öğrenin](quickstart-create-new-tenant.md).

## <a name="step-1-register-the-directorysearcher-application"></a>1. Adım: DirectorySearcher uygulamayı kaydetme

Belirteçleri almak üzere uygulamanızı etkinleştirme için uygulamanızı Azure AD kiracınıza kaydetme ve Azure AD Graph API'sine erişim izni verin:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üst çubuğunda, hesabınızı seçin ve altındaki **dizin** listesinde, uygulamanızı kaydetmek istediğiniz Active Directory kiracısı seçin.
3. SELECT deyiminde **tüm hizmetleri** , sol taraftaki gezinti ve **Azure Active Directory**.
4. Üzerinde **uygulama kayıtları**, seçin **Ekle**.
5. Komut istemlerini izleyin ve yeni bir **yerel** istemci uygulaması.
    * **Adı** uygulamanız son kullanıcılara uygulamayı açıklanmıştır
    * **Yeniden yönlendirme URI'si** Azure AD'nin belirteç yanıtlarını döndürmek için kullanacağı bir şema ve dize birleşiminden oluşur. Uygulamanıza özgü bir değer girin, örneğin, `http://DirectorySearcher`.

6. Kayıt tamamlandıktan sonra AAD uygulamanızın benzersiz bir uygulama kimliği atar. Bu değer gerekir sonraki bölümlerde, bu nedenle uygulama sayfasından kopyalayın.
7. Gelen **ayarları** sayfasında **gerekli izinler** ve **Ekle**. Seçin **Microsoft Graph** API olarak ve altında **temsilci izinleri** ekleme **dizin verilerini okuma** izni. Bu izni, uygulamanızın kullanıcıları için Graph API sorgu olanak tanır.

## <a name="step-2-install-and-configure-adal"></a>2. Adım: Yükleme ve ADAL'ı yapılandırma

Artık Azure AD'de bir uygulamanız olduğuna göre, ADAL'ı yükleyebilir ve kimlikle ilgili kodunuzu yazabilirsiniz. ADAL'ın Azure AD ile iletişim kurabilmesi için, ona uygulama kaydınız hakkında bazı bilgiler sağlamanız gerekir.

1. Başlangıç için ADAL ekleyerek `DirectorySearcher` Paket Yöneticisi konsolu kullanarak proje.

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

1. İçinde `DirectorySearcher` projesini açarsanız `app.config`.
1. Öğe değerlerini değiştirin `<appSettings>` Azure portalında giriş değerleri yansıtacak şekilde bölümü. Kodunuz ADAL'ı her kullandığında bu değerlere başvurur.
   * `ida:Tenant` Contoso.onmicrosoft.com gibi Azure AD kiracınızın etki alanı
   * `ida:ClientId` Portaldan kopyaladığınız uygulamanızın istemci kimliği.
   * `ida:RedirectUri` Portalı'nda kayıtlı yeniden yönlendirme URL'si.

## <a name="step-3-use-adal-to-get-tokens-from-azure-ad"></a>3. Adım: Azure AD belirteçlerini almak için ADAL'ı kullanın

Uygulamanızı bir erişim belirteci gerektiğinde uygulamanızı yalnızca çağrı yaptığını ADAL ardındaki temel buradaki prensip şudur: `authContext.AcquireTokenAsync(...)`, ve ADAL geri kalanını yapar.

1. İçinde `DirectorySearcher` projesini açarsanız `MainWindow.xaml.cs`.
1. Bulun `MainWindow()` yöntemi. 
1. Uygulamanızın başlatmak `AuthenticationContext` -birincil sınıf ADAL. `AuthenticationContext` ADAL geçirdiğiniz nerede olduğunu Azure AD ile iletişim kurmak ve belirteçleri önbelleğe alınacağını bildirmek için ihtiyaç duyduğu koordinatlar.

    ```csharp
    public MainWindow()
    {
        InitializeComponent();

        authContext = new AuthenticationContext(authority, new FileCache());

        CheckForCachedToken();
    }
    ```

1. Bulun `Search(...)` kullanıcı seçtiğinde çağrılacak yöntem **arama** uygulamanın kullanıcı arabiriminde düğmesi. Bu yöntemde, UPN'leri verilen arama terimiyle başlayan kullanıcıları sorgulamak için Azure AD Graph API'sinden bir GET isteğinde bulunulur.
1. Graph API'sini sorgulamak için bir access_token dahil `Authorization` istek üstbilgisinin ADAL noktada devreye olduğu.

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

    Uygulamanız istediğinde bir belirteç çağırarak `AcquireTokenAsync(...)`, ADAL kullanıcı kimlik bilgilerinin sorulmasına gerek kalmadan bir belirteç döndürecek şekilde çalışır.
    * ADAL bir belirteç almak üzere oturum açmak kullanıcının gerektiğini belirlerse, bir oturum açma iletişim kutusu görüntüler, kullanıcının kimlik bilgilerini toplamak ve başarılı kimlik doğrulamadan sonra bir belirteç döndürecek. 
    * ADAL belirteç herhangi bir nedenle döndüremedi ise, throw bir `AdalException`.

1. Dikkat `AuthenticationResult` nesne içeren bir `UserInfo` uygulamanız gerekebilir bilgilerini toplamak için kullanılan nesne. İçinde DirectorySearcher `UserInfo` kullanıcının kimliği ile uygulamanın kullanıcı arabirimini özelleştirmek için kullanılır
1. Kullanıcı seçtiğinde **oturumunuzu** düğmesi, sonraki için çağrı emin `AcquireTokenAsync(...)` kullanıcının oturum açmasını ister. Kolayca ile ADAL belirteç önbelleğini kaldırarak bunu yapabilirsiniz:

    ```csharp
    private void SignOut(object sender = null, RoutedEventArgs args = null)
    {
        // Clear the token cache
        authContext.TokenCache.Clear();

        ...
    }
    ```

    Kullanıcı olmayan tıklarsanız **oturumunuzu** düğmesi, kullanıcının oturumu bir sonraki kez DirectorySearcher çalıştırdığı korumak için ihtiyacınız. Uygulamayı başlattığında, ADAL'ın mevcut bir belirteç için belirteç önbelleği denetleyin ve kullanıcı Arabirimi gerektiği gibi güncelleştirin.

1. İçinde `CheckForCachedToken()` yöntemi, başka bir çağrı yapmak `AcquireTokenAsync(...)`, bu süre içinde geçirme `PromptBehavior.Never` parametresi. `PromptBehavior.Never` ADAL, kullanıcı oturum açma için sizden ve ADAL belirteç döndüremedi ise bunun yerine özel bir durum oluşturmamalıdır bildirir.

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

            // If user interaction is required, proceed to main page without signing the user in.
            return;
        }

        // A valid token is in the cache
        SignOutButton.Visibility = Visibility.Visible;
        UserNameLabel.Content = result.UserInfo.DisplayableId;
    }
    ```

Tebrikler! Artık çalışan bir kullanıcıların kimlik doğrulaması, güvenli bir şekilde OAuth 2.0 kullanarak Web API'leri çağırmak, ve kullanıcı ile ilgili temel bilgileri alın .NET WPF uygulaması vardır. Henüz yapmadıysanız, kiracınızı biraz kullanıcıyla doldurmanın zamanı geldi. DirectorySearcher uygulamanızı çalıştırın ve oturum kullanıcılarla birini açın. UPN'lerine göre diğer kullanıcılar için arama yapın. Uygulamayı kapatın ve yeniden çalıştırmalısınız. Kullanıcının oturumunu nasıl değişmeden kalır dikkat edin. Oturumu kapatın ve başka bir kullanıcı olarak yeniden oturum açın.

ADAL, bu ortak kimlik özellikleri uygulamanıza eklemenize kolaylaştırır. Bunu kirli çalışması önbellek yönetimi, kullanıcı bir oturum açma kullanıcı Arabirimi, süresi dolan ve daha fazlasını yenileme sunan, OAuth protokol desteği dahil olmak üzere sizin için üstlenir. Gerçekten tek bilmeniz gereken, tek bir API çağrısıdır (`authContext.AcquireTokenAsync(...)`).

Tamamlanan örnek (olmadan yapılandırma değerlerinize) başvuru için bkz. [github'da](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).

## <a name="next-steps"></a>Sonraki adımlar

OAuth 2.0 taşıyıcı erişim belirteçleri kullanarak bir web API'sini korumak öğrenin.
> [!div class="nextstepaction"]
> [.NET Web API'si Azure AD ile güvenli >>](quickstart-v1-dotnet-webapi.md)
