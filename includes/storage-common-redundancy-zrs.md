---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/04/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 037996385f34c5037e0386686e3bdf8dc1b7a37a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60731410"
---
Bölgesel olarak yedekli depolama (ZRS), verilerinizi tek bir bölgede üç depolama kümeleri arasında eşzamanlı olarak çoğaltır. Her Depolama kümesi diğerlerinden fiziksel olarak ayrılır ve kendi kullanılabilirlik bölgesinde (AZ) bulunur. Her kullanılabilirlik alanı&mdash;ve ZRS küme içindeki&mdash;otonom ve ayrı bir yardımcı programları ve ağ özellikleri içerir.

ZRS çoğaltmalı kullanarak bir depolama hesabında verilerinizin depoladığınızda, erişmek ve bir kullanılabilirlik alanı kullanılamaz duruma gelirse, verilerinizi yönetmek devam edebilirsiniz. ZRS, üstün performans ve düşük gecikme sağlar. ZRS sunar aynı [ölçeklenebilirlik hedefleri](../articles/storage/common/storage-scalability-targets.md) olarak [yerel olarak yedekli depolama (LRS)](../articles/storage/common/storage-redundancy-lrs.md).

ZRS, tutarlılık, dayanıklılık ve yüksek kullanılabilirlik gerektiren senaryolar için göz önünde bulundurun. Bir kesinti veya doğal afetler bir kullanılabilirlik bölgesi kullanılamıyor işler olsa bile, ZRS depolama nesnelerin en az % 99,9999999999 dayanıklılık sunar (12 9) belirli bir yıl boyunca.

Kullanılabilirlik alanları hakkında daha fazla bilgi için bkz. [kullanılabilirlik alanlarına genel bakış](https://docs.microsoft.com/azure/availability-zones/az-overview).