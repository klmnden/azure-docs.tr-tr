---
title: Azure Kubernetes Service (AKS) pod'ları için statik bir birim oluşturun
description: El ile bir pod Azure Kubernetes Service (AKS) ile kullanılmak üzere Azure diskleri olan bir birim oluşturmayı öğrenin
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 03/01/2019
ms.author: iainfou
ms.openlocfilehash: 02a863a4ddf59fb36c5f2ae7f3092896d2e1d860
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60467536"
---
# <a name="manually-create-and-use-a-volume-with-azure-disks-in-azure-kubernetes-service-aks"></a>El ile oluşturma ve birim Azure diskleri Azure Kubernetes Service (AKS) kullanma

Kapsayıcı tabanlı uygulamalar genellikle erişmek ve bir dış veri birimdeki veriler kalıcı hale getirmek gerekir. Tek bir pod depolama erişmesi gerekirse, uygulama kullanımı için yerel bir birime sunmak için Azure diskleri kullanabilirsiniz. Bu makalede, el ile bir Azure disk oluşturun ve bir pod aks'deki ekleme işlemini göstermektedir.

> [!NOTE]
> Bir Azure diskinin aynı anda yalnızca tek bir pod bağlanabilir. Kalıcı hacim birden çok podunuz arasında paylaşmak ihtiyacınız varsa [Azure dosyaları][azure-files-volume].

Kubernetes birimleri hakkında daha fazla bilgi için bkz. [AKS uygulamalar için Depolama Seçenekleri][concepts-storage].

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, var olan bir AKS kümesi olduğunu varsayar. AKS hızlı bir AKS kümesi gerekirse bkz [Azure CLI kullanarak] [ aks-quickstart-cli] veya [Azure portalını kullanarak][aks-quickstart-portal].

Ayrıca Azure CLI Sürüm 2.0.59 gerekir veya daha sonra yüklü ve yapılandırılmış. Çalıştırma `az --version` sürümü bulmak için. Gerekirse yüklemek veya yükseltmek bkz [Azure CLI yükleme][install-azure-cli].

## <a name="create-an-azure-disk"></a>Bir Azure diskinin oluşturma

AKS ile kullanmak için bir Azure diskinin oluşturduğunuzda, disk kaynak oluşturabilirsiniz **düğüm** kaynak grubu. Bu yaklaşım erişmek ve disk kaynağı yönetmek AKS kümesi sağlar. Bunun yerine ayrı kaynak grubunda disk oluşturursanız, kümeniz için Azure Kubernetes Service (AKS) hizmet sorumlusu vermelisiniz `Contributor` rolüne diskin kaynak grubu.

Bu makalede, düğüm kaynak grubunda disk oluşturun. İlk olarak, kaynak grubu adını alın [az aks show] [ az-aks-show] komut ve ekleme `--query nodeResourceGroup` sorgu parametresi. Aşağıdaki örnek, düğüm kaynak grubu için AKS kümesinin adını alır. *myAKSCluster* kaynak grubu adında *myResourceGroup*:

```azurecli-interactive
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
> Azure diskleri için belirli bir boyut SKU tarafından faturalandırılır. Bu SKU'ları 32GiB S4 veya P4 diskleri ile arasındadır 32TiB S80 veya P80 disklerin (önizlemede). Aktarım hızı ve IOPS performansı bir Premium yönetilen disk bağlıdır AKS kümesindeki düğümlere örnek boyutu ve SKU. Bkz: [fiyatlandırma ve yönetilen diskler performansını][managed-disk-pricing-performance].

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
  name: mypod
spec:
  containers:
  - image: nginx:1.15.5
    name: mypod
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
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

Artık takılı olduğu bir Azure diskinin ile çalışan bir pod sahip `/mnt/azure`. Kullanabileceğiniz `kubectl describe pod mypod` disk başarıyla oluşturulmuş doğrulayın. Aşağıdaki sıkıştırılmış örneğe çıktı kapsayıcıda takılı birim gösterir:

```
[...]
Volumes:
  azure:
    Type:         AzureDisk (an Azure Data Disk mount on the host and bind mount to the pod)
    DiskName:     myAKSDisk
    DiskURI:      /subscriptions/<subscriptionID/resourceGroups/MC_myResourceGroupAKS_myAKSCluster_eastus/providers/Microsoft.Compute/disks/myAKSDisk
    Kind:         Managed
    FSType:       ext4
    CachingMode:  ReadWrite
    ReadOnly:     false
  default-token-z5sd7:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-z5sd7
    Optional:    false
[...]
Events:
  Type    Reason                 Age   From                               Message
  ----    ------                 ----  ----                               -------
  Normal  Scheduled              1m    default-scheduler                  Successfully assigned mypod to aks-nodepool1-79590246-0
  Normal  SuccessfulMountVolume  1m    kubelet, aks-nodepool1-79590246-0  MountVolume.SetUp succeeded for volume "default-token-z5sd7"
  Normal  SuccessfulMountVolume  41s   kubelet, aks-nodepool1-79590246-0  MountVolume.SetUp succeeded for volume "azure"
[...]
```

## <a name="next-steps"></a>Sonraki adımlar

İlişkili en iyi yöntemler için bkz: [en iyi uygulamalar için depolama ve yedekleme aks'deki][operator-best-practices-storage].

AKS hakkında daha fazla bilgi için kümeleri ile Azure disklerini etkileşim, bkz: [Azure diskleri için Kubernetes eklentisi][kubernetes-disks].

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
[install-azure-cli]: /cli/azure/install-azure-cli
[azure-files-volume]: azure-files-volume.md
[operator-best-practices-storage]: operator-best-practices-storage.md
[concepts-storage]: concepts-storage.md
