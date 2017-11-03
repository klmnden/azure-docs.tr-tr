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
ms.date: 10/30/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 6e88c590e11aa8d2f4ae17e8b5e164483f0a6820
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="using-azure-files-with-kubernetes"></a>Azure dosyaları ile Kubernetes kullanma

Kapsayıcı tabanlı uygulamaları genellikle erişmek ve bir dış veri birimdeki veriler kalıcı hale getirmek gerekir. Azure dosyaları bu dış veri deposu olarak kullanılabilir. Bu makale Azure kapsayıcı Hizmeti'nde bir Kubernetes birimi olarak Azure dosyaları kullanılarak ayrıntıları.

Kubernetes birimleri hakkında daha fazla bilgi için bkz: [Kubernetes birimleri][kubernetes-volumes].

## <a name="creating-a-file-share"></a>Bir dosya paylaşımı oluşturma

Var olan bir Azure dosya paylaşımı Azure kapsayıcı hizmeti ile kullanılabilir. Bir oluşturmanız gerekiyorsa, aşağıdaki komutları kümesini kullanın.

Kullanarak Azure dosya paylaşımı için bir kaynak grubu oluşturmak [az grubu oluşturma] [ az-group-create] komutu. Depolama hesabı ve Kubernetes küme kaynak grubu aynı bölgede bulunması gerekir.

```azurecli-interactive
az group create --name myResourceGroup --location westus2
```

Kullanım [az depolama hesabı oluşturma] [ az-storage-create] bir Azure Storage hesabı oluşturmak için komutu. Depolama hesabı adı benzersiz olmalıdır. Değerini güncelleştirmek `--name` bağımsız değişkeni ile benzersiz bir değer.

```azurecli-interactive
az storage account create --name mystorageaccount --resource-group myResourceGroup --sku Standard_LRS
```

Kullanım [az depolama hesabı anahtarları listesi ] [ az-storage-key-list] depolama anahtarı geri dönmek için komutu. Değerini güncelleştirmek `--account-name` bağımsız değişkeni ile benzersiz depolama hesabı adı.

Not anahtar değerlerin her birini bu sonraki adımda kullanılır.

```azurecli-interactive
az storage account keys list --account-name mystorageaccount --resource-group myResourceGroup --output table
```

Kullanım [az depolama paylaşımı oluşturmak] [ az-storage-share-create] Azure dosya paylaşımı oluşturmak için komutu. Güncelleştirme `--account-key` değeriyle toplanan son adımı.

```azurecli-interactive
az storage share create --name myfileshare --account-name mystorageaccount --account-key <key>
```

## <a name="create-kubernetes-secret"></a>Kubernetes gizli anahtar oluşturma

Kubernetes dosya paylaşımına erişmek için kimlik bilgileri gerekir. Azure depolama hesabı adı ve her pod anahtarla depolamak yerine, bir kez depolanır bir [Kubernetes gizli] [ kubernetes-secret] ve her Azure dosyaları birim tarafından başvurulan. 

Kubernetes gizli bildiriminde değerlerinin base64 ile kodlanmış olması gerekir. Kodlanmış değer döndürmek için aşağıdaki komutları kullanın.

İlk olarak, depolama hesabının adını kodlayın. Değiştir `storage-account` Azure depolama hesabınızın adı.

```azurecli-interactive
echo -n <storage-account> | base64
```

Ardından, depolama hesabının erişim anahtarı gereklidir. Şifrelenmiş anahtar döndürmek için aşağıdaki komutu çalıştırın. Değiştir `storage-key` bir önceki adımda toplanan anahtarla

```azurecli-interactive
echo -n <storage-key> | base64
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

Kullanım [kubectl uygulamak] [ kubectl-apply] gizli anahtarı oluşturmak için komutu.

```azurecli-interactive
kubectl apply -f azure-secret.yml
```

## <a name="mount-file-share-as-volume"></a>Birim olarak dosya paylaşımını bağlama

Kendi spec birim yapılandırarak, pod, Azure dosya paylaşımı bağlayabilir. Adlı yeni bir dosya oluşturun `azure-files-pod.yml` aşağıdaki içeriğe sahip. Güncelleştirme `share-name` Azure dosyaları verilen ad paylaşın.

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
      shareName: <share-name>
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
[kubectl-apply]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#apply
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[az-group-create]: /cli/azure/group#az_group_create