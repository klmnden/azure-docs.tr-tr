---
title: Azure dosya AKS ile kullanma
description: AKS ile Azure disklerini kullanma
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/21/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: bac80e354a4d629bfbfc8207b9fed7c69c765839
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39429276"
---
# <a name="persistent-volumes-with-azure-files"></a>Azure dosyaları ile kalıcı birimleri

Kalıcı hacim, bir Kubernetes kümesinde kullanmak için oluşturulan depolama parçasıdır. Kalıcı hacim, bir veya daha çok pod'ları tarafından kullanılabilir ve dinamik veya statik olarak oluşturulabilir. Bu belge ayrıntıları **dinamik oluşturma** kalıcı bir birim olarak bir Azure dosya paylaşımının.

Kubernetes hakkında daha fazla bilgi için bkz: kalıcı birimleri statik oluşturma dahil, [Kubernetes kalıcı birimler][kubernetes-volumes].

## <a name="create-storage-account"></a>Depolama hesabı oluştur

Dinamik olarak Kubernetes birimi olarak Azure dosya paylaşımı oluştururken, AKS içinde olduğu sürece herhangi bir depolama hesabı kullanılabilir **düğüm** kaynak grubu. Biriyle budur `MC_` AKS kümesi için kaynakları sağlama tarafından oluşturulan ön eki. Kaynak grubu adını alın [az resource show] [ az-resource-show] komutu.

```azurecli-interactive
$ az resource show --resource-group myResourceGroup --name myAKSCluster --resource-type Microsoft.ContainerService/managedClusters --query properties.nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Kullanım [az depolama hesabı oluşturma] [ az-storage-account-create] depolama hesabı oluşturmak için komutu.

Güncelleştirme `--resource-group` toplanan son adımda, kaynak grubu adını ve `--name` tercih ettiğiniz bir adı.

```azurecli-interactive
az storage account create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name mystorageaccount --location eastus --sku Standard_LRS
```

> Azure dosyaları şu anda yalnızca standart depolama ile çalışır. Premium depolama kullanırsanız, toplu sağlamak başarısız olur.

## <a name="create-storage-class"></a>Depolama sınıfı oluşturma

Bir depolama sınıfı, bir Azure dosya paylaşımı nasıl oluşturulduğunu tanımlamak için kullanılır. Sınıfı, belirli bir depolama hesabına belirtilebilir. Bir depolama hesabı belirtilmemişse bir `skuName` ve `location` belirtilmelidir ve ilişkili kaynak grubundaki tüm depolama hesapları için bir eşleşme olarak değerlendirilir.

Kubernetes Azure dosyaları için depolama sınıfları hakkında daha fazla bilgi için bkz. [Kubernetes depolama sınıfları][kubernetes-storage-classes].

Adlı bir dosya oluşturun `azure-file-sc.yaml` aşağıdaki bildirimi kopyalayın. Güncelleştirme `storageAccount` hedef depolama hesabınızın adıyla. Daha fazla bilgi için [bağlama seçeneklerini] bölümüne `mountOptions`.

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

Depolama sınıfı ile oluşturma [kubectl uygulamak] [ kubectl-apply] komutu.

```azurecli-interactive
kubectl apply -f azure-file-sc.yaml
```

## <a name="create-persistent-volume-claim"></a>Kalıcı hacim talep oluşturma

Kalıcı hacim talep (PVC), dinamik olarak Azure dosya paylaşımını sağlamak için depolama sınıfı nesnesini kullanır.

Kalıcı hacim talep oluşturmak için aşağıdaki YAML kullanılabilir `5GB` boyutu `ReadWriteMany` erişim. Erişim modları hakkında daha fazla bilgi için bkz. [Kubernetes kalıcı hacim] [ access-modes] belgeleri.

Adlı bir dosya oluşturun `azure-file-pvc.yaml` aşağıdaki YAML'ye kopyalayın. Emin olun `storageClassName` son adımda oluşturduğunuz depolama sınıfı ile eşleşir.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azurefile
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: azurefile
  resources:
    requests:
      storage: 5Gi
```

Kalıcı hacim taleple oluşturma [kubectl uygulamak] [ kubectl-apply] komutu.

```azurecli-interactive
kubectl apply -f azure-file-pvc.yaml
```

Tamamlandığında, dosya paylaşımı oluşturulur. Kubernetes gizli bağlantı bilgilerini ve kimlik bilgilerini içeren da oluşturulur.

## <a name="using-the-persistent-volume"></a>Kalıcı hacim kullanma

Kalıcı hacim talep kullanan bir pod aşağıdaki YAML oluşturur `azurefile` Azure dosya paylaşımını bağlayabilmeniz için `/mnt/azure` yolu.

Adlı bir dosya oluşturun `azure-pvc-files.yaml`, aşağıdaki YAML'ye kopyalayın. Emin olun `claimName` son adımda oluşturduğunuz PVC ile eşleşir.

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

Pod ile oluşturma [kubectl uygulamak] [ kubectl-apply] komutu.

```azurecli-interactive
kubectl apply -f azure-pvc-files.yaml
```

Artık takılabilir diskinizi Azure ile çalışan bir pod sahip `/mnt/azure` dizin. Bu yapılandırma ile pod incelenirken görülebilir `kubectl describe pod mypod`.

## <a name="mount-options"></a>Bağlama seçenekleri

Varsayılan fileMode ve dirMode değerler, aşağıdaki tabloda açıklandığı gibi Kubernetes sürümleri arasında farklılık gösterir.

| sürüm | değer |
| ---- | ---- |
| v1.6.x, v1.7.x | 0777 |
| v1.8.0-v1.8.5 | 0700 |
| V1.8.6 veya üzeri | 0755 |
| V1.9.0 | 0700 |
| V1.9.1 veya üzeri | 0755 |

Bir küme 1.8.5 sürümü kullanıyorsanız veya daha büyük ve dinamik olarak persistant birim bir depolama sınıfı ile oluşturma, bağlama seçeneklerini depolama sınıfı nesne üzerinde belirtilebilir. Aşağıdaki örnek kümeleri `0777`.

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

Bir küme 1.8.5 sürümü kullanıyorsanız veya daha büyük ve statik olarak persistant toplu nesne oluşturma, bağlama seçeneklerini üzerinde belirtilmesine gerek `PersistentVolume` nesne. statik olarak persistant birim oluşturma hakkında daha fazla bilgi için bkz. [statik kalıcı birimler][pv-static].

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

Sürüm 1.8.0 - 1.8.4, kümesi kullanılıyorsa bir güvenlik bağlamı ile belirtilebilir `runAsUser` değerine `0`. Pod güvenlik bağlamı hakkında daha fazla bilgi için bkz. [bir güvenlik bağlamı yapılandırma][kubernetes-security-context].

## <a name="next-steps"></a>Sonraki adımlar

Azure dosyaları'nı kullanarak Kubernetes kalıcı birimleri hakkında daha fazla bilgi edinin.

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
[az-group-create]: /cli/azure/group#az-group-create
[az-group-list]: /cli/azure/group#az-group-list
[az-resource-show]: /cli/azure/resource#az-resource-show
[az-storage-account-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-key-list]: /cli/azure/storage/account/keys#az-storage-account-keys-list
[az-storage-share-create]: /cli/azure/storage/share#az-storage-share-create
[mount-options]: #mount-options
