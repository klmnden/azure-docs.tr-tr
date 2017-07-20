---
title: "Hızlı Başlangıç - Linux için Azure Kubernetes kümesi | Microsoft Docs"
description: "Azure CLI ile Azure Container Service'de Linux kapsayıcıları için Kubernetes kümesi oluşturmayı hızlı bir şekilde öğrenin."
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 8da267e8-2aeb-4c24-9a7a-65bdca3a82d6
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 26c07d30f9166e0e52cb396cdd0576530939e442
ms.openlocfilehash: 3be2079d205d6bfd4c796e5f6abcd7ac5fe595a2
ms.contentlocale: tr-tr
ms.lasthandoff: 07/19/2017

---

# <a name="deploy-kubernetes-cluster-for-linux-containers"></a>Linux kapsayıcıları için Kubernetes kümesi dağıtma

Azure CLI, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu kılavuz, [Azure Container Service](container-service-intro.md)’te [Kubernetes](https://kubernetes.io/docs/home/) kümesi dağıtmak için Azure CLI’nın kullanımını ayrıntılı olarak açıklar. Küme dağıtıldıktan sonra, Kubernetes `kubectl` komut satırı aracı ile kümeye bağlanır ve ilk Linux kapsayıcınızı dağıtırsınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği mantıksal bir gruptur. 

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-kubernetes-cluster"></a>Kubernetes kümesi oluşturma
Azure Container Service’de [az acs create](/cli/azure/acs#create) komutuyla Kubernetes kümesi oluşturun. 

Aşağıdaki örnekte, bir Linux ana düğümü ve iki Linux aracı düğümüyle *myK8sCluster* adlı bir küme oluşturulmuştur. Bu örnekte, varsayılan konumlarda olmayan SSH anahtarları oluşturulmuştur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın. Küme adını, ortamınız için uygun olan bir adla güncelleştirin. 



```azurecli-interactive 
az acs create --orchestrator-type=kubernetes \
    --resource-group myResourceGroup \
    --name=myK8sCluster \
    --generate-ssh-keys 
```

Birkaç dakika sonra komut tamamlanır ve size dağıtımınız hakkındaki bilgileri gösterir.

## <a name="install-kubectl"></a>Kubectl yükleyin

İstemci bilgisayarınızdan Kubernetes kümesine bağlanmak için Kubernetes’in komut satırı istemcisini ([`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/)) kullanın. 

Azure CloudShell'i kullanıyorsanız `kubectl` zaten yüklüdür. Yerel olarak yüklemek istiyorsanız [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) komutunu kullanabilirsiniz.

Aşağıdaki Azure CLI örneğinde, `kubectl` sisteminize yüklenir. Azure CLI’yı macOS ya da Linux’ta çalıştırıyorsanız komutu `sudo` ile çalıştırmanız gerekebilir.

```azurecli-interactive 
az acs kubernetes install-cli 
```

## <a name="connect-with-kubectl"></a>kubectl ile bağlanma

`kubectl` öğesini Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials) komutunu çalıştırın. Aşağıdaki örnekte, Kubernetes kümeniz için küme yapılandırması indirilmiştir.

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


## <a name="deploy-an-nginx-container"></a>NGINX kapsayıcısı dağıtma

Bir veya daha fazla kapsayıcı içeren bir Kubernetes *pod*’unun içinde Docker kapsayıcısı çalıştırabilirsiniz. 

Aşağıdaki komut, düğümlerin birindeki Kubernetes pod’u içinde NGINX Docker kapsayıcısını başlatır. Bu durumda, kapsayıcı [Docker Hub](https://hub.docker.com/_/nginx/)’daki bir görüntüden alınan NGINX web sunucusunu çalıştırır.

```azurecli-interactive
kubectl run nginx --image nginx
```
Kapsayıcının çalıştığını görmek için şunu çalıştırın:

```azurecli-interactive
kubectl get pods
```

## <a name="view-the-nginx-welcome-page"></a>NGINX karşılama sayfasını görüntüleme
NGINX sunucusunu genel bir IP adresiyle kullanıma sunmak için aşağıdaki komutu yazın:

```azurecli-interactive
kubectl expose deployments nginx --port=80 --type=LoadBalancer
```

Bu komutla Kubernetes, hizmet için genel bir IP adresiyle birlikte bir hizmet ve [Azure yük dengeleyici kuralı](container-service-kubernetes-load-balancing.md) oluşturur. 

Hizmetin durumunu görmek için aşağıdaki komutu çalıştırın.

```azurecli-interactive
kubectl get svc
```

Başlangıçta IP adresi `pending` olarak görünür. Birkaç dakika sonra, hizmetin dış IP adresi ayarlanır:
  
```azurecli-interactive
NAME         CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE       
kubernetes   10.0.0.1       <none>          443/TCP        21h       
nginx        10.0.111.25    52.179.3.96     80/TCP         22m
```

Dış IP adresinde varsayılan NGINX karşılama sayfasını görmek için istediğiniz bir web tarayıcısını kullanabilirsiniz:

![Nginx’e göz atma görüntüsü](media/container-service-kubernetes-walkthrough/kubernetes-nginx4.png)  


## <a name="delete-cluster"></a>Küme silme
Kümeye artık ihtiyacınız yoksa [az group delete](/cli/azure/group#delete) komutunu kullanarak kaynak grubunu, kapsayıcı hizmetini ve ilgili tüm kaynakları kaldırabilirsiniz.

```azurecli-interactive 
az group delete --name myResourceGroup
```


## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, `kubectl` ile bağlı bir Kubernetes kümesi ve NGINX kapsayıcısı ile birlikte bir pod dağıttınız. Azure Container Service hakkında daha fazla bilgi için Kubernetes kümesi öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [ACS Kubernetes kümesini yönetme](./container-service-tutorial-kubernetes-prepare-app.md)

