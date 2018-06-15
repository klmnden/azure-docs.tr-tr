---
title: Azure AD v2 UWP Başlarken | Microsoft Docs
description: Evrensel Windows platformu uygulamaları (UWP) Azure Active Directory v2 bitiş noktası tarafından erişim belirteçleri gerektiren bir API nasıl çağırabilirsiniz
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
ms.openlocfilehash: c2d5681e30651aac7a09a8ead923015e9a892d42
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34796623"
---
# <a name="call-microsoft-graph-api-from-a-universal-windows-platform-application-xaml"></a>Evrensel Windows platformu uygulamasından (XAML) Microsoft Graph API çağırma

Bu kılavuz, nasıl bir yerel Evrensel Windows Platformu (UWP) uygulaması bir erişim belirteci istemek ve Microsoft Graph API çağrısı açıklar. Kılavuzu erişim belirteçleri Azure Active Directory v2 uç noktasından gerektiren diğer API'leri için de geçerlidir.

Bu kılavuzun sonunda Kişisel hesapları kullanarak uygulamanızı korumalı bir API çağırır. Örnekler, outlook.com, live.com ve diğerleri ' dir. Uygulamanız herhangi bir şirket veya Azure Active Directory sahip kuruluş iş ve Okul hesapları de çağırır.

