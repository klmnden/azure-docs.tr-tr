---
title: Azure diskleri AKS ile kullanma
description: Azure diskleri AKS ile kullanma
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 03/08/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: e6b2d6e31a843259001a36ffb8c588c879a3f2b9
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="volumes-with-azure-disks"></a>Azure diskleri birimlerle

Kapsayıcı tabanlı uygulamalara erişmek ve bir dış veri birimdeki veriler kalıcı hale getirmek çoğunlukla gerekir. Azure diskleri bu dış veri deposu olarak kullanılabilir. Bir Azure kapsayıcı hizmeti (AKS) kümesindeki Kubernetes birimi olarak Azure bir diski kullanarak bu makale ayrıntıları.

Kubernetes birimleri hakkında daha fazla bilgi için bkz: [Kubernetes birimleri][kubernetes-volumes].

## <a name="create-an-azure-disk"></a>Azure diski oluşturun

Bir Azure takma önce disk yönetilen Kubernetes birimi olarak AKS küme kaynakları ile aynı kaynak grubunda disk bulunmalıdır. Bu kaynak grubu bulmak için [az grup listesi] [ az-group-list] komutu.

```azurecli-interactive
az group list --output table
```

Benzer şekilde bir ada sahip bir kaynak grubu arıyorsunuz `MC_clustername_clustername_locaton`, burada clustername AKS kümenizin adıdır ve konum burada küme dağıtılan Azure bölgesi.

```console
Name                                 Location    Status
-----------------------------------  ----------  ---------
MC_myAKSCluster_myAKSCluster_eastus  eastus      Succeeded
myAKSCluster                         eastus      Succeeded
```

Kullanım [az disketi] [ az-disk-create] Azure diski oluşturmak için komutu. 

Bu örneği kullanarak, güncelleştirme `--resource-group` kaynak grubu adını ve `--name` için tercih ettiğiniz bir ad.

```azurecli-interactive
az disk create \
  --resource-group MC_myAKSCluster_myAKSCluster_eastus \
  --name myAKSDisk  \
  --size-gb 20 \
  --query id --output tsv
```

Disk oluşturulduktan sonra aşağıdakine benzer bir çıktı görmeniz gerekir. Bu değer Kubernetes pod diske bağlanması sırasında kullanılan disk kimliğidir.

```console
/subscriptions/<subscriptionID>/resourceGroups/MC_myAKSCluster_myAKSCluster_eastus/providers/Microsoft.Compute/disks/myAKSDisk
```

## <a name="mount-file-share-as-volume"></a>Birim olarak dosya paylaşımını bağlama

Azure diski birim kapsayıcı spec yapılandırarak, pod bağlayın. 

Adlı yeni bir dosya oluşturun `azure-disk-pod.yaml` aşağıdaki içeriğe sahip. Güncelleştirme `diskName` yeni oluşturulan disk adını ve `diskURI` disk kimliğiyle Ayrıca, not edin `mountPath`, hangi Azure diski takılı pod yolu budur.

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
