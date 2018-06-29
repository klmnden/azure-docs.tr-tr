---
title: Azure diskleri AKS ile kullanma
description: Azure diskleri AKS ile kullanma
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/21/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: cfad5ebd1212df03cee86b71340d8a73c2594f57
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37096102"
---
# <a name="volumes-with-azure-disks"></a>Azure diskleri birimlerle

Kapsayıcı tabanlı uygulamalara erişmek ve bir dış veri birimdeki veriler kalıcı hale getirmek çoğunlukla gerekir. Azure diskleri bu dış veri deposu olarak kullanılabilir. Bir Azure Kubernetes hizmet (AKS) kümesindeki Kubernetes birimi olarak Azure bir diski kullanarak bu makale ayrıntıları.

Kubernetes birimleri hakkında daha fazla bilgi için bkz: [Kubernetes birimleri][kubernetes-volumes].

## <a name="create-an-azure-disk"></a>Azure diski oluşturun

Azure tarafından yönetilen bir disk Kubernetes toplu olarak takma önce disk AKS bulunmalıdır **düğümü** kaynak grubu. Kaynak grubu adı ile alma [az kaynak Göster] [ az-resource-show] komutu.

```azurecli-interactive
$ az resource show --resource-group myResourceGroup --name myAKSCluster --resource-type Microsoft.ContainerService/managedClusters --query properties.nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Kullanım [az disketi] [ az-disk-create] Azure diski oluşturmak için komutu.

Güncelleştirme `--resource-group` kaynak grubu adı ile toplanan son adımda ve `--name` için tercih ettiğiniz bir ad.

```azurecli-interactive
az disk create \
  --resource-group MC_myResourceGroup_myAKSCluster_eastus \
  --name myAKSDisk  \
  --size-gb 20 \
  --query id --output tsv
```

Disk oluşturulduktan sonra aşağıdakine benzer bir çıktı görmeniz gerekir. Bu değer diski takma kullanıldığında disk kimliğidir.

```console
/subscriptions/<subscriptionID>/resourceGroups/MC_myAKSCluster_myAKSCluster_eastus/providers/Microsoft.Compute/disks/myAKSDisk
```

## <a name="mount-disk-as-volume"></a>Disk birimi olarak bağlama

Azure diski birim kapsayıcı spec yapılandırarak, pod bağlayın.

Adlı yeni bir dosya oluşturun `azure-disk-pod.yaml` aşağıdaki içeriğe sahip. Güncelleştirme `diskName` yeni oluşturulan disk adını ve `diskURI` disk kimliğiyle Ayrıca, not edin `mountPath`, Azure diskin pod bağlı yolun olduğu.

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

Artık takılı olduğu bir Azure diski ile çalışan bir pod sahip `/mnt/azure`.

## <a name="next-steps"></a>Sonraki adımlar

Azure diskleri kullanarak Kubernetes birimleri hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Azure diskleri için Kubernetes eklentisi][kubernetes-disks]

<!-- LINKS - external -->
[kubernetes-disks]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_disk/README.md
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/

<!-- LINKS - internal -->
[az-disk-list]: /cli/azure/disk#az_disk_list
[az-disk-create]: /cli/azure/disk#az_disk_create
[az-group-list]: /cli/azure/group#az_group_list
[az-resource-show]: /cli/azure/resource#az-resource-show
