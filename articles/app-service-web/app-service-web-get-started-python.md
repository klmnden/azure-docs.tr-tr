---
title: "Azure'da Python web uygulaması oluşturma | Microsoft Docs"
description: "Azure App Service Web Uygulamalarında ilk Python Hello World uygulamanızı birkaç dakika içinde dağıtın."
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
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: 233db1cb74a6c81cf044953ecdf6e9de6cc50ee8
ms.contentlocale: tr-tr
ms.lasthandoff: 06/28/2017

---
<a id="create-a-python-web-app-in-azure" class="xliff"></a>

# Azure’da Python web uygulaması oluşturma

[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar.  Bu hızlı başlangıç öğreticisi, Azure Web Apps'te bir Python uygulaması geliştirip dağıtma konusunda yol göstermektedir. Web uygulamasını [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)'yi kullanarak oluşturabilir ve web uygulamasında örnek Python kodu dağıtmak için Git'i kullanabilirsiniz.

![Azure'da çalışan örnek uygulama](media/app-service-web-get-started-python/hello-world-in-browser.png)

Mac, Windows veya Linux makinesi kullanarak aşağıdaki adımları izleyebilirsiniz. Önkoşullar yüklendikten sonra adımların tamamlanması yaklaşık olarak beş dakika sürer.
<a id="prerequisites" class="xliff"></a>

## Önkoşullar

Bu öğreticiyi tamamlamak için:

1. [Git'i yükleyin](https://git-scm.com/)
1. [Python'ı yükleyin](https://www.python.org/downloads/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

<a id="download-the-sample" class="xliff"></a>

## Örneği indirme

Bir terminal penceresinde, örnek uygulama deposunu yerel makinenize kopyalamak için aşağıdaki komutu çalıştırın.

```bash
git clone https://github.com/Azure-Samples/python-docs-hello-world
```

Bu hızlı başlangıç öğreticisindeki tüm komutları çalıştırmak için bu terminal penceresini kullanırsınız.

Örnek kodu içeren dizine geçin.

```bash
cd Python-docs-hello-world
```

<a id="run-the-app-locally" class="xliff"></a>

## Uygulamayı yerel olarak çalıştırma

Yerleşik Python web sunucusunu başlatmak için bir terminal penceresi açıp ve `Python` komutunu kullanıp uygulamayı yerel olarak çalıştırın.

```bash
python main.py
```

Bir web tarayıcısı açın ve http://localhost:5000 konumundaki örnek uygulamaya gidin.

Sayfada gösterilen örnek uygulamada **Hello World** iletisini görebilirsiniz.

![Yerel olarak çalışan örnek uygulama](media/app-service-web-get-started-python/localhost-hello-world-in-browser.png)

Terminal pencerenizde **Ctrl+C** tuşlarına basarak web sunucusundan çıkın.

[!INCLUDE [Log in to Azure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Boş web uygulaması sayfası](media/app-service-web-get-started-python/app-service-web-service-created.png)

Azure'da yeni bir boş uygulama oluşturdunuz.

<a id="configure-to-use-python" class="xliff"></a>

## Python kullanacak şekilde yapılandırma

Web uygulamasını Python `3.4` sürümünü kullanacak şekilde yapılandırmak için [az webapp config set](/cli/azure/webapp/config#set) komutunu kullanın.

```azurecli-interactive
az webapp config set --python-version 3.4 --name <app_name> --resource-group myResourceGroup
```


Python sürümü bu şekilde ayarlandığında, platform tarafından sağlanan varsayılan bir kapsayıcı kullanılır. Kendi kapsayıcınızı kullanmak üzere, [az webapp config container set](/cli/azure/webapp/config/container#set) komutuna ilişkin CLI başvurusuna bakın.

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push to Azure](../../includes/app-service-web-git-push-to-azure.md)] 

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

<a id="browse-to-the-app" class="xliff"></a>

## Uygulamaya göz atma

Web tarayıcınızı kullanarak dağıtılan uygulamaya göz atın.

```bash
http://<app_name>.azurewebsites.net
```

Python örnek kodu bir Azure App Service web uygulamasında çalışıyor.

![Azure'da çalışan örnek uygulama](media/app-service-web-get-started-python/hello-world-in-browser.png)

**Tebrikler!** App Service’e ilk Python uygulamanızı dağıttınız.

<a id="update-and-redeploy-the-code" class="xliff"></a>

## Kodu güncelleştirme ve yeniden dağıtma

Yerel bir metin düzenleyici kullanarak `main.py` dosyasını Python uygulamasında açın ve `return` deyiminin yanındaki metinde küçük bir değişiklik yapın:

```python
return 'Hello, Azure!'
```

Değişikliklerinizi Git’e işleyin ve ardından kod değişikliklerini Azure’a gönderin.

```bash
git commit -am "updated output"
git push azure master
```

Dağıtım tamamlandıktan sonra [Uygulamaya göz atma](#browse-to-the-app) adımında açılan tarayıcı penceresine dönüp sayfayı yenileyin.

![Azure'da çalışan güncelleştirilmiş örnek uygulama](media/app-service-web-get-started-python/hello-azure-in-browser.png)

<a id="manage-your-new-azure-web-app" class="xliff"></a>

## Yeni Azure web uygulamanızı yönetme

Oluşturduğunuz web uygulamasını yönetmek için <a href="https://portal.azure.com" target="_blank">Azure portalına</a> gidin.

Sol menüden **Uygulama Hizmetleri**'ne ve ardından Azure web uygulamanızın adına tıklayın.

![Portaldan Azure web uygulamasına gitme](./media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-list.png)

Web uygulamanızın Genel Bakış sayfasını görürsünüz. Buradan göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz. 

![Azure portalında App Service dikey penceresi](media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-detail.png)

Soldaki menü, uygulamanızın yaplandırılmasına yönelik farklı sayfalar sağlar. 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a id="next-steps" class="xliff"></a>

## Sonraki adımlar

> [!div class="nextstepaction"]
> [PostgreSQL ile Python](app-service-web-tutorial-docker-python-postgresql-app.md)

