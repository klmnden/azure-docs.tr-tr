---
title: Azure Search Hizmeti REST API sürümü 2017-11-11-Önizleme | Microsoft Docs
description: Azure Search Hizmeti REST API sürümü 2017-11-11-Önizleme eş anlamlıları ve moreLikeThis aramaları gibi Deneysel özellikler içerir.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 05/01/2018
ms.author: HeidiSteen
ms.openlocfilehash: 3a9ff6697357dec443691b261ae6fc0477603a9c
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="azure-search-service-rest-api-version-2017-11-11-preview"></a>Azure Search Hizmeti REST API'si: Sürüm 2017-11-11-Önizleme
Bu makalede `api-version=2017-11-11-Preview` Azure Search hizmeti aşağıdaki Deneysel özellikleri sağlayan REST API sürümü:

+ [Bilişsel arama](cognitive-search-concept-intro.md), metin olmayan kaynakları ve Azure Search'te bir tam metin aranabilir içeriğe dönüştürme protokole metni görünmeyen bilgi bulduğu Azure Search dizin içinde yeni bir iyileştirmesini özelliği.

API Önizleme, aşağıdaki iki işlemleri hariç olmak üzere genel olarak kullanılabilir API eşdeğerdir:

+ [Skillset oluşturun (API sürümü 2017-11-11-Önizleme =)](ref-create-skillset.md)
+ [Dizin Oluşturucu yapın (API sürümü 2017-11-11-Önizleme =)](ref-create-indexer.md)

Önizleme API sürümü hedef özen `api-version=2017-11-11-Preview` yayın öncesi özelliklere değerlendirirken. Aşağıdaki örnek sözdizimi Önizleme API sürümü için bir çağrı gösterilmektedir.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2017-11-11-Preview

> [!NOTE]
> Önizleme özellikleri, test ve deneme geri bildirimi toplama amacı ile kullanılabilir ve değiştirilebilir. **Önizleme API'leri üretim uygulamalarında kullanılmamasını öneriyoruz.**

Azure Search hizmeti birden çok sürümlerinde kullanılabilir. Lütfen [API sürümleri](search-api-versions.md) Ayrıntılar için.
