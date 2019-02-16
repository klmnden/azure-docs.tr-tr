---
title: Azure veri fabrikası veri akışı eşleme dönüştürme var
description: Azure veri fabrikası veri akışı eşleme dönüştürme var
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 9d21b304f55ec746da4b7b42194fe0d168261b53
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56272178"
---
# <a name="azure-data-factory-mapping-data-flow-exists-transformation"></a>Azure veri fabrikası veri akışı eşleme dönüştürme var

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

EXISTS dönüşümü durdurur veya veri akışına satır sağlayan dönüştürme filtreleme bir satırdır. Mevcut dönüştürme benzer ```SQL WHERE EXISTS``` ve ```SQL WHERE NOT EXISTS```. Bir filtre dönüştürme işleminin ardından ortaya çıkan satırlar, veri akışından ya da sütun değerleri kaynağından 1, 2 kaynağında bulunduğu tüm satırları içerecek veya 2 kaynağında yok.

![Ayarları var](media/data-flow/exsits.png "1 var.")

Veri akışı Stream 2 karşı Stream 1 değerlerinden karşılaştırabilmek için mevcut ikinci kaynağı seçin.

Kaynak 1'den ve kaynak değerleri EXISTS veya yok denetlemek istediğiniz 2 sütun seçin.
