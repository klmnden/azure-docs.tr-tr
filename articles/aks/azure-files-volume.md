---
title: Azure dosya AKS ile kullanma
description: AKS ile Azure disklerini kullanma
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 03/08/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: b35d0e33009f76e0b2d6f90c52c98ce5f317792d
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39436769"
---
# <a name="volumes-with-azure-files"></a>Azure dosyaları ile birimleri

Kapsayıcı tabanlı uygulamalar genellikle erişmek ve bir dış veri birimdeki veriler kalıcı hale getirmek gerekir. Azure dosyaları, bu dış veri deposu olarak kullanılabilir. Bu makalede, Azure Kubernetes Service'teki bir Kubernetes birimi olarak Azure dosyaları'nı kullanarak ayrıntıları.

Kubernetes birimleri hakkında daha fazla bilgi için bkz. [Kubernetes birimleri][kubernetes-volumes].

## <a name="create-an-azure-file-share"></a>Azure dosya paylaşımı oluşturma

Bir Azure dosya paylaşımı Kubernetes birim olarak kullanmadan önce bir Azure depolama hesabı ve dosya paylaşımı oluşturmanız gerekir. Bu görevleri tamamlamak için aşağıdaki betiği kullanılabilir. Not alın veya parametre değerlerini güncelleştirin, bunlardan bazıları Kubernetes hacmi oluştururken gereklidir.

```azurecli-interactive
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

## <a name="create-kubernetes-secret"></a>Kubernetes gizli dizi oluşturma

Kubernetes dosya paylaşımına erişmek için kimlik bilgileri gerekir. Bu kimlik bilgileri depolanan bir [Kubernetes gizli][kubernetes-secret], bir Kubernetes pod oluştururken başvuruyor.

Gizli dizi oluşturmak için aşağıdaki komutu kullanın. Değiştirin `STORAGE_ACCOUNT_NAME` ile depolama hesabı adınızı ve `STORAGE_ACCOUNT_KEY` depolama anahtarınız ile.

```console
kubectl create secret generic azure-secret --from-literal=azurestorageaccountname=STORAGE_ACCOUNT_NAME --from-literal=azurestorageaccountkey=STORAGE_ACCOUNT_KEY
```

## <a name="mount-file-share-as-volume"></a>Dosya paylaşımını birim olarak bağlama

Azure dosya paylaşımınızı, bir pod içinde kendi spec birim yapılandırarak bağlayın. Adlı yeni bir dosya oluşturun `azure-files-pod.yaml` aşağıdaki içeriğe sahip. Güncelleştirme `aksshare` Azure dosyaları'na verilen ad ile paylaşın.

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

Kubectl bir pod oluşturmak için kullanın.

```azurecli-interactive
kubectl apply -f azure-files-pod.yaml
```

Artık Azure dosya paylaşımınızı takılabilir ile çalışan bir kapsayıcıya sahip `/mnt/azure` dizin.  Birimi aracılığıyla pod incelerken bağlama gördüğünüz `kubectl describe pod azure-files-pod`.

## <a name="next-steps"></a>Sonraki adımlar

Azure dosyaları'nı kullanarak Kubernetes birimleri hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Azure dosyaları için Kubernetes eklentisi][kubernetes-files]

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#create
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-storage-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-key-list]: /cli/azure/storage/account/keys#az-storage-account-keys-list
[az-storage-share-create]: /cli/azure/storage/share#az-storage-share-create
