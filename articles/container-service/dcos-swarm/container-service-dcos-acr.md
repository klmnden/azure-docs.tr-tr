---
title: (KULLANIM DIŞI) ACR bir Azure DC/OS kümesi ile kullanma
description: Azure Container Service’te DC/OS kümesi ile Azure Container Registry kullanma
services: container-service
author: julienstroheker
manager: dcaro
ms.service: container-service
ms.topic: tutorial
ms.date: 03/23/2017
ms.author: juliens
ms.custom: mvc
ms.openlocfilehash: 8319f2f5405271679d0c11d4ac68492cdec8fc14
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66148924"
---
# <a name="deprecated-use-acr-with-a-dcos-cluster-to-deploy-your-application"></a>(KULLANIM DIŞI) ACR, uygulamanızı dağıtmak için bir DC/OS kümesi ile kullanma

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Bu makalede, Azure Container Registry’yi bir DC/OS kümesi ile kullanma işlemi araştırılmaktadır. ACR kullanarak, kapsayıcı görüntülerinizi özel olarak depolayıp yönetebilirsiniz. Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Azure Container Registry’yi dağıtma (gerekirse)
> * Bir DC/OS kümesinde ACR kimlik doğrulamasını yapılandırma
> * Azure Container Registry’ye görüntü yükleme
> * Azure Container Registry’den görüntü çalıştırma

Bu öğreticideki adımları tamamlamak için bir ACS DC/OS kümesi gerekir. Gerekirse, [bu betik örneği](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) sizin için bir tane oluşturabilir.

Bu öğretici, Azure CLI 2.0.4 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükseltme gerekiyorsa, bkz. [Azure CLI yükleme]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="deploy-azure-container-registry"></a>Azure Container Registry’yi dağıtma

