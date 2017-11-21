---
title: "Güvenli bir şekilde bir web uygulaması için gizli uygulama ayarları kaydediliyor | Microsoft Docs"
description: "Güvenli bir şekilde Azure kimlik bilgileri veya üçüncü taraf API gibi gizli uygulama ayarlarını kaydetmek için ASP.NET kullanmayı anahtarları anahtar kasası sağlayıcısı, kullanıcı gizli ya da .NET 4.7.1 çekirdek nasıl yapılandırma oluşturucular"
services: visualstudio
documentationcenter: 
author: cawa
manager: paulyuk
editor: 
ms.assetid: 
ms.service: 
ms.workload: web, azure
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: cawa
ms.openlocfilehash: 3284f9f9c3cef27cba599238f06b0dcf0f35de78
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2017
---
# <a name="securely-save-secret-application-settings-for-a-web-application"></a>Güvenli bir şekilde bir web uygulaması için gizli uygulama ayarlarını Kaydet

## <a name="overview"></a>Genel Bakış
Bu makalede, güvenli bir şekilde Azure uygulamalarını gizli uygulama yapılandırma ayarlarını kaydetmek açıklar.

Geleneksel olarak tüm uygulama yapılandırması gibi Web.config yapılandırma dosyalarını kaydedilir web. Bu yöntem, Github gibi ortak kaynak denetim sistemleri için bulut kimlik bilgileri gibi gizli ayarlarında denetimi için yol gösterir. Bu sırada, en iyi güvenlik uygulaması kaynak kodunu değiştirmek ve geliştirme ayarlarını yeniden yapılandırmak için gerekli olan ek yük nedeniyle izleyin zor olabilir.

Geliştirme sürecinin güvenli olduğundan emin olmak için uygulama gizli ayarlarıyla güvenli bir şekilde minimal veya hiçbir kaynak kod değişikliği kaydetmek için araç ve framework kitaplıkları oluşturulur.

## <a name="aspnet-and-net-core-applications"></a>ASP.NET ve .NET core uygulamaları

