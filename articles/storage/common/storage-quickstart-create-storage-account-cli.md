---
title: "Azure Hızlı Başlangıç - Azure CLI’sini kullanarak depolama hesabı oluşturma | Microsoft Docs"
description: "Azure CLI kullanarak hızlı bir şekilde yeni bir depolama hesabı oluşturmayı öğrenin."
services: storage
documentationcenter: na
author: tamram
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
ms.openlocfilehash: 7186c5e2ce94d06b21d95a557e960b82e268cdce
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="create-a-storage-account-using-the-azure-cli"></a>Azure CLI kullanarak depolama hesabı oluşturma

Azure CLI, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu Hızlı Başlangıç’ta bir Azure Depolama hesabı oluşturmak için Azure CLI kullanma hakkında ayrıntılı bilgiler verilmektedir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). 

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#create) komutuyla bir Azure kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Bu örnek *eastus* bölgesinde *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create \
    --name myResourceGroup \
    --location eastus
```

`--location` parametresi için hangi bölgeyi belirteceğinizden emin değilseniz, [az account list-locations](/cli/azure/account#list) komutuyla aboneliğiniz için desteklenen bölgelerin bir listesini alabilirsiniz.

```azurecli-interactive
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```

## <a name="create-a-general-purpose-standard-storage-account"></a>Genel amaçlı standart depolama hesabı oluşturma

Her biri bir veya daha çok depolama hizmetini (blob, dosya, tablo veya kuyruk) destekleyen, farklı kullanım senaryoları için uygun birkaç depolama hesabı türü vardır. Aşağıdaki tabloda kullanılabilir hesap türlerinin ayrıntıları bulunur.

|**Depolama hesabı türü**|**Genel amaçlı Standart**|**Genel amaçlı Premium**|**Blob depolama, sık erişimli ve seyrek erişimli erişim katmanları**|
|-----|-----|-----|-----|
|**Desteklenen hizmetler**| Blob, Dosya, Tablo, Kuyruk hizmetleri | Blob hizmeti | Blob hizmeti|
|**Desteklenen blob türleri**|Blok blobları, sayfa blobları, ek bloblar | Sayfa blobları | Blok blobları ve ek blobları|

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

Bu Hızlı Başlangıç öğreticisinde oluşturduğunuz depolama hesabı dahil, kaynak grubunuzdaki hiçbir kaynağa artık ihtiyacınız yoksa [az group delete](/cli/azure/group#delete) komutuyla kaynak grubunu silin.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu Hızlı Başlangıç’ta, bir kaynak grubu ve genel amaçlı bir standart depolama hesabı oluşturdunuz. Depolama hesabınızdan verileri aktarmayı öğrenmek için, Blob depolama Hızlı Başlangıcı ile devam edin.

> [!div class="nextstepaction"]
> [Azure CLI kullanarak nesneleri Azure Blob depolama içine ve dışına aktarma](../blobs/storage-quickstart-blobs-cli.md)