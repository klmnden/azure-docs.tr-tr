---
title: "Bir Azure DC/OS kümesi ile ACR kullanarak | Microsoft Docs"
description: "Azure kapsayıcı hizmeti DC/OS kümesinde ile bir Azure kapsayıcı kayıt defteri kullanma"
services: container-service
documentationcenter: 
author: julienstroheker
manager: dcaro
editor: 
tags: acs, azure-container-service, acr, azure-container-registry
keywords: "Docker, kapsayıcıları, mikro hizmetler, Mesos, Azure, dosya paylaşımı, CIFS"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/23/2017
ms.author: juliens
ms.custom: mvc
ms.openlocfilehash: 36e57bb6ebf9f55d42c526a361fed33b4238b313
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="use-acr-with-a-dcos-cluster-to-deploy-your-application"></a>ACR DC/OS kümesi ile uygulamanızı dağıtmak için kullanın.

Bu makalede, Azure kapsayıcı kayıt defteri ile DC/OS kümesi kullanma keşfedin. ACR kullanarak özel olarak depolamak ve kapsayıcı görüntüleri yönetmenize olanak sağlar. Bu öğretici, aşağıdaki görevleri içerir:

> [!div class="checklist"]
> * Azure kapsayıcı kayıt defteri (gerekirse) dağıtın
> * DC/OS kümesinde ACR kimlik doğrulamasını yapılandırma
> * Azure kapsayıcı kayıt defterine görüntüyü karşıya
> * Azure kapsayıcı kayıt defterinden bir kapsayıcı görüntüsü çalıştırın

Bu öğreticide adımları tamamlamak için bir ACS DC/OS kümesi gerekir. Gerekirse, [bu komut dosyası örneği](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) sizin için bir tane oluşturabilirsiniz.

Bu öğretici, Azure CLI 2.0.4 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükseltme gerekiyorsa, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="deploy-azure-container-registry"></a>Azure kapsayıcı kayıt defteri dağıtın

