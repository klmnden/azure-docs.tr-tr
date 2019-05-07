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
ms.openlocfilehash: 10b95a92f36ad6eb340ae864cbfd9fcbeac371a8
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65148763"
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



Bir galeri açıklamasını güncelleştirmek için [güncelleştirme AzGallery](https://docs.microsoft.com/powershell/module/az.compute/update-azgallery).

```azurepowershell-interactive
Update-AzGallery `
   -Name $gallery.Name ` 
   -ResourceGroupName $resourceGroup.Name
```

Bu örnek nasıl kullanılacağını gösterir [güncelleştirme AzGalleryImageDefinition](https://docs.microsoft.com/powershell/module/az.compute/update-azgalleryimagedefinition) bizim görüntü tanımı için yaşam bitiş tarihini güncelleştirmek için.

```azurepowershell-interactive
Update-AzGalleryImageDefinition `
   -GalleryName $gallery.Name `
   -Name $galleryImage.Name `
   -ResourceGroupName $resourceGroup.Name `
   -EndOfLifeDate 01/01/2030
```

Bu örnek nasıl kullanılacağını gösterir [güncelleştirme AzGalleryImageVersion](https://docs.microsoft.com/powershell/module/az.compute/update-azgalleryimageversion) olarak kullanılan bu görüntü sürümü dışlanacak *son* görüntü.

```azurepowershell-interactive
Update-AzGalleryImageVersion `
   -GalleryImageDefinitionName $galleryImage.Name `
   -GalleryName $gallery.Name `
   -Name $galleryVersion.Name `
   -ResourceGroupName $resourceGroup.Name `
   -PublishingProfileExcludeFromLatest
```


## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynakları silme, son öğenin iç içe geçmiş kaynaklar - görüntü sürümü ile başlamanız gerekir. Sürümleri silindikten sonra da görüntü tanımı silebilirsiniz. Altındaki tüm kaynakları silinene kadar galeri nelze odstranit.

```azurepowershell-interactive
$resourceGroup = "myResourceGroup"
$gallery = "myGallery"
$imageDefinition = "myImageDefinition"
$imageVersion = "myImageVersion"

Remove-AzGalleryImageVersion `
   -GalleryImageDefinitionName $imageDefinition `
   -GalleryName $gallery `
   -Name $imageVersion `
   -ResourceGroupName $resourceGroup

Remove-AzGalleryImageDefinition `
   -ResourceGroupName $resourceGroup `
   -GalleryName $gallery `
   -GalleryImageDefinitionName $imageDefinition

Remove-AzGallery `
   -Name $gallery `
   -ResourceGroupName $resourceGroup

Remove-AzResourceGroup -Name $resourceGroup
```

