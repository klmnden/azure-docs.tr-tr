---
title: include dosyası
description: include dosyası
services: search
author: HeidiSteen
ms.service: search
ms.topic: include
ms.date: 05/06/2019
ms.author: heidist
ms.custom: include file
ms.openlocfilehash: 1e147e8bd9260cd1ece60b70641968a229995ec1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66160499"
---
Bir abonelik içindeki birden çok hizmeti oluşturabilirsiniz. Her biri belirli bir katman üzerinden sağlanabilir. Yalnızca her katmanında izin verilen hizmetler sayısına göre sınırlama getirilir. Örneğin, en fazla 12 temel katmanında ve başka bir 12 Hizmetleri aynı abonelik içindeki S1 katmanında oluşturabilirsiniz. Katmanları hakkında daha fazla bilgi için bkz. [bir SKU veya katmanı için Azure Search seçin](../articles/search/search-sku-tier.md).

En yüksek hizmet sınırları, istek üzerine oluşturulabilir. Diğer hizmetler aynı abonelik içindeki gerekiyorsa, Azure desteğine başvurun.

| Resource            | Ücretsiz<sup>1</sup> | Temel | S1  | S2 | S3 | S3&nbsp;HD | L1 | L2 |
| ------------------- | ---- | ----- | --- | -- | -- | ----- | -- | -- |
| En fazla Hizmetleri    |1     | 16    | 16  | 8  | 6  | 6     | 6  | 6  |
| Maksimum ölçek, arama birimi (SU)<sup>2</sup> |Yok |3 SU |36 SU |36 SU |36 SU |36 SU |36 SU |36 SU |

<sup>1</sup> ücretsiz, paylaşılan ve ayrılmış, kaynaklarda dayanır. Ölçek büyütme paylaşılan kaynaklar üzerinde desteklenmiyor.

<sup>2</sup> arama birimleri olarak ayrılan birimleri, faturalama bir *çoğaltma* veya *bölüm*. Her iki kaynakları, depolama, dizinleme ve sorgu işlemleri için gerekir. SU hesaplamalar hakkında daha fazla bilgi için bkz: [sorgu ve dizin iş yükleri için kaynak düzeylerini ölçeklendirme](../articles/search/search-capacity-planning.md). 