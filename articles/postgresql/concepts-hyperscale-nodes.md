---
title: PostgreSQL – hiper ölçekli (Citus) (Önizleme) için Azure veritabanı'nda düğümleri
description: Bir sunucu grubundaki düğümleri iki tür.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: c6b948ed63f43f1597103d123be5ed39f42bd276
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65077283"
---
# <a name="nodes-in-azure-database-for-postgresql--hyperscale-citus-preview"></a>PostgreSQL – hiper ölçekli (Citus) (Önizleme) için Azure veritabanı'nda düğümleri

Barındırma türü hiper ölçekli (Citus) (Önizleme), "hiçbir şey paylaşılmayan" bir mimaride birbiriyle koordine etmek için (düğümler adı verilir) PostgreSQL sunucuları için Azure veritabanı sağlar. Sunucu grubundaki düğümleri topluca daha fazla veri tutun ve tek bir sunucu üzerinde sağlayabileceğinizden çok daha fazla CPU çekirdeği kullanın. Mimari Ayrıca sunucu grubuna daha fazla düğüm ekleyerek ölçeğini veritabanı sağlar.

## <a name="coordinator-and-workers"></a>Düzenleyici ve çalışanları

Her sunucu grubu bir düzenleyici düğüm ve birden fazla çalışana sahip. Uygulamaları, sonuçları toplar ve ilgili çalışanlarına geçişleri Düzenleyici düğüm sorguları gönderin. Uygulamaları doğrudan çalışanlarına bağlanmanız mümkün değildir.

Her bir sorgu için düzenleyici için tek çalışan düğüme yönlendiren ya da gerekli verileri tek bir düğüm veya birden çok olup yaşadığı bağlı olarak birkaç arasında parallelizes. Düzenleyici meta veri tablolarını consulting tarafından yapmanız gerekenler karar verir. Bu tablolar düğümlere DNS adları ve çalışan düğümlerinin durumunu ve veri dağıtımını izleyin.

## <a name="next-steps"></a>Sonraki adımlar
- Düğümler nasıl depolamak öğrenin [dağıtılmış veriler](concepts-hyperscale-distributed-data.md)
