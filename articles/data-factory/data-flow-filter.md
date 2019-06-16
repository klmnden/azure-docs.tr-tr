---
title: Azure veri fabrikası veri akışı filtre dönüştürme eşlemesi
description: Azure veri fabrikası veri akışı filtre dönüştürme eşlemesi
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: e0b41850c149ff7095333cf77b780dec1f03b882
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66234424"
---
# <a name="azure-data-factory-filter-transformation"></a>Azure veri fabrikası filtre dönüştürme

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
