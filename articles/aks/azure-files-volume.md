---
title: Azure Kubernetes Service (AKS) için birden çok podunuz statik bir birim oluşturun
description: El ile birden çok eş zamanlı pod Azure Kubernetes Service (AKS) ile kullanmak için Azure dosyaları ile birim oluşturma hakkında bilgi edinin
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 03/01/2019
ms.author: mlearned
ms.openlocfilehash: ad80b738058b4048fa1a51144a37eb4f62b538c0
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67616034"
---
# <a name="manually-create-and-use-a-volume-with-azure-files-share-in-azure-kubernetes-service-aks"></a>El ile oluşturma ve Azure dosyaları paylaşımı Azure Kubernetes Service (AKS) ile bir birimi kullanın

Kapsayıcı tabanlı uygulamalar genellikle erişmek ve bir dış veri birimdeki veriler kalıcı hale getirmek gerekir. Birden çok pod'ların aynı depolama birimine eş zamanlı erişim gerekiyorsa, Azure dosyaları kullanarak bağlanmak için kullanabileceğiniz [sunucu ileti bloğu (SMB) Protokolü][smb-overview]. Bu makalede aks'deki bir pod ekleme ve el ile bir Azure dosya paylaşımı oluşturma gösterilmektedir.

Kubernetes birimleri hakkında daha fazla bilgi için bkz. [AKS uygulamalar için Depolama Seçenekleri][concepts-storage].

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, var olan bir AKS kümesi olduğunu varsayar. AKS hızlı bir AKS kümesi gerekirse bkz [Azure CLI kullanarak][aks-quickstart-cli] or [using the Azure portal][aks-quickstart-portal].

Ayrıca Azure CLI Sürüm 2.0.59 gerekir veya daha sonra yüklü ve yapılandırılmış. Çalıştırma `az --version` sürümü bulmak için. Gerekirse yüklemek veya yükseltmek bkz [Azure CLI yükleme][install-azure-cli].

## <a name="create-an-azure-file-share"></a>Azure dosya paylaşımı oluşturma

Kubernetes birimi olarak Azure dosyaları'nı kullanmadan önce bir Azure depolama hesabı ve dosya paylaşımı oluşturmanız gerekir. Aşağıdaki komutları adlı bir kaynak grubu oluşturma *myAKSShare*, bir depolama hesabı ve adlı bir dosya paylaşımı *aksshare*:

```azurecli-interactive
# Change these four parameters as needed for your own environment
AKS_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
AKS_PERS_RESOURCE_GROUP=myAKSShare
AKS_PERS_LOCATION=eastus
AKS_PERS_SHARE_NAME=aksshare

# Create a resource group
az group create --name $AKS_PERS_RESOURCE_GROUP --location $AKS_PERS_LOCATION

# Create a storage account
az storage account create -n $AKS_PERS_STORAGE_ACCOUNT_NAME -g $AKS_PERS_RESOURCE_GROUP -l $AKS_PERS_LOCATION --sku Standard_LRS

# Export the connection string as an environment variable, this is used when creating the Azure file share
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $AKS_PERS_STORAGE_ACCOUNT_NAME -g $AKS_PERS_RESOURCE_GROUP -o tsv`

# Create the file share
az storage share create -n $AKS_PERS_SHARE_NAME --connection-string $AZURE_STORAGE_CONNECTION_STRING

# Get storage account key
STORAGE_KEY=$(az storage account keys list --resource-group $AKS_PERS_RESOURCE_GROUP --account-name $AKS_PERS_STORAGE_ACCOUNT_NAME --query "[0].value" -o tsv)

