---
title: Azure dosya AKS ile kullanma
description: Azure diskleri AKS ile kullanma
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 03/08/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 163db8fdaecefbf51174392ba37039115cdb91c8
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="volumes-with-azure-files"></a>Azure dosyaları ile birimleri

Kapsayıcı tabanlı uygulamalara erişmek ve bir dış veri birimdeki veriler kalıcı hale getirmek çoğunlukla gerekir. Azure dosyaları bu dış veri deposu olarak kullanılabilir. Bu makale Azure Kubernetes hizmetindeki Kubernetes birimi olarak Azure dosyaları kullanılarak ayrıntıları.

Kubernetes birimleri hakkında daha fazla bilgi için bkz: [Kubernetes birimleri][kubernetes-volumes].

## <a name="create-an-azure-file-share"></a>Azure dosya paylaşımı oluşturma

Bir Azure dosya paylaşımı Kubernetes birimi olarak kullanmadan önce bir Azure Storage hesabı ve dosya paylaşımı oluşturmanız gerekir. Aşağıdaki komut dosyası, bu görevleri tamamlamak için kullanılabilir. Not edin veya parametre değerleri güncelleştirmek, bunlardan bazıları Kubernetes birim oluşturulurken gereklidir.

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
echo Storgae account key: $STORAGE_KEY
```

## <a name="create-kubernetes-secret"></a>Kubernetes gizli anahtar oluşturma

Kubernetes dosya paylaşımına erişmek için kimlik bilgileri gerekir. Bu kimlik bilgileri depolanmış bir [Kubernetes gizli][kubernetes-secret], Kubernetes pod oluştururken başvurulan.

Gizli anahtarı oluşturmak için aşağıdaki komutu kullanın. Değiştir `STORAGE_ACCOUNT_NAME` , depolama hesabı adı ile ve `STORAGE_ACCOUNT_KEY` depolama anahtarınız ile.

```console
kubectl create secret generic azure-secret --from-literal=azurestorageaccountname=STORAGE_ACCOUNT_NAME --from-literal=azurestorageaccountkey=STORAGE_ACCOUNT_KEY
```

## <a name="mount-file-share-as-volume"></a>Birim olarak dosya paylaşımını bağlama

Azure dosya paylaşımınıza kendi spec birim yapılandırarak, pod bağlayın. Adlı yeni bir dosya oluşturun `azure-files-pod.yaml` aşağıdaki içeriğe sahip. Güncelleştirme `aksshare` Azure dosyaları verilen ad paylaşın.

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

Şimdi takılabilir, Azure dosya paylaşımı ile çalışan bir kapsayıcıya sahip `/mnt/azure` dizin.  Birimi, pod aracılığıyla incelerken bağlama görebilirsiniz `kubectl describe pod azure-files-pod`.

## <a name="next-steps"></a>Sonraki adımlar

Azure dosyaları kullanarak Kubernetes birimleri hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Azure dosyaları için Kubernetes eklentisi][kubernetes-files]

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#create
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az_group_create
[az-storage-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-key-list]: /cli/azure/storage/account/keys#az_storage_account_keys_list
[az-storage-share-create]: /cli/azure/storage/share#az_storage_share_create
