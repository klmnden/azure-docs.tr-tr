---
title: Azure Disk AKS ile kullanma
description: Azure diskleri AKS ile kullanma
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 1/25/2018
ms.author: nepeters
ms.openlocfilehash: aa89cf9fe4e2cd5b63017558e89401de86effdc9
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2018
---
# <a name="persistent-volumes-with-azure-disks"></a>Azure diskleri kalıcı birimler

Kalıcı bir birim Kubernetes kümesinde kullanmak için sağlanan depolama parçasını temsil eder. Kalıcı bir birim bir veya daha çok pod'ları tarafından kullanılabilir ve dinamik veya statik olarak sağlanabilir. Bu belgede bir AKS kümesindeki Kubernetes kalıcı birim olarak dinamik bir Azure diski sağlama ayrıntıları verilmektedir. 

Kubernetes kalıcı birimler hakkında daha fazla bilgi için bkz: [Kubernetes kalıcı birimler][kubernetes-volumes].

## <a name="built-in-storage-classes"></a>Depolama sınıfları yerleşik

Depolama sınıfı dinamik olarak oluşturulan bir kalıcı birimi nasıl yapılandırıldığını tanımlamak için kullanılır. Kubernetes depolama sınıfları hakkında daha fazla bilgi için bkz: [Kubernetes depolama sınıfları][kubernetes-storage-classes].

Hem Azure diskler ile çalışmak için yapılandırılmış her AKS küme iki önceden oluşturulmuş depolama sınıfları içerir. Kullanım `kubectl get storageclass` bu görmek için komutu.

```console
NAME                PROVISIONER                AGE
default (default)   kubernetes.io/azure-disk   1h
managed-premium     kubernetes.io/azure-disk   1h
```

Bu depolama sınıfları ihtiyaçlarınız için çalışıyorsanız, yeni bir tane oluşturmak gerekmez.

## <a name="create-storage-class"></a>Depolama sınıfı oluşturma

Azure diskleri için yapılandırılan yeni bir depolama sınıfı oluşturmak istiyorsanız, bunu aşağıdaki örnek bildirimi kullanarak yapabilirsiniz. 

`storageaccounttype` Değerini `Standard_LRS` standart bir disk oluşturulduğunu gösterir. Bu değer değiştirilebilir `Premium_LRS` oluşturmak için bir [premium disk][premium-storage]. Premium diskleri kullanmak için AKS düğümü sanal makine boyutu premium diskler ile uyumlu olmalıdır. Bkz: [bu belgeyi] [ premium-storage] uyumlu boyutları listesi.

```yaml
apiVersion: storage.k8s.io/v1beta1
kind: StorageClass
metadata:
  name: azure-managed-disk
provisioner: kubernetes.io/azure-disk
parameters:
  kind: Managed
  storageaccounttype: Standard_LRS
```

## <a name="create-persistent-volume-claim"></a>Kalıcı birim oluşturma

Kalıcı birim talep depolama sınıfı nesnesi dinamik olarak bir depolama sağlamak için kullanır. Azure disk kullanırken, disk AKS kaynaklar ile aynı kaynak grubunda oluşturulur.

Bu örnek bildirimi kalıcı birimi kullanılarak talep oluşturur `azure-managed-disk` bir disk oluşturmak için depolama sınıfı `5GB` boyutta `ReadWriteOnce` erişim. PVC erişim modları hakkında daha fazla bilgi için bkz: [erişim modları][access-modes].

> [!NOTE]
> Azure disk yalnızca tek bir AKS düğüm için kullanılabilir hale getirir ReadWriteOnce erişim modu türüyle bağlanabilir. Kalıcı bir birim birden çok düğüm arasında paylaşmak gerek kullanmayı [Azure dosyaları][azure-files-pvc]. 

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azure-managed-disk
  annotations:
    volume.beta.kubernetes.io/storage-class: azure-managed-disk
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
```

## <a name="using-the-persistent-volume"></a>Kalıcı birim kullanma

Sonra kalıcı birim talep oluşturulan ve başarıyla sağlanan bir disk disk erişimi olan bir pod oluşturulabilir. Aşağıdaki bildirimi kalıcı birim talep kullanan bir pod oluşturur `azure-managed-disk` adresindeki Azure diski `/var/www/html` yolu. 

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
      - mountPath: "/var/www/html"
        name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azure-managed-disk
```

## <a name="next-steps"></a>Sonraki adımlar

Azure diskleri kullanarak Kubernetes kalıcı birimleri hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Azure diskleri için Kubernetes eklentisi][kubernetes-disk]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
[kubernetes-disk]: https://kubernetes.io/docs/concepts/storage/storage-classes/#new-azure-disk-storage-class-starting-from-v172
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/

<!-- LINKS - internal -->
[azure-files-pvc]: azure-files-dynamic-pv.md
[premium-storage]: ../virtual-machines/windows/premium-storage.md