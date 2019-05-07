---
title: Dinamik olarak Azure Kubernetes Service (AKS) için birden çok podunuz bir Disk birimi oluşturma
description: Dinamik olarak birden çok eş zamanlı pod Azure Kubernetes Service (AKS) ile kullanılmak üzere Azure diskleri olan bir kalıcı hacim oluşturmayı öğrenin
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 03/01/2019
ms.author: iainfou
ms.openlocfilehash: 735be71faecb9882b13f6f536d43715139d0f4db
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65071991"
---
# <a name="dynamically-create-and-use-a-persistent-volume-with-azure-disks-in-azure-kubernetes-service-aks"></a>Dinamik olarak oluşturabilen ve Azure diskleri Azure Kubernetes Service (AKS) ile kalıcı hacim kullanma

Kalıcı hacim Kubernetes pod'ları ile kullanmak için sağlanan depolama parçasını temsil eder. Kalıcı hacim, bir veya daha çok pod'ları tarafından kullanılabilir ve dinamik veya statik olarak sağlanabilir. Bu makalede dinamik olarak Azure Kubernetes Service (AKS) kümesi içinde tek bir pod tarafından kullanılmak üzere Azure disklerle kalıcı birimler oluşturma işlemini gösterir.

> [!NOTE]
> Bir Azure diskinin yalnızca ile bağlanabilir *erişim modu* türü *ReadWriteOnce*, hangi kullanılabilir hale getirir, yalnızca tek bir pod için AKS. Kalıcı hacim birden çok podunuz arasında paylaşmak ihtiyacınız varsa [Azure dosyaları][azure-files-pvc].

Kubernetes birimleri hakkında daha fazla bilgi için bkz. [AKS uygulamalar için Depolama Seçenekleri][concepts-storage].

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, var olan bir AKS kümesi olduğunu varsayar. AKS hızlı bir AKS kümesi gerekirse bkz [Azure CLI kullanarak] [ aks-quickstart-cli] veya [Azure portalını kullanarak][aks-quickstart-portal].

Ayrıca Azure CLI Sürüm 2.0.59 gerekir veya daha sonra yüklü ve yapılandırılmış. Çalıştırma `az --version` sürümü bulmak için. Gerekirse yüklemek veya yükseltmek bkz [Azure CLI yükleme][install-azure-cli].

## <a name="built-in-storage-classes"></a>Yerleşik depolama sınıfları

Bir depolama sınıfı, depolama birimi dinamik olarak oluşturulan bir kalıcı hacim ile nasıl tanımlamak için kullanılır. Kubernetes depolama sınıfları hakkında daha fazla bilgi için bkz. [Kubernetes depolama sınıfları][kubernetes-storage-classes].

Her bir AKS kümesi iki önceden oluşturulmuş depolama sınıfları içerir; her ikisi ile Azure disklerini çalışmak üzere yapılandırıldı:

* *Varsayılan* Azure standart disk depolama sınıfı sağlar.
    * Standart depolama, HDD'ler ile desteklenir ve hala performansa sahip olmanın yanı sıra, düşük maliyetli depolama sunar. Standart diskler, uygun maliyetli bir geliştirme ve iş yükü testi için idealdir.
* *Premium yönetilen* Azure disk bir premium depolama sınıfı sağlar.
    * Premium diskler SSD tabanlı, yüksek performanslı ve düşük gecikme süreli disk ile desteklenir. Üretim iş yükü çalıştıran VM'ler için son derece uygundur. AKS düğümleri premium depolama kullanıyorsanız, belirleyin *premium yönetilen* sınıfı.

Kullanım [kubectl alma sc] [ kubectl-get] önceden oluşturulmuş depolama sınıfları görmek için komutu. Aşağıdaki örnekte gösterildiği bir AKS kümesi içinde kullanılabilen depolama sınıfları önceden oluştur:

```console
$ kubectl get sc

NAME                PROVISIONER                AGE
default (default)   kubernetes.io/azure-disk   1h
managed-premium     kubernetes.io/azure-disk   1h
```

> [!NOTE]
> Kalıcı hacim talep GiB belirtildi, ancak Azure yönetilen diskler için belirli bir boyut SKU tarafından faturalandırılır. Bu SKU'ları 32GiB S4 veya P4 diskleri ile arasındadır 32TiB S80 veya P80 disklerin (önizlemede). Aktarım hızı ve IOPS performansı bir Premium yönetilen disk, her iki SKU üzerinde bağlıdır ve AKS kümesindeki düğümlere örneği boyutu. Daha fazla bilgi için [fiyatlandırma ve yönetilen diskler performans][managed-disk-pricing-performance].

## <a name="create-a-persistent-volume-claim"></a>Kalıcı hacim talep oluşturma

