---
title: "Kapsayıcılar - Azure için Web uygulaması için özel bir Docker görüntü kullanın | Microsoft Docs"
description: "Kapsayıcıları için Web uygulaması için özel bir Docker görüntü kullanma"
keywords: "Azure uygulama hizmeti, web uygulaması, linux, docker, kapsayıcı"
services: app-service
documentationcenter: 
author: SyntaxC4
manager: SyntaxC4
editor: 
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/24/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 2580c2109ce33b1ce99aa491f7d0002edf060693
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="use-a-custom-docker-image-for-web-app-for-containers"></a>Kapsayıcıları için Web uygulaması için özel bir Docker görüntü kullanın

[Web uygulaması kapsayıcıları için](app-service-linux-intro.md) PHP 7.0 ve Node.js 4.5 gibi belirli sürümler için yerleşik Docker görüntüsü Linux'ta desteğiyle sağlar. Web uygulaması kapsayıcıları için bir hizmet olarak platform olarak yerleşik görüntüler ve özel resimler barındırmak için Docker kapsayıcısı teknolojisini kullanır. Bu öğreticide, özel bir Docker yansıması oluştur ve kapsayıcıları için Web uygulamasına dağıtma hakkında bilgi edinin. Bu desen yerleşik resimleri dilinizi tercih içerme veya uygulamanızın yerleşik görüntüler içinde sağlanmayan özel bir yapılandırma gerektirir yararlıdır.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* [Git](https://git-scm.com/downloads)
* Etkin bir [Azure aboneliği](https://azure.microsoft.com/pricing/free-trial/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Docker](https://docs.docker.com/get-started/#setup)
* A [Docker hub'a hesabı](https://docs.docker.com/docker-id/)

[!INCLUDE [Free trial note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Örneği indirme

Bir terminal penceresi örnek uygulama depoyu yerel makinenize kopyalayın ve ardından örnek kodunu içeren dizini değiştirmek için aşağıdaki komutu çalıştırın.

```bash
git clone https://github.com/Azure-Samples/docker-django-webapp-linux.git --config core.autocrlf=input
cd docker-django-webapp-linux
```

## <a name="build-the-image-from-the-docker-file"></a>Docker dosyasından yansıması oluştur

Git deposunda bir göz atalım _Dockerfile_. Bu dosya, uygulamanızı çalıştırmak için gereken Python ortamı tanımlar. Ayrıca, görüntünün ayarlayan bir [SSH](https://www.ssh.com/ssh/protocol/) kapsayıcı ve ana bilgisayar arasında güvenli iletişim için sunucu.

```docker
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

Docker görüntü oluşturmak için çalıştırın `docker build` komut ve bir ad sağlayın `mydockerimage`ve etiket, `v1.0.0`. Değiştir `<docker-id>` Docker Hub'ınıza hesap kimliği.

```bash
docker build --tag <docker-id>/mydockerimage:v1.0.0 .
```

Komutu, aşağıdakine benzer bir çıktı üretir:

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

Yapı Docker kapsayıcısı çalıştırarak çalışıp çalışmadığını sınayın. Sorun [çalıştırmak docker](https://docs.docker.com/engine/reference/commandline/run/) komut ve adını ve etiket resminin ona geçirin. Bağlantı noktası kullanılarak belirttiğinizden emin olun `-p` bağımsız değişkeni.

```bash
docker run -p 2222:8000 <docker-ID>/mydockerimage:v1.0.0
```

Web uygulaması ve kapsayıcı düzgün göz atarak doğrulayın `http://localhost:2222`.

![Yerel olarak test web uygulaması](./media/app-service-linux-using-custom-docker-image/app-service-linux-browse-local.png)

## <a name="push-the-docker-image-to-docker-hub"></a>Docker görüntü Docker hub'a anında iletme

Bir kayıt defteri görüntüleri barındıran ve Hizmetleri görüntü ve kapsayıcı hizmetleri sağlayan bir uygulamadır. Görüntünüzü paylaşmak için bir kayıt defterine göndermelisiniz. 

<!-- Depending on your requirements, you may have your docker images in a Public Docker Registry, such as Docker Hub, or a Private Docker Registry such as Azure Container Registry. Select the appropriate tab for your scenario below (your selection will switch multiple tabs on this page). -->

> [!NOTE]
> Bir özel Docker kayıt defterine Ftp'den? İsteğe bağlı yönergeler için bkz: [Docker görüntü için özel kayıt defteri anında](#push-a-docker-image-to-private-registry-optional).

<!--## [Docker Hub](#tab/docker-hub)-->

Docker hub'a genel veya özel kendi depoları barındırmak izin veren bir kayıt defteri Docker görüntüleri için ' dir. Ortak Docker hub'a özel Docker görüntü göndermek için kullanmanız [docker itme](https://docs.docker.com/engine/reference/commandline/push/) komut ve tam görüntü adı ve etiketi belirtin. Tam görüntü adı ve etiket aşağıdaki örnek gibi görünür:

```
<docker-id>/image-name:tag
```

Docker Hub'ına oturum henüz bunu kullanarak yapılandırmazsanız [docker oturum açma](https://docs.docker.com/engine/reference/commandline/login/) Görüntü göndermeyi denemeden önce komutu.

```bash
docker login --username <docker-id> --password <docker-hub-password>
```

"Oturum açma başarılı oldu" iletisi, oturum açtığınız onaylar. Oturum açtıktan sonra Docker hub'ı kullanarak görüntü gönderebilir [docker itme](https://docs.docker.com/engine/reference/commandline/push/) komutu.

```bash
docker push <docker-id>/mydockerimage:v1.0.0
```

Komut inceleyerek başarılı itme çıkışını olduğunu doğrulayın.

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

## <a name="deploy-app-to-azure"></a>Azure için uygulama dağıtma

Azure Web Apps kullanarak yerel Linux uygulamalarını bulutta barındırabilir. Kapsayıcıları için bir Web uygulaması oluşturmak için bir grubu ardından bir hizmet planına ve son olarak web uygulaması oluşturma Azure CLI komutları çalıştırmanız gerekir. 

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-linux-app-service-plan"></a>Linux uygulama hizmeti planı oluştur

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)] 

### <a name="create-a-web-app"></a>Web uygulaması oluşturma

Cloud Shell’de, [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) komutuyla `myAppServicePlan` App Service planında bir [web uygulaması](app-service-linux-intro.md) oluşturun. Değiştirmeyi unutmayın `<app_name>` bir benzersiz uygulama adı ve < docker-kimliği > kimliğinizi Docker ile

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --deployment-container-image-name <docker-ID>/mydockerimage:v1.0.0
```

Web uygulaması oluşturduğunuzda Azure CLI çıktı aşağıdaki örneğe benzer şekilde gösterir:

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

### <a name="configure-environment-variables"></a>Ortam değişkenleri yapılandırın

Çoğu Docker resimler yapılandırılması gereken ortam değişkenleri vardır. Başka bir kullanıcı tarafından oluşturulmuş var olan bir Docker görüntüsünü kullanıyorsanız, görüntünün 80 dışında bir bağlantı noktası kullanabilirsiniz. Azure kullanarak görüntünüzü kullandığı bağlantı noktası hakkında size `WEBSITES_PORT` uygulama ayarı. İçin GitHub sayfası [Python örneği bu öğreticideki](https://github.com/Azure-Samples/docker-django-webapp-linux) ayarlamak için gereken gösterir `WEBSITES_PORT` için _8000_.

Uygulama ayarlarını belirlemek için kullanın [az webapp config appsettings kümesi](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) bulut Kabuğu'nda komutu. Uygulama ayarları, büyük küçük harfe duyarlı ve boşlukla ayrılmış.

```azurecli-interactive
az webapp config appsettings set --resource-group myResourceGroup --name <app_name> --settings WEBSITES_PORT=8000
```

<!-- Depending on your requirements, you may have your docker images in a Public Docker Registry, such as Docker Hub, or a Private Docker Registry, such as Azure Container Registry. Select the appropriate tab for your scenario below: -->

> [!NOTE]
> Özel Docker kayıt defterinden dağıtma? İsteğe bağlı yönergeler için bkz: [Docker kapsayıcısı özel bir kayıt defterinden kullanmak için Web uygulaması yapılandırma](#configure-web-app-to-use-docker-container-from-a-private-registry-optional).

<!-- # [Docker Hub](#tab/docker-hub)-->

<!-- # [Private Registry](#tab/private-registry)

// Place Private Registry text back here once Tabbed Conceptual bug is fixed

---
-->

### <a name="test-the-web-app"></a>Web uygulaması sınama

Web uygulaması göz atarak çalıştığını doğrulayın (`http://<app_name>azurewebsites.net`). 

![Test web uygulama bağlantı noktası yapılandırması](./media/app-service-linux-using-custom-docker-image/app-service-linux-browse-azure.png)

## <a name="change-web-app-and-redeploy"></a>Değişiklik web uygulaması ve yeniden dağıtın

Yerel Git deponuzu içinde app/templates/app/index.html açın. İlk HTML öğesini bulun ve şekilde değiştirin.

```python
<nav class="navbar navbar-inverse navbar-fixed-top">
    <div class="container">
      <div class="navbar-header">         
        <a class="navbar-brand" href="#">Azure App Service - Updated Here!</a>       
      </div>            
    </div>
  </nav> 
```

Python dosyası değiştirildiğinde ve kaydettiyseniz sonra yeniden oluşturun ve yeni Docker görüntü gönderme. Daha sonra değişikliklerin etkili olması web uygulaması başlatın. Bu öğreticide daha önce kullandığınız aynı komutları kullanın. Başvurabilirsiniz [Docker dosya görüntüden yapı](#build-the-image-from-the-docker-file) ve [Docker görüntü Docker hub'a anında](#push-the-docker-image-to-docker-hub). Web uygulaması'ndaki yönergeleri izleyerek test [web uygulamasını Test](#test-the-web-app).

## <a name="connect-to-web-app-for-containers-using-ssh"></a>SSH kullanarak kapsayıcıları için Web uygulamasına bağlanmak

SSH bir kapsayıcı ve istemci arasında güvenli iletişim sağlar. SSH desteklemek özel bir Docker görüntü için sırayla bir Dockerfile oluşturmalısınız. SSH Docker dosyasındaki etkinleştirin. Kendi özel görüntünüzü ile bu yönergeleri takip edebilirsiniz şekilde SSH yönergeleri zaten örnek dockerfile için eklenmiştir:

* A [çalıştırmak](https://docs.docker.com/engine/reference/builder/#run) çağırır yönerge `apt-get`, kök hesabının parolasını ayarlar `"Docker!"`.

    ```docker
    ENV SSH_PASSWD "root:Docker!"
    RUN apt-get update \
            && apt-get install -y --no-install-recommends dialog \
            && apt-get update \
      && apt-get install -y --no-install-recommends openssh-server \
      && echo "$SSH_PASSWD" | chpasswd 
    ```

    > [!NOTE]
    > Bu yapılandırma, kapsayıcıya dış bağlantılara izin vermiyor. SSH yalnızca Kudu/SCM sitesi üzerinden kullanılabilir. Kudu/SCM site yayımlama kimlik bilgileri ile doğrulanır.

* A [kopya](https://docs.docker.com/engine/reference/builder/#copy) kopyalamak için Docker altyapısına bildirir yönerge [sshd_config](http://man.openbsd.org/sshd_config) dosya */vb./ssh/* dizin. Yapılandırma dosyanızı dayanmalıdır [bu sshd_config dosya](https://github.com/Azure-App-Service/node/blob/master/6.11.1/sshd_config).

    ```docker
    COPY sshd_config /etc/ssh/
    ```

    > [!NOTE]
    > *Sshd_config* dosyası, aşağıdaki öğeleri içermelidir: 
    > * `Ciphers`Bu listedeki en az bir öğe içermelidir: `aes128-cbc,3des-cbc,aes256-cbc`.
    > * `MACs`Bu listedeki en az bir öğe içermelidir: `hmac-sha1,hmac-sha1-96`.

* Bir [kullanıma](https://docs.docker.com/engine/reference/builder/#expose) çıkarır kapsayıcısında 2222 bağlantı yönerge. Kök parolasını bilinen karşın, bağlantı noktası 2222 internet'ten erişilemez. Yalnızca özel bir sanal ağ köprüsü ağının kapsayıcılara tarafından erişilebilir bağlantı bir iç noktasıdır. Bundan sonra komutları SSH yapılandırma ayrıntılarını kopyalayın ve başlangıç `ssh` hizmet.

    ```docker
    EXPOSE 8000 2222
    ```

* Emin olun [Başlat ssh hizmeti ](https://github.com/Azure-App-Service/node/blob/master/6.9.3/startup/init_container.sh) /bin dizininde bir kabuk betiği kullanarak.
 
    ```bash
    #!/bin/bash
    service ssh start
    ```
     
### <a name="open-ssh-connection-to-container"></a>SSH bağlantı kapsayıcı Aç

Web uygulaması kapsayıcıları için kapsayıcısı dış bağlantılara izin vermiyor. Yalnızca en erişilebilen Kudu sitesi üzerinden SSH kullanılabilir `https://<app_name>.scm.azurewebsites.net`.

Bağlanmak için Gözat `https://<app_name>.scm.azurewebsites.net/webssh/host` ve Azure hesabınızla oturum açın.

Ardından, etkileşimli konsol görüntüleme bir sayfaya yönlendirilir. 

Bazı uygulamalar kapsayıcısında çalıştığını doğrulamak isteyebilir. Kapsayıcı inceleyin ve çalışan işlemleri doğrulamak için sorunu `top` komut isteminde.

```bash
top
```

`top` Komutu bir kapsayıcıdaki tüm çalışan işlemleri gösterir.

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

Tebrikler! Kapsayıcıları için bir Web uygulaması için özel bir Docker görüntü yapılandırdığınız.

## <a name="use-a-private-image-from-docker-hub-optional"></a>Docker hub'a (isteğe bağlı) özel bir görüntüden kullanın

İçinde [bir web uygulaması oluşturma](#create-a-web-app), Docker Hub görüntüyü belirtilen `az webapp create` komutu. Bu genel bir görüntü için yeterince iyi olur. Özel bir görüntü kullanmak için Docker hesap Kimliğini ve parolayı kullanarak Azure web uygulamanızda yapılandırmanız gerekir.

Bulut Kabuğu'nda izleyin `az webapp create` komutunu [az webapp config kapsayıcı kümesi](/cli/azure/webapp/config/container?view=azure-cli-latest#az_webapp_config_container_set). Değiştir  *\<app_name >*hem de _< docker-kimliği >_ ve  _<password>_  Docker kimliği ve parolası.

```azurecli-interactive
az webapp config container set --name <app_name> --resource-group myResourceGroup --docker-registry-server-user <docker-id> --docker-registry-server-password <password>
```

Komut yapılandırma değişikliği başarılı olduğunu gösteren aşağıdaki JSON dizeye benzer bir çıktı ortaya çıkarır:

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

## <a name="use-a-docker-image-from-any-private-registry-optional"></a>Docker görüntü herhangi özel kayıt defterinden (isteğe bağlı) kullanın

Bu bölümde, Docker görüntü kapsayıcıları için özel bir kayıt defteri Web uygulamasında kullanmayı öğrenin ve Azure kapsayıcı kayıt defteri örnek olarak kullanır. Diğer özel kayıt defterleri kullanmaya yönelik adımlar benzerdir. 

Azure kapsayıcı kayıt defteri bir yönetilen Docker azure'dan özel görüntüleri barındırmak için bir hizmettir. Dağıtımlar, herhangi bir türden olabilir dahil olmak üzere [Docker Swarm](https://docs.docker.com/engine/swarm/), [Kubernetes](https://kubernetes.io/)ve kapsayıcıları için Web uygulaması. 

### <a name="create-an-azure-container-registry"></a>Azure Container Registry oluşturma

Bulut Kabuğu'nda kullanmak [az acr oluşturmak](/cli/azure/acr?view=azure-cli-latest#az_acr_create) Azure kapsayıcı kayıt oluşturmak üzere komutu. Adı, kaynak grubu, geçirmek ve `Basic` SKU. Kullanılabilir SKU'lar `Classic`, `Basic`, `Standard`, ve `Premium`.

```azurecli-interactive
az acr create --name <azure-container-registry-name> --resource-group myResourceGroup --sku Basic --admin-enabled true
```

Bir kapsayıcı oluşturma şu çıkışı üretir:

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

### <a name="log-in-to-azure-container-registry"></a>Azure kapsayıcı kayıt defterine oturum açın

Kayıt defterine görüntü göndermek için kayıt anında kabul edecek biçimde kimlik bilgilerini sağlamanız gerekir. Bu kimlik bilgilerini kullanarak alabilirsiniz [az acr Göster](/cli/azure/acr?view=azure-cli-latest#az_acr_show) bulut Kabuğu'nda komutu. 

```azurecli-interactive
az acr credential show --name <azure-container-registry-name>
```

Komut kullanıcı adıyla kullanılabilir iki parola ortaya çıkarır.

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

Azure kapsayıcı kayıt defterini kullanarak, yerel terminal penceresinden oturum açtığınız `docker login` komutu. Sunucu adı, oturum açmak için gereklidir. Biçimini kullanın `{azure-container-registry-name>.azurecr.io`.

```bash
docker login <azure-container-registry-name>.azurecr.io --username <registry-username> --password <password> 
```

Oturum açma başarılı olduğunu onaylayın. 

### <a name="push-an-image-to-azure-container-registry"></a>Görüntüyü Azure kapsayıcı kayıt defterine bildirme

> [!NOTE]
> Kendi görüntünüzü kullanıyorsanız, aşağıdaki gibi görüntü etiketi:
> ```bash
> docker tag <azure-container-registry-name>.azurecr.io/mydockerimage
> ```

Görüntü kullanarak anında iletme `docker push` komutu. Etiket ve görüntü adı ve ardından kayıt defteri ada sahip bir görüntü etiketi.

```bash
docker push <azure-container-registry-name>.azurecr.io/mydockerimage:v1.0.0
```

Zorlama başarıyla bir kapsayıcı kayıt defterine ACR depoları listeleyerek eklediğinizi doğrulayın. 

```azurecli-interactive
az acr repository list -n <azure-container-registry-name>
```

Onaylar kayıt defterinde görüntüleri listeleme `mydockerimage` kayıt defteri.

```json
[
  "mydockerimage"
]
```

### <a name="configure-web-app-to-use-the-image-from-azure-container-registry-or-any-private-registry"></a>Azure kapsayıcı kayıt defteri (veya özel bir kayıt defteri) görüntüden kullanmak için Web uygulamasını yapılandırma

Azure kapsayıcı kayıt defterinde depolanan bir kapsayıcı çalışan Web uygulaması kapsayıcıları için yapılandırabilirsiniz. Azure kapsayıcı kayıt defterini kullanarak özel kayıt defteri, bu görevi tamamlamak için gereken adımları kullanmak, kendi gerekirse; bu nedenle benzer yalnızca özel bir kayıt defteri gibi kullanmaktır.

Bulut Kabuğu'nda çalıştırmak [az acr kimlik bilgisi Göster](/cli/azure/acr/credential?view=azure-cli-latest#az_acr_credential_show) kullanıcı adı ve parola Azure kapsayıcı kayıt defteri görüntülenecek. Sonraki adımda web uygulaması yapılandırmak üzere kullanabilmeniz için kullanıcı adı ve parolaları birini kopyalayın.

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

Bulut Kabuğu'nda çalıştırmak [az webapp config kapsayıcı kümesi](/cli/azure/webapp/config/container?view=azure-cli-latest#az_webapp_config_container_set) özel Docker görüntü web uygulaması'na atamak için komutu. Değiştir  *\<app_name >*,  *\<docker-kayıt defteri-server-url >*,  _\<kayıt defteri-username >_ve  _\<parola >_. Azure kapsayıcı kayıt defteri,  *\<docker-kayıt defteri-server-url >* biçimde `https://<azure-container-registry-name>.azurecr.io`. 

```azurecli-interactive
az webapp config container set --name <app_name> --resource-group myResourceGroup --docker-custom-image-name mydockerimage --docker-registry-server-url https://<azure-container-registry-name>.azurecr.io --docker-registry-server-user <registry-username> --docker-registry-server-password <password>
```

> [!NOTE]
> `https://`gereklidir  *\<docker-kayıt defteri-server-url >*.
>

Komut yapılandırma değişikliği başarılı olduğunu gösteren aşağıdaki JSON dizeye benzer bir çıktı ortaya çıkarır:

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
> [Azure'da Docker Python ve PostgreSQL bir web uygulaması oluşturma](tutorial-docker-python-postgresql-app.md)
