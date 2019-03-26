---
title: Kapsayıcılar - Azure App Service için Web uygulaması için özel Docker görüntüsü kullanma | Microsoft Docs
description: Kapsayıcılar için Web App’e yönelik özel Docker görüntüsü kullanın.
keywords: azure app service, web uygulaması, linux, docker, kapsayıcı
services: app-service
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/24/2017
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: 98e690ab73b9a51126f4eae9ac5eff410e211957
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58435851"
---
# <a name="use-a-custom-docker-image-for-web-app-for-containers"></a>Kapsayıcılar için Web App’e yönelik özel Docker görüntüsü kullanma

[Kapsayıcılar için Web App](app-service-linux-intro.md), Linux’ta PHP 7.0 ve Node.js 4.5 gibi belirli sürümleri destekleyen yerleşik Docker görüntüleri sağlar. Kapsayıcılar için Web App hem yerleşik görüntüleri hem de özel görüntüleri bir hizmet olarak platform şeklinde barındırmak için Docker kapsayıcı teknolojisini kullanır. Bu öğreticide, özel Docker görüntüsü oluşturmayı ve bunu Kapsayıcılar için Web App'e dağıtmayı öğreneceksiniz. Bu desen, yerleşik görüntülerin sizin tercih ettiğiniz dili içermediği veya uygulamanızın yerleşik görüntülerde sağlanmayan belirli bir yapılandırma gerektirdiği durumlarda yararlı olur.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure'a özelleştirilmiş bir Docker görüntüsü dağıtma
> * Kapsayıcıyı çalıştırmak için ortam değişkenlerini yapılandırma
> * Docker görüntüsünü güncelleştirme ve yeniden dağıtma
> * SSH kullanarak kapsayıcıya bağlanma
> * Azure'a özel bir Docker görüntüsü dağıtma

