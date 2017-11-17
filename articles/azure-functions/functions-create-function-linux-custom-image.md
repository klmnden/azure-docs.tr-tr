---
title: "Özel görüntü (Önizleme) kullanarak Linux üzerinde bir işlev oluşturun | Microsoft Docs"
description: "Azure özel Linux görüntü üzerinde çalışan işlevleri oluşturmayı öğrenin."
services: functions
keywords: 
author: ggailey777
ms.author: glenga
ms.date: 11/15/2017
ms.topic: article
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: cfowler
ms.openlocfilehash: 67ee02df2c42ba39c2f186cc95fa886a3d735ed2
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="create-a-function-on-linux-using-a-custom-image-preview"></a>Özel görüntü (Önizleme) kullanarak Linux üzerinde bir işlev oluşturun

Azure işlevleri, kendi özel kapsayıcısında Linux'ta işlevlerinizi konak olanak sağlar. Bu işlevsellik şu anda önizlemede değil. Ayrıca [varsayılan Azure uygulama hizmeti kapsayıcısını konakta](functions-create-first-azure-function-azure-cli-linux.md).  

Bu öğreticide, bir özel Docker görüntü olarak işlevi uygulamasının nasıl dağıtılacağını öğrenin. Yerleşik uygulama hizmeti kapsayıcı görüntüsünü özelleştirmek ihtiyacınız olduğunda bu deseni yararlıdır. İşlevlerinizi belirli bir dil sürümü gerekir ya da belirli bağımlılık veya içinde yerleşik görüntü sağlanmayan yapılandırma gerektiren özel bir görüntü kullanmak isteyebilirsiniz.

Bu öğreticide, Azure işlevleri oluşturma ve özel bir görüntü Docker hub'a gönderme için nasıl kullanılacağı açıklanmaktadır. Ardından Linux üzerinde çalışan bir işlev uygulaması için bu görüntü dağıtım kaynağı olarak kullanın. Derleme ve görüntü gönderme için Docker kullanın. Bir işlev uygulaması oluşturma ve Docker hub'a görüntüden dağıtmak için Azure CLI kullanın. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Docker kullanarak özel bir görüntü oluşturun.
> * Özel görüntü bir kapsayıcı kayıt defterine yayımlayın. 
> * Bir Azure depolama hesabı oluşturun. 
> * Bir Linux uygulama hizmeti planı oluşturun. 
> * Docker hub'a bir işlev uygulamasını dağıtın.
> * Uygulama ayarları işlevi uygulamaya ekleyin. 

