---
title: Azure dosya AKS ile kullanma
description: Azure diskleri AKS ile kullanma
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 1/04/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: ce37cfdd70f95822a912f6ea71b9e4a3f9a30a14
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2018
---
# <a name="persistent-volumes-with-azure-files"></a>Azure dosyaları ile kalıcı birimleri

Kalıcı bir birim Kubernetes kümesinde kullanmak için sağlanan depolama parçasını temsil eder. Kalıcı bir birim bir veya daha çok pod'ları tarafından kullanılabilir ve dinamik veya statik olarak sağlanabilir. Bu belgede bir AKS kümesindeki Kubernetes kalıcı birim olarak dinamik bir Azure dosya paylaşımı sağlama ayrıntıları verilmektedir. 

Kubernetes kalıcı birimler hakkında daha fazla bilgi için bkz: [Kubernetes kalıcı birimler][kubernetes-volumes].

## <a name="prerequisites"></a>Önkoşullar

AKS kümesi ile aynı kaynak grubunda bulunan sürece Kubernetes birimi olarak Azure dosya paylaşımının dinamik olarak sağlanırken, herhangi bir depolama hesabı kullanılabilir. Gerekirse, AKS kümesi ile aynı kaynak grubunda bir depolama hesabı oluşturun. 

Doğru kaynak grubunu tanımlamak için kullanmak [az grup listesi] [ az-group-list] komutu.

```azurecli-interactive
az group list --output table
```

Aşağıdaki örnek çıkış, her ikisi de AKS kümesi ile ilişkili kaynak gruplarını gösterir. Kaynak grubu adıyla ister *MC_myAKSCluster_myAKSCluster_eastus* AKS küme kaynaklarını içeren ve burada depolama hesabının oluşturulması gerekir. 

```
Name                                 Location    Status
-----------------------------------  ----------  ---------
MC_myAKSCluster_myAKSCluster_eastus  eastus      Succeeded
myAKSCluster                         eastus      Succeeded
```

Kaynak grubu belirlendikten sonra depolama hesabıyla oluşturma [az depolama hesabı oluşturma] [ az-storage-account-create] komutu.

```azurecli-interactive
az storage account create --resource-group  MC_myAKSCluster_myAKSCluster_eastus --name mystorageaccount --location eastus --sku Standard_LRS
```

## <a name="create-storage-class"></a>Depolama sınıfı oluşturma

Depolama sınıfı dinamik olarak oluşturulan bir kalıcı birimi nasıl yapılandırıldığını tanımlamak için kullanılır. Azure depolama hesabı adı, SKU ve bölge gibi öğeleri depolama sınıf nesnesinde tanımlanır. Kubernetes depolama sınıfları hakkında daha fazla bilgi için bkz: [Kubernetes depolama sınıfları][kubernetes-storage-classes].

Aşağıdaki örnek sku'sunun herhangi bir depolama hesabı türü belirtir `Standard_LRS` içinde `eastus` bölge depolama isterken kullanılabilir. 

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
parameters:
  skuName: Standard_LRS
```

Belirli bir depolama hesabı kullanmak üzere `storageAccount` parametresi kullanılabilir.

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
parameters:
  storageAccount: azure_storage_account_name
```

## <a name="create-persistent-volume-claim"></a>Kalıcı birim oluşturma

Kalıcı birim talep depolama sınıf nesnesi dinamik olarak bir depolama sağlamak için kullanır. Azure dosyaları kullanırken, Azure dosya paylaşımının seçilen veya depolama sınıf nesnesinde belirtilen depolama hesabı oluşturulur.

>  [!NOTE]
>   Uygun depolama hesabı AKS küme kaynakları ile aynı kaynak grubunda önceden oluşturulmuş olduğundan emin olun. Bu kaynak grubu gibi bir ada sahip *MC_myAKSCluster_myAKSCluster_eastus*. Kalıcı birim talep için Azure dosya paylaşımı bir depolama hesabı yoksa sağlama başarısız olur. 

Aşağıdaki bildirim kalıcı birim talep oluşturmak için kullanılan `5GB` boyutta `ReadWriteOnce` erişim. PVC erişim modları hakkında daha fazla bilgi için bkz: [erişim modları][access-modes].

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

## <a name="using-the-persistent-volume"></a>Kalıcı birim kullanma

Sonra kalıcı birim talep oluşturulan ve birime erişimi bir pod başarıyla kaynak sağlandı depolama alanının oluşturulabilir. Aşağıdaki bildirimi kalıcı birim talep kullanan bir pod oluşturur `azurefile` adresindeki Azure dosya paylaşımı bağlamak için `/var/www/html` yolu. 

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
        claimName: azurefile
```

## <a name="mount-options"></a>Bağlama seçenekleri

Varsayılan fileMode ve dirMode değerler aşağıdaki tabloda açıklandığı gibi Kubernetes sürümleri arasında farklılık gösterir. 

| sürüm | değer |
| ---- | ---- |
| v1.6.x, v1.7.x | 0777 |
| v1.8.0-v1.8.5 | 0700 |
| V1.8.6 veya üstü | 0755 |
| v1.9.0 | 0700 |
| V1.9.1 veya üstü | 0755 |

Sürüm 1.8.5 oluşan bir küme kullanıyorsanız veya büyük, bağlama seçenekleri depolama sınıfı nesnesinde belirtilebilir. Aşağıdaki örnek kümeleri `0777`. 

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

Sürüm 1.8.0 - 1.8.4, oluşan bir küme kullanıyorsanız, bir güvenlik bağlamı ile belirtilebilir `runAsUser` değerine `0`. Pod güvenlik bağlamı ile ilgili daha fazla bilgi için bkz: [bir güvenlik bağlamı yapılandırma][kubernetes-security-context].

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
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az_group_create
[az-group-list]: /cli/azure/group#az_group_list
[az-storage-account-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-key-list]: /cli/azure/storage/account/keys#az_storage_account_keys_list
[az-storage-share-create]: /cli/azure/storage/share#az_storage_share_create