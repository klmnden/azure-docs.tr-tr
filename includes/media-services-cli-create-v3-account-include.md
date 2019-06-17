---
title: include dosyası
description: include dosyası
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 05/01/2019
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: feec6a695ad867d26d32904d020648b029f9da35
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66155743"
---
## <a name="create-a-media-services-account"></a>Media Services hesabı oluşturma

İlk olarak bir Media Services hesabı oluşturmanız gerekir. Bu bölümde, Azure CLI kullanarak hesap oluşturma için gerekenler gösterilmektedir.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Aşağıdaki komutu kullanarak bir kaynak grubu oluşturun. Azure kaynak grubu; Azure Media Services hesapları ve ilişkili Depolama hesapları gibi kaynakların dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Yerine, `amsResourceGroup` , değerine sahip.

```azurecli
az group create --name amsResourceGroup --location westus2
```

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Media Services hesabı oluştururken, bir Azure Depolama hesabı kaynağının adını sağlamanız gerekir. Belirtilen depolama hesabı, Media Services hesabınıza eklenir. Depolama hesaplarının Media Services’ta nasıl kullanıldığı hakkında daha fazla bilgi için bkz. [Depolama hesapları](../articles/media-services/latest/storage-account-concept.md).

Tek bir **Birincil** depolama hesabınız olması gerekir, ancak Media Services hesabınızla ilişkili istediğiniz sayıda **İkincil** depolama hesabınız olabilir. Media Services, **General-purpose v2** (GPv2) veya **General-purpose v1** (GPv1) hesaplarını destekler. Yalnızca blob hesaplarının **Birincil** olmasına izin verilmez. Depolama hesapları hakkında daha fazla bilgi edinmek istiyorsanız bkz. [Azure Depolama hesap seçenekleri](../articles/storage/common/storage-account-options.md). 

Bu örnekte, oluştururuz genel amaçlı v2, standart LRS hesabı. Depolama hesapları ile denemek istiyorsanız, kullanın `--sku Standard_LRS`. Ancak, üretim için bir SKU seçilmesi sırasında dikkate almanız gereken, `--sku Standard_RAGRS`, coğrafi çoğaltma için iş sürekliliği sağlar. Daha fazla bilgi için [depolama hesapları](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest).
 
Aşağıdaki komut, Media Services Hesabı ile ilişkilendirilecek Depolama hesabını oluşturur. Aşağıdaki betikte `storageaccountforams` öğesini kendi değeriniz ile değiştirebilirsiniz. `amsResourceGroup` Önceki adımda kaynak grubu için verdiğiniz değerle eşleşmelidir. Depolama hesabı adı, 24'den az uzunlukta olmalıdır.

```azurecli
az storage account create --name storageaccountforams \  
  --kind StorageV2 \
  --sku Standard_LRS \
  -l westus2 \
  -g amsResourceGroup
```

### <a name="create-a-media-services-account"></a>Media Services hesabı oluşturma

Aşağıdaki Azure CLI komutu yeni bir Media Services hesabı oluşturur. Aşağıdaki değerleri değiştirebilirsiniz: `amsaccount` `storageaccountforams` (depolama hesabınız için verdiğiniz değerle eşleşmelidir) ve `amsResourceGroup` (kaynak grubu için verdiğiniz değerle eşleşmelidir).

```azurecli
az ams account create --name amsaccount \
  -l westus2 \
  -g amsResourceGroup --storage-account storageaccountforams
```
