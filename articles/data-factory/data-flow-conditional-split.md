---
title: Azure veri fabrikası veri akışı koşullu bölme dönüştürme eşlemesi
description: Azure Data Factory veri akışı koşullu bölme dönüştürme
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: a4ea79e05165dfae4f79aa6473a07151ba7c00fc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60627645"
---
# <a name="azure-data-factory-mapping-data-flow-conditional-split-transformation"></a>Azure veri fabrikası veri akışı koşullu bölme dönüştürme eşlemesi

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Koşullu bölünmüş dönüşümü, veri içeriğini bağlı olarak farklı akışlara veri satırları yönlendirebilirsiniz. Koşullu bölünmüş dönüşümü uygulaması, programlama dilinde büyük/küçük harf karar yapısına benzer. Dönüştürme ifadeleri değerlendirir ve sonuçlarına göre belirtilen akış veri satırına yönlendirir. Herhangi bir ifade bir satır eşleşiyorsa varsayılan çıkışa yönlendirilir, bu dönüşümü bir varsayılan çıkış de sağlar.

![Koşullu bölünmüş](media/data-flow/cd1.png "koşullu Böl")

Ek koşullar eklemek için "Ekle Stream" alt yapılandırma bölmesinde seçin ve ifadeniz derlemek için ifade oluşturucu metin kutusuna tıklayın.
