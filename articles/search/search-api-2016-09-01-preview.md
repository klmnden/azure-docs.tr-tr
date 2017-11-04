---
title: "Azure Search Hizmeti REST API sürümü 2016-09-01-Önizleme | Microsoft Docs"
description: "Azure Search Hizmeti REST API sürümü 2016-09-01-Önizleme eş anlamlıları ve moreLikeThis aramaları gibi Deneysel özellikler içerir."
services: search
documentationcenter: na
author: mhko
manager: jlembicz
editor: 
ms.assetid: 3dba3bf8-9c83-42f6-82bc-04727bd11037
ms.service: search
ms.devlang: rest-api
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 10/25/2017
ms.author: nateko
ms.openlocfilehash: 082c207f892fcc277d30d66c6165dd9920d3ab27
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="azure-search-service-rest-api-version-2016-09-01-preview"></a>Azure Search Hizmeti REST API'si: Sürüm 2016-09-01-Önizleme
Bu makale için başvuru belgesidir `api-version=2016-09-01-Preview`. Bu önizleme geçerli genel olarak kullanılabilir sürüm genişletir [API sürümü 2016-09-01 =](https://msdn.microsoft.com/library/dn798935.aspx), aşağıdaki Deneysel özellikleri sağlayarak:

* [Eş anlamlıları API](search-synonyms.md) eş eşlemelerini karşıya yükleyin ve arama genişletin.
* [`moreLikeThis`sorgu parametresi](search-more-like-this.md) belirli bir belge için uygun olan belgeleri bulmak için.

Lütfen Önizleme API sürümü hedeflemek için olun `api-version=2016-09-01-Preview` bu Deneysel özellikleri denemek için. Aşağıdaki örnek gösterilmektedir nasıl Önizleme API sürümü, daha fazla bilgi-benzer-bu bir sorgu yaparken belirtilir.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?moreLikeThis=a1&api-version=2016-09-01-Preview

> [!NOTE]
> Önizleme özellikleri, test ve deneme geri bildirimi toplama amacı ile kullanılabilir ve değiştirilebilir. **Önizleme API'leri üretim uygulamalarında kullanılmamasını öneriyoruz.**

Azure Search hizmeti birden çok sürümlerinde kullanılabilir. Lütfen [arama hizmet sürümü oluşturma](http://msdn.microsoft.com/library/azure/dn864560.aspx) Ayrıntılar için.
