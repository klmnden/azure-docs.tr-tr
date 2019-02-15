---
title: Azure veri fabrikası veri akışı vekil anahtar dönüştürme eşlemesi
description: Azure veri fabrikası veri akışı vekil anahtar dönüştürme eşlemesi
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 62e9879f6bc75f042669ecb931ca7459d1317cc7
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56272172"
---
# <a name="azure-data-factory-mapping-data-flow-surrogate-key-transformation"></a>Azure veri fabrikası veri akışı vekil anahtar dönüştürme eşlemesi

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Yedek anahtar dönüşümü, veri akışı satır kümesi için artan iş kolu rastgele anahtar değeri eklemek için kullanın. Bu, boyut tabloları, boyut tabloları içindeki her üyenin Kimball DW metodolojiyi iş kolu anahtar, parçası olan benzersiz bir anahtar olması gereken yere bir yıldız şeması analitik veri modelinde tasarlarken kullanışlıdır.

![Yedek anahtar dönüştürme](media/data-flow/surrogate.png "vekil anahtar dönüştürme")

"Anahtar sütun", yeni vekil anahtar sütunu sunacak addır.

"Başlangıç değeri" başlangıç noktasını artımlı değeri olur.
