---
title: "Azure AD Windows Evrensel Başlarken Platformu (UWP/XAML) | Microsoft Docs"
description: "Oturum açma için Azure AD ile tümleştirme, Azure AD çağıran derleme Windows mağazası uygulamaları API'leri OAuth kullanan korumalı."
services: active-directory
documentationcenter: windows
author: jmprieur
manager: mtillman
editor: 
ms.assetid: 3b96a6d1-270b-4ac1-b9b5-58070c896a68
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 11/30/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 2282a59c9dd5d5d76a5b3e19f602e9d3dcc0b4ef
ms.sourcegitcommit: 234c397676d8d7ba3b5ab9fe4cb6724b60cb7d25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2017
---
# <a name="azure-ad-windows-universal-platform-uwpxaml-getting-started"></a>Azure AD Windows Evrensel Başlarken Platformu (UWP/XAML)
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

> [!NOTE]
> Windows mağazası 8.1 ve önceki sürümü projeleri Visual Studio 2017 desteklenmez.  Daha fazla bilgi için bkz. [Visual Studio 2017 Platform Desteği ve Uyumluluk](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).

Windows mağazası için uygulama geliştiriyorsanız, Azure Active Directory (Azure AD), basit ve kendi Active Directory hesaplarıyla, kullanıcıların kimliklerini doğrulamak kolay hale getirir. Bir uygulama, Azure AD ile tümleştirerek, güvenli bir şekilde Office 365 API'leri veya Azure API'sini gibi Azure AD tarafından korunan tüm web API'si kullanabilir.

Korunan kaynaklara erişim için gereken Windows mağazası Masaüstü uygulamaları için Active Directory Authentication Library (ADAL) Azure AD sağlar. ADAL tek amacı, erişim belirteçleri almak uygulama için kolay hale getirmektir. Ne kadar kolay olduğunu göstermek için bu makalede DirectorySearcher Windows mağazası uygulamasının nasıl oluşturulacağını gösterir:

* Alır erişim belirteçleri kullanarak Azure AD Graph API çağırma [OAuth 2.0 kimlik doğrulama protokolü](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Sağlanan kullanıcı asıl adı (UPN) sahip kullanıcılar için bir dizin arar.
* İşaretlerini kullanıcıları.

## <a name="before-you-get-started"></a>Başlamadan önce
* Karşıdan [çatı projesini](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip), veya indirme [tamamlanan örnek](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip). Her yükleme Visual Studio 2015 çözümüdür.
* Ayrıca, kullanıcılar oluşturmak ve uygulamayı kaydetmek Azure AD kiracısı gerekir. Bir kiracı yoksa [edinebileceğinizi öğrenin](active-directory-howto-tenant.md).

Hazır olduğunuzda, sonraki üç bölümde yordamları izleyin.

## <a name="step-1-register-the-directorysearcher-app"></a>1. adım: DirectorySearcher uygulamayı Kaydet
Belirteçleri almak uygulamayı etkinleştirmek için önce Azure AD kiracınızda kaydetmek ve Azure AD grafik API'sine erişim izni vermek gerekir. Bunu yapmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üst çubuğunda hesabınızı tıklatın. Ardından, altında **Directory** listesinde, uygulama kaydetmek istediğiniz Active Directory Kiracı seçin.
3. Tıklatın **daha Hizmetleri** sol bölmesinde ve seçip **Azure Active Directory**.
4. Tıklatın **uygulama kayıtlar**ve ardından **Ekle**.
5. Oluşturmak için istemleri izleyerek bir **yerel istemci uygulaması**.
  * **Ad** kullanıcılara uygulamasının açıklar.
  * **Yeniden yönlendirme URI'si** belirteci yanıtları döndürmek için Azure AD kullanır ve dize şeması bir birleşimidir. Şu an için bir yer tutucu değerini girin (örneğin, **http://DirectorySearcher**). Değer daha sonra değiştirirsiniz.
6. Kayıt tamamladıktan sonra Azure AD uygulama benzersiz uygulama kimliği atar. Değer kopyalayın **uygulama** sekmesinde, çünkü daha sonra ihtiyacınız olacak.
7. Üzerinde **ayarları** sayfasında, **gerekli izinler**ve ardından **Ekle**.
8. İçin **Azure Active Directory** uygulama, select **Microsoft Graph** API olarak.
9. Altında **izinlere temsilci**, ekleme **gibi oturum açan kullanıcının dizine erişim** izni. Bunun yapılması, kullanıcılar için grafik API'si sorgulamak uygulama sağlar.

## <a name="step-2-install-and-configure-adal"></a>2. adım: Yükleme ve ADAL yapılandırma
Azure AD'de bir uygulamaya sahip olduğunuza göre ADAL yükleyin ve kimlikle ilgili kodunuzu yazın. Azure AD ile iletişim kurmak ADAL etkinleştirmek için uygulama kaydı hakkında bazı bilgileri verin.

1. ADAL Paket Yöneticisi konsolu kullanılarak DirectorySearcher projeye ekleyin.

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

2. DirectorySearcher projesinde MainPage.xaml.cs açın.
3. Değerleri değiştirin **yapılandırma değerleri** Azure portalında girdiğiniz değerleri bölgesiyle. ADAL kullandığında kodunuzu bu değerleri gösterir.
  * *Kiracı* Azure AD kiracınız (örneğin, contoso.onmicrosoft.com) etki alanıdır.
  * *ClientID* portalından kopyalandığından uygulama istemci Kimliğini gösterir.
4. Artık Windows mağazası uygulamanız için geri çağırma URI bulmanız gerekir. Bu satırında bir kesme noktası belirleyerek `MainPage` yöntemi:
    ```
    redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
    ```
5. Çözüm derleme, tüm başvuruları paket emin olarak geri yüklenir. Herhangi bir paket eksikse, NuGet Paket Yöneticisi'ni açın ve bunları geri yükleyin.
6. Uygulamayı çalıştırın ve değerini kopyalayın `redirectUri` zaman kesme noktası isabet. Değer aşağıdaki gibi görünmelidir:

    ```
    ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
    ```

7. Geri **ayarları** sekmesi Azure portalında uygulama ekleme bir **RedirectUri** önceki değerine sahip.  

## <a name="step-3-use-adal-to-get-tokens-from-azure-ad"></a>3. adım: Kullanım Azure AD'den belirteçleri almak için ADAL
Temel ADAL arkasında uygulamanın bir erişim belirteci ihtiyacı olduğunda, onu yalnızca çağırdığı ilkesidir `authContext.AcquireToken(…)`, ve ADAL rest yapar.  

1. Uygulamanın başlatma `AuthenticationContext`, ADAL birincil sınıfının olduğu. Bu eylem ADAL geçirir gereken Azure AD ile iletişim kurmasını ve onu nasıl belirteçleri önbelleğe söyleyin koordinatları.

    ```C#
    public MainPage()
    {
        ...

        authContext = new AuthenticationContext(authority);
    }
    ```

2. Bulun `Search(...)` kullanıcılar'ı tıklattığınızda, çağrılan yöntem **arama** uygulamanın UI düğmesinde. Bu yöntem, UPN verilen arama terimiyle kullanıcıları için Azure AD Graph API sorgu için bir get isteği yapar. Grafik API'si sorgulamak için bir erişim belirteci isteğin içinde dahil **yetkilendirme** üstbilgi. ADAL nereden geldiğini budur.

    ```C#
    private async void Search(object sender, RoutedEventArgs e)
    {
        ...
        AuthenticationResult result = null;
        try
        {
            result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectURI, new PlatformParameters(PromptBehavior.Auto, false));
        }
        catch (AdalException ex)
        {
            if (ex.ErrorCode != "authentication_canceled")
            {
                ShowAuthError(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", ex.ErrorCode, ex.Message));
            }
            return;
        }
        ...
    }
    ```
    Uygulama isteğinde bulunduğunda bir belirteç çağırarak `AcquireTokenAsync(...)`, ADAL kullanıcı kimlik bilgilerinin sorulmasına olmadan bir simge döndürür dener. ADAL kullanıcı bir belirteç almak üzere oturum açmak gerektiğini belirlerse, bir oturum açma iletişim kutusu görüntüler, kullanıcının kimlik bilgilerini toplar ve kimlik doğrulaması başarılı olduktan sonra bir belirteç döndürür. ADAL herhangi bir nedenle bir belirteç döndüremedi ise *yazılımdan AuthenticationResult* bir hata durumudur.
3. Artık yalnızca aldığınız erişim belirtecini kullanmak için zaman yapılır. Ayrıca, `Search(...)` yöntemi, grafik API'si get isteğine belirteç ekleme **yetkilendirme** üstbilgisi:

    ```C#
    // Add the access token to the Authorization header of the call to the Graph API, and call the Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new HttpCredentialsHeaderValue("Bearer", result.AccessToken);

    ```
4. Kullanabileceğiniz `AuthenticationResult` nesnesi kullanıcının kimliği gibi uygulama kullanıcı hakkındaki bilgileri görüntülemek için:

    ```C#
    // Update the page UI to represent the signed-in user
    ActiveUser.Text = result.UserInfo.DisplayableId;
    ```
5. ADAL, kullanıcılar uygulama dışında imzalamak için de kullanabilirsiniz. Kullanıcı tıkladığında **oturum kapatma** düğmesini tıklatın, sonraki çağrısı emin `AcquireTokenAsync(...)` bir oturum açma görünümü gösterir. ADAL ile bu eylem belirteç önbelleği temizleme olarak kadar kolaydır:

    ```C#
    private void SignOut()
    {
        // Clear session state from the token cache.
        authContext.TokenCache.Clear();

        ...
    }
    ```

## <a name="whats-next"></a>Sırada ne var?
Artık çalışan bir kullanıcıların kimlik doğrulaması, güvenli bir şekilde web OAuth 2.0 kullanan API'leri çağırmak ve kullanıcı hakkındaki temel bilgileri elde Windows mağazası uygulaması sahipsiniz.

Kiracı kullanıcılarla zaten doldurulmuş yapmadıysanız, bunu yapmak için gereken süre sunulmuştur.
1. DirectorySearcher uygulamanızı çalıştırın ve ardından kullanıcılardan birine oturum açın.
2. Diğer kullanıcıların UPN Değerlerinin üzerinde göre arayın.
3. Uygulamayı kapatın ve onu yeniden çalıştırın. Kullanıcının oturumunu nasıl olduğu gibi kalır dikkat edin.
4. Sağ tıklayarak alt çubukta görüntülemek için oturumu kapatın ve ardından başka bir kullanıcı olarak yeniden oturum açın.

ADAL, bir uygulamaya bu ortak kimlik özelliklerin tümü eklemenizi kolaylaştırır. Bu tüm dirty iş sizin için önbellek yönetimi gibi OAuth protokol desteği, kullanıcı bir oturum açma kullanıcı Arabirimi, sunan mvc'deki ve yenileme belirteçleri süresi. Tek bir API çağrısı yalnızca, bilmeniz gereken `authContext.AcquireToken*(…)`.

Başvuru için karşıdan [tamamlanan örnek](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip) (yapılandırma değerleriniz olmadan).

Artık ek kimlik senaryolara geçebilirsiniz. Örneğin, [Azure AD ile .NET Web API'si güvenli](active-directory-devquickstarts-webapi-dotnet.md).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
