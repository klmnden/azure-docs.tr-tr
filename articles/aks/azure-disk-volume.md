---
title: AKS ile Azure disklerini kullanma
description: AKS ile Azure disklerini kullanma
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/21/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: f807264dc2c2e07ccd175fb1b0427b7ce9e9f524
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37868254"
---
# <a name="volumes-with-azure-disks"></a>Azure diskleri ile birimleri

Kapsayıcı tabanlı uygulamalar genellikle erişmek ve bir dış veri birimdeki veriler kalıcı hale getirmek gerekir. Azure diskleri bu dış veri deposu olarak kullanılabilir. Bu makalede Azure Kubernetes Service (AKS) kümesi içinde bir Kubernetes birimi olarak bir Azure diskinin kullanarak ayrıntıları verilmektedir.

Kubernetes birimleri hakkında daha fazla bilgi için bkz. [Kubernetes birimleri][kubernetes-volumes].

## <a name="create-an-azure-disk"></a>Bir Azure diskinin oluşturma

Kubernetes birimi olarak Azure tarafından yönetilen bir disk bağlamadan önce disk AKS bulunmalıdır **düğüm** kaynak grubu. Kaynak grubu adını alın [az resource show] [ az-resource-show] komutu.

```azurecli-interactive
$ az resource show --resource-group myResourceGroup --name myAKSCluster --resource-type Microsoft.ContainerService/managedClusters --query properties.nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Kullanım [az disk oluşturma] [ az-disk-create] Azure disk oluşturmak için komutu.

Güncelleştirme `--resource-group` toplanan son adımda, kaynak grubu adını ve `--name` tercih ettiğiniz bir adı.

```azurecli-interactive
az disk create \
  --resource-group MC_myResourceGroup_myAKSCluster_eastus \
  --name myAKSDisk  \
  --size-gb 20 \
  --query id --output tsv
```

Disk oluşturulduktan sonra aşağıdakine benzer bir çıktı görmeniz gerekir. Disk takılamadı sırasında kullanılan disk kimliği değerdir.

```console
/subscriptions/<subscriptionID>/resourceGroups/MC_myAKSCluster_myAKSCluster_eastus/providers/Microsoft.Compute/disks/myAKSDisk
```
> [!NOTE]
> Azure yönetilen diskler için belirli bir boyut SKU tarafından faturalandırılır. Bu SKU'ları 32GiB S4 veya P4 diskleri ile arasındadır 4TiB S50 veya P50 disk için. Ayrıca, aktarım hızı ve IOPS performansı bir Premium yönetilen disk bağlıdır AKS kümesindeki düğümlere örnek boyutu ve SKU. Bkz: [fiyatlandırma ve yönetilen diskler performansını][managed-disk-pricing-performance].

> [!NOTE]
> Ayrı kaynak grubunda disk oluşturmanız gerekiyorsa, Azure Kubernetes Service (AKS) hizmet sorumlusu, küme için disk barındıran kaynak grubunu eklemek etmeniz `Contributor` rol. 
>

## <a name="mount-disk-as-volume"></a>Disk birimi olarak bağlama

Azure disk birimi kapsayıcı spec yapılandırarak pod bağlayın.

Adlı yeni bir dosya oluşturun `azure-disk-pod.yaml` aşağıdaki içeriğe sahip. Güncelleştirme `diskName` yeni oluşturulan disk adı ile ve `diskURI` disk kimliğiyle Ayrıca, Not `mountPath`, burada Azure disk bağlı pod yolu olduğu.

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

Kubectl pod oluşturmak için kullanın.

```azurecli-interactive
kubectl apply -f azure-disk-pod.yaml
```

Artık takılı olduğu bir Azure diskinin ile çalışan bir pod sahip `/mnt/azure`.

## <a name="next-steps"></a>Sonraki adımlar

Azure diskleri kullanarak Kubernetes birimleri hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Azure diskleri için Kubernetes eklentisi][kubernetes-disks]

<!-- LINKS - external -->
[kubernetes-disks]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_disk/README.md
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/
[managed-disk-pricing-performance]: https://azure.microsoft.com/pricing/details/managed-disks/

<!-- LINKS - internal -->
[az-disk-list]: /cli/azure/disk#az_disk_list
[az-disk-create]: /cli/azure/disk#az_disk_create
[az-group-list]: /cli/azure/group#az_group_list
[az-resource-show]: /cli/azure/resource#az-resource-show
