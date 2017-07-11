---
title: "Hızlı Başlangıç - Linux için Azure Kubernetes kümesi | Microsoft Docs"
description: "Azure CLI ile Azure Container Service'de Linux kapsayıcıları için Kubernetes kümesi oluşturmayı hızlı bir şekilde öğrenin."
services: container-service
documentationcenter: 
author: anhowe
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
ms.date: 05/31/2017
ms.author: anhowe
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 7948c99b7b60d77a927743c7869d74147634ddbf
ms.openlocfilehash: 25043f6bf5e5ab3def8563bd2c096b79706bfec1
ms.contentlocale: tr-tr
ms.lasthandoff: 06/20/2017

---

<a id="deploy-kubernetes-cluster-for-linux-containers" class="xliff"></a>

# Linux kapsayıcıları için Kubernetes kümesi dağıtma

Azure CLI, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu kılavuz, [Azure Container Service](container-service-intro.md)’te [Kubernetes](https://kubernetes.io/docs/home/) kümesi dağıtmak için Azure CLI’nın kullanımını ayrıntılı olarak açıklar. Küme dağıtıldıktan sonra, Kubernetes `kubectl` komut satırı aracı ile kümeye bağlanır ve ilk Linux kapsayıcınızı dağıtırsınız.

Bu öğretici, Azure CLI 2.0.4 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükseltme gerekiyorsa, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

<a id="log-in-to-azure" class="xliff"></a>

## Azure'da oturum açma 

[az login](/cli/azure/#login) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```azurecli-interactive
az login
```

<a id="create-a-resource-group" class="xliff"></a>

## Kaynak grubu oluşturma

[az group create](/cli/azure/group#create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği mantıksal gruptur. 

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

<a id="create-kubernetes-cluster" class="xliff"></a>

## Kubernetes kümesi oluşturma
Azure Container Service’de [az acs create](/cli/azure/acs#create) komutuyla Kubernetes kümesi oluşturun. 

Aşağıdaki örnekte, bir Linux ana düğümü ve iki Linux aracı düğümüyle *myK8sCluster* adlı bir küme oluşturulmuştur. Bu örnekte, varsayılan konumlarda olmayan SSH anahtarları oluşturulmuştur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın. Küme adını, ortamınız için uygun olan bir adla güncelleştirin. 



```azurecli-interactive 
az acs create --orchestrator-type=kubernetes \
    --resource-group myResourceGroup \
    --name=myK8sCluster \
    --generate-ssh-keys 
```

Birkaç dakika sonra komut tamamlanır ve size dağıtımınız hakkındaki bilgileri gösterir.

<a id="install-kubectl" class="xliff"></a>

## Kubectl yükleyin

İstemci bilgisayarınızdan Kubernetes kümesine bağlanmak için Kubernetes’in komut satırı istemcisini ([`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/)) kullanın. 

Azure CloudShell kullanıyorsanız `kubectl` zaten yüklenmiştir. Yerel olarak yüklemek istiyorsanız [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) komutunu kullanabilirsiniz.

Aşağıdaki Azure CLI örneğinde, `kubectl` sisteminize yüklenir. Azure CLI’yı macOS ya da Linux’ta çalıştırıyorsanız komutu `sudo` ile çalıştırmanız gerekebilir.

```azurecli-interactive 
az acs kubernetes install-cli 
```

<a id="connect-with-kubectl" class="xliff"></a>

## kubectl ile bağlanma

`kubectl` öğesini Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials) komutunu çalıştırın. Aşağıdaki örnekte, Kubernetes kümeniz için küme yapılandırması indirilmiştir.

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8sCluster
```

Makinenizden kümenizin bağlantısını doğrulamak için şunu çalıştırmayı deneyin:

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


<a id="deploy-an-nginx-container" class="xliff"></a>

## NGINX kapsayıcısı dağıtma

Bir veya daha fazla kapsayıcı içeren bir Kubernetes *pod*’unun içinde Docker kapsayıcısı çalıştırabilirsiniz. 

Aşağıdaki komut, düğümlerin birindeki Kubernetes pod’u içinde NGINX Docker kapsayıcısını başlatır. Bu durumda, kapsayıcı [Docker Hub](https://hub.docker.com/_/nginx/)’daki bir görüntüden alınan NGINX web sunucusunu çalıştırır.

```azurecli-interactive
kubectl run nginx --image nginx
```
Kapsayıcının çalıştığını görmek için şunu çalıştırın:

```azurecli-interactive
kubectl get pods
```

<a id="view-the-nginx-welcome-page" class="xliff"></a>

## NGINX karşılama sayfasını görüntüleme
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


<a id="delete-cluster" class="xliff"></a>

## Küme silme
Kümeye artık ihtiyacınız yoksa [az group delete](/cli/azure/group#delete) komutunu kullanarak kaynak grubunu, kapsayıcı hizmetini ve ilgili tüm kaynakları kaldırabilirsiniz.

```azurecli-interactive 
az group delete --name myResourceGroup
```


<a id="next-steps" class="xliff"></a>

## Sonraki adımlar

Bu hızlı başlangıçta, `kubectl` ile bağlı bir Kubernetes kümesi ve NGINX kapsayıcısı ile birlikte bir pod dağıttınız. Azure Container Service hakkında daha fazla bilgi için Kubernetes kümesi öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [ACS Kubernetes kümesini yönetme](./container-service-tutorial-kubernetes-prepare-app.md)