### <a name="save-secret-settings-in-user-secret-store-that-is-outside-of-source-control-folder"></a>Kaynak denetimi klasörüne dışında kullanıcı deposunda gizli ayarlarını Kaydet
Hızlı bir prototip yapmakta olduğunuz veya internet erişimi yoksa, gizli ayarlarınızı kaynak denetimi klasörü dışında kullanıcı gizliliği depoya taşımak ile başlatın. Kullanıcı gizli deposu gizli kaynak denetimine iade edilmez şekilde kullanıcı profil oluşturucu klasörü altında kaydedilen bir dosyadır. Aşağıdaki diyagramda gösterilmiştir nasıl [kullanıcı gizliliği](https://docs.microsoft.com/en-us/aspnet/core/security/app-secrets?tabs=visual-studio#SecretManager) çalışır.

![Kullanıcı parolası gizli ayarları kaynak denetimi dışında tutar](./media/vs-secure-secret-appsettings/aspnetcore-usersecret.PNG)

.NET core konsol uygulaması çalıştırıyorsanız, gizli anahtarı güvenli bir şekilde kaydetmek için anahtar Kasası'nı kullanın.

### <a name="save-secret-settings-in-azure-key-vault"></a>Azure anahtar kasası gizli ayarlarını Kaydet
Bir takım projesi geliştirme ve kaynak kodu güvenli bir şekilde paylaşmaya gereksinim kullanın [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/).

1. Azure aboneliğinizde bir anahtar kasası oluşturun. ' A tıklayın ve kullanıcı Arabirimi tüm gerekli alanları doldurun *oluşturma* dikey pencerenin altındaki üzerinde

    ![Azure anahtar kasası oluşturma](./media/vs-secure-secret-appsettings/create-keyvault.PNG)

2. Sizin ve ekip üyelerinizin anahtar Kasası'na erişim verin. Büyük bir ekibiniz varsa, oluşturabileceğiniz bir [Azure Active Directory grubu](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-groups-create-azure-portal) ve bu güvenlik grubu erişim anahtar Kasası'na ekleyin. İçinde *gizli izinleri* açılan listesinde, onay *almak* ve *listesi* altında *gizli yönetim işlemlerini*.

    ![Anahtar kasası Erişim İlkesi Ekle](./media/vs-secure-secret-appsettings/add-keyvault-access-policy.png)

3. Gizli anahtar Kasası'na Azure Portal'da ekleyin. İç içe geçmiş yapılandırma ayarları için Değiştir ':' ile '--' anahtar kasası gizli anahtar adı geçerli olacak şekilde. ':' anahtar kasası adına gizli olmasını izin verilmiyor.

    ![Anahtar kasası gizli Ekle](./media/vs-secure-secret-appsettings/add-keyvault-secret.png)

4. Yükleme [Visual Studio için Azure Hizmetleri kimlik doğrulaması uzantısı](https://go.microsoft.com/fwlink/?linkid=862354). Bu uzantısı aracılığıyla uygulama anahtar kasası oturum açma Visual Studio kimliğini kullanarak erişebilirsiniz.

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
7. Anahtar kasası URL'si launchsettings.json dosyasına ekleyin. Ortam değişkeni adı *KEYVAULT_ENDPOINT* 6. adımda eklediğiniz kodun tanımlanır.

    ![Anahtar kasası URL'si proje ortam değişkeni ekleme](./media/vs-secure-secret-appsettings/add-keyvault-url.png)

8. Projeyi hata ayıklamayı Başlat. Başarılı bir şekilde çalıştırılması.

## <a name="aspnet-and-net-applications"></a>ASP.NET ve .NET uygulamaları

.NET 4.7.1 anahtar kasası ve gizli yapılandırma Oluşturucular, gizli kod değişiklikleri olmadan kaynak denetimi klasörü dışında taşınabilir sağlar destekler.
Devam etmek için [.NET 4.7.1 karşıdan](https://www.microsoft.com/download/details.aspx?id=56115) ve .NET framework'ün daha eski bir sürümünü kullanıyorsanız, uygulamanızı geçirmek.

### <a name="save-secret-settings-in-a-secret-file-that-is-outside-of-source-control-folder"></a>Kaynak denetim klasörü dışında bir gizli dosya gizli ayarları kaydedin
Hızlı bir prototip yazma ve Azure kaynakları sağlamak üzere istemiyorsanız, bu seçenek ile gidin.

1. Aşağıdaki NuGet paketini projenize yükleyin
    ```
    Microsoft.Configuration.ConfigurationBuilders.Basic.1.0.0-alpha1.nupkg
    ```

2. İzleme için benzer bir dosya oluşturun. Altında proje klasörünüzdeki dışındaki bir konuma kaydedin.

    ```xml

       <root>
              <secrets ver="1.0">
                     <secret name="secret1" value="foo_one" />
                        <secret name="secret2" value="foo_two" />
                       </secrets>
      </root>
      ```

3. Web.config dosyanızda yapılandırma oluşturucusu olması gizli dosyasını tanımlayın. Önce bu bölümdeki put *appSettings* bölümü.

    ```xml
    <configBuilders>
        <builders>
            <add name="Secrets"
                 secretsFile="C:\Users\AppData\MyWebApplication1\secret.xml" type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
                    Microsoft.Configuration.ConfigurationBuilders, Version=1.0.0.0, Culture=neutral" />
        </builders>
    </configBuilders>
    ```

4. AppSettings bölümünü gizli yapılandırma Oluşturucusu'nu kullanarak belirtin. Bir kukla değer gizli ayarı için herhangi bir giriş olduğundan emin olun.

    ```xml
        <appSettings configBuilders="Secrets">
            <add key="webpages:Version" value="3.0.0.0" />
            <add key="webpages:Enabled" value="false" />
            <add key="ClientValidationEnabled" value="true" />
            <add key="UnobtrusiveJavaScriptEnabled" value="true" />
            <add key="secret" value="" />
        </appSettings>
    ```

5. Uygulamanızın hatalarını ayıklama. Başarılı bir şekilde çalıştırılması.

### <a name="save-secret-settings-in-an-azure-key-vault"></a>Bir Azure anahtar kasasına gizli ayarlarını Kaydet
Projeniz için bir anahtar kasası yapılandırmak için ASP.NET core bölümündeki yönergeleri izleyin.

1. Aşağıdaki NuGet paketini projenize yükleyin
```
Microsoft.Configuration.ConfigurationBuilders.Azure.1.0.0-alpha1.nupkg
```

2. Anahtar kasası yapılandırma Oluşturucu Web.config dosyasında tanımlayın. Önce bu bölümdeki put *appSettings* bölümü. Değiştir *vaultName* anahtar kasanızı ortak Azure veya tam URI ise Sovereign bulut kullanıyorsanız, anahtar kasası adı olmalıdır.

    ```xml
    <configSections>
        <section name="configBuilders" type="System.Configuration.ConfigurationBuildersSection, System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" restartOnExternalChanges="false" requirePermission="false" />
    </configSections>
    <configBuilders>
        <builders>
            <add name="KeyVault" vaultName="Test911" type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder, ConfigurationBuilders, Version=1.0.0.0, Culture=neutral" />
        </builders>
    </configBuilders>
    ```
3.  Anahtar kasası yapılandırma Oluşturucu appSettings bölümünü kullanarak belirtin. Bir kukla değer gizli ayarı için herhangi bir giriş olduğundan emin olun.

    ```xml
    <appSettings configBuilders="AzureKeyVault">
        <add key="webpages:Version" value="3.0.0.0" />
        <add key="webpages:Enabled" value="false" />
        <add key="ClientValidationEnabled" value="true" />
        <add key="UnobtrusiveJavaScriptEnabled" value="true" />
        <add key="secret" value="" />
    </appSettings>
    ```

4. Projeyi hata ayıklamayı Başlat. Başarılı bir şekilde çalıştırılması.
