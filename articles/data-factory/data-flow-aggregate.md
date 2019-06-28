---
title: Toplama dönüşümü eşleme veri akışı - Azure Data Factory | Microsoft Docs
description: Azure Data Factory ile eşleme veri akışı toplama dönüşümü bir ölçekte veri toplama hakkında bilgi edinin.
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: 21135b26d4bc840b3fcb091e675e5e6bd24d8548
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67312095"
---
# <a name="aggregate-transformation-in-mapping-data-flow"></a>Veri akışı eşleme toplama dönüşümü 

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Burada, veri akışlarını toplamalar sütunlar tanımlarsınız. toplama dönüşümdür. İfade Oluşturucusu kullanarak toplamalar Topla, MIN, Maks ve mevcut veya hesaplanan sütunlara göre gruplanmış sayısı gibi farklı türlerde tanımlayabilirsiniz.

## <a name="group-by"></a>Gruplandırma ölçütü
Mevcut bir sütun seçin veya bir grup olarak için bir toplama yan tümcesi ile kullanmak için yeni bir hesaplanmış sütun oluşturun. Varolan bir sütunla kullanmak için açılan listeden istediğiniz sütunu seçin. Yeni bir hesaplanmış sütun oluşturmak için over yan tümcesinde gelin ve 'Hesaplanan sütunu' tıklayın. Bu açılır [veri akışı ifade oluşturucu](concepts-data-flow-expression-builder.md). Hesaplanmış bir sütun oluşturduktan sonra 'Adıyla' alanı altında çıkış sütun adı girin. Yan tümcesi ile başka bir grup eklemek istiyorsanız, var olan bir yan tümcesi içinde gelin ve '+'.

![Toplama dönüşümü gruplandırma ayarları](media/data-flow/agg.png "toplama dönüşümü ayarlarına göre grupla")

> [!NOTE]
> Group by tümcesi bir toplama dönüşümü isteğe bağlıdır.

## <a name="aggregate-column"></a>Toplama sütunu 
Toplama ifadeleri oluşturmak için 'Toplamalar' sekmesini seçin. Var olan bir sütun seçin değerin üzerine toplama veya yeni bir adla yeni bir alan oluşturabilirsiniz. Sütun adı seçiciyi yanındaki sağ taraftaki kutuya Toplama ifadesi girilir. İfade düzenlemek için ifade oluşturucuyu açmak için metin kutusuna tıklayın. Yeni bir toplama sütun oluşturmak için ek bir toplama, var olan bir ifade üzerine geldiğinizde ekleme tıklatıp '+' veya [sütun deseni](concepts-data-flow-column-pattern.md).

![Toplama dönüşümü toplama ayarları](media/data-flow/agg2.png "toplama dönüşümü toplama ayarları")

> [!NOTE]
> Her bir toplama ifadesi en az bir toplama işlevi içermelidir.

> [!NOTE]
> Hata ayıklama modunda ifade Oluşturucu ile toplama işlevleri veri önizlemelerinin üretemiyor. Veri önizlemelerini toplama dönüşümlere görüntülemek için ifade oluşturucu kapatın ve 'Veri Önizleme' sekmesi aracılığıyla verileri görüntüleyin.

## <a name="next-steps"></a>Sonraki adımlar

-Pencere tabanlı toplama şunu kullanarak tanımla [penceresi dönüştürme](data-flow-window.md)