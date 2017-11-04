---
title: "Azure AD v2 Windows Masaüstü başlamak - kullanın | Microsoft Docs"
description: "Windows Masaüstü .NET (XAML) uygulamaları, Azure Active Directory v2 bitiş noktası tarafından erişim belirteçleri gerektiren bir API nasıl çağırabilirsiniz"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 826ba0a00b26993d4f37f0a8ce587d7bb77e7eb4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
## <a name="use-the-microsoft-authentication-library-msal-to-get-a-token-for-the-microsoft-graph-api"></a>Microsoft Graph API için bir belirteç almak üzere Microsoft kimlik doğrulama kitaplığı (MSAL) kullanın

Bu bölümde MSAL Microsoft Graph API'si bir belirteç almak için nasıl kullanılacağını gösterir.

1.  İçinde `MainWindow.xaml.cs`, MSAL Kitaplığı Başvurusu sınıfına ekleyin:

```csharp
using Microsoft.Identity.Client;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Değiştir <code>MainWindow</code> sınıf koduyla:
</li>
</ol>

```csharp
public partial class MainWindow : Window
{
    //Set the API Endpoint to Graph 'me' endpoint
    string _graphAPIEndpoint = "https://graph.microsoft.com/v1.0/me";

    //Set the scope for API call to user.read
    string[] _scopes = new string[] { "user.read" };


    public MainWindow()
    {
        InitializeComponent();
    }

