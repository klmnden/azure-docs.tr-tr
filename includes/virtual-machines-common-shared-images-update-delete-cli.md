---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 04/25/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 8d0f9866864ca4b02ca6238be2ac44537a586c2d
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188383"
---
## <a name="update-resources"></a>Kaynakları güncelleştirme

Ne güncelleştirilebilir bazı sınırlamalar vardır. Aşağıdaki öğeler güncelleştirilebilir: 

Paylaşılan görüntü Galerisi:
- Açıklama

görüntü tanımı:
- Önerilen Vcpu
- Önerilen bellek
- Açıklama
- Sonu yaşam tarihi

Görüntü sürümü:
- Bölgesel çoğaltma sayısı
- Hedef bölgeler
- En son çıkarma
- Sonu yaşam tarihi

Çoğaltma bölgeleri ekleme planlıyorsanız, kaynak yönetilen bir görüntü silmeyin. Kaynak yönetilen bir görüntü, görüntü sürümü başka bölgelere çoğaltmak için aşağıdakiler gereklidir. 

Bir galeri kullanarak Güncelleştirmesi ([az sig güncelleştirme](https://docs.microsoft.com/cli/azure/sig?view=azure-cli-latest#az-sig-update). 

```azurecli-interactive
az sig update \
   --gallery-name myGallery \
   --resource-group myGalleryRG \
   --set description="My updated description."
```


Kullanarak bir görüntü tanım güncelleştirmesi [az sig görüntü tanım güncelleştirmesi](https://docs.microsoft.com/cli/azure/sig/image-definition?view=azure-cli-latest#az-sig-image-definition-update).

```azurecli-interactive
az sig image-definition update \
   --gallery-name myGallery\
   --resource-group myGalleryRG \
   --gallery-image-definition myImageDefinition \
   --set description="My updated description."
```

Bir bölge kullanmaya çoğaltmak için eklemek için bir görüntü sürümü güncelleştirme [az sig görüntü sürümü güncelleştirme](https://docs.microsoft.com/cli/azure/sig/image-definition?view=azure-cli-latest#az-sig-image-definition-update). Bu değişiklik, görüntünün yeni bölgesine çoğaltılmış olarak biraz zaman alır.

```azurecli-interactive
az sig image-version update \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 \
   --add publishingProfile.targetRegions  name=eastus
```

## <a name="delete-resources"></a>Kaynakları silme

Görüntü sürümü ilk silerek ters sırada kaynakları silmeniz gerekir. Tüm görüntü sürümlerinin sildikten sonra da görüntü tanımı silebilirsiniz. Tüm görüntü tanımlarını sildikten sonra galerinin silebilirsiniz. 

Kullanarak bir görüntü sürümü Sil [az sig görüntü sürümü Sil](https://docs.microsoft.com/cli/azure/sig/image-version?view=azure-cli-latest#az-sig-image-version-delete).

```azurecli-interactive
az sig image-version delete \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 
```

Kullanarak bir görüntü tanımı Sil [az sig görüntü tanımı Sil](https://docs.microsoft.com/cli/azure/sig/image-definition?view=azure-cli-latest#az-sig-image-definition-delete).

```azurecli-interactive
az sig image-definition delete \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition
```


Bir galeri görüntüsü kullanarak silme [az sig Sil](https://docs.microsoft.com/cli/azure/sig?view=azure-cli-latest#az-sig-delete).

```azurecli-interactive
az sig delete \
   --resource-group myGalleryRG \
   --gallery-name myGallery
```
