---
title: "Azure kapsayıcı hizmeti Öğreticisi - DC/OS yönetme"
description: "Azure kapsayıcı hizmeti Öğreticisi - DC/OS yönetme"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 07/17/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 0c58bd764cf0fdacd55675f8343c6e7481a11823
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
---
# <a name="azure-container-service-tutorial---manage-dcos"></a>Azure kapsayıcı hizmeti Öğreticisi - DC/OS yönetme

DC/OS çalışan modern ve kapsayıcılı uygulamaları için Dağıtılmış bir platform sağlar. Azure kapsayıcı hizmeti ile basit ve hızlı bir üretim hazır DC/OS kümesi sağlama. Bu hızlı başlangıç ayrıntıları için temel adımlar gerekli, DC/OS kümesi ve çalışma temel iş yükü dağıtın.

> [!div class="checklist"]
> * Bir ACS DC/OS kümesi oluşturma
> * Kümeye bağlanma
> * DC/OS CLI'yi yükleyin
> * Bir uygulamayı kümeye dağıtma
> * Kümede uygulama ölçeklendirme
> * DC/OS küme düğümleri ölçeklendirme
> * Temel DC/OS Yönetimi
> * DC/OS kümesi Sil

Bu öğretici, Azure CLI 2.0.4 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükseltme gerekiyorsa, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="create-dcos-cluster"></a>DC/OS kümesi oluşturma

İlk olarak, bir kaynak grubu ile oluşturmak [az grubu oluşturma](/cli/azure/group#create) komutu. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

Aşağıdaki örnek *westeurope* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli
az group create --name myResourceGroup --location westeurope
```

Ardından, DC/OS kümesi ile oluşturma [az acs oluşturmak](/cli/azure/acs#create) komutu.

Aşağıdaki örnek, adlandırılmış bir DC/OS kümesi oluşturur *myDCOSCluster* ve zaten mevcut değilse SSH anahtarları oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.  

```azurecli
az acs create \
  --orchestrator-type dcos \
  --resource-group myResourceGroup \
  --name myDCOSCluster \
  --generate-ssh-keys
```

Birkaç dakika sonra komut tamamlandıktan ve dağıtım hakkında bilgi döndürür.

## <a name="connect-to-dcos-cluster"></a>DC/OS kümesine bağlanma

DC/OS kümesi oluşturulduktan sonra bir SSH tüneli üzerinden erişimleri olabilir. DC/OS asıl ortak IP adresini döndürmek için aşağıdaki komutu çalıştırın. Bu IP adresi bir değişkende depolanan ve sonraki adımda kullanılır.

```azurecli
ip=$(az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-master')].[ipAddress]" -o tsv)
```

SSH tüneli oluşturmak için aşağıdaki komutu çalıştırın ve izleyin ekrandaki yönergeleri. Bağlantı noktası 80 kullanımda olduğundan komut başarısız olur. Tünel bağlantı noktası gibi bir içinde değil kullanımına güncelleştirme `85:localhost:80`. 

```azurecli
sudo ssh -i ~/.ssh/id_rsa -fNL 80:localhost:80 -p 2200 azureuser@$ip
```

## <a name="install-dcos-cli"></a>DC/OS CLI'yi yükleyin

DC/OS CLI kullanarak yükleme [az acs dcos yükleme-CLI](/azure/acs/dcos#install-cli) komutu. Azure CloudShell kullanıyorsanız, DC/OS CLI zaten yüklü. Azure CLI macOS ya da Linux üzerinde çalıştırıyorsanız, sudo komutu çalıştırmanız gerekebilir.

```azurecli
az acs dcos install-cli
```

CLI kümeyle kullanılabilmesi için önce SSH tüneli kullanacak şekilde yapılandırılmalıdır. Bunu yapmak için bağlantı noktası gerekiyorsa ayarlayarak aşağıdaki komutu çalıştırın.

```azurecli
dcos config set core.dcos_url http://localhost
```

## <a name="run-an-application"></a>Bir uygulamayı çalıştırın

Bir ACS DC/OS kümesi için mekanizma zamanlama Marathon varsayılandır. Marathon, uygulama başlatma ve DC/OS kümesinde uygulama durumunu yönetmek için kullanılır. Marathon aracılığıyla bir uygulama zamanlamak için adlı bir dosya oluşturun **marathon app.json**ve aşağıdaki içeriği buraya kopyalayın. 

```json
{
  "id": "demo-app-private",
  "cmd": null,
  "cpus": 1,
  "mem": 32,
  "disk": 0,
  "instances": 1,
  "container": {
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ]
    },
    "type": "DOCKER"
  }
}
```

Uygulamayı DC/OS kümesinde çalışacak şekilde zamanlamak için aşağıdaki komutu çalıştırın.

```azurecli
dcos marathon app add marathon-app.json
```

Uygulama dağıtımı durumunu görmek için aşağıdaki komutu çalıştırın.

```azurecli
dcos marathon app list
```

Zaman **görevleri** sütun değeri geçer *0/1* için *1/1*, uygulama dağıtımı tamamlandı.

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     0/1    ---       ---      False      DOCKER   None
```

