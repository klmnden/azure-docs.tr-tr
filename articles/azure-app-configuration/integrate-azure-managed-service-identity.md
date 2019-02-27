---
title: Azure yönetilen kimlikleri ile tümleştirmek için öğretici | Microsoft Docs
description: Bu öğreticide, Azure yönetilen kimlikleri ile kimlik doğrulaması ve Azure uygulama yapılandırması erişim kazanmak için nasıl bilgi edinin
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: na
ms.topic: tutorial
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: bf8a1955f2c8e4a72e7cf88b013f8d5d5c2e672f
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56884769"
---
# <a name="tutorial-integrate-with-azure-managed-identities"></a>Öğretici: Azure yönetilen kimlikleriniz ile tümleştirebilirsiniz

Azure Active Directory [yönetilen kimlikleri](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) yardımcı olur, bulut uygulamanız için gizli dizileri Yönetimi basitleştirin. Yönetilen bir kimlik ile oluşturulmuş hizmet sorumlusunu Azure anahtar kasası veya bir yerel bağlantı dizesi içinde depolanan ayrı bir kimlik bilgisi yerine üzerinde çalıştığı Azure işlem hizmetlerini kullanmak için kodunuzu oluşturan ayarlayabilirsiniz. Azure uygulama yapılandırması ve .NET Core, .NET ve Java Spring istemci kitaplıkları, yerleşik MSI desteğiyle gelir. Bunu kullanmak için gerekli olmasa da, MSI gizli dizileri içeren bir erişim belirteci ihtiyacını ortadan kaldırır. Kodunuzu yalnızca erişebilmesi için bir uygulama yapılandırma deposu için hizmet uç noktasını bilmesi gerekir ve bu URL, kodunuzdaki herhangi bir gizli dizi gösterme, doğrudan kaygısı olmadan ekleyebilir.

Bu öğreticide, uygulama yapılandırması ile erişmek için MSI avantajlarından nasıl gerçekleştirebileceğiniz gösterilir. Bu hızlı başlangıçlar, sunulan web uygulaması oluşturur. Tam [uygulama yapılandırması ile bir ASP.NET Core uygulaması oluşturma](./quickstart-aspnet-core-app.md) devam etmeden önce ilk.

Bu öğreticideki adımları tamamlamak için herhangi bir kod Düzenleyicisi'ni kullanabilirsiniz. Ancak, Windows, macOS ve Linux platformlarında sağlanan [Visual Studio Code](https://code.visualstudio.com/) mükemmel bir seçenektir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Uygulama yapılandırması için bir yönetilen kimlik erişimi verme
> * Yönetilen bir kimlik uygulama yapılandırması için bağlanırken kullanmak için uygulamanızı yapılandırın

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakilere sahip olmanız gerekir:

* [.NET Core SDK](https://www.microsoft.com/net/download/windows)
* [Yapılandırılmış Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="add-a-managed-identity"></a>Yönetilen bir kimlik Ekle

Portalda yönetilen bir kimlik ayarlamak için ilk olarak normal bir uygulama oluşturun ve ardından özelliği etkinleştirmek.

1. Bir uygulama oluşturun [Azure portalında](https://aka.ms/azconfig/portal) normalde yaptığınız gibi. İçin portalda gidin.

2. Ekranı aşağı kaydırarak **ayarları** seçin ve grup sol gezinti bölmesinde **kimlik**.

3. İçinde **sistem tarafından atanan** sekmesinde, geçiş **durumu** için **üzerinde** tıklatıp **Kaydet**.

    ![App Service'te yönetilen kimlik ayarlayın](./media/set-managed-identity-app-service.png)

## <a name="grant-access-to-app-configuration"></a>Uygulama yapılandırması için erişim izni ver

1. İçinde [Azure portalında](https://aka.ms/azconfig/portal), tıklayın **tüm kaynakları** ve hızlı başlangıçta oluşturduğunuz uygulama yapılandırma deposu.

2. Seçin **erişim denetimi (IAM)**.

3. İçinde **denetleyin erişim** sekmesini tıklatın, **Ekle** içinde **bir rol ataması Ekle** UI kartı.

4. Ayarlama **rol** olmasını *katkıda bulunan* ve **erişim Ata** olmasını *App Service* (altında *sistem tarafından atanan yönetilen kimlik*).

5. Ayarlama **abonelik** Azure aboneliği ve uygulamanız için App Service kaynak seçin.

6. **Kaydet**’e tıklayın.

    ![Yönetilen kimlik Ekle](./media/add-managed-identity.png)

## <a name="use-a-managed-identity"></a>Yönetilen kimlik kullanma

1. Açık *appsettings.json*, aşağıdaki ekleme ve değiştirme *< service_endpoint >* (köşeli ayraçlar dahil), uygulama yapılandırma deposunun URL'si ile:

    ```json
    "AppConfig": {
        "Url": "<service_endpoint>"
    }
    ```

2. Açık *Program.cs* ve güncelleştirme `CreateWebHostBuilder` değiştirerek yöntemi `config.AddAzureAppConfiguration()` yöntemi.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(o => o.ConnectWithManagedIdentity(settings["AppConfig:Url"]));
            })
            .UseStartup<Startup>();
    ```

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

## <a name="deploy-from-local-git"></a>Yerel Git’ten dağıtım

Kudu derleme sunucusu ile uygulamanıza yönelik yerel Git dağıtımını etkinleştirmek için en kolay yolu, Cloud Shell'i kullanmaktır.

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

Aşağıdaki komutla uygulamanızı dağıtmak için Azure uzak deposuna gönderin. Parola istendiğinde Azure portalında oturum açarken kullandığınız parolayı değil [Dağıtım kullanıcısı yapılandırma](#configure-a-deployment-user) adımında oluşturduğunuz parolayı girdiğinizden emin olun.

```bash
git push azure master
```

Çalışma zamanı özel Otomasyon çıkışında, ASP.NET, MSBuild gibi görebilirsiniz `npm install` Node.js için ve `pip install` Python için.

### <a name="browse-to-the-azure-web-app"></a>Azure web uygulamasına göz atma

Web uygulamanıza içerik dağıtıldığını doğrulamak için bir tarayıcı kullanarak gidin.

```bash
http://<app_name>.azurewebsites.net
```

![App Service’te çalışan uygulama](../app-service/media/app-service-web-tutorial-dotnetcore-sqldb/azure-app-in-browser.png)

<!-- ### Use a managed identity in a .NET Core app -->

<!-- ### Use a managed identity in a .NET Framework app -->

<!-- ### Use a managed identity in a Java Spring app -->

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, eklediğiniz Azure yönetilen hizmet kimliği erişim uygulama yapılandırmasını kolaylaştırmaya ve uygulamanız için kimlik bilgileri yönetimi iyileştirmek için. Uygulama yapılandırmasını kullanma hakkında daha fazla bilgi edinmek için Azure CLI örnekleri için devam edin.

> [!div class="nextstepaction"]
> [CLI örnekleri](./cli-samples.md)
