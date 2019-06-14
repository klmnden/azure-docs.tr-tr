---
title: Analiz edici yöntemi - Lingistic analizi API'si
titlesuffix: Azure Cognitive Services
description: REST API Çözümleyicileri, dil analizi API'si tarafından şu anda desteklenen Çözümleyicileri listesini sağlar.
services: cognitive-services
author: RichardSunMS
manager: nitinme
ms.service: cognitive-services
ms.subservice: linguistic-analysis
ms.topic: conceptual
ms.date: 06/30/2016
ms.author: lesun
ROBOTS: NOINDEX
ms.openlocfilehash: 1b33b60f674eebb15fdc6112e1d630b93b98494b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60404734"
---
# <a name="analyzers-method"></a>Analiz edici yöntemi

> [!IMPORTANT]
> Dilbilimsel Analiz önizleme sürümü 9 Ağustos 2018 tarihinde kullanımdan kaldırılmıştır. Metin işleme ve analiz için [Azure Machine Learning metin analizi modüllerini](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/text-analytics) kullanmanızı öneririz.

**Çözümleyicileri** REST API, hizmet tarafından şu anda desteklenen Çözümleyicileri listesini sağlar.
Yanıtı içeren kendi [adları](Analyzer-Names.md) ve her (örneğin, İngilizce için "en") tarafından desteklenen diller.

## <a name="request-parameters"></a>İstek parametreleri
None

<br>

## <a name="response-parameters"></a>Yanıt parametreleri

Ad | Tür | Açıklama
-----|------|--------------
Diller | dize listesi | Bu çözümleyici kullanılabilecek iki harf ISO dil kodlarının listesi.
id   | string | Bu çözümleyici için benzersiz kimlik
tür | string | Burada Çözümleyicisi geniş türü
Belirtimi | string | Bu çözümleyici için kullanılan belirtimi adı
Uygulama | string | model ve/veya bu Çözümleyicisi arkasında algoritması açıklaması

<br>

## <a name="example"></a>Örnek
/Analyzers Al

Yanıt: JSON
```json
[
    {
        "id": "22A6B758-420F-4745-8A3C-46835A67C0D2",
        "languages": ["en"],
        "kind": "Constituency_Tree",  
        "specification": "PennTreebank3",
        "implementation": "SplitMerge"
    },
    {
        "id" : "4FA79AF1-F22C-408D-98BB-B7D7AEEF7F04",
        "languages": ["en"],
        "kind": "POS_Tags",
        "specification": "PennTreebank3",
        "implementation": "cmm"
    },
    {
        "id" : "08EA174B-BFDB-4E64-987E-602F85DA7F72",
        "languages": ["en"],
        "kind": "Tokens",
        "specification":"PennTreebank3",
        "implementation": "regexes"
    }
]
```
