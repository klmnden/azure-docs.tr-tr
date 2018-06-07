---
title: Anahtar kasası desteği Visual Studio kullanarak ASP.NET projenize ekleyin | Microsoft Docs
description: Bir ASP.NET veya ASP.NET Core web uygulamasına anahtar kasası desteği eklemek öğrenmenize yardımcı olmak için bu öğreticiyi kullanın.
services: key-vault
author: ghogen
manager: douge
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 04/15/2018
ms.author: ghogen
ms.openlocfilehash: b4fed559b6364149170dc8b1da421c9c3ee1203c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34635772"
---
# <a name="add-key-vault-to-your-web-application-by-using-visual-studio-connected-services"></a>Visual Studio bağlantılı hizmetler kullanarak web uygulamanıza anahtar kasası ekleyin

Bu öğreticide, ASP.NET Core veya ASP.NET projesi herhangi bir türde kullanıp kullanmadığınızı Visual Studio'da web projeleri için parolaları yönetmek için Azure anahtar kasası kullanmaya başlamak için ihtiyacınız olan her şeyi kolayca eklemenize olanak öğreneceksiniz. Visual Studio 2017 bağlantılı hizmetler özelliğini kullanarak, tüm Azure anahtar kasası bağlanmak için gereken yapılandırma ayarlarını ve NuGet paketleri otomatik olarak Ekle Visual Studio olabilir. 

Bağlantılı Hizmetler anahtar kasası etkinleştirmek için projenizde yaptığı değişiklikleri hakkında daha fazla bilgi için bkz: [anahtar kasası bağlı my ASP.NET 4.7.1 ne hizmeti - proje](vs-key-vault-aspnet-what-happened.md) veya [anahtar kasası bağlı ne oldu hizmeti - ASP.NET Core proje](vs-key-vault-aspnet-core-what-happened.md).

## <a name="prerequisites"></a>Önkoşullar

- **Bir Azure aboneliği**. Bir aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
- **Visual Studio 2017 sürüm 15.7** ile **Web geliştirme** yüklü iş yükü. [Şimdi indir](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).
- ASP.NET (çekirdek değildir) için varsayılan olarak yüklü olmayan .NET Framework 4.7.1 geliştirme araçları gerekir. Bunları yüklemek için Visual Studio yükleyicisi başlatın, seçin **Değiştir**ve ardından **bileşenleri tek tek**, sağ tarafta genişletin **ASP.NET ve web geliştirme**ve seçin **4.7.1 .NET Framework geliştirme araçları**.
- Bir ASP.NET 4.7.1 veya ASP.NET Core 2.0 web projesini açın.

## <a name="add-key-vault-support-to-your-project"></a>Anahtar kasası destek projenize ekleyin

1. İçinde **Çözüm Gezgini**, seçin **Ekle** > **bağlı hizmet**.
   Projenize eklemek Hizmetleri ile bağlı hizmet sayfası görüntülenir.
1. Kullanılabilir hizmetler menüde seçin **güvenli parolaları ile Azure anahtar kasası**.

   !["Azure anahtar kasası ile güvenli parolaları" seçin](media/vs-key-vault-add-connected-service/KeyVaultConnectedService1.PNG)

   Visual Studio uygulamasına oturum açtığınız ve hesabınızla ilişkili bir Azure aboneliğiniz varsa, bir açılır liste aboneliklerinizi ile bir sayfa görüntülenir.
1. Kullanın ve ardından yeni veya var olan bir anahtar kasası eklemek istediğiniz aboneliği seçin veya otomatik olarak oluşturulan adı değiştirmek için düzenleme bağlantısını seçin.

   ![Aboneliğinizi seçme](media/vs-key-vault-add-connected-service/KeyVaultConnectedService3.PNG)

1. Anahtar kasası için kullanmak istediğiniz adı yazın.

   ![Anahtar kasası yeniden adlandırın ve kaynak grubu seçin](media/vs-key-vault-add-connected-service/KeyVaultConnectedService-Edit.PNG)

