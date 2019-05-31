---
title: Azure ile tümleştirilmesini yönetilen kimlikleri | Microsoft Docs
description: Azure'ı kullanmayı öğrenin yönetilen kimlikleri ile kimlik doğrulaması ve Azure uygulama yapılandırması erişim kazanmak için
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: na
ms.topic: conceptual
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: 3977991386dbcd07e92f21d1ac541f486b4f7f0a
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393659"
---
# <a name="integrate-with-azure-managed-identities"></a>Azure ile tümleştirilmesini yönetilen kimlikleri

Azure Active Directory [yönetilen kimlikleri](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) bulut uygulamanız için gizli dizileri Yönetimi basitleştirin. Yönetilen bir kimlikle Azure bilgi işlem hizmetinin üzerinde çalıştığı için oluşturulan hizmet sorumlusu kullanmak kodunuzu oluşturan ayarlayabilirsiniz. Azure anahtar kasası veya bir yerel bağlantı dizesi içinde depolanan ayrı bir kimlik bilgisi yerine yönetilen bir kimlik kullanın. 

Azure uygulama yapılandırması ve kendi .NET Core, .NET ve Java Spring istemci kitaplıkları, yönetilen hizmet kimliği (MSI) destekleyen yerleşik gelir. Bunu kullanmak için gerekli değildir ancak MSI gizli dizileri içeren bir erişim belirteci ihtiyacını ortadan kaldırır. Kodunuzu yalnızca erişebilmesi için depolama hizmet uç uygulama yapılandırması için bilmeniz gerekir. Bu URL, kodunuzdaki herhangi bir gizli dizi gösterme, doğrudan kaygısı olmadan ekleyebilir.

Bu öğreticide, uygulama yapılandırması ile erişmek için MSI avantajlarından nasıl gerçekleştirebileceğiniz gösterilir. Bu hızlı başlangıçlar, sunulan web uygulaması oluşturur. Devam etmeden önce [uygulama yapılandırması ile bir ASP.NET Core uygulaması oluşturma](./quickstart-aspnet-core-app.md) ilk.

Bu öğreticideki adımları uygulamak için herhangi bir kod Düzenleyicisi'ni kullanabilirsiniz. [Visual Studio Code](https://code.visualstudio.com/) Windows, macOS ve Linux platformlarını mükemmel bir seçenek kullanılabilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Uygulama yapılandırması için bir yönetilen kimlik erişim.
> * Uygulama yapılandırması bağlandığınızda yönetilen bir kimlik kullanacak şekilde yapılandırın.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gereklidir:

