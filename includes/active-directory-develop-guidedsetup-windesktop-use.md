
## <a name="use-msal-to-get-a-token-for-the-microsoft-graph-api"></a>Microsoft Graph API için bir belirteç almak üzere MSAL kullanın

Bu bölümde, Microsoft Graph API için bir belirteç almak üzere MSAL kullanın.

1.  İçinde *MainWindow.xaml.cs* dosya, başvuru için MSAL sınıfına ekleyin:

    ```csharp
    using Microsoft.Identity.Client;
    ```
<!-- Workaround for Docs conversion bug -->

2. Değiştir `MainWindow` sınıfında aşağıdaki kodu:

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
### <a name="more-information"></a>Daha fazla bilgi
#### <a name="get-a-user-token-interactively"></a>Kullanıcı etkileşimli olarak belirteci alma
Çağırma `AcquireTokenAsync` yöntemi, kullanıcının oturum açmasına izin isteyen bir pencere sonuçlanıyor. Uygulamalar genellikle etkileşimli olarak korunan bir kaynağa erişmek için ihtiyaç duydukları ilk kez oturum açmalarını gerektirir. (Örneğin, bir kullanıcının parolasının süresi sona erdiğinde) bir belirtecini almak üzere sessiz bir işlemi başarısız olduğunda oturum açmak de gerekebilir.

#### <a name="get-a-user-token-silently"></a>Bir kullanıcı sessizce belirteci alma
`AcquireTokenSilentAsync` Yöntemi belirteci satın almalar ve herhangi bir kullanıcı etkileşimi olmadan yenilemeleri işler. Sonra `AcquireTokenAsync` ilk kez yürütüldüğünde `AcquireTokenSilentAsync` veya belirteçleri yenileme isteği için çağrıları sessizce yapmış sonraki çağrılar için korunan kaynaklara erişim belirteçleri almak için kullanılacak normal bir yöntemdir.

Sonuç olarak, `AcquireTokenSilentAsync` yöntemi başarısız olur. Hatanın nedenlerini kullanıcı oturumunuz veya başka bir cihazda kendi parolanın değiştirilmesi emin olabilir. MSAL algıladığında, etkileşimli bir eylem gerektirmeyen tarafından sorunu çözmek için harekete bir `MsalUiRequiredException` özel durum. Uygulamanız bu özel durumun iki yolla işleyebilir:

* Karşı arama yapabilmeniz için `AcquireTokenAsync` hemen. Bu çağrı, oturum açmak için kullanıcıdan içinde sonuçlanır. Bu deseni genelde çevrimiçi uygulamalarda kullanılır kullanıcı için kullanılabilir çevrimdışı içerik olduğu. Bu Destekli kurulum tarafından oluşturulan örnek, örnek yürütme eylemi ilk zamanında görebilirsiniz bu deseni takip eder. 
    * Hiçbir kullanıcı, uygulamayı kullanıldığından `PublicClientApp.Users.FirstOrDefault()` bir null değer içeriyor ve bir `MsalUiRequiredException` özel durumu oluşur. 
    * Ardından örnek kodda çağırarak özel durumu işler `AcquireTokenAsync`, oturum açmak için kullanıcıdan içinde sonuçlanır.

* Bunun yerine oturum açmak için doğru zamanı seçebilmeniz için bir etkileşimli oturum açma gerekli olan kullanıcılar için görsel bir gösterge sunabilir. Veya uygulamayı yeniden deneyebilirsiniz `AcquireTokenSilentAsync` daha sonra. Çevrimdışı içeriği uygulamada kullanılabilir olduğunda kullanıcıların diğer uygulama işlevselliği kesintiye uğratmadan--Örneğin, ne zaman kullanabileceğini bu deseni sıkça kullanılır. Bu durumda, kullanıcıların korunan bir kaynağa erişmek veya eski bilgileri yenilemek oturum açmak istedikleri zaman karar verebilirsiniz. Yeniden denemek alternatif olarak, uygulama karar `AcquireTokenSilentAsync` zaman ağ geri geçici olarak devre dışı bırakıldı sonra.
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-by-using-the-token-you-just-obtained"></a>Yalnızca edinilen belirteçle kullanarak Microsoft Graph API çağırma

Aşağıdaki yeni yöntemine ekleyin, `MainWindow.xaml.cs`. Yöntem yapmak için kullanılan bir `GET` grafik API'sine karşı bir Authorize üstbilgisi kullanarak isteği:

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
### <a name="more-information-about-making-a-rest-call-against-a-protected-api"></a>Karşı korumalı bir API REST çağrısı yapma hakkında daha fazla bilgi

Bu örnek uygulamasında kullandığınız `GetHttpContentWithToken` yöntemini bir HTTP sağlamak için `GET` isteği için bir belirteç gerekiyor korunan bir kaynağa karşı ve ardından içeriği çağırana dönün. Bu yöntem alınan belirteç HTTP yetkilendirme üst bilgi ekler. Bu örnek için Microsoft Graph API kaynaktır *bana* uç noktası kullanıcı profili bilgilerini görüntüler.
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-a-user"></a>Bir kullanıcı oturum için bir yöntem ekleyin

Bir kullanıcı imzalamak için aşağıdaki yöntemi ekleyin, `MainWindow.xaml.cs` dosyası:

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
### <a name="more-information-about-user-sign-out"></a>Kullanıcı oturum kapatma hakkında daha fazla bilgi

`SignOutButton_Click` Yöntemi, kullanıcılar yalnızca etkileşimli olarak yapılırsa, bir belirteç almak için gelecekteki bir istek başarılı olur, böylece geçerli kullanıcının unuttunuz MSAL etkili bir şekilde söyler MSAL kullanıcı önbellekten kaldırır.

Bu örnek uygulamasında tek kullanıcılı desteklese de MSAL nerede birden fazla hesap aynı zamanda oturum açmadan senaryolarını destekler. Örnek bir kullanıcı birden fazla hesap bulunduğu bir e-posta uygulamasıdır.
<!--end-collapse-->

## <a name="display-basic-token-information"></a>Temel belirteci bilgilerini görüntüle

Belirteç hakkındaki temel bilgileri görüntülemek için aşağıdaki yöntemi ekleyin, *MainWindow.xaml.cs* dosyası:

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
### <a name="more-information"></a>Daha fazla bilgi

Kullanıcı açtığında sonra Microsoft grafik API'sini çağırmak için kullanılan erişim belirteci ek olarak, MSAL ayrıca bir kimliği belirteci alır. Bu belirteç küçük bir alt kullanıcılara ilgili bilgi içerir. `DisplayBasicTokenInfo` Yöntemi belirteç temel bilgiler görüntüler. Örneğin, kullanıcının görünen adı ve kimliği yanı sıra belirteci süre sonu tarihi ve erişim belirteci temsil eden dize görüntüler. Seçebileceğiniz *Microsoft Graph API çağrısı* birden çok kez düğmesine tıklayın ve aynı belirtecin sonraki istekleri için yeniden kullanılmış bakın. MSAL onu kapılarını açtığında genişletilen belirteci yenileme süresi sona erme tarihi de görebilirsiniz.
<!--end-collapse-->

