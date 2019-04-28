---
title: Azure veri fabrikası veri akışı filtre dönüştürme eşlemesi
description: Azure veri fabrikası veri akışı filtre dönüştürme eşlemesi
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: d1751c47ad4507260d9f8d6ea44fcb32ed0e7338
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61347438"
---
# <a name="azure-data-factoryfilter-transformation"></a>Azure veri FactoryFilter dönüştürme

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Filtre dönüşümleri satır filtreleme sağlar. Filtre koşulu tanımlayan bir ifade oluşturun. İfade oluşturucuyu başlatmak için metin kutusuna tıklayın. Hangi satır, geçerli veri akışı (filtre) geçmek için İleri dönüşümü izin verileceğini denetlemek için bir filtre ifadesi ifade oluşturucu içinde oluşturun. Filtre dönüşümü SQL deyiminin WHERE yan tümcesi olarak düşünün.

## <a name="filter-on-loanstatus-column"></a>Loan_status sütunu Filtrele:

```
in([‘Default’, ‘Charged Off’, ‘Fully Paid’], loan_status).
```

Filmler tanıtım yıl sütunu filtreleyin:

```
year > 1980
```

## <a name="next-steps"></a>Sonraki adımlar

Dönüştürme, filtre sütunu deneyin [dönüştürme seçin](data-flow-select.md)
