
## <a name="set-up-your-project"></a>Projenizin kurulumunu

Bu bölümde bir Windows Masaüstü .NET uygulaması (XAML) tümleştirme göstermek için yeni bir proje oluşturmak için adım adım yönergeler sağlar *Microsoft ile oturum açma* için bir belirteç gerekiyor Web API'leri sorgulayabilirsiniz.

Bu kılavuz tarafından oluşturulan uygulama grafiği ve sonuçları ekran ve oturum kapatma düğmesini göster düğmesini gösterir.

> Bunun yerine bu örneği ait Visual Studio projesi indirmeyi tercih ediyorsunuz? [Bir proje indirme](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/master.zip) ve geçin [yapılandırma adımı](#create-an-application-express) kod örneği çalıştırmadan önce yapılandırmak için.


### <a name="create-your-application"></a>Uygulamanızı oluşturun
1. Visual Studio'da:`File` > `New` > `Project`<br/>
2. Altında *şablonları*seçin`Visual C#`
3. Seçin `WPF App` (veya *WPF uygulaması* , Visual Studio sürümüne bağlı olarak)

## <a name="add-the-microsoft-authentication-library-msal-to-your-project"></a>Microsoft kimlik doğrulama kitaplığı (MSAL) projenize ekleyin
1. Visual Studio'da:`Tools` > `Nuget Package Manager` > `Package Manager Console`
2. Paket Yöneticisi konsolu penceresinde aşağıdakileri kopyalayıp yapıştırın:

```powershell
Install-Package Microsoft.Identity.Client -Pre
```

> Yukarıdaki paket Microsoft kimlik doğrulama kitaplığı (MSAL) yükler. MSAL alınırken, önbelleğe alma ve Azure Active Directory v2 tarafından korunan API'leri erişmek için kullanılan kullanıcı toskens yenilemeyi işler.

## <a name="add-the-code-to-initialize-msal"></a>MSAL başlatmak için kod ekleme
Bu adımı MSAL kitaplığı ile etkileşim belirteçleri işleme gibi işlemek için bir sınıf oluşturmanıza yardımcı olur.

1. Açık `App.xaml.cs` dosya ve MSAL Kitaplığı Başvurusu sınıfına ekleyin:

```csharp
using Microsoft.Identity.Client;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Uygulama sınıfı şu güncelleştirin:
</li>
</ol>

```csharp
public partial class App : Application
{
    //Below is the clientId of your app registration. 
    //You have to replace the below with the Application Id for your app registration
    private static string ClientId = "your_client_id_here";

    public static PublicClientApplication PublicClientApp = new PublicClientApplication(ClientId);

}
```

## <a name="create-your-applications-ui"></a>Uygulamanızın kullanıcı Arabirimi oluşturma
Aşağıdaki bölümü, bir uygulama gibi Microsoft Graph korumalı arka uç sunucusuna nasıl sorgulayabilir gösterir. MainWindow.xaml dosya proje şablonu bir parçası olarak otomatik olarak oluşturulması gerekir. Bu dosya bu dosyayı açın ve aşağıdaki yönergeleri izleyin:

Uygulamanızın Değiştir `<Grid>` aşağıdaki olmalıdır:

```xml
<Grid>
    <StackPanel Background="Azure">
        <StackPanel Orientation="Horizontal" HorizontalAlignment="Right">
            <Button x:Name="CallGraphButton" Content="Call Microsoft Graph API" HorizontalAlignment="Right" Padding="5" Click="CallGraphButton_Click" Margin="5" FontFamily="Segoe Ui"/>
            <Button x:Name="SignOutButton" Content="Sign-Out" HorizontalAlignment="Right" Padding="5" Click="SignOutButton_Click" Margin="5" Visibility="Collapsed" FontFamily="Segoe Ui"/>
        </StackPanel>
        <Label Content="API Call Results" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="ResultText" TextWrapping="Wrap" MinHeight="120" Margin="5" FontFamily="Segoe Ui"/>
        <Label Content="Token Info" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="TokenInfoText" TextWrapping="Wrap" MinHeight="70" Margin="5" FontFamily="Segoe Ui"/>
    </StackPanel>
</Grid>
```
