---
title: Azure kapsayıcı durumlarda Azure dosyaları birim
description: Azure kapsayıcı örnekleri durumuyla kalıcı hale getirmek için bir Azure dosyaları birim öğrenin
services: container-instances
author: seanmck
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 02/20/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 239150c1e752ce6a4f2a19fa1192cd1a910ebea9
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32166805"
---
# <a name="mount-an-azure-file-share-in-azure-container-instances"></a>Azure kapsayıcı durumlarda bir Azure dosya paylaşımını bağlama

Varsayılan olarak, Azure kapsayıcı durum bilgisiz örnekleridir. Kapsayıcı kilitlenmesine veya durdurur, durumuna kaybolur. Kapsayıcı ömür ötesinde durumunu kalıcı hale getirmek için bir dış depolama alanından bir birim bağlama gerekir. Bu makalede, Azure kapsayıcı örnekleri ile kullanmak için bir Azure dosya paylaşımının gösterilmektedir.

> [!NOTE]
> Bir Azure dosya paylaşımına bağlanması Linux kapsayıcılara şu anda kısıtlı. Tüm özellikleri Windows kapsayıcılarına getirmek için çalışmamız esnasında, geçerli platform farklılıklarını [Azure Kapsayıcı Örnekleri için kotalar ve bölge kullanılabilirliği](container-instances-quotas.md) bölümünde bulabilirsiniz.

## <a name="create-an-azure-file-share"></a>Azure dosya paylaşımı oluşturma

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
    --image microsoft/aci-hellofiles \
    --dns-name-label aci-demo \
    --ports 80 \
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --azure-file-volume-account-key $STORAGE_KEY \
    --azure-file-volume-share-name $ACI_PERS_SHARE_NAME \
    --azure-file-volume-mount-path /aci/logs/
```

`--dns-name-label` değeri, kapsayıcı örneğini oluşturduğunuz Azure bölgesi içinde benzersiz olmalıdır. Yukarıdaki komut değeri alırsanız güncelleştirin bir **DNS ad etiketi** hata iletisi komutu yürütün.

## <a name="manage-files-in-mounted-volume"></a>Takılan birimin dosyalarını yönetme

Kapsayıcı başlatıldığında sonra aracılığıyla dağıtılan basit web uygulaması kullanarak [aci/microsoft-hellofiles] [ aci-hellofiles] belirttiğiniz bağlama yolundaki Azure dosya paylaşımında dosyaları yönetmek için resim. Web uygulamanızın tam etki alanı adı (FQDN) ile elde [az kapsayıcı Göster] [ az-container-show] komutu:

```azurecli-interactive
az container show --resource-group $ACI_PERS_RESOURCE_GROUP --name hellofiles --query ipAddress.fqdn
```

Kullanabileceğiniz [Azure portal] [ portal] veya bir aracı [Microsoft Azure Storage Gezgini] [ storage-explorer] almak ve yazılan dosyasını inceleyin Dosya Paylaşımı.

## <a name="mount-multiple-volumes"></a>Birden çok birim bağlama

Bir kapsayıcı örneğinde birden çok birimi bağlamak, kullanarak dağıtmanız gerekir bir [Azure Resource Manager şablonu](/azure/templates/microsoft.containerinstance/containergroups).

İlk olarak, paylaşım detayları sağlayın ve doldurarak birimleri tanımlama `volumes` içinde dizi `properties` şablon bölümünü. Örneğin, iki Azure dosya paylaşımlarının adlı oluşturduysanız *share1* ve *share2* depolama hesabındaki *myStorageAccount*, `volumes` dizi görüneceği aşağıdakine benzer:

```json
"volumes": [{
  "name": "myvolume1",
  "azureFile": {
    "shareName": "share1",
    "storageAccountName": "myStorageAccount",
    "storageAccountKey": "<storage-account-key>"
  }
},
{
  "name": "myvolume2",
  "azureFile": {
    "shareName": "share2",
    "storageAccountName": "myStorageAccount",
    "storageAccountKey": "<storage-account-key>"
  }
}]
```

Ardından, içinde gibi birimlerini kapsayıcı grubundaki her kapsayıcı için doldurmak `volumeMounts` içinde dizi `properties` kapsayıcı tanımının bölümü. Örneğin, bu iki birim bağlar *myvolume1* ve *myvolume2*, önceden tanımlanmış:

```json
"volumeMounts": [{
  "name": "myvolume1",
  "mountPath": "/mnt/share1/"
},
{
  "name": "myvolume2",
  "mountPath": "/mnt/share2/"
}]
```

Örnek bir Azure Resource Manager şablonu ile kapsayıcı örnek dağıtım görmek için bkz: [çok kapsayıcı grupları Azure kapsayıcı durumlarda dağıtmak](container-instances-multi-container-group.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure kapsayıcı örnekleri diğer birim türlerinde bağlama öğrenin:

* [Azure kapsayıcı durumlarda emptyDir birim](container-instances-volume-emptydir.md)
* [Azure kapsayıcı durumlarda gitRepo birim](container-instances-volume-gitrepo.md)
* [Azure kapsayıcı durumlarda gizli bir birim](container-instances-volume-secret.md)

<!-- LINKS - External -->
[aci-hellofiles]: https://hub.docker.com/r/microsoft/aci-hellofiles/
[portal]: https://portal.azure.com
[storage-explorer]: https://storageexplorer.com

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az_container_create
[az-container-show]: /cli/azure/container#az_container_show