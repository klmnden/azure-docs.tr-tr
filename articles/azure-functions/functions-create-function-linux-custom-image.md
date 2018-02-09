---
title: "Linux üzerinde özel görüntü kullanarak bir işlev oluşturma (önizleme) | Microsoft Docs"
description: "Özel bir Linux görüntüsü üzerinde çalışan Azure İşlevleri oluşturmayı öğrenin."
services: functions
keywords: 
author: ggailey777
ms.author: glenga
ms.date: 11/15/2017
ms.topic: tutorial
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: cfowler
ms.openlocfilehash: 555d05c6cd5e804e5f80ecb8df77237fd8270105
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="create-a-function-on-linux-using-a-custom-image-preview"></a>Linux üzerinde özel görüntü kullanarak bir işlev oluşturma (önizleme)

Azure İşlevleri, işlevlerinizi Linux’ta kendi özel kapsayıcınızda barındırmanıza olanak sağlar. Ayrıca, [varsayılan bir Azure App Service kapsayıcısı üzerinde barındırabilirsiniz](functions-create-first-azure-function-azure-cli-linux.md). Bu işlevsellik şu anda önizleme aşamasındadır ve yine önizleme aşamasında olan [İşlevler 2.0 çalışma zamanını](functions-versions.md) gerektirir.

Bu öğreticide, bir işlev uygulamasını özel bir Docker görüntüsü olarak dağıtmayı öğreneceksiniz. Yerleşik App Service kapsayıcı görüntüsünü özelleştirmeniz gerektiğinde bu desen yararlıdır. İşlevleriniz belirli bir dil sürümüne gereksinim duyduğunda veya yerleşik görüntüde sağlanmayan belirli bir bağımlılık ya da yapılandırma gerektirdiğinde özel görüntü kullanmak isteyebilirsiniz.

Bu öğreticide, Azure İşlevleri’ni kullanarak özel görüntü oluşturma ve Docker Hub’a gönderme işlemi adım adım gösterilmektedir. Daha sonra bu görüntüyü, Linux üzerinde çalışan bir işlev uygulaması için dağıtım kaynağı olarak kullanırsınız. Docker kullanarak görüntüyü derleyip gönderirsiniz. Bir işlev uygulaması oluşturmak ve görüntüyü Docker Hub'dan dağıtmak için Azure CLI kullanırsınız. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Docker kullanarak özel bir görüntü oluşturun.
> * Özel görüntüyü bir kapsayıcı kayıt defterinde yayımlayın. 
> * Bir Azure Depolama hesabı oluşturun. 
> * Bir Linux App Service planı oluşturun. 
> * Docker Hub’dan bir işlev uygulaması dağıtın.
> * Uygulama ayarlarını işlevi uygulamasına ekleyin. 

