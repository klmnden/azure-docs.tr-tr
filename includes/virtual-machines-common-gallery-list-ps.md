---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 11/07/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: a55653c0f23be594fe65e7a322c11edc37ee1ce6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60188359"
---
## <a name="shared-image-management"></a>Paylaşılan Görüntü Yönetimi 

Genel Yönetim görevleri ve bunları nasıl bazı örnekleri aşağıda verilmiştir PowerShell kullanarak.

Tüm galeri ada göre listele.

```azurepowershell-interactive
$galleries = Get-AzResource -ResourceType Microsoft.Compute/galleries
$galleries.Name
```

Tüm görüntü tanımlarını ada göre listeler.

```azurepowershell-interactive
$imageDefinitions = Get-AzResource -ResourceType Microsoft.Compute/galleries/images
$imageDefinitions.Name
```

Tüm görüntü sürümleri ada göre listele.

```azurepowershell-interactive
$imageVersions = Get-AzResource -ResourceType Microsoft.Compute/galleries/images/versions
$imageVersions.Name
```

Görüntü sürümü silin. Bu örnek adlı görüntü sürümü siler *1.0.0*.

```azurepowershell-interactive
Remove-AzGalleryImageVersion `
   -GalleryImageDefinitionName myImageDefinition `
   -GalleryName myGallery `
   -Name 1.0.0 `
   -ResourceGroupName myGalleryRG
```





