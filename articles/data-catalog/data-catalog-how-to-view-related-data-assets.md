---
title: Azure veri Kataloğu'nda ilgili veri varlıklarını görüntüleme
description: Bu makalede, Azure veri Kataloğu'nda, seçilen veri varlığının ilgili veri varlıklarını görüntülemek açıklanmaktadır.
services: data-catalog
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: b01c328812113ad721b7632978ad28e54a6a3ef1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61000136"
---
# <a name="how-to-view-related-data-assets-in-azure-data-catalog"></a>Azure veri Kataloğu'nda ilgili veri varlıklarını görüntülemek nasıl?
Azure veri Kataloğu, aralarında seçili veri varlığına ve görünümü ilişkiler ilgili veri varlıklarını görüntülemenizi sağlar. 

## <a name="supported-data-sources"></a>Desteklenen veri kaynakları 
Aşağıdaki veri kaynaklarından veri varlıklarını kaydetme, Azure veri Kataloğu, seçili veri varlıkları arasında birleştirme ilişkilerini hakkındaki meta verileri otomatik olarak kaydeder. 

- SQL Server
- Azure SQL Veritabanı
- MySQL
- Oracle

> [!NOTE]
> Veri Kataloğu'nın iki veri varlıkları arasındaki ilişkileri içeri aktarmak aynı anda hem varlıkları kaydetmeniz gerekir. Bunlardan biri ayrı olarak eklemeden yeniden ve aralarındaki ilişkiyi almak için diğer veri varlığı ekleyin.

## <a name="view-related-data-assets"></a>İlgili veri varlıklarını görüntüleme
Seçilen bir veri kümesi için ilgili veri varlıklarını görüntülemek için kullanın **ilişkileri** sekmesinde aşağıdaki görüntüde gösterildiği gibi: 

![Azure veri Kataloğu - veri varlıkları ile ilgili görüntüleyin](media/data-catalog-how-to-view-related-data-assets/relationships-tab.png)

Bu örnekte, seçilen iki ilişkisi vardır **ProductSubcategory** veri varlığına: 

- Product tablosunda ProductSubcategoryID sütunu seçili ProductSubcategory tablosunun ProductSubcategoryID sütunla yabancı anahtar ilişkisi vardır. 
- ProductSubCategory tablosunda ProductCategoryID sütun, seçili ProductCategory tablosunun ProductCategoryID sütunla yabancı anahtar ilişkisi vardır.

> [!NOTE]
> Yön okun ilişkileri ağaç görünümünde dikkat edin.  

Sütunun tam adı gibi daha fazla ayrıntı görmek için fareyi üzerine getirin ve aşağıdaki görüntüye benzer bir açılır pencere görürsünüz: 

![Azure veri Kataloğu - ilişki açılan menüsü](media/data-catalog-how-to-view-related-data-assets/relationship-popup.png)

Zaten kayıtlı varlıklar arasında ilişki eklemek için bu varlıkları yeniden kaydedin.

## <a name="next-steps"></a>Sonraki adımlar
- [Veri varlıklarını yönetme](data-catalog-how-to-manage.md)