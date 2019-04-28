---
title: (KULLANIM DIŞI) Azure Container Service Öğreticisi - DC/OS yönetme
description: Azure Container Service öğreticisi - DC/OS Yönetme
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 02/26/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: fe943ae5ac7894cdd8d8e104615cea670513b7eb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61456668"
---
# <a name="deprecated-azure-container-service-tutorial---manage-dcos"></a>(KULLANIM DIŞI) Azure Container Service Öğreticisi - DC/OS yönetme

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

DC/OS, modern ve kapsayıcılı uygulamalar çalıştırmak için dağıtılmış bir platform sunar. Azure Container Service ile üretime hazır bir DC/OS kümesinin dağıtımı basit ve hızlıdır. Bu hızlı başlangıçta, DC/OS kümesi dağıtmak ve temel iş yükü çalıştırmak için gerekli temel adımlar ayrıntılı olarak açıklanır.

> [!div class="checklist"]
> * ACS DC/OS kümesi oluşturma
> * Kümeye bağlanma
> * DC/OS CLI’yi yükleme
> * Kümeye bir uygulama dağıtma
> * Küme üzerinde bir uygulama ölçeklendirme
> * DC/OS kümesi düğümlerini ölçeklendirme
> * Temel DC/OS yönetimi
> * DC/OS kümesini silme

Bu öğretici, Azure CLI 2.0.4 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükseltme gerekiyorsa, bkz. [Azure CLI yükleme]( /cli/azure/install-azure-cli). 

## <a name="create-dcos-cluster"></a>DC/OS kümesi oluşturma