# Echo storage account name and key
echo Storage account name: $AKS_PERS_STORAGE_ACCOUNT_NAME
echo Storage account key: $STORAGE_KEY
```

Depolama hesabı adını ve anahtarını betik çıktısı sonunda gösterilen bir not edin. Kubernetes hacmi aşağıdaki adımlardan birini oluşturduğunuzda bu değerlere gereklidir.

## <a name="create-a-kubernetes-secret"></a>Bir Kubernetes gizli dizisi oluşturma

Kubernetes önceki adımda oluşturduğunuz dosya paylaşımına erişmek için kimlik bilgileri gerekir. Bu kimlik bilgileri depolanan bir [Kubernetes gizli][kubernetes-secret], bir Kubernetes pod oluşturduğunuzda başvuruyor.

Kullanım `kubectl create secret` gizli dizi oluşturmak için komutu. Aşağıdaki örnek, paylaşılan adlı oluşturur *azure-secret* ve dolduran *azurestorageaccountname* ve *azurestorageaccountkey* önceki adımdan. Mevcut bir Azure depolama hesabını kullanmak için hesap adını ve anahtarını belirtin.

```console
kubectl create secret generic azure-secret --from-literal=azurestorageaccountname=$AKS_PERS_STORAGE_ACCOUNT_NAME --from-literal=azurestorageaccountkey=$STORAGE_KEY
```

## <a name="mount-the-file-share-as-a-volume"></a>Bir birim olarak dosya paylaşımını bağlama

Pod içinde Azure dosya paylaşımını bağlayabilmeniz için birim kapsayıcı spec içinde yapılandırın. Adlı yeni bir dosya oluşturun `azure-files-pod.yaml` aşağıdaki içeriğe sahip. Dosya Paylaşımı veya gizli dizi adı adını değiştirdiyseniz, güncelleştirme *shareName* ve *secretName*. İsterseniz, güncelleştirme `mountPath`, burada dosya paylaşma yolu olduğu pod bağlanmıştır. Kapsayıcılar (şu anda önizlemede AKS), Windows Server için belirtin bir *mountPath* gibi Windows yol kuralı kullanılarak *'D:'* .

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
    azureFile:
      secretName: azure-secret
      shareName: aksshare
      readOnly: false
```

Kullanım `kubectl` pod oluşturmak için komutu.

```console
kubectl apply -f azure-files-pod.yaml
```

Artık takılı olduğu Azure dosyaları paylaşımına sahip çalışan bir pod sahip */mnt/azure*. Kullanabileceğiniz `kubectl describe pod mypod` paylaşımı başarıyla oluşturulmuş doğrulayın. Aşağıdaki sıkıştırılmış örneğe çıktı kapsayıcıda takılı birim gösterir:

```
Containers:
  mypod:
    Container ID:   docker://86d244cfc7c4822401e88f55fd75217d213aa9c3c6a3df169e76e8e25ed28166
    Image:          nginx:1.15.5
    Image ID:       docker-pullable://nginx@sha256:9ad0746d8f2ea6df3a17ba89eca40b48c47066dfab55a75e08e2b70fc80d929e
    State:          Running
      Started:      Sat, 02 Mar 2019 00:05:47 +0000
    Ready:          True
    Mounts:
      /mnt/azure from azure (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-z5sd7 (ro)
[...]
Volumes:
  azure:
    Type:        AzureFile (an Azure File Service mount on the host and bind mount to the pod)
    SecretName:  azure-secret
    ShareName:   aksshare
    ReadOnly:    false
  default-token-z5sd7:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-z5sd7
[...]
```

## <a name="mount-options"></a>Bağlama Seçenekleri

Varsayılan *fileMode* ve *dirMode* değerler aşağıdaki tabloda açıklandığı gibi Kubernetes sürümleri arasında farklılık gösterir.

| version | value |
| ---- | ---- |
| v1.6.x, v1.7.x | 0777 |
| v1.8.0-v1.8.5 | 0700 |
| V1.8.6 veya üzeri | 0755 |
| v1.9.0 | 0700 |
| V1.9.1 veya üzeri | 0755 |

Bir küme 1.8.5 sürümü kullanıyorsanız veya daha büyük ve statik olarak kalıcı hacim nesne oluşturma, bağlama seçeneklerini üzerinde belirtilmesine gerek *PersistentVolume* nesne.

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

İlişkili en iyi yöntemler için bkz: [en iyi uygulamalar için depolama ve yedekleme aks'deki][operator-best-practices-storage].

Azure dosyaları ile etkileşim AKS kümeleri hakkında daha fazla bilgi için bkz: [Azure dosyaları için Kubernetes eklentisi][kubernetes-files].

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#create
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/
[smb-overview]: /windows/desktop/FileIO/microsoft-smb-protocol-and-cifs-protocol-overview
[kubernetes-security-context]: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-storage-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-key-list]: /cli/azure/storage/account/keys#az-storage-account-keys-list
[az-storage-share-create]: /cli/azure/storage/share#az-storage-share-create
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[operator-best-practices-storage]: operator-best-practices-storage.md
[concepts-storage]: concepts-storage.md