## <a name="scale-marathon-application"></a>Marathon uygulamayı Ölçeklendir

Önceki örnekte, bir tek örnek uygulaması oluşturuldu. Uygulamanın üç örneğine kullanılabilir olacak şekilde, bu dağıtımı güncelleştirmek için açık **marathon app.json** dosya ve 3'e örnek özelliğini güncelleştirin.

```json
{
  "id": "demo-app-private",
  "cmd": null,
  "cpus": 1,
  "mem": 32,
  "disk": 0,
  "instances": 3,
  "container": {
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ]
    },
    "type": "DOCKER"
  }
}
```

Uygulamayı kullanarak güncelleştirme `dcos marathon app update` komutu.

```azurecli
dcos marathon app update demo-app-private < marathon-app.json
```

Uygulama dağıtımı durumunu görmek için aşağıdaki komutu çalıştırın.

```azurecli
dcos marathon app list
```

Zaman **görevleri** sütun değeri geçer *1/3* için *3/1*, uygulama dağıtımı tamamlandı.

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     1/3    ---       ---      False      DOCKER   None
```

## <a name="run-internet-accessible-app"></a>Internet erişilebilir uygulamayı çalıştırma

ACS DC/OS kümesi iki düğüm kümesi, internet üzerinde erişilebilir olan bir ortak ve internet'te erişilemeyen bir özel oluşur. Varsayılan, Son örnekte kullanılan özel düğümleri kümesidir.

Bir uygulama Internet üzerinden erişilebilir olması için ortak düğüm kümesi dağıtın. Bunu yapmak için vermek `acceptedResourceRoles` değerini nesne `slave_public`.

Adlı bir dosya oluşturun **nginx public.json** ve aşağıdaki içeriği buraya kopyalayın.

```json
{
  "id": "demo-app",
  "cmd": null,
  "cpus": 1,
  "mem": 32,
  "disk": 0,
  "instances": 1,
  "container": {
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "hostPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ]
    },
    "type": "DOCKER"
  },
  "acceptedResourceRoles": [
    "slave_public"
  ]
}
```

Uygulamayı DC/OS kümesinde çalışacak şekilde zamanlamak için aşağıdaki komutu çalıştırın.

```azurecli 
dcos marathon app add nginx-public.json
```

DC/OS ortak küme aracıların genel IP adresi alın.

```azurecli 
az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-agent')].[ipAddress]" -o tsv
```

Bu adrese gözatma varsayılan NGINX site döndürür.

![NGINX](./media/container-service-dcos-manage-tutorial/nginx.png)

## <a name="scale-dcos-cluster"></a>Ölçek DC/OS kümesi

Önceki örneklerde, bir uygulama için birden çok örneği ölçeklendirilmiş. DC/OS altyapı da fazla veya az işlem kapasitesini sağlamak için genişletilebilir. Bu gerçekleştirilir [az acs ölçeklendirme]() komutu. 

DC/OS aracıları geçerli sayısını görmek için [acs az göster](/cli/azure/acs#show) komutu.

```azurecli
az acs show --resource-group myResourceGroup --name myDCOSCluster --query "agentPoolProfiles[0].count"
```

5 sayısını artırmak için [az acs ölçeklendirme](/cli/azure/acs#scale) komutu. 

```azurecli
az acs scale --resource-group myResourceGroup --name myDCOSCluster --new-agent-count 5
```

## <a name="delete-dcos-cluster"></a>DC/OS kümesi Sil

Artık gerekli olduğunda, kullanabileceğiniz [az grubu Sil](/cli/azure/group#delete) kaynak grubu, DC/OS kümesi kaldırmak için komut ve ilişkili tüm kaynakları.

```azurecli 
az group delete --name myResourceGroup --no-wait
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdakiler de dahil olmak üzere temel DC/OS yönetim görevle ilgili öğrendiniz. 

> [!div class="checklist"]
> * Bir ACS DC/OS kümesi oluşturma
> * Kümeye bağlanma
> * DC/OS CLI'yi yükleyin
> * Bir uygulamayı kümeye dağıtma
> * Kümede uygulama ölçeklendirme
> * DC/OS küme düğümleri ölçeklendirme
> * DC/OS kümesi Sil

Yük Dengeleme DC/OS Azure ile ilgili uygulamada hakkında bilgi almak için sonraki öğretici ilerleyin. 

> [!div class="nextstepaction"]
> [Uygulamalarda yük dengeleme gerçekleştirme](container-service-load-balancing.md)