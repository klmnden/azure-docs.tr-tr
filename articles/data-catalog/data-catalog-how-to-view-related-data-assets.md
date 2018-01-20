---
title: "Azure veri Kataloğu'nda ilgili veri varlıklarını görüntülemek nasıl | Microsoft Docs"
description: "Bu makalede, Azure veri Kataloğu'nda, seçilen veri varlığını ilgili veri varlıklarını görüntülemek açıklanmaktadır."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 01/18/2018
ms.author: maroche
ms.openlocfilehash: 37d12209d28b73f0d7fc6d940ded344fbeae968d
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-view-related-data-assets-in-azure-data-catalog"></a>Azure veri Kataloğu'nda ilgili veri varlıklarını görüntülemek nasıl?
Azure veri Kataloğu, aralarında seçili veri varlığına ve görünümü ilişkiler ilgili veri varlıklarını görüntülemenizi sağlar. 

## <a name="supported-data-sources"></a>Desteklenen veri kaynakları 
Aşağıdaki veri kaynaklarından veri varlıklarını kaydetme, Azure veri Kataloğu seçilen veri varlıklarının birleştirme ilişkilerini hakkındaki meta verileri otomatik olarak kaydeder. 

- SQL Server
- Azure SQL Database
- MySQL
- Oracle

> [!NOTE]
> Veri Kataloğu'nın iki veri varlıklar arasındaki ilişki içeri aktarmak aynı anda hem varlıkları kaydetmeniz gerekir. Bunlardan birini ayrı ayrı eklediyseniz, tekrar ve bunlar arasındaki ilişkinin almak için diğer veri varlığına ekleyin.

## <a name="view-related-data-assets"></a>İlgili veri varlıklarını görüntülemek
Seçilen bir veri kümesine ilgili veri varlıklarını görüntülemek için kullanın **ilişkileri** sekmesinde aşağıdaki resimde gösterildiği gibi: 

![Azure veri Kataloğu - veri varlıklarını ilgili görüntüleyin](media\data-catalog-how-to-view-related-data-assets\relationships-tab.png)

Bu örnekte, seçilen iki ilişkisi vardır **ProductSubcategory** veri varlığına: 

- Ürün tablosunun ProductSubcategoryID sütunu seçili ProductSubcategory tablosunun ProductSubcategoryID sütunla yabancı anahtar ilişkisi vardır. 
- ProductSubCategory tablosunun ProductCategoryID sütunu seçili ProductCategory tablosunun ProductCategoryID sütunla yabancı anahtar ilişkisi vardır.

> [!NOTE]
> İlişkileri ağaç görünümünde ok yönünü dikkat edin.  

Sütun tam adı gibi daha fazla ayrıntı için fare üzerine getirin ve aşağıdaki görüntüye benzer popup bakın: 

![Azure veri Kataloğu - ilişki açılan](media\data-catalog-how-to-view-related-data-assets\relationship-popup.png)

Zaten kayıtlı varlıklar arasındaki ilişkiler eklemek için bu varlıkları yeniden kaydedin.

## <a name="next-steps"></a>Sonraki adımlar
- [Veri varlıklarını yönetme](data-catalog-how-to-manage.md)