Gerekirse, [az acr create](/cli/azure/acr#az-acr-create) komutuyla Azure Container Registry oluşturun. 

Aşağıdaki örnek, rastgele oluşturulmuş bir ad ile kayıt defteri oluşturur. Kayıt defteri aynı zamanda `--admin-enabled` bağımsız değişkeni kullanılarak bir yönetici hesabı ile yapılandırılır.

```azurecli-interactive
az acr create --resource-group myResourceGroup --name myContainerRegistry$RANDOM --sku Basic
```

Kayıt defteri oluşturulduktan sonra, Azure CLI aşağıdakine benzer veriler çıkarır. `name` ve `loginServer` değerlerini not edin; bunlar daha sonraki adımlarda kullanılacaktır.

```azurecli
{
  "adminUserEnabled": false,
  "creationDate": "2017-06-06T03:40:56.511597+00:00",
  "id": "/subscriptions/f2799821-a08a-434e-9128-454ec4348b10/resourcegroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry23489",
  "location": "eastus",
  "loginServer": "mycontainerregistry23489.azurecr.io",
  "name": "myContainerRegistry23489",
  "provisioningState": "Succeeded",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "mycontainerregistr034017"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

[az acr credential show](/cli/azure/acr/credential) komutunu kullanarak kapsayıcı kayıt defteri kimlik bilgilerini alın. `--name` değerini son adımda not ettiğiniz değerle değiştirin. Bir parolayı not alın, sonraki bir adımda gerekli olacaktır.

```azurecli-interactive
az acr credential show --name myContainerRegistry23489
```

Azure Container Registry hakkında daha fazla bilgi için bkz. [Özel Docker kapsayıcı kayıt defterlerine giriş](../../container-registry/container-registry-intro.md). 

## <a name="manage-acr-authentication"></a>ACR kimlik doğrulamasını yönetme

Özel bir kayıt defterinden görüntü gönderme ve almanın geleneksel yolu, ilk olarak kayıt defterinde kimlik doğrulaması yapmaktır. Bunu yapmak için, özel kayıt defterine erişmesi gereken her istemci üzerinde `docker login` komutunu kullanın. Bir DC/OS kümesinde kimliği ACR ile doğrulanması gereken çok sayıda düğüm olabildiği için, bu işlemi her bir düğümde otomatik hale getirmek yararlıdır. 

### <a name="create-shared-storage"></a>Paylaşılan depolama oluşturma

Bu işlem, kümedeki her bir düğümde bağlanmış bir Azure dosya paylaşımı kullanır. Paylaşılan depolamayı henüz ayarlamadıysanız bkz. [Bir DC/OS kümesi içinde dosya paylaşımı ayarlama](container-service-dcos-fileshare.md).

### <a name="configure-acr-authentication"></a>ACR kimlik doğrulamasını yapılandırma

İlk olarak, DC/OS ana şablonunun FQDN'sini alın ve bir değişkende depolayın.

```azurecli-interactive
FQDN=$(az acs list --resource-group myResourceGroup --query "[0].masterProfile.fqdn" --output tsv)
```

DC/OS tabanlı kümenizin ana şablonuyla (veya ilk ana şablonuyla) bir SSH bağlantısı oluşturun. Küme oluşturulurken varsayılan olmayan bir değeri kullanıldıysa kullanıcı adını güncelleştirin.

```azurecli-interactive
ssh azureuser@$FQDN
```

Azure Container Registry’de oturum açmak için aşağıdaki komutu çalıştırın. `--username` değerini kapsayıcı kayıt defterinin adı ile, `--password` değerini ise belirtilen parolalardan biri ile değiştirin. Örnekteki son *mycontainerregistry.azurecr.io* bağımsız değişkenini kapsayıcı kayıt defterinin loginServer adıyla değiştirin. 

Bu komut, kimlik doğrulama değerlerini `~/.docker` yolu altında yerel olarak depolar.

```azurecli-interactive
docker -H tcp://localhost:2375 login --username=myContainerRegistry23489 --password=//=ls++q/m+w+pQDb/xCi0OhD=2c/hST mycontainerregistry.azurecr.io
```

Kapsayıcı kayıt defteri kimlik doğrulama değerlerini içeren sıkıştırılmış bir dosya oluşturun.

```azurecli-interactive
tar czf docker.tar.gz .docker
```

Bu dosyayı küme paylaşılan depolama alanına kopyalayın. Bu adım, dosyayı DC/OS kümesinin tüm düğümlerinde kullanılabilir hale getirir.

```azurecli-interactive
cp docker.tar.gz /mnt/share/dcosshare
```

## <a name="upload-image-to-acr"></a>Görüntüyü ACR’ye yükleme

Şimdi bir geliştirme makinesinden ya da Docker’ın yüklü olduğu herhangi bir sistemden görüntü oluşturun ve Azure Container Registry’ye yükleyin.

Ubuntu görüntüsünden bir kapsayıcı oluşturun.

```azurecli-interactive
docker run ubuntu --name base-image
```

Şimdi kapsayıcıyı yeni bir görüntüye yakalayın. Görüntü adı, kapsayıcı kayıt defterinin `loginServer` adını `loginServer/imageName` biçiminde içermelidir.

```azurecli-interactive
docker -H tcp://localhost:2375 commit base-image mycontainerregistry30678.azurecr.io/dcos-demo
```

Azure Container Registry’de oturum açın. Adı loginServer adıyla, --username değerini kapsayıcı kayıt defterinin adıyla ve --password değerini sağlanan parolalardan biriyle değiştirin.

```azurecli-interactive
docker login --username=myContainerRegistry23489 --password=//=ls++q/m+w+pQDb/xCi0OhD=2c/hST mycontainerregistry2675.azurecr.io
```

Son olarak, görüntüyü ACR kayıt defterine yükleyin. Bu örnek dcos-demo adlı bir görüntüyü karşıya yükler.

```azurecli-interactive
docker push mycontainerregistry30678.azurecr.io/dcos-demo
```

## <a name="run-an-image-from-acr"></a>ACR’den görüntü çalıştırma

ACR kayıt defterinden bir görüntüyü kullanmak için *acrDemo.json* adlı bir dosya oluşturun ve içine aşağıdaki metni kopyalayın. Görüntü adını, kapsayıcı kayıt defterinin loginServer adı ve görüntü adı ile değiştirin; örneğin `loginServer/imageName`. `uris` özelliğini not edin. Bu özellik, bu örnekte DC/OS kümesindeki her bir düğüme bağlanan Azure dosya paylaşımı olan kapsayıcı kayıt defteri kimlik doğrulama dosyasının konumunu içerir.

```json
{
  "id": "myapp",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "mycontainerregistry30678.azurecr.io/dcos-demo",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "hostPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "uris":  [
       "file:///mnt/share/dcosshare/docker.tar.gz"
   ]
}
```

DC/OC CLI ile uygulamayı dağıtın.

```azurecli-interactive
dcos marathon app add acrDemo.json
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki görevlerle birlikte DC/OS’yi Azure Container Registry kullanacak şekilde yapılandırdınız:

> [!div class="checklist"]
> * Azure Container Registry’yi dağıtma (gerekirse)
> * Bir DC/OS kümesinde ACR kimlik doğrulamasını yapılandırma
> * Azure Container Registry’ye görüntü yükleme
> * Azure Container Registry’den görüntü çalıştırma
