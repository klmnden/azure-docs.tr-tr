---
title: Güvenli bir şekilde bir web uygulaması - Azure anahtar kasası için gizli uygulama ayarları kaydediliyor. | Microsoft Docs
description: Güvenli bir şekilde Azure kimlik bilgileri veya üçüncü taraf API gibi gizli uygulama ayarlarını kaydetmek için ASP.NET kullanarak anahtarlarını Key Vault sağlayıcısının, kullanıcının gizliliği ya da .NET 4.7.1 çekirdek nasıl yapılandırma oluşturucular
services: visualstudio
author: cawaMS
manager: paulyuk
editor: ''
ms.service: key-vault
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: cawa
ms.openlocfilehash: 79b1c740bca56982243ddc130d8747fdc955247f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60304879"
---
# <a name="securely-save-secret-application-settings-for-a-web-application"></a>Güvenli bir şekilde bir web uygulaması için gizli uygulama ayarlarını Kaydet

## <a name="overview"></a>Genel Bakış
Bu makalede, güvenli bir şekilde Azure uygulamaları için gizli bir uygulama yapılandırma ayarlarını kaydetmek açıklar.

Geleneksel olarak tüm uygulama yapılandırma ayarları gibi Web.config yapılandırma dosyalarında kaydedilir web. Bu yöntem, genel kaynak denetimi sistemlerine GitHub gibi bulut kimlik bilgileri gibi gizli dizi ayarlarını iade için yol açar. Bu arada, kaynak kodunu değiştirin ve geliştirme ayarlarını yeniden yapılandırmanız için gerekli ek yükü nedeniyle iyi güvenlik uygulamalarını takip zor olabilir.

Geliştirme işlemi güvenli olduğundan emin olmak için uygulama gizli ayarları güvenli bir şekilde değişikliğiyle veya hiç kaynak kod değişikliği kaydetmek için Araçlar ve framework kitaplıkları oluşturulur.

## <a name="aspnet-and-net-core-applications"></a>ASP.NET ve .NET core uygulamaları

