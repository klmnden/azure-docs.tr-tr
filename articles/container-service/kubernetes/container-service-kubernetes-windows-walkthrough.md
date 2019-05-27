---
title: (KULLANIM DIŞI) Hızlı Başlangıç - Windows için Azure Kubernetes kümesi
description: Azure CLI ile Azure Container Service'te Windows kapsayıcıları için Kubernetes kümesi oluşturmayı hızlı bir şekilde öğrenin.
services: container-service
author: dlepow
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/18/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017, mvc, devcenter
ms.openlocfilehash: d7ce702bb726fb89780d251f31023c9490112c36
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66148808"
---
# <a name="deprecated-deploy-kubernetes-cluster-for-windows-containers"></a>(KULLANIM DIŞI) Windows kapsayıcıları için Kubernetes kümesi dağıtma

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

Azure CLI, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu kılavuzda, [Azure Container Service](../container-service-intro.md)'te [Kubernetes](https://kubernetes.io/docs/home/) kümesi dağıtmak için Azure CLI'yi nasıl kullanacağınız ayrıntılı olarak açıklanmaktadır. Küme dağıtıldıktan sonra, Kubernetes `kubectl` komut satırı aracı ile kümeye bağlanır ve ilk Windows kapsayıcınızı dağıtırsınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli). 

> [!NOTE]
> Azure Container Service'te Kubernetes için Windows kapsayıcıları desteği önizleme aşamasındadır. 
>

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturun

