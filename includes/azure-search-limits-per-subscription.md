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
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38756230"
---
Yalnızca her katmanında izin verilen hizmetler sayısı tarafından sınırlı, belirli bir katman, sağlanan her bir aboneliği içinde birden çok hizmeti oluşturabilirsiniz. Örneğin, en fazla 12 temel katmanında ve başka bir 12 Hizmetleri aynı abonelik içindeki S1 katmanında oluşturabilirsiniz. Katmanları hakkında daha fazla bilgi için bkz. [Azure Search için SKU veya katman seçin](../articles/search/search-sku-tier.md).

En yüksek hizmet sınırları, istek üzerine oluşturulabilir. Diğer hizmetler aynı abonelik içindeki ihtiyacınız olursa Azure Destek'e başvurun.

| Kaynak            | Ücretsiz&nbsp;<sup>1</sup> | Temel | S1  | S2 | S3 | S3&nbsp;HD |
| ------------------- | ---- | ----- | --- | -- | -- | ----- |
| En fazla Hizmetleri    |1     | 12    | 12  | 6  | 6  | 6     |
| Maksimum ölçek SU&nbsp;<sup>2</sup> |Yok |3 SU |36 SU |36 SU |36 SU |36 SU |

<sup>1</sup> ücretsiz, paylaşılan ve ayrılmış, kaynaklarda dayanır. Ölçek büyütme paylaşılan kaynaklar üzerinde desteklenmiyor.

<sup>2</sup> arama birimleri (SU) olarak ayrılmış birim, faturalama bir *çoğaltma* veya *bölüm*. Her iki kaynakları, depolama, dizinleme ve sorgu işlemleri için gerekir. SU hesaplamalar hakkında daha fazla bilgi için bkz: [sorgu ve dizin iş yükleri için kaynak düzeylerini ölçeklendirme](../articles/search/search-capacity-planning.md). 