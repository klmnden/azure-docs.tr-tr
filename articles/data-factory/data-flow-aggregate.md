---
title: Azure veri fabrikası veri akışı toplama dönüştürme eşlemesi
description: Azure Data Factory veri akışı toplama dönüştürme
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: 3793c49da32845d559d73faa25c571d3f86e062f
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56272156"
---
# <a name="azure-data-factory-mapping-data-flow-aggregate-transformation"></a>Azure veri fabrikası veri akışı toplama dönüştürme eşlemesi

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Burada, veri akışlarını toplamalar sütunlar tanımlarsınız. toplama dönüşümdür. İfade Oluşturucu'da toplamalar (yani Topla, MIN, MAX, sayısı, vb.) farklı türlerini tanımlamak ve isteğe bağlı gruplandırma ölçütü alanları ile bu toplamalar içeren çıkış, yeni bir alan oluşturamazsınız.

![Dönüştürme seçenekleri toplama](media/data-flow/agg.png "toplam 1")

## <a name="group-by"></a>Gruplandırma Ölçütü
(İsteğe bağlı) Group by yan, toplama için seçin ve var olan bir sütunun adını ya da yeni bir ad kullanın. Kullanım "Sütun Ekle", daha fazla group by yan tümcesi ekleyin ve ya da yalnızca varolan bir sütun, sütunlar veya ifadeleri, gruplandırma birleşimi seçmek için ifade oluşturucuyu başlatmak için sütun adının yanındaki metin kutusuna tıklayın.

## <a name="the-aggregate-column-tab"></a>Toplama sütunu sekmesi 
(Gerekli) Toplama ifadeleri oluşturmak için toplama sütunu sekmesini seçin. Varolan bir sütunla toplama değerin üzerine yazmayı seçin veya toplama için Yeni adla yeni bir alan oluşturabilirsiniz. Sağ sütun adı seçiciyi yanlarındaki toplama için kullanmak istediğiniz ifadesi girilir. Bu metin kutusunu tıklatarak ifade oluşturucuyu açılır.

![Dönüştürme seçenekleri toplama](media/data-flow/agg2.png "toplayıcısı")

## <a name="data-preview-in-expression-builder"></a>Veri önizleme ifade oluşturucusu

Hata ayıklama modunda ifade Oluşturucu ile toplama işlevleri veri önizlemelerinin üretemiyor. Veri önizlemelerini toplama dönüşümlere görüntülemek için İfade Oluşturucusu'nu kapatın ve veri profili veri akışı Tasarımcısı'ndan görüntüleyin.
