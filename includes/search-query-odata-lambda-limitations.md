---
author: brjohnstmsft
ms.service: search
ms.topic: include
ms.date: 06/13/2018
ms.author: brjohnst
ms.openlocfilehash: b4d0bc73e652a1cd6ef59589318b92f63727c7f5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079645"
---
| Veri türü | Lambda ifadeleri ile izin özellikleri `any` | Lambda ifadeleri ile izin özellikleri `all` |
|---|---|---|
| `Collection(Edm.ComplexType)` | Dışında her şeyi `search.ismatch` ve `search.ismatchscoring` | Aynı |
| `Collection(Edm.String)` | Karşılaştırmalar `eq` veya `search.in` <br/><br/> Alt ifadeler ile birleştirme `or` | Karşılaştırmalar `ne` veya `not search.in()` <br/><br/> Alt ifadeler ile birleştirme `and` |
| `Collection(Edm.Boolean)` | Karşılaştırmalar `eq` veya `ne` | Aynı |
| `Collection(Edm.GeographyPoint)` | Kullanarak `geo.distance` ile `lt` veya `le` <br/><br/> `geo.intersects` <br/><br/> Alt ifadeler ile birleştirme `or` | Kullanarak `geo.distance` ile `gt` veya `ge` <br/><br/> `not geo.intersects(...)` <br/><br/> Alt ifadeler ile birleştirme `and` |
| `Collection(Edm.DateTimeOffset)`, `Collection(Edm.Double)`, `Collection(Edm.Int32)`, `Collection(Edm.Int64)` | Kullanarak karşılaştırmalar `eq`, `ne`, `lt`, `gt`, `le`, veya `ge` <br/><br/> Diğer alt ifadeler kullanan karşılaştırmalar birleştirme `or` <br/><br/> Dışındaki karşılaştırmaları birleştirme `ne` ile kullanarak diğer alt ifadeler `and` <br/><br/> Birleşimlerini kullanarak ifadeleri `and` ve `or` içinde [ayırt edici Normal Form (DNF)](https://en.wikipedia.org/wiki/Disjunctive_normal_form) | Kullanarak karşılaştırmalar `eq`, `ne`, `lt`, `gt`, `le`, veya `ge` <br/><br/> Diğer alt ifadeler kullanan karşılaştırmalar birleştirme `and` <br/><br/> Dışındaki karşılaştırmaları birleştirme `eq` ile kullanarak diğer alt ifadeler `or` <br/><br/> Birleşimlerini kullanarak ifadeleri `and` ve `or` içinde [Conjunctive Normal Form (CNF)](https://en.wikipedia.org/wiki/Conjunctive_normal_form) |
