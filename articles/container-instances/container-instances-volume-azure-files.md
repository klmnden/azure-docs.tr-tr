---
title: Azure Container ınstances'da bir Azure dosya birimi bağlama
description: Azure Container Instances ile durum kalıcı hale getirmek için bir Azure dosya birimi bağlama hakkında bilgi edinin
services: container-instances
author: seanmck
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 02/20/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 83c86d8310aff80f148e878261ba33b01846006b
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39441332"
---
# <a name="mount-an-azure-file-share-in-azure-container-instances"></a>Azure Container ınstances'da bir Azure dosya paylaşımını bağlama

Varsayılan olarak, Azure Container Instances, durum bilgisi bulunmaz. Kapsayıcı kilitleniyor veya durdurur, durumuna kaybolur. Kapsayıcı ömür ötesinde durumunu kalıcı hale getirmek için bir dış depodan bir birimi bağlamak gerekir. Bu makalede, Azure Container Instances ile kullanım için bir Azure dosya paylaşımını bağlama gösterilmektedir.

> [!NOTE]
> Azure dosyaları paylaşımı bağlayarak, Linux kapsayıcıları için şu anda sınırlıdır. Tüm özellikleri Windows kapsayıcılarına getirmek için çalışmamız esnasında, geçerli platform farklılıklarını [Azure Kapsayıcı Örnekleri için kotalar ve bölge kullanılabilirliği](container-instances-quotas.md) bölümünde bulabilirsiniz.

## <a name="create-an-azure-file-share"></a>Azure dosya paylaşımı oluşturma

Azure Container Instances ile bir Azure dosya paylaşımı kullanmadan önce oluşturmanız gerekir. Dosya Paylaşımı ve Paylaşım barındırmak için bir depolama hesabı oluşturmak için aşağıdaki betiği çalıştırın. Betik rastgele bir değeri temel dizesi olarak ekler. Bu nedenle, depolama hesabı adı genel olarak benzersiz olmalıdır.

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

Azure Container ınstances'da bir birimi olarak Azure dosya paylaşımını bağlayabilmeniz için üç değer gerekir: depolama hesabı adı, paylaşım adı ve depolama erişim anahtarı.

Yukarıdaki betik kullandıysanız, depolama hesabı adı ile rastgele bir değeri en sonda oluşturuldu. (Rastgele bölümüne dahil) son dizede sorgulamak için aşağıdaki komutları kullanın:

```azurecli-interactive
STORAGE_ACCOUNT=$(az storage account list --resource-group $ACI_PERS_RESOURCE_GROUP --query "[?contains(name,'$ACI_PERS_STORAGE_ACCOUNT_NAME')].[name]" --output tsv)
echo $STORAGE_ACCOUNT
```

Paylaşım adı zaten bilinen (olarak tanımlanan *acishare* yukarıdaki komut dosyasında), bu nedenle tüm kalır olduğu aşağıdaki komutu kullanarak bulunabilir depolama hesabı anahtarı:

```azurecli-interactive
STORAGE_KEY=$(az storage account keys list --resource-group $ACI_PERS_RESOURCE_GROUP --account-name $STORAGE_ACCOUNT --query "[0].value" --output tsv)
echo $STORAGE_KEY
```

## <a name="deploy-container-and-mount-volume"></a>Birim kapsayıcısı ve bağlama dağıtma

Bir kapsayıcıdaki bir birimi olarak Azure dosya paylaşımını bağlayabilmeniz için paylaşım ve birim bağlama noktası kapsayıcı ile oluşturduğunuzda belirtin [az kapsayıcı oluşturma][az-container-create]. Önceki adımları izlediyseniz bir kapsayıcı oluşturmak için aşağıdaki komutu kullanarak daha önce oluşturduğunuz paylaşımı bağlayabilir:

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

`--dns-name-label` değeri, kapsayıcı örneğini oluşturduğunuz Azure bölgesi içinde benzersiz olmalıdır. Alırsanız önceki komutta değeri güncelleştirin bir **DNS ad etiketi** komutu yürütürken bir hata iletisi.

## <a name="manage-files-in-mounted-volume"></a>Takılan birimin dosyalarını yönetme

Kapsayıcı başlatıldığında sonra aracılığıyla dağıtılan basit web uygulaması kullanabilirsiniz [Acı/microsoft-hellofiles] [ aci-hellofiles] dosyaları belirttiğiniz bağlama yolu Azure dosya paylaşımını yönetmek için resim. Web uygulaması'nın tam etki alanı adı (FQDN) ile elde [az container show] [ az-container-show] komutu:

```azurecli-interactive
az container show --resource-group $ACI_PERS_RESOURCE_GROUP --name hellofiles --query ipAddress.fqdn
```

Kullanabileceğiniz [Azure portalında] [ portal] veya bir aracı gibi [Microsoft Azure Depolama Gezgini] [ storage-explorer] alıp yazılan dosyasını inceleyin Dosya Paylaşımı.

## <a name="mount-multiple-volumes"></a>Birden çok birim bağlama

Birden çok birim bir kapsayıcı örneğine bağlanacak kullanarak dağıtmalısınız bir [Azure Resource Manager şablonu](/azure/templates/microsoft.containerinstance/containergroups).

İlk olarak, paylaşım ayrıntılarını sağlayın ve doldurarak birimleri tanımlama `volumes` içindeki dizi `properties` şablon bölümü. Örneğin, adlı iki Azure dosya paylaşımlarını oluşturduysanız *share1* ve *share2* depolama hesabındaki *myStorageAccount*, `volumes` dizi görüneceği aşağıdakine benzer:

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

Ardından, içine istediğiniz bağlama birimleri kapsayıcı grubundaki her kapsayıcı için doldurma `volumeMounts` içindeki dizi `properties` kapsayıcı tanımının bölümü. Örneğin, bu iki birimi bağlar *myvolume1* ve *myvolume2*, önceden tanımlanmış:

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

Kapsayıcı örneği dağıtımıyla bir Azure Resource Manager şablonu ile bir örneğini görmek için bkz: [Azure Container ınstances'da çok kapsayıcılı grupları dağıtma](container-instances-multi-container-group.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure Container ınstances'da diğer birim türleri bağlama işlemleri gerçekleştirmeyi öğreneceksiniz:

* [Azure kapsayıcı durumlarda emptyDir birim](container-instances-volume-emptydir.md)
* [Azure Container ınstances'da bir gitRepo birimi](container-instances-volume-gitrepo.md)
* [Azure Container ınstances'da bir gizli birimi](container-instances-volume-secret.md)

<!-- LINKS - External -->
[aci-hellofiles]: https://hub.docker.com/r/microsoft/aci-hellofiles/
[portal]: https://portal.azure.com
[storage-explorer]: https://storageexplorer.com

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-show]: /cli/azure/container#az-container-show