Öncelikle, [az group create](/cli/azure/group#az-group-create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

Aşağıdaki örnek *westeurope* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli
az group create --name myResourceGroup --location westeurope
```

Sonra, [az acs create](/cli/azure/acs#az-acs-create) komutuyla bir DC/OS kümesi oluşturun.

Aşağıdaki örnekte *myDCOSCluster* adlı bir DC/OS kümesi ve henüz yoksa SSH anahtarları oluşturulur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.  

```azurecli
az acs create \
  --orchestrator-type dcos \
  --resource-group myResourceGroup \
  --name myDCOSCluster \
  --generate-ssh-keys
```

Birkaç dakika sonra komut tamamlanır ve dağıtım hakkında bilgi döndürür.

## <a name="connect-to-dcos-cluster"></a>DC/OS kümesine bağlanma

DC/OS kümesi oluşturulduktan sonra bir SSH tüneli üzerinden kümeye erişilebilir. DC/OS ana kümesinin genel IP adresini döndürmek için aşağıdaki komutu çalıştırın. Bu IP adresi bir değişkende depolanır ve bir sonraki adımda kullanılır.

```azurecli
ip=$(az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-master')].[ipAddress]" -o tsv)
```

SSH tüneli oluşturmak için aşağıdaki komutu çalıştırın ve ekrandaki yönergeleri izleyin. 80 bağlantı noktası zaten kullanımdaysa komut başarısız olur. Tünel oluşturulacak bağlantı noktasını kullanımda olmayan bir bağlantı noktası olarak (`85:localhost:80` gibi) güncelleştirin. 

```azurecli
sudo ssh -i ~/.ssh/id_rsa -fNL 80:localhost:80 -p 2200 azureuser@$ip
```

## <a name="install-dcos-cli"></a>DC/OS CLI’yı yükleyin

[az acs dcos install-cli](/cli/azure/acs/dcos#az-acs-dcos-install-cli) komutunu kullanarak DC/OS CLI’yı yükleyin. Azure CloudShell kullanıyorsanız DC/OS CLI zaten yüklüdür. Azure CLI’yı macOS ya da Linux’ta çalıştırıyorsanız komutu sudo ile çalıştırmanız gerekebilir.

```azurecli
az acs dcos install-cli
```

CLI’nın kümeyle kullanılabilmesi için önce SSH tünelini kullanacak şekilde yapılandırılması gerekir. Bunu yapmak için, gerekirse bağlantı noktasını değiştirerek aşağıdaki komutu çalıştırın.

```azurecli
dcos config set core.dcos_url http://localhost
```

## <a name="run-an-application"></a>Bir uygulamayı çalıştırma

Bir ACS DC/OS kümesi için varsayılan zamanlama mekanizması Marathon’dur. DC/OS kümesinde bir uygulamayı başlatmak ve uygulamanın durumunu yönetmek için Marathon kullanılır. Marathon aracılığıyla uygulama zamanlamak için **marathon-app.json** adlı bir dosya oluşturun ve aşağıdaki içeriği buna kopyalayın. 

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

Uygulamanın dağıtım durumunu görmek için aşağıdaki komutu çalıştırın.

```azurecli
dcos marathon app list
```

**TASKS** sütunundaki *0/1* değeri *1/1* olarak değiştiğinde uygulama dağıtımı tamamlanmış olur.

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     0/1    ---       ---      False      DOCKER   None
```

## <a name="scale-marathon-application"></a>Marathon uygulamasını ölçeklendirme

Önceki örnekte, bir tek örnek uygulaması oluşturuldu. Bu dağıtımı, uygulamanın üç örneği kullanılabilir olacak şekilde güncelleştirmek için, **marathon-app.json** dosyasını açın ve örnek özelliğini 3’e güncelleştirin.

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

`dcos marathon app update` komutunu kullanarak uygulamayı güncelleştirin.

```azurecli
dcos marathon app update demo-app-private < marathon-app.json
```

Uygulamanın dağıtım durumunu görmek için aşağıdaki komutu çalıştırın.

```azurecli
dcos marathon app list
```

**TASKS** sütunundaki *1/3* değeri *3/1* olarak değiştiğinde uygulama dağıtımı tamamlanmış olur.

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     1/3    ---       ---      False      DOCKER   None
```

## <a name="run-internet-accessible-app"></a>İnternetten erişilebilir uygulamayı çalıştırma

ACS DC/OS kümesi, internet üzerinden erişilebilen bir genel ve internet üzerinden erişilemeyen bir özel küme olmak üzere, iki düğüm kümesinden oluşur. Varsayılan küme, son örnekte kullanılan özel düğümlerdir.

Bir uygulamayı internet üzerinden erişilebilir hale getirmek için, genel düğüm kümesine dağıtın. Bunu yapmak için, `acceptedResourceRoles` nesnesine `slave_public` değerini verin.

**nginx-public.json** adlı bir dosya oluşturun ve şu içerikleri dosyaya kopyalayın.

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

DC/OS genel kümesi aracılarının genel IP adresini alın.

```azurecli 
az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-agent')].[ipAddress]" -o tsv
```

Bu adrese göz atıldığında varsayılan NGINX sitesi döndürülür.

![NGINX](./media/container-service-dcos-manage-tutorial/nginx.png)

## <a name="scale-dcos-cluster"></a>DC/OS kümesini ölçeklendirme

Önceki örneklerde, bir uygulama birden çok örneğe ölçeklendirilmişti. DC/OS altyapısı ayrıca daha fazla veya daha az işlem kapasitesi sağlayacak şekilde ölçeklendirilebilir. Bu, [az acs scale](/cli/azure/acs#az-acs-scale) komutuyla sağlanır. 

DC/OS aracılarının geçerli sayısını görmek için [acs az show](/cli/azure/acs#az-acs-show) komutunu kullanın.

```azurecli
az acs show --resource-group myResourceGroup --name myDCOSCluster --query "agentPoolProfiles[0].count"
```

Sayıyı 5’e artırmak için [az acs scale](/cli/azure/acs#az-acs-scale) komutunu kullanın. 

```azurecli
az acs scale --resource-group myResourceGroup --name myDCOSCluster --new-agent-count 5
```

## <a name="delete-dcos-cluster"></a>DC/OS kümesini silme

Artık gerekli değilse, [az group delete](/cli/azure/group#az-group-delete) komutunu kullanarak kaynak grubunu, DC/OS kümesini ve tüm ilgili kaynakları kaldırabilirsiniz.

```azurecli 
az group delete --name myResourceGroup --no-wait
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdakiler de dahil olmak üzere temel DC/OS yönetimi görevini öğrendiniz. 

> [!div class="checklist"]
> * ACS DC/OS kümesi oluşturma
> * Kümeye bağlanma
> * DC/OS CLI’yi yükleme
> * Kümeye bir uygulama dağıtma
> * Küme üzerinde bir uygulama ölçeklendirme
> * DC/OS kümesi düğümlerini ölçeklendirme
> * DC/OS kümesini silme

Azure üzerinde DC/OS’de yük dengeleme uygulaması hakkında bilgi edinmek için sonraki öğreticiye ilerleyin. 

> [!div class="nextstepaction"]
> [Uygulamalarda yük dengeleme gerçekleştirme](container-service-load-balancing.md)
