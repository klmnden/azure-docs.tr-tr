---
title: Azure dosya AKS ile kullanma
description: Azure diskleri AKS ile kullanma
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 03/06/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: a5126bc4c5e7c9cd9832f33fc908e6c8b9e02b91
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="persistent-volumes-with-azure-files"></a>Azure dosyaları ile kalıcı birimleri

Kalıcı bir birim Kubernetes kümesinde kullanmak için sağlanan depolama parçasını temsil eder. Kalıcı bir birim bir veya daha çok pod'ları tarafından kullanılabilir ve dinamik veya statik olarak sağlanabilir. Bu belgede bir AKS kümesindeki Kubernetes kalıcı birim olarak dinamik bir Azure dosya paylaşımı sağlama ayrıntıları verilmektedir. 

Kubernetes kalıcı birimler hakkında daha fazla bilgi için bkz: [Kubernetes kalıcı birimler][kubernetes-volumes].

## <a name="create-storage-account"></a>Depolama hesabı oluştur

AKS kümesi ile aynı kaynak grubunda bulunan sürece Kubernetes birimi olarak Azure dosya paylaşımının dinamik olarak sağlanırken, herhangi bir depolama hesabı kullanılabilir. Gerekirse, AKS kümesi ile aynı kaynak grubunda bir depolama hesabı oluşturun. 

Doğru kaynak grubunu tanımlamak için kullanmak [az grup listesi] [ az-group-list] komutu.

```azurecli-interactive
az group list --output table
```

Benzer şekilde bir ada sahip bir kaynak grubu arıyorsunuz `MC_clustername_clustername_locaton`, burada clustername AKS kümenizin adıdır ve konum burada küme dağıtılan Azure bölgesi.

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

Depolama sınıfı oluşturmak [kubectl oluşturma] [ kubectl-create] komutu.

```azurecli-interactive
kubectl create -f azure-file-sc.yaml
```

## <a name="create-persistent-volume-claim"></a>Kalıcı birim oluşturma

Kalıcı birim talep (PVC) depolama sınıf nesnesi Azure dosya paylaşımının dinamik olarak sağlamak için kullanır. 

Aşağıdaki bildirim kalıcı birim talep oluşturmak için kullanılan `5GB` boyutta `ReadWriteOnce` erişim.

Adlı bir dosya oluşturun `azure-file-pvc.yaml` ve aşağıdaki bildiriminde kopyalayın. Olduğundan emin olun `storageClassName` son adımda oluşturduğunuz depolama sınıfı eşleşir.

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

Kalıcı birim taleple oluşturma [kubectl oluşturma] [ kubectl-create] komutu.

```azurecli-interactive
kubectl create -f azure-file-pvc.yaml
```

Tamamlandığında, dosya paylaşımı oluşturulur. Kubernetes gizli bağlantı bilgilerini ve kimlik bilgilerini içeren da oluşturulur.

## <a name="using-the-persistent-volume"></a>Kalıcı birim kullanma

Aşağıdaki bildirimi kalıcı birim talep kullanan bir pod oluşturur `azurefile` adresindeki Azure dosya paylaşımı bağlamak için `/mnt/azure` yolu.

Adlı bir dosya oluşturun `azure-pvc-files.yaml`ve aşağıdaki bildiriminde kopyalayın. Olduğundan emin olun `claimName` son adımda oluşturulan PVC ile eşleşir.

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

İle pod oluşturma [kubectl oluşturma] [ kubectl-create] komutu.

```azurecli-interactive
kubectl create -f azure-pvc-files.yaml
```

Şimdi takılabilir diskinizin Azure ile çalışan bir pod sahip `/mnt/azure` dizin. Birimi, pod aracılığıyla incelerken bağlama görebilirsiniz `kubectl describe pod mypod`.

## <a name="next-steps"></a>Sonraki adımlar

Azure dosyaları kullanarak Kubernetes kalıcı birimleri hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Azure dosyaları için Kubernetes eklentisi][kubernetes-files]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
[kubectl-create]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#create
[kubectl-describe]: https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_describe/
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-security-context]: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/#azure-file
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az_group_create
[az-group-list]: /cli/azure/group#az_group_list
[az-storage-account-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-key-list]: /cli/azure/storage/account/keys#az_storage_account_keys_list
[az-storage-share-create]: /cli/azure/storage/share#az_storage_share_create