[!INCLUDE [Free trial note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* [Git](https://git-scm.com/downloads)
* [Docker](https://docs.docker.com/get-started/#setup)
* [Docker Hub hesabı](https://docs.docker.com/docker-id/)

## <a name="download-the-sample"></a>Örneği indirme

Terminal penceresinde, örnek uygulama deposunu yerel makinenize kopyalamak ve örnek kodu içeren dizine gitmek için aşağıdaki komutları çalıştırın.

```bash
git clone https://github.com/Azure-Samples/docker-django-webapp-linux.git --config core.autocrlf=input
cd docker-django-webapp-linux
```

## <a name="build-the-image-from-the-docker-file"></a>Docker dosyasından görüntüyü oluşturma

Git deposunda _Dockerfile_ dosyasına göz atın. Bu dosya, uygulamanızı çalıştırmak için gereken Python ortamını açıklar. Görüntü ayrıca, kapsayıcı ile konak arasında güvenli iletişim için bir [SSH](https://www.ssh.com/ssh/protocol/) sunucusu ayarlar.

```Dockerfile
FROM python:3.4

RUN mkdir /code
WORKDIR /code
ADD requirements.txt /code/
RUN pip install -r requirements.txt
ADD . /code/

# ssh
ENV SSH_PASSWD "root:Docker!"
RUN apt-get update \
        && apt-get install -y --no-install-recommends dialog \
        && apt-get update \
    && apt-get install -y --no-install-recommends openssh-server \
    && echo "$SSH_PASSWD" | chpasswd 

COPY sshd_config /etc/ssh/
COPY init.sh /usr/local/bin/
    
RUN chmod u+x /usr/local/bin/init.sh
EXPOSE 8000 2222
#CMD ["python", "/code/manage.py", "runserver", "0.0.0.0:8000"]
ENTRYPOINT ["init.sh"]
```

Docker görüntüsünü oluşturmak için, `docker build` komutunu çalıştırın ve bir ad (_mydockerimage_) ve etiket (_v1.0.0_) sağlayın. _\<docker-kimliği>_ değerini Docker Hub hesabınızın kimliğiyle değiştirin.

```bash
docker build --tag <docker-id>/mydockerimage:v1.0.0 .
```

Komut aşağıdakine benzer bir çıkış oluşturur:

```
# The output from the commands in this article has been shortened for brevity.

Sending build context to Docker daemon  5.558MB
Step 1/13 : FROM python:3.4
 ---> 9ff45ddb54e9
Step 2/13 : RUN mkdir /code
 ---> Using cache
 ---> f3f3ac01db0a
Step 3/13 : WORKDIR /code
 ---> Using cache
 ---> 38b32f15b442
.
.
.
Step 13/13 : ENTRYPOINT init.sh
 ---> Running in 5904e4c70043
 ---> e7cf08275692
Removing intermediate container 5904e4c70043
Successfully built e7cf08275692
Successfully tagged cephalin/mydockerimage:v1.0.0
```

Docker kapsayıcısını çalıştırarak derlemenin çalışmasını test edin. [`docker run`](https://docs.docker.com/engine/reference/commandline/run/) komutunu gönderin ve bu komuta görüntünün adını ve etiketini geçirin. `-p` bağımsız değişkenini kullanarak bağlantı noktasını belirttiğinizden emin olun.

```bash
docker run -p 2222:8000 <docker-ID>/mydockerimage:v1.0.0
```

`http://localhost:2222` konumuna göz atarak web uygulamasının ve kapsayıcının düzgün çalıştığını doğrulayın.

![Web uygulamasını yerel olarak test etme](./media/app-service-linux-using-custom-docker-image/app-service-linux-browse-local.png)

> [!NOTE] 
> SSH, SFTP veya Visual Studio Code (Node.js apps canlı hata ayıklaması için) kullanarak doğrudan yerel geliştirme makinenizden de uygulama kapsayıcısına bağlanabilirsiniz. Daha fazla bilgi için bkz. [Linux üzerinde App Service’te uzaktan hata ayıklama ve SSH](https://aka.ms/linux-debug).
>

## <a name="push-the-docker-image-to-docker-hub"></a>Docker görüntüsünü Docker Hub'a gönderme

Kayıt defteri, görüntüleri barındıran ve hizmet görüntüsüyle kapsayıcı hizmetleri sağlayan bir uygulamadır. Görüntünüzü paylaşmak için bunu kayıt defterine göndermeniz gerekir. 

<!-- Depending on your requirements, you may have your docker images in a Public Docker Registry, such as Docker Hub, or a Private Docker Registry such as Azure Container Registry. Select the appropriate tab for your scenario below (your selection will switch multiple tabs on this page). -->

> [!NOTE]
> Özel Docker Kayıt Defteri'ne göndermek mi? [Herhangi bir kayıt defterinden Docker görüntüsünü kullanma](#use-a-docker-image-from-any-private-registry-optional) işleminin isteğe bağlı yönergelerine bakın.

<!--## [Docker Hub](#tab/docker-hub)-->

Docker Hub, Docker görüntüleri için kayıt defteridir ve kendi depolarınızı (ortak veya özel) barındırmanıza olanak tanır. Özel Docker görüntüsünü ortak Docker Hub'a göndermek için [`docker push`](https://docs.docker.com/engine/reference/commandline/push/) komutunu kullanın ve tüm görüntü adıyla etiketini sağlayın. Tam görüntü adı ve etiketi aşağıdaki örneğe benzer:

```
<docker-id>/image-name:tag
```

Görüntüyü gönderebilmek için önce [`docker login`](https://docs.docker.com/engine/reference/commandline/login/) komutunu kullanarak Docker Hub'da oturum açmalısınız. _\<docker-id>_ yerine hesabınızı kullanın ve konsoldaki bilgi istemine parolanızı yazın.

```bash
docker login --username <docker-id>
```

"Oturum açma başarılı oldu" iletisi, oturumunuzun açıldığını onaylar. Oturumu açtıktan sonra, [`docker push`](https://docs.docker.com/engine/reference/commandline/push/) komutunu kullanarak görüntüyü Docker Hub'a gönderebilirsiniz.

```bash
docker push <docker-id>/mydockerimage:v1.0.0
```

Komutun çıkışını inceleyerek göndermenin başarılı olduğunu doğrulayın.

```
The push refers to a repository [docker.io/<docker-id>/mydockerimage:v1.0.0]
c33197c3f6d4: Pushed
ccd2c850ee43: Pushed
02dff2853466: Pushed
6ce78153632a: Pushed
efef3f03cc58: Pushed
3439624d77fb: Pushed
3a07adfb35c5: Pushed
2fcec228e1b7: Mounted from library/python
97d2d3bae505: Mounted from library/python
95aadeabf504: Mounted from library/python
b456afdc9996: Mounted from library/python
d752a0310ee4: Mounted from library/python
db64edce4b5b: Mounted from library/python
d5d60fc34309: Mounted from library/python
c01c63c6823d: Mounted from library/python
v1.0.0: digest: sha256:21f2798b20555f4143f2ca0591a43b4f6c8138406041f2d32ec908974feced66 size: 3676
```

<!--
# [Private Registry](#tab/private-registry)

// Move Private Registry instructions here when Tabbed Conceptual bug is fixed

---
-->

[!INCLUDE [Try Cloud Shell](../../../includes/cloud-shell-try-it.md)]

## <a name="deploy-app-to-azure"></a>Uygulamayı Azure’da dağıtma

Gönderdiğiniz görüntünün kullanan bir uygulamayı oluşturmak için bir grubu ardından bir hizmet planına ve son olarak web uygulaması oluşturmayı ve Azure CLI komutlarını çalıştırın. 

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)] 

### <a name="create-a-linux-app-service-plan"></a>Bir Linux App Service planı oluşturma

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)] 

### <a name="create-a-web-app"></a>Web uygulaması oluşturma

Cloud Shell’de, [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) komutuyla `myAppServicePlan` App Service planında bir [web uygulaması](app-service-linux-intro.md) oluşturun. _<appname>_ değerini benzersiz bir uygulama adıyla ve _\<docker-ID>_ öğesini kendi Docker kimliğinizle değiştirmeyi unutmayın.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --deployment-container-image-name <docker-ID>/mydockerimage:v1.0.0
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
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

### <a name="configure-environment-variables"></a>Ortam değişkenlerini yapılandırma

Çoğu Docker görüntüsünün, yapılandırılması gereken ortam değişkenleri vardır. Başka biri tarafından hazırlanmış mevcut bir Docker görüntüsü kullanıyorsanız, görüntü 80'den farklı bir bağlantı noktası kullanıyor olabilir. `WEBSITES_PORT` uygulama ayarını kullanarak Azure'a görüntünüzün kullandığı bağlantı noktası hakkında bilgi verirsiniz. [Bu öğreticideki Python örneği](https://github.com/Azure-Samples/docker-django-webapp-linux) için GitHub sayfası, `WEBSITES_PORT` olarak _8000_ ayarlamanız gerektiğini gösterir.

Uygulama ayarlarını belirlemek için Cloud Shell'de [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) komutunu kullanın. Uygulama ayarları büyük/küçük harfe duyarlıdır ve boşlukla ayrılmıştır.

```azurecli-interactive
az webapp config appsettings set --resource-group myResourceGroup --name <app_name> --settings WEBSITES_PORT=8000
```

<!-- Depending on your requirements, you may have your docker images in a Public Docker Registry, such as Docker Hub, or a Private Docker Registry, such as Azure Container Registry. Select the appropriate tab for your scenario below: -->

> [!NOTE]
> Özel Docker Kayıt Defteri'nden mi dağıtım yapılıyor? [Herhangi bir kayıt defterinden Docker görüntüsünü kullanma](#use-a-docker-image-from-any-private-registry-optional) işleminin isteğe bağlı yönergelerine bakın.

<!-- # [Docker Hub](#tab/docker-hub)-->

<!-- # [Private Registry](#tab/private-registry)

// Place Private Registry text back here once Tabbed Conceptual bug is fixed

---
-->

### <a name="test-the-web-app"></a>Web uygulamasını test etme

Web uygulamasına göz atarak uygulamanın çalıştığını doğrulayın (`http://<app_name>.azurewebsites.net`). 

![Web uygulaması bağlantı noktası yapılandırmasını test etme](./media/app-service-linux-using-custom-docker-image/app-service-linux-browse-azure.png)

## <a name="change-web-app-and-redeploy"></a>Web uygulamasını değiştirme ve yeniden dağıtma

Git deponuzda app/templates/app/index.html dosyasını açın. İlk HTML öğesini bulun ve şöyle değiştirin.

```python
<nav class="navbar navbar-inverse navbar-fixed-top">
    <div class="container">
      <div class="navbar-header">         
        <a class="navbar-brand" href="#">Azure App Service - Updated Here!</a>       
      </div>            
    </div>
  </nav> 
```

Python dosyasını değiştirdikten ve kaydettikten sonra, yeni Docker görüntüsünü yeniden oluşturup göndermeniz gerekir. Ardından değişikliklerin geçerlilik kazanması için web uygulamasını yeniden başlatın. Bu öğreticide daha önce kullandığınız komutları kullanın. [Docker dosyasından görüntüyü oluşturma](#build-the-image-from-the-docker-file) ve [Docker görüntüsünü Docker Hub'a gönderme](#push-the-docker-image-to-docker-hub) bölümlerine bakabilirsiniz. [Web uygulamasını test etme](#test-the-web-app) başlığı altındaki yönergeleri izleyerek web uygulamasını test edin.

## <a name="connect-to-web-app-for-containers-using-ssh"></a>SSH kullanarak Kapsayıcılar için Web App'e bağlanma

SSH, kapsayıcı ile istemci arasında güvenli iletişime olanak tanır. Özel Docker görüntüsünün SSH'yi desteklemesi için, bunu Dockerfile içinde oluşturmanız gerekir. SSH'yi Docker dosyasının kendi içinden etkinleştirirsiniz. SSH yönergeleri örnek dockerfile içine zaten eklenmiştir, dolayısıyla kendi özel görüntünüzde bu yönergeleri izleyebilirsiniz:

* `apt-get` çağrısı yapan ve ardından kök hesabın parolasını `"Docker!"` olarak ayarlayan bir [RUN](https://docs.docker.com/engine/reference/builder/#run) yönergesi.

    ```Dockerfile
    ENV SSH_PASSWD "root:Docker!"
    RUN apt-get update \
            && apt-get install -y --no-install-recommends dialog \
            && apt-get update \
      && apt-get install -y --no-install-recommends openssh-server \
      && echo "$SSH_PASSWD" | chpasswd 
    ```

    > [!NOTE]
    > Bu yapılandırma kapsayıcıya dış bağlantılar kurulmasına izin vermez. SSH yalnızca Kudu/SCM Sitesi aracılığıyla kullanılabilir. Kudu/SCM sitesinin kimliği yayımlama kimlik bilgileriyle doğrulanır.

* Docker altyapısına [sshd_config](https://man.openbsd.org/sshd_config) dosyasını */etc/ssh/* dizinine taşımasını bildiren bir [COPY](https://docs.docker.com/engine/reference/builder/#copy) yönergesi. Yapılandırma dosyanızın [bu sshd_config dosyasına](https://github.com/Azure-App-Service/node/blob/master/6.11.1/sshd_config) dayanması gerekir.

    ```Dockerfile
    COPY sshd_config /etc/ssh/
    ```

    > [!NOTE]
    > *Sshd_config* dosyası şu öğeleri içermelidir: 
    > * `Ciphers`, bu listedeki en az bir öğeyi içermelidir: `aes128-cbc,3des-cbc,aes256-cbc`.
    > * `MACs`, bu listedeki en az bir öğeyi içermelidir: `hmac-sha1,hmac-sha1-96`.

* Kapsayıcıda 2222 numaralı bağlantı noktasını ortaya çıkaran [EXPOSE](https://docs.docker.com/engine/reference/builder/#expose) yönergesi. Kök parolası biliniyor olsa da, 2222 numaralı bağlantı noktasına İnternet üzerinden erişilemez. Bu, yalnızca özel sanal ağın köprü ağı içinde yer alan kapsayıcıların erişebildiği dahili bir bağlantı noktasıdır. Bundan sonra, komutlar SSH yapılandırma ayrıntılarını kopyalar ve `ssh` hizmetini başlatır.

    ```Dockerfile
    EXPOSE 8000 2222
    ```

* /bin dizininde kabuk betiği kullanarak [ssh hizmetini başlatmayı](https://github.com/Azure-App-Service/node/blob/master/8.9/startup/init_container.sh#L18) unutmayın.
 
    ```bash
    #!/bin/bash
    service ssh start
    ```
     
### <a name="open-ssh-connection-to-container"></a>Kapsayıcıya SSH bağlantısı açma

Kapsayıcılar için Web App kapsayıcıya dış bağlantılar kurulmasına izin vermez. SSH yalnızca `https://<app_name>.scm.azurewebsites.net` adresinden erişilebilen Kudu sitesi aracılığıyla kullanılabilir.

Bağlanmak için, `https://<app_name>.scm.azurewebsites.net/webssh/host` adresine göz atın ve Azure hesabınızla oturum açın.

Ardından etkileşimli konsolu görüntüleyen bir sayfaya yeniden yönlendirilirsiniz. 

Bazı uygulamaların kapsayıcı içinde çalıştırıldığını doğrulamak isteyebilirsiniz. Kapsayıcıyı incelemek ve çalıştırma işlemlerini doğrulamak için, komut isteminde `top` komutunu verin.

```bash
top
```

`top` komutu kapsayıcıda çalıştırılan tüm işlemleri ortaya çıkarır.

```
PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
 1 root      20   0  945616  35372  15348 S  0.0  2.1   0:04.63 node
20 root      20   0   55180   2776   2516 S  0.0  0.2   0:00.00 sshd
42 root      20   0  944596  33340  15352 S  0.0  1.9   0:05.80 node /opt/s+
56 root      20   0   59812   5244   4512 S  0.0  0.3   0:00.93 sshd
58 root      20   0   20228   3128   2664 S  0.0  0.2   0:00.00 bash
62 root      20   0   21916   2272   1944 S  0.0  0.1   0:03.15 top
63 root      20   0   59812   5344   4612 S  0.0  0.3   0:00.03 sshd
65 root      20   0   20228   3140   2672 S  0.0  0.2   0:00.00 bash
71 root      20   0   59812   5380   4648 S  0.0  0.3   0:00.02 sshd
73 root      20   0   20228   3160   2696 S  0.0  0.2   0:00.00 bash
77 root      20   0   21920   2304   1972 R  0.0  0.1   0:00.00 top
```

Tebrikler! Kapsayıcılar için Web App’e yönelik özel Docker görüntüsü yapılandırdınız.

## <a name="use-a-private-image-from-docker-hub-optional"></a>Docker Hub'dan özel bir görüntü kullanma (isteğe bağlı)

[Web uygulaması oluşturma](#create-a-web-app) adımında, `az webapp create` komutuyla Docker Hub'da bir görüntü belirttiniz. Bu, genel bir görüntü için yeterlidir. Özel görüntü kullanmak için, Azure web uygulamanızda Docker hesabınızın kimliğini ve parolasını yapılandırmanız gerekir.

Cloud Shell'de, `az webapp create` komutunu [`az webapp config container set`](/cli/azure/webapp/config/container?view=azure-cli-latest#az-webapp-config-container-set) ile izleyin. *\<app_name>* değerini ve ayrıca _\<docker-id>_ ile _\<password>_ değerlerini Docker kimliğiniz ve parolanızla değiştirin.

```azurecli-interactive
az webapp config container set --name <app_name> --resource-group myResourceGroup --docker-registry-server-user <docker-id> --docker-registry-server-password <password>
```

Komut aşağıdaki JSON dizesine benzer, yapılandırma değişikliğinin başarılı olduğunu gösteren bir çıkış döndürür:

```json
[
  {
    "name": "WEBSITES_ENABLE_APP_SERVICE_STORAGE",
    "slotSetting": false,
    "value": "false"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_USERNAME",
    "slotSetting": false,
    "value": "<docker-id>"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_PASSWORD",
    "slotSetting": false,
    "value": null
  },
  {
    "name": "DOCKER_CUSTOM_IMAGE_NAME",
    "value": "DOCKER|<image-name-and-tag>"
  }
]
```

## <a name="use-a-docker-image-from-any-private-registry-optional"></a>Herhangi bir özel kayıt defterinden Docker görüntüsünü kullanma (isteğe bağlı)

Bu bölümde, Kapsayıcılar için Web App'te özel bir kayıt defterinden Docker görüntüsü kullanma işlemi öğretilir ve örnek olarak Azure Container Registry kullanılır. Diğer özel kayıt defterlerini kullanma adımları da buradakilere benzer. 

Azure Container Registry, özel görüntüleri barındırmak üzere Azure'dan yönetilen bir Docker hizmetidir. Dağıtımlar [Docker Swarm](https://docs.docker.com/engine/swarm/), [Kubernetes](https://kubernetes.io/) ve Kapsayıcılar için Web App da dahil olmak üzere herhangi bir türde olabilir. 

### <a name="create-an-azure-container-registry"></a>Azure Container Registry oluşturma

Cloud Shell'de, [`az acr create`](/cli/azure/acr?view=azure-cli-latest#az-acr-create) komutunu kullanarak bir Azure Container Registry oluşturun. Buna adı, kaynak grubunu ve SKU için `Basic` değerini geçirin. Kullanılabilir SKU'lar `Classic`, `Basic`, `Standard` ve `Premium`'dur.

```azurecli-interactive
az acr create --name <azure-container-registry-name> --resource-group myResourceGroup --sku Basic --admin-enabled true
```

Kapsayıcı oluşturulduğunda aşağıdaki çıkış üretilir:

```
 - Finished ..
Create a new service principal and assign access:
  az ad sp create-for-rbac --scopes /subscriptions/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/<azure-container-registry-name> --role Owner --password <password>

Use an existing service principal and assign access:
  az role assignment create --scope /subscriptions/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/<azure-container-registry-name> --role Owner --assignee <app-id>
{
  "adminUserEnabled": false,
  "creationDate": "2017-08-09T04:21:09.654153+00:00",
  "id": "/subscriptions/<subscriptionId>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/<azure-container-registry-name>",
  "location": "westeurope",
  "loginServer": "<azure-container-registry-name>.azurecr.io",
  "name": "<azure-container-registry-name>",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "myazurecontainerre042025"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

### <a name="log-in-to-azure-container-registry"></a>Azure Container Registry oturumu açma

Görüntüyü kayıt defterine göndermek için, kimlik bilgilerini girerek kayıt defterinin gönderimi kabul etmesini sağlamanız gerekir. Cloud Shell'de [`az acr show`](/cli/azure/acr?view=azure-cli-latest#az-acr-show) komutunu kullanarak bu kimlik bilgilerini alabilirsiniz. 

```azurecli-interactive
az acr credential show --name <azure-container-registry-name>
```

Komut, kullanıcı adıyla kullanılabilen iki parola ortaya çıkarır.

```json
<
  "passwords": [
    {
      "name": "password",
      "value": "{password}"
    },
    {
      "name": "password2",
      "value": "{password}"
    }
  ],
  "username": "<registry-username>"
}
```

Yerel terminal pencerenizde, `docker login` komutunu kullanarak Azure Container Registry oturumu açın. Oturum açmak için sunucu adı gerekir. `{azure-container-registry-name>.azurecr.io` biçimini kullanın. Konsoldaki bilgi istemine parolanızı yazın.

```bash
docker login <azure-container-registry-name>.azurecr.io --username <registry-username>
```

Oturumun başarıyla açıldığını onaylayın. 

### <a name="push-an-image-to-azure-container-registry"></a>Azure Container Registry’ye görüntü gönderme

> [!NOTE]
> Kendi görüntünüzü kullanıyorsanız, görüntüyü aşağıda gösterildiği gibi etiketleyin:
> ```bash
> docker tag <azure-container-registry-name>.azurecr.io/mydockerimage
> ```

`docker push` komutunu kullanarak görüntüyü gönderin. Görüntüyü kayıt defterinin adıyla etiketleyin ve arkasına görüntü adınızı ve etiketinizi ekleyin.

```bash
docker push <azure-container-registry-name>.azurecr.io/mydockerimage:v1.0.0
```

ACR depolarını listeleyerek gönderimin kayıt defterine başarılı bir şekilde kapsayıcı eklediğini doğrulayın. 

```azurecli-interactive
az acr repository list -n <azure-container-registry-name>
```

Kayıt defterindeki görüntüler listelenerek `mydockerimage` görüntüsünün kayıt defterinde olduğu onaylanır.

```json
[
  "mydockerimage"
]
```

### <a name="configure-web-app-to-use-the-image-from-azure-container-registry-or-any-private-registry"></a>Azure Container Registry'den (veya herhangi bir özel kayıt defterinden) görüntüyü kullanmak için Web App'i yapılandırma

Kapsayıcılar için Web App'i yapılandırarak Azure Container Registry'de depolanan bir kapsayıcıyı çalıştırmasını sağlayabilirsiniz. Azure Container Registry, aynı herhangi bir özel kayıt defteri gibi kullanılır, dolayısıyla kendi özel kayıt defterinizi kullanmanız gerekiyorsa çok benzer adımlarla bu görevi tamamlayabilirsiniz.

Cloud Shell'de [`az acr credential show`](/cli/azure/acr/credential?view=azure-cli-latest#az-acr-credential-show) komutunu çalıştırarak Azure Container Registry için kullanıcı adını ve parolayı görüntüleyin. Kullanıcı adını ve parolalardan birini kopyalayın; bunları sonraki adımda web uygulamasını yapılandırmak için kullanırsınız.

```bash
az acr credential show --name <azure-container-registry-name>
```

```json
{
  "passwords": [
    {
      "name": "password",
      "value": "password"
    },
    {
      "name": "password2",
      "value": "password2"
    }
  ],
  "username": "<registry-username>"
}
```

Cloud Shell'de [`az webapp config container set`](/cli/azure/webapp/config/container?view=azure-cli-latest#az-webapp-config-container-set) komutunu çalıştırarak web uygulamasına özel Docker görüntüsünü atayın. *\<app_name>*, *\<docker-registry-server-url>*, _\<registry-username>_ ve _\<password>_ değerlerini değiştirin. Azure Container Registry için *\<docker-registry-server-url>*, `https://<azure-container-registry-name>.azurecr.io` biçimindedir. Docker Hub'ın yanı sıra herhangi bir kayıt defteri kullanıyorsanız, görüntü adının kayıt defterinizin tam etki alanı adıyla (FQDN) başlaması gerekir. Azure Container Registry için, `<azure-container-registry>.azurecr.io/mydockerimage` benzeri bir değer olmalıdır. 

```azurecli-interactive
az webapp config container set --name <app_name> --resource-group myResourceGroup --docker-custom-image-name <azure-container-registry-name>.azurecr.io/mydockerimage --docker-registry-server-url https://<azure-container-registry-name>.azurecr.io --docker-registry-server-user <registry-username> --docker-registry-server-password <password>
```

> [!NOTE]
> *\<docker-registry-server-url>* değerinde `https://` gereklidir.
>
> [!NOTE]
> Kayıt defteri dockerhub, dışındaki kullanırken `docker-custom-image-name` kayıt defterinizin tam etki alanı adı (FQDN) içermelidir.  
> Azure Container Registry için, `<azure-container-registry>.azurecr.io/mydockerimage` benzeri bir değer olmalıdır.

Komut aşağıdaki JSON dizesine benzer, yapılandırma değişikliğinin başarılı olduğunu gösteren bir çıkış döndürür:

```json
[
  {
    "name": "DOCKER_CUSTOM_IMAGE_NAME",
    "slotSetting": false,
    "value": "mydockerimage"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_URL",
    "slotSetting": false,
    "value": "<azure-container-registry-name>.azurecr.io"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_USERNAME",
    "slotSetting": false,
    "value": "<registry-username>"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_PASSWORD",
    "slotSetting": false,
    "value": null
  }
]
```

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure'da Docker Python ve PostgreSQL web uygulaması oluşturma](tutorial-python-postgresql-app.md)