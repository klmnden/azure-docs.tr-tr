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
ms.openlocfilehash: e5cd7fcd0c853f03dbafb4d95b8459dcc83aecdf
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="azure-search-service-rest-api-version-2016-09-01-preview"></a>Azure Search Hizmeti REST API'si: Sürüm 2016-09-01-Önizleme
Bu makalede Önizleme özellikleri listelenmektedir `api-version=2016-09-01-Preview`. Bu önizleme önceki genel olarak kullanılabilir sürüm genişletir [API sürümü 2016-09-01 =](https://docs.microsoft.com/rest/api/searchservice), aşağıdaki Deneysel özellikleri sağlayarak:

* [`moreLikeThis` sorgu parametresi](search-more-like-this.md) belirli bir belge için uygun olan belgeleri bulmak için. Bu özellik, önceki önizlemelerde olmuştur. Bu API önceki bir API sürümü ile arıyoruz varsa, bu sürümü kullanmaya devam edebilirsiniz.


## <a name="how-to-call-a-preview-api"></a>Önizleme API çağrısı yapma

Aşağıdaki örnek gösterilmektedir nasıl Önizleme API sürümü, daha fazla bilgi-benzer-bu bir sorgu yaparken belirtilir.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?moreLikeThis=a1&api-version=2016-09-01-Preview

> [!NOTE]
> Önizleme özellikleri, test ve deneme geri bildirimi toplama amacı ile kullanılabilir ve değiştirilebilir. **Önizleme API'leri üretim uygulamalarında kullanılmamasını öneriyoruz.**

Azure Search hizmeti birden çok sürümlerinde kullanılabilir. Lütfen [arama hizmet sürümü oluşturma](https://docs.microsoft.com/azure/search/search-api-versions) Ayrıntılar için.
