
## <a name="set-up-your-project"></a>Projenizin kurulumunu

Bu bölümde yüklemek ve Openıd Connect kullanarak bir ASP.NET projede OWIN ara yazılımı üzerinden kimlik doğrulaması ardışık düzenini yapılandırmak için gereken adımları gösterir. 

> Bunun yerine bu örneği ait Visual Studio projesi indirmeyi tercih ediyorsunuz? [Bir proje indirme](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-DotNet/archive/master.zip) ve geçin [yapılandırma adımı](#create-an-application-express) kod örneği çalıştırmadan önce yapılandırmak için.

<!--start-collapse-->
> ### <a name="create-your-aspnet-project"></a>ASP.NET projesi oluşturma

> 1. Visual Studio'da:`File` > `New` > `Project`<br/>
> 2. Altında *Visual C# \Web*seçin `ASP.NET Web Application (.NET Framework)`.
> 3. Uygulamanızı adlandırın ve tıklayın *Tamam*
> 4. Seçin `Empty` eklemek için onay kutusunu seçip `MVC` başvuruları
<!--end-collapse-->

## <a name="add-authentication-components"></a>Kimlik doğrulaması Bileşenleri Ekle

1. Visual Studio'da:`Tools` > `Nuget Package Manager` > `Package Manager Console`
2. Ekleme *OWIN ara yazılımı NuGet paketlerini* Paket Yöneticisi konsolu penceresinde aşağıdakileri yazarak:

```powershell
Install-Package Microsoft.Owin.Security.OpenIdConnect
Install-Package Microsoft.Owin.Security.Cookies
Install-Package Microsoft.Owin.Host.SystemWeb
```

<!--start-collapse-->
> ### <a name="about-these-libraries"></a>Bu kitaplıklar hakkında

>Çoklu oturum tanımlama bilgisi tabanlı kimlik doğrulaması Openıd Connect kullanarak açma (SSO) yukarıdaki kitaplıkları etkinleştirin. Kimlik doğrulama tamamlandıktan sonra kullanıcıyı temsil eden simge uygulamanıza gönderilir, OWIN ara yazılımı bir oturum tanımlama bilgisi oluşturur. Tarayıcı daha sonra bu tanımlama bilgisi sonraki isteklerde kullanıcının parolasını yeniden yazın gerekmez ve hiçbir ek doğrulama gerektiği şekilde kullanır.
<!--end-collapse-->

## <a name="configure-the-authentication-pipeline"></a>Kimlik doğrulama ardışık düzenini yapılandırın
Aşağıdaki adımlar, bir OWIN ara yazılımı Openıd Connect kimlik doğrulamasını yapılandırmak için başlangıç sınıfı oluşturmak için kullanılır. Bu sınıf, IIS işlemi başladığında otomatik olarak yürütülür.

> Projenizi yoksa, bir `Startup.cs` dosyası kök klasöründe:<br/>
> 1. Projenin kök klasörü sağ tıklatın: >`Add` > `New Item...` > `OWIN Startup class`<br/>
> 2. Adlandırın`Startup.cs`

> OWIN başlangıç sınıfı ve değil bir standart C# sınıf seçilen sınıf olduğundan emin olun. Bu görürseniz denetleyerek doğrulayabilirsiniz `[assembly: OwinStartup(typeof({NameSpace}.Startup))]` ad alanı üzerinde.


1. Ekleme *OWIN* ve *Microsoft.IdentityModel* başvurular `Startup.cs`:

```csharp
using Microsoft.Owin;
using Owin;
using Microsoft.IdentityModel.Protocols;
using Microsoft.Owin.Security;
using Microsoft.Owin.Security.Cookies;
using Microsoft.Owin.Security.OpenIdConnect;
using Microsoft.Owin.Security.Notifications;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Başlangıç sınıfı aşağıdaki kodla değiştirin:
</li>
</ol>

```csharp
public class Startup
{        
    // The Client ID is used by the application to uniquely identify itself to Azure AD.
    string clientId = System.Configuration.ConfigurationManager.AppSettings["ClientId"];

    // RedirectUri is the URL where the user will be redirected to after they sign in.
    string redirectUri = System.Configuration.ConfigurationManager.AppSettings["RedirectUri"];

    // Tenant is the tenant ID (e.g. contoso.onmicrosoft.com, or 'common' for multi-tenant)
    static string tenant = System.Configuration.ConfigurationManager.AppSettings["Tenant"];

    // Authority is the URL for authority, composed by Azure Active Directory v2 endpoint and the tenant name (e.g. https://login.microsoftonline.com/contoso.onmicrosoft.com/v2.0)
    string authority = String.Format(System.Globalization.CultureInfo.InvariantCulture, System.Configuration.ConfigurationManager.AppSettings["Authority"], tenant);

    /// <summary>
    /// Configure OWIN to use OpenIdConnect 
    /// </summary>
    /// <param name="app"></param>
    public void Configuration(IAppBuilder app)
    {
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseCookieAuthentication(new CookieAuthenticationOptions());
            app.UseOpenIdConnectAuthentication(
            new OpenIdConnectAuthenticationOptions
            {
                // Sets the ClientId, authority, RedirectUri as obtained from web.config
                ClientId = clientId,
                Authority = authority,
                RedirectUri = redirectUri,
                // PostLogoutRedirectUri is the page that users will be redirected to after sign-out. In this case, it is using the home page
                PostLogoutRedirectUri = redirectUri,
                Scope = OpenIdConnectScopes.OpenIdProfile,
                // ResponseType is set to request the id_token - which contains basic information about the signed-in user
                ResponseType = OpenIdConnectResponseTypes.IdToken,
                // ValidateIssuer set to false to allow personal and work accounts from any organization to sign in to your application
                // To only allow users from a single organizations, set ValidateIssuer to true and 'tenant' setting in web.config to the tenant name
                // To allow users from only a list of specific organizations, set ValidateIssuer to true and use ValidIssuers parameter 
                TokenValidationParameters = new System.IdentityModel.Tokens.TokenValidationParameters() { ValidateIssuer = false },
                // OpenIdConnectAuthenticationNotifications configures OWIN to send notification of failed authentications to OnAuthenticationFailed method
                Notifications = new OpenIdConnectAuthenticationNotifications
                {
                    AuthenticationFailed = OnAuthenticationFailed
                }
            }
        );
    }

    /// <summary>
    /// Handle failed authentication requests by redirecting the user to the home page with an error in the query string
    /// </summary>
    /// <param name="context"></param>
    /// <returns></returns>
    private Task OnAuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> context)
    {
        context.HandleResponse();
        context.Response.Redirect("/?errormessage=" + context.Exception.Message);
        return Task.FromResult(0);
    }
}

```
<!--start-collapse-->
> ### <a name="more-information"></a>Daha Fazla Bilgi

> Sağladığınız içinde parametreleri *OpenIDConnectAuthenticationOptions* Azure AD ile iletişim kurmak uygulamanın koordinatları olarak hizmet. Openıd Connect ara yazılımı arka planda tanımlama bilgilerini kullandığından, ayrıca tanımlama bilgisi kimlik doğrulamasını gösterir yukarıdaki kodu olarak ayarlamanız gerekir. *Validateıssuer* değer belirli bir kuruluş için erişimi kısıtlamak değil Openıdconnect söyler.
<!--end-collapse-->

