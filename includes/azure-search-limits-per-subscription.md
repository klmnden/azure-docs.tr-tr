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
ms.openlocfilehash: a466ea29fa31c4c628724f3d5138a1612ef7a0f4
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553911"
---
Bir abonelik içindeki birden çok hizmeti oluşturabilirsiniz. Her biri belirli bir katman üzerinden sağlanabilir. Yalnızca her katmanında izin verilen hizmetler sayısına göre sınırlama getirilir. Örneğin, en fazla 12 temel katmanında ve başka bir 12 Hizmetleri aynı abonelik içindeki S1 katmanında oluşturabilirsiniz. Katmanları hakkında daha fazla bilgi için bkz. [bir SKU veya katmanı için Azure Search seçin](../articles/search/search-sku-tier.md).

En yüksek hizmet sınırları, istek üzerine oluşturulabilir. Diğer hizmetler aynı abonelik içindeki gerekiyorsa, Azure desteğine başvurun.

| Kaynak            | Ücretsiz<sup>1</sup> | Temel | S1  | S2 | S3 | S3&nbsp;HD |
| ------------------- | ---- | ----- | --- | -- | -- | ----- |
| En fazla Hizmetleri    |1     | 12    | 12  | 6  | 6  | 6     |
| Maksimum ölçek, arama birimi (SU)<sup>2</sup> |Yok |3 SU |36 SU |36 SU |36 SU |36 SU |

<sup>1</sup>ücretsiz, paylaşılan ve ayrılmış, kaynaklarda dayanır. Ölçek büyütme paylaşılan kaynaklar üzerinde desteklenmiyor.

<sup>2</sup>arama birimleri olarak ayrılan birimleri, faturalama bir *çoğaltma* veya *bölüm*. Her iki kaynakları, depolama, dizinleme ve sorgu işlemleri için gerekir. SU hesaplamalar hakkında daha fazla bilgi için bkz: [sorgu ve dizin iş yükleri için kaynak düzeylerini ölçeklendirme](../articles/search/search-capacity-planning.md). 