* [.NET core SDK'sı](https://www.microsoft.com/net/download/windows).
* [Azure Cloud Shell yapılandırılmış](https://docs.microsoft.com/azure/cloud-shell/quickstart).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="add-a-managed-identity"></a>Yönetilen bir kimlik Ekle

Portalda yönetilen bir kimlik ayarlamak için ilk olarak normal bir uygulama oluşturun ve ardından özelliği etkinleştirin.

1. Bir uygulama oluşturun [Azure portalında](https://portal.azure.com) normalde yaptığınız gibi. Portalda gidin.

2. Ekranı aşağı kaydırarak **ayarları** grup sol bölmede, belirleyin **kimlik**.

3. Üzerinde **sistem tarafından atanan** sekmesinde, geçiş **durumu** için **üzerinde** seçip **Kaydet**.

    ![App Service'te yönetilen kimlik ayarlayın](./media/set-managed-identity-app-service.png)

## <a name="grant-access-to-app-configuration"></a>Uygulama yapılandırması için erişim izni ver

1. İçinde [Azure portalında](https://portal.azure.com)seçin **tüm kaynakları** ve hızlı başlangıç bölümünde oluşturduğunuz uygulama yapılandırma deposu seçin.

2. Seçin **erişim denetimi (IAM)** .

3. Üzerinde **denetleyin erişim** sekmesinde **Ekle** içinde **rol ataması Ekle** UI kartı.

4. Altında **rol**seçin **katkıda bulunan**. Altında **erişim Ata**seçin **App Service** altında **sistem atanan yönetilen kimlik**.

5. Altında **abonelik**, Azure aboneliğinizi seçin. Uygulamanız için App Service kaynak seçin.

6. **Kaydet**’i seçin.

    ![Yönetilen bir kimlik Ekle](./media/add-managed-identity.png)

## <a name="use-a-managed-identity"></a>Yönetilen kimlik kullanma

1. Açık *appsettings.json*ve aşağıdaki betiği ekleyin. Değiştirin  *\<service_endpoint >* , köşeli parantez ile uygulama yapılandırma deposu URL'si de dahil olmak üzere:

    ```json
    "AppConfig": {
        "Endpoint": "<service_endpoint>"
    }
    ```

2. Açık *Program.cs*ve güncelleştirme `CreateWebHostBuilder` değiştirerek yöntemi `config.AddAzureAppConfiguration()` yöntemi.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(options =>
                    options.ConnectWithManagedIdentity(settings["AppConfig:Endpoint"]));
            })
            .UseStartup<Startup>();
    ```

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

## <a name="deploy-from-local-git"></a>Yerel Git’ten dağıtım

Kudu derleme sunucusu ile uygulamanıza yönelik yerel Git dağıtımını etkinleştirmek için en kolay yolu, Azure Cloud Shell kullanmaktır.

### <a name="configure-a-deployment-user"></a>Dağıtım kullanıcısı yapılandırma

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="enable-local-git-with-kudu"></a>Yerel Git Kudu ile etkinleştirme

Kudu derleme sunucusu ile uygulamanıza yönelik yerel Git dağıtımını etkinleştirmek için çalıştıracağınız [ `az webapp deployment source config-local-git` ](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-local-git) Cloud shell'de.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group <group_name>
```

Bunun yerine Git özellikli bir uygulama oluşturmak için çalıştırın [ `az webapp create` ](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) ile Cloud shell'de `--deployment-local-git` parametresi.

```azurecli-interactive
az webapp create --name <app_name> --resource-group <group_name> --plan <plan_name> --deployment-local-git
```

`az webapp create` Komut size aşağıdaki çıktıya benzer bir şey:

```json
Local git is configured with url of 'https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

### <a name="deploy-your-project"></a>Projenizi dağıtın

_Yerel terminal penceresine_ dönüp yerel Git deponuza bir Azure uzak deposu ekleyin. Değiştirin  _\<url >_ aldığınız Git uzak URL'si ile [uygulamanızın Git etkinleştirme](#enable-local-git-with-kudu).

```bash
git remote add azure <url>
```

Aşağıdaki komutla uygulamanızı dağıtmak için Azure uzak deposuna gönderin. Parola sorulduğunda, oluşturduğunuz parolayı girin [dağıtım kullanıcısı yapılandırma](#configure-a-deployment-user). Azure portalında oturum açmak için kullandığınız parolayı kullanmayın.

```bash
git push azure master
```

Çalışma zamanı özel Otomasyon çıkışında, ASP.NET, MSBuild gibi görebileceğiniz `npm install` Node.js için ve `pip install` Python için.

### <a name="browse-to-the-azure-web-app"></a>Azure web uygulamasına göz atma

Web uygulamanıza içerik dağıtıldığını doğrulamak için bir tarayıcı kullanarak gidin.

```bash
http://<app_name>.azurewebsites.net
```

![App Service'te çalışan uygulama](../app-service/media/app-service-web-tutorial-dotnetcore-sqldb/azure-app-in-browser.png)

## <a name="use-managed-identity-in-other-languages"></a>Diğer dillerde yönetilen kimliği kullanma

Yönetilen kimlik için yerleşik destek de .NET Framework ve Java Spring için uygulama yapılandırma sağlayıcısı var. Bir sağlayıcı yapılandırdığınızda bu gibi durumlarda yerine tam bağlantı dizesini uygulama yapılandırma mağazanın URL uç noktasını kullanın. Örneğin, bu hızlı başlangıçta oluşturulan .NET Framework konsol uygulaması için aşağıdaki ayarları belirtin *App.config* dosyası:

```xml
    <configSections>
        <section name="configBuilders" type="System.Configuration.ConfigurationBuildersSection, System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" restartOnExternalChanges="false" requirePermission="false" />
    </configSections>

    <configBuilders>
        <builders>
            <add name="MyConfigStore" mode="Greedy" endpoint="${Endpoint}" type="Microsoft.Configuration.ConfigurationBuilders.AzureAppConfigurationBuilder, Microsoft.Configuration.ConfigurationBuilders.AzureAppConfiguration" />
            <add name="Environment" mode="Greedy" type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder, Microsoft.Configuration.ConfigurationBuilders.Environment" />
        </builders>
    </configBuilders>

    <appSettings configBuilders="Environment,MyConfigStore">
        <add key="AppName" value="Console App Demo" />
        <add key="Endpoint" value ="Set via an environment variable - for example, dev, test, staging, or production endpoint." />
    </appSettings>
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [CLI örnekleri](./cli-samples.md)
