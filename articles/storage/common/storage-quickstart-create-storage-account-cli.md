---
title: "Azure hızlı başlangıç - Azure CLI kullanarak bir depolama hesabı oluşturma | Microsoft Docs"
description: "Hızlı bir şekilde Azure CLI kullanarak yeni bir depolama hesabı oluşturmayı öğrenin."
services: storage
documentationcenter: na
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 06/28/2017
ms.author: marsma
ms.openlocfilehash: b1fb2da4acf6e06219d790f2354cada4f1e34285
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-storage-account-using-the-azure-cli"></a>Azure CLI kullanarak bir depolama hesabı oluşturma

Azure CLI, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bir Azure Storage hesabı oluşturmak için Azure CLI kullanarak bu hızlı başlangıç ayrıntıları.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). 

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#create) komutuyla bir Azure kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Bu örnek, bir kaynak grubu oluşturur *myResourceGroup* içinde *eastus* bölge.

```azurecli-interactive
az group create \
    --name myResourceGroup \
    --location eastus
```

Belirtmek için hangi bölgede değilseniz `--location` parametresi, aboneliğinizle için desteklenen bölgelerin bir listesi alabilir [az hesap listesi-konumlarını](/cli/azure/account#list) komutu.

```azurecli-interactive
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```

## <a name="create-a-general-purpose-standard-storage-account"></a>Genel amaçlı standart depolama hesabı oluşturma

Her biri bir veya daha fazla depolama hizmet (BLOB'lar, dosyalar, tabloların veya kuyrukların) destekleyen farklı kullanım senaryoları için uygun çeşitli türlerde depolama hesapları vardır. Aşağıdaki tabloda kullanılabilir depolama hesabı türlerini ayrıntılarını verir.

|**Depolama hesabı türü**|**Genel amaçlı Standart**|**Genel amaçlı Premium**|**Blob depolama, sık erişimli ve seyrek erişimli erişim katmanları**|
|-----|-----|-----|-----|
|**Desteklenen hizmetler**| BLOB, dosya, tablo, kuyruk Hizmetleri | Blob Hizmeti | Blob Hizmeti|
|**Desteklenen blob türleri**|Blok blobları, sayfa blobları, ekleme blobları | Sayfa blobları | Blok blobları ve ek blobları|

[az storage account create](/cli/azure/storage/account#create) komutuyla bir genel amaçlı standart depolama hesabı oluşturun.

```azurecli-interactive
az storage account create \
    --name mystorageaccount \
    --resource-group myResourceGroup \
    --location eastus \
    --sku Standard_LRS \
    --encryption blob
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık herhangi bir kaynağa, kaynak grubunda gerekiyorsa, kaynak grubuyla silmek Bu hızlı başlangıç oluşturulan depolama hesabı da dahil olmak üzere [az grubu Sil](/cli/azure/group#delete) komutu.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç bir kaynak grubu ve genel amaçlı standart depolama hesabı oluşturuldu. Depolama hesabınız gelen ve giden veri aktarımı öğrenmek için hızlı başlangıç Blob depolama alanına devam edin.

> [!div class="nextstepaction"]
> [Azure CLI kullanarak Azure Blob storage aktarımı nesneleri](../blobs/storage-quickstart-blobs-cli.md)