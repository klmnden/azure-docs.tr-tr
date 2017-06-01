---
title: "Azure Web Uygulamasında Node.js Uygulaması oluşturma | Microsoft Docs"
description: "App Service Web Uygulaması ile birkaç dakika içinde ilk Node.js Merhaba Dünya uygulamanızı dağıtın."
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/05/2017
ms.author: cfowler
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: ced6f54603120d8832ee417b02b6673f80a99613
ms.contentlocale: tr-tr
ms.lasthandoff: 05/10/2017

---
# <a name="create-a-nodejs-application-on-web-app"></a>Web Uygulamasında Node.js Uygulaması oluşturma

Bu hızlı başlangıç öğreticisi, Azure’da bir Node.js uygulaması geliştirip dağıtma konusunda yol göstermektedir. Uygulamayı [Azure App Service planı](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) kullanılarak çalıştıracağız ve Azure CLI aracılığıyla uygulamanın içinde yeni bir Web Uygulaması oluşturulup yapılandıracağız. Ardından, Node.js uygulamasını Azure’a dağıtmak için git kullanılacaktır.

![hello-world-in-browser](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

Mac, Windows veya Linux makinesi kullanarak aşağıdaki adımları izleyebilirsiniz. Aşağıdaki tüm adımların tamamlanması yalnızca yaklaşık 5 dakika sürer.

## <a name="prerequisites"></a>Ön koşullar

Bu örneği oluşturmadan önce aşağıdakileri indirip yükleyin:

* [Git](https://git-scm.com/)
* [Node.js ve NPM](https://nodejs.org/)
* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Örneği indirme

Hello World örnek uygulama deposunu yerel makinenize kopyalayın.

```bash
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world
```

Örnek kodu içeren dizine geçin.

```bash
cd nodejs-docs-hello-world
```

## <a name="run-the-app-locally"></a>Uygulamayı yerel olarak çalıştırma

Yerleşik Node.js http sunucusunu başlatmak üzere bir terminal penceresi açıp örneğin `npm start` betiğini kullanarak uygulamayı yerel olarak çalıştırın.

```bash
npm start
```

Bir web tarayıcısı açın ve örneğe gidin.

```bash
http://localhost:1337
```

Sayfada gösterilen örnek uygulamada **Hello World** iletisini görebilirsiniz.

![localhost-hello-world-in-browser](media/app-service-web-get-started-nodejs-poc/localhost-hello-world-in-browser.png)

Terminal pencerenizde **Ctrl+C** tuşlarına basarak web sunucusundan çıkın.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Şimdi Node.js uygulamamızı Azure’da barındırmak için gereken kaynakları oluşturmak üzere bir terminal penceresinde Azure CLI 2.0 kullanacağız. [az login](/cli/azure/#login) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```azurecli
az login
```

<!-- ## Configure a Deployment User -->
[!INCLUDE [login-to-azure](../../includes/configure-deployment-user.md)]

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#create) ile bir kaynak grubu oluşturun. Azure kaynak grubu; web uygulamaları, veritabanları ve depolama hesapları gibi Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

```azurecli
az group create --name myResourceGroup --location westeurope
```

## <a name="create-an-azure-app-service-plan"></a>Azure App Service planı oluşturma

[az appservice plan create](/cli/azure/appservice/plan#create) komutuyla "ÜCRETSİZ" [App Service planı](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) oluşturun.

<!--
 An App Service plan represents the collection of physical resources used to ..
-->
[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

Aşağıdaki örnek, **Ücretsiz** fiyatlandırma katmanını kullanarak `quickStartPlan` adlı bir App Service planı oluşturur.

```azurecli
az appservice plan create --name quickStartPlan --resource-group myResourceGroup --sku FREE
```

App Service Planı oluşturulduğunda Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir:

```json
{
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/quickStartPlan",
    "location": "West Europe",
    "sku": {
    "capacity": 1,
    "family": "S",
    "name": "S1",
    "tier": "Standard"
    },
    "status": "Ready",
    "type": "Microsoft.Web/serverfarms"
}
```

## <a name="create-a-web-app"></a>Web uygulaması oluşturma

App Service planı oluşturulduktan sonra, `quickStartPlan` App Service planı içinde bir [Web Uygulaması](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) oluşturun. Web uygulaması, kodun dağıtılacağı bir barındırma alanı ve dağıtılmış uygulamayı görüntülememiz için bir URL sağlar. Web Uygulamasını oluşturmak için [az appservice web create](/cli/azure/appservice/web#create) komutunu kullanın.

Aşağıdaki komutta `<app_name>` yer tutucusunu kendi benzersiz uygulamanızın adıyla değiştirin. Web uygulamasının varsayılan DNS sitesinde `<app_name>` kullanılır. `<app_name>` benzersiz değilse "Belirtilen <app_name> adına sahip web sitesi zaten var." hata iletisi görüntülenir.

<!-- removed per https://github.com/Microsoft/azure-docs-pr/issues/11878
You can later map any custom DNS entry to the web app before you expose it to your users.
-->

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan quickStartPlan
```

Web Uygulaması oluşturulduğunda Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir.

```json
{
    "clientAffinityEnabled": true,
    "defaultHostName": "<app_name>.azurewebsites.net",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/sites/<app_name>",
    "isDefaultContainer": null,
    "kind": "app",
    "location": "West Europe",
    "name": "<app_name>",
    "repositorySiteName": "<app_name>",
    "reserved": true,
    "resourceGroup": "myResourceGroup",
    "serverFarmId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/quickStartPlan",
    "state": "Running",
    "type": "Microsoft.Web/sites",
}
```

Yeni oluşturduğunuz Web Uygulamasını görmek için siteye göz atın.

```bash
http://<app_name>.azurewebsites.net
```

![app-service-web-service-created](media/app-service-web-get-started-nodejs-poc/app-service-web-service-created.png)

Azure’da yeni bir boş Web Uygulaması oluşturduk.

## <a name="configure-local-git-deployment"></a>Yerel Git dağıtımını yapılandırma

Web Uygulamanıza FTP, yerel Git, GitHub, Visual Studio Team Services ve Bitbucket gibi çeşitli yollarla dağıtım yapabilirsiniz.

Web Uygulamasına yerel git erişimini yapılandırmak için [az appservice web source-control config-local-git](/cli/azure/appservice/web/source-control#config-local-git) komutunu kullanın.

```azurecli
az appservice web source-control config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

Sonraki adımda kullanılacak çıktıyı terminalden kopyalayın.

```bash
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
```

## <a name="push-to-azure-from-git"></a>Git üzerinden Azure'a gönderme

Yerel Git deponuza bir Azure uzak deposu ekleyin.

```bash
git remote add azure <paste-previous-command-output-here>
```

Uygulamanızı dağıtmak için Azure uzak deposuna gönderin. Daha önce dağıtım kullanıcısını oluştururken girdiğiniz parola istenir. Azure portalında oturum açarken kullandığınız parolayı değil [Dağıtım kullanıcısı yapılandırma](#configure-a-deployment-user) adımında oluşturduğunuz parolayı girdiğinizden emin olun.

```bash
git push azure master
```

Dağıtım sırasında Azure App Service, ilerleme durumunu Git'e iletir.

```bash
Counting objects: 23, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (21/21), done.
Writing objects: 100% (23/23), 3.71 KiB | 0 bytes/s, done.
Total 23 (delta 8), reused 7 (delta 1)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'bf114df591'.
remote: Generating deployment script.
remote: Generating deployment script for node.js Web Site
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling node.js deployment.
remote: Kudu sync from: '/home/site/repository' to: '/home/site/wwwroot'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'README.md'
remote: Copying file: 'index.js'
remote: Copying file: 'package.json'
remote: Copying file: 'process.json'
remote: Deleting file: 'hostingstart.html'
remote: Ignoring: .git
remote: Using start-up script index.js from package.json.
remote: Node.js versions available on the platform are: 4.4.7, 4.5.0, 6.2.2, 6.6.0, 6.9.1.
remote: Selected node.js version 6.9.1. Use package.json file to choose a different version.
remote: Selected npm version 3.10.8
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net:443/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-to-the-app"></a>Uygulamaya göz atma

Web tarayıcınızı kullanarak dağıtılan uygulamaya göz atın.

```bash
http://<app_name>.azurewebsites.net
```

Bu kez, Merhaba Dünya iletisini gösteren sayfa bir Azure App Service web uygulaması olarak çalışan Node.js kodumuz kullanılarak çalıştırılır.

## <a name="updating-and-deploying-the-code"></a>Kodu Güncelleştirme ve Dağıtma

Bir yerel metin düzenleyicisi kullanarak `index.js` dosyasını Node.js uygulaması içinde açın ve `response.end` çağrısındaki metinde küçük bir değişiklik yapın:

```nodejs
response.end("Hello Azure!");
```

Değişikliklerinizi git’e işleyin, ardından kod değişikliklerini Azure’a gönderin.

```bash
git commit -am "updated output"
git push azure master
```

Dağıtım tamamlandıktan sonra **Uygulamaya göz at** adımında açılan tarayıcı penceresine dönüp yenile öğesine dokunun.

![hello-world-in-browser](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a>Yeni Azure web uygulamanızı yönetme

Azure portalına giderek yeni oluşturduğunuz web uygulamasına göz atın.

Bunu yapmak için [https://portal.azure.com](https://portal.azure.com) sayfasında oturum açın.

Sol menüden **Uygulama Hizmetleri**’ne ve ardından Azure web uygulamanızın adına tıklayın.

![Portaldan Azure web uygulamasına gitme](./media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-list.png)

Web uygulamanızın _dikey penceresini_ açtınız (yatay yönde açılan portal sayfası).

Varsayılan olarak, web uygulamanızın dikey penceresinde **Genel Bakış** sayfası gösterilir. Bu sayfa, uygulamanızın nasıl çalıştığını gösterir. Buradan ayrıca göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz. Dikey pencerenin sol tarafındaki sekmeler, açabileceğiniz farklı yapılandırma sayfalarını gösterir.

![Azure portalında App Service dikey penceresi](media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-detail.png)

Dikey penceredeki bu sekmelerde web uygulamanıza ekleyebileceğiniz çok sayıda harika özellik gösterilir. Aşağıdaki listede yalnızca birkaç olasılık sunulmaktadır:

* Özel bir DNS adı eşleme
* Özel bir SSL sertifikası bağlama
* Sürekli dağıtımı yapılandırma
* Ölçeği artırma ve genişletme
* Kullanıcı kimlik doğrulaması ekleme

**Tebrikler!** App Service’e ilk Node.js uygulamanızı dağıttınız.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

> [!div class="nextstepaction"]
> [Örnek Web Apps CLI betiklerini keşfedin](app-service-cli-samples.md)

