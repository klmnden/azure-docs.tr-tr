---
title: Azure veri fabrikası veri akışı toplama dönüştürme eşlemesi
description: Azure Data Factory veri akışı toplama dönüştürme
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: 7b488b243c0520befb6b5470598f460b5a759fed
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61467386"
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
