---
title: Azure veri fabrikası veri akışı filtre dönüştürme eşlemesi
description: Azure veri fabrikası veri akışı filtre dönüştürme eşlemesi
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: b7e7b123560aae3a2d3086c8536969297d31f7ba
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56272142"
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