    /// <summary>
    /// Call AcquireTokenAsync - to acquire a token requiring user to sign-in
    /// </summary>
    private async void CallGraphButton_Click(object sender, RoutedEventArgs e)
    {
        AuthenticationResult authResult = null;

        try
        {
            authResult = await App.PublicClientApp.AcquireTokenSilentAsync(_scopes, App.PublicClientApp.Users.FirstOrDefault());
        }
        catch (MsalUiRequiredException ex)
        {
            // A MsalUiRequiredException happened on AcquireTokenSilentAsync. This indicates you need to call AcquireTokenAsync to acquire a token
            System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");

            try
            {
                authResult = await App.PublicClientApp.AcquireTokenAsync(_scopes);
            }
            catch (MsalException msalex)
            {
                ResultText.Text = $"Error Acquiring Token:{System.Environment.NewLine}{msalex}";
            }
        }
        catch (Exception ex)
        {
            ResultText.Text = $"Error Acquiring Token Silently:{System.Environment.NewLine}{ex}";
            return;
        }

        if (authResult != null)
        {
            ResultText.Text = await GetHttpContentWithToken(_graphAPIEndpoint, authResult.AccessToken);
            DisplayBasicTokenInfo(authResult);
            this.SignOutButton.Visibility = Visibility.Visible;
        }
    }
}
```

<!--start-collapse-->
### <a name="more-information"></a>Daha Fazla Bilgi
#### <a name="getting-a-user-token-interactive"></a>Etkileşimli bir kullanıcı belirteci alma
Çağırma `AcquireTokenAsync` yöntemi, oturum açmak için kullanıcıdan bir pencere sonuçlanıyor. Uygulamalar genellikle gerektirir etkileşimli olarak korunan bir kaynağa erişmek için ihtiyaç duydukları ilk kez oturum açmak bir kullanıcı ya da (örneğin kullanıcının parolasının süresi) belirteci başarısız edinmeye sessiz bir işlem.

#### <a name="getting-a-user-token-silently"></a>Bir kullanıcı sessizce belirteci alma
`AcquireTokenSilentAsync`belirteç satın almalar ve herhangi bir kullanıcı etkileşimi olmadan yenileme işler. Sonra `AcquireTokenAsync` ilk kez yürütüldüğünde `AcquireTokenSilentAsync` veya belirteçleri yenileme isteği için çağrıları sessizce gerçekleştirilmediğinden sonraki çağrılar için-korumalı kaynaklara erişmek için kullanılan belirteçleri elde etmek için kullanılan normal yöntemidir.
Sonuç olarak, `AcquireTokenSilentAsync` başarısız olur – örneğin kullanıcı oturumunuz veya başka bir aygıtta parolalarını değişti. MSAL algıladığında, etkileşimli bir eylem gerektirmeyen tarafından sorunu çözmek için harekete bir `MsalUiRequiredException`. Uygulamanız bu özel durumun iki yolla işleyebilir:

1.  Karşı çağırmaya `AcquireTokenAsync` hemen hangi sonuçları oturum açma kullanıcıdan içinde. Bu deseni genelde çevrimiçi uygulamalarda kullanılır söz konusu olduğunda çevrimdışı içerik uygulamada kullanıcı için kullanılabilir. Destekli Bu kurulum tarafından oluşturulan örnek bu deseni kullanır: örnek yürütme eylemi ilk zamanında görebilirsiniz: hiçbir kullanıcı, uygulamayı her zamankinden kullanıldığından `PublicClientApp.Users.FirstOrDefault()` bir null değer içerir ve bir `MsalUiRequiredException` özel durum. Ardından örnek kodda çağırarak özel durumu işler `AcquireTokenAsync` oturum açma kullanıcıdan içinde sonuçlanır.

2.  Uygulamalar bir etkileşimli oturum açma kullanıcı oturum açmak için doğru zamanı seçebilir ya da uygulama deneyebilirsiniz gerekli olan kullanıcı için görsel bir gösterge de yapabilirsiniz `AcquireTokenSilentAsync` daha sonra. Bu genellikle kullanılan kullanıcı kesintiye olmadan diğer uygulamanın işlevselliğini kullanabilir - Örneğin, çevrimdışı içeriği uygulamada kullanılabilir olduğunda. Bu durumda, kullanıcı ne zaman korumalı kaynağa erişmek için ya da eski bilgileri yenilemek için oturum açmak istedikleri veya uygulamanızın denemeye karar verebilirsiniz karar verebilir `AcquireTokenSilentAsync` ağ zaman geri geçici olarak kullanılamıyor kaldıktan sonra.
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>Yalnızca edinilen belirteçle kullanarak Microsoft Graph API çağrısı

1. Aşağıda yeni yöntemine ekleyin, `MainWindow.xaml.cs`. Yöntem yapmak için kullanılan bir `GET` bir Authorize üstbilgisi kullanarak grafik API'sine isteği:

```csharp
/// <summary>
/// Perform an HTTP GET request to a URL using an HTTP Authorization header
/// </summary>
/// <param name="url">The URL</param>
/// <param name="token">The token</param>
/// <returns>String containing the results of the GET operation</returns>
public async Task<string> GetHttpContentWithToken(string url, string token)
{
    var httpClient = new System.Net.Http.HttpClient();
    System.Net.Http.HttpResponseMessage response;
    try
    {
        var request = new System.Net.Http.HttpRequestMessage(System.Net.Http.HttpMethod.Get, url);
        //Add the token in Authorization header
        request.Headers.Authorization = new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", token);
        response = await httpClient.SendAsync(request);
        var content = await response.Content.ReadAsStringAsync();
        return content;
    }
    catch (Exception ex)
    {
        return ex.ToString();
    }
}
```
<!--start-collapse-->
### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>Karşı korumalı bir API REST çağrısı yapma hakkında daha fazla bilgi

Bu örnek uygulamasında `GetHttpContentWithToken` yöntemi bir HTTP yapmak için kullanılan `GET` isteği için bir belirteç gerekiyor korunan bir kaynağa karşı ve ardından içeriği çağırana dönün. Bu yöntem alınan belirteç ekler *HTTP Authorization Üstbilgisi*. Bu örnek için Microsoft Graph API kaynaktır *bana* endpoint – kullanıcı profili bilgilerini görüntüler.
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>Kullanıcı imzalamak için bir yöntem ekleyin

1. Aşağıdaki yöntemi ekleyin, `MainWindow.xaml.cs` kullanıcı imzalamak için:

```csharp
/// <summary>
/// Sign out the current user
/// </summary>
private void SignOutButton_Click(object sender, RoutedEventArgs e)
{
    if (App.PublicClientApp.Users.Any())
    {
        try
        {
            App.PublicClientApp.Remove(App.PublicClientApp.Users.FirstOrDefault());
            this.ResultText.Text = "User has signed-out";
            this.CallGraphButton.Visibility = Visibility.Visible;
            this.SignOutButton.Visibility = Visibility.Collapsed;
        }
        catch (MsalException ex)
        {
            ResultText.Text = $"Error signing-out user: {ex.Message}";
        }
    }
}
```
<!--start-collapse-->
### <a name="more-info-on-sign-out"></a>Oturum kapatma hakkında daha fazla bilgi

`SignOutButton_Click`Kullanıcı etkileşimli olarak yapılırsa bir belirteç almak için gelecekteki bir isteği yalnızca başarılı şekilde geçerli kullanıcının unuttunuz için bu MSAL etkili bir şekilde söyler MSAL kullanıcı önbellekten – kaldırır.
Bu örnek uygulamasında tek bir kullanıcı desteklese de MSAL nerede birden fazla hesap oturum açmış aynı anda – bir e-posta uygulamasının bir kullanıcı birden fazla hesap sahip olduğu örneğidir senaryolarını destekler.
<!--end-collapse-->

## <a name="display-basic-token-information"></a>Temel belirteci bilgilerini görüntüle

1. İçin aşağıdaki yöntemi ekleyin, `MainWindow.xaml.cs` belirteci ile ilgili temel bilgileri görüntülemek için:

```csharp
/// <summary>
/// Display basic information contained in the token
/// </summary>
private void DisplayBasicTokenInfo(AuthenticationResult authResult)
{
    TokenInfoText.Text = "";
    if (authResult != null)
    {
        TokenInfoText.Text += $"Name: {authResult.User.Name}" + Environment.NewLine;
        TokenInfoText.Text += $"Username: {authResult.User.DisplayableId}" + Environment.NewLine;
        TokenInfoText.Text += $"Token Expires: {authResult.ExpiresOn.ToLocalTime()}" + Environment.NewLine;
        TokenInfoText.Text += $"Access Token: {authResult.AccessToken}" + Environment.NewLine;
    }
}
```
<!--start-collapse-->
### <a name="more-information"></a>Daha Fazla Bilgi

Belirteçleri edinilen aracılığıyla *Openıd Connect* küçük bir alt kullanıcıya ilgili bilgileri de içerir. `DisplayBasicTokenInfo`belirtecinde yer alan temel bilgileri görüntüler: Örneğin, kullanıcının görünen adı ve kimliği için belirteç süre sonu tarihi ve erişim temsil eden dize yanı sıra kendi simge. Bu bilgileri görmek için görüntülenir. İsabet *Microsoft Graph API çağrısı* birden çok kez düğmesine tıklayın ve aynı belirtecin sonraki istekleri için yeniden kullanılmış bakın. MSAL onu kapılarını açtığında genişletilen belirteci yenileme süresi sona erme tarihi de görebilirsiniz.
<!--end-collapse-->

