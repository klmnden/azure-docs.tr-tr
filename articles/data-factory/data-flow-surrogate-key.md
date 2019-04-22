---
title: Azure veri fabrikası veri akışı vekil anahtar dönüştürme eşlemesi
description: Sıralı anahtar değerleri oluşturmak için Azure Data Factory'nin eşleme veri akışı vekil anahtar dönüştürme kullanma
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: eaa1c577f7e208400d3430222b006e0dbbd7956a
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59698444"
---
# <a name="mapping-data-flow-surrogate-key-transformation"></a>Veri akışı vekil anahtar dönüştürme eşlemesi

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Yedek anahtar dönüşümü, veri akışı satır kümesi için artan iş kolu rastgele anahtar değeri eklemek için kullanın. Bu, boyut tabloları, boyut tabloları içindeki her üyenin Kimball DW metodolojiyi iş kolu anahtar, parçası olan benzersiz bir anahtar olması gereken yere bir yıldız şeması analitik veri modelinde tasarlarken kullanışlıdır.

![Yedek anahtar dönüştürme](media/data-flow/surrogate.png "vekil anahtar dönüştürme")

"Anahtar sütun", yeni vekil anahtar sütunu sunacak addır.

"Başlangıç değeri" başlangıç noktasını artımlı değeri olur.

## <a name="increment-keys-from-existing-sources"></a>Mevcut kaynaklarına artış anahtarları

Dizinizin bir kaynakta var olan bir değer başlamak istiyorsanız, bir sütunu türetilmiş dönüşümü vekil anahtar dönüşümünüzü takip kullanın ve iki değer birlikte ekleyin:

![SK eklemek en fazla](media/data-flow/sk006.png "vekil anahtar dönüştürme Max Ekle")

Önceki en yüksek anahtar değeriyle sağlamak için kullanabileceğiniz iki teknik vardır:

### <a name="database-sources"></a>Veritabanı kaynakları

Kaynak koordinat dönüştürmesini kullanma kaynağınızdan MAX() seçmek için "Sorgu" seçeneğini kullanın:

![Yedek anahtar sorgu](media/data-flow/sk002.png "vekil anahtar dönüşüm sorgusu")

### <a name="file-sources"></a>Dosya kaynakları

Bir dosyada, önceki en yüksek değer ise kaynak dönüşümünüzü bir toplama dönüşümü ile birlikte kullanın ve önceki en yüksek değeri elde etmek MAX() ifade işlevini kullanın:

![Yedek anahtar dosyası](media/data-flow/sk008.png "vekil anahtar dosyası")

Her iki durumda da, önceki en büyük değeri içeren kaynak ile birlikte gelen yeni verilerinizi eklemeniz gerekir:

![Yedek anahtar birleştirme](media/data-flow/sk004.png "vekil anahtar birleştirme")

## <a name="next-steps"></a>Sonraki adımlar

Bu örneklerde [katılın](data-flow-join.md) ve [türetilmiş sütun](data-flow-derived-column.md) dönüşümler.
