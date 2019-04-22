---
title: Azure veri fabrikası veri akışı eşleme dönüştürme var
description: Azure veri fabrikası veri akışı eşleme dönüştürme var
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 6ce27ba699ae766ed4d2428f67d91379464bb9f1
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59783471"
---
# <a name="azure-data-factory-mapping-data-flow-exists-transformation"></a>Azure veri fabrikası veri akışı eşleme dönüştürme var

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

EXISTS dönüşümü durdurur veya veri akışına satır sağlayan dönüştürme filtreleme bir satırdır. Mevcut dönüştürme benzer ```SQL WHERE EXISTS``` ve ```SQL WHERE NOT EXISTS```. Bir filtre dönüştürme işleminin ardından ortaya çıkan satırlar, veri akışından ya da sütun değerleri kaynağından 1, 2 kaynağında bulunduğu tüm satırları içerecek veya 2 kaynağında yok.

![Ayarları var](media/data-flow/exsits.png "1 var.")

Veri akışı Stream 2 karşı Stream 1 değerlerinden karşılaştırabilmek için mevcut ikinci kaynağı seçin.

Kaynak 1'den ve kaynak değerleri EXISTS veya yok denetlemek istediğiniz 2 sütun seçin.

## <a name="multiple-exists-conditions"></a>Birden çok koşul var.

Sütun koşullarınız Exsits için her satırının yanındaki bulursunuz bir + oturum kullanılabilir üzerine geldiğinizde satır ulaşın. Bu, mevcut koşulları için birden çok satır eklemek olanak tanır.

## <a name="next-steps"></a>Sonraki adımlar

