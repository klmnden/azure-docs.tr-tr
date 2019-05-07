---
title: Microsoft kimlik platformu UWP kullanmaya başlama | Azure
description: Evrensel Windows platformu uygulamaları (UWP), Microsoft kimlik platformu uç noktası tarafından erişim belirteçleri gerektiren bir API'yi nasıl çağırabilirsiniz.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 448858efeaae4c3e2a41d41181e9ec74b03223f6
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65138248"
---
# <a name="call-microsoft-graph-api-from-a-universal-windows-platform-application-xaml"></a>Microsoft Graph API'sini çağırmak bir evrensel Windows platformu uygulaması (XAML)

> [!div renderon="docs"]
> [!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

Bu kılavuz nasıl yerel bir evrensel Windows Platformu (UWP) uygulama erişim belirteci isteği ve ardından Microsoft Graph API'sini çağırmak açıklar. Kılavuzu, Microsoft kimlik platformu uç noktasından erişim belirteçlerini gerektiren diğer API'ler için de geçerlidir.

Bu kılavuzun sonunda, uygulama, kişisel hesapları kullanarak korumalı bir API çağırır. Outlook.com, live.com ve diğerleri verilebilir. Uygulamanızı herhangi bir şirket veya Azure Active Directory (Azure AD) sahip kuruluş iş ve Okul hesaplarında da çağırır.

>[!NOTE]
> Bu kılavuzda Visual Studio 2017 ile Evrensel Windows platformu geliştirme yüklü gerektirir. Bkz: [ayarlanmasını](https://docs.microsoft.com/windows/uwp/get-started/get-set-up) indirin ve evrensel Windows platformu uygulamaları geliştirmek için Visual Studio'yu yapılandırma hakkında yönergeler için.

## <a name="how-this-guide-works"></a>Bu kılavuz nasıl çalışır?

![Bu öğretici tarafından oluşturulan örnek uygulamasını nasıl çalıştığını gösterir](./media/tutorial-v2-windows-uwp/uwp-intro.svg)

Bu kılavuzda, Microsoft Graph API'si sorguları örnek UWP uygulaması veya Microsoft kimlik platformu uç noktasından belirteçleri kabul eden bir Web API oluşturur. Bu senaryo için bir belirteç HTTP istekleri aracılığıyla yetkilendirme üst bilgisi eklenir. Microsoft Authentication Library (MSAL), belirteç edinme ve yenileme işlemini gerçekleştirir.

## <a name="nuget-packages"></a>NuGet paketleri

Bu kılavuz, aşağıdaki NuGet paketlerini kullanır:

|Kitaplık|Açıklama|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft Authentication Library|

## <a name="set-up-your-project"></a>Projenizi ayarlama

Bu bölümde, ile Windows Masaüstü .NET uygulaması (XAML) tümleştirmek için adım adım yönergeler sağlar. *Microsoft ile oturum açma*. Ardından, Microsoft Graph API gibi bir belirteç gerektiren bir Web API sorgulayabilirsiniz.

Bu kılavuz, Graph API'si sorguları ve oturum kapatma düğmesi çağrılarının sonuçlarını görüntülemek ve metin kutuları bir düğme görüntüleyen bir uygulama oluşturur.

> [!NOTE]
> Bunun yerine bu örneği ait Visual Studio projeyi indirmek istiyor musunuz? [Bir projeyi indirirken](https://github.com/Azure-Samples/active-directory-dotnet-native-uwp-v2/archive/msal3x.zip) ve atlamak [uygulama kaydı](#register-your-application "uygulama kayıt adım") adım çalıştırılmadan önce kod örneği yapılandırmak için.

### <a name="create-your-application"></a>Uygulamanızı oluşturma

1. Visual Studio'da **dosya** > **yeni** > **proje**.
2. Altında **şablonları**seçin **Visual C#**.
3. **Boş Uygulama (Evrensel Windows)** seçeneğini belirleyin.
4. Uygulama adını ve seçin **Tamam**.
5. İstenirse, herhangi bir sürümünü seçin **hedef** ve **Minimum** sürümleri ve select **Tamam**.

    >![En az ve hedef sürümleri](./media/tutorial-v2-windows-uwp/vs-minimum-target.png)

## <a name="add-microsoft-authentication-library-to-your-project"></a>Microsoft kimlik doğrulama kitaplığı projenize ekleyin.
1. Visual Studio’da **Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**’nu seçin.
2. Aşağıdaki komutu kopyalayıp **Paket Yöneticisi Konsolu** penceresi:

    ```powershell
    Install-Package Microsoft.Identity.Client -IncludePrerelease
    ```

> [!NOTE]
> Bu komut yükler [Microsoft kimlik doğrulama Kitaplığı](https://aka.ms/msal-net). MSAL alır, önbelleğe alır ve Microsoft kimlik platformu tarafından korunan API'lere kullanıcı belirteçleri yeniler.

## <a name="create-your-applications-ui"></a>Uygulamanızın kullanıcı Arabirimi oluşturma

A **MainPage.xaml** dosyası, proje şablonunun bir parçası olarak otomatik olarak oluşturulur. Bu dosyayı açın ve ardından yönergeleri izleyin:

* Uygulamanızın değiştirin **kılavuz** aşağıdaki kodla düğüm:

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
    
## <a name="use-msal-to-get-a-token-for-microsoft-graph-api"></a>Microsoft Graph API için bir belirteç almak için MSAL kullanın

Bu bölümde, Microsoft Graph API için bir belirteç almak için MSAL kullanmayı gösterir.

1.  İçinde **MainPage.xaml.cs**, başvuru için MSAL sınıfa ekleyin:

    ```csharp
    using Microsoft.Identity.Client;
    ```

2. Değiştirin, <code>MainPage</code> aşağıdaki kodla sınıfı:

    ```csharp
    public sealed partial class MainPage : Page
    {
        //Set the API Endpoint to Graph 'me' endpoint
        string graphAPIEndpoint = "https://graph.microsoft.com/v1.0/me";

        //Set the scope for API call to user.read
        string[] scopes = new string[] { "user.read" };

        // Below are the clientId (Application Id) of your app registration and the tenant information. 
        // You have to replace:
        // - the content of ClientID with the Application Id for your app registration
        // - Te content of Tenant by the information about the accounts allowed to sign-in in your application:
        //   - For Work or School account in your org, use your tenant ID, or domain
        //   - for any Work or School accounts, use organizations
        //   - for any Work or School accounts, or Microsoft personal account, use common
        //   - for Microsoft Personal account, use consumers
        private const string ClientId = "0b8b0665-bc13-4fdc-bd72-e0227b9fc011";        

        public IPublicClientApplication PublicClientApp { get; } 

        public MainPage()
        {
          this.InitializeComponent();

          PublicClientApp = PublicClientApplicationBuilder.Create(ClientId)
                .WithAuthority(AadAuthorityAudience.AzureAdAndPersonalMicrosoftAccount)
                .WithLogging((level, message, containsPii) =>
                {
                    Debug.WriteLine($"MSAL: {level} {message} ");
                }, LogLevel.Warning, enablePiiLogging:false,enableDefaultPlatformLogging:true)
                .WithUseCorporateNetwork(true)
                .Build();
        }

        /// <summary>
        /// Call AcquireTokenAsync - to acquire a token requiring user to sign-in
        /// </summary>
        private async void CallGraphButton_Click(object sender, RoutedEventArgs e)
        {
         AuthenticationResult authResult = null;
         ResultText.Text = string.Empty;
         TokenInfoText.Text = string.Empty;

         // It's good practice to not do work on the UI thread, so use ConfigureAwait(false) whenever possible.            
         IEnumerable<IAccount> accounts = await PublicClientApp.GetAccountsAsync().ConfigureAwait(false); 
         IAccount firstAccount = accounts.FirstOrDefault();

         try
         {
          authResult = await PublicClientApp.AcquireTokenSilent(scopes, firstAccount)
                                                  .ExecuteAsync();
         }
         catch (MsalUiRequiredException ex)
         {
          // A MsalUiRequiredException happened on AcquireTokenSilent.
          // This indicates you need to call AcquireTokenInteractive to acquire a token
          System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");

          try
          {
           authResult = await PublicClientApp.AcquireTokenInteractive(scopes)
                                                      .ExecuteAsync()
                                                      .ConfigureAwait(false);
           }
           catch (MsalException msalex)
           {
            await DisplayMessageAsync($"Error Acquiring Token:{System.Environment.NewLine}{msalex}");
           }
          }
          catch (Exception ex)
          {
           await DisplayMessageAsync($"Error Acquiring Token Silently:{System.Environment.NewLine}{ex}");
           return;
          }

          if (authResult != null)
          {
           var content = await GetHttpContentWithToken(graphAPIEndpoint,
                                                       authResult.AccessToken).ConfigureAwait(false);

           // Go back to the UI thread to make changes to the UI
           await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
           {
            ResultText.Text = content;
            DisplayBasicTokenInfo(authResult);
            this.SignOutButton.Visibility = Visibility.Visible;
           });
          }
        }
    ```

### <a name="more-information"></a>Daha fazla bilgi

#### <a name="get-a-user-token-interactively"></a>Etkileşimli olarak kullanıcı belirteci alma

Bir çağrı `AcquireTokenInteractive` oturum açmak için kullanıcıların ister bir pencere yöntemi sonuçlanıyor. Uygulamalar genellikle etkileşimli olarak korunan bir kaynağa erişmek için ihtiyaç duydukları ilk kez oturum açmalarını gerektirir. Bunlar bir belirteç almak için sessiz bir işlemi başarısız olduğunda oturum açmanız gerekebilir. Bir kullanıcı parolasının süresi sona erdiğinde bir örnektir.

#### <a name="get-a-user-token-silently"></a>Kullanıcı belirtecini sessizce alma

`AcquireTokenSilent` Yöntemi, belirteç edinme ve herhangi bir kullanıcı etkileşimi olmadan yenilemeler işler. Sonra `AcquireTokenInteractive` ilk kez yürütülür ve kullanıcı kimlik bilgileri istenir `AcquireTokenSilent` yöntemi belirteçleri sessiz bir şekilde alması nedeniyle yapılan sonraki çağrılar için belirteçler istemek için kullanılması gerekir. MSAL, belirteç önbelleği ve yenileme işleyecektir.

Sonuç olarak, `AcquireTokenSilent` yöntemi başarısız olur. Hatanın nedenlerini kullanıcılara oturumunuz veya başka bir cihazda kendi parola değiştirildi, olabilir. MSAL etkileşimli bir eylem gerektirerek sorun çözülebilir, harekete algıladığında bir `MsalUiRequiredException` özel durum. Uygulamanız, bu özel durumun iki şekilde işleyebilir:

* Bir çağrısı yapabilirsiniz `AcquireTokenInteractive` hemen. Bu çağrı, kullanıcının oturum açmasını isteyen içinde sonuçlanır. Normalde, bu düzen çevrimiçi uygulamalarda kullanılır kullanıcı için çevrimdışı kullanılabilir içerik olduğunda. Bu Kılavuzlu kurulum tarafından oluşturulan örnek deseni izler. Bu örnek çalışma eylem ilk zamanı'na bakın.
  * Hiçbir kullanıcı uygulama kullanıldığından `accounts.FirstOrDefault()` bir null değer içeriyor ve bir `MsalUiRequiredException` özel durumu oluşturulur.
  * Ardından kod çağırarak özel durumu işleyen `AcquireTokenInteractive`. Bu çağrı, kullanıcının oturum açmasını isteyen içinde sonuçlanır.

* Veya bunun yerine, görsel gösterimi kullanıcılara etkileşimli bir oturum açma gerekli olduğunu gösterir. Ardından, oturum açmak için doğru zamanda seçebilirsiniz. Veya uygulama yeniden deneyebilirsiniz `AcquireTokenSilent` daha sonra. Genellikle, kullanıcılar, diğer uygulama işlevleri kesintiye uğratmadan kullanabilir bu deseni kullanılır. Çevrimdışı içeriği uygulamada kullanılabilir olduğunda bir örnektir. Korumalı kaynağa ya da eski bilgileri Yenile oturum açmaya istediğinizde, bu durumda, kullanıcılar karar verebilirsiniz. Ya da uygulama yeniden denemek başka bir karar verebilirsiniz `AcquireTokenSilent` zaman ağ geri sonra geçici olarak kullanılamıyor.

## <a name="call-microsoft-graph-api-by-using-the-token-you-just-obtained"></a>Yalnızca edinilen belirteçle'ı kullanarak Microsoft Graph API çağırma

* Aşağıdaki yeni yöntemi ekleyin **MainPage.xaml.cs**. Bu yöntem yapmak için kullanılan bir `GET` kullanarak Graph API'si isteğinin bir `Authorization` üst bilgi:

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
            request.Headers.Authorization = 
              new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", token);
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

Bu örnek uygulamasında `GetHttpContentWithToken` yöntemi bir HTTP yapmak için kullanılan `GET` isteği karşı korumalı bir kaynağın bir belirteç gerekiyor. Ardından yöntemi, içeriği çağırana döner. Bu yöntem alınan belirteç ekler **HTTP yetkilendirme** başlığı. Bu örnek için Microsoft Graph API kaynaktır **bana** uç noktası için kullanıcı profili bilgilerini görüntüler.
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>Kullanıcının oturumunu kapatmaz için yöntem ekleme

* Kullanıcının oturumunu kapatmaz için aşağıdaki yöntemi ekleyin. **MainPage.xaml.cs**:

    ```csharp
    /// <summary>
    /// Sign out the current user
    /// </summary>
    private async void SignOutButton_Click(object sender, RoutedEventArgs e)
    {
        IEnumerable<IAccount> accounts = await PublicClientApp.GetAccountsAsync
                                                              .ConfigureAwait(false);
        IAccount firstAccount = accounts.FirstOrDefault();

        try
        {
            await PublicClientApp.RemoveAsync(firstAccount).ConfigureAwait(false);
            await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
            {
                ResultText.Text = "User has signed-out";
                this.CallGraphButton.Visibility = Visibility.Visible;
                    this.SignOutButton.Visibility = Visibility.Collapsed;
                });
            }
            catch (MsalException ex)
            {
                ResultText.Text = $"Error signing-out user: {ex.Message}";
            }
        }
    ```

> [!NOTE]
> MSAL.NET hesapları değiştirebilen veya belirteçlerini almak için zaman uyumsuz yöntemleri kullanır ve bu nedenle kullanıcı Arabirimi iş parçacığı, bu nedenle kullanıcı Arabirimi ed eylemleri yapmanın halletmeniz ihtiyacınız `Dispatcher.RunAsync`ve çağırmak için önlemler `ConfigureAwait(false)`

### <a name="more-information-on-sign-out"></a>Oturum kapatma hakkında daha fazla bilgi

`SignOutButton_Click` Yöntemi, kullanıcının MSAL kullanıcı önbellekten kaldırır. Bu yöntem, geçerli kullanıcının unutmak çok MSAL etkili bir şekilde söyler. Yalnızca etkileşimli olarak yaptı, ardından bir belirteç almak için gelecekteki bir istek başarılı olur.
Bu örnek uygulamasında tek bir kullanıcı destekler. Ancak MSAL burada birden fazla hesap aynı zamanda oturum senaryolarını destekler. Bir örnek kullanıcı çeşitli hesapların sahip olduğu bir e-posta uygulamasıdır.

## <a name="display-basic-token-information"></a>Temel belirteç bilgilerini görüntüle

* Aşağıdaki yöntemi ekleyin **MainPage.xaml.cs** belirteç hakkındaki temel bilgileri görüntülemek için:

    ```csharp
    /// <summary>
    /// Display basic information contained in the token. Needs to be called from the UI thead.
    /// </summary>
    private void DisplayBasicTokenInfo(AuthenticationResult authResult)
    {
        TokenInfoText.Text = "";
        if (authResult != null)
        {
            TokenInfoText.Text += $"User Name: {authResult.Account.Username}" + Environment.NewLine;
            TokenInfoText.Text += $"Token Expires: {authResult.ExpiresOn.ToLocalTime()}" + Environment.NewLine;
        }
    }
    ```

### <a name="more-information"></a>Daha fazla bilgi

Kimlik belirteçlerini alınan aracılığıyla **Openıd Connect** kullanıcıya testlerinizle ilgili olabilecek bilgilere küçük bir kısmı da içerir. `DisplayBasicTokenInfo` belirteçteki temel bilgileri görüntüler. Kullanıcının görünen adı ve kimliği, sona erme tarihi belirteç ve erişim belirteci temsil eden dize verilebilir. Seçerseniz **Microsoft Graph API çağrısı** düğmesine birkaç kez, sonraki istekler için aynı belirteci yeniden olduğunu görürsünüz. Belirteci yenileme zamanı MSAL karar verdiğinde genişletilmiş sona erme tarihini de görebilirsiniz.

## <a name="register-your-application"></a>Uygulamanızı kaydetme

Şimdi Microsoft uygulama kayıt Portalı'nda uygulamanızı kaydetmeniz gerekir:

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Birden fazla Azure AD kiracısında hesabınız varsa, seçin `Directory + Subscription` sayfası ve anahtar üzerinde menüde sağ üst köşesindeki portal oturumunuzu istediğiniz Azure ad Kiracı.
1. Geliştiriciler için Microsoft identity platformuna gidin [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfası.
1. Seçin **yeni kayıt**.
   - **Ad** alanına uygulama kullanıcılarına gösterilecek anlamlı bir uygulama adı girin, örneğin `UWP-App-calling-MSGraph`.
   - İçinde **desteklenen hesap türleri** bölümünden **herhangi bir kuruluş dizinini ve kişisel Microsoft hesapları (örneğin, Skype, Xbox, Outlook.com) hesaplarında**.
   - Uygulamayı kaydetmek için **Kaydet**'i seçin.
1. Uygulamasında **genel bakış** sayfasında, bulmak **uygulama (istemci) kimliği** değeri ve daha sonra kullanmak üzere kaydedin. Visual Studio, açık dönün **MainPage.xaml.cs**, ClientID değerini yalnızca kayıtlı uygulama Kimliğiyle değiştirin:
1. Uygulama sayfa listesinde **Kimlik doğrulaması**'nı seçin.
   1. İçinde **yeniden yönlendirme URI'leri** bölümünde yeniden yönlendirme URI'leri listesi:
   1. İçinde **türü** Sütun Seç **genel istemci (Mobil ve Masaüstü)**.
   1. Girin `urn:ietf:wg:oauth:2.0:oob` içinde **yeniden yönlendirme URI'si** sütun.
1. **Kaydet**’i seçin.
1. Uygulama sayfaları listesinde seçin **API izinleri**
   - Tıklayın **bir izin eklemek** düğmesine ve ardından,
   - Emin **Microsoft API** sekmesi seçili
   - İçinde *Microsoft APIs kullanılan* bölümünde, tıklayarak **Microsoft Graph**
   - İçinde **temsilci izinleri** bölümünde, doğru izinler seçildiğinden emin olun: **User.Read**. Gerekirse arama kutusunu kullanın.
   - Seçin **izinleri eklemek** düğmesi

## <a name="enable-integrated-authentication-on-federated-domains-optional"></a>Federasyon etki alanları (isteğe bağlı) tümleşik kimlik doğrulamasını etkinleştirin

Bir Federasyon Azure ile kullanıldığında Windows-Integrated kimlik doğrulamasını etkinleştirmek için AD etki alanı, uygulama bildirimini gerekir. ek özellikleri sağlar:

1. Çift **Package.appxmanifest**.
2. Seçin **özellikleri** sekme ve aşağıdaki ayarların etkin olduğundan emin olun:

    - Kurumsal kimlik doğrulaması
    - Özel ağlar (istemci ve sunucu)
    - Paylaşılan kullanıcı sertifikaları

> [!IMPORTANT]
> [Tümleşik Windows kimlik doğrulaması](https://aka.ms/msal-net-iwa) Bu örnek için varsayılan olarak yapılandırılmamış. İstek uygulamaları *Kurumsal kimlik* veya *paylaşılan kullanıcı sertifikaları* doğrulaması ile Windows Store yüksek seviyede özellikleri gerektirir. Ayrıca, tüm geliştiriciler doğrulama yüksek seviyede yapmak istiyorsunuz. Yalnızca bir Federasyon Azure ile Windows tümleşik kimlik doğrulaması gerekiyorsa bu ayarı etkinleştirdiğinizde AD etki alanı.

## <a name="test-your-code"></a>Kodunuzu test etme

Uygulamanızı test etmek için projenizi Visual Studio'da çalıştırmak için F5'i seçin. Uygulamanızın ana penceresi görünür:

![Uygulamanın kullanıcı arabirimi](./media/tutorial-v2-windows-uwp/testapp-ui.png)

Test için hazır olduğunuzda **Microsoft Graph API çağrısı**. Daha sonra oturum açmak için bir Azure AD kuruluş hesabı veya live.com veya outlook.com gibi bir Microsoft hesabı kullanın. İlk kez ise, kullanıcının oturum açmasını isteyen bir pencere bakın:

![Oturum açma sayfası](./media/tutorial-v2-windows-uwp/sign-in-page.png)

### <a name="consent"></a>Onayla

İlk uygulamanızı, oturum açtığınızda aşağıdakine benzer bir onay ekranında eklemediğiniz. Seçin **Evet** açıkça onay verme erişmek için:

![Erişim onay ekranı](./media/tutorial-v2-windows-uwp/consentscreen.png)

### <a name="expected-results"></a>Beklenen sonuçlar

Üzerinde Microsoft Graph API çağrısı tarafından döndürülen profil bilgilerini görüntüleyebilmesine **API çağrısı sonuçları** ekran:

![API arama sonuçları](./media/tutorial-v2-windows-uwp/uwp-results-screen.PNG)

Aracılığıyla edinilen belirteci hakkında temel bilgileri de görebilirsiniz `AcquireTokenInteractive` veya `AcquireTokenSilent` içinde **belirteci bilgilerini** kutusunda:

|Özellik  |Biçimlendir  |Açıklama |
|---------|---------|---------|
|**Kullanıcı Adı** |<span>user@domain.com</span> |Kullanıcıyı tanımlayan kullanıcı adı.|
|**Belirteç süre sonu** |DateTime |Belirtecin süresinin sona erdiği zaman. MSAL, gerekirse belirteci yenilemeye tarafından sona erme tarihini genişleten.|

### <a name="more-information-about-scopes-and-delegated-permissions"></a>Kapsamlar ve temsilci izinleri hakkında daha fazla bilgi

Microsoft Graph API gerektirir *user.read* kapsamı, bir kullanıcının profilini okuma için. Bu kapsam, uygulama kayıt Portalı'nda kayıtlı her uygulamada varsayılan olarak otomatik olarak eklenir. Diğer Microsoft Graph API'leri ve özel API'ler, arka uç sunucusu için ek kapsamlarla gerektirebilir. Microsoft Graph API gerektirir *Calendars.Read* kullanıcının takvimleri listelemek için kapsam.

Bir uygulama bağlamında kullanıcının takvimler erişmek için ekleme *Calendars.Read* izin uygulama kayıt bilgileri için temsilci. Ardından Ekle *Calendars.Read* için kapsam `acquireTokenSilent` çağırın.

> [!NOTE]
> Kapsamların sayısı arttıkça, kullanıcılar için ek bir onayları istenebilir.

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="issue-1"></a>Sorunu 1

Uygulamanızda bir Federasyon azure'da oturum açtığınızda aşağıdaki hata iletilerinden birini alma AD etki alanı:

* Geçerli istemci sertifika isteği bulunamadı.
* Geçerli sertifika kullanıcının sertifika deposunda bulunamadı.
* Farklı kimlik doğrulama yöntemini tekrar seçmeyi deneyin.

**Neden:** Kurumsal ve sertifika özellikleri etkin değil.

**Çözüm:** Bağlantısındaki [tümleşik kimlik doğrulaması Federasyon etki alanlarında](#enable-integrated-authentication-on-federated-domains-optional).

### <a name="issue-2"></a>Sorun 2

Etkinleştirdiğiniz [tümleşik kimlik doğrulaması Federasyon etki alanlarında](#enable-integrated-authentication-on-federated-domains-optional) Windows Hello Windows 10 bilgisayarında bir ortamda yapılandırılmış çok faktörlü kimlik doğrulaması ile oturum açmak için kullanmayı deneyin. Sertifika listesi sunulur. Ancak, PIN kodunuzu kullanmayı seçerseniz, hiçbir zaman pencereyi SABİTLE sunulur.

**Neden:** Bu sorun, Windows 10 Masaüstü üzerinde çalışan UWP uygulamaları içinde web kimlik doğrulama aracısı bilinen bir sınırlamadır. Windows 10 Mobile üzerinde düzgün çalışır.

**Geçici çözüm:** Seçin **oturum oturum diğer seçenekleri**. Ardından **oturum adı ve parola ile oturum**. Seçin **parolanızı girebilirsiniz**. Telefon kimlik doğrulama işleminden sonra gidin.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
