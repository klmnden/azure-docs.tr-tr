---
title: Azure veri fabrikası veri akışını referans düğümün eşleme
description: Data Factory veri akışı birleşimler, arama, birleşimler için bir başvuru düğüm ekler
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 626943143e8fa193f143e66d856d9b00e3589fb5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61262692"
---
# <a name="mapping-data-flow-reference-node"></a>Veri akışı referans düğümün eşleme

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Başvuru düğümü](media/data-flow/referencenode.png "başvuru düğümü")

Referans düğümün otomatik olarak bağlı olduğu düğüme tuval var olan başka bir düğümde başvuran geldiğiniz tuvaline eklenir. Bir referans düğümün bir işaretçi veya başka bir veri akışı dönüştürme için bir başvuru olarak düşünün.

Örneğin: Ne zaman, birleştirme veya birleşim birden fazla veri bir akış, veri akışı tuval adı ve birincil olmayan gelen akış ayarları yansıtan bir referans düğümün ekleyebilir.

Referans düğümün taşınmış veya silinmiş. Ancak, kaynak dönüştürme ayarlarını değiştirmek için düğümüne tıklayın.

Kullanılabilir alan ve dikey boşluğu satırlar arasında veri akışını referans düğümün eklediğinde yöneten UI kurallarını temel alır.
