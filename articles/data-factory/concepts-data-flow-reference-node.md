---
title: Azure veri fabrikası veri akışını referans düğümün eşleme
description: Data Factory veri akışı birleşimler, arama, birleşimler için bir başvuru düğüm ekler
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 641375c2b848957ffc0f5ad092a28460d91b6690
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56213565"
---
# <a name="azure-data-factory-mapping-data-flow-reference-node"></a>Azure veri fabrikası veri akışını referans düğümün eşleme

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Başvuru düğümü](media/data-flow/referencenode.png "başvuru düğümü")

Referans düğümün otomatik olarak bağlı olduğu düğüme tuval var olan başka bir düğümde başvuran geldiğiniz tuvaline eklenir. Bir referans düğümün bir işaretçi veya başka bir veri akışı dönüştürme için bir başvuru olarak düşünün.

Örneğin: Ne zaman, birleştirme veya birleşim birden fazla veri bir akış, veri akışı tuval adı ve birincil olmayan gelen akış ayarları yansıtan bir referans düğümün ekleyebilir.

Referans düğümün taşınmış veya silinmiş. Ancak, kaynak dönüştürme ayarlarını değiştirmek için düğümüne tıklayın.

Kullanılabilir alan ve dikey boşluğu satırlar arasında veri akışını referans düğümün eklediğinde yöneten UI kurallarını temel alır.
