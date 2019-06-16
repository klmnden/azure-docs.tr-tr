---
title: Key Vault desteği, Azure Key Vault Visual Studio kullanarak ASP.NET projenize ekleyin | Microsoft Docs
description: Bir ASP.NET veya ASP.NET Core web uygulaması için Key Vault desteği eklemek öğrenmenize yardımcı olmak için bu öğreticiyi kullanın.
services: key-vault
author: ghogen
manager: jillfra
ms.prod: visual-studio
ms.technology: vs-azure
ms.custom: vs-azure
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: ghogen
ms.openlocfilehash: 154eaa577ea66056c301db9516b425931b81d24d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64730440"
---
# <a name="add-key-vault-to-your-web-application-by-using-visual-studio-connected-services"></a>Key Vault, Visual Studio bağlı Hizmetler'i kullanarak web uygulamanıza ekleyin

Bu öğreticide, ASP.NET Core veya ASP.NET projesi herhangi bir türde kullanıp kullanmadığınızı Visual Studio'da, web projeleri, gizli verileri yönetmek için Azure anahtar kasası kullanmaya başlamak için ihtiyacınız olan her şeyi kolayca eklemek öğreneceksiniz. Visual Studio'da bağlı hizmetler kullanarak Visual Studio otomatik olarak tüm Azure Key Vault'ta bağlanmanız gereken yapılandırma ayarlarını ve NuGet paketleri Ekle olabilir. 

Bağlı hizmetler anahtar Kasası'nı etkinleştirmek için projenizde yaptığı değişiklikleri hakkında daha fazla bilgi için bkz: [Key Vault bağlı my ASP.NET 4.7.1 ne hizmetine - proje](#how-your-aspnet-framework-project-is-modified) veya [Key Vault bağlı ne hizmetine - ASP.NET Core projeme](#how-your-aspnet-core-project-is-modified).

## <a name="prerequisites"></a>Önkoşullar

- **Bir Azure aboneliği**. Bir aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
- **Visual Studio 2019** veya **Visual Studio 2017 sürüm 15.7** ile **Web geliştirme** iş yükü yüklenmiş. [Şimdi indir](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).
- Visual Studio 2017 ile ASP.NET (çekirdek değil), varsayılan olarak yüklü olmayan sonraki geliştirme araçları ve .NET Framework 4.7.1 gerekir. Bunları yüklemek için Visual Studio Yükleyicisi'ni başlatın, **Değiştir**ve ardından **tek tek bileşenler**, sonra sağ tarafında genişletmek **ASP.NET ve web geliştirme**ve **.NET Framework 4.7.1 geliştirme araçları**.
- Bir ASP.NET 4.7.1 veya sonrası, veya ASP.NET Core 2.0 web projesi açın.

## <a name="add-key-vault-support-to-your-project"></a>Key Vault desteği projenize ekleyin.

1. **Çözüm Gezgini**’nde **Ekle** > **Bağlı Hizmet** seçeneklerini belirleyin.
   Projenize ekleyebileceğiniz hizmetlerle birlikte Bağlı Hizmet sayfası görüntülenir.
1. Kullanılabilir hizmetler menüsünde **güvenli parolaları ile Azure anahtar kasası**.

   !["Azure anahtar kasası ile güvenli gizli dizileri" seçin](media/vs-key-vault-add-connected-service/KeyVaultConnectedService1.PNG)

   Visual Studio’da oturum açtıysanız ve hesabınızla ilişkili bir Azure aboneliğiniz varsa, aboneliklerinizi içeren bir açılır listenin yer aldığı bir sayfa görüntülenir. Visual Studio'ya oturum açmadıysanız ve hesabı ile oturum açmadıysanız, Azure aboneliğiniz için kullandığınız hesabın aynısını olduğundan emin olun.

1. Yeni veya mevcut bir anahtar Kasası'nı seçin ve istediğiniz aboneliği seçin veya otomatik olarak oluşturulan adı değiştirmek için düzenleme bağlantısını seçin.

   ![Aboneliğinizi seçme](media/vs-key-vault-add-connected-service/KeyVaultConnectedService3.PNG)

1. Anahtar kasası için kullanmak istediğiniz adı yazın.

   ![Key Vault yeniden adlandırabilir ve bir kaynak grubu seçin](media/vs-key-vault-add-connected-service/KeyVaultConnectedService-Edit.PNG)

1. Mevcut bir kaynak grubunu seçin ya da otomatik olarak oluşturulan benzersiz bir ad ile yeni bir tane seçin.  Farklı bir adla yeni bir grup oluşturmak istiyorsanız, kullanabileceğiniz [Azure portalı](https://portal.azure.com)ve sonra sayfayı kapatın ve kaynak gruplarının listesi yeniden yüklemek için yeniden başlatın.
1. Key Vault oluşturulacağı bölgeyi seçin. Web uygulamanızı Azure'da barındırılıyorsa, en iyi performans için web uygulamasını barındıran bölgeyi seçin.
1. Bir fiyatlandırma modelini seçin. Ayrıntılar için bkz [anahtar kasası fiyatlandırma](https://azure.microsoft.com/pricing/details/key-vault/).
1. Yapılandırma seçimlerini kabul etmek için Tamam'ı seçin.
1. Seçin **Ekle** anahtar kasası oluşturmak için. Zaten kullanılan bir ad seçerseniz oluşturma işlemi başarısız olabilir.  Bu durumda kullanın **Düzenle** bağlantı Key Vault yeniden adlandırıp tekrar deneyin.

   ![Bağlı hizmet projeye ekleniyor](media/vs-key-vault-add-connected-service/KeyVaultConnectedService4.PNG)

1. Şimdi, Azure anahtar Kasası'nda bir gizli dizi ekleyin. Portalı'nda doğru yere almak için bu anahtar Kasası'nda depolanan Yönet gizli bağlantısına tıklayın. Sayfa veya proje kapattıysanız, kendisine gidebilirsiniz [Azure portalında](https://portal.azure.com) seçerek **tüm hizmetleri**altında **güvenlik**, seçin **Key Vault**, oluşturduğunuz anahtar Kasası'nı seçin.

   ![Portalında gezinme](media/vs-key-vault-add-connected-service/manage-secrets-link.jpg)

1. Anahtar için Key Vault bölümünde oluşturduğunuz kasa öğesini **gizli dizileri**, ardından **Oluştur/içeri aktarma**.

   ![Gizli dizi Oluştur/İçeri Aktar](media/vs-key-vault-add-connected-service/generate-secrets.jpg)

1. "Ettiyseniz" gibi bir gizli dizi girin ve bir test olarak herhangi bir dize değeri verin ve ardından seçin **Oluştur** düğmesi.

   ![Gizli anahtar oluşturma](media/vs-key-vault-add-connected-service/create-a-secret.jpg)

1. (isteğe bağlı) Başka bir gizli dizi, ancak bu kez "Gizli dizileri--ettiyseniz" adlandırarak bir kategoriye put girin. Bu söz dizimi içeren bir gizli dizi "Ettiyseniz." Kategori "Gizli" belirtir.
 
Artık, kod, gizli dizileri erişebilirsiniz. Sonraki adımlar 4.7.1 ASP.NET veya ASP.NET Core kullanmanıza bağlı olarak farklılık gösterir.

## <a name="access-your-secrets-in-code"></a>Gizli anahtarlarınız kod erişimi

1. Bu iki nuget paketi yüklemesi [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) ve [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault) NuGet kitaplıkları.

2. Program.cs dosyasını açın ve kodu aşağıdaki kodla güncelleştirin: 
   ```
    public class Program
    {
        public static void Main(string[] args)
        {
            BuildWebHost(args).Run();
        }

        public static IWebHost BuildWebHost(string[] args) =>
           WebHost.CreateDefaultBuilder(args)
               .ConfigureAppConfiguration((ctx, builder) =>
               {
                   var keyVaultEndpoint = GetKeyVaultEndpoint();
                   if (!string.IsNullOrEmpty(keyVaultEndpoint))
                   {
                       var azureServiceTokenProvider = new AzureServiceTokenProvider();
                       var keyVaultClient = new KeyVaultClient(
                           new KeyVaultClient.AuthenticationCallback(
                               azureServiceTokenProvider.KeyVaultTokenCallback));
                       builder.AddAzureKeyVault(
                           keyVaultEndpoint, keyVaultClient, new DefaultKeyVaultSecretManager());
                   }
               }
            ).UseStartup<Startup>()
             .Build();

        private static string GetKeyVaultEndpoint() => "https://<YourKeyVaultName>.vault.azure.net";
    }
   ```

3. Sonraki About.cshtml.cs dosyasını açın ve aşağıdaki kodu yazın:
   1. Using deyimi tarafından bu Microsoft.Extensions.Configuration başvuru şunlardır:

       ```csharp
       using Microsoft.Extensions.Configuration
       ```

   1. Bu oluşturucu ekleyin:

       ```csharp
       public AboutModel(IConfiguration configuration)
       {
           _configuration = configuration;
       }
       ```

   1. OnGet yöntemi güncelleştirin. Yukarıdaki komutlar oluşturduğunuz gizli dizi adı ile burada gösterilen yer tutucu değerini güncelleştirin.

       ```csharp
       public void OnGet()
       {
           //Message = "Your application description page.";
           Message = "My key val = " + _configuration["<YourSecretNameThatWasCreatedAbove>"];
       }
       ```

Hakkında sayfasına göz atarak, uygulamayı yerel olarak çalıştırın. Alınan, gizli değer görmeniz gerekir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse kaynak grubunu silin. Bu, Key Vault ve ilgili kaynakları siler. Kaynak grubunu portal aracılığıyla silmek için:

1. Portalın üst kısmındaki Arama kutusuna kaynak grubunuzun adını girin. Bu Hızlı Başlangıçta kullanılan kaynak grubunu arama sonuçlarında gördüğünüzde seçin.
2. **Kaynak grubunu sil**'i seçin.
3. **KAYNAK GRUBU ADINI YAZIN:** kutusuna kaynak grubunun adını yazın ve **Sil**’i seçin.

## <a name="how-your-aspnet-core-project-is-modified"></a>ASP.NET Core projenizi nasıl değiştirilir

Bu bölümde ekleme Key Vault bağlı hizmeti Visual Studio kullanarak bir ASP.NET projesi için yapılan değişiklikleri tam tanımlar.

### <a name="added-references"></a>Ek başvurular

NuGet paket başvuruları ve proje dosya .NET başvuruları etkiler.

| Tür | Başvuru |
| --- | --- |
| NuGet | Microsoft.AspNetCore.AzureKeyVault.HostingStartup |

### <a name="added-files"></a>Eklenen dosyalar

- Bağlı hizmet sağlayıcısı, sürümü ve belgeleri bağlantısı hakkında bazı bilgiler kaydeder ConnectedService.json eklenir.

### <a name="project-file-changes"></a>Proje dosya değişiklikleri

- Bağlı Hizmetleri ItemGroup ve ConnectedServices.json dosya eklendi.

### <a name="launchsettingsjson-changes"></a>launchsettings.json changes

- IIS Express profili ve web projenizin adına eşleşen profili için aşağıdaki ortam değişkeni girdileri eklendi:

    ```json
      "environmentVariables": {
        "ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONENABLED": "true",
        "ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONVAULT": "<your keyvault URL>"
      }
    ```

### <a name="changes-on-azure"></a>Azure üzerindeki değişiklikler

- Bir kaynak grubu oluşturulur (veya var olan bir kullanılır).
- Belirtilen kaynak grubunda bir Key Vault oluşturdunuz.

## <a name="how-your-aspnet-framework-project-is-modified"></a>ASP.NET Framework projenizin nasıl değiştirilir

Bu bölümde ekleme Key Vault bağlı hizmeti Visual Studio kullanarak bir ASP.NET projesi için yapılan değişiklikleri tam tanımlar.

### <a name="added-references"></a>Ek başvurular

Proje .NET başvurulara etkiler ve `packages.config` (NuGet başvurularını).

| Tür | Başvuru |
| --- | --- |
| .NET; NuGet | Microsoft.Azure.KeyVault |
| .NET; NuGet | Microsoft.Azure.KeyVault.WebKey |
| .NET; NuGet | Microsoft.Rest.ClientRuntime |
| .NET; NuGet | Microsoft.Rest.ClientRuntime.Azure |

### <a name="added-files"></a>Eklenen dosyalar

- Bağlı hizmet sağlayıcısı, sürümü ve belgeleri bağlantısı hakkında bazı bilgiler kaydeder ConnectedService.json eklenir.

### <a name="project-file-changes"></a>Proje dosya değişiklikleri

- Bağlı Hizmetleri ItemGroup ve ConnectedServices.json dosya eklendi.
- Açıklanan .NET derlemesine ilişkin başvurular [eklenen başvuruları](#added-references) bölümü.

### <a name="webconfig-or-appconfig-changes"></a>Web.config veya app.config değişiklikleri

- Aşağıdaki yapılandırma girdileri eklendi:

    ```xml
    <configSections>
      <section
           name="configBuilders"
           type="System.Configuration.ConfigurationBuildersSection, System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" 
           restartOnExternalChanges="false"
           requirePermission="false" />
    </configSections>
    <configBuilders>
      <builders>
        <add 
             name="AzureKeyVault"
             vaultName="vaultname"
             type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder, Microsoft.Configuration.ConfigurationBuilders.Azure, Version=1.0.0.0, Culture=neutral" 
             vaultUri="https://vaultname.vault.azure.net" />
      </builders>
    </configBuilders>
    ```

### <a name="changes-on-azure"></a>Azure üzerindeki değişiklikler

- Bir kaynak grubu oluşturulur (veya var olan bir kullanılır).
- Belirtilen kaynak grubunda bir Key Vault oluşturdunuz.

## <a name="next-steps"></a>Sonraki adımlar

Key Vault geliştirme hakkında daha fazla bilgi edinmek [anahtar kasası Geliştirici Kılavuzu](key-vault-developers-guide.md)