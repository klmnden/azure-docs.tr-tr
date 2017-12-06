---
title: "Azure kapsayıcı hizmeti Öğreticisi - küme dağıtma"
description: "Azure kapsayıcı hizmeti Öğreticisi - küme dağıtma"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c91eea3734820239187bcf7b497fb06d7fd5f7ef
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
---
# <a name="deploy-a-kubernetes-cluster-in-azure-container-service"></a>Azure kapsayıcı Hizmeti'nde bir Kubernetes kümesini dağıtma

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

Kubernetes, kapsayıcılı uygulamalar için dağıtılmış bir platform sunar. Azure kapsayıcı hizmeti ile basit ve hızlı bir üretim hazır Kubernetes kümenin sağlama. Bu öğreticide, 7, 3 parçası Azure kapsayıcı hizmeti Kubernetes küme dağıtılır. Tamamlanan adımları içerir:

> [!div class="checklist"]
> * Kubernetes ACS kümesini dağıtma
> * Kubernetes CLI (kubectl) yükleme
> * Kubectl yapılandırması

Sonraki öğreticilerde, Azure oy uygulama olduğundan kümeye dağıtılan, ölçeği, güncelleştirilmiş ve Operations Management Suite Kubernetes küme izlemek için yapılandırılır.

## <a name="before-you-begin"></a>Başlamadan önce

Önceki eğitimlerine bir kapsayıcı görüntüsü oluşturuldu ve Azure kapsayıcı kayıt defteri örneğine yüklenir. Bu adımları yapmadıysanız ve izlemek istediğiniz, geri dönüp [Öğreticisi 1 – Oluştur kapsayıcı görüntüleri](./container-service-tutorial-kubernetes-prepare-app.md).

## <a name="create-kubernetes-cluster"></a>Kubernetes kümesi oluşturma

Azure Container Service'te [az acs create](/cli/azure/acs#create) komutuyla Kubernetes kümesi oluşturun. 

Aşağıdaki örnek adlı bir küme oluşturur `myK8sCluster` adlı bir kaynak grubunda `myResourceGroup`. Bu kaynak grubunun oluşturulduğu [önceki Öğreticisi](./container-service-tutorial-kubernetes-prepare-acr.md).

```azurecli-interactive 
az acs create --orchestrator-type kubernetes --resource-group myResourceGroup --name myK8SCluster --generate-ssh-keys 
```

Sınırlı deneme sürümünde olduğu gibi bazı durumlarda, bir Azure aboneliğinin Azure kaynaklarına sınırlı erişimi olur. Dağıtım sınırlı kullanılabilir çekirdek sayısı nedeniyle başarısız olursa, `--agent-count 1` öğesini [az acs create](/cli/azure/acs#create) komutuna ekleyerek varsayılan aracı sayısını azaltın. 

Birkaç dakika sonra dağıtım tamamlar ve ACS dağıtımı hakkında bilgi döndürür json biçimli.

## <a name="install-the-kubectl-cli"></a>CLI kubectl yükleyin

İstemci bilgisayarından Kubernetes kümeye bağlanmak için [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), Kubernetes komut satırı istemcisi. 

Azure CloudShell kullanıyorsanız kubectl zaten yüklüdür. Yerel olarak yüklemek istiyorsanız, kullanmak [az acs kubernetes yükleme-CLI](/cli/azure/acs/kubernetes#install-cli) komutu.

Linux veya macOS çalıştırılıyorsa, sudo çalıştırmanız gerekebilir. Windows üzerinde Kabuk yönetici olarak çalıştır emin olun.

```azurecli-interactive 
az acs kubernetes install-cli 
```

Windows, varsayılan yüklemedir *c:\program files (x86)\kubectl.exe*. Bu dosyayı Windows yoluna eklemeniz gerekebilir. 

## <a name="connect-with-kubectl"></a>kubectl ile bağlanma

kubectl’i Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials) komutunu çalıştırın.

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group myResourceGroup --name myK8SCluster
```

Kümenize bağlantıyı doğrulamak için Çalıştır [kubectl alma düğümleri](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) komutu.

```azurecli-interactive
kubectl get nodes
```

Çıktı:

```bash
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.6.2
k8s-agent-98dc3136-1    Ready                      5m        v1.6.2
k8s-agent-98dc3136-2    Ready                      5m        v1.6.2
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.6.2
```

Eğitmen tamamlandığında, bir ACS Kubernetes küme iş yükleri için hazır olması. Sonraki öğreticilerde, bir çok kapsayıcı uygulama bu kümeye dağıtılan, çıkışı ölçeği, güncelleştirilmiş ve izlenen.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure kapsayıcı hizmeti Kubernetes küme dağıtıldı. Aşağıdaki adımlar tamamlandı:

> [!div class="checklist"]
> * Bir Kubernetes ACS kümesi dağıtılmış
> * Kubernetes CLI (kubectl) yüklü
> * Yapılandırılmış kubectl

Uygulama küme üzerinde çalışan hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Kubernetes uygulamasında dağıtma](./container-service-tutorial-kubernetes-deploy-application.md)
