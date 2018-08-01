---
title: Azure Kapsayıcılar için Web App’te bir Python uygulaması dağıtma
description: Kapsayıcılar için Web App’e yönelik Python uygulaması çalıştıran bir Docker görüntüsü dağıtma.
keywords: azure app service, web uygulaması, python, docker, kapsayıcı
services: app-service
author: cephalin
manager: jeconnoc
ms.service: app-service
ms.devlang: python
ms.topic: quickstart
ms.date: 07/13/2018
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 5686266774603413fc255c53a0d1ad30f9baa8eb
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39173869"
---
# <a name="deploy-a-python-web-app-in-web-app-for-containers"></a>Kapsayıcılar için Web App’te bir Python web uygulaması dağıtma

[Linux’ta App Service](app-service-linux-intro.md) Linux işletim sistemini kullanan yüksek oranda ölçeklenebilir, otomatik olarak düzeltme eki uygulayan bir web barındırma hizmeti sağlar. Bu hızlı başlangıçta bir web uygulaması oluşturma ve özel bir Docker Hub görüntüsü kullanarak basit bir Flask uygulaması dağıtma adımları gösterilmektedir. Web uygulamasını [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) kullanarak oluşturursunuz.

![Azure'da çalışan örnek uygulama](media/quickstart-python/hello-world-in-browser.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

* <a href="https://git-scm.com/" target="_blank">Git'i yükleyin</a>
* <a href="https://www.docker.com/community-edition" target="_blank">Docker Community Edition'ı yükleyin</a>
* <a href="https://hub.docker.com/" target="_blank">Docker Hub hesabı için kaydolma</a>

## <a name="download-the-sample"></a>Örneği indirme

Bir terminal penceresinde, örnek uygulamayı yerel makinenize kopyalamak ve örnek kodu içeren dizine gitmek için aşağıdaki komutları çalıştırın.

```bash
git clone https://github.com/Azure-Samples/python-docs-hello-world
cd python-docs-hello-world
```

Bu deponun _/app_ klasöründe basit bir Flask uygulaması ve üç öğe belirten bir _Dockerfile_ bulunur:

- [tiangolo/uwsgi-nginx-flask:python3.6-alpine3.7](https://hub.docker.com/r/tiangolo/uwsgi-nginx-flask/) temel görüntüsünü kullanın.
- Kapsayıcının 8000 numaralı bağlantı noktasını dinlemesi gerekir.
- `/app` dizinini kapsayıcının `/app` dizinine kopyalayın.

Yapılandırmada [temel görüntü talimatları](https://hub.docker.com/r/tiangolo/uwsgi-nginx-flask/) kullanılmaktadır.

## <a name="run-the-app-locally"></a>Uygulamayı yerel olarak çalıştırma

Uygulamayı Docker kapsayıcısında çalıştırın.

```bash
docker build --rm -t flask-quickstart .
docker run --rm -it -p 8000:8000 flask-quickstart
```

Bir web tarayıcısı açın ve `http://localhost:8000` konumundaki örnek uygulamaya gidin.

Sayfada gösterilen örnek uygulamada **Hello World** iletisini görebilirsiniz.

![Yerel olarak çalışan örnek uygulama](media/quickstart-python/localhost-hello-world-in-browser.png)

Terminal penceresinde **Ctrl+C** tuşlarına basarak kapsayıcıyı durdurun.

## <a name="deploy-image-to-docker-hub"></a>Görüntüyü Docker Hub'a dağıtma

Docker Hub hesabınızda oturum açın. Yönlendirmeleri izleyerek Docker Hub kimlik bilgilerinizi girin.

```bash
docker login
```

Görüntünüzü etiketleyin ve Docker Hub hesabınızda `flask-quickstart` adlı yeni bir _genel_ depoya gönderin. *\<dockerhub_id>* yerine Docker Hub kimliğinizi yazın.

```bash
docker tag flask-quickstart <dockerhub_id>/flask-quickstart
docker push <dockerhub_id>/flask-quickstart
```

> [!NOTE]
> `docker push`, belirtilen depo bulunamadığında bir genel depo oluşturulmasını sağlar. Bu hızlı başlangıçta Docker Hub'da bir genel depo olduğu kabul edilmektedir. Özel depoya göndermek isterseniz daha sonra Docker Hub kimlik bilgilerinizi Azure App Service'te yapılandırmanız gerekir. Bkz. [Web uygulaması oluşturma](#create-a-web-app).

Görüntü gönderme işlemi tamamlandıktan sonra Azure web uygulamanızda kullanabilirsiniz.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app"></a>Web uygulaması oluşturma

[az webapp create](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) komutuyla `myAppServicePlan` App Service planında bir [web uygulaması](../app-service-web-overview.md) oluşturun. *\<app name>* yerine global olarak benzersiz bir uygulama adı, *\<dockerhub_id>* yerine de Docker Hub kimliğinizi yazın.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --deployment-container-image-name <dockerhub_id>/flask-quickstart
```

Web uygulaması oluşturulduğunda Azure CLI aşağıda yer alan çıktıdaki gibi bilgiler gösterir:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app name>.scm.azurewebsites.net/<app name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

Önceden yüklediğiniz bir özel depo varsa Docker Hub kimlik bilgilerini App Service'te yapılandırmanız gerekir. Daha fazla bilgi için bkz. [Docker Hub'daki bir özel görüntüyü kullanma](tutorial-custom-docker-image.md#use-a-private-image-from-docker-hub-optional).

### <a name="specify-container-port"></a>Kapsayıcı bağlantı noktasını belirtme

_Dockerfile_ içinde belirtildiği üzere kapsayıcınız 8000 numaralı bağlantı noktasını dinler. App Service'in isteğinizi doğru bağlantı noktasından yönlendirmesi için *WEBSITES_PORT* uygulama ayarını kullanmanız gerekir.

Cloud Shell'de, [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) komutunu çalıştırın.


```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings WEBSITES_PORT=8000
```

## <a name="browse-to-the-app"></a>Uygulamaya göz atma

```bash
http://<app_name>.azurewebsites.net/
```

![Azure'da çalışan örnek uygulama](media/quickstart-python/hello-world-in-browser.png)

> [!NOTE]
> Web uygulaması ilk kez çağrıldığında Docker Hub görüntüsünün indirilmesi ve çalışması gerektiğinden başlaması biraz uzun sürebilir. Uzun bir süre bekledikten sonra hata görürseniz sayfayı yenilemeniz yeterlidir.

**Tebrikler!** Kapsayıcılar için Web App’e yönelik Python uygulaması çalıştıran özel bir Docker görüntüsü dağıttınız.

## <a name="update-locally-and-redeploy"></a>Yerel olarak güncelleştirme ve yeniden dağıtma

Yerel bir metin düzenleyici kullanarak `app/main.py` dosyasını Python uygulamasında açın ve `return` deyiminin yanındaki metinde küçük bir değişiklik yapın:

```python
return 'Hello, Azure!'
```

Görüntüyü yeniden oluşturun ve yeniden Docker Hub'a gönderin.

```bash
docker build --rm -t flask-quickstart .
docker tag flask-quickstart <dockerhub_id>/flask-quickstart
docker push <dockerhub_id>/flask-quickstart
```

Cloud Shell'de uygulamayı yeniden başlatın. Uygulamayı yeniden başlatma, tüm ayarların uygulandığından ve kayıt defterinden en son kapsayıcının alındığından emin olmanızı sağlar.

```azurecli-interactive
az webapp restart --resource-group myResourceGroup --name <app_name>
```

App Service'in güncelleştirilmiş görüntüyü çekmesi için yaklaşık 15 saniye bekleyin. **Uygulamaya göz atma** adımında açılan tarayıcı penceresine dönüp sayfayı yenileyin.

![Azure'da çalışan güncelleştirilmiş örnek uygulama](media/quickstart-python/hello-azure-in-browser.png)

## <a name="manage-your-azure-web-app"></a>Azure web uygulamanızı yönetme

Oluşturduğunuz web uygulamasını görmek için [Azure portalına](https://portal.azure.com) gidin.

Sol menüden **Uygulama Hizmetleri**’ne ve ardından Azure web uygulamanızın adına tıklayın.

![Portaldan Azure web uygulamasına gitme](./media/quickstart-python/app-service-list.png)

Portal, varsayılan olarak web uygulamanızın **Genel Bakış** sayfasında görünür. Bu sayfa, uygulamanızın nasıl çalıştığını gösterir. Buradan ayrıca göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz. Sayfanın sol tarafındaki sekmeler, açabileceğiniz farklı yapılandırma sayfalarını gösterir.

![Azure portalında App Service sayfası](./media/quickstart-python/app-service-detail.png)

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [PostgreSQL ile Python](tutorial-docker-python-postgresql-app.md)

> [!div class="nextstepaction"]
> [Özel görüntüleri kullanma](tutorial-custom-docker-image.md)