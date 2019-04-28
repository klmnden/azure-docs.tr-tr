---
title: Azure veri fabrikası veri akışı sıralama dönüştürme eşlemesi
description: Azure veri fabrikası veri sıralama dönüştürme eşlemesi
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/08/2018
ms.openlocfilehash: d0482d1081c16dc89e7371c4c33de9b2bb4e4c2e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61348387"
---
# <a name="azure-data-factory-data-flow-sort-transformations"></a>Azure Data Factory veri akışı sıralama dönüşümleri

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Sıralama ayarları](media/data-flow/sort.png "sıralama")

Sıralama dönüşümü geçerli veri akışını gelen satırda sıralamanıza olanak sağlar. Sıralama dönüşümünde giden satırların sonradan ayarladığınız sıralama kuralları izler. Her bir sütun seçin ve her alanın yanındaki oku göstergesi kullanarak ASC veya Ara, sıralayın. Sütun sıralama uygulamadan önce değiştirmeniz gerekiyorsa, "Hesaplanan ifade Düzenleyicisi'ni başlatmak için sütunlarda"'a tıklayın. Bu, bir ifade yalnızca bir sütun sıralama için uygulama yerine sıralama işlemi için derleme olanağı sağlar.

## <a name="case-insensitive"></a>Büyük/Küçük harfe duyarsız
Dize veya metin alanları sıralarken durumu yok saymak istiyorsanız "Büyük küçük harfe duyarlı üzerinde" kapatabilirsiniz.

Spark veri bölümleme "Sıralama bölümler yalnızca içinde" yararlanır. Her bölüm yalnızca içinde gelen veri sıralayarak veri akışları yerine tüm veri akışını sıralama bölümlenmiş verileri sıralayabilirsiniz.

Her bir sıralama dönüşümünde sıralama koşul yeniden. Bu nedenle sıralama önceliği daha yüksek bir sütuna taşımanız gerekirse, satır farenizle alın ve daha yüksek veya düşük sıralama listesinde taşıyabilirsiniz.

Sıralama bölümleme etkileri

ADF veri akışı, büyük veri Spark kümeleri, birden çok düğümlerine ve bölümlerine yayılmaz dağıtılmış veriler üzerinde yürütülür. Bu, aynı sırada verileri tutmak için sıralama dönüştürme bağlı değilse, veri akışı mimarileri oluşturma zaman içinde göz önünde bulundurmanız önemlidir. Bir sonraki dönüştürme verilerinizdeki bölümlemek kullanmayı tercih ederseniz söz konusu verilerin reshuffling nedeniyle sıralama kaybedebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Sıraladıktan sonra kullanmak isteyebilirsiniz [toplama dönüşümü](data-flow-aggregate.md)