Kalıcı hacim talep (PVC) otomatik olarak bir depolama sınıfına göre depolama sağlamak için kullanılır. Bu durumda, PVC önceden oluşturulmuş depolama sınıfları bir standart veya premium Azure oluşturmak için kullanabilirsiniz yönetilen disk.

Adlı bir dosya oluşturun `azure-premium.yaml`, aşağıdaki bildirim kopyalayın. Adlı bir disk talep istekleri `azure-managed-disk` diğer bir deyişle *5GB* boyutu *ReadWriteOnce* erişim. *Premium yönetilen* depolama sınıfı depolama sınıfı belirtildi.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azure-managed-disk
spec:
  accessModes:
  - ReadWriteOnce
  storageClassName: managed-premium
  resources:
    requests:
      storage: 5Gi
```

> [!TIP]
> Standart depolama kullanan bir disk oluşturmak için kullanın `storageClassName: default` yerine *premium yönetilen*.

Kalıcı hacim taleple oluşturma [kubectl uygulamak] [ kubectl-apply] komut ve belirtin, *azure premium.yaml* dosyası:

```console
$ kubectl apply -f azure-premium.yaml

persistentvolumeclaim/azure-managed-disk created
```

## <a name="use-the-persistent-volume"></a>Kalıcı hacim kullanın

Kalıcı hacim talep oluşturulduktan sonra başarıyla kaynak sağlandı, disk disk erişimi olan bir pod oluşturulabilir. Aşağıdaki bildirim adlı kalıcı hacim talep kullanan temel bir NGINX pod oluşturur *azure yönetilen diski* Azure diski yolda `/mnt/azure`.

Adlı bir dosya oluşturun `azure-pvc-disk.yaml`, aşağıdaki bildirim kopyalayın.

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: mypod
spec:
  containers:
  - name: mypod
    image: nginx:1.15.5
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
    volumeMounts:
    - mountPath: "/mnt/azure"
      name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azure-managed-disk
```

Pod ile oluşturma [kubectl uygulamak] [ kubectl-apply] aşağıdaki örnekte gösterildiği gibi komut:

```console
$ kubectl apply -f azure-pvc-disk.yaml

pod/mypod created
```

Artık takılabilir diskinizi Azure ile çalışan bir pod sahip `/mnt/azure` dizin. Bu yapılandırma ile pod incelenirken görülebilir `kubectl describe pod mypod`sıkıştırılmış aşağıdaki örnekte gösterildiği gibi:

```console
$ kubectl describe pod mypod

[...]
Volumes:
  volume:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  azure-managed-disk
    ReadOnly:   false
  default-token-smm2n:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-smm2n
    Optional:    false
[...]
Events:
  Type    Reason                 Age   From                               Message
  ----    ------                 ----  ----                               -------
  Normal  Scheduled              2m    default-scheduler                  Successfully assigned mypod to aks-nodepool1-79590246-0
  Normal  SuccessfulMountVolume  2m    kubelet, aks-nodepool1-79590246-0  MountVolume.SetUp succeeded for volume "default-token-smm2n"
  Normal  SuccessfulMountVolume  1m    kubelet, aks-nodepool1-79590246-0  MountVolume.SetUp succeeded for volume "pvc-faf0f176-8b8d-11e8-923b-deb28c58d242"
[...]
```

## <a name="back-up-a-persistent-volume"></a>Kalıcı bir birimi yedekleyin

Kalıcı toplu verileri yedeklemek için birimin yönetilen diskin anlık görüntüsünü alın. Ardından bu anlık görüntüyü geri yüklenen diski oluşturmak ve verileri geri bir yol pod'ların eklemek için de kullanabilirsiniz.

İlk olarak, birim adını alın `kubectl get pvc` komutu gibi adlı PVC *azure yönetilen diski*:

```console
$ kubectl get pvc azure-managed-disk

NAME                 STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS      AGE
azure-managed-disk   Bound     pvc-faf0f176-8b8d-11e8-923b-deb28c58d242   5Gi        RWO            managed-premium   3m
```

Bu birim adı, temel alınan Azure disk adı oluşturur. Disk kimliği ile sorgu [az disk listesi] [ az-disk-list] ve PVC birim adınızı, aşağıdaki örnekte gösterildiği gibi sağlayın:

```azurecli-interactive
$ az disk list --query '[].id | [?contains(@,`pvc-faf0f176-8b8d-11e8-923b-deb28c58d242`)]' -o tsv

/subscriptions/<guid>/resourceGroups/MC_MYRESOURCEGROUP_MYAKSCLUSTER_EASTUS/providers/MicrosoftCompute/disks/kubernetes-dynamic-pvc-faf0f176-8b8d-11e8-923b-deb28c58d242
```

