---
title: Analiz edici yöntemi - Lingistic analizi API'si
titlesuffix: Azure Cognitive Services
description: REST API Çözümleyicileri, dil analizi API'si tarafından şu anda desteklenen Çözümleyicileri listesini sağlar.
services: cognitive-services
author: RichardSunMS
manager: cgronlun
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: conceptual
ms.date: 06/30/2016
ms.author: lesun
ROBOTS: NOINDEX
ms.openlocfilehash: 762ebf50999a88251dcd05824f2ed450cec97f04
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48237408"
---
# <a name="analyzers-method"></a>Analiz edici yöntemi

> [!IMPORTANT]
> Dil analizi önizlemesi, 9 Ağustos 2018 tarihinde kullanımdan. Kullanmanızı öneririz [Azure Machine Learning metin analiz modüllerini](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/text-analytics) metin işleme ve analiz için.

**Çözümleyicileri** REST API, hizmet tarafından şu anda desteklenen Çözümleyicileri listesini sağlar.
Yanıtı içeren kendi [adları](Analyzer-Names.md) ve her (örneğin, İngilizce için "en") tarafından desteklenen diller.

## <a name="request-parameters"></a>İstek parametreleri
None

<br>

## <a name="response-parameters"></a>Yanıt parametreleri
Ad | Tür | Açıklama
-----|------|--------------
Diller | dize listesi | Bu çözümleyici kullanılabilecek iki harf ISO dil kodlarının listesi.
id   | dize | Bu çözümleyici için benzersiz kimlik
tür | dize | Burada Çözümleyicisi geniş türü
Belirtimi | dize | Bu çözümleyici için kullanılan belirtimi adı
Uygulama | dize | model ve/veya bu Çözümleyicisi arkasında algoritması açıklaması

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
