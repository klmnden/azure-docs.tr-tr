---
title: "Azure AD Windows Phone Başlarken | Microsoft Docs"
description: "Oturum açmak için Azure AD ile tümleşir ve Azure AD çağıran bir Windows Phone uygulama oluşturmak nasıl OAuth kullanan API'ler korumalı."
services: active-directory
documentationcenter: windows
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 66f5ac20-5e1f-4b9d-bb99-9b3305e26416
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 01/07/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 03c4b6d225dce99d79ef6c1ba2af43af8dea3eae
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="integrate-azure-ad-with-a-windows-phone-app"></a>Azure AD ile Windows Phone Uygulama Tümleştirme
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

> [!NOTE]
> Windows Phone 8.1 ve önceki sürümlerde oluşturulmuş projeler Visual Studio 2017’de desteklenmez.  Daha fazla bilgi için bkz. [Visual Studio 2017 Platform Desteği ve Uyumluluk](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).

Windows Phone 8.1 uygulama geliştiriyorsanız, Azure AD Basit ve kolay, kullanıcılarınız kendi Active Directory hesapları ile kimlik doğrulaması kolaylaştırır.  Ayrıca, tüm web API Office 365 API'leri veya Azure API'sini gibi Azure AD tarafından korunan güvenli bir şekilde kullanmak uygulamanızı sağlar.

> [!NOTE]
> Bu kod örneği, ADAL v2.0 kullanır.  En son teknoloji için bunun yerine denemek isteyebilirsiniz bizim [ADAL v3.0 kullanarak Windows Evrensel Öğreticisine](active-directory-devquickstarts-windowsstore.md).  Windows Phone 8.1 için gerçekten bir uygulama geliştiriyorsanız, bu en iyi yerdir.  ADAL v2.0 hala tam olarak desteklenir ve Windows Phone 8.1 uygulamaları agianst geliştirmenin önerilen yöntem Azure AD kullanıyor.
> 
> 

Korunan kaynaklara erişim için gereken .NET yerel istemciler için Azure AD Active Directory kimlik doğrulama kitaplığı veya ADAL sağlar.  ADAL'ın tek amacı hayatta erişim belirteçleri almak uygulamanızı daha kolay hale getirmektir.  Ne kadar kolay olduğunu göstermek için burada size bir "Directory noktasıdır" Windows Phone 8.1 uygulama, yapı:

