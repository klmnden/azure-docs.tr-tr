---
title: Kimlik doğrulaması, kayıt, profili Azure Active Directory B2C, Düzenle | Microsoft Docs
description: Ve profil Yönetimi Azure Active Directory B2C kullanarak oturum açma, kaydolma, içeren bir Windows masaüstü uygulaması oluşturmayı öğrenin.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 01/07/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 43da5b32fe3ad8891f89544d0f9bdbd1d4d127d0
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53606191"
---
# <a name="azure-ad-b2c-build-a-windows-desktop-app"></a>Azure AD B2C: Windows masaüstü uygulaması oluşturma
Azure Active Directory (Azure AD) B2C'yi kullanarak masaüstü uygulamanızı birkaç adımda güçlü Self Servis kimlik yönetimi özellikleri ekleyebilirsiniz. Bu makale, kullanıcı oturum açma, kaydolma ve profil yönetimini kapsayan .NET Windows Presentation Foundation (WPF) "Yapılacaklar listesi" uygulamasının nasıl oluşturulacağını gösterir. Uygulama, bir kullanıcı adı veya e-posta kullanarak kaydolma ve oturum açma desteği içerecektir. Facebook ve Google gibi sosyal medya hesaplarını kullanarak, kaydolma ve oturum açma desteği de içerecektir.

## <a name="get-an-azure-ad-b2c-directory"></a>Azure AD B2C dizini alma
Azure AD B2C'yi kullanabilmek için önce dizin veya kiracı oluşturmanız gerekir. Dizin; tüm kullanıcılarınız, uygulamalarınız, gruplarınız ve daha fazlası için bir kapsayıcıdır. Henüz yoksa devam etmeden önce bu kılavuzda [bir B2C dizini oluşturun](active-directory-b2c-get-started.md).

## <a name="create-an-application"></a>Uygulama oluşturma
Ardından B2C dizininizde uygulama oluşturmanız gerekir. Bu, uygulamanız ile güvenli şekilde iletişim kurması için gereken bilgileri Azure AD'ye verir. Bir uygulama oluşturmak için [bu talimatları](active-directory-b2c-app-registration.md) izleyin. Şunları yaptığınızdan emin olun:

* Dahil bir **yerel istemci** uygulama.
* Kopyalama **yeniden yönlendirme URI'si** `urn:ietf:wg:oauth:2.0:oob`. Bu URL, bu kod örneği için varsayılan URL'dir.
* Uygulamanıza atanan **Uygulama Kimliği**'ni kopyalayın. Buna daha sonra ihtiyacınız olacak.

## <a name="create-your-policies"></a>İlkelerinizi oluşturma
Azure AD B2C'de her kullanıcı deneyimi, bir [ilke](active-directory-b2c-reference-policies.md) ile tanımlanır. Bu kod örneği, üç kimlik deneyimi içerir: kaydolma, oturum açın ve profillerini düzenleyebilir. Açıklandığı gibi her tür için bir ilke oluşturmak gereken [ilke başvurusu makalesinde](active-directory-b2c-reference-policies.md#create-a-sign-up-user-flow). Üç ilkeyi oluştururken şunları yaptığınızdan emin olun:

* Kimlik sağlayıcılar dikey penceresinde **Kullanıcı Kimliği ile kayıt** veya **E-posta ile kayıt** yöntemini seçin.
* Kaydolma ilkenizde, **Görünen ad** ve diğer kaydolma özniteliklerini seçin.
* Her ilke için uygulamanın talep ettiği gibi **Görünen ad** ve **Nesne Kimliği** öğelerini seçin. Diğer talepleri de seçebilirsiniz.
* Oluşturduktan sonra her ilkenin **Adını** kaydedin. `b2c_1_` önekine sahip olmalıdır. Bu ilke adlarına daha sonra ihtiyacınız olacak.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Üç ilkenizi başarıyla oluşturduktan sonra uygulamanızı oluşturmaya hazırsınız.

