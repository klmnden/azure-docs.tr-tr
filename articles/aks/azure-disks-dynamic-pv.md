---
title: Azure Disk AKS ile kullanma
description: Azure diskleri AKS ile kullanma
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 03/06/2018
ms.author: nepeters
ms.openlocfilehash: 858961db439b28a71d3475d2608073287e02f2fd
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="persistent-volumes-with-azure-disks"></a>Azure diskleri kalıcı birimler

Kalıcı bir birim Kubernetes pod'ları ile kullanılmak üzere sağlanan depolama parçasını temsil eder. Kalıcı bir birim bir veya daha çok pod'ları tarafından kullanılabilir ve dinamik veya statik olarak sağlanabilir. Kubernetes kalıcı birimler hakkında daha fazla bilgi için bkz: [Kubernetes kalıcı birimler][kubernetes-volumes].

Bir Azure Kubernetes hizmet (AKS) kümesindeki Azure diskleri kalıcı birimler kullanarak bu belge ayrıntıları.

> [!NOTE]
> Azure disk yalnızca tek bir AKS düğüm için kullanılabilir hale getirir ReadWriteOnce erişim modu türüyle bağlanabilir. Kalıcı bir birim birden çok düğüm arasında paylaşmak gerek kullanmayı [Azure dosyaları][azure-files-pvc].

## <a name="built-in-storage-classes"></a>Depolama sınıfları yerleşik

Depolama sınıfı, depolama birimi olan kalıcı bir birim dinamik olarak nasıl oluşturulacağını tanımlamak için kullanılır. Kubernetes depolama sınıfları hakkında daha fazla bilgi için bkz: [Kubernetes depolama sınıfları][kubernetes-storage-classes].

Hem Azure diskler ile çalışmak için yapılandırılmış her AKS küme iki önceden oluşturulmuş depolama sınıfları içerir. `default` Standart bir Azure disk depolama sınıfı sağlar. `managed-premium` Premium Azure disk depolama sınıfı sağlar. Premium depolama kümenizdeki AKS düğümlerin kullanırsanız, seçin `managed-premium` sınıfı.

Kullanım [kubectl alma sc] [ kubectl-get] önceden oluşturulmuş depolama sınıfları görmek için komutu.

```console
NAME                PROVISIONER                AGE
default (default)   kubernetes.io/azure-disk   1h
managed-premium     kubernetes.io/azure-disk   1h
```

## <a name="create-persistent-volume-claim"></a>Kalıcı birim oluşturma

Kalıcı birim talep (PVC) otomatik olarak bir depolama sınıfına dayalı depolama sağlamak için kullanılır. Bu durumda, PVC önceden oluşturulmuş depolama sınıflarından standart veya premium Azure oluşturmak için kullanabilirsiniz yönetilen disk.

Adlı bir dosya oluşturun `azure-premimum.yaml`ve aşağıdaki bildiriminde kopyalayın.

Not alın `managed-premium` depolama sınıfı açıklama içinde belirtilen ve talep bir disk isteyen `5GB` boyutta `ReadWriteOnce` erişim.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azure-managed-disk
  annotations:
    volume.beta.kubernetes.io/storage-class: managed-premium
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
```

Kalıcı birim taleple oluşturma [kubectl uygulamak] [ kubectl-apply] komutu.

```azurecli-interactive
kubectl apply -f azure-premimum.yaml
```

## <a name="using-the-persistent-volume"></a>Kalıcı birim kullanma

Sonra kalıcı birim talep oluşturulan ve başarıyla sağlanan bir disk disk erişimi olan bir pod oluşturulabilir. Aşağıdaki bildirimi kalıcı birim talep kullanan bir pod oluşturur `azure-managed-disk` adresindeki Azure diski `/mnt/azure` yolu.

Adlı bir dosya oluşturun `azure-pvc-disk.yaml`ve aşağıdaki bildiriminde kopyalayın.

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

İle pod oluşturma [kubectl uygulamak] [ kubectl-apply] komutu.

```azurecli-interactive
kubectl apply -f azure-pvc-disk.yaml
```

Şimdi takılabilir diskinizin Azure ile çalışan bir pod sahip `/mnt/azure` dizin. Bu yapılandırma, pod aracılığıyla incelerken görülebilir `kubectl describe pod mypod`.

## <a name="next-steps"></a>Sonraki adımlar

Azure diskleri kullanarak Kubernetes kalıcı birimleri hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Azure diskleri için Kubernetes eklentisi][kubernetes-disk]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-disk]: https://kubernetes.io/docs/concepts/storage/storage-classes/#new-azure-disk-storage-class-starting-from-v172
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/

<!-- LINKS - internal -->
[azure-files-pvc]: azure-files-dynamic-pv.md
[premium-storage]: ../virtual-machines/windows/premium-storage.md