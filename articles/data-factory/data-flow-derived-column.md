---
title: Azure veri fabrikası veri akışı eşleme türetilmiş sütun dönüştürme
description: Azure veri fabrikası eşleme veri akışı türetilmiş sütun dönüştürme ile uygun ölçekte verileri dönüştürme hakkında
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/08/2018
ms.openlocfilehash: f53e122eb1b2a5b6dabb9a44aef42394d0c7edb6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60478740"
---
# <a name="mapping-data-flow-derived-column-transformation"></a>Veri akışı eşleme türetilmiş sütun dönüştürme

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Türetilmiş sütun dönüşümü, yeni sütunlar, veri akışı oluşturmak veya var olan alanları değiştirmek için kullanın.

![Sütun türetmek](media/data-flow/dc1.png "türetilmiş sütun")

Tek bir sütunu türetilmiş dönüşümünde birden çok türetilmiş sütun eylemler gerçekleştirebilirsiniz. Tek dönüşümünü adım 1'den fazla sütunda dönüştürmek için "Sütun Ekle"'a tıklayın.

Sütun alanındaki türetilmiş yeni bir değer ile üzerine yazmak için mevcut bir sütun seçin veya "Oluşturma yeni yeni türetilmiş değerine sahip yeni bir sütun oluşturmak için sütun"'a tıklayın.

İfade işlevleri kullanarak türetilmiş sütunlar ifadesi burada oluşturabileceğinizi ifade oluşturucu ifade metin kutusu açılır.

## <a name="column-patterns"></a>Sütun desenleri

Kaynaklarınızı değişkeninden, sütun adları varsa, türetilmiş sütun içinde dönüşümler oluşturmak sütunları adlı kullanmak yerine sütun desenleri kullanarak isteyebilir. Bkz: [şema değişikliklerini](concepts-data-flow-schema-drift.md) makale daha fazla ayrıntı için.

![sütun düzeni](media/data-flow/columnpattern.png "sütun desenleri")

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [Data Factory ifade dili dönüşümlere](http://aka.ms/dataflowexpressions) ve [ifade oluşturucusu](concepts-data-flow-expression-builder.md)
