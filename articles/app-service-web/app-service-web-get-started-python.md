---
title: "Beş dakika içinde Azure’da ilk Python web uygulamanızı oluşturma | Microsoft Docs"
description: "App Service Web Uygulaması ile birkaç dakika içinde ilk Python Merhaba Dünya uygulamanızı dağıtın."
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
ms.assetid: 928ee2e5-6143-4c0c-8546-366f5a3d80ce
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/17/2017
ms.author: cfowler
translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: 9bd8db6c765f8f702a6e4ea5b17507269d3310d1
ms.lasthandoff: 04/25/2017


---
# <a name="create-a-python-application-on-web-app"></a>Web Uygulaması üzerinde Python uygulaması oluşturma

Bu hızlı başlangıç öğreticisi, Azure’da bir Python uygulaması geliştirip dağıtma konusunda yol göstermektedir. Uygulama, bir Azure App Service kullanılarak çalıştırılacak ve Azure CLI kullanılarak uygulamanın içinde yeni bir Web Uygulaması yapılandırılacaktır. Ardından, Python uygulamasını Azure’a dağıtmak için git kullanılacaktır.

![hello-world-in-browser](media/app-service-web-get-started-python/hello-world-in-browser.png)

Bir Mac, Windows veya Linux makine kullanarak aşağıdaki adımları izleyebilirsiniz. Aşağıdaki tüm adımların tamamlanması yalnızca yaklaşık 5 dakika sürer.

## <a name="before-you-begin"></a>Başlamadan önce

Bu örneği çalıştırmadan önce aşağıdaki önkoşulları yerel olarak yükleyin:

1. [Git indirme ve yükleme](https://git-scm.com/)
1. [Python indirme ve yükleme](https://www.python.org/downloads/)
1. [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) indirme ve yükleme

## <a name="download-the-sample"></a>Örneği indirme

Merhaba Dünya örnek uygulama deposunu yerel makinenize kopyalayın.

```bash
git clone https://github.com/Azure-Samples/python-docs-hello-world
```

> [!TIP]
> Alternatif olarak, [örneği zip dosyası olarak indirip](https://github.com/Azure-Samples/Python-docs-hello-world/archive/master.zip) çıkartabilirsiniz.

Örnek kodu içeren dizine geçin.

```bash
cd Python-docs-hello-world
```

## <a name="run-the-app-locally"></a>Uygulamayı yerel olarak çalıştırma

Yerleşik Python web sunucusunu başlatmak üzere bir terminal penceresi açıp `Python` komutunu kullanarak uygulamayı çalıştırın.

```bash
python main.py
```

Bir web tarayıcısı açın ve örneğe gidin.

```bash
http://localhost:5000
```

Sayfada gösterilen örnek uygulamada **Merhaba Dünya** iletisini görebilirsiniz.

![localhost-hello-world-in-browser](media/app-service-web-get-started-python/localhost-hello-world-in-browser.png)

Terminal pencerenizde **Ctrl+C** tuşlarına basarak web sunucusundan çıkın.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Şimdi Python uygulamamızı Azure’da barındırmak için gereken kaynakları oluşturmak üzere bir terminal penceresinde Azure CLI 2.0 kullanacağız. [az login](/cli/azure/#login) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```azurecli
az login
```

## <a name="configure-a-deployment-user"></a>Dağıtım Kullanıcısı Yapılandırma

FTP ve yerel Git için sunucu üzerinde dağıtım kimliğini doğrulamak amacıyla bir dağıtım kullanıcısı yapılandırılmış olmalıdır. Dağıtım kullanıcısı oluşturmak tek seferlik bir yapılandırmadır; aşağıdaki bir adımda kullanacağınız kullanıcı adı ile parolayı not alın.

> [!NOTE]
> Bir Web Uygulamasına FTP ve Yerel Git dağıtımı için dağıtım kullanıcısı gereklidir.
> `username` ve `password` hesap düzeyinde olduğundan Azure Aboneliği kimlik bilgilerinizden farklıdır. **Bu kimlik bilgilerinin yalnızca bir kez oluşturulması gerekir**.
>

Hesap düzeyinde kimlik bilgilerinizi oluşturmak için [az appservice web deployment user set](/cli/azure/appservice/web/deployment/user#set) komutunu kullanın.

```azurecli
az appservice web deployment user set --user-name <username> --password <password>
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#create) ile bir kaynak grubu oluşturun. Azure kaynak grubu; web uygulamaları, veritabanları ve depolama hesapları gibi Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

```azurecli
az group create --name myResourceGroup --location westeurope
```

## <a name="create-an-azure-app-service"></a>Azure App Service oluşturma

[az appservice plan create](/cli/azure/appservice/plan#create) komutu ile Linux tabanlı bir App Service Planı oluşturun.

> [!NOTE]
> App Service planı, uygulamalarınızı barındırmak için kullanılan fiziksel kaynak bileşimini temsil eder. Bir App Service planına atanan tüm uygulamalar plan tarafından tanımlanan kaynakları paylaşarak birden çok uygulamayı barındırırken maliyetten tasarruf etmenize imkan sağlar.
>
> App Service planları şunları tanımlar:
> * Bölge (Kuzey Avrupa, Doğu ABD, Güneydoğu Asya)
> * Örnek Boyutu (Küçük, Orta, Büyük)
> * Ölçek Sayısı (bir, iki, üç örnek, vb.)
> * SKU (Ücretsiz, Paylaşılan, Temel, Standart, Premium)
>

Aşağıdaki örnek, **ÜCRETSİZ** fiyatlandırma katmanını kullanarak Linux Çalışanları üzerinde `quickStartPlan` adlı bir App Service Planı oluşturur.

```azurecli
az appservice plan create --name quickStartPlan --resource-group myResourceGroup --sku FREE
```

App Service Planı oluşturulduğunda Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir.

```json
{
"appServicePlanName": "quickStartPlan",
"geoRegion": "North Europe",
"id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/quickStartPlan",
"kind": "app",
"location": "North Europe",
"maximumNumberOfWorkers": 1,
"name": "quickStartPlan",
"provisioningState": "Succeeded",
"resourceGroup": "myResourceGroup",
"sku": {
  "capacity": 0,
  "family": "F",
  "name": "F1",
  "size": "F1",
  "tier": "Free"
},
"status": "Ready",
"type": "Microsoft.Web/serverfarms",
}
```

## <a name="create-a-web-app"></a>Web uygulaması oluşturma

Bir App Service planı oluşturduktan sonra `quickStartPlan` App Service planı içinde bir Web Uygulaması oluşturun. Web uygulaması, kodun dağıtılacağı bir barındırma alanı ve dağıtılmış uygulamayı görüntülememiz için bir URL sağlar. Web Uygulamasını oluşturmak için [az appservice web create](/cli/azure/appservice/web#create) komutunu kullanın.

Aşağıdaki komutta `<app_name>` yer tutucusunu kendi benzersiz uygulamanızın adıyla değiştirin. `<app_name>`, web uygulamasının varsayılan DNS sitesi olarak kullanılacağı için adın Azure’daki tüm uygulamalarda benzersiz olması gerekir. Daha sonra, uygulamanızı kullanıcılarınızın kullanımına sunmadan önce web uygulamasına herhangi bir özel DNS girişi eşleyebilirsiniz.

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan quickStartPlan
```

Web Uygulaması oluşturulduğunda Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir.

```json
{
  "clientAffinityEnabled": true,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "<app_name>.azurewebsites.net",
    "<app_name>.scm.azurewebsites.net"
  ],
  "hostNames": [
    "<app_name>.azurewebsites.net"
  ],
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/sites/<app_name>",
  "kind": "app",
  "location": "North Europe",
  "outboundIpAddresses": "13.69.190.80,13.69.191.239,13.69.186.193,13.69.187.34",
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

![app-service-web-service-created](media/app-service-web-get-started-python/app-service-web-service-created.png)

Azure’da yeni bir boş Web Uygulaması oluşturduk. Şimdi de Web Uygulamamızı Python kullanacak şekilde yapılandırıp uygulamamızı dağıtalım.

## <a name="configure-to-use-python"></a>Python kullanacak şekilde yapılandırma

Web Uygulamasını `3.4` Python sürümünü kullanacak şekilde yapılandırmak için [az appservice web config update](/cli/azure/app-service/web/config#update) komutunu kullanın.

> [!TIP]
> Python sürümü bu şekilde ayarlandığında platform tarafından sağlanan varsayılan kapsayıcı kullanılır; kendi kapsayıcınızı kullanmak isterseniz [az appservice web config container update](https://docs.microsoft.com/cli/azure/appservice/web/config/container#update) komutunun CLI başvurusuna bakın.

```azurecli
az appservice web config update --python-version 3.4 --name <app-name> --resource-group myResourceGroup
```

## <a name="configure-local-git-deployment"></a>Yerel git dağıtımını yapılandırma

Web Uygulamanıza FTP, yerel Git ve GitHub, Visual Studio Team Services ve Bitbucket gibi çeşitli yollarla dağıtım yapabilirsiniz.

Web Uygulamasına yerel git erişimini yapılandırmak için [az appservice web source-control config-local-git](/cli/azure/appservice/web/source-control#config-local-git) komutunu kullanın.

```azurecli
az appservice web source-control config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

Sonraki adımda kullanılacak çıktıyı terminalden kopyalayın.

```bash
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
```

## <a name="push-to-azure-from-git"></a>Git üzerinden Azure'a gönderme

Git deponuza bir Azure uzak öğesi ekleyin.

```bash
git remote add azure <paste-previous-command-output-here>
```

Azure uzak öğesini uygulamanıza dağıtmak için gönderin. Daha önce dağıtım kullanıcısı oluştururken belirttiğiniz parola istenir.

```azurecli
git push azure master
```

Dağıtım sırasında Azure App Service, ilerleme durumunu Git’e iletir.

```bash
Counting objects: 18, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (16/16), done.
Writing objects: 100% (18/18), 4.31 KiB | 0 bytes/s, done.
Total 18 (delta 4), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '44e74fe7dd'.
remote: Generating deployment script.
remote: Generating deployment script for python Web Site
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling python deployment.
remote: KuduSync.NET from: 'D:\home\site\repository' to: 'D:\home\site\wwwroot'
remote: Deleting file: 'hostingstart.html'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'main.py'
remote: Copying file: 'README.md'
remote: Copying file: 'requirements.txt'
remote: Copying file: 'virtualenv_proxy.py'
remote: Copying file: 'web.2.7.config'
remote: Copying file: 'web.3.4.config'
remote: Detected requirements.txt.  You can skip Python specific steps with a .skipPythonDeployment file.
remote: Detecting Python runtime from site configuration
remote: Detected python-3.4
remote: Creating python-3.4 virtual environment.
remote: .................................
remote: Pip install requirements.
remote: Successfully installed Flask click itsdangerous Jinja2 Werkzeug MarkupSafe
remote: Cleaning up...
remote: .
remote: Overwriting web.config with web.3.4.config
remote:         1 file(s) copied.
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-to-the-app"></a>Uygulamaya göz atma

Web tarayıcınızı kullanarak dağıtılan uygulamaya göz atın.

```bash
http://<app_name>.azurewebsites.net
```

Bu kez, Merhaba Dünya iletisini gösteren sayfa bir Azure Uygulama Hizmeti web uygulaması olarak çalışan Python kodumuz kullanılarak çalıştırılır.

![]()

## <a name="updating-and-deploying-the-code"></a>Kodu Güncelleştirme ve Dağıtma

Bir yerel metin düzenleyicisi kullanarak `main.py` dosyasını Python uygulaması içinde açın ve `return` deyiminin yanındaki dizenin içinde bulunan metinde küçük bir değişiklik yapın:

```python
return 'Hello, Azure!'
```

Değişikliklerinizi git’e işleyin, ardından kod değişikliklerini Azure’a gönderin.

```bash
git commit -am "updated output"
git push azure master
```

Dağıtım tamamlandıktan sonra uygulama adımındaki Göz At menüsünde açılan tarayıcı penceresine dönüp yenile öğesine dokunun.

![hello-world-in-browser](media/app-service-web-get-started-python/hello-world-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a>Yeni Azure web uygulamanızı yönetme

Azure portalına giderek yeni oluşturduğunuz web uygulamasına göz atın.

Bunu yapmak için [https://portal.azure.com](https://portal.azure.com) sayfasında oturum açın.

Sol menüden **Uygulama Hizmetleri**’ne ve ardından Azure web uygulamanızın adına tıklayın.

![Portaldan Azure web uygulamasına gitme](./media/app-service-web-get-started-python/Python-docs-hello-world-app-service-list.png)

Web uygulamanızın _dikey penceresini_ açtınız (yatay yönde açılan portal sayfası).

Varsayılan olarak, web uygulamanızın dikey penceresinde **Genel Bakış** sayfası gösterilir. Bu sayfa, uygulamanızın nasıl çalıştığını gösterir. Buradan ayrıca göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz. Dikey pencerenin sol tarafındaki sekmeler, açabileceğiniz farklı yapılandırma sayfalarını gösterir.

![Azure portalında App Service dikey penceresi](media/app-service-web-get-started-python/Python-docs-hello-world-app-service-detail.png)

Dikey penceredeki bu sekmelerde web uygulamanıza ekleyebileceğiniz çok sayıda harika özellik gösterilir. Aşağıdaki listede yalnızca birkaç olasılık sunulmaktadır:

* Özel bir DNS adı eşleme
* Özel bir SSL sertifikası bağlama
* Sürekli dağıtımı yapılandırma
* Ölçeği artırma ve genişletme
* Kullanıcı kimlik doğrulaması ekleme

**Tebrikler!** App Service’e ilk Python uygulamanızı dağıttınız.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

Önceden oluşturulmuş [Web Apps CLI betiklerini](app-service-cli-samples.md) keşfedin.
