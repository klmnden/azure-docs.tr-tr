---
title: Dinamik olarak Azure Kubernetes Service (AKS) için birden çok podunuz dosyaları birim oluşturma
description: Dinamik olarak eşzamanlı birden çok podunuz Azure Kubernetes Service (AKS) ile kullanmak için Azure dosyaları ile kalıcı hacim oluşturmayı öğrenin
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 10/08/2018
ms.author: iainfou
ms.openlocfilehash: 022ffeaf75f8f03447b931ed9c3a474286a17f89
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49067814"
---
# <a name="dynamically-create-and-use-a-persistent-volume-with-azure-files-in-azure-kubernetes-service-aks"></a>Dinamik olarak oluşturabilen ve Azure dosyaları Azure Kubernetes Service (AKS) ile kalıcı hacim kullanma

Kalıcı hacim Kubernetes pod'ları ile kullanmak için sağlanan depolama parçasını temsil eder. Kalıcı hacim, bir veya daha çok pod'ları tarafından kullanılabilir ve dinamik veya statik olarak sağlanabilir. Birden çok pod'ların aynı depolama birimine eş zamanlı erişim gerekiyorsa, Azure dosyaları kullanarak bağlanmak için kullanabileceğiniz [sunucu ileti bloğu (SMB) Protokolü][smb-overview]. Bu makalede, dinamik olarak bir Azure Kubernetes Service (AKS) kümesi içinde birden çok podunuz tarafından kullanılmak üzere bir Azure dosya paylaşımı oluşturma işlemini gösterir.

Kubernetes sürekli birimleri hakkında daha fazla bilgi için bkz. [Kubernetes kalıcı birimler][kubernetes-volumes].

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, var olan bir AKS kümesi olduğunu varsayar. AKS hızlı bir AKS kümesi gerekirse bkz [Azure CLI kullanarak] [ aks-quickstart-cli] veya [Azure portalını kullanarak][aks-quickstart-portal].

Ayrıca Azure CLI Sürüm 2.0.46 gerekir veya daha sonra yüklü ve yapılandırılmış. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][install-azure-cli].

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Kubernetes birimi olarak Azure dosyaları paylaşımına dinamik olarak oluşturduğunuzda, AKS içinde olduğu sürece herhangi bir depolama hesabı kullanılabilir **düğüm** kaynak grubu. Bu grubun sahip olduğu *MC_* AKS kümesi için kaynakları sağlama tarafından oluşturulan ön eki. Kaynak grubu adını alın [az aks show] [ az-aks-show] komutu.

```azurecli
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Kullanım [az depolama hesabı oluşturma] [ az-storage-account-create] depolama hesabı oluşturmak için komutu.

Güncelleştirme `--resource-group` toplanan son adımda, kaynak grubu adını ve `--name` tercih ettiğiniz bir adı. Kendi benzersiz depolama hesabı adı girin:

```azurecli
az storage account create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name mystorageaccount --sku Standard_LRS
```

> [!NOTE]
> Azure, şu anda yalnızca standart depolama ile çalışma dosyaları. Premium depolama kullanırsanız, birim sağlamak başarısız olur.

## <a name="create-a-storage-class"></a>Bir depolama sınıfı oluşturma

Bir depolama sınıfı, bir Azure dosya paylaşımı nasıl oluşturulduğunu tanımlamak için kullanılır. Sınıfı, bir depolama hesabı belirtilebilir. Bir depolama hesabı belirtilmemişse bir *skuName* ve *konumu* belirtilmelidir ve ilişkili kaynak grubundaki tüm depolama hesapları için bir eşleşme olarak değerlendirilir. Azure dosyaları için Kubernetes depolama sınıfları hakkında daha fazla bilgi için bkz. [Kubernetes depolama sınıfları][kubernetes-storage-classes].

Adlı bir dosya oluşturun `azure-file-sc.yaml` ve aşağıdaki örnek bildirimde kopyalayın. Güncelleştirme *storageAccount* değeri önceki adımda oluşturduğunuz depolama hesabınızın adıyla. Daha fazla bilgi için *mountOptions*, bkz: [bağlama seçenekleri] [ mount-options] bölümü.

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
  storageAccount: mystorageaccount
```

Depolama sınıfı ile oluşturma [kubectl uygulamak] [ kubectl-apply] komutu:

```console
kubectl apply -f azure-file-sc.yaml
```

## <a name="create-a-cluster-role-and-binding"></a>Bir küme rolünü ve bağlama oluşturma

AKS kümeleri gerçekleştirilebilir sınırı eylemlerine Kubernetes rol tabanlı erişim denetimi (RBAC) kullanın. *Rolleri* vermek için izinleri tanımlamanıza ve *bağlamaları* bunları istediğiniz kullanıcılar için geçerlidir. Bu atamaları, tüm küme üzerinde veya belirtilen bir ad alanı için uygulanabilir. Daha fazla bilgi için [kullanarak RBAC yetkilendirme][kubernetes-rbac].

Gerekli depolama kaynakları oluşturmak üzere Azure platformunun izin vermek için oluşturduğunuz bir *ClusterRole* ve *ClusterRoleBinding*. Adlı bir dosya oluşturun `azure-pvc-roles.yaml` aşağıdaki YAML'ye kopyalayın:

```yaml
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: system:azure-cloud-provider
rules:
- apiGroups: ['']
  resources: ['secrets']
  verbs:     ['get','create']
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: system:azure-cloud-provider
roleRef:
  kind: ClusterRole
  apiGroup: rbac.authorization.k8s.io
  name: system:azure-cloud-provider
subjects:
- kind: ServiceAccount
  name: persistent-volume-binder
  namespace: kube-system
```

