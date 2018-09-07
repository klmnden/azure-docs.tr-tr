---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/11/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 4a43966180850645584043690b1be9d6ae232f6e
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2018
ms.locfileid: "44027456"
---
Bölgesel olarak yedekli depolama (ZRS), verilerinizi tek bir bölgede üç depolama kümeleri arasında eşzamanlı olarak çoğaltır. Her Depolama kümesi diğerlerinden fiziksel olarak ayrılır ve kendi kullanılabilirlik bölgesinde (AZ) yer alıyor. Her kullanılabilirlik alanı ve ZRS küme içindeki ayrı yardımcı programları ve ağ özellikleri ile otonom.

Verilerinizi depolanması bir ZRS hesabına, mümkün erişimin ve bölge kullanılamaz hale gelmesi durumunda, verilerinizi yönetin sağlar. ZRS, üstün performans ve düşük gecikme sağlar. ZRS sunar aynı [ölçeklenebilirlik hedefleri](../articles/storage/common/storage-scalability-targets.md) olarak [yerel olarak yedekli depolama (LRS)](../articles/storage/common/storage-redundancy-lrs.md).

ZRS bir bölgesel veri merkezinde bir kesinti veya doğal afetler kullanılamaz işler olsa bile, güçlü tutarlılık, güçlü dayanıklılık ve yüksek kullanılabilirlik gerektiren senaryolar için göz önünde bulundurun. ZRS, en az % 99,9999999999 depolama nesneleri için dayanıklılık sunar (12 9) belirli bir yıl boyunca.

Kullanılabilirlik alanları hakkında daha fazla bilgi için bkz. [kullanılabilirlik alanlarına genel bakış](https://docs.microsoft.com/azure/availability-zones/az-overview).