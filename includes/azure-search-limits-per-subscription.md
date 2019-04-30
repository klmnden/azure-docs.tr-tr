---
title: include dosyası
description: include dosyası
services: search
author: HeidiSteen
ms.service: search
ms.topic: include
ms.date: 04/04/2018
ms.author: heidist
ms.custom: include file
ms.openlocfilehash: 0da7ad35f6efc031a52ef43caa514559c08c94fe
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61464916"
---
Bir abonelik içindeki birden çok hizmeti oluşturabilirsiniz. Her biri belirli bir katman üzerinden sağlanabilir. Yalnızca her katmanında izin verilen hizmetler sayısına göre sınırlama getirilir. Örneğin, en fazla 12 temel katmanında ve başka bir 12 Hizmetleri aynı abonelik içindeki S1 katmanında oluşturabilirsiniz. Katmanları hakkında daha fazla bilgi için bkz. [bir SKU veya katmanı için Azure Search seçin](../articles/search/search-sku-tier.md).

En yüksek hizmet sınırları, istek üzerine oluşturulabilir. Diğer hizmetler aynı abonelik içindeki gerekiyorsa, Azure desteğine başvurun.

| Kaynak            | Ücretsiz<sup>1</sup> | Temel | S1  | S2 | S3 | S3&nbsp;HD | L1 | L2 |
| ------------------- | ---- | ----- | --- | -- | -- | ----- | -- | -- |
| En fazla Hizmetleri    |1     | 12    | 12  | 6  | 6  | 6     | 6  | 6  |
| Maksimum ölçek, arama birimi (SU)<sup>2</sup> |Yok |3 SU |36 SU |36 SU |36 SU |36 SU |36 SU |36 SU |

<sup>1</sup> ücretsiz, paylaşılan ve ayrılmış, kaynaklarda dayanır. Ölçek büyütme paylaşılan kaynaklar üzerinde desteklenmiyor.

<sup>2</sup> arama birimleri olarak ayrılan birimleri, faturalama bir *çoğaltma* veya *bölüm*. Her iki kaynakları, depolama, dizinleme ve sorgu işlemleri için gerekir. SU hesaplamalar hakkında daha fazla bilgi için bkz: [sorgu ve dizin iş yükleri için kaynak düzeylerini ölçeklendirme](../articles/search/search-capacity-planning.md). 