### <a name="save-secret-settings-in-user-secret-store-that-is-outside-of-source-control-folder"></a>Kaynak denetim klasörü dışında kullanıcı gizli dizi deposu gizli dizi ayarlarını kaydedin
Hızlı bir prototip yapıyor veya internet erişimi yoksa, kullanıcı gizli dizi deposu için kaynak denetim klasörü dışında gizli ayarlarınızı taşıma ile başlayın. Kullanıcı gizli dizi deposu, gizli dizileri kaynak denetimine iade edilmez için kullanıcı profil oluşturucu klasörü altında kaydedilen bir dosyadır. Aşağıdaki diyagramda gösterilmiştir nasıl [kullanıcı gizliliği](https://docs.microsoft.com/aspnet/core/security/app-secrets?tabs=visual-studio) çalışır.

![Kullanıcı gizliliği gizli dizi ayarlarını kaynak denetimi dışında tutar.](./media/vs-secure-secret-appsettings/aspnetcore-usersecret.PNG)

.NET core konsol uygulaması çalıştırıyorsanız, gizli anahtarı güvenli bir şekilde kaydetmek için Key Vault'u kullanın.

### <a name="save-secret-settings-in-azure-key-vault"></a>Azure anahtar Kasası'nda gizli dizi ayarlarını Kaydet
Bir proje geliştiriyorsanız ve güvenli bir şekilde kaynak kodu paylaşmak ihtiyacınız varsa, kullanmak [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/).

1. Azure aboneliğinizde bir Key Vault oluşturacaksınız. UI tıklayıp gerekli tüm alanları doldurun *Oluştur* dikey pencerenin alt kısmındaki

    ![Azure Key Vault oluşturma](./media/vs-secure-secret-appsettings/create-keyvault.PNG)

2. Siz ve ekip üyeleriniz, anahtar Kasası'na erişim verin. Büyük bir ekibiniz varsa, oluşturabileceğiniz bir [Azure Active Directory grubu](https://docs.microsoft.com/azure/active-directory/active-directory-groups-create-azure-portal) ve bu güvenlik grubuna erişim anahtar Kasası'na ekleyin. İçinde *gizli dizi izinleri* açılır listesinde, onay *alma* ve *listesi* altında *gizli dizi yönetimi işlemleri*.

    ![Anahtar kasası erişim ilkesi ekleme](./media/vs-secure-secret-appsettings/add-keyvault-access-policy.png)

3. Gizli anahtar Kasası'na, Azure Portal'da ekleyin. İç içe geçmiş yapılandırma ayarları için Değiştir ':' ile '--' Key Vault'a gizli dizi adı geçerli olacak şekilde. ':' adında bir Key Vault gizli olması izin verilmiyor.

    ![Key Vault gizli Ekle](./media/vs-secure-secret-appsettings/add-keyvault-secret.png)

4. Yükleme [Visual Studio için Azure Hizmetleri kimlik doğrulaması uzantısı](https://go.microsoft.com/fwlink/?linkid=862354). Bu uzantı Visual Studio oturum açma kimliğini kullanarak Key Vault uygulama erişebilirsiniz.

5. Aşağıdaki NuGet paketlerini projenize ekleyin:

    ```
    Microsoft.Azure.Services.AppAuthentication
    ```
6. Program.cs dosyasına aşağıdaki kodu ekleyin:

    ```csharp
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
                    })
                    .UseStartup<Startup>()
                    .Build();

        private static string GetKeyVaultEndpoint() => Environment.GetEnvironmentVariable("KEYVAULT_ENDPOINT");
    ```
7. Anahtar kasası URL'nizi launchsettings.json dosyasına ekleyin. Ortam değişkeni adı *KEYVAULT_ENDPOINT* 6. adımda eklediğiniz kod içinde tanımlanır.

    ![Key Vault URL'si bir proje ortam değişkeni Ekle](./media/vs-secure-secret-appsettings/add-keyvault-url.png)

8. Projede hata ayıklamaya başlayın. Başarılı bir şekilde çalışması gerekir.

## <a name="aspnet-and-net-applications"></a>ASP.NET ve .NET uygulamaları

.NET 4.7.1 yapılandırma Oluşturucular, Key Vault ve gizli anahtarını da gizli kod değişikliği yapmadan kaynak denetim klasörü dışında taşınabilir sağlar destekler.
Devam etmek için [.NET 4.7.1 indirme](https://www.microsoft.com/download/details.aspx?id=56115) ve .NET framework'ün daha eski bir sürümünü kullanıyorsanız, uygulamanızı geçirme.

### <a name="save-secret-settings-in-a-secret-file-that-is-outside-of-source-control-folder"></a>Gizli dizi ayarlarını kaynak denetim klasörü dışında olan bir gizli dizi dosyasını kaydedin.
Hızlı bir prototip yazma ve Azure kaynaklarını hazırlayacak istemiyorsanız, bu seçenekle gidin.

1. Aşağıdaki NuGet paketini projenize yükleyin.
    ```
    Microsoft.Configuration.ConfigurationBuilders.Basic
    ```

2. İzleme için benzer bir dosya oluşturun. Altındaki proje klasörünüzün dışında bir konuma kaydedin.

    ```xml
    <root>
        <secrets ver="1.0">
            <secret name="secret1" value="foo_one" />
            <secret name="secret2" value="foo_two" />
        </secrets>
    </root>
    ```

3. Bir yapılandırma Oluşturucu, Web.config dosyasında olmasını gizli dizi dosyasını tanımlayın. Bu bölümde önce yerleştirin *appSettings* bölümü.

    ```xml
    <configBuilders>
        <builders>
            <add name="Secrets"
                 secretsFile="C:\Users\AppData\MyWebApplication1\secret.xml" type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
                    Microsoft.Configuration.ConfigurationBuilders, Version=1.0.0.0, Culture=neutral" />
        </builders>
    </configBuilders>
    ```

4. AppSettings bölümünde, gizli yapılandırma Oluşturucusu'nu kullanarak belirtin. Gizli bir kukla değer ayarı için herhangi bir giriş olduğundan emin olun.

    ```xml
        <appSettings configBuilders="Secrets">
            <add key="webpages:Version" value="3.0.0.0" />
            <add key="webpages:Enabled" value="false" />
            <add key="ClientValidationEnabled" value="true" />
            <add key="UnobtrusiveJavaScriptEnabled" value="true" />
            <add key="secret" value="" />
        </appSettings>
    ```

5. Uygulamanızda hata ayıklama. Başarılı bir şekilde çalışması gerekir.

### <a name="save-secret-settings-in-an-azure-key-vault"></a>Bir Azure anahtar Kasası'nda gizli dizi ayarlarını Kaydet
Projeniz için bir anahtar Kasası'nı yapılandırmak için ASP.NET core bölümündeki yönergeleri izleyin.

1. Aşağıdaki NuGet paketini projenize yükleyin.
   ```
   Microsoft.Configuration.ConfigurationBuilders.UserSecrets
   ```

2. Key Vault yapılandırma Oluşturucu, Web.config içinde tanımlayın. Bu bölümde önce yerleştirin *appSettings* bölümü. Değiştirin *vaultName* Key Vault'unuza genel Azure veya tam URI ise bağımsız bulut kullanıyorsanız, anahtar kasası adı olacak.

    ```xml
    <configSections>
        <section name="configBuilders" type="System.Configuration.ConfigurationBuildersSection, System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" restartOnExternalChanges="false" requirePermission="false" />
    </configSections>
    <configBuilders>
        <builders>
            <add name="AzureKeyVault" vaultName="Test911" type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder, ConfigurationBuilders, Version=1.0.0.0, Culture=neutral" />
        </builders>
    </configBuilders>
    ```
3. AppSettings bölümünde, Key Vault yapılandırma Oluşturucusu'nu kullanarak belirtin. Gizli bir kukla değer ayarı için herhangi bir giriş olduğundan emin olun.

   ```xml
   <appSettings configBuilders="AzureKeyVault">
       <add key="webpages:Version" value="3.0.0.0" />
       <add key="webpages:Enabled" value="false" />
       <add key="ClientValidationEnabled" value="true" />
       <add key="UnobtrusiveJavaScriptEnabled" value="true" />
       <add key="secret" value="" />
   </appSettings>
   ```

4. Projede hata ayıklamaya başlayın. Başarılı bir şekilde çalışması gerekir.
