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
ms.openlocfilehash: 8a4f794c8ef24a90498954629c131904621c5b43
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
ms.locfileid: "30844101"
---
Yalnızca her katmanını izin verilen hizmetler sayısı sınırlı belirli bir katman, sağlanan her biri bir abonelik içindeki birden fazla hizmet oluşturabilirsiniz. Örneğin, 12 temel katmana adresindeki ve başka bir 12 Hizmetleri aynı abonelik içindeki S1 katmanı adresindeki kadar oluşturabilirsiniz. Katmanları hakkında daha fazla bilgi için bkz: [bir SKU katmanı için Azure Search seçin veya](../articles/search/search-sku-tier.md).

Maksimum hizmet sınırları, istek üzerine yükseltilebilir. Daha fazla hizmet aynı abonelik içindeki ihtiyacınız varsa Azure desteğine başvurun.

| Kaynak            | Ücretsiz&nbsp;<sup>1</sup> | Temel | S1  | S2 | S3 | S3&nbsp;HD |
| ------------------- | ---- | ----- | --- | -- | -- | ----- |
| En fazla Hizmetleri    |1     | 12    | 12  | 6  | 6  | 6     |
| SU en fazla ölçek&nbsp;<sup>2</sup> |Yok |3 SU |36 SU |36 SU |36 SU |36 SU |

<sup>1</sup> serbest paylaşılan, ayrılmış değil, kaynakları dayanır. Büyütme paylaşılan kaynakları desteklenmez.

<sup>2</sup> arama birimi (SU) ya da ayrılan birimler, faturalama bir *çoğaltma* veya *bölüm*. Depolama, dizin oluşturma ve sorgu işlemleri için her iki kaynağın gerekir. SU hesaplamalar hakkında daha fazla bilgi için bkz: [ölçeklendirme sorgu ve dizin iş yükleri için kaynak düzeylerinin](../articles/search/search-capacity-planning.md). 