Aşağıdaki adımlar Mac, Windows veya Linux bilgisayarlarda desteklenir.  

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* [Git](https://git-scm.com/downloads)
* Etkin bir [Azure aboneliği](https://azure.microsoft.com/pricing/free-trial/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Docker](https://docs.docker.com/get-started/#setup)
* [Docker Hub hesabı](https://docs.docker.com/docker-id/)

[!INCLUDE [Free trial note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Örneği indirme

Terminal penceresinde, örnek uygulama deposunu yerel makinenize kopyalamak ve örnek kodu içeren dizine gitmek için aşağıdaki komutları çalıştırın.

```bash
git clone https://github.com/Azure-Samples/functions-linux-custom-image.git --config core.autocrlf=input
cd functions-linux-custom-image
```

## <a name="build-the-image-from-the-docker-file"></a>Docker dosyasından görüntüyü oluşturma

Bu Git deposunda _Dockerfile_ dosyasına göz atın. Bu dosya, işlev uygulamasını Linux üzerinde çalıştırmak için gereken ortamı açıklar. 

```docker
# Base the image on the built-in Azure Functions Linux image.
FROM microsoft/azure-functions-runtime:2.0.0-jessie
ENV AzureWebJobsScriptRoot=/home/site/wwwroot

# Add files from this repo to the root site folder.
COPY . /home/site/wwwroot 
```
>[!NOTE]
> Bir görüntüyü özel bir kapsayıcı kayıt defterinde barındırırken, Dockerfile dosyasındaki **ENV** değişkenlerini kullanarak işlev uygulamasına bağlantı ayarlarını eklemeniz gerekir. Bu öğretici özel bir kayıt defteri kullanmanızı garanti edemediğinden, en iyi güvenlik yöntemi olarak bağlantı ayarları, [dağıtımdan sonra Azure CLI kullanılarak eklenir](#configure-the-function-app).   

### <a name="run-the-build-command"></a>Derleme komutunu çalıştırma
Docker görüntüsünü oluşturmak için, `docker build` komutunu çalıştırın ve bir ad (`mydockerimage`) ve etiket (`v1.0.0`) sağlayın. `<docker-id>` değerini Docker Hub hesabınızın kimliğiyle değiştirin.

```bash
docker build --tag <docker-id>/mydockerimage:v1.0.0 .
```

Komut aşağıdakine benzer bir çıkış oluşturur:

```bash
Sending build context to Docker daemon  169.5kB
Step 1/3 : FROM microsoft/azure-functions-runtime:v2.0.0-jessie
v2.0.0-jessie: Pulling from microsoft/azure-functions-runtime
b178b12f7913: Pull complete
2d9ce077a781: Pull complete
4775d4ba55c8: Pull complete
Digest: sha256:073f45fc167b3b5c6642ef4b3c99064430d6b17507095...
Status: Downloaded newer image for microsoft/azure-functions-runtime:v2.0.0-jessie
 ---> 217799efa500
Step 2/3 : ENV AzureWebJobsScriptRoot /home/site/wwwroot
 ---> Running in 528fa2077d17
 ---> 7cc6323b8ae0
Removing intermediate container 528fa2077d17
Step 3/3 : COPY . /home/site/wwwroot
 ---> 5bdac9878423
Successfully built 5bdac9878423
Successfully tagged ggailey777/mydockerimage:v1.0.0
```

### <a name="test-the-image-locally"></a>Görüntüyü yerel olarak test etme
Docker görüntüsünü yerel bir kapsayıcıda çalıştırarak, derlenen görüntünün çalıştığını doğrulayın. [docker run](https://docs.docker.com/engine/reference/commandline/run/) komutunu gönderin ve bu komuta görüntünün adını ve etiketini geçirin. `-p` bağımsız değişkenini kullanarak bağlantı noktasını belirttiğinizden emin olun.

```bash
docker run -p 8080:80 -it <docker-ID>/mydockerimage:v1.0.0
```

Özel görüntü ile yerel bir Docker kapsayıcısı içinde çalışırken <http://localhost:8080> sayfasına göz atarak uygulama ve kapsayıcının doğru şekilde çalıştığını onaylayın.

![İşlev uygulamasını yerel olarak test edin.](./media/functions-create-function-linux-custom-image/run-image-local-success.png)

Kapsayıcıda işlev uygulamasını doğruladıktan sonra, yürütmeyi durdurun. Şimdi, özel görüntüyü Docker Hub hesabınıza gönderebilirsiniz.

## <a name="push-the-custom-image-to-docker-hub"></a>Özel görüntüyü Docker Hub’a gönderme

Kayıt defteri, görüntüleri barındıran ve hizmet görüntüsüyle kapsayıcı hizmetleri sağlayan bir uygulamadır. Görüntünüzü paylaşmak için bunu kayıt defterine göndermeniz gerekir. Docker Hub, Docker görüntüleri için kayıt defteridir ve kendi depolarınızı (ortak veya özel) barındırmanıza olanak tanır. 

Görüntüyü gönderebilmek için önce [docker login](https://docs.docker.com/engine/reference/commandline/login/) komutunu kullanarak Docker Hub'da oturum açmalısınız. `<docker-id>` yerine hesabınızı kullanın ve konsoldaki bilgi istemine parolanızı yazın. Diğer Docker Hub parola seçenekleri için [docker oturum açma komutu belgelerine](https://docs.docker.com/engine/reference/commandline/login/) bakın.

```bash
docker login --username <docker-id> 
```

"Oturum açma başarılı oldu" iletisi, oturumunuzun açıldığını onaylar. Oturum açtıktan sonra [docker push](https://docs.docker.com/engine/reference/commandline/push/) komutunu kullanarak görüntüyü Docker Hub’a gönderin.

```bash
docker push <docker-id>/mydockerimage:v1.0.0 .
```

Komutun çıkışını inceleyerek göndermenin başarılı olduğunu doğrulayın.

```bash
The push refers to a repository [docker.io/<docker-id>/mydockerimage:v1.0.0]
24d81eb139bf: Pushed
fd9e998161c9: Mounted from microsoft/azure-functions-runtime
e7796c35add2: Mounted from microsoft/azure-functions-runtime
ae9a05b85848: Mounted from microsoft/azure-functions-runtime
45c86e20670d: Mounted from microsoft/azure-functions-runtime
v1.0.0: digest: sha256:be080d80770df71234eb893fbe4d... size: 2422
```
Artık bu görüntüyü, Azure’da yeni bir işlev uygulaması için dağıtım kaynağı olarak kullanabilirsiniz. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0.21 veya sonraki bir sürümünü kullanmanız gerekir. Kullandığınız sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>Bir Linux App Service planı oluşturma

Linux İşlev barındırma şu anda tüketim planlarında desteklenmemektedir. Bir Linux App Service planı üzerinde çalıştırmanız gerekir. Barındırma hakkında daha fazla bilgi edinmek için, bkz. [Azure İşlevleri barındırma planları karşılaştırması](functions-scale.md). 

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]


## <a name="create-and-deploy-the-custom-image"></a>Özel görüntü oluşturup dağıtma

İşlev uygulaması, işlevlerinizin yürütülmesini barındırır. [az functionapp create](/cli/azure/functionapp#az_functionapp_create) komutunu kullanarak bir Docker Hub görüntüsünden işlev uygulaması oluşturun. 

Aşağıdaki komutta benzersiz bir işlev uygulamasının adını `<app_name>` yer tutucusunun ve `<storage_name>` depolama hesabı adının yerine ekleyin. `<app_name>`, işlev uygulamasının varsayılan DNS etki alanı olarak kullanılacağı için adın Azure’daki tüm uygulamalarda benzersiz olması gerekir. Daha önce olduğu gibi, `<docker-id>` Docker hesap adınızdır.

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--plan myAppServicePlan --deployment-container-image-name <docker-id>/mydockerimage:v1.0.0 
```
İşlev uygulaması oluşturulduktan sonra Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "containerSize": 1536,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "quickstart.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "quickstart.azurewebsites.net",
    "quickstart.scm.azurewebsites.net"
  ],
   ....
    // Remaining output has been truncated for readability.
}
```

_deployment-container-image-name_ parametresi, işlev uygulamasını oluşturmak üzere kullanmak için Docker Hub üzerinde barındırılan görüntüyü belirtir. 


## <a name="configure-the-function-app"></a>İşlev uygulamasını yapılandırma

İşlevin varsayılan depolama hesabına bağlanması için bağlantı dizesi gerekir. Özel görüntünüzü bir özel kapsayıcı hesabında yayımlarken, bunun yerine [ENV instruction](https://docs.docker.com/engine/reference/builder/#env) ya da eşdeğerini kullanarak Dockerfile dosyasında ortam değişkeni olarak bu uygulama ayarlarını belirlemeniz gerekir. 

Bu durumda `<storage_account>`, oluşturduğunuz depolama hesabının adıdır. [az storage account show-connection-string](/cli/azure/storage/account#show-connection-string) komutu ile bağlantı dizesini alın. [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#az_functionapp_config_appsettings_set) komutu ile bu uygulama ayarlarını işlev uygulamasına ekleyin.

```azurecli-interactive
storageConnectionString=$(az storage account show-connection-string \
--resource-group myResourceGroup --name <storage_account> \
--query connectionString --output tsv)

az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings AzureWebJobsDashboard=$storageConnectionString \
AzureWebJobsStorage=$storageConnectionString
```

Artık Azure’da Linux üzerinde çalışan işlevlerinizi test edebilirsiniz.

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Docker kullanarak özel bir görüntü oluşturun.
> * Özel görüntüyü bir kapsayıcı kayıt defterinde yayımlayın. 
> * Bir Azure Depolama hesabı oluşturun. 
> * Bir Linux App Service planı oluşturun. 
> * Docker Hub’dan bir işlev uygulaması dağıtın.
> * Uygulama ayarlarını işlevi uygulamasına ekleyin.

Azure İşlevleri Çekirdek Araçları’nı kullanarak yerel olarak Azure İşlevleri geliştirme hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"] 
> [Azure İşlevleri’ni yerel olarak kodlama ve test etme](functions-run-local.md)
