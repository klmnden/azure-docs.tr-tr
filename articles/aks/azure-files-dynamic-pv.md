---
title: Azure dosya AKS ile kullanma
description: Azure diskleri AKS ile kullanma
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/17/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 991db1fc32ae89ab04ca040cfb6e8d59ffe5262f
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="persistent-volumes-with-azure-files"></a>Azure dosyaları ile kalıcı birimleri

Kalıcı bir birim Kubernetes kümesinde kullanmak için oluşturulan depolama parçasıdır. Kalıcı bir birim dinamik veya statik olarak oluşturulabilir ve bir veya daha çok pod'ları tarafından kullanılabilir. Bu belge ayrıntıları **dinamik oluşturma** bir Azure dosya paylaşımının kalıcı bir birim olarak.

Kubernetes hakkında daha fazla bilgi için bkz: statik oluşturma dahil olmak üzere kalıcı birimler [Kubernetes kalıcı birimler][kubernetes-volumes].

## <a name="create-storage-account"></a>Depolama hesabı oluştur

AKS kümesi ile aynı kaynak grubunda olduğu sürece herhangi bir depolama hesabı dinamik olarak Kubernetes birimi olarak bir Azure dosya paylaşımı oluşturulurken kullanılabilir. Gerekirse, AKS kümesi ile aynı kaynak grubunda bir depolama hesabı oluşturun.

Doğru kaynak grubunu tanımlamak için kullanmak [az grup listesi] [ az-group-list] komutu.

```azurecli-interactive
az group list --output table
```

Benzer şekilde bir ada sahip bir kaynak grubu arayın `MC_clustername_clustername_locaton`.

```
Name                                 Location    Status
-----------------------------------  ----------  ---------
MC_myAKSCluster_myAKSCluster_eastus  eastus      Succeeded
myAKSCluster                         eastus      Succeeded
```

Kullanım [az depolama hesabı oluşturma] [ az-storage-account-create] depolama hesabını oluşturmak için komutu.

Bu örneği kullanarak, güncelleştirme `--resource-group` kaynak grubu adını ve `--name` için tercih ettiğiniz bir ad.

```azurecli-interactive
az storage account create --resource-group MC_myAKSCluster_myAKSCluster_eastus --name mystorageaccount --location eastus --sku Standard_LRS
```

## <a name="create-storage-class"></a>Depolama sınıfı oluşturma

Depolama sınıfı, Azure dosya paylaşımının nasıl oluşturulacağını tanımlamak için kullanılır. Belirli bir depolama hesabı sınıfında belirtilebilir. Bir depolama hesabı belirtilmezse, bir `skuName` ve `location` belirtilmesi gerekir ve ilişkili kaynak grubundaki tüm depolama hesapları için bir eşleşme olarak değerlendirilir.

Azure dosyaları için Kubernetes depolama sınıfları hakkında daha fazla bilgi için bkz: [Kubernetes depolama sınıfları][kubernetes-storage-classes].

Adlı bir dosya oluşturun `azure-file-sc.yaml` ve aşağıdaki bildiriminde kopyalayın. Güncelleştirme `storageAccount` hedef depolama hesabı adı.

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
parameters:
  storageAccount: mystorageaccount
```

Depolama sınıfı oluşturmak [kubectl uygulamak] [ kubectl-apply] komutu.

```azurecli-interactive
kubectl apply -f azure-file-sc.yaml
```

## <a name="create-persistent-volume-claim"></a>Kalıcı birim oluşturma

Kalıcı birim talep (PVC) depolama sınıf nesnesi Azure dosya paylaşımının dinamik olarak sağlamak için kullanır.

Aşağıdaki YAML kalıcı birim talep oluşturmak için kullanılan `5GB` boyutta `ReadWriteOnce` erişim. Erişim modları hakkında daha fazla bilgi için bkz: [Kubernetes kalıcı birim] [ access-modes] belgeleri.

Adlı bir dosya oluşturun `azure-file-pvc.yaml` ve aşağıdaki YAML kopyalayın. Olduğundan emin olun `storageClassName` son adımda oluşturduğunuz depolama sınıfı eşleşir.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azurefile
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: azurefile
  resources:
    requests:
      storage: 5Gi
```

Kalıcı birim taleple oluşturma [kubectl uygulamak] [ kubectl-apply] komutu.

```azurecli-interactive
kubectl apply -f azure-file-pvc.yaml
```

Tamamlandığında, dosya paylaşımı oluşturulur. Kubernetes gizli bağlantı bilgilerini ve kimlik bilgilerini içeren da oluşturulur.

## <a name="using-the-persistent-volume"></a>Kalıcı birim kullanma

Aşağıdaki YAML kalıcı birim talep kullanan bir pod oluşturur `azurefile` adresindeki Azure dosya paylaşımı bağlamak için `/mnt/azure` yolu.

Adlı bir dosya oluşturun `azure-pvc-files.yaml`ve aşağıdaki YAML kopyalayın. Olduğundan emin olun `claimName` son adımda oluşturulan PVC ile eşleşir.

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
        claimName: azurefile
```

İle pod oluşturma [kubectl uygulamak] [ kubectl-apply] komutu.

```azurecli-interactive
kubectl apply -f azure-pvc-files.yaml
```

Şimdi takılabilir diskinizin Azure ile çalışan bir pod sahip `/mnt/azure` dizin. Bu yapılandırma, pod aracılığıyla incelerken görülebilir `kubectl describe pod mypod`.

## <a name="mount-options"></a>Bağlama seçenekleri

Varsayılan fileMode ve dirMode değerler aşağıdaki tabloda açıklandığı gibi Kubernetes sürümleri arasında farklılık gösterir.

| sürüm | değer |
| ---- | ---- |
| v1.6.x, v1.7.x | 0777 |
| v1.8.0-v1.8.5 | 0700 |
| V1.8.6 veya üstü | 0755 |
| V1.9.0 | 0700 |
| V1.9.1 veya üstü | 0755 |

Sürüm 1.8.5 oluşan bir küme kullanıyorsanız veya daha büyük ve dinamik olarak persistant birimin depolama sınıfı ile oluşturma, bağlama seçenekleri depolama sınıfı nesnesinde belirtilebilir. Aşağıdaki örnek kümeleri `0777`.

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=1000
  - gid=1000
parameters:
  skuName: Standard_LRS
```

Sürüm 1.8.5 oluşan bir küme kullanıyorsanız veya daha büyük ve statik olarak persistant birim nesnesi oluşturma, bağlama seçenekleri üzerinde belirtilmesi gereken `PersistentVolume` nesnesi. statik olarak persistant birim oluşturma ile ilgili daha fazla bilgi için bkz: [statik kalıcı birimler][pv-static].

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: azurefile
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteMany
  azureFile:
    secretName: azure-secret
    shareName: azurefile
    readOnly: false
  mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=1000
  - gid=1000
  ```

Sürüm 1.8.0 - 1.8.4, oluşan bir küme kullanıyorsanız, bir güvenlik bağlamı ile belirtilebilir `runAsUser` değerine `0`. Pod güvenlik bağlamı ile ilgili daha fazla bilgi için bkz: [bir güvenlik bağlamı yapılandırma][kubernetes-security-context].

## <a name="next-steps"></a>Sonraki adımlar

Azure dosyaları kullanarak Kubernetes kalıcı birimleri hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Azure dosyaları için Kubernetes eklentisi][kubernetes-files]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-describe]: https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_describe/
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-security-context]: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/#azure-file
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/
[pv-static]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#static

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az_group_create
[az-group-list]: /cli/azure/group#az_group_list
[az-storage-account-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-key-list]: /cli/azure/storage/account/keys#az_storage_account_keys_list
[az-storage-share-create]: /cli/azure/storage/share#az_storage_share_create
