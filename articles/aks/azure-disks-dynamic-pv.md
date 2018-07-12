---
title: Azure Kubernetes hizmeti ile kalıcı birimler oluşturun
description: Azure diskleri kalıcı birimleri pod'ları için Azure Kubernetes Service (AKS) oluşturmak için kullanmayı öğrenin
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/10/2018
ms.author: iainfou
ms.openlocfilehash: 14617b57f59c068aa015c9bfea9b4d18520b4152
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38473691"
---
# <a name="create-persistent-volumes-with-azure-disks-for-azure-kubernetes-service-aks"></a>Kalıcı birimleri, Azure Kubernetes Service (AKS) ile Azure disklerini oluşturma

Kalıcı hacim Kubernetes pod'ları ile kullanmak için sağlanan depolama parçasını temsil eder. Kalıcı hacim, bir veya daha çok pod'ları tarafından kullanılabilir ve dinamik veya statik olarak sağlanabilir. Kubernetes sürekli birimleri hakkında daha fazla bilgi için bkz. [Kubernetes kalıcı birimler][kubernetes-volumes]. Bu makalede Azure Kubernetes Service (AKS) kümesini Azure diskleri ile kalıcı birimler kullanmayı gösterir.

> [!NOTE]
> Bir Azure diskinin yalnızca ile bağlanabilir *erişim modu* türü *ReadWriteOnce*, hangi kullanılabilir hale getirir, yalnızca tek bir AKS düğümü. Kalıcı hacim paylaşan birden fazla düğümde gerek kalmadan kullanmayı [Azure dosyaları][azure-files-pvc].

## <a name="built-in-storage-classes"></a>Yerleşik depolama sınıfları

Bir depolama sınıfı, depolama birimi dinamik olarak oluşturulan bir kalıcı hacim ile nasıl tanımlamak için kullanılır. Kubernetes depolama sınıfları hakkında daha fazla bilgi için bkz. [Kubernetes depolama sınıfları][kubernetes-storage-classes].

Her bir AKS kümesi iki önceden oluşturulmuş depolama sınıfları içerir; her ikisi ile Azure disklerini çalışmak üzere yapılandırıldı. *Varsayılan* Azure standart disk depolama sınıfı sağlar. *Premium yönetilen* Azure disk bir premium depolama sınıfı sağlar. AKS düğümleri premium depolama kullanıyorsanız, belirleyin *premium yönetilen* sınıfı.

Kullanım [kubectl alma sc] [ kubectl-get] önceden oluşturulmuş depolama sınıfları görmek için komutu. Aşağıdaki örnekte gösterildiği bir AKS kümesi içinde kullanılabilen depolama sınıfları önceden oluştur:

```
$ kubectl get sc

NAME                PROVISIONER                AGE
default (default)   kubernetes.io/azure-disk   1h
managed-premium     kubernetes.io/azure-disk   1h
```

> [!NOTE]
> Kalıcı hacim talep GiB belirtildi, ancak Azure yönetilen diskler için belirli bir boyut SKU tarafından faturalandırılır. Bu SKU'ları 32GiB S4 veya P4 diskleri ile arasındadır 4TiB S50 veya P50 disk için. Ayrıca, aktarım hızı ve IOPS performansı bir Premium yönetilen disk, her iki SKU üzerinde bağlıdır ve AKS kümesindeki düğümlere örneği boyutu. Daha fazla bilgi için [fiyatlandırma ve yönetilen diskler performans][managed-disk-pricing-performance].

## <a name="create-a-persistent-volume-claim"></a>Kalıcı hacim talep oluşturma

Kalıcı hacim talep (PVC) otomatik olarak bir depolama sınıfına göre depolama sağlamak için kullanılır. Bu durumda, PVC önceden oluşturulmuş depolama sınıfları bir standart veya premium Azure oluşturmak için kullanabilirsiniz yönetilen disk.

Adlı bir dosya oluşturun `azure-premium.yaml`, aşağıdaki bildirim kopyalayın.

Not, *premium yönetilen* Ek açıklamada depolama sınıfı belirtildi ve talep bir disk isteyen *5GB* boyutu *ReadWriteOnce* erişim.

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

Kalıcı hacim taleple oluşturma [kubectl uygulamak] [ kubectl-apply] komut ve belirtin, *azure premium.yaml* dosyası:

```console
kubectl apply -f azure-premium.yaml
```

## <a name="use-the-persistent-volume"></a>Kalıcı hacim kullanın

Sonra kalıcı hacim talep oluşturuldu ve başarıyla kaynak sağlandı, disk disk erişimi olan bir pod oluşturulabilir. Kalıcı hacim talep kullanan bir pod aşağıdaki bildirimi oluşturur *azure yönetilen diski* Azure disk bağlamak için `/mnt/azure` yolu.

Adlı bir dosya oluşturun `azure-pvc-disk.yaml`, aşağıdaki bildirim kopyalayın.

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: mypod
spec:
  containers:
    - name: myfrontend
      image: nginx
      volumeMounts:
      - mountPath: "/mnt/azure"
        name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azure-managed-disk
```

Pod ile oluşturma [kubectl uygulamak] [ kubectl-apply] komutu.

```console
kubectl apply -f azure-pvc-disk.yaml
```

Artık takılabilir diskinizi Azure ile çalışan bir pod sahip `/mnt/azure` dizin. Bu yapılandırma ile pod incelenirken görülebilir `kubectl describe pod mypod`.

## <a name="next-steps"></a>Sonraki adımlar

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
[premium-storage]: ../virtual-machines/windows/premium-storage.md
