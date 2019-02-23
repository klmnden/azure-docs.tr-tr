---
title: Azure veri fabrikası veri akışı filtre dönüştürme eşlemesi
description: Azure veri fabrikası veri akışı filtre dönüştürme eşlemesi
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: 1930ca7406b6ef1b9fd20e1651bc031150dd952e
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2019
ms.locfileid: "56729552"
---
# <a name="azure-data-factory-mapping-data-flow-filter-transformation"></a>Azure veri fabrikası veri akışı filtre dönüştürme eşlemesi

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Filtre dönüşümleri satır filtreleme sağlar. Filtre koşulu tanımlayan bir ifade oluşturun. İfade oluşturucuyu başlatmak için metin kutusuna tıklayın. Hangi satır, geçerli veri akışı (filtre) geçmek için İleri dönüşümü izin verileceğini denetlemek için bir filtre ifadesi ifade oluşturucu içinde oluşturun.

Yani loan_status sütunu Filtrele:

```
in([‘Default’, ‘Charged Off’, ‘Fully Paid’], loan_status).
```

Filmler tanıtım yıl sütunu filtreleyin:

```
year > 1980
```