Atama izinleri olan [kubectl uygulamak] [ kubectl-apply] komutu:

```console
kubectl apply -f azure-pvc-roles.yaml
```

## <a name="create-a-persistent-volume-claim"></a>Kalıcı hacim talep oluşturma

Kalıcı hacim talep (PVC), dinamik olarak Azure dosya paylaşımını sağlamak için depolama sınıfı nesnesini kullanır. Kalıcı hacim talep oluşturmak için aşağıdaki YAML kullanılabilir *5GB* boyutu *ReadWriteMany* erişim. Erişim modları hakkında daha fazla bilgi için bkz. [Kubernetes kalıcı hacim] [ access-modes] belgeleri.

Artık adlı bir dosya oluşturun `azure-file-pvc.yaml` aşağıdaki YAML'ye kopyalayın. Emin olun *storageClassName* son adımda oluşturduğunuz depolama sınıfı ile eşleşen:

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

Kalıcı hacim taleple oluşturma [kubectl uygulamak] [ kubectl-apply] komutu:

```console
kubectl apply -f azure-file-pvc.yaml
```

Tamamlandığında, dosya paylaşımı oluşturulur. Kubernetes gizli bağlantı bilgilerini ve kimlik bilgilerini içeren da oluşturulur. Kullanabileceğiniz [kubectl alma] [ kubectl-get] PVC durumunu görüntülemek için komut:

```
$ kubectl get pvc azurefile

NAME        STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
azurefile   Bound     pvc-8436e62e-a0d9-11e5-8521-5a8664dc0477   5Gi        RWX            azurefile      5m
```

## <a name="use-the-persistent-volume"></a>Kalıcı hacim kullanın

Kalıcı hacim talep kullanan bir pod aşağıdaki YAML oluşturur *azurefile* Azure dosya paylaşımını bağlayabilmeniz için */mnt/azure* yolu.

Adlı bir dosya oluşturun `azure-pvc-files.yaml`, aşağıdaki YAML'ye kopyalayın. Emin olun *claimName* son adımda oluşturduğunuz PVC ile eşleşir.

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
        claimName: azurefile
```

Pod ile oluşturma [kubectl uygulamak] [ kubectl-apply] komutu.

```console
kubectl apply -f azure-pvc-files.yaml
```

Artık takılabilir diskinizi Azure ile çalışan bir pod sahip */mnt/azure* dizin. Bu yapılandırma ile pod incelenirken görülebilir `kubectl describe pod mypod`. Aşağıdaki sıkıştırılmış örneğe çıktı kapsayıcıda takılı birim gösterir:

```
Containers:
  mypod:
    Container ID:   docker://053bc9c0df72232d755aa040bfba8b533fa696b123876108dec400e364d2523e
    Image:          nginx:1.15.5
    Image ID:       docker-pullable://nginx@sha256:d85914d547a6c92faa39ce7058bd7529baacab7e0cd4255442b04577c4d1f424
    State:          Running
      Started:      Wed, 15 Aug 2018 22:22:27 +0000
    Ready:          True
    Mounts:
      /mnt/azure from volume (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-8rv4z (ro)
[...]
Volumes:
  volume:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  azurefile
    ReadOnly:   false
[...]
```

## <a name="mount-options"></a>Bağlama seçenekleri

Varsayılan *fileMode* ve *dirMode* değerler aşağıdaki tabloda açıklandığı gibi Kubernetes sürümleri arasında farklılık gösterir.

| version | değer |
| ---- | ---- |
| v1.6.x, v1.7.x | 0777 |
| v1.8.0-v1.8.5 | 0700 |
| V1.8.6 veya üzeri | 0755 |
| V1.9.0 | 0700 |
| V1.9.1 veya üzeri | 0755 |

Bir küme 1.8.5 sürümü kullanıyorsanız veya daha büyük ve dinamik olarak kalıcı hacim, bir depolama sınıfı ile oluşturma, bağlama seçeneklerini depolama sınıfı nesne üzerinde belirtilebilir. Aşağıdaki örnek kümeleri *0777*:

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

Bir küme 1.8.5 sürümü kullanıyorsanız veya daha büyük ve statik olarak kalıcı hacim nesne oluşturma, bağlama seçeneklerini üzerinde belirtilmesine gerek *PersistentVolume* nesne. statik olarak kalıcı bir birim oluşturma hakkında daha fazla bilgi için bkz. [statik kalıcı birimler][pv-static].

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

Sürüm 1.8.0 - 1.8.4, kümesi kullanılıyorsa bir güvenlik bağlamı ile belirtilebilir *farklıkullanıcı* değerine *0*. Pod güvenlik bağlamı hakkında daha fazla bilgi için bkz. [bir güvenlik bağlamı yapılandırma][kubernetes-security-context].

## <a name="next-steps"></a>Sonraki adımlar

Azure dosyaları'nı kullanarak Kubernetes kalıcı birimleri hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Azure dosyaları için Kubernetes eklentisi][kubernetes-files]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-security-context]: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/#azure-file
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/
[pv-static]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#static
[kubernetes-rbac]: https://kubernetes.io/docs/reference/access-authn-authz/rbac/
[smb-overview]: /windows/desktop/FileIO/microsoft-smb-protocol-and-cifs-protocol-overview

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-group-list]: /cli/azure/group#az-group-list
[az-resource-show]: /cli/azure/aks#az-aks-show
[az-storage-account-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-key-list]: /cli/azure/storage/account/keys#az-storage-account-keys-list
[az-storage-share-create]: /cli/azure/storage/share#az-storage-share-create
[mount-options]: #mount-options
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
