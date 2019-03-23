---
title: Azure Container ınstances'da bir Azure dosya birimi bağlama
description: Azure Container Instances ile durum kalıcı hale getirmek için bir Azure dosya birimi bağlama hakkında bilgi edinin
services: container-instances
author: dlepow
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 11/05/2018
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 365264d40554f45533e2ddf0aeb9d85f3e8f8d2d
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58370627"
---
# <a name="mount-an-azure-file-share-in-azure-container-instances"></a>Azure Container ınstances'da bir Azure dosya paylaşımını bağlama

Varsayılan olarak, Azure Container Instances, durum bilgisi bulunmaz. Kapsayıcı kilitleniyor veya durdurur, durumuna kaybolur. Kapsayıcı ömür ötesinde durumunu kalıcı hale getirmek için bir dış depodan bir birimi bağlamak gerekir. Bu makalede ile oluşturulmuş bir Azure dosya paylaşımını bağlama işlemi gösterilmektedir [Azure dosyaları](../storage/files/storage-files-introduction.md) Azure Container Instances ile kullanım için. Azure Dosyaları bulutta tamamen yönetilen dosya paylaşımları sunar. Bu dosyalara sektör standardı olan Sunucu İleti Bloğu (SMB) protokolü aracılığıyla erişilebilir. Azure Container Instances ile bir Azure dosya paylaşımı kullanarak bir Azure dosya paylaşımı ile Azure sanal makinelerini kullanmaya benzer dosya paylaşım özellikleri sağlar.

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

# Create the file share
az storage share create --name $ACI_PERS_SHARE_NAME --account-name $ACI_PERS_STORAGE_ACCOUNT_NAME
```

## <a name="get-storage-credentials"></a>Depolama kimlik bilgilerini alma

Azure Container ınstances'da bir birimi olarak Azure dosya paylaşımını bağlayabilmeniz için üç değer gerekir: depolama hesabı adı, paylaşım adı ve depolama erişim anahtarı.

Yukarıdaki betik kullandıysanız, depolama hesabı adı $ACI_PERS_STORAGE_ACCOUNT_NAME değişkeninde depolanan. Hesap adı görmek için şunu yazın:

```console
echo $ACI_PERS_STORAGE_ACCOUNT_NAME
```

Paylaşım adı zaten bilinen (olarak tanımlanan *acishare* yukarıdaki komut dosyasında), bu nedenle tüm kalır olduğu aşağıdaki komutu kullanarak bulunabilir depolama hesabı anahtarı:

```azurecli-interactive
STORAGE_KEY=$(az storage account keys list --resource-group $ACI_PERS_RESOURCE_GROUP --account-name $ACI_PERS_STORAGE_ACCOUNT_NAME --query "[0].value" --output tsv)
echo $STORAGE_KEY
```

## <a name="deploy-container-and-mount-volume"></a>Birim kapsayıcısı ve bağlama dağıtma

Bir kapsayıcıdaki bir birimi olarak Azure dosya paylaşımını bağlayabilmeniz için paylaşım ve birim bağlama noktası kapsayıcı ile oluşturduğunuzda belirtin [az kapsayıcı oluşturma][az-container-create]. Önceki adımları izlediyseniz bir kapsayıcı oluşturmak için aşağıdaki komutu kullanarak daha önce oluşturduğunuz paylaşımı bağlayabilir:

```azurecli-interactive
az container create \
    --resource-group $ACI_PERS_RESOURCE_GROUP \
    --name hellofiles \
    --image mcr.microsoft.com/azuredocs/aci-hellofiles \
    --dns-name-label aci-demo \
    --ports 80 \
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --azure-file-volume-account-key $STORAGE_KEY \
    --azure-file-volume-share-name $ACI_PERS_SHARE_NAME \
    --azure-file-volume-mount-path /aci/logs/
```

`--dns-name-label` değeri, kapsayıcı örneğini oluşturduğunuz Azure bölgesi içinde benzersiz olmalıdır. Alırsanız önceki komutta değeri güncelleştirin bir **DNS ad etiketi** komutu yürütürken bir hata iletisi.

## <a name="manage-files-in-mounted-volume"></a>Takılan birimin dosyalarını yönetme

Kapsayıcı başlatıldığında sonra Microsoft dağıtılan basit web uygulaması kullanabilirsiniz [Acı hellofiles] [ aci-hellofiles] küçük metin dosyaları, belirtilen bağlama yolu Azure dosya paylaşımını oluşturmak için görüntü. Web uygulaması'nın tam etki alanı adı (FQDN) ile elde [az container show] [ az-container-show] komutu:

```azurecli-interactive
az container show --resource-group $ACI_PERS_RESOURCE_GROUP --name hellofiles --query ipAddress.fqdn
```

Kullanabileceğiniz [Azure portalında] [ portal] veya bir aracı gibi [Microsoft Azure Depolama Gezgini] [ storage-explorer] alıp yazılan dosyasını inceleyin Dosya Paylaşımı.

## <a name="mount-multiple-volumes"></a>Birden çok birim bağlama

Birden çok birim bir kapsayıcı örneğine bağlanacak kullanarak dağıtmalısınız bir [Azure Resource Manager şablonu](/azure/templates/microsoft.containerinstance/containergroups) veya bir YAML dosyası.

Bir şablonu kullanmak için paylaşım ayrıntılarını sağlayın ve doldurarak birimleri tanımlama `volumes` içindeki dizi `properties` şablon bölümü. Örneğin, adlı iki Azure dosya paylaşımlarını oluşturduysanız *share1* ve *share2* depolama hesabındaki *myStorageAccount*, `volumes` dizi görüneceği aşağıdakine benzer:

```JSON
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

```JSON
"volumeMounts": [{
  "name": "myvolume1",
  "mountPath": "/mnt/share1/"
},
{
  "name": "myvolume2",
  "mountPath": "/mnt/share2/"
}]
```

Kapsayıcı örneği dağıtımıyla bir Azure Resource Manager şablonu ile bir örneğini görmek için bkz: [bir kapsayıcı grubu dağıtma](container-instances-multi-container-group.md). Bir YAML dosyası kullanarak bir örnek için bkz [YAML ile çok kapsayıcılı bir grup dağıtma](container-instances-multi-container-yaml.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure Container ınstances'da diğer birim türleri bağlama işlemleri gerçekleştirmeyi öğreneceksiniz:

* [Azure kapsayıcı durumlarda emptyDir birim](container-instances-volume-emptydir.md)
* [Azure Container ınstances'da bir gitRepo birimi](container-instances-volume-gitrepo.md)
* [Azure Container ınstances'da bir gizli birimi](container-instances-volume-secret.md)

<!-- LINKS - External -->
[aci-hellofiles]: https://hub.docker.com/_/microsoft-azuredocs-aci-hellofiles 
[portal]: https://portal.azure.com
[storage-explorer]: https://storageexplorer.com

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-show]: /cli/azure/container#az-container-show