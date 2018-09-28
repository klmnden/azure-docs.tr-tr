---
title: Azure Kubernetes Service (AKS) için birden çok podunuz statik bir birim oluşturun
description: El ile birden çok eş zamanlı pod Azure Kubernetes Service (AKS) ile kullanmak için Azure dosyaları ile birim oluşturma hakkında bilgi edinin
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 09/26/2018
ms.author: iainfou
ms.openlocfilehash: e5518ebb2985635507368943774e6be803cfffa8
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47409071"
---
# <a name="manually-create-and-use-an-azure-files-share-in-azure-kubernetes-service-aks"></a>El ile oluşturma ve Azure dosyaları paylaşımına Azure Kubernetes Service (AKS) kullanma

Kapsayıcı tabanlı uygulamalar genellikle erişmek ve bir dış veri birimdeki veriler kalıcı hale getirmek gerekir. Birden çok pod'ların aynı depolama birimine eş zamanlı erişim gerekiyorsa, Azure dosyaları kullanarak bağlanmak için kullanabileceğiniz [sunucu ileti bloğu (SMB) Protokolü][smb-overview]. Bu makalede aks'deki bir pod ekleme ve el ile bir Azure dosya paylaşımı oluşturma gösterilmektedir.

Kubernetes birimleri hakkında daha fazla bilgi için bkz. [Kubernetes birimleri][kubernetes-volumes].

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, var olan bir AKS kümesi olduğunu varsayar. AKS hızlı bir AKS kümesi gerekirse bkz [Azure CLI kullanarak] [ aks-quickstart-cli] veya [Azure portalını kullanarak][aks-quickstart-portal].

Ayrıca Azure CLI Sürüm 2.0.46 gerekir veya daha sonra yüklü ve yapılandırılmış. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][install-azure-cli].

## <a name="create-an-azure-file-share"></a>Azure dosya paylaşımı oluşturma

Kubernetes birimi olarak Azure dosyaları'nı kullanmadan önce bir Azure depolama hesabı ve dosya paylaşımı oluşturmanız gerekir. Aşağıdaki betiği adlı bir kaynak grubu oluşturur *myAKSShare*, bir depolama hesabı ve adlı bir dosya paylaşımı *aksshare*:

```azurecli
#!/bin/bash

# Change these four parameters
AKS_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
AKS_PERS_RESOURCE_GROUP=myAKSShare
AKS_PERS_LOCATION=eastus
AKS_PERS_SHARE_NAME=aksshare

# Create the Resource Group
az group create --name $AKS_PERS_RESOURCE_GROUP --location $AKS_PERS_LOCATION

# Create the storage account
az storage account create -n $AKS_PERS_STORAGE_ACCOUNT_NAME -g $AKS_PERS_RESOURCE_GROUP -l $AKS_PERS_LOCATION --sku Standard_LRS

# Export the connection string as an environment variable, this is used when creating the Azure file share
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $AKS_PERS_STORAGE_ACCOUNT_NAME -g $AKS_PERS_RESOURCE_GROUP -o tsv`

# Create the file share
az storage share create -n $AKS_PERS_SHARE_NAME

# Get storage account key
STORAGE_KEY=$(az storage account keys list --resource-group $AKS_PERS_RESOURCE_GROUP --account-name $AKS_PERS_STORAGE_ACCOUNT_NAME --query "[0].value" -o tsv)

# Echo storage account name and key
echo Storage account name: $AKS_PERS_STORAGE_ACCOUNT_NAME
echo Storage account key: $STORAGE_KEY
```

Depolama hesabı adını ve anahtarını betik çıktısı sonunda gösterilen bir not edin. Kubernetes hacmi aşağıdaki adımlardan birini oluşturduğunuzda bu değerlere gereklidir.

## <a name="create-a-kubernetes-secret"></a>Bir Kubernetes gizli dizisi oluşturma

Kubernetes önceki adımda oluşturduğunuz dosya paylaşımına erişmek için kimlik bilgileri gerekir. Bu kimlik bilgileri depolanan bir [Kubernetes gizli][kubernetes-secret], bir Kubernetes pod oluşturduğunuzda başvuruyor.

Kullanım `kubectl create secret` gizli dizi oluşturmak için komutu. Aşağıdaki örnek, paylaşılan adlı oluşturur *azure-secret*. Değiştirin *STORAGE_ACCOUNT_NAME* ile önceki adımın çıktıda gösterilen depolama hesabı adınızı ve *STORAGE_ACCOUNT_KEY* depolama anahtarınız ile:

```console
kubectl create secret generic azure-secret --from-literal=azurestorageaccountname=STORAGE_ACCOUNT_NAME --from-literal=azurestorageaccountkey=STORAGE_ACCOUNT_KEY
```

## <a name="mount-the-file-share-as-a-volume"></a>Bir birim olarak dosya paylaşımını bağlama

Pod içinde Azure dosya paylaşımını bağlayabilmeniz için birim kapsayıcı spec içinde yapılandırın. Adlı yeni bir dosya oluşturun `azure-files-pod.yaml` aşağıdaki içeriğe sahip. Dosya Paylaşımı veya gizli dizi adı adını değiştirdiyseniz, güncelleştirme *shareName* ve *secretName*. İsterseniz, güncelleştirme `mountPath`, burada dosya paylaşma yolu olduğu pod bağlanmıştır.

```yaml
apiVersion: v1
kind: Pod
metadata:
 name: azure-files-pod
spec:
 containers:
  - image: microsoft/sample-aks-helloworld
    name: azure
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

```azurecli-interactive
kubectl apply -f azure-files-pod.yaml
```

Artık takılı olduğu Azure dosyaları paylaşımına sahip çalışan bir pod sahip */mnt/azure*. Kullanabileceğiniz `kubectl describe pod azure-files-pod` paylaşımı başarıyla oluşturulmuş doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Azure dosyaları ile etkileşim AKS kümeleri hakkında daha fazla bilgi için bkz: [Azure dosyaları için Kubernetes eklentisi][kubernetes-files].

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#create
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/
[smb-overview]: /windows/desktop/FileIO/microsoft-smb-protocol-and-cifs-protocol-overview

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-storage-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-key-list]: /cli/azure/storage/account/keys#az-storage-account-keys-list
[az-storage-share-create]: /cli/azure/storage/share#az-storage-share-create
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