* Alır erişim belirteçleri kullanarak Azure AD Graph API çağırma [OAuth 2.0 kimlik doğrulama protokolü](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Verilen UPN olan kullanıcılar için bir dizin arar.
* İşaretlerini kullanıcıları.

Tam çalışan uygulama oluşturmak için ihtiyacınız:

1. Uygulamanızı Azure AD ile kaydedin.
2. Yükleme & ADAL yapılandırın.
3. ADAL, Azure AD'den belirteçleri almak için kullanın.

Başlamak için [bir çatı projesini indirin](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/skeleton.zip) veya [tamamlanan örnek indirme](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).  Her bir Visual Studio 2013 çözümüdür.  Ayrıca, kullanıcı oluşturma ve bir uygulamayı kaydetme Azure AD kiracısı gerekir.  Bir kiracı yoksa [edinebileceğinizi öğrenin](active-directory-howto-tenant.md).

## <a name="1-register-the-directory-searcher-application"></a>1. Dizin noktasıdır uygulamayı Kaydet
Belirteçleri almak uygulamanızı etkinleştirmek için ilk Azure AD kiracınızda kaydetmek ve Azure AD grafik API'sine erişim izni vermek gerekir:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üst çubuğunda hesabınızda altında tıklatıp **Directory** listesinde, Active Directory Kiracı uygulamanızı kaydetmek için istediğiniz yeri seçin.
3. Tıklayın **daha Hizmetleri** sol taraftaki gezinti içinde ve **Azure Active Directory**.
4. Tıklayın **uygulama kayıtlar** ve **Ekle**.
5. Komut istemlerini izleyin ve yeni bir **yerel istemci uygulaması**.
  * **Adı** uygulamayı son kullanıcılar uygulamanıza anlatmaktadır
  * **Yeniden yönlendirme URI'si** Azure AD belirteci yanıtları döndürmek için kullanacağı düzeni ve dize bir birleşimidir.  Şimdilik, örneğin bir yer tutucu değerini girin `http://DirectorySearcher`.  Biz bu değer daha sonra değiştirirsiniz.
6. Kayıt tamamladıktan sonra AAD uygulamanızı benzersiz bir uygulama kimliği atar.  Bu değer gerekir sonraki bölümlerde, bu nedenle uygulama sekmesinden kopyalayın.
7. Gelen **ayarları** sayfasında, **gerekli izinler** ve **Ekle**. Seçin **Microsoft Graph** API olarak ve ekleme **dizin verilerini okuma** altında izni **izinlere temsilci**.  Bu kullanıcılar için grafik API'si sorgulamak için uygulamanıza olanak sağlar.

## <a name="2-install--configure-adal"></a>2. Yükleme & ADAL yapılandırın
Azure AD'de bir uygulamanız varsa, ADAL yükleyin ve kimlikle ilgili kodunuzu yazın.  Azure AD ile iletişim kurabilmesi adal sırada uygulama kaydınızı hakkında bazı bilgiler ile sağlamanız gerekir.

* Paket Yöneticisi konsolu kullanılarak DirectorySearcher projeye ADAL ekleyerek başlayın.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

* DirectorySearcher projeyi açın `MainPage.xaml.cs`.  Değerleri değiştirin `Config Values` Azure Portalı'na giriş değerleri yansıtacak şekilde bölge.  ADAL kullandığında kodunuzu bu değerleri başvurur.
  * `tenant` Azure AD kiracınız, ör. contoso.onmicrosoft.com etki alanıdır
  * `clientId` Portaldan kopyaladığınız uygulamanızın istemci kimliği değil.
* Artık Windows Phone uygulamanız için geri çağırma URI bulmanız gerekir.  Bu satırında bir kesme noktası belirleyerek `MainPage` yöntemi:

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
* Uygulamayı çalıştırın ve kenara değerini kopyalayın `redirectUri` zaman kesme noktası isabet.  Aşağıdakine benzer görünmelidir

```
ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
```

* Geri **yapılandırma** sekmesi uygulamanızın Azure Yönetim Portalı'nda, değerini değiştirin **RedirectUri** bu değere sahip.  

## <a name="3-use-adal-to-get-tokens-from-aad"></a>3. AAD belirteçleri almak için ADAL'ı kullanın
Temel ADAL arkasında uygulamanızı bir erişim belirteci her gerektiğinde, onu yalnızca çağırdığı ilkesidir `authContext.AcquireToken(…)`, ve ADAL rest yapar.  

* İlk adım, uygulamanızın başlatmaktır `AuthenticationContext` -birincil sınıfı ADAL.  ADAL burada geçirdiğiniz budur gereken Azure AD ile iletişim kurmasını ve onu nasıl belirteçleri önbelleğe söyleyin koordinatları.

```C#
public MainPage()
{
    ...

    // ADAL for Windows Phone 8.1 builds AuthenticationContext instances through a factory
    authContext = AuthenticationContext.CreateAsync(authority).GetResults();
}
```

* Şimdi bulun `Search(...)` uygulamanın kullanıcı Arabiriminde kullanıcı cliks "Ara" düğmesini olduğunda çağrılacak yöntem.  Bu yöntem, UPN verilen arama terimiyle kullanıcıları için Azure AD Graph API sorgu için bir GET isteği yapar.  Ancak bir access_token ile dahil etmeniz grafik API'si sorgulamak için `Authorization` üstbilgi ve istek - ADAL nereden geldiğini olan budur.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    ...

    // Try to get a token without triggering any user prompt.
    // ADAL will check whether the requested token is in ADAL's token cache or can otherwise be obtained without user interaction.
    AuthenticationResult result = await authContext.AcquireTokenSilentAsync(graphResourceId, clientId);
    if (result != null && result.Status == AuthenticationStatus.Success)
    {
        // A token was successfully retrieved.
        QueryGraph(result);
    }
    else
    {
        // Acquiring a token without user interaction was not possible.
        // Trigger an authentication experience and specify that once a token has been obtained the QueryGraph method should be called
        authContext.AcquireTokenAndContinue(graphResourceId, clientId, redirectURI, QueryGraph);
    }
}
```
* Etkileşimli kimlik doğrulaması gerekli olduğunda, ADAL Windows Phone ' ın Web kimlik doğrulama Aracısı (WAB) kullanır ve [devamlılık modeli](http://www.cloudidentity.com/blog/2014/06/16/adal-for-windows-phone-8-1-deep-dive/) Azure AD oturum açma sayfasında görüntülenecek.  Kullanıcı oturum açtığında ADAL geçirmek uygulamanız gereken WAB etkileşim sonuçları.  Bu uygulama olarak kadar basittir `ContinueWebAuthentication` arabirimi:

```C#
// This method is automatically invoked when the application
// is reactivated after an authentication interaction through WebAuthenticationBroker.
public async void ContinueWebAuthentication(WebAuthenticationBrokerContinuationEventArgs args)
{
    // pass the authentication interaction results to ADAL, which will
    // conclude the token acquisition operation and invoke the callback specified in AcquireTokenAndContinue.
    await authContext.ContinueAcquireTokenAsync(args);
}
```

* Kullanın şimdi `AuthenticationResult` , uygulamanız için ADAL döndürdü.  İçinde `QueryGraph(...)` geri arama, yetkilendirme üst GET isteğini aldığınız access_token ekleme:

```C#
private async void QueryGraph(AuthenticationResult result)
{
    if (result.Status != AuthenticationStatus.Success)
    {
        MessageDialog dialog = new MessageDialog(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", result.Error, result.ErrorDescription), "Sorry, an error occurred while signing you in.");
        await dialog.ShowAsync();
    }

    // Add the access token to the Authorization Header of the call to the Graph API, and call the Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

    ...
}
```
* Aynı zamanda `AuthenticationResult` uygulamanızda kullanıcı hakkındaki bilgileri görüntülemek için nesne. İçinde `QueryGraph(...)` yöntemi, kullanıcının kimliğini sayfada göstermek için sonuç kullanın:

```C#
// Update the Page UI to represent the signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
* Son olarak, kullanıcı uygulama da dışında imzalamak için ADAL kullanabilirsiniz.  Kullanıcı "Oturumu Kapat" düğmesini tıklattığında sonraki çağrısı emin olmak istiyoruz `AcquireTokenSilentAsync(...)` başarısız olur.  ADAL ile bu belirteç önbelleği temizleme olarak kadar kolaydır:

```C#
private void SignOut()
{
    // Clear session state from the token cache.
    authContext.TokenCache.Clear();

    ...
}
```

Tebrikler! Artık çalışma kullanıcıların kimliğini doğrulamak için güvenli bir şekilde Web OAuth 2.0 kullanan API'leri çağırmak gönderebilen Windows Phone Uygulama sahip ve kullanıcı hakkındaki temel bilgileri alın.  Henüz yapmadıysanız, bazı kullanıcılar ile Kiracı doldurmak için zaman sunulmuştur.  DirectorySearcher uygulamanızı çalıştırın ve bu kullanıcıların biri oturum oturum.  Diğer kullanıcıların UPN Değerlerinin üzerinde göre arayın.  Uygulamayı kapatın ve yeniden çalıştırın.  Kullanıcının oturumunu nasıl olduğu gibi kalır dikkat edin.  Oturumu kapatın ve başka bir kullanıcı olarak yeniden oturum açın.

ADAL, bu ortak kimlik özelliklerin tümü, uygulamanıza eklemenizi kolaylaştırır.  Bu tüm dirty iş, - önbellek yönetimi, OAuth protokol desteği, kullanıcı bir oturum açma kullanıcı Arabirimi, belirteçlerin süresinin ve daha fazlasını yenileme sunmak için mvc'deki.  Gerçekten bilmeniz gereken tek şey tek bir API çağrısı `authContext.AcquireToken*(…)`.

Başvuru için tamamlanan örnek (yapılandırma değerleriniz olmadan) sağlanır [burada](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).  Artık ek kimlik senaryolara geçebilirsiniz.  Denemek isteyebilirsiniz:

[.NET Web API'si Azure AD ile güvenli >>](active-directory-devquickstarts-webapi-dotnet.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

