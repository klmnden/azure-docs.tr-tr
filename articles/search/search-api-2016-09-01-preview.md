---
title: Azure Search Hizmeti REST API sürümü 2016-09-01-Önizleme | Microsoft Docs
description: Azure Search Hizmeti REST API sürümü 2016-09-01-Önizleme moreLikeThis aramaları gibi Deneysel özellikler içerir.
author: mhko
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: reference
ms.date: 04/18/2018
ms.author: nateko
ms.openlocfilehash: 8eae54c912711a11c015737903b6898b98fd5159
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="azure-search-service-rest-api-version-2016-09-01-preview"></a>Azure Search Hizmeti REST API'si: Sürüm 2016-09-01-Önizleme
Bu makale için başvuru belgesidir `api-version=2016-09-01-Preview`. Bu önizleme geçerli genel olarak kullanılabilir sürüm genişletir [API sürümü 2016-09-01 =](https://docs.microsoft.com/rest/api/searchservice), aşağıdaki Deneysel özellikleri sağlayarak:

* [`moreLikeThis` sorgu parametresi](search-more-like-this.md) belirli bir belge için uygun olan belgeleri bulmak için.

Lütfen Önizleme API sürümü hedeflemek için olun `api-version=2016-09-01-Preview` bu Deneysel özellikleri denemek için. Aşağıdaki örnek gösterilmektedir nasıl Önizleme API sürümü, daha fazla bilgi-benzer-bu bir sorgu yaparken belirtilir.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?moreLikeThis=a1&api-version=2016-09-01-Preview

> [!NOTE]
> Önizleme özellikleri, test ve deneme geri bildirimi toplama amacı ile kullanılabilir ve değiştirilebilir. **Önizleme API'leri üretim uygulamalarında kullanılmamasını öneriyoruz.**

Azure Search hizmeti birden çok sürümlerinde kullanılabilir. Lütfen [arama hizmet sürümü oluşturma](https://docs.microsoft.com/azure/search/search-api-versions) Ayrıntılar için.