[az group create](/cli/azure/group#az-group-create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği mantıksal bir gruptur. 

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-kubernetes-cluster"></a>Kubernetes kümesi oluşturma
Azure Container Service'te [az acs create](/cli/azure/acs#az-acs-create) komutuyla Kubernetes kümesi oluşturun. 

Aşağıdaki örnekte, bir Linux ana düğümü ve iki Windows aracı düğümüyle *myK8sCluster* adlı bir küme oluşturulmuştur. Bu örnekte, Linux ana düğümüne bağlanmak için gereken SSH anahtarları oluşturulmuştur. Bu örnekte, yönetici kullanıcı adı olarak *azureuser*, Windows düğümlerindeki parola olarak ise *myPassword12* kullanılmıştır. Bu değerleri ortamınız için uygun olan bir değerle güncelleştirin. 



```azurecli-interactive 
az acs create --orchestrator-type=kubernetes \
    --resource-group myResourceGroup \
    --name=myK8sCluster \
    --agent-count=2 \
    --generate-ssh-keys \
    --windows --admin-username azureuser \
    --admin-password myPassword12
```

Birkaç dakika sonra komut tamamlanır ve size dağıtımınız hakkındaki bilgiler gösterilir.

## <a name="install-kubectl"></a>Kubectl yükleyin

İstemci bilgisayarınızdan Kubernetes kümesine bağlanmak için Kubernetes’in komut satırı istemcisini ([`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/)) kullanın. 

Azure CloudShell'i kullanıyorsanız `kubectl` zaten yüklüdür. Yerel olarak yüklemek istiyorsanız [az acs kubernetes install-cli](/cli/azure/acs/kubernetes) komutunu kullanabilirsiniz.

Aşağıdaki Azure CLI örneğinde `kubectl`, sisteminize yüklenir. Windows'da bu komutu yönetici olarak çalıştırın.

```azurecli-interactive 
az acs kubernetes install-cli
```


## <a name="connect-with-kubectl"></a>kubectl ile bağlanma

`kubectl` öğesini Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes) komutunu çalıştırın. Aşağıdaki örnekte, Kubernetes kümeniz için küme yapılandırması indirilmiştir.

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8sCluster
```

Makinenizden küme bağlantısını doğrulamak için şunu çalıştırmayı deneyin:

```azurecli-interactive
kubectl get nodes
```

`kubectl`, ana ve aracı düğümleri listeler.

```azurecli-interactive
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.5.3
k8s-agent-98dc3136-1    Ready                      5m        v1.5.3
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.5.3

```

## <a name="deploy-a-windows-iis-container"></a>Windows IIS kapsayıcısı dağıtma

Bir veya daha fazla kapsayıcı içeren bir Kubernetes *pod*'unun içinde Docker kapsayıcısı çalıştırabilirsiniz. 

Bu temel örnekte Microsoft Internet Information Server (IIS) kapsayıcısı belirtmek için bir JSON dosyası kullanılmış ve ardından `kubctl apply` komutu kullanılarak pod oluşturulmuştur. 

`iis.json` adlı bir yerel dosya oluşturun ve aşağıdaki metni kopyalayın. Bu dosya Kubernetes'e, Windows Server 2016 Nano Server üzerinde, [Docker Hub](https://hub.docker.com/r/microsoft/iis/)'daki genel bir görüntüyü kullanarak IIS çalıştırmasını söyler. Kapsayıcı 80 numaralı bağlantı noktasını kullanır, ancak başlangıçta yalnızca küme ağından erişim sağlanabilir.

 ```JSON
 {
  "apiVersion": "v1",
  "kind": "Pod",
  "metadata": {
    "name": "iis",
    "labels": {
      "name": "iis"
    }
  },
  "spec": {
    "containers": [
      {
        "name": "iis",
        "image": "microsoft/iis:nanoserver",
        "ports": [
          {
          "containerPort": 80
          }
        ]
      }
    ],
    "nodeSelector": {
     "beta.kubernetes.io/os": "windows"
     }
   }
 }
 ```

Pod'u başlatmak için şunları yazın:
  
```azurecli-interactive
kubectl apply -f iis.json
```  

Dağıtımı izlemek için şunları yazın:
  
```azurecli-interactive
kubectl get pods
```

Pod dağıtılırken durum şudur: `ContainerCreating`. Kapsayıcının `Running` durumuna geçmesi birkaç dakika sürebilir.

```azurecli-interactive
NAME     READY        STATUS        RESTARTS    AGE
iis      1/1          Running       0           32s
```

## <a name="view-the-iis-welcome-page"></a>IIS karşılama sayfasını görüntüleme

Pod'u genel bir IP adresiyle herkesin kullanımına sunmak için aşağıdaki komutu yazın:

```azurecli-interactive
kubectl expose pods iis --port=80 --type=LoadBalancer
```

Bu komutla Kubernetes, hizmet ve hizmet için genel bir IP adresi ile Azure load balancer kuralı oluşturur. 

Hizmetin durumunu görmek için aşağıdaki komutu çalıştırın.

```azurecli-interactive
kubectl get svc
```

IP adresi başlangıçta `pending` olarak görünür. Birkaç dakika sonra, `iis` pod'unun dış IP adresi şu şekilde ayarlanır:
  
```azurecli-interactive
NAME         CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE       
kubernetes   10.0.0.1       <none>          443/TCP        21h       
iis          10.0.111.25    13.64.158.233   80/TCP         22m
```

Dış IP adresinde varsayılan IIS karşılama sayfasını görmek için istediğiniz bir web tarayıcısını kullanabilirsiniz:

![IIS’e göz atma görüntüsü](./media/container-service-kubernetes-windows-walkthrough/kubernetes-iis.png)  


## <a name="delete-cluster"></a>Kümeyi silme
Kümeye artık ihtiyacınız yoksa [az group delete](/cli/azure/group#az-group-delete) komutunu kullanarak kaynak grubunu, kapsayıcı hizmetini ve ilgili tüm kaynakları kaldırabilirsiniz.

```azurecli-interactive 
az group delete --name myResourceGroup
```


## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, `kubectl` bağlantılı bir Kubernetes kümesi ve IIS kapsayıcısı ile birlikte bir pod dağıttınız. Azure Container Service hakkında daha fazla bilgi edinmek için Kubernetes öğreticisine geçin.

> [!div class="nextstepaction"]
> [ACS Kubernetes kümesini yönetme](container-service-tutorial-kubernetes-prepare-app.md)