## <a name="download-the-code"></a>Kodu indirme
Bu öğretici için kod [GitHub'da korunur](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet). İşlemlere devam ederken örneği oluşturmak için [ bir çatı projesini .zip dosyası olarak indirebilirsiniz](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip). Ayrıca çatıyı kopyalayabilirsiniz:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git
```

Tamamlanan uygulama aynı zamanda [.zip dosyası olarak](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) veya aynı deponun `complete` dalı üzerinde de kullanılabilir.

Örnek kodu indirdikten sonra başlamak için Visual Studio .sln dosyasını açın. `TaskClient` Kullanıcının etkileşime WPF masaüstü uygulaması projesidir. Bu öğreticinin amaçları doğrultusunda, arka uç görev web API'si, her kullanıcının yapılacaklar listesini depolayan Azure üzerinde barındırılan çağırır. Web API'si oluşturma gerekmez, biz zaten çalışıyor olması.

Web API'si güvenli bir şekilde istekleri Azure AD B2C kullanarak kimliğini nasıl bilgi edinmek için kullanıma [web API'si ile çalışmaya başlama makale](active-directory-b2c-devquickstarts-api-dotnet.md).

## <a name="execute-policies"></a>İlkeleri yürütün
Uygulamanızı, HTTP isteğinin bir parçası olarak çalıştırmak istediğiniz ilkeyi belirtin kimlik doğrulaması iletiler göndererek Azure AD B2C ile iletişim kurar. .NET Masaüstü uygulamaları için OAuth 2.0 kimlik doğrulama iletileri, ilkeleri yürütmek için Microsoft kimlik doğrulama kitaplığı (MSAL) önizleme kullanın ve web API'lerini çağırma belirteçleri almak.

### <a name="install-msal"></a>MSAL yükleyin
Ekleme için MSAL `TaskClient` Visual Studio Paket Yöneticisi konsolunu kullanarak bir proje.

```
PM> Install-Package Microsoft.Identity.Client -IncludePrerelease
```

### <a name="enter-your-b2c-details"></a>B2C bilgilerinizi girme
Dosyayı açmak `Globals.cs` ve her bir özellik değerlerini kendi değerlerinizle değiştirin. Bu sınıf genelinde kullanılan `TaskClient` için yaygın olarak kullanılan başvuru değeri.

```csharp
public static class Globals
{
    ...

    // TODO: Replace these five default with your own configuration values
    public static string tenant = "fabrikamb2c.onmicrosoft.com";
    public static string clientId = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6";
    public static string signInPolicy = "b2c_1_sign_in";
    public static string signUpPolicy = "b2c_1_sign_up";
    public static string editProfilePolicy = "b2c_1_edit_profile";

    ...
}
```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### <a name="create-the-publicclientapplication"></a>PublicClientApplication oluşturma
MSAL, birincil sınıf `PublicClientApplication`. Bu sınıf, uygulamanızı Azure AD B2C sistemde temsil eder. Uygulama initalizes örneğini oluşturduğunuzda `PublicClientApplication` içinde `MainWindow.xaml.cs`. Bu pencerenin kullanılabilir.

```csharp
protected async override void OnInitialized(EventArgs e)
{
    base.OnInitialized(e);

    pca = new PublicClientApplication(Globals.clientId)
    {
        // MSAL implements an in-memory cache by default. Since we want tokens to persist when the user closes the app,
        // we've extended the MSAL TokenCache and created a simple FileCache in this app.
        UserTokenCache = new FileCache(),
    };

    ...
```

### <a name="initiate-a-sign-up-flow"></a>Bir kayıt akışını başlatmak
Bir kullanıcı yukarı işaretlerine seçtiğinde, oluşturduğunuz kaydolma İlkesi kullanan bir kaydolma akış başlatma istiyorsunuz. MSAL kullanarak çağrı `pca.AcquireTokenAsync(...)`. İçin geçirdiğiniz parametreler `AcquireTokenAsync(...)` hangi belirteci alırsınız, kimlik doğrulama isteği ve daha fazla kullanılan ilke belirleyin.

```csharp
private async void SignUp(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        // Use the app's clientId here as the scope parameter, indicating that
        // you want a token to the your app's backend web API (represented by
        // the cloud hosted task API). Use the UiOptions.ForceLogin flag to
        // indicate to MSAL that it should show a sign-up UI no matter what.
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                Globals.signUpPolicy);

        // Upon success, indicate in the app that the user is signed in.
        SignInButton.Visibility = Visibility.Collapsed;
        SignUpButton.Visibility = Visibility.Collapsed;
        EditProfileButton.Visibility = Visibility.Visible;
        SignOutButton.Visibility = Visibility.Visible;

        // When the request completes successfully, you can get user
        // information from the AuthenticationResult
        UsernameLabel.Content = result.User.Name;

        // After the sign up successfully completes, display the user's To-Do List
        GetTodoList();
    }

    // Handle any exeptions that occurred during execution of the policy.
    catch (MsalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
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

### <a name="initiate-a-sign-in-flow"></a>Oturum açma akışını başlatmak
Kayıt bir akışı başlatan aynı şekilde oturum açma akışını başlatabilirsiniz. Bir kullanıcı oturum açtığında, oturum açma İlkesi kullanarak bu süre MSAL, aynı çağrı yapın:

```csharp
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.signInPolicy);
        ...
```

### <a name="initiate-an-edit-profile-flow"></a>Profili Düzenle akış başlatma
Yeniden aynı biçimde bir profil Düzenleme İlkesi yürütebilirsiniz:

```csharp
private async void EditProfile(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.editProfilePolicy);
```

Bu durumların tümünde, MSAL belirteç ya da döndürür `AuthenticationResult` veya bir özel durum oluşturur. MSAL belirteci Al her zaman kullanabileceğiniz `AuthenticationResult.User` uygulamada, kullanıcı Arabirimi gibi kullanıcı verileri güncelleştirmek için nesne. ADAL belirteç kullanılmak üzere uygulamanın diğer bölümlerini de önbelleğe alır.

### <a name="check-for-tokens-on-app-start"></a>Uygulama başlangıç menüsünde belirteçleri denetle
MSAL, kullanıcının oturum açma durumunu izlemek için de kullanabilirsiniz. Bu uygulamada bile kullanıcılar uygulamayı kapatın ve yeniden açtıktan sonra oturum açmış durumda kalma kullanıcı istiyoruz. Geri içinde `OnInitialized` geçersiz kılmak, MSAL'ın kullanın `AcquireTokenSilent` yöntemi olup olmadığını denetlemek için önbelleğe alınan belirteçleri:

```csharp
AuthenticationResult result = null;
try
{
    // If the user has has a token cached with any policy, we'll display them as signed-in.
    TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
    string existingPolicy = tci == null ? null : tci.Policy;
    result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    SignInButton.Visibility = Visibility.Collapsed;
    SignUpButton.Visibility = Visibility.Collapsed;
    EditProfileButton.Visibility = Visibility.Visible;
    SignOutButton.Visibility = Visibility.Visible;
    UsernameLabel.Content = result.User.Name;
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "failed_to_acquire_token_silently")
    {
        // There are no tokens in the cache. Proceed without calling the To Do list service.
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
```

## <a name="call-the-task-api"></a>' % S'görev API'si çağırma
MSAL, ilkeleri yürütün ve belirteçleri almak için artık kullandınız. Bir görev API'si çağırma için bu belirteçleri kullanmak istediğinizde, MSAL'ın yeniden kullanabilirsiniz `AcquireTokenSilent` yöntemi olup olmadığını denetlemek için önbelleğe alınan belirteçleri:

```csharp
private async void GetTodoList()
{
    AuthenticationResult result = null;
    try
    {
        // Here we want to check for a cached token, independent of whatever policy was used to acquire it.
        TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
        string existingPolicy = tci == null ? null : tci.Policy;

        // Use AcquireTokenSilent to indicate that MSAL should throw an exception if a token cannot be acquired
        result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    }
    // If a token could not be acquired silently, we'll catch the exception and show the user a message.
    catch (MsalException ex)
    {
        // There is no access token in the cache, so prompt the user to sign-in.
        if (ex.ErrorCode == "failed_to_acquire_token_silently")
        {
            MessageBox.Show("Please sign up or sign in first");
            SignInButton.Visibility = Visibility.Visible;
            SignUpButton.Visibility = Visibility.Visible;
            EditProfileButton.Visibility = Visibility.Collapsed;
            SignOutButton.Visibility = Visibility.Collapsed;
            UsernameLabel.Content = string.Empty;
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
    ...
```

Zaman çağrısı `AcquireTokenSilentAsync(...)` başarılı olur ve önbellekte bir belirteç bulunana, belirtece eklediğiniz `Authorization` HTTP isteği üstbilgisi. Görev web API'si, bu üst bilgi, kullanıcının yapılacaklar listesini okuma isteği kimlik doğrulaması için kullanır:

```csharp
    ...
    // Once the token has been returned by MSAL, add it to the http authorization header, before making the call to access the To Do list service.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);

    // Call the To Do list service.
    HttpResponseMessage response = await httpClient.GetAsync(Globals.taskServiceUrl + "/api/tasks");
    ...
```

## <a name="sign-the-user-out"></a>Kullanıcı Oturumu Kapat
Son olarak, kullanıcı seçtiğinde uygulaması ile bir kullanıcının oturumunu sona erdirmek için MSAL kullanabilirsiniz **oturumunuzu**. MSAL kullanarak, bu belirteç önbellekten belirteçlerinin tümü temizleyerek gerçekleştirilir:

```csharp
private void SignOut(object sender, RoutedEventArgs e)
{
    // Clear any remnants of the user's session.
    pca.UserTokenCache.Clear(Globals.clientId);

    // This is a helper method that clears browser cookies in the browser control that MSAL uses, it is not part of MSAL.
    ClearCookies();

    // Update the UI to show the user as signed out.
    TaskList.ItemsSource = string.Empty;
    SignInButton.Visibility = Visibility.Visible;
    SignUpButton.Visibility = Visibility.Visible;
    EditProfileButton.Visibility = Visibility.Collapsed;
    SignOutButton.Visibility = Visibility.Collapsed;
    return;
}
```

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma
Son olarak, derleme ve örneği çalıştırın. Bir e-posta adresi veya kullanıcı adı kullanarak uygulamaya kaydoluyor. Oturumu kapatın ve aynı kullanıcı olarak yeniden oturum açın. Bu kullanıcının profilini düzenleyin. Oturumu kapatın ve farklı bir kullanıcı kullanarak kaydolun.

## <a name="add-social-idps"></a>Sosyal Idp'yi Ekle
Şu anda, uygulamayı yalnızca kullanıcı kaydolma ve oturum kullanan açma destekler **yerel hesaplar**. Bunlar, bir kullanıcı adı ve parola kullanan B2C dizininizde saklanan hesaplarıdır. Azure AD B2C'yi kullanarak kodunuzu değiştirmeden diğer kimlik sağlayıcı (IDP) için destek ekleyebilirsiniz.

Sosyal Idp'yi uygulamanıza eklemek için şu makalelere ayrıntılı yönergeleri takip ederek başlayın. Desteklemek istediğiniz her bir IDP için bu sistemde bir uygulamayı kaydetme ve bir istemci kimliği almak gerekiyor

* [Facebook bir IDP ayarlayın](active-directory-b2c-setup-fb-app.md)
* [Google bir IDP ayarlayın.](active-directory-b2c-setup-goog-app.md)
* [Amazon bir IDP ayarlayın](active-directory-b2c-setup-amzn-app.md)
* [LinkedIn bir IDP ayarlayın](active-directory-b2c-setup-li-app.md)

Kimlik sağlayıcıları B2C dizininizi ekledikten sonra her yeni IDP içerecek şekilde ilkelerinizi üç açıklandığı gibi Düzenle gerek [ilke başvurusu makalesinde](active-directory-b2c-reference-policies.md). İlkelerinizi kaydettikten sonra uygulamayı yeniden çalıştırın. Oturum açma eklenen yeni IDP görmeniz gerekir ve her kimliğinizi kaydolma seçenekleri deneyimleri.

Uygulamanızın ilkelerini deneyebilir ve örnek uygulamanızı etkilerini gözlemleyin. Ekleme veya Idp'yi kaldırmak, uygulama taleplerini yönetmek veya kaydolma özniteliklerini değiştirebilirsiniz. İlkeleri ve kimlik doğrulama isteklerini MSAL nasıl birbirine bağlayın görene kadar denemeler yapın.

Tamamlanan örnek, başvuru için [.zip dosyası olarak sağlanan](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip). Aynı zamanda GitHub'dan kopyalayabilirsiniz:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git```
