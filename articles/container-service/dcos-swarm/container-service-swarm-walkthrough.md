---
title: "Hızlı Başlangıç - Linux için Azure Docker Swarm kümesi"
description: "Azure CLI ile Azure Container Service'de Linux kapsayıcıları için Docker Swarm kümesi oluşturmayı hızlı bir şekilde öğrenin."
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: get-started-article
ms.date: 08/14/2017
ms.author: nepeters
ms.custom: 
ms.openlocfilehash: 79e4fab9501141c78bca4b28eabb3964604be909
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
---
# <a name="deploy-docker-swarm-cluster"></a>Docker Swarm kümesi dağıtma

Bu hızlı başlangıçta, Azure CLI kullanılarak Docker Swarm kümesi dağıtılır. Ardından web ön ucu ve bir Redis örneğinden oluşan çok kapsayıcılı bir uygulama dağıtılıp küme üzerinde çalıştırılır. Tamamlandığında, uygulamaya İnternet üzerinden erişilebilir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği mantıksal bir gruptur.

Aşağıdaki örnek *westus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name myResourceGroup --location westus
```

Çıktı:

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup",
  "location": "westcentralus",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-docker-swarm-cluster"></a>Docker Swarm kümesi oluşturma

Azure Container Service'te [az acs create](/cli/azure/acs#create) komutuyla bir Docker Swarm kümesi oluşturun. 

Aşağıdaki örnekte, bir Linux ana düğümü ve üç Linux aracı düğümüyle *mySwarmCluster* adlı bir küme oluşturulur.

```azurecli-interactive
az acs create --name mySwarmCluster --orchestrator-type Swarm --resource-group myResourceGroup --generate-ssh-keys
```

Sınırlı deneme sürümünde olduğu gibi bazı durumlarda, bir Azure aboneliğinin Azure kaynaklarına sınırlı erişimi olur. Dağıtım sınırlı kullanılabilir çekirdek sayısı nedeniyle başarısız olursa, `--agent-count 1` öğesini [az acs create](/cli/azure/acs#create) komutuna ekleyerek varsayılan aracı sayısını azaltın. 

Birkaç dakika sonra komut tamamlanır ve küme hakkında json tarafından biçimlendirilmiş bilgiler gösterilir.

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma

Bu hızlı başlangıç boyunca hem Docker Swarm ana şablonunun hem de Docker aracı havuzunun IP adresine ihtiyacınız olur. He iki IP adresini de döndürmek için aşağıdaki komutu çalıştırın.


```bash
az network public-ip list --resource-group myResourceGroup --query "[*].{Name:name,IPAddress:ipAddress}" -o table
```

Çıktı:

```bash
Name                                                                 IPAddress
-------------------------------------------------------------------  -------------
swarmm-agent-ip-myswarmcluster-myresourcegroup-d5b9d4agent-66066781  52.179.23.131
swarmm-master-ip-myswarmcluster-myresourcegroup-d5b9d4mgmt-66066781  52.141.37.199
```

Swarm ana şablonuna yönelik bir SSH tüneli oluşturun. `IPAddress` değerini Swarm ana şablonunun IP adresiyle değiştirin.

```bash
ssh -p 2200 -fNL 2375:localhost:2375 azureuser@IPAddress
```

`DOCKER_HOST` ortam değişkenini ayarlayın. Bu, konağın adını belirtmenize gerek kalmadan Docker Swarm’a karşı docker komutları çalıştırmanıza imkan tanır.

```bash
export DOCKER_HOST=:2375
```

Şimdi Docker Swarm’da Docker hizmetlerini çalıştırmaya hazırsınız.


## <a name="run-the-application"></a>Uygulamayı çalıştırma

`docker-compose.yaml` adlı bir dosya oluşturun ve aşağıdaki içeriği dosyaya kopyalayın.

```yaml
version: '3'
services:
  azure-vote-back:
    image: redis
    container_name: azure-vote-back
    ports:
        - "6379:6379"

  azure-vote-front:
    image: microsoft/azure-vote-front:redis-v1
    container_name: azure-vote-front
    environment:
      REDIS: azure-vote-back
    ports:
        - "80:80"
```

Azure Vote hizmeti oluşturmak için aşağıdaki komutu çalıştırın.

```bash
docker-compose up -d
```

Çıktı:

```bash
Creating network "user_default" with the default driver
Pulling azure-vote-front (microsoft/azure-vote-front:redis-v1)...
swarm-agent-EE873B23000005: Pulling microsoft/azure-vote-front:redis-v1...
swarm-agent-EE873B23000004: Pulling microsoft/azure-vote-front:redis-v1... : downloaded
Pulling azure-vote-back (redis:latest)...
swarm-agent-EE873B23000004: Pulling redis:latest... : downloaded
Creating azure-vote-front ... 
Creating azure-vote-back ... 
Creating azure-vote-front
Creating azure-vote-back ...
```

## <a name="test-the-application"></a>Uygulamayı test etme

Azure Vote uygulamasını test etmek için Swarm aracı havuzunun IP adresine göz atın.

![Azure Vote’a göz atma görüntüsü](media/container-service-docker-swarm-mode-walkthrough/azure-vote.png)

## <a name="delete-cluster"></a>Kümeyi silme
Kümeye artık ihtiyacınız yoksa [az group delete](/cli/azure/group#delete) komutunu kullanarak kaynak grubunu, kapsayıcı hizmetini ve ilgili tüm kaynakları kaldırabilirsiniz.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="get-the-code"></a>Kodu alma

Bu hızlı başlangıçta, önceden oluşturulmuş kapsayıcı görüntüleri kullanılarak Docker hizmeti oluşturulur. İlgili uygulama kodu, Dockerfile ve Compose dosyası GitHub'da bulunur.

[https://github.com/Azure-Samples/azure-voting-app-redis](https://github.com/Azure-Samples/azure-voting-app-redis.git)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Docker Swarm kümesi dağıtıp ve bu kümeye çok kapsayıcılı bir uygulama dağıttınız.

Docker Swarm’u Visual Studio Team Services ile tümleştirme hakkında bilgi edinmek için Docker Swarm ve VSTS ile CI/CD’ye devam edin.

> [!div class="nextstepaction"]
> [Docker Swarm ve VSTS ile CI/CD](./container-service-docker-swarm-setup-ci-cd.md)