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
ms.openlocfilehash: 33d9a01f063ee8ad531a3f7e01dcfbf1c4ba8901
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="volumes-with-azure-disks"></a>Azure diskleri birimlerle

Kapsayıcı tabanlı uygulamalara erişmek ve bir dış veri birimdeki veriler kalıcı hale getirmek çoğunlukla gerekir. Azure diskleri bu dış veri deposu olarak kullanılabilir. Bir Azure Kubernetes hizmet (AKS) kümesindeki Kubernetes birimi olarak Azure bir diski kullanarak bu makale ayrıntıları.

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

## <a name="mount-disk-as-volume"></a>Disk birimi olarak bağlama

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
