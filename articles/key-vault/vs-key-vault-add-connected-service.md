---
title: Key Vault desteği Visual Studio kullanarak ASP.NET projenize ekleyin | Microsoft Docs
description: Bir ASP.NET veya ASP.NET Core web uygulaması için Key Vault desteği eklemek öğrenmenize yardımcı olmak için bu öğreticiyi kullanın.
services: key-vault
author: ghogen
manager: douge
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 04/15/2018
ms.author: ghogen
ms.openlocfilehash: c90ef26c0170db67b1d422701b6969ca3f9c9e38
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49958525"
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

1. Mevcut bir kaynak grubunu seçin ya da otomatik olarak oluşturulan benzersiz bir ada sahip yeni bir tane seçin.  Farklı bir adla yeni bir grup oluşturmak istiyorsanız, kullanabileceğiniz [Azure portalı](https://portal.azure.com)ve sonra sayfayı kapatın ve kaynak gruplarının listesi yeniden yüklemek için yeniden başlatın.
1. Key Vault oluşturulacağı bölgeyi seçin. Web uygulamanızı Azure'da barındırılıyorsa, en iyi performans için web uygulamasını barındıran bölgeyi seçin.
1. Bir fiyatlandırma modelini seçin. Ayrıntılar için bkz [anahtar kasası fiyatlandırma](https://azure.microsoft.com/pricing/details/key-vault/).
1. Yapılandırma seçimlerini kabul etmek için Tamam'ı seçin.
1. Seçin **Ekle** anahtar kasası oluşturmak için. Zaten kullanılan bir ad seçerseniz oluşturma işlemi başarısız olabilir.  Bu durumda kullanın **Düzenle** bağlantı Key Vault yeniden adlandırıp tekrar deneyin.

   ![Bağlı hizmet projeye ekleniyor](media/vs-key-vault-add-connected-service/KeyVaultConnectedService4.PNG)

1. Şimdi, Azure anahtar Kasası'nda bir gizli dizi ekleyin. Portalı'nda doğru yere almak için bu anahtar Kasası'nda depolanan Yönet gizli bağlantısına tıklayın. Sayfa veya proje kapattıysanız, kendisine gidebilirsiniz [Azure portalında](https://portal.azure.com) seçerek **tüm hizmetleri**altında **güvenlik**, seçin **Key Vault**, yeni oluşturduğunuz anahtar Kasası'nı seçin.

   ![Portalında gezinme](media/vs-key-vault-add-connected-service/manage-secrets-link.jpg)

1. Anahtar için Key Vault bölümünde oluşturduğunuz kasa öğesini **gizli dizileri**, ardından **Oluştur/içeri aktarma**.

   ![Gizli dizi Oluştur/İçeri Aktar](media/vs-key-vault-add-connected-service/generate-secrets.jpg)

1. "Ettiyseniz" gibi bir gizli dizi girin ve bir test olarak herhangi bir dize değeri verin ve ardından seçin **Oluştur** düğmesi.

   ![Gizli anahtar oluşturma](media/vs-key-vault-add-connected-service/create-a-secret.jpg)

1. (isteğe bağlı) Başka bir gizli dizi, ancak bu kez "Gizli dizileri--ettiyseniz" adlandırarak bir kategoriye put girin. Bu söz dizimi içeren bir gizli dizi "Ettiyseniz." Kategori "Gizli" belirtir.
 
Artık, kod, gizli dizileri erişebilirsiniz. Sonraki adımlar 4.7.1 ASP.NET veya ASP.NET Core kullanmanıza bağlı olarak farklılık gösterir.

## <a name="access-your-secrets-in-code-aspnet-core-projects"></a>Kod (ASP.NET Core projeleri için), gizli erişim

Key Vault bağlantısı başlangıçta uygulayan bir sınıf tarafından ayarlanmış [Microsoft.AspNetCore.Hosting.IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup?view=aspnetcore-2.1) açıklanan başlangıç davranışını genişletme yöntemi kullanarak [harici bir uygulama geliştirin ASP.NET Core ile Ihostingstartup derlemede](/aspnet/core/fundamentals/host/platform-specific-configuration). Key Vault bağlantı bilgilerini içeren iki ortam değişkenlerini başlangıç sınıfı kullanır: ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONENABLED ayarlamak, doğru ve ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONVAULT, anahtarı ayarlayın Vault URL'si. Çalıştırdığınızda bu launchsettings.json dosyasına eklenir **bağlı hizmet Ekle** işlem.

Gizli anahtarlarınız erişmek için:

1. Visual Studio kullanarak ASP.NET Core projenizde, artık bu gizli dizileri kodda aşağıdaki ifadeleri kullanarak başvurabilirsiniz:
 
   ```csharp
      config["MySecret"] // Access a secret without a section
      config["Secrets:MySecret"] // Access a secret in a section
      config.GetSection("Secrets")["MySecret"] // Get the configuration section and access a secret in it.
   ```

1. Söyleyin About.cshtml, .cshtml sayfasında, eklemek @inject yönerge bir değişkeni ayarlamak için dosyasının en üstüne yakın anahtar kasası yapılandırması ile erişmek için kullanabilirsiniz.

   ```cshtml
      @inject Microsoft.Extensions.Configuration.IConfiguration config
   ```

1. Bir test olarak sayfalardan birine görüntüleyerek gizli değeri, kullanılabilir olduğunu doğrulayabilirsiniz. Kullanım @config config değişkenine başvurmak için.
 
   ```cshtml
      <p> @config["MySecret"] </p>
      <p> @config.GetSection("Secrets")["MySecret"] </p>
      <p> @config["Secrets:MySecret"] </p>
   ```

1. Derleme ve web uygulamasını çalıştırmak, hakkında sayfasına gidin ve "gizli" değere bakın.

## <a name="access-your-secrets-in-code-aspnet-471-projects"></a>Kod, gizli erişim (ASP.NET 4.7.1 projeleri)

Key Vault'unuza bağlantısı çalıştırdığınızda, web.config dosyasına eklenen bilgileri kullanarak ConfigurationBuilder sınıfı tarafından ayarlanır **bağlı hizmet Ekle** işlem.

Gizli anahtarlarınız erişmek için:

1. Web.config şu şekilde değiştirin. Anahtarları Key Vault'ta gizli diziler değerleriyle AzureKeyVault ConfigurationBuilder ile değiştirilecek tutuculardır.

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

1. Hakkında denetleyici yönteminde HomeController sırrı alınmaya ve içinde görünüm paketini depolamak için aşağıdaki satırları ekleyin.
 
   ```csharp
            var secret = ConfigurationManager.AppSettings["MySecret"];
            var secret2 = ConfigurationManager.AppSettings["Secrets--MySecret"];
            ViewBag.Secret = $"Secret: {secret}";
            ViewBag.Secret2 = $"Secret2: {secret2}";
   ```

1. About.cshtml Görünümü'nde (yalnızca test için) gizli dizi değerini görüntülemek için aşağıdakini ekleyin.

   ```csharp
      <h3>@ViewBag.Secret</h3>
      <h3>@ViewBag.Secret2</h3>
   ```

1. Azure portalında, yapılandırma dosyasından değil kukla değer girdiğiniz gizli değer okuyabilirsiniz yerel olarak doğrulamak için uygulamanızı çalıştırın.

Ardından, uygulamanızı Azure'a yayımlayın.

## <a name="publish-to-azure-app-service"></a>Azure App Service'e yayımlama

1. Proje düğümünü sağ tıklatın ve seçin **Yayımla**. Bildiren bir ekran görünür **bir yayımlama hedefi seçin**. Sol tarafta, seçin **App Service**, ardından **Yeni Oluştur**.

   ![App Service'te yayımlama](media/vs-key-vault-add-connected-service/AppServicePublish1.PNG)

1. Üzerinde **App Service Oluştur** ekranında, abonelik ve kaynak grubu, anahtar Kasası'nda oluşturulan aynıdır ve seçin emin **Oluştur**.

   ![App Service oluşturun](media/vs-key-vault-add-connected-service/AppServicePublish2.PNG)

1. Web uygulamanızı oluşturduktan sonra **Yayımla** ekranı görüntülenir. Azure'da barındırılan yayımlanan web uygulamanız için URL'yi not alın. Görürseniz **hiçbiri** yanındaki **Key Vault**, App Service bağlanmak için hangi Key Vault bildirmek çözümlenmedi. Seçin **ekleme Key Vault** bağlantısını ve oluşturduğunuz anahtar Kasası'nı seçin.

   ![Anahtar kasası Ekle](media/vs-key-vault-add-connected-service/AppServicePublish3.PNG)

   Görürseniz **yönetme Key Vault**, düzenleme izinlerine geçerli ayarları görüntülemek için tıklayın veya Azure Portalı'nda, gizli dizileri için değişiklikler yapın.

1. Şimdi, tarayıcıda web uygulamasını ziyaret etmek için Site URL'sini bağlantıyı seçin. Anahtar Kasası'ndaki doğru değeri gördüğünüzü doğrulayın.

Tebrikler, web uygulamanızı Azure'da çalıştırdığınızda güvenli şekilde depolanan gizli dizileri erişmek için Key Vault'u kullanabilir onaylandı.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse kaynak grubunu silin. Bu, Key Vault ve ilgili kaynakları siler. Kaynak grubunu portal aracılığıyla silmek için:

1. Portalın üst kısmındaki Arama kutusuna kaynak grubunuzun adını girin. Bu Hızlı Başlangıçta kullanılan kaynak grubunu arama sonuçlarında gördüğünüzde seçin.
2. **Kaynak grubunu sil**'i seçin.
3. **KAYNAK GRUBU ADINI YAZIN:** kutusuna kaynak grubunun adını yazın ve **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Key Vault geliştirme hakkında daha fazla bilgi edinmek [anahtar kasası Geliştirici Kılavuzu](key-vault-developers-guide.md)