---
title: "Docker kapsayıcı kümesi dağıtma - Azure CLI"
description: "Bir Kubernetes, DC/OS veya Docker Swarm çözümünü, Azure CLI 2.0 kullanarak Azure Container Service’e dağıtın"
services: container-service
author: sauryadas
manager: timlt
ms.service: container-service
ms.topic: quickstart
ms.date: 03/01/2017
ms.author: saudas
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 40d5ea0e7abce165659219db8842ab64ac75fda7
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
---
# <a name="deploy-a-docker-container-hosting-solution-using-the-azure-cli-20"></a>Azure CLI 2.0 kullanarak Docker kapsayıcısı barındırma çözümü dağıtma

Azure Container Service’te küme oluşturmak ve yönetmek için Azure CLI 2.0 aracındaki `az acs` komutlarını kullanın. Bir Azure Container Service kümesini ayrıca [Azure portalı](container-service-deployment.md) veya Azure Container Service API’leri kullanarak dağıtabilirsiniz.

`az acs` komutları hakkında yardım için `-h` parametresini herhangi bir komuta geçirin. Örneğin: `az acs create -h`.



## <a name="prerequisites"></a>Ön koşullar
Azure CLI 2.0 ile bir Azure Container Service kümesi oluşturmak için şunlar gerekir:
* bir Azure hesabı ([ücretsiz deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/))
* [Azure CLI 2.0](/cli/azure/install-az-cli2) aracının yüklü ve ayarlanmış olması

## <a name="get-started"></a>Başlarken 
### <a name="log-in-to-your-account"></a>Hesabınızda oturum açın
```azurecli
az login 
```

Etkileşimli olarak oturum açmak için istemleri izleyin. Diğer oturum açma yöntemleri için bkz. [Azure CLI 2.0 ile çalışmaya başlama](/cli/azure/get-started-with-az-cli2).

### <a name="set-your-azure-subscription"></a>Azure aboneliğinizi ayarlama

Birden fazla Azure aboneliğiniz varsa varsayılan aboneliği ayarlayın. Örneğin:

```
az account set --subscription "f66xxxxx-xxxx-xxxx-xxx-zgxxxx33cha5"
```


### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Her küme için bir kaynak grubu oluşturmanız önerilir. Azure Container Service’in [kullanılabilir](https://azure.microsoft.com/en-us/regions/services/) olduğu bir Azure bölgesi belirtin. Örneğin:

```azurecli
az group create -n acsrg1 -l "westus"
```
Çıktı aşağıdakine benzer:

![Kaynak grubu oluşturma](./media/container-service-create-acs-cluster-cli/rg-create.png)


## <a name="create-an-azure-container-service-cluster"></a>Azure Container Service Kümesi oluşturma

Küme oluşturmak için `az acs create` seçeneğini kullanın.
Önceki adımda oluşturulan küme adı ve kaynak grubu adı zorunlu parametrelerdir. 

İlgili anahtarlar kullanılarak üzerine yazılmadıkça, diğer girişler varsayılan değerlere ayarlanır (aşağıdaki ekrana bakın). Örneğin, düzenleyici varsayılan olarak DC/OS’ye ayarlanır. Siz belirtmezseniz, küme adını temel alan bir DNS adı ön eki oluşturulur.

![az acs create usage](./media/container-service-create-acs-cluster-cli/create-help.png)


### <a name="quick-acs-create-using-defaults"></a>Varsayılan değerleri kullanarak hızlı `acs create`
Varsayılan konumda bir `id_rsa.pub` SSH RSA ortak anahtar dosyanız varsa (veya [OS X ve Linux](../../virtual-machines/linux/mac-create-ssh-keys.md) ya da [Windows](../../virtual-machines/linux/ssh-from-windows.md) için bir tane oluşturduysanız) aşağıdakine benzer bir komut kullanın:

```azurecli
az acs create -n acs-cluster -g acsrg1 -d applink789
```
SSH ortak anahtarınız yoksa, bu ikinci komutu kullanın. `--generate-ssh-keys` anahtarıyla bu komut sizin için bir anahtar oluşturur.

```azurecli
az acs create -n acs-cluster -g acsrg1 -d applink789 --generate-ssh-keys
```

Komutu girdikten sonra kümenin oluşturulması için yaklaşık 10 dakika bekleyin. Komut çıktısı, ana ve aracı düğümlerin tam etki alanı adlarını (FQDN) ve ilk ana düğüme bağlanmaya yönelik SSH komutunu içerir. Kısaltılmış çıktı şu şekildedir:

![ACS oluşturma görüntüsü](./media/container-service-create-acs-cluster-cli/cluster-create.png)

> [!TIP]
> [Kubernetes kılavuzu](../kubernetes/container-service-kubernetes-walkthrough.md), bir Kubernetes kümesi oluşturmak için varsayılan değerlerle `az acs create` kullanma işlemini gösterir.
>

## <a name="manage-acs-clusters"></a>ACS kümelerini yönetme

Kümenizi yönetmek için ek `az acs` komutlarını kullanın. Bazı örnekler aşağıda verilmiştir.

### <a name="list-clusters-under-a-subscription"></a>Bir abonelik altındaki kümeleri listeleme

```azurecli
az acs list --output table
```

### <a name="list-clusters-in-a-resource-group"></a>Bir kaynak grubundaki kümeleri listeleme

```azurecli
az acs list -g acsrg1 --output table
```

![acs list](./media/container-service-create-acs-cluster-cli/acs-list.png)


### <a name="display-details-of-a-container-service-cluster"></a>Bir kapsayıcı hizmeti kümesinin ayrıntılarını görüntüleme

```azurecli
az acs show -g acsrg1 -n acs-cluster --output list
```

![acs show](./media/container-service-create-acs-cluster-cli/acs-show.png)


### <a name="scale-the-cluster"></a>Kümeyi ölçeklendirme
Aracı düğümlerinde hem ölçek artırma hem de ölçek azaltma yapılabilir. `new-agent-count` parametresi, ACS kümesindeki yeni aracı sayısıdır.

```azurecli
az acs scale -g acsrg1 -n acs-cluster --new-agent-count 4
```

![acs scale](./media/container-service-create-acs-cluster-cli/acs-scale.png)

## <a name="delete-a-container-service-cluster"></a>Kapsayıcı hizmeti kümesini silme
```azurecli
az acs delete -g acsrg1 -n acs-cluster 
```
Bu komut, kapsayıcı hizmetini oluştururken oluşturulan tüm kaynakları (ağ ve depolama) silmez. Tüm kaynakları kolaylıkla silmek için her kümeyi ayrı bir kaynak grubuna dağıtmanız önerilir. Ardından, küme artık gerekli olmadığında kaynak grubunu silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Artık çalışan bir kümeniz olduğuna göre, bağlantı ve yönetim ayrıntıları için aşağıdaki belgelere bakın:

* [Azure Container Service kümesine bağlanma](../container-service-connect.md)
* [Azure Container Service ve DC/OS ile çalışma](container-service-mesos-marathon-rest.md)
* [Azure Container Service ve Docker Swarm ile çalışma](container-service-docker-swarm.md)
* [Azure Container Service ve Kubernetes ile çalışma](../kubernetes/container-service-kubernetes-walkthrough.md)