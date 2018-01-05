---
title: "Azure kapsayıcı durumlarda Azure dosyaları birim"
description: "Azure kapsayıcı örnekleri durumuyla kalıcı hale getirmek için bir Azure dosyaları birim öğrenin"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 06a6e91725e751fbea97d9a3b60f48fa50121fc4
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2018
---
# <a name="mount-an-azure-file-share-with-azure-container-instances"></a>Azure kapsayıcı örneği ile bir Azure dosya paylaşımını bağlama

Varsayılan olarak, Azure kapsayıcı durum bilgisiz örnekleridir. Kapsayıcı kilitlenmesine veya durdurur, durumuna kaybolur. Kapsayıcı ömür ötesinde durumunu kalıcı hale getirmek için bir dış depolama alanından bir birim bağlama gerekir. Bu makalede, Azure kapsayıcı örnekleri ile kullanmak için bir Azure dosya paylaşımının gösterilmektedir.

> [!NOTE]
> Azure dosya paylaşımının takma Linux kapsayıcılara şu anda kısıtlı. Tüm özellikleri Windows kapsayıcılara getirmek için çalışıyoruz, ancak geçerli platform farklılıkları bulabilirsiniz [kotalar ve Azure kapsayıcı örnekleri için bölge kullanılabilirliği](container-instances-quotas.md).

## <a name="create-an-azure-file-share"></a>Bir Azure dosya paylaşımı oluşturma

Azure kapsayıcı örnekleri ile Azure dosya paylaşımının kullanmadan önce oluşturmanız gerekir. Dosya Paylaşımı ve Paylaşım barındırmak için bir depolama hesabı oluşturmak için aşağıdaki betiği çalıştırın. Betik rastgele bir değeri temel dizesi olarak ekler ve böylece depolama hesabı adı genel olarak benzersiz olması gerekir.

```azurecli-interactive
# Change these four parameters as needed
ACI_PERS_RESOURCE_GROUP=myResourceGroup
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
ACI_PERS_LOCATION=eastus
ACI_PERS_SHARE_NAME=acishare

# Create the storage account with the parameters
az storage account create \
    --resource-group $ACI_PERS_RESOURCE_GROUP \
    --name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --location $ACI_PERS_LOCATION \
    --sku Standard_LRS

# Export the connection string as an environment variable. The following 'az storage share create' command
# references this environment variable when creating the Azure file share.
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string --resource-group $ACI_PERS_RESOURCE_GROUP --name $ACI_PERS_STORAGE_ACCOUNT_NAME --output tsv`

# Create the file share
az storage share create -n $ACI_PERS_SHARE_NAME
```

## <a name="get-storage-credentials"></a>Depolama kimlik bilgilerini alma

Azure kapsayıcı durumlarda bir birim olarak Azure dosya paylaşımının bağlamak için üç değerden gerekir: depolama hesabı adı, paylaşım adı ve depolama erişim tuşu.

Yukarıdaki betik kullandıysanız, depolama hesabı adı sonunda rastgele bir değeri ile oluşturuldu. (Rastgele bölümü dahil) son dizede sorgulamak için aşağıdaki komutları kullanın:

```azurecli-interactive
STORAGE_ACCOUNT=$(az storage account list --resource-group $ACI_PERS_RESOURCE_GROUP --query "[?contains(name,'$ACI_PERS_STORAGE_ACCOUNT_NAME')].[name]" --output tsv)
echo $STORAGE_ACCOUNT
```

Paylaşım adı zaten bilinen (olarak tanımlanan *acishare* yukarıdaki komut), böylece tüm kalır olduğunu aşağıdaki komutu kullanarak bulunabilir depolama hesabı anahtarı:

```azurecli-interactive
STORAGE_KEY=$(az storage account keys list --resource-group $ACI_PERS_RESOURCE_GROUP --account-name $STORAGE_ACCOUNT --query "[0].value" --output tsv)
echo $STORAGE_KEY
```

## <a name="deploy-container-and-mount-volume"></a>Kapsayıcı ve takma birimi dağıtma

Bir kapsayıcıda bir birim olarak Azure dosya paylaşımının bağlamak için paylaşımı ve birim kapsayıcısı ile oluşturduğunuzda noktası bağlama belirtin [az kapsayıcı oluşturmak][az-container-create]. Önceki adımları uyguladıysanız, daha önce bir kapsayıcı oluşturmak için aşağıdaki komutu kullanarak oluşturduğunuz paylaşımı bağlayabilir:

```azurecli-interactive
az container create \
    --resource-group $ACI_PERS_RESOURCE_GROUP \
    --name hellofiles \
    --image seanmckenna/aci-hellofiles \
    --ip-address Public \
    --ports 80 \
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --azure-file-volume-account-key $STORAGE_KEY \
    --azure-file-volume-share-name $ACI_PERS_SHARE_NAME \
    --azure-file-volume-mount-path /aci/logs/
```

## <a name="manage-files-in-mounted-volume"></a>Takılan birimin dosyalarını yönetme

Kapsayıcı başlatıldığında sonra aracılığıyla dağıtılan basit web uygulaması kullanarak [aci/seanmckenna-hellofiles] [ aci-hellofiles] belirttiğiniz bağlama yolundaki Azure dosya paylaşımında dosyaları yönetmek için resim. Web uygulaması ile IP adresi al [az kapsayıcı Göster] [ az-container-show] komutu:

```azurecli-interactive
az container show --resource-group $ACI_PERS_RESOURCE_GROUP --name hellofiles --output table
```

Kullanabileceğiniz [Azure portal] [ portal] veya bir aracı [Microsoft Azure Storage Gezgini] [ storage-explorer] almak ve yazılan dosyasını inceleyin Dosya Paylaşımı.

## <a name="next-steps"></a>Sonraki adımlar

Arasındaki ilişki hakkında bilgi edinin [Azure kapsayıcı örnekleri ve kapsayıcı orchestrators](container-instances-orchestrator-relationship.md).

<!-- LINKS - External -->
[aci-hellofiles]: https://hub.docker.com/r/seanmckenna/aci-hellofiles/
[portal]: https://portal.azure.com
[storage-explorer]: https://storageexplorer.com

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az_container_create
[az-container-show]: /cli/azure/container#az_container_show