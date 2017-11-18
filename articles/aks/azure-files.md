---
title: Azure dosya AKS ile kullanma | Microsoft Docs
description: Azure diskleri AKS ile kullanma
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/17/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: bae60e7f78934deacac173767ca3013ce93cf9ad
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
---
# <a name="using-azure-files-with-kubernetes"></a>Azure dosyaları ile Kubernetes kullanma

Kapsayıcı tabanlı uygulamalara erişmek ve bir dış veri birimdeki veriler kalıcı hale getirmek çoğunlukla gerekir. Azure dosyaları bu dış veri deposu olarak kullanılabilir. Bu makale Azure kapsayıcı Hizmeti'nde bir Kubernetes birimi olarak Azure dosyaları kullanılarak ayrıntıları.

Kubernetes birimleri hakkında daha fazla bilgi için bkz: [Kubernetes birimleri][kubernetes-volumes].

## <a name="create-an-azure-file-share"></a>Bir Azure dosya paylaşımı oluşturma

Bir Azure dosya paylaşımı Kubernetes birimi olarak kullanmadan önce bir Azure Storage hesabı ve dosya paylaşımı oluşturmanız gerekir. Aşağıdaki komut dosyası, bu görevleri tamamlamak için kullanılabilir. Not edin veya parametre değerleri güncelleştirmek, bunlardan bazıları Kubernetes birim oluşturulurken gereklidir.

```azurecli-interactive
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
```

## <a name="create-kubernetes-secret"></a>Kubernetes gizli anahtar oluşturma

Kubernetes dosya paylaşımına erişmek için kimlik bilgileri gerekir. Bu kimlik bilgileri depolanmış bir [Kubernetes gizli][kubernetes-secret], Kubernetes pod oluştururken başvurulan.

Bir Kubernetes gizli oluştururken, gizli değerleri base64 ile kodlanmış olmalıdır.

İlk olarak, depolama hesabının adını kodlayın. Gerekirse, Değiştir `$AKS_PERS_STORAGE_ACCOUNT_NAME` Azure depolama hesabı adı.

```azurecli-interactive
echo -n $AKS_PERS_STORAGE_ACCOUNT_NAME | base64
```

Ardından, depolama hesabı anahtarı kodlayın. Gerekirse, Değiştir `$STORAGE_KEY` Azure depolama hesabı anahtarı adı.

```azurecli-interactive
echo -n $STORAGE_KEY | base64
```

Adlı bir dosya oluşturun `azure-secret.yml` ve aşağıdaki YAML kopyalayın. Güncelleştirme `azurestorageaccountname` ve `azurestorageaccountkey` değerleri base64 ile kodlanmış son adımda alınan değer.

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: azure-secret
type: Opaque
data:
  azurestorageaccountname: <base64_encoded_storage_account_name>
  azurestorageaccountkey: <base64_encoded_storage_account_key>
```

Kullanım [kubectl oluşturma] [ kubectl-create] gizli anahtarı oluşturmak için komutu.

```azurecli-interactive
kubectl create -f azure-secret.yml
```

## <a name="mount-file-share-as-volume"></a>Birim olarak dosya paylaşımını bağlama

Kendi spec birim yapılandırarak, pod, Azure dosya paylaşımı bağlayabilir. Adlı yeni bir dosya oluşturun `azure-files-pod.yml` aşağıdaki içeriğe sahip. Güncelleştirme `aksshare` Azure dosyaları verilen ad paylaşın.

```yaml
apiVersion: v1
kind: Pod
metadata:
 name: azure-files-pod
spec:
 containers:
  - image: kubernetes/pause
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
kubectl apply -f azure-files-pod.yml
```

Şimdi takılabilir, Azure dosya paylaşımı ile çalışan bir kapsayıcıya sahip `/mnt/azure` dizin. Birimi, pod aracılığıyla incelerken bağlama görebilirsiniz `kubectl describe pod azure-files-pod`.

## <a name="next-steps"></a>Sonraki adımlar

Azure dosyaları kullanarak Kubernetes birimleri hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Azure dosyaları için Kubernetes eklentisi](https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md)

<!-- LINKS -->
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/
[az-storage-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-key-list]: /cli/azure/storage/account/keys#az_storage_account_keys_list
[az-storage-share-create]: /cli/azure/storage/share#az_storage_share_create
[kubectl-create]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#create
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[az-group-create]: /cli/azure/group#az_group_create