---
title: Azure Kubernetes Service (AKS) pod'ları için statik bir birim oluşturun
description: El ile Azure disklerini pod'ları Azure Kubernetes Service (AKS) ile kullanım için bir birim oluşturmayı öğrenin
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 09/26/2018
ms.author: iainfou
ms.openlocfilehash: 68a7883e7f8b3fb62265375208f66b761d43d82e
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47391218"
---
# <a name="manually-create-and-use-kubernetes-volume-with-azure-disks-in-azure-kubernetes-service-aks"></a>El ile oluşturma ve Azure diskleri Azure Kubernetes Service (AKS) ile Kubernetes hacmi kullanma

Kapsayıcı tabanlı uygulamalar genellikle erişmek ve bir dış veri birimdeki veriler kalıcı hale getirmek gerekir. Azure diskleri bu dış veri deposu olarak kullanılabilir. AKS, birimler, kalıcı hacim talep kullanarak dinamik olarak oluşturulabilir veya el ile oluşturun ve bir Azure diskinin doğrudan ekleyin. Bu makalede, el ile bir Azure disk oluşturun ve bir pod aks'deki ekleme işlemini göstermektedir.

Kubernetes birimleri hakkında daha fazla bilgi için bkz. [Kubernetes birimleri][kubernetes-volumes].

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, var olan bir AKS kümesi olduğunu varsayar. AKS hızlı bir AKS kümesi gerekirse bkz [Azure CLI kullanarak] [ aks-quickstart-cli] veya [Azure portalını kullanarak][aks-quickstart-portal].

Ayrıca Azure CLI Sürüm 2.0.46 gerekir veya daha sonra yüklü ve yapılandırılmış. Sürümü bulmak için `az --version` komutunu çalıştırın. [Azure CLI yükleme] [yükleme-azure-cli] yüklemeniz veya yükseltmeniz gerekirse, bkz.

## <a name="create-an-azure-disk"></a>Bir Azure diskinin oluşturma

AKS ile kullanmak için bir Azure diskinin oluşturduğunuzda, disk kaynak oluşturabilirsiniz **düğüm** kaynak grubu. Bu yaklaşım erişmek ve disk kaynağı yönetmek AKS kümesi sağlar. Bunun yerine ayrı kaynak grubunda disk oluşturursanız, kümeniz için Azure Kubernetes Service (AKS) hizmet sorumlusu vermelisiniz `Contributor` rolüne diskin kaynak grubu.

Bu makalede, düğüm kaynak grubunda disk oluşturun. İlk olarak, kaynak grubu adını alın [az aks show] [ az-aks-show] komut ve ekleme `--query nodeResourceGroup` sorgu parametresi. Aşağıdaki örnek, düğüm kaynak grubu için AKS kümesinin adını alır. *myAKSCluster* kaynak grubu adında *myResourceGroup*:

```azurecli
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Şimdi bir diski kullanarak oluşturmak [az disk oluşturma] [ az-disk-create] komutu. Önceki komutta ve ardından disk kaynağı için bir ad gibi elde düğüm kaynak grubunu adını belirtmelisiniz *myAKSDisk*. Aşağıdaki örnek, oluşturur bir *20*GiB disk ve oluşturulduktan sonra disk çıkışlarını kimliği:

```azurecli-interactive
az disk create \
  --resource-group MC_myResourceGroup_myAKSCluster_eastus \
  --name myAKSDisk  \
  --size-gb 20 \
  --query id --output tsv
```

> [!NOTE]
> Azure diskleri için belirli bir boyut SKU tarafından faturalandırılır. Bu SKU'ları aralığından 32GiB S4 veya P4 disk için 8TiB S60 veya P60 diskler için. Aktarım hızı ve IOPS performansı bir Premium yönetilen disk bağlıdır AKS kümesindeki düğümlere örnek boyutu ve SKU. Bkz: [fiyatlandırma ve yönetilen diskler performansını][managed-disk-pricing-performance].

Komut başarıyla tamamlandıktan sonra disk kaynak kimliği aşağıdaki örnek çıktıda gösterildiği gibi görüntülenir. Bu disk kimliği, sonraki adımda disk bağlamak için kullanılır.

```console
/subscriptions/<subscriptionID>/resourceGroups/MC_myAKSCluster_myAKSCluster_eastus/providers/Microsoft.Compute/disks/myAKSDisk
```

## <a name="mount-disk-as-volume"></a>Disk birimi olarak bağlama

Azure disk pod bağlamak için birim kapsayıcı spec içinde yapılandırın. Adlı yeni bir dosya oluşturun `azure-disk-pod.yaml` aşağıdaki içeriğe sahip. Güncelleştirme `diskName` önceki adımda oluşturduğunuz disk adı ile ve `diskURI` disk çıktıda gösterilen disk kimliği ile komut oluşturma. İsterseniz, güncelleştirme `mountPath`, burada Azure disk bağlı pod yolu olduğu.

```yaml
apiVersion: v1
kind: Pod
metadata:
 name: azure-disk-pod
spec:
 containers:
  - image: microsoft/sample-aks-helloworld
    name: azure
    volumeMounts:
      - name: azure
        mountPath: /mnt/azure
 volumes:
      - name: azure
        azureDisk:
          kind: Managed
          diskName: myAKSDisk
          diskURI: /subscriptions/<subscriptionID>/resourceGroups/MC_myAKSCluster_myAKSCluster_eastus/providers/Microsoft.Compute/disks/myAKSDisk
```

Kullanım `kubectl` pod oluşturmak için komutu.

```console
kubectl apply -f azure-disk-pod.yaml
```

Artık takılı olduğu bir Azure diskinin ile çalışan bir pod sahip `/mnt/azure`. Kullanabileceğiniz `kubectl describe pod azure-disk-pod` disk başarıyla oluşturulmuş doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

AKS hakkında daha fazla bilgi için kümeleri ile Azure disklerini etkileşim, bkz: [Azure diskleri için Kubernetes eklentisi][kubernetes-disks]

<!-- LINKS - external -->
[kubernetes-disks]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_disk/README.md
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/
[managed-disk-pricing-performance]: https://azure.microsoft.com/pricing/details/managed-disks/

<!-- LINKS - internal -->
[az-disk-list]: /cli/azure/disk#az-disk-list
[az-disk-create]: /cli/azure/disk#az-disk-create
[az-group-list]: /cli/azure/group#az-group-list
[az-resource-show]: /cli/azure/resource#az-resource-show
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[az-aks-show]: /cli/azure/aks#az-aks-show
