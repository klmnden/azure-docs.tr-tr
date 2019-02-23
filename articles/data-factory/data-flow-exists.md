---
title: Azure veri fabrikası veri akışı eşleme dönüştürme var
description: Azure veri fabrikası veri akışı eşleme dönüştürme var
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 18d8a0e231e8b4dbe33911dd6267966674366904
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2019
ms.locfileid: "56734498"
---
# <a name="azure-data-factory-mapping-data-flow-exists-transformation"></a>Azure veri fabrikası veri akışı eşleme dönüştürme var

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

EXISTS dönüşümü durdurur veya veri akışına satır sağlayan dönüştürme filtreleme bir satırdır. Mevcut dönüştürme benzer ```SQL WHERE EXISTS``` ve ```SQL WHERE NOT EXISTS```. Bir filtre dönüştürme işleminin ardından ortaya çıkan satırlar, veri akışından ya da sütun değerleri kaynağından 1, 2 kaynağında bulunduğu tüm satırları içerecek veya 2 kaynağında yok.

![Ayarları var](media/data-flow/exsits.png "1 var.")

Veri akışı Stream 2 karşı Stream 1 değerlerinden karşılaştırabilmek için mevcut ikinci kaynağı seçin.

Kaynak 1'den ve kaynak değerleri EXISTS veya yok denetlemek istediğiniz 2 sütun seçin.