Disk kimliği bir anlık görüntü diskle oluşturulacağı [az anlık görüntü oluşturma][az-snapshot-create]. Aşağıdaki örnekte adlı bir anlık görüntü oluşturur *pvcSnapshot* AKS kümesi ile aynı kaynak grubunda (*MC_myResourceGroup_myAKSCluster_eastus*). Anlık görüntüleri oluşturur ve AKS kümesi erişimi olmayan kaynak grupları disklerin geri yüklerseniz, izin sorunları karşılaşabilirsiniz.

```azurecli-interactive
$ az snapshot create \
    --resource-group MC_myResourceGroup_myAKSCluster_eastus \
    --name pvcSnapshot \
    --source /subscriptions/<guid>/resourceGroups/MC_myResourceGroup_myAKSCluster_eastus/providers/MicrosoftCompute/disks/kubernetes-dynamic-pvc-faf0f176-8b8d-11e8-923b-deb28c58d242
```

Veri miktarına bağlı olarak, disk üzerinde bu anlık görüntü oluşturmak için birkaç dakika sürebilir.

## <a name="restore-and-use-a-snapshot"></a>Geri yükleme ve bir anlık görüntüsünü kullanma

Bir disk ile oluşturduğunuzda diski geri yükleme ve bir Kubernetes pod ile kullanmak için anlık görüntü kaynağı olarak kullanın [az disk oluşturma][az-disk-create]. Ardından özgün veri anlık erişim gerekiyorsa, bu işlem, özgün kaynak korur. Aşağıdaki örnekte adlı bir disk oluşturur *pvcRestored* adlı anlık görüntüden *pvcSnapshot*:

```azurecli-interactive
az disk create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name pvcRestored --source pvcSnapshot
```

Geri yüklenen diski ile birlikte bir pod kullanmak için bildirimde disk kimliği belirtin. İle disk Kimliğini edinmek [az disk show] [ az-disk-show] komutu. Aşağıdaki örnekte disk kimliği alır *pvcRestored* önceki adımda oluşturduğunuz:

```azurecli-interactive
az disk show --resource-group MC_myResourceGroup_myAKSCluster_eastus --name pvcRestored --query id -o tsv
```

Adlı bir pod bildirimi oluşturma `azure-restored.yaml` ve önceki adımda elde edilen URI diski belirtin. Aşağıdaki örnek, bir birim olarak bağlanmış geri yüklenen diski ile temel bir NGINX web sunucusu oluşturur. */mnt/azure*:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: mypodrestored
spec:
  containers:
  - name: mypodrestored
    image: nginx:1.15.5
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
    volumeMounts:
    - mountPath: "/mnt/azure"
      name: volume
  volumes:
    - name: volume
      azureDisk:
        kind: Managed
        diskName: pvcRestored
        diskURI: /subscriptions/<guid>/resourceGroups/MC_myResourceGroupAKS_myAKSCluster_eastus/providers/Microsoft.Compute/disks/pvcRestored
```

Pod ile oluşturma [kubectl uygulamak] [ kubectl-apply] aşağıdaki örnekte gösterildiği gibi komut:

```console
$ kubectl apply -f azure-restored.yaml

pod/mypodrestored created
```

Kullanabileceğiniz `kubectl describe pod mypodrestored` birim bilgilerini gösteren aşağıdaki sıkıştırılmış örneğe gibi pod ayrıntılarını görüntülemek için:

```console
$ kubectl describe pod mypodrestored

[...]
Volumes:
  volume:
    Type:         AzureDisk (an Azure Data Disk mount on the host and bind mount to the pod)
    DiskName:     pvcRestored
    DiskURI:      /subscriptions/19da35d3-9a1a-4f3b-9b9c-3c56ef409565/resourceGroups/MC_myResourceGroupAKS_myAKSCluster_eastus/providers/Microsoft.Compute/disks/pvcRestored
    Kind:         Managed
    FSType:       ext4
    CachingMode:  ReadWrite
    ReadOnly:     false
[...]
```

## <a name="next-steps"></a>Sonraki adımlar

İlişkili en iyi yöntemler için bkz: [en iyi uygulamalar için depolama ve yedekleme aks'deki][operator-best-practices-storage].

Azure diskleri kullanarak Kubernetes kalıcı birimleri hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Azure diskleri için Kubernetes eklentisi][azure-disk-volume]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/
[managed-disk-pricing-performance]: https://azure.microsoft.com/pricing/details/managed-disks/

<!-- LINKS - internal -->
[azure-disk-volume]: azure-disk-volume.md
[azure-files-pvc]: azure-files-dynamic-pv.md
[premium-storage]: ../virtual-machines/windows/disks-types.md
[az-disk-list]: /cli/azure/disk#az-disk-list
[az-snapshot-create]: /cli/azure/snapshot#az-snapshot-create
[az-disk-create]: /cli/azure/disk#az-disk-create
[az-disk-show]: /cli/azure/disk#az-disk-show
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[operator-best-practices-storage]: operator-best-practices-storage.md
[concepts-storage]: concepts-storage.md
