---
title: include dosyası
description: include dosyası
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 11/11/2018
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: 4207031652db7ec4b2ae5468dc159320f6efdbc2
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228823"
---
## <a name="create-a-media-services-account"></a>Media Services hesabı oluşturma

İlk olarak bir Media Services hesabı oluşturmanız gerekir. Bu bölümde, Azure CLI kullanarak hesap oluşturma için gerekenler gösterilmektedir.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Aşağıdaki komutu kullanarak bir kaynak grubu oluşturun. Azure kaynak grubu; Azure Media Services hesapları ve ilişkili Depolama hesapları gibi kaynakların dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

```azurecli
az group create --name amsResourceGroup --location westus2
```

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Media Services hesabı oluştururken, bir Azure Depolama hesabı kaynağının adını sağlamanız gerekir. Belirtilen depolama hesabı, Media Services hesabınıza eklenir. 

Tek bir **Birincil** depolama hesabınız olması gerekir, ancak Media Services hesabınızla ilişkili istediğiniz sayıda **İkincil** depolama hesabınız olabilir. Media Services, **General-purpose v2** (GPv2) veya **General-purpose v1** (GPv1) hesaplarını destekler. Yalnızca blob hesaplarının **Birincil** olmasına izin verilmez. Depolama hesapları hakkında daha fazla bilgi edinmek istiyorsanız bkz. [Azure Depolama hesap seçenekleri](../articles/storage/common/storage-account-options.md). 

Depolama hesaplarının Media Services’ta nasıl kullanıldığı hakkında daha fazla bilgi için bkz. [Depolama hesapları](../articles/media-services/latest/storage-account-concept.md).

Aşağıdaki komut, Media Services Hesabı ile ilişkilendirilecek Depolama hesabını oluşturur. Aşağıdaki betikte `storageaccountforams` öğesini kendi değeriniz ile değiştirebilirsiniz. Hesap adı en fazla 24 karakter uzunluğunda olmalıdır.

```azurecli
az storage account create --name storageaccountforams \  
--kind StorageV2 \
--sku Standard_RAGRS \
--resource-group amsResourceGroup
```

### <a name="create-a-media-services-account"></a>Media Services hesabı oluşturma

Aşağıdaki Azure CLI komutu yeni bir Media Services hesabı oluşturur. Aşağıdaki değerleri değiştirebilirsiniz: `amsaccount` `storageaccountforams` (depolama hesabınız için verdiğiniz değerle eşleşmelidir) ve `amsResourceGroup` (kaynak grubu için verdiğiniz değerle eşleşmelidir).

```azurecli
az ams account create --name amsaccount --resource-group amsResourceGroup --storage-account storageaccountforams
```