Gerekirse, Azure kapsayıcı kayıt defteri ile oluşturma [az acr oluşturmak](/cli/azure/acr#create) komutu. 

Aşağıdaki örnek, bir kayıt defteri ile oluşturur bir rastgele adı oluşturmak. Kayıt defteri aynı zamanda bir yönetici hesabı kullanarak ile yapılandırılmış olan `--admin-enabled` bağımsız değişkeni.

```azurecli-interactive
az acr create --resource-group myResourceGroup --name myContainerRegistry$RANDOM --sku Basic
```

Kayıt defteri oluşturulduktan sonra Azure CLI aşağıdakine benzer veri çıkarır. Not edin `name` ve `loginServer`, bunlar daha sonraki adımlarda kullanılır.

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

Kapsayıcı kullanarak kayıt defteri kimlik bilgilerini almak [az acr kimlik bilgisi Göster](/cli/azure/acr/credential) komutu. Yedek `--name` son adımda not ettiğiniz bir. Not bir parola bir sonraki adımda gereklidir.

```azurecli-interactive
az acr credential show --name myContainerRegistry23489
```

Azure kapsayıcı kayıt defteri hakkında daha fazla bilgi için bkz: [özel Docker kapsayıcısı kayıt defterleri giriş](../../container-registry/container-registry-intro.md). 

## <a name="manage-acr-authentication"></a>ACR kimlik doğrulamasını yönetmek

Geleneksel anında iletme ve çekme görüntüsüne özel bir kayıt defteri önce kayıt defteri ile kimlik doğrulaması yoludur. Bunu yapmak için kullanacağınız `docker login` özel kayıt defteri erişmesi gereken herhangi bir istemci üzerinde komutu. DC/OS kümesi her biri ile ACR kimliğinizin doğrulanması gerekiyor, birçok düğümleri içerebileceğinden her düğüm üzerinde bu işlemi otomatikleştirmek yararlıdır. 

### <a name="create-shared-storage"></a>Paylaşılan depolama alanı oluşturma

Bu işlem, kümedeki her düğümde takıldıktan bir Azure dosya paylaşımı kullanır. Paylaşılan depolama alanı zaten ayarlamadıysanız bkz [bir DC/OS küme içindeki bir dosya paylaşımı Kurulum](container-service-dcos-fileshare.md).

### <a name="configure-acr-authentication"></a>ACR kimlik doğrulamasını yapılandırma

İlk olarak, DC/OS asıl FQDN'sini almak ve bir değişkende saklayın.

```azurecli-interactive
FQDN=$(az acs list --resource-group myResourceGroup --query "[0].masterProfile.fqdn" --output tsv)
```

Bir SSH bağlantısı asıl (veya ilk ana) DC/OS tabanlı kümenizin ile oluşturun. Varsayılan olmayan bir değer kümesi oluştururken kullanılan kullanıcı adını güncelleştirin.

```azurecli-interactive
ssh azureuser@$FQDN
```

Azure kapsayıcı kayıt oturum açmak için aşağıdaki komutu çalıştırın. Değiştir `--username` kapsayıcı kayıt defteri adını ve `--password` girilen parolalar biriyle. Son bağımsız değişken Değiştir *mycontainerregistry.azurecr.io* loginServer adla örnekte kapsayıcı kayıt defteri. 

Bu komut kimlik doğrulaması değerleri yerel olarak altında depolayan `~/.docker` yolu.

```azurecli-interactive
docker -H tcp://localhost:2375 login --username=myContainerRegistry23489 --password=//=ls++q/m+w+pQDb/xCi0OhD=2c/hST mycontainerregistry.azurecr.io
```

Kapsayıcı kayıt defteri kimlik doğrulama değerleri içeren sıkıştırılmış bir dosya oluşturun.

```azurecli-interactive
tar czf docker.tar.gz .docker
```

Bu dosya, Küme Paylaşılan depolama alanına kopyalayın. Bu adım, dosyayı DC/OS kümenin tüm düğümlerinde kullanılabilir hale getirir.

```azurecli-interactive
cp docker.tar.gz /mnt/share/dcosshare
```

## <a name="upload-image-to-acr"></a>Görüntü için ACR karşıya yükle

Şimdi bir geliştirme makine ya da herhangi bir yüklü Docker sistemiyle, bir görüntü oluşturun ve Azure kapsayıcı kayıt defterine yükleyin.

Ubuntu görüntüsünden bir kapsayıcı oluşturun.

```azurecli-interactive
docker run ubuntu --name base-image
```

Şimdi kapsayıcının yeni bir görüntüsünü yakalayın. Görüntü adı içermesi gerekir `loginServer` biçimlerinin bir kapsayıcı registrywith adını `loginServer/imageName`.

```azurecli-interactive
docker -H tcp://localhost:2375 commit base-image mycontainerregistry30678.azurecr.io/dcos-demo
````

Azure kapsayıcı kayıt defterine oturum açın. Adı loginServer adıyla--kullanıcıadı kapsayıcı kayıt defteri adını değiştirin ve girilen parolalar biriyle parola.

```azurecli-interactive
docker login --username=myContainerRegistry23489 --password=//=ls++q/m+w+pQDb/xCi0OhD=2c/hST mycontainerregistry2675.azurecr.io
```

Son olarak, görüntünün ACR kayıt defterine karşıya yükleyin. Bu örnek dcos demo adlı bir resim yükler.

```azurecli-interactive
docker push mycontainerregistry30678.azurecr.io/dcos-demo
```

## <a name="run-an-image-from-acr"></a>Bir görüntü ACR çalıştırın

Bir görüntü ACR kayıt defterinden kullanmak için bir dosya adları oluşturun *acrDemo.json* ve aşağıdaki metni buraya kopyalayın. Görüntü adı kapsayıcı kayıt defteri loginServer adı ve görüntü adı ile örnek yerine `loginServer/imageName`. Not edin `uris` özelliği. Bu özellik, bu durumda DC/OS kümedeki her düğümde takılı Azure dosya paylaşımının olduğundan kapsayıcı kayıt defteri kimlik doğrulaması dosyasının konumu tutar.

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

Bu öğreticide sahip yapılandırdığınız Azure kapsayıcı aşağıdaki görevler de dahil olmak üzere kayıt defterini kullanmak için DC/OS:

> [!div class="checklist"]
> * Azure kapsayıcı kayıt defteri (gerekirse) dağıtın
> * DC/OS kümesinde ACR kimlik doğrulamasını yapılandırma
> * Azure kapsayıcı kayıt defterine görüntüyü karşıya
> * Azure kapsayıcı kayıt defterinden bir kapsayıcı görüntüsü çalıştırın
