---
title: "Azure Container Service öğreticisi - Küme Dağıtma"
description: "Azure Container Service öğreticisi - Küme Dağıtma"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 6ef789bc017e670566d25dd9d167698515e88349
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="deploy-a-kubernetes-cluster-in-azure-container-service"></a>Azure Container Service’te bir Kubernetes kümesi dağıtma

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

Kubernetes, kapsayıcılı uygulamalar için dağıtılmış bir platform sunar. Azure Container Service ile üretime hazır bir Kubernetes kümesinin sağlanması basit ve hızlıdır. 7 parçalık bu öğreticinin 3. kısmında, bir Azure Container Service Kubernetes kümesi dağıtılır. Tamamlanan adımlar:

> [!div class="checklist"]
> * Kubernetes ACS kümesini dağıtma
> * Kubernetes CLI (kubectl) yüklemesi
> * Kubectl yapılandırması

Sonraki öğreticilerde, Azure Vote uygulaması kümeye dağıtılır, ölçeklendirilir, güncelleştirilir ve Operations Management Suite, Kubernetes kümesini izlemek için yapılandırılır.

## <a name="before-you-begin"></a>Başlamadan önce

Önceki öğreticilerde, bir kapsayıcı görüntüsü oluşturuldu ve Azure Container Registry örneğine yüklendi. Bu adımları tamamlamadıysanız ve takip etmek istiyorsanız, [Öğretici 1 – Kapsayıcı görüntüleri oluşturma](./container-service-tutorial-kubernetes-prepare-app.md) konusuna dönün.

## <a name="create-kubernetes-cluster"></a>Kubernetes kümesi oluşturma

Azure Container Service'te [az acs create](/cli/azure/acs#az_acs_create) komutuyla Kubernetes kümesi oluşturun. 

Şu örnek, `myResourceGroup` adlı Kaynak Grubunda `myK8sCluster` adlı bir küme oluşturur. Bu Kaynak Grubu, [bir önceki öğreticide](./container-service-tutorial-kubernetes-prepare-acr.md) oluşturuldu.

```azurecli-interactive 
az acs create --orchestrator-type kubernetes --resource-group myResourceGroup --name myK8SCluster --generate-ssh-keys 
```

Sınırlı deneme sürümünde olduğu gibi bazı durumlarda, bir Azure aboneliğinin Azure kaynaklarına sınırlı erişimi olur. Dağıtım sınırlı kullanılabilir çekirdek sayısı nedeniyle başarısız olursa, `--agent-count 1` öğesini [az acs create](/cli/azure/acs#az_acs_create) komutuna ekleyerek varsayılan aracı sayısını azaltın. 

Birkaç dakika sonra dağıtım tamamlanır ve ACS dağıtımı hakkında JSON tarafından biçimlendirilmiş bilgiler döndürür.

## <a name="install-the-kubectl-cli"></a>kubectl CLI yükleme

İstemci bilgisayarınızdan Kubernetes kümesine bağlanmak için Kubernetes’in komut satırı istemcisini ([kubectl](https://kubernetes.io/docs/user-guide/kubectl/)) kullanın. 

Azure CloudShell kullanıyorsanız kubectl zaten yüklüdür. Yerel olarak yüklemek istiyorsanız [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) komutunu kullanın.

Linux veya macOS’ta çalıştırılıyorsa, sudo ile çalıştırmanız gerekebilir. Windows üzerinde kabuğunuzun yönetici olarak çalıştırıldığından emin olun.

```azurecli-interactive 
az acs kubernetes install-cli 
```

Windows’ta, varsayılan yükleme *c:\program files (x86)\kubectl.exe*’dir. Bu dosyayı Windows yoluna eklemeniz gerekebilir. 

## <a name="connect-with-kubectl"></a>kubectl ile bağlanma

kubectl’i Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials) komutunu çalıştırın.

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group myResourceGroup --name myK8SCluster
```

Kümenize yönelik bağlantıyı doğrulamak için [kubectl get nodes](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) komutunu çalıştırın.

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

Öğretici tamamlandığında, iş yüklerine hazır bir ACS Kubernetes kümesine sahip olursunuz. Sonraki öğreticilerde bu kümeye birden çok kapsayıcılı uygulama dağıtılır, ölçeklendirilir, güncelleştirilir ve izlenir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Azure Container Service Kubernetes kümesi dağıtıldı. Aşağıdaki adımlar tamamlandı:

> [!div class="checklist"]
> * Kubernetes ACS kümesi dağıtıldı
> * Kubernetes CLI (kubectl) yüklendi
> * Kubectl yapılandırıldı

Kümede uygulama çalıştırma hakkında daha fazla bilgi için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Kubernetes'te uygulama dağıtma](./container-service-tutorial-kubernetes-deploy-application.md)
