---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 09/20/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 1e78109472668c0f9a73af6430253a0d709979af
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66156149"
---
## <a name="using-rbac-to-share-images"></a>Görüntüleri paylaşmak için RBAC kullanma

Rol tabanlı erişim denetimi (RBAC) kullanarak abonelikler arasında görüntüleri paylaşabilir. Bir görüntü sürümü bile abonelikler arasında okuma izni herhangi bir kullanıcı görüntü sürümünü kullanarak bir sanal makineyi dağıtmak mümkün olacaktır.

RBAC kullanarak kaynakları paylaşma hakkında daha fazla bilgi için bkz. [RBAC ve Azure CLI kullanarak erişimini yönetme](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli).


## <a name="list-information"></a>Liste bilgileri

Konum, durum ve kullanılabilir resim galerileri kullanma hakkındaki diğer bilgileri alma [az sig listesi](/cli/azure/sig#az-sig-list).

```azurecli-interactive 
az sig list -o table
```

İşletim sistemi türü ve durumu hakkında bilgiler dahil olmak üzere, kullanarak bir galeride görüntü tanımları listesi [az sig görüntü tanım listesi](/cli/azure/sig/image-definition#az-sig-image-definition-list).

```azurecli-interactive 
az sig image-definition list -g myGalleryRG -r myGallery -o table
```

Bir galeride bulunan paylaşılan görüntü sürümleri listesi kullanarak [az sig görüntü sürüm listesi](/cli/azure/sig/image-version#az-sig-image-version-list).

```azurecli-interactive
az sig image-version list -g myGalleryRG -r myGallery -i myImageDefinition -o table
```

Kullanarak bir görüntü sürüm Kimliğini alın [az sig görüntü sürümü göster](/cli/azure/sig/image-version#az-sig-image-version-show).

```azurecli-interactive
az sig image-version show \
   -g myGalleryRG \
   -r myGallery \
   -i myImageDefinition \
   --gallery-image-version 1.0.0 \
   --query "id"
```