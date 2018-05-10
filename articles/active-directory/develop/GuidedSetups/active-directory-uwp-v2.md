---
title: Azure AD v2 UWP Başlarken | Microsoft Docs
description: Evrensel Windows Platformu (XAML) uygulamaları, Azure Active Directory v2 bitiş noktası tarafından erişim belirteçleri gerektiren bir API nasıl çağırabilirsiniz
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/20/2018
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 390559922b3b8fb293d1c8b38f36dfd0a1df9ebd
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="call-the-microsoft-graph-api-from-a-universal-windows-platform-uwp-application"></a>Microsoft Graph API çağrısından bir evrensel Windows Platformu (UWP) uygulaması

Bu kılavuz, nasıl yerel bir evrensel Windows Platformu (XAML) uygulama erişim belirteci almak ve Microsoft Graph API veya Azure Active Directory v2 uç noktasından erişim belirteçleri gerektiren diğer API'leri çağırmak için bu erişim belirtecini kullanır gösterir.

Bu kılavuzun sonunda uygulamanız (outlook.com, live.com ve diğerleri dahil) kişisel hesapları hem de iş kullanan korumalı bir API çağrısı ve Okul hesapları herhangi bir şirket veya Azure Active Directory sahip kuruluş mümkün olacaktır.  

> Bu kılavuz ile Evrensel Windows platformu geliştirme yüklü Visual Studio 2017 gerektirir. Bu kontrol [makale](https://docs.microsoft.com/windows/uwp/get-started/get-set-up "UWP için Visual Studio'yu ayarlama") Evrensel Windows platformu uygulamaları geliştirmek karşıdan yükleyip Visual Studio'yu yapılandırma konusunda yönergeler için.

### <a name="how-this-guide-works"></a>Bu kılavuz nasıl çalışır?

![Bu kılavuz nasıl çalışır?](media/active-directory-mobileanddesktopapp-windowsuniversalplatform-introduction/uwp-intro.png)

Bu kılavuz tarafından oluşturulan örnek uygulamayı Microsoft Graph API veya Azure Active Directory v2 uç noktasından belirteçleri kabul eder bir Web API sorgulamak için bir UWP uygulaması sağlar. Bu senaryoda, HTTP isteklerini Authorization Üstbilgisi yoluyla bir belirteç eklenir. Belirteç satın almalar ve yenilemeleri Microsoft kimlik doğrulama kitaplığı (MSAL) tarafından işlenir.

### <a name="nuget-packages"></a>NuGet paketleri

Bu kılavuz aşağıdaki NuGet paketlerini kullanır:

|Kitaplık|Açıklama|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft kimlik doğrulama kitaplığı (MSAL)|


## <a name="set-up-your-project"></a>Projenizin kurulumunu

Bu bölümde bir Windows Masaüstü .NET uygulaması (XAML) tümleştirileceği hakkında adım adım yönergeler sağlar *Microsoft ile oturum açma* şekilde Microsoft grafik API'si gibi bir belirteç gerektiren Web API'leri sorgulayabilirsiniz.

Bu kılavuz tarafından oluşturulan uygulama grafik API'si, oturum kapatma düğmesini ve çağrıları sonuçlarını görüntülemek metin kutularına sorgulamak için kullanılan bir düğme görüntüler.

> Bunun yerine bu örneği ait Visual Studio projesi indirmeyi tercih ediyorsunuz? [Bir proje indirme](https://github.com/Azure-Samples/active-directory-dotnet-native-uwp-v2/archive/master.zip) ve geçin [uygulama kaydı](#register-your-application "uygulama kayıt adımından") kod örneği çalıştırmadan önce yapılandırmak için adım.


### <a name="create-your-application"></a>Uygulamanızı oluşturun
1. Visual Studio içinde: **dosya** > **yeni** > **proje**<br/>
2. Altında *şablonları*seçin **Visual C#**
3. Seçin **boş uygulama (Evrensel Windows)**
4. Bir ad verin ve 'Tamam' düğmesini tıklatın.
5. İstenirse, herhangi bir sürümünü seçmek ücretsiz altına düştü *hedef* ve *Minimum* sürümü ve 'Tamam' düğmesini tıklatın:<br/><br/>![En az ve hedef sürümleri](media/active-directory-uwp-v2.md/vs-minimum-target.png)

## <a name="add-the-microsoft-authentication-library-msal-to-your-project"></a>Microsoft kimlik doğrulama kitaplığı (MSAL) projenize ekleyin
1. Visual Studio içinde: **Araçları** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**
2. Paket Yöneticisi konsol penceresinde aşağıdaki komutu kopyalayıp yapıştırın:

    ```powershell
    Install-Package Microsoft.Identity.Client -Pre
    ```

> [!NOTE]
> Paketi yükler yukarıda [Microsoft kimlik doğrulama kitaplığı (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet). MSAL alınırken önbelleğe alma ve Azure Active Directory v2 tarafından korunan API'leri erişmek için kullanılan kullanıcı belirteçleri yenilemeyi işler.

## <a name="initialize-msal"></a>MSAL başlatma
Bu adım, MSAL kitaplığı ile etkileşim belirteçleri işleme gibi işlemek için bir sınıf oluşturmanıza yardımcı olur.

1. Açık **App.xaml.cs** dosya ve MSAL Kitaplığı Başvurusu sınıfına ekleyin:

    ```csharp
    using Microsoft.Identity.Client;
    ```

2. Uygulamanın sınıfına aşağıdaki iki satırı ekleyin (içinde <code>sealed partial class App : Application</code> bloğu):

    ```csharp
    // Below is the clientId of your app registration. 
    // You have to replace the below with the Application Id for your app registration
    private static string ClientId = "your_client_id_here";
    
    public static PublicClientApplication PublicClientApp = new PublicClientApplication(ClientId);
    ```

## <a name="create-your-applications-ui"></a>Uygulamanızın kullanıcı Arabirimi oluşturma

A **MainPage.xaml** dosya proje şablonu bir parçası olarak otomatik olarak oluşturulmalıdır. Bu dosyayı açın ve ardından yönergeleri izleyin:

1.  Uygulamanızın Değiştir **<Grid>** düğümle:

    ```xml
    <Grid>
        <StackPanel Background="Azure">
            <StackPanel Orientation="Horizontal" HorizontalAlignment="Right">
                <Button x:Name="CallGraphButton" Content="Call Microsoft Graph API" HorizontalAlignment="Right" Padding="5" Click="CallGraphButton_Click" Margin="5" FontFamily="Segoe Ui"/>
                <Button x:Name="SignOutButton" Content="Sign-Out" HorizontalAlignment="Right" Padding="5" Click="SignOutButton_Click" Margin="5" Visibility="Collapsed" FontFamily="Segoe Ui"/>
            </StackPanel>
            <TextBlock Text="API Call Results" Margin="2,0,0,-5" FontFamily="Segoe Ui" />
            <TextBox x:Name="ResultText" TextWrapping="Wrap" MinHeight="120" Margin="5" FontFamily="Segoe Ui"/>
            <TextBlock Text="Token Info" Margin="2,0,0,-5" FontFamily="Segoe Ui" />
            <TextBox x:Name="TokenInfoText" TextWrapping="Wrap" MinHeight="70" Margin="5" FontFamily="Segoe Ui"/>
        </StackPanel>
    </Grid>
    ```
    
## <a name="use-the-microsoft-authentication-library-msal-to-get-a-token-for-the-microsoft-graph-api"></a>Microsoft Graph API için bir belirteç almak üzere Microsoft kimlik doğrulama kitaplığı (MSAL) kullanın

Bu bölümde Microsoft Graph API'si için bir belirteç almak üzere MSAL kullanmayı gösterir.

1.  İçinde **MainPage.xaml.cs**, MSAL Kitaplığı Başvurusu sınıfına ekleyin:

    ```csharp
    using Microsoft.Identity.Client;
    ```
2. Kodunu değiştirmek, <code>MainPage</code> ile sınıfı:

    ```csharp
    public sealed partial class MainPage : Page
    {
        // Set the API Endpoint to Graph 'me' endpoint
        string graphAPIEndpoint = "https://graph.microsoft.com/v1.0/me";
    
        // Set the scope for API call to user.read
        string[] scopes = new string[] { "user.read" };
    
        public MainPage()
        {
            this.InitializeComponent();
        }
    
        /// <summary>
        /// Call AcquireTokenAsync - to acquire a token requiring user to sign-in
        /// </summary>
        private async void CallGraphButton_Click(object sender, RoutedEventArgs e)
        {
            AuthenticationResult authResult = null;
            ResultText.Text = string.Empty;
            TokenInfoText.Text = string.Empty;
    
            try
            {
                authResult = await App.PublicClientApp.AcquireTokenSilentAsync(scopes, App.PublicClientApp.Users.FirstOrDefault());
            }
            catch (MsalUiRequiredException ex)
            {
                // A MsalUiRequiredException happened on AcquireTokenSilentAsync. This indicates you need to call AcquireTokenAsync to acquire a token
                System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");
    
                try
                {
                    authResult = await App.PublicClientApp.AcquireTokenAsync(scopes);
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
                ResultText.Text = await GetHttpContentWithToken(graphAPIEndpoint, authResult.AccessToken);
                DisplayBasicTokenInfo(authResult);
                this.SignOutButton.Visibility = Visibility.Visible;
            }
        }
    }
    ```

### <a name="more-information"></a>Daha Fazla Bilgi
#### <a name="get-a-user-token-interactively"></a>Kullanıcı etkileşimli olarak belirteci alma
Çağırma `AcquireTokenAsync` yöntemi, kullanıcının oturum açmasına izin isteyen bir pencere sonuçlanıyor. Uygulamalar genellikle etkileşimli olarak korunan bir kaynağa erişmek için ihtiyaç duydukları ilk kez oturum açmalarını gerektirir. (Örneğin, bir kullanıcının parolasının süresi sona erdiğinde) bir belirtecini almak üzere sessiz bir işlemi başarısız olduğunda oturum açmak de gerekebilir.

#### <a name="get-a-user-token-silently"></a>Bir kullanıcı sessizce belirteci alma
`AcquireTokenSilentAsync` Yöntemi belirteci satın almalar ve herhangi bir kullanıcı etkileşimi olmadan yenilemeleri işler. Sonra `AcquireTokenAsync` ilk kez yürütüldüğünde `AcquireTokenSilentAsync` veya belirteçleri yenileme isteği için çağrıları sessizce yapmış sonraki çağrılar için korunan kaynaklara erişim belirteçleri almak için kullanılacak normal bir yöntemdir.

Sonuç olarak, `AcquireTokenSilentAsync` yöntemi başarısız olur. Hatanın nedenlerini kullanıcı oturumunuz veya başka bir cihazda kendi parolanın değiştirilmesi emin olabilir. MSAL algıladığında, etkileşimli bir eylem gerektirmeyen tarafından sorunu çözmek için harekete bir `MsalUiRequiredException` özel durum. Uygulamanız bu özel durumun iki yolla işleyebilir:

* Karşı arama yapabilmeniz için `AcquireTokenAsync` hemen. Bu çağrı, oturum açmak için kullanıcıdan içinde sonuçlanır. Bu desen normalde çevrimiçi uygulamalarda kullanılır kullanıcı için kullanılabilir çevrimdışı içerik olduğu. Bu Destekli kurulum tarafından oluşturulan örnek, örnek yürütme eylemi ilk zamanında görebilirsiniz bu deseni takip eder. 
    * Hiçbir kullanıcı, uygulamayı kullanıldığından `PublicClientApp.Users.FirstOrDefault()` bir null değer içeriyor ve bir `MsalUiRequiredException` özel durumu oluşur. 
    * Ardından örnek kodda çağırarak özel durumu işler `AcquireTokenAsync`, oturum açmak için kullanıcıdan içinde sonuçlanır.

* Bunun yerine oturum açmak için doğru zamanı seçebilmeniz için bir etkileşimli oturum açma gerekli olan kullanıcılar için görsel bir gösterge sunabilir. Veya uygulamayı yeniden deneyebilirsiniz `AcquireTokenSilentAsync` daha sonra. Çevrimdışı içeriği uygulamada kullanılabilir olduğunda kullanıcıların diğer uygulama işlevselliği kesintiye uğratmadan--Örneğin, ne zaman kullanabileceğini bu deseni sıkça kullanılır. Bu durumda, kullanıcıların korunan bir kaynağa erişmek veya eski bilgileri yenilemek oturum açmak istedikleri zaman karar verebilirsiniz. Yeniden denemek alternatif olarak, uygulama karar `AcquireTokenSilentAsync` zaman ağ geri geçici olarak devre dışı bırakıldı sonra.

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>Yalnızca edinilen belirteçle kullanarak Microsoft Graph API çağrısı

1. Aşağıdaki yeni yöntemine ekleyin, **MainPage.xaml.cs**. Yöntem yapmak için kullanılan bir `GET` bir Authorize üstbilgisi kullanarak grafik API'sine isteği:

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
            // Add the token in Authorization header
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

### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>Karşı korumalı bir API REST çağrısı yapma hakkında daha fazla bilgi

Bu örnek uygulamasında `GetHttpContentWithToken` yöntemi bir HTTP yapmak için kullanılan `GET` isteği için bir belirteç gerekiyor korunan bir kaynağa karşı ve ardından içeriği çağırana dönün. Bu yöntem alınan belirteç ekler *HTTP Authorization Üstbilgisi*. Bu örnek için Microsoft Graph API kaynaktır *bana* endpoint – kullanıcı profili bilgilerini görüntüler.
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>Kullanıcı imzalamak için bir yöntem ekleyin

1. Kullanıcı imzalamak için aşağıdaki yöntemi ekleyin **MainPage.xaml.cs**:

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

### <a name="more-info-on-sign-out"></a>Oturum kapatma hakkında daha fazla bilgi

Yöntem `SignOutButton_Click` kullanıcı MSAL kullanıcı önbellekten – bu MSAL etkili bir şekilde söyler etkileşimli olarak yapılırsa bir belirteç almak için gelecekteki bir isteği yalnızca başarılı şekilde geçerli kullanıcının unuttunuz için kaldırır.
Bu örnek uygulamasında tek bir kullanıcı desteklese de MSAL nerede birden fazla hesap oturum açmış aynı anda – bir e-posta uygulamasının bir kullanıcı birden fazla hesap sahip olduğu örneğidir senaryolarını destekler.

## <a name="display-basic-token-information"></a>Temel belirteci bilgilerini görüntüle

1. Aşağıdaki yöntemi ekleyin, **MainPage.xaml.cs** belirteci ile ilgili temel bilgileri görüntülemek için:

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

### <a name="more-information"></a>Daha Fazla Bilgi

Kimlik belirteçlerini edinilen aracılığıyla *Openıd Connect* küçük bir alt kullanıcıya ilgili bilgileri de içerir. `DisplayBasicTokenInfo` belirtecinde yer alan temel bilgileri görüntüler: Örneğin, kullanıcının görünen adı ve kimliği için belirteç süre sonu tarihi ve erişim temsil eden dize yanı sıra kendi simge. Bu bilgileri görmek için görüntülenir. İsabet **Microsoft Graph API çağrısı** birden çok kez düğmesine tıklayın ve aynı belirtecin sonraki istekleri için yeniden kullanılmış bakın. MSAL onu kapılarını açtığında genişletilen belirteci yenileme süresi sona erme tarihi de görebilirsiniz.

## <a name="register-your-application"></a>Uygulamanızı kaydetme

Uygulamanızı kaydetmeniz gerekir artık *Microsoft uygulama kayıt portalı*:
1. Git [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app) bir uygulamayı kaydetmek için
2. Uygulamanız için bir ad girin 
3. Destekli kurulumu için seçeneğinin işaretli olduğundan emin olun
4. Tıklatın **eklemek platformları**seçeneğini belirleyip **yerel uygulama** ve kaydetme ulaştı.
5. Uygulama kimliği GUID kopyalayın, Visual Studio, açık dön **App.xaml.cs** ve değiştirme `your_client_id_here` yalnızca kayıtlı uygulama kimliği:

    ```csharp
    private static string ClientId = "your_application_id_here";
    ```

## <a name="enable-integrated-authentication-on-federated-domains-optional"></a>Federasyon etki alanı (isteğe bağlı) tümleşik kimlik doğrulamasını etkinleştirin

Windows tümleşik federe bir Azure Active Directory etki alanı ile kullanıldığında kimlik doğrulamasını etkinleştirmek için uygulama bildirimi ek özellikler etkinleştirmeniz gerekir:

1. Çift **Package.appxmanifest**
2. Seçin **yetenekleri** sekmesinde ve aşağıdaki ayarların etkin olduğundan emin olun:

    - Kurumsal kimlik doğrulama
    - Özel ağlar (istemci ve sunucu)
    - Paylaşılan kullanıcı sertifikaları 

3. Ardından, açın **App.xaml.cs**ve bir uygulama Oluşturucu aşağıdaki satırı ekleyin:

    ```csharp
    App.PublicClientApp.UseCorporateNetwork = true;
    ```

> [!IMPORTANT]
> Windows tümleşik kimlik doğrulaması yapılandırılamadı Bu örnek için varsayılan olarak çünkü isteyen uygulamaları *Kurumsal kimlik doğrulama* veya *paylaşılan kullanıcı sertifikaları* özellikleri gerektiren bir daha yüksek düzeyde doğrulama Windows mağazası ve tüm geliştiriciler tarafından daha yüksek düzeyde doğrulama gerçekleştirmek istediğiniz. Lütfen yalnızca bir Federasyon Azure Active Directory etki alanı ile Windows tümleşik kimlik doğrulaması gerekiyorsa bu ayarı etkinleştirin.


## <a name="test-your-code"></a>Kodunuzu test

Uygulamanızı test etmek için basın `F5` Visual Studio'da projeyi çalıştırın. Ana penceresi görünmelidir:

![Uygulamanın kullanıcı arabirimi](media/active-directory-uwp-v2.md/testapp-ui.png)

Test etmek hazır olduğunuzda, tıklatın *Microsoft Graph API çağrısı* ve oturum açmak için bir Microsoft Azure Active Directory (kuruluş hesabı) veya bir Microsoft Account (live.com, outlook.com) hesabı kullanın. Bunu, ilk kez kullanıyorsanız, oturum açmak için kullanıcının isteyen bir pencere görürsünüz:

![Oturum açma sayfası](media/active-directory-uwp-v2.md/sign-in-page.png)

### <a name="consent"></a>Onay
İlk uygulamanıza oturum açtığınızda, aşağıdakine benzer bir onay ekranı burada açıkça kabul etmeniz gerekir sunulur:

![Onay ekranı](media/active-directory-uwp-v2.md/consentscreen.png)
### <a name="expected-results"></a>Beklenen sonuçları
Kullanıcı profili bilgilerini API çağrısı sonuçları ekranda Microsoft Graph API çağrısı tarafından döndürülen görmeniz gerekir:

![Sonuçları ekranı](media/active-directory-uwp-v2.md/uwp-results-screen.PNG)

Aracılığıyla edinilen belirteci hakkında temel bilgiler görmeniz gerekir `AcquireTokenAsync` veya `AcquireTokenSilentAsync` belirteci bilgisi kutusunda:

|Özellik  |Biçimlendir  |Açıklama |
|---------|---------|---------|
|**Ad** |Kullanıcının tam adı |Kullanıcı adı ve soyadı.|
|**Kullanıcı Adı** |<span>user@domain.com</span> |Kullanıcıyı tanımlamak için kullanılan kullanıcı adı.|
|**Belirtecinin süresi** |DateTime |Belirtecin süresinin dolma zamanı. MSAL belirteci gerekli olarak yenileyerek sona erme tarihini genişletir.|
|**Erişim belirteci** |Dize |HTTP gönderilen belirteç dizesini gerektiren ister bir *Authorization Üstbilgisi*.|

#### <a name="see-what-is-in-the-access-token-optional"></a>(İsteğe bağlı) erişim belirteci bakın
İsteğe bağlı olarak, 'Erişim belirteci' değerini kopyalayın ve yapıştırın https://jwt.ms çözülmesi ve talep listesine bakın.

### <a name="more-information-about-scopes-and-delegated-permissions"></a>Kapsamlar ve temsilci izinleri hakkında daha fazla bilgi

Microsoft Graph API gerektiriyor *user.read* kullanıcı profilini okuma için kapsamı. Bu kapsam, varsayılan olarak uygulama kayıt Portalı'nda kayıtlı her uygulama otomatik olarak eklenir. Diğer Microsoft Graph için API'leri yanı sıra, arka uç sunucusu için özel API'leri ek kapsamlar gerektirebilir. Microsoft Graph API gerektiriyor *Calendars.Read* kullanıcının takvimleri listelemek için kapsam.

Bir uygulama bağlamında kullanıcının takvimleri erişmek için eklemeniz *Calendars.Read* izin uygulama kayıt bilgileri için temsilci. Ardından, ekleyin *Calendars.Read* için kapsam `acquireTokenSilent` çağırın. 

> [!NOTE]
> Kapsam sayısı arttıkça, kullanıcı için ek onayları istenebilir.

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="issue-1"></a>Sorun 1:
Uygulamanızdaki bir Federasyon Azure Active Directory etki alanı üzerinde oturum açma, aşağıdaki hatalardan biri alabilirsiniz:
 - Geçerli istemci sertifikası isteğinde bulundu.
 - Geçerli sertifikası kullanıcının sertifika deposunda bulunamadı.
 - Farklı kimlik doğrulama yöntemini tekrar seçmeyi deneyin.

**Neden:** Enterprise ve sertifikaları yetenekleri etkin değil

**Çözüm:** adımları [tümleşik kimlik doğrulaması Federasyon etki alanları](#enable-integrated-authentication-on-federated-domains-optional)

### <a name="issue-2"></a>Sorun 2:
Etkinleştirdikten sonra [tümleşik kimlik doğrulaması Federasyon etki alanı üzerinde](#enable-integrated-authentication-on-federated-domains-optional) ve Windows Hello bir Windows 10 bilgisayarında bir ortamda çok / multi-Factor-yapılandırılmış authentication ile oturum açmak için kullandığınız çalışırsanız, sertifika listesi sunulur, ancak PIN'İNİZİ kullanmayı seçerseniz, PIN penceresi hiçbir zaman sunulur.

**Neden:** Web kimlik doğrulama Aracısı Windows 10 Masaüstü (Windows 10 Mobile ince çalışır) üzerinde çalışan UWP uygulamalarında sınırlamasını bilinen

**Geçici çözüm:** kullanıcılar gereken diğer seçenekleri oturum açın ve ardından için seçilecek *bir kullanıcı adı ve parola ile oturum açma* bunun yerine, select parolanızı girin ve telefon kimlik doğrulaması gidin.

