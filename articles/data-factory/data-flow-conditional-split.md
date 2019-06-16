---
title: Azure veri fabrikası veri akışı koşullu bölme dönüştürme eşlemesi
description: Azure Data Factory veri akışı koşullu bölme dönüştürme
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: f9fd346d4c4eaed0797d564fe52dd44e9f0e240a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65795678"
---
# <a name="mapping-data-flow-conditional-split-transformation"></a>Bölme dönüştürme eşlemesi veri akışı koşullu

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Araç kutusu bölme koşullu](media/data-flow/conditionalsplit2.png "koşullu araç kutusu Böl")

Koşullu bölünmüş dönüşümü, veri içeriğini bağlı olarak farklı akışlara veri satırları yönlendirebilirsiniz. Koşullu bölünmüş dönüşümü uygulaması, programlama dilinde büyük/küçük harf karar yapısına benzer. Dönüştürme ifadeleri değerlendirir ve sonuçlarına göre belirtilen akış veri satırına yönlendirir. Herhangi bir ifade bir satır eşleşiyorsa varsayılan çıkışa yönlendirilir, bu dönüşümü bir varsayılan çıkış de sağlar.

![Koşullu bölünmüş](media/data-flow/conditionalsplit1.png "koşullu bölme seçenekleri")

## <a name="multiple-paths"></a>Birden çok yol

Ek koşullar eklemek için "Ekle Stream" alt yapılandırma bölmesinde seçin ve ifadeniz derlemek için ifade oluşturucu metin kutusuna tıklayın.

![Koşullu multi bölme](media/data-flow/conditionalsplit3.png "koşullu multi Böl")

## <a name="next-steps"></a>Sonraki adımlar

Ortak veri akışı dönüşümleri koşullu bölmesiyle kullanılan: [Dönüştürme katılın](data-flow-join.md), [Loopup dönüştürme](data-flow-lookup.md), [dönüştürme seçin](data-flow-select.md)
