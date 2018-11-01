---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: c5cb1c5a005265950f8dcd02c8e7675918751dab
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50165346"
---
## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#az_group_create) komutuyla bir Azure kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

```azurecli-interactive
az group create \
    --name myResourceGroup \
    --location eastus
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

[az storage account create](/cli/azure/storage/account#create) komutuyla bir genel amaçlı depolama hesabı oluşturun. Genel amaçlı depolama hesabı; bloblar, dosyalar, tablolar ve kuyruklar olmak üzere dört hizmet için de kullanılabilir. 

```azurecli-interactive
az storage account create \
    --name mystorageaccount \
    --resource-group myResourceGroup \
    --location eastus \
    --sku Standard_LRS \
    --encryption blob
```

## <a name="specify-storage-account-credentials"></a>Depolama hesabının kimlik bilgilerini belirtme

Bu öğreticideki komutların çoğu için Azure CLI’sının depolama hesabı kimlik bilgilerinize ihtiyacı vardır. Bunu yapmak için birkaç seçenek olsa da bunları sağlamanın en kolay yollarından biri `AZURE_STORAGE_ACCOUNT` ve `AZURE_STORAGE_ACCESS_KEY` ortam değişkenlerini ayarlamaktır.

Öncelikle [az storage account keys list](/cli/azure/storage/account/keys#list) komutunu kullanarak depolama hesabı anahtarlarınızı görüntüleyin:

```azurecli-interactive
az storage account keys list \
    --account-name mystorageaccount \
    --resource-group myResourceGroup \
    --output table
```

Şimdi `AZURE_STORAGE_ACCOUNT` ve `AZURE_STORAGE_ACCESS_KEY` ortam değişkenlerini ayarlayın. Bunu Bash kabuğunda `export` komutunu kullanarak gerçekleştirebilirsiniz:

```bash
export AZURE_STORAGE_ACCOUNT="mystorageaccountname"
export AZURE_STORAGE_ACCESS_KEY="myStorageAccountKey"
```