1. Varolan bir kaynak grubu seçin veya bir otomatik olarak oluşturulan unqiue adıyla yeni bir tane oluşturmak seçin.  Farklı bir adla yeni bir grup oluşturmak istiyorsanız, kullanabileceğiniz [Azure Portal](https://portal.azure.com)ve ardından sayfasını kapatın ve kaynak gruplarının listesini yeniden yüklemek için yeniden başlatın.
1. Anahtar kasası oluşturmak üzere bir bölge seçin. Web uygulamanızın Azure üzerinde barındırılıyorsa, en iyi performansı için web uygulamasını barındıran bir bölge seçin.
1. Bir fiyatlandırma modelini seçin. Ayrıntılar için bkz [anahtar kasası fiyatlandırma](https://azure.microsoft.com/pricing/details/key-vault/).
1. Yapılandırma seçeneklerinin kabul etmek için Tamam'ı seçin.
1. Seçin **Ekle** anahtar kasası oluşturmak için. Zaten kullanılmış bir ad seçerseniz oluşturma işlemi başarısız olabilir.  Bu durumda, kullanın **Düzenle** bağlantı anahtar kasası yeniden adlandırabilir ve yeniden deneyin.

   ![Bağlı hizmet projesine ekleniyor](media/vs-key-vault-add-connected-service/KeyVaultConnectedService4.PNG)

1. Şimdi, Azure anahtar kasası bir gizlilik ekleyin. Portal doğru yerde almak için bu anahtar kasasında depolanan Yönet gizli için bağlantıyı tıklatın. Sayfa veya proje kapattıysanız, kendisine gidebilirsiniz [Azure portal](https://portal.azure.com) seçerek **tüm hizmetleri**altında **güvenlik**, seçin **anahtar kasası**, yeni oluşturduğunuz anahtar Kasası'ı seçin.

   ![Portala gezinme](media/vs-key-vault-add-connected-service/manage-secrets-link.jpg)

1. Anahtar için anahtar kasası bölümünde oluşturduğunuz kasa, seçin **gizli**, ardından **Oluştur/içe aktarma**.

   ![Bir gizli anahtarı oluştur/içe aktarma](media/vs-key-vault-add-connected-service/generate-secrets.jpg)

1. "Ettiyseniz" gibi bir parola girin ve bir test olarak herhangi bir dize değeri verin, sonra seçin **oluşturma** düğmesi.

   ![Gizli anahtar oluşturma](media/vs-key-vault-add-connected-service/create-a-secret.jpg)

1. (isteğe bağlı) Başka bir parolası, ancak "Gizli--ettiyseniz" adlandırarak bir kategoriye put bu kez girin. Gizli anahtarı "Ettiyseniz." içeren bir kategori "Gizli" Bu sözdizimini belirtir
 
Artık, kodda gizli anahtarlarınız erişebilirsiniz. Sonraki adım, ASP.NET 4.7.1 ya da ASP.NET Core kullanmanıza bağlı olarak farklı değildir.

## <a name="access-your-secrets-in-code-aspnet-core-projects"></a>Gizli anahtarlarınız (ASP.NET Core projeleri) kodda erişim

1. Visual Studio'da ASP.NET Core projenizde, artık bu Sırları kodda aşağıdaki ifadeler kullanarak başvurabilirsiniz:
 
   ```csharp
      config["MySecret"] // Access a secret without a section
      config["Secrets:MySecret"] // Access a secret in a section
      config.GetSection("Secrets")["MySecret"] // Get the configuration section and access a secret in it.
   ```

1. About.cshtml, söyleyin .cshtml sayfasında, eklemek @inject bir değişken ayarlamak üzere dosyanın en üstüne yakın yönerge anahtar kasası yapılandırmaya erişmek için kullanabilirsiniz.

   ```cshtml
      @inject Microsoft.Extensions.Configuration.IConfiguration config
   ```

1. Bir test olarak sayfalardan birinde görüntüleyerek gizli değerinin kullanılabilir olup olmadığını doğrulayabilirsiniz. Kullanım @config config değişkenine başvurmak için.
 
   ```cshtml
      <p> @config["MySecret"] </p>
      <p> @config.GetSection("Secrets")["MySecret"] </p>
      <p> @config["Secrets:MySecret"] </p>
   ```

1. Derleme ve web uygulamasını çalıştırmak, hakkında sayfasına gidin ve "gizli" değere bakın.

## <a name="access-your-secrets-in-code-aspnet-471-projects"></a>Gizli anahtarlarınız kod erişim (ASP.NET 4.7.1 projeler)

1. Web.config aşağıdaki gibi değiştirin. Anahtarları, gizli anahtar kasasında değerlerle AzureKeyVault ConfigurationBuilder tarafından değiştirilecek yer tutucuları şunlardır.

   ```xml
     <appSettings configBuilders="AzureKeyVault">
       <add key="webpages:Version" value="3.0.0.0" />
       <add key="webpages:Enabled" value="false" />
       <add key="ClientValidationEnabled" value="true" />
       <add key="UnobtrusiveJavaScriptEnabled" value="true" />
       <add key="MySecret" value="dummy1"/>
       <add key="Secrets--MySecret" value="dummy2"/>
     </appSettings>
   ```

1. Hakkında denetleyici yönteminde HomeController ViewBag depolamak ve gizli anahtarı almak için aşağıdaki satırları ekleyin.
 
   ```csharp
            var secret = ConfigurationManager.AppSettings["MySecret"];
            var secret2 = ConfigurationManager.AppSettings["Secrets--MySecret"];
            ViewBag.Secret = $"Secret: {secret}";
            ViewBag.Secret2 = $"Secret2: {secret2}";
   ```

1. About.cshtml Görünümü'nde (yalnızca test için) gizli anahtarı değerini görüntülemek için aşağıdakini ekleyin.

   ```csharp
      <h3>@ViewBag.Secret</h3>
      <h3>@ViewBag.Secret2</h3>
   ```

Tebrikler, web uygulamanızı anahtar kasası güvenli şekilde depolanan gizli erişmek için kullanabileceğiniz şimdi onayladıktan.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olduğunda, kaynak grubunu silin. Bu, anahtar kasası ve ilgili kaynakları siler. Kaynak grubunu portal aracılığıyla silmek için:

1. Portalın üst kısmındaki Arama kutusuna kaynak grubunuzun adını girin. Bu Hızlı Başlangıçta kullanılan kaynak grubunu arama sonuçlarında gördüğünüzde seçin.
2. **Kaynak grubunu sil**'i seçin.
3. **KAYNAK GRUBU ADINI YAZIN:** kutusuna kaynak grubunun adını yazın ve **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Anahtar kasası geliştirme hakkında daha fazla bilgi okuyarak [anahtar kasası Geliştirici Kılavuzu](key-vault-developers-guide.md)