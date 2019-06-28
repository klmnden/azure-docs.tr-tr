---
title: Eşleme veri akışı - Azure Data Factory sütun dönüşümünde türetilmiş | Microsoft Docs
description: Uygun ölçekte Azure Data factory'de veri akışını türetilmiş sütun eşleme dönüştürme ile verileri dönüştürme hakkında bilgi edinin.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/08/2018
ms.openlocfilehash: 941c629fd8359edc7fc1cf364a6735314044d95e
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67312177"
---
# <a name="derived-column-transformation-in-mapping-data-flow"></a>Türetilmiş sütun dönüşümünde eşleme veri akışı

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Türetilmiş sütun dönüşümü, yeni sütunlar, veri akışı oluşturmak veya var olan alanları değiştirmek için kullanın.

## <a name="derived-column-settings"></a>Türetilmiş sütun ayarları

Varolan bir sütunla geçersiz kılmak için sütun açılan seçin. Aksi takdirde, bir metin kutusu ve yeni sütunun adı türü olarak sütun seçimi alanını kullanın. Türetilmiş sütun ifade oluşturmak için açık için 'ifadesi girin' kutuyu tıklatın [veri akışı ifade oluşturucu](concepts-data-flow-expression-builder.md).

![Sütun ayarlarını türetilmiş](media/data-flow/dc1.png "türetilmiş sütun ayarları")

Mevcut bir üzerine gelindiğinde ek türetilmiş sütunlar eklemek için tıklayın ve sütunu türetilmiş '+'. Ardından 'Sütunu Ekle' veya 'Ekle Sütun Düzeni' seçin. Sütun desenleri, sütun adlarını kaynaklarınızı değişkeninden varsa yararlı olabilecek. Daha fazla bilgi için [sütun desenleri](concepts-data-flow-column-pattern.md).

![Yeni türetilmiş sütun seçimi](media/data-flow/columnpattern.png "yeni türetilmiş sütun seçimi")

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [eşleme veri akışı ifade dili](data-flow-expression-functions.md).