>[!NOTE]
> Bu kılavuz ile Evrensel Windows platformu geliştirme yüklü Visual Studio 2017 gerektirir. Bkz: [ayarlanmasını](https://docs.microsoft.com/windows/uwp/get-started/get-set-up) indirin ve evrensel Windows platformu uygulamaları geliştirmek için Visual Studio'yu yapılandırma hakkında yönergeler için.

### <a name="how-this-guide-works"></a>Bu kılavuz nasıl çalışır?

![Bu kılavuz grafik nasıl çalışır?](media/active-directory-mobileanddesktopapp-windowsuniversalplatform-introduction/uwp-intro.png)

Bu kılavuz, Microsoft grafik API'si sorguları bir örnek UWP uygulaması veya Azure Active Directory v2 uç noktasından belirteçleri kabul eder bir Web API oluşturur. Bu senaryoda, HTTP isteklerini Authorization Üstbilgisi yoluyla bir belirteç eklenir. Microsoft kimlik doğrulama kitaplığı (MSAL) belirteci satın almalar ve yenilemeleri işler.

### <a name="nuget-packages"></a>NuGet paketleri

Bu kılavuz aşağıdaki NuGet paketlerini kullanır:

|Kitaplık|Açıklama|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft kimlik doğrulama kitaplığı|


## <a name="set-up-your-project"></a>Projenizin kurulumunu

Bu bölümde bir Windows Masaüstü .NET uygulaması (XAML) tümleştirmek için adım adım yönergeler sağlar *Microsoft ile oturum açma*. Ardından, Microsoft grafik API'si gibi bir belirteç gerektiren Web API'leri sorgulayabilirsiniz.

Bu kılavuz, bu sorguları grafik API'si, oturum kapatma düğmesini ve çağrıları sonuçlarını görüntülemek metin kutuları bir düğme görüntüleyen bir uygulama oluşturur.

>[!NOTE]
> Bunun yerine bu örnekte 's Visual Studio projesi indirme istiyor musunuz? [Bir proje indirme](https://github.com/Azure-Samples/active-directory-dotnet-native-uwp-v2/archive/master.zip) ve geçin [uygulama kaydı](#register-your-application "uygulama kayıt adımından") adım çalıştırılmadan önce kod örneği yapılandırmak için.


### <a name="create-your-application"></a>Uygulamanızı oluşturun
1. Visual Studio'da seçin **dosya** > **yeni** > **proje**.
2. Altında **şablonları**seçin **Visual C#**.
3. **Boş Uygulama (Evrensel Windows)** seçeneğini belirleyin.
4. Uygulama adını ve seçin **Tamam**.
5. İstenirse, herhangi bir sürümünü seçin **hedef** ve **Minimum** sürümleri ve select **Tamam**.

    >![En az ve hedef sürümleri](media/active-directory-uwp-v2.md/vs-minimum-target.png)

## <a name="add-microsoft-authentication-library-to-your-project"></a>Microsoft kimlik doğrulama kitaplığını projenize ekleme
1. Visual Studio'da seçin **Araçları** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.
2. Aşağıdaki komutu kopyalayıp **Paket Yöneticisi Konsolu** penceresi:

    ```powershell
    Install-Package Microsoft.Identity.Client -Pre
    ```

> [!NOTE]
> Bu komut yükler [Microsoft kimlik doğrulama Kitaplığı](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet). MSAL edinir, önbelleğe alır ve Azure Active Directory v2 tarafından korunan API'lere erişim kullanıcı belirteçleri yeniler.

## <a name="initialize-msal"></a>MSAL başlatma
Bu adım, belirteçleri işleme gibi MSAL, etkileşim işlemek için bir sınıf oluşturmanıza yardımcı olur.

1. Açık **App.xaml.cs** dosya ve başvuru için MSAL sınıfına ekleyin:

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

A **MainPage.xaml** dosya proje şablonu bir parçası olarak otomatik olarak oluşturulur. Bu dosyayı açın ve ardından yönergeleri izleyin:

* Uygulamanızın Değiştir **kılavuz** aşağıdaki kodla düğümü:

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
    
## <a name="use-msal-to-get-a-token-for-microsoft-graph-api"></a>Microsoft Graph API için bir belirteç almak üzere MSAL kullanın

Bu bölümde MSAL Microsoft Graph API için bir belirteç almak için nasıl kullanılacağını gösterir.

1.  İçinde **MainPage.xaml.cs**, başvuru için MSAL sınıfına ekleyin:

    ```csharp
    using Microsoft.Identity.Client;
    ```
2. Kodunu değiştirmek, <code>MainPage</code> aşağıdaki kodla sınıfı:

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

### <a name="more-information"></a>Daha fazla bilgi
#### <a name="get-a-user-token-interactively"></a>Kullanıcı etkileşimli olarak belirteci alma
Çağrı `AcquireTokenAsync` yöntemi, kullanıcının oturum açmasına izin isteyen bir pencere sonuçlanıyor. Uygulamalar genellikle etkileşimli olarak korunan bir kaynağa erişmek için ihtiyaç duydukları ilk kez oturum açmalarını gerektirir. Bir belirteç almak üzere sessiz bir işlemi başarısız olduğunda oturum açmak de gerekebilir. Bir kullanıcının parolasının süresi sona erdiğinde bir örnektir.

#### <a name="get-a-user-token-silently"></a>Bir kullanıcı sessizce belirteci alma
`AcquireTokenSilentAsync` Yöntemi belirteci satın almalar ve herhangi bir kullanıcı etkileşimi olmadan yenilemeleri işler. Sonra `AcquireTokenAsync` ilk kez yürütüldüğünde ve kullanıcı kimlik bilgileri sorulur `AcquireTokenSilentAsync` yöntemi, bunu almak için belirteçleri sessizce sonraki çağrılar için belirteçler istemek için kullanılmalıdır. MSAL belirteç önbelleği ve yenileme işleyecek.

Sonuç olarak, `AcquireTokenSilentAsync` yöntemi başarısız olur. Hatanın nedenlerini, kullanıcılar ya da oturumu kapattınız veya başka bir cihazda kendi parolanın değiştirilmesi olması olabilir. MSAL algıladığında, etkileşimli bir eylem gerektirmeyen tarafından sorunu çözmek için harekete bir `MsalUiRequiredException` özel durum. Uygulamanız bu özel durumun iki yolla işleyebilir:

* Karşı arama yapabilmeniz için `AcquireTokenAsync` hemen. Bu çağrı, oturum açmak için kullanıcıdan içinde sonuçlanır. Normalde, çevrimiçi uygulamalarında bu deseni kullanılır söz konusu olduğunda kullanıcı için kullanılabilir çevrimdışı içerik. Bu Destekli kurulum tarafından oluşturulan örnek deseni izler. Bu örneği çalıştırmadan eylem ilk zamanında bakın. 
    * Hiçbir kullanıcı, uygulamayı kullanıldığından `PublicClientApp.Users.FirstOrDefault()` bir null değer içeriyor ve bir `MsalUiRequiredException` özel durumu oluşur.
    * Ardından örnek kodda çağırarak özel durumu işler `AcquireTokenAsync`. Bu çağrı, oturum açmak için kullanıcıdan içinde sonuçlanır.

* Veya bunun yerine, etkileşimli oturum açma gerekli olduğunu kullanıcılara görsel bir gösterge gösterir. Daha sonra oturum açmak için doğru zamanı seçebilirsiniz. Veya uygulamayı yeniden deneyebilirsiniz `AcquireTokenSilentAsync` daha sonra. Genellikle, kullanıcıların diğer uygulama işlevselliğini kesintiye uğratmadan kullanabilirsiniz Bu deseni kullanılır. Çevrimdışı içeriği uygulamada kullanılabilir olduğunda bir örnektir. Bu durumda, kullanıcıların korunan bir kaynağa erişmek veya eski bilgileri yenilemek oturum açmak istedikleri zaman karar verebilirsiniz. Ya da yeniden denemek başka uygulama karar `AcquireTokenSilentAsync` zaman ağ geri sonra geçici olarak kullanılamıyor.

## <a name="call-microsoft-graph-api-by-using-the-token-you-just-obtained"></a>Yalnızca edinilen belirteçle kullanarak Microsoft Graph API çağırma

* Aşağıdaki yeni yöntemine ekleyin **MainPage.xaml.cs**. Bu yöntem yapmak için kullanılan bir `GET` grafik API'sine karşı bir [Authorize] üstbilgisi kullanarak isteği:

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

Bu örnek uygulamasında `GetHttpContentWithToken` yöntemi bir HTTP yapmak için kullanılan `GET` isteği için bir belirteç gerekiyor korunan bir kaynağa göre. Ardından yöntemi içeriği çağırana döndürür. Bu yöntem alınan belirteç ekler **HTTP yetkilendirme** üstbilgi. Bu örnek için Microsoft Graph API kaynaktır **bana** uç noktası kullanıcı profili bilgilerini görüntüler.
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>Kullanıcı imzalamak için bir yöntem ekleyin

* Kullanıcı imzalamak için aşağıdaki yöntemi ekleyin **MainPage.xaml.cs**:

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

### <a name="more-information-on-sign-out"></a>Oturum kapatma hakkında daha fazla bilgi

`SignOutButton_Click` Yöntemi, kullanıcının MSAL kullanıcı önbellekten kaldırır. Bu yöntem geçerli kullanıcının unuttunuz MSAL etkili bir şekilde söyler. Yalnızca, etkileşimli olarak ayarlamışsa bir belirteç almak için gelecekteki bir istek başarılı olur.
Bu örnek uygulamasında tek bir kullanıcı destekler. Ancak MSAL nerede birden fazla hesap aynı zamanda oturum açmadan senaryolarını destekler. Bir e-posta uygulamasının kullanıcı çeşitli hesapların bulunduğu örneğidir.

## <a name="display-basic-token-information"></a>Temel belirteci bilgilerini görüntüle

* Aşağıdaki yöntemi ekleyin **MainPage.xaml.cs** belirteci ile ilgili temel bilgileri görüntülemek için:

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

### <a name="more-information"></a>Daha fazla bilgi

Kimlik belirteçlerini edinilen aracılığıyla **Openıd Connect** küçük bir alt kullanıcıya ilgili bilgileri de içerir. `DisplayBasicTokenInfo` belirtecinde yer alan temel bilgileri görüntüler. Kullanıcının görünen adı ve kimliği, sona erme tarihini belirteç ve erişim belirteci temsil eden dize örnektir. Seçerseniz **Microsoft Graph API çağrısı** birkaç kez düğmesini tıklatın, aynı belirtecin sonraki istekleri için yeniden kullanılmış görürsünüz. Belirteç yenileme zamanı MSAL kapılarını açtığında genişletilmiş sona erme tarihini de görebilirsiniz.

## <a name="register-your-application"></a>Uygulamanızı kaydetme

Şimdi, uygulamanızın Microsoft uygulama kayıt Portalı'nda kaydetmeniz gerekir:
1. Git [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app) bir uygulamayı kaydetmek için.
2. Uygulamanız için bir ad girin.
3. Olduğundan emin olun seçeneğini **destekli Kurulum** olan *seçili*.
4. Seçin **eklemek platformları**seçin **yerel uygulama**ve ardından **kaydetmek**.
5. GUID kopyalayın **uygulama kimliği**, Visual Studio, açık geri gidin **App.xaml.cs**ve yerine `your_client_id_here` yalnızca kayıtlı uygulama kimliği:

    ```csharp
    private static string ClientId = "your_application_id_here";
    ```

## <a name="enable-integrated-authentication-on-federated-domains-optional"></a>Federasyon etki alanı (isteğe bağlı) tümleşik kimlik doğrulamasını etkinleştirin

Federe bir Azure Active Directory etki alanı ile kullanıldığında, Windows tümleşik kimlik doğrulamasını etkinleştirmek için uygulama bildirimi ek özellikler etkinleştirmeniz gerekir:

1. Çift **Package.appxmanifest**.
2. Seçin **yetenekleri** sekmesinde ve aşağıdaki ayarların etkin olduğundan emin olun:

    - Kurumsal kimlik doğrulama
    - Özel ağlar (istemci ve sunucu)
    - Paylaşılan kullanıcı sertifikaları

3. Açık **App.xaml.cs** ve uygulama oluşturucuda aşağıdaki satırı ekleyin:

    ```csharp
    App.PublicClientApp.UseCorporateNetwork = true;
    ```

> [!IMPORTANT]
> Windows tümleşik kimlik doğrulaması varsayılan olarak bu örnek için yapılandırılmamış. İstek uygulamaları *Kurumsal kimlik doğrulama* veya *paylaşılan kullanıcı sertifikaları* özellikleri daha yüksek bir düzeyde doğrulama Windows mağazası tarafından gerektirir. Ayrıca, tüm geliştiriciler daha yüksek düzeyde doğrulama yapmak istiyorsunuz. Federe bir Azure Active Directory etki alanı ile Windows tümleşik kimlik doğrulaması gerekiyorsa bu ayarı etkinleştirin.


## <a name="test-your-code"></a>Kodunuzu test

Uygulamanızı test etmek için projenizi Visual Studio'da çalıştırmak için F5'i seçin. Ana penceresi görünür:

![Uygulamanın kullanıcı arabirimi](media/active-directory-uwp-v2.md/testapp-ui.png)

Test etmek hazır olduğunuzda, seçin **Microsoft Graph API çağrısı**. Daha sonra oturum açmak için bir Microsoft Azure Active Directory kuruluş hesabı veya live.com veya outlook.com, gibi bir Microsoft hesabı kullanın. İlk kez kullanıyorsanız, oturum açmak için kullanıcının isteyen bir pencere görürsünüz:

![Oturum açma sayfası](media/active-directory-uwp-v2.md/sign-in-page.png)

### <a name="consent"></a>Onayla
Uygulamanız için oturum ilk kez aşağıdakine benzer bir onay ekranı sunulur. Seçin **Evet** açıkça onaylaması için erişmek için:

![Erişim onay ekranı](media/active-directory-uwp-v2.md/consentscreen.png)
### <a name="expected-results"></a>Beklenen sonuçları
Kullanıcı profili bilgilerini üzerinde Microsoft Graph API çağrısı tarafından döndürülen bkz **API çağrısı sonuçları** ekran:

![API çağrısı sonuçları ekranı](media/active-directory-uwp-v2.md/uwp-results-screen.PNG)

Aracılığıyla edinilen belirteci hakkındaki temel bilgileri de görebilirsiniz `AcquireTokenAsync` veya `AcquireTokenSilentAsync` içinde **belirteci bilgisi** kutusunda:

|Özellik  |Biçimlendir  |Açıklama |
|---------|---------|---------|
|**Ad** |Kullanıcının tam adı|Kullanıcı adı ve soyadı.|
|**Kullanıcı Adı** |<span>user@domain.com</span> |Kullanıcı tanımlayan kullanıcı adı.|
|**Belirtecinin süresi** |DateTime |Belirtecin süresinin dolduğu saati. MSAL belirteci gerekli olarak yenileyerek sona erme tarihini genişletir.|
|**Erişim belirteci** |Dize |HTTP gönderilen belirteç dizesini gerektiren ister bir *Authorization Üstbilgisi*.|

#### <a name="see-whats-in-the-access-token-optional"></a>Erişim belirtecinde yer (isteğe bağlı) nedir bakın
İsteğe bağlı olarak, değeri kopyalayabilir **erişim belirteci** ve yapıştırın https://jwt.ms çözülmesi ve talep listesine bakın.

### <a name="more-information-about-scopes-and-delegated-permissions"></a>Kapsamlar ve temsilci izinleri hakkında daha fazla bilgi

Microsoft Graph API gerektiriyor *user.read* kullanıcı profilini okuma için kapsamı. Bu kapsam, varsayılan olarak uygulama kayıt Portalı'nda kayıtlı her uygulama otomatik olarak eklenir. Diğer Microsoft Graph API'ler ve özel API'leri, arka uç sunucusu için ek kapsamlar gerektirebilir. Microsoft Graph API gerektiriyor *Calendars.Read* kullanıcının takvimleri listelemek için kapsam.

Bir uygulama bağlamında kullanıcının takvimleri erişmek için eklemeniz *Calendars.Read* izin uygulama kayıt bilgileri için temsilci. Ardından ekleyin *Calendars.Read* için kapsam `acquireTokenSilent` çağırın. 

> [!NOTE]
> Kapsam sayısı arttıkça, kullanıcılar için ek onayları istenebilir.

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="issue-1"></a>Sorunu 1
Uygulamanızdaki Federasyon Azure Active Directory etki alanında oturum açtığınızda, aşağıdaki hata iletilerinden birini alabilirsiniz:
 - Geçerli istemci sertifikası isteğinde bulundu.
 - Geçerli sertifikası kullanıcının sertifika deposunda bulunamadı.
 - Farklı kimlik doğrulama yöntemini tekrar seçmeyi deneyin.

**Neden:** olmayan Enterprise ve sertifika özellikleri etkinleştirilmiş.

**Çözüm:** adımları [tümleşik kimlik doğrulaması Federasyon etki alanı üzerinde](#enable-integrated-authentication-on-federated-domains-optional).

### <a name="issue-2"></a>Sorun 2
Etkinleştirmeniz [tümleşik kimlik doğrulaması Federasyon etki alanları](#enable-integrated-authentication-on-federated-domains-optional) Windows Hello bir Windows 10 bilgisayarında bir ortamda yapılandırılmış çok faktörlü kimlik doğrulaması ile oturum açmak için kullanmayı deneyin. Sertifika listesi sunulur. Ancak, PIN kodunuzu kullanmayı seçerseniz, PIN penceresi hiçbir zaman sunulur.

**Neden:** bu sorunu web kimlik doğrulama Aracısı Windows 10 Masaüstü çalıştıran UWP uygulamalarında bilinen kısıtlamasıdır. Ayrıca Windows 10 Mobile düzgün çalışır.

**Geçici çözüm:** seçin **oturum diğer seçenekleri oturum**. Ardından **bir kullanıcı adı ve parolayla oturum**. Seçin **parolanızı sağlayın**. Ardından telefon kimlik doğrulama işleminde size gidin.
