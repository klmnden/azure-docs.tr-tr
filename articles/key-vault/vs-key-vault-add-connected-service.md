---
title: Key Vault desteği, Azure Key Vault Visual Studio kullanarak ASP.NET projenize ekleyin | Microsoft Docs
description: Bir ASP.NET veya ASP.NET Core web uygulaması için Key Vault desteği eklemek öğrenmenize yardımcı olmak için bu öğreticiyi kullanın.
services: key-vault
author: ghogen
manager: douge
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: ghogen
ms.openlocfilehash: de849ae290228826ee500ae1c7e623210e585d34
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58113257"
---
# <a name="add-key-vault-to-your-web-application-by-using-visual-studio-connected-services"></a>Key Vault, Visual Studio bağlı Hizmetler'i kullanarak web uygulamanıza ekleyin

Bu öğreticide, ASP.NET Core veya ASP.NET projesi herhangi bir türde kullanıp kullanmadığınızı Visual Studio'da, web projeleri, gizli verileri yönetmek için Azure anahtar kasası kullanmaya başlamak için ihtiyacınız olan her şeyi kolayca eklemek öğreneceksiniz. Visual Studio 2017'de bağlı hizmetler özelliğini kullanarak, Visual Studio otomatik olarak tüm Azure Key Vault'ta bağlanmanız gereken yapılandırma ayarlarını ve NuGet paketleri Ekle olabilir. 

Bağlı hizmetler anahtar Kasası'nı etkinleştirmek için projenizde yaptığı değişiklikleri hakkında daha fazla bilgi için bkz: [Key Vault bağlı my ASP.NET 4.7.1 ne hizmetine - proje](vs-key-vault-aspnet-what-happened.md) veya [Key Vault bağlı ne hizmetine - ASP.NET Core projeme](vs-key-vault-aspnet-core-what-happened.md).

## <a name="prerequisites"></a>Önkoşullar

- **Bir Azure aboneliği**. Bir aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
- **Web Geliştirme** iş yükünün yüklendiği **Visual Studio 2017 sürüm 15.7**. [Şimdi indir](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).
- ASP.NET (çekirdek değil), varsayılan olarak yüklü olmayan .NET Framework 4.7.1 geliştirme araçları gerekir. Bunları yüklemek için Visual Studio Yükleyicisi'ni başlatın, **Değiştir**ve ardından **tek tek bileşenler**, sonra sağ tarafında genişletmek **ASP.NET ve web geliştirme**ve **.NET Framework 4.7.1 geliştirme araçları**.
- 4.7.1 ASP.NET veya ASP.NET Core 2.0 web projesi açın.

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
3. Sonraki About.cshtml.cs dosyasını açın ve aşağıdaki kodu yazın
   1. Using deyimi tarafından bu Microsoft.Extensions.Configuration başvuru dahil    
       ```
       using Microsoft.Extensions.Configuration
       ```
   2. Bu oluşturucu Ekle
       ```
       public AboutModel(IConfiguration configuration)
       {
           _configuration = configuration;
       }
       ```
   3. OnGet yöntemi güncelleştirin. Yukarıdaki komutlar oluşturduğunuz gizli dizi adı ile burada gösterilen yer tutucu değerini güncelleştirin
       ```
       public void OnGet()
       {
           //Message = "Your application description page.";
           Message = "My key val = " + _configuration["<YourSecretNameThatWasCreatedAbove>"];
       }
       ```

Sayfa hakkında göz atarak, uygulamayı yerel olarak çalıştırın. Alınan, gizli değer gerekir

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse kaynak grubunu silin. Bu, Key Vault ve ilgili kaynakları siler. Kaynak grubunu portal aracılığıyla silmek için:

1. Portalın üst kısmındaki Arama kutusuna kaynak grubunuzun adını girin. Bu Hızlı Başlangıçta kullanılan kaynak grubunu arama sonuçlarında gördüğünüzde seçin.
2. **Kaynak grubunu sil**'i seçin.
3. **KAYNAK GRUBU ADINI YAZIN:** kutusuna kaynak grubunun adını yazın ve **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Key Vault geliştirme hakkında daha fazla bilgi edinmek [anahtar kasası Geliştirici Kılavuzu](key-vault-developers-guide.md)