Aşağıdaki adımlar, Mac, Windows veya Linux bilgisayarı üzerinde desteklenir.  

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* [Git](https://git-scm.com/downloads)
* Etkin bir [Azure aboneliği](https://azure.microsoft.com/pricing/free-trial/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Docker](https://docs.docker.com/get-started/#setup)
* A [Docker hub'a hesabı](https://docs.docker.com/docker-id/)

[!INCLUDE [Free trial note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Örneği indirme

Bir terminal penceresi örnek uygulama depoyu yerel makinenize kopyalayın ve ardından örnek kodunu içeren dizini değiştirmek için aşağıdaki komutu çalıştırın.

```bash
git clone https://github.com/Azure-Samples/functions-linux-custom-image.git --config core.autocrlf=input
cd functions-linux-custom-image
```

## <a name="build-the-image-from-the-docker-file"></a>Docker dosyasından yansıması oluştur

Bu Git deposunda bir göz atalım _Dockerfile_. Bu dosya, işlev uygulaması Linux üzerinde çalıştırmak için gerekli ortam tanımlar. 

```docker
# Base the image on the built-in Azure Functions Linux image.
FROM microsoft/azure-functions-runtime:2.0.0-jessie
ENV AzureWebJobsScriptRoot=/home/site/wwwroot

# Add files from this repo to the root site folder.
COPY . /home/site/wwwroot 
```
>[!NOTE]
> Bir özel kapsayıcı kayıt defteri görüntüdeki barındırdığında bağlantı ayarlarını işlev uygulaması kullanarak eklemelisiniz **ENV** Dockerfile değişkenleri. Bu öğretici, özel bir kayıt defteri kullanmanızı garanti edemez bağlantı ayarlarını olduklarından, [Azure CLI kullanarak dağıtım sonrasında eklenen](#configure-the-function-app) güvenlik açısından en iyisi.   

### <a name="run-the-build-command"></a>Yapı komutunu çalıştırın
Docker görüntü oluşturmak için çalıştırın `docker build` komut ve bir ad sağlayın `mydockerimage`ve etiket, `v1.0.0`. Değiştir `<docker-id>` Docker Hub'ınıza hesap kimliği.

```bash
docker build --tag <docker-id>/mydockerimage:v1.0.0 .
```

Komutu, aşağıdakine benzer bir çıktı üretir:

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

### <a name="test-the-image-locally"></a>Resmin Yerel olarak test etme
Yerleşik görüntü yerel bir kapsayıcıda Docker görüntüsünü çalıştırarak çalıştığını doğrulayın. Sorun [çalıştırmak docker](https://docs.docker.com/engine/reference/commandline/run/) komut ve adını ve etiket resminin ona geçirin. Bağlantı noktası kullanılarak belirttiğinizden emin olun `-p` bağımsız değişkeni.

```bash
docker run -p 8080:80 -it <docker-ID>/mydockerimage:v1.0.0
```

Özel görüntü ile yerel bir Docker kapsayıcısı içinde çalışan doğrulayın kapsayıcı ve işlev uygulaması düzgün göz atarak <http://localhost: 8080>.

![İşlev uygulaması yerel olarak test edin.](./media/functions-create-function-linux-custom-image/run-image-local-success.png)

Kapsayıcı işlevi uygulamada, hav doğrulandıktan sonra yürütmeyi durdurun. Şimdi, Docker hub'a hesabınıza özel görüntü gönderebilir.

## <a name="push-the-custom-image-to-docker-hub"></a>Özel görüntü Docker hub'a anında iletme

Bir kayıt defteri görüntüleri barındıran ve Hizmetleri görüntü ve kapsayıcı hizmetleri sağlayan bir uygulamadır. Görüntünüzü paylaşmak için bir kayıt defterine göndermelisiniz. Docker hub'a genel veya özel kendi depoları barındırmak izin veren bir kayıt defteri Docker görüntüleri için ' dir. 

Bir görüntü gönderebilir önce Docker hub'ı kullanarak oturum açmanız gerekir [docker oturum açma](https://docs.docker.com/engine/reference/commandline/login/) komutu. Değiştir `<docker-id>` hesap adınızı ve parolanızı yazın isteminde konsoluna ile. Diğer Docker hub'a parola seçenekleri için bkz: [docker oturum açma komut belgelerine](https://docs.docker.com/engine/reference/commandline/login/).

```bash
docker login --username <docker-id> 
```

"Oturum açma başarılı oldu" iletisi, oturum açtığınız onaylar. Oturum açtıktan sonra görüntüyü Docker hub'a kullanarak anında iletme [docker itme](https://docs.docker.com/engine/reference/commandline/push/) komutu.

```bash
docker push <docker-id>/mydockerimage:v1.0.0 .
```

Komut inceleyerek başarılı itme çıkışını olduğunu doğrulayın.

```bash
The push refers to a repository [docker.io/<docker-id>/mydockerimage:v1.0.0]
24d81eb139bf: Pushed
fd9e998161c9: Mounted from microsoft/azure-functions-runtime
e7796c35add2: Mounted from microsoft/azure-functions-runtime
ae9a05b85848: Mounted from microsoft/azure-functions-runtime
45c86e20670d: Mounted from microsoft/azure-functions-runtime
v1.0.0: digest: sha256:be080d80770df71234eb893fbe4d... size: 2422
```
Şimdi, Azure içinde yeni bir işlev uygulaması için dağıtım kaynağı olarak bu görüntüsü kullanabilirsiniz. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, bu konu Azure CLI Sürüm 2.0.21 gerektirir veya sonraki bir sürümü. Çalıştırma `az --version` sürüm bulunamadı. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>Linux uygulama hizmeti planı oluştur

Linux barındırma işlevleri için tüketim planlarında şu anda desteklenmiyor. Bir Linux uygulama hizmeti planı çalıştırmanız gerekir. Barındırma hakkında daha fazla bilgi için bkz: [Azure işlevleri barındırma planları karşılaştırma](functions-scale.md). 

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]


## <a name="create-and-deploy-the-custom-image"></a>Özel görüntü oluşturup

İşlevlerinizin yürütülmesini işlev uygulaması barındırır. Kullanarak bir Docker hub'a görüntüsünden bir işlev uygulaması oluşturma [az functionapp oluşturma](/cli/azure/functionapp#create) komutu. 

Aşağıdaki komutta, gördüğünüz benzersiz işlev uygulama adı yerine `<app_name>` yer tutucu ve depolama hesabı adı için `<storage_name>`. `<app_name>`, işlev uygulamasının varsayılan DNS etki alanı olarak kullanılacağı için adın Azure’daki tüm uygulamalarda benzersiz olması gerekir. Önceki gibi `<docker-id>` Docker hesap adı.

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

_Dağıtım kapsayıcı görüntü adı_ parametresi, işlev uygulaması oluşturmak için kullanılacak Docker hub'da barındırılan görüntü gösterir. 


## <a name="configure-the-function-app"></a>İşlev uygulamayı yapılandırma

İşlevi için varsayılan depolama hesabına bağlanmak için bağlantı dizesi gerekiyor. Bir özel kapsayıcı hesabına özel görüntünüzü yayımlarken, bunun yerine bu uygulama ayarları Dockerfile kullanarak ortam değişkenleri olarak ayarlamalısınız [ENV yönerge](https://docs.docker.com/engine/reference/builder/#env), ya da eşdeğer. 

Bu durumda, `<storage_account>` oluşturduğunuz depolama hesabının adıdır. Bağlantı dizesi alma [az depolama hesabı Göster bağlantı dizesi](/cli/azure/storage/account#show-connection-string) komutu. Bu uygulama ayarları işlevi uygulamayla eklemek [az functionapp config appsettings kümesi](/cli/azure/functionapp/config/appsettings#set) komutu.

```azurecli-interactive
storageConnectionString=$(az storage account show-connection-string \
--resource-group myResourceGroup --name <storage_account> \
--query connectionString --output tsv)

az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings AzureWebJobsDashboard=$storageConnectionString \
AzureWebJobsStorage=$storageConnectionString
```

Artık Azure Linux üzerinde çalışan işlevlerinizi test edebilirsiniz.

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Docker kullanarak özel bir görüntü oluşturun.
> * Özel görüntü bir kapsayıcı kayıt defterine yayımlayın. 
> * Bir Azure depolama hesabı oluşturun. 
> * Bir Linux uygulama hizmeti planı oluşturun. 
> * Docker hub'a bir işlev uygulamasını dağıtın.
> * Uygulama ayarları işlevi uygulamaya ekleyin.

Azure işlevleri çekirdek araçları kullanarak yerel olarak Azure işlevleri geliştirme hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"] 
> [Yerel kod ve test Azure işlevleri](functions-run-local.md)
