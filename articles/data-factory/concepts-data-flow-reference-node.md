---
title: Azure veri fabrikası veri akışını referans düğümün eşleme
description: Data Factory veri akışı birleşimler, arama, birleşimler için bir başvuru düğüm ekler
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 2e318210d96822b13f65eadeef79798b1b4595d1
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/16/2019
ms.locfileid: "56325906"
---
# <a name="mapping-data-flow-reference-node"></a>Veri akışı referans düğümün eşleme

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Başvuru düğümü](media/data-flow/referencenode.png "başvuru düğümü")

Referans düğümün otomatik olarak bağlı olduğu düğüme tuval var olan başka bir düğümde başvuran geldiğiniz tuvaline eklenir. Bir referans düğümün bir işaretçi veya başka bir veri akışı dönüştürme için bir başvuru olarak düşünün.

Örneğin: Ne zaman, birleştirme veya birleşim birden fazla veri bir akış, veri akışı tuval adı ve birincil olmayan gelen akış ayarları yansıtan bir referans düğümün ekleyebilir.

Referans düğümün taşınmış veya silinmiş. Ancak, kaynak dönüştürme ayarlarını değiştirmek için düğümüne tıklayın.

Kullanılabilir alan ve dikey boşluğu satırlar arasında veri akışını referans düğümün eklediğinde yöneten UI kurallarını temel alır.
