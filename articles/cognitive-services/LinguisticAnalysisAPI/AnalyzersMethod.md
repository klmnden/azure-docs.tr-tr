---
title: Lingistic analiz API çözümleyiciler yönteminde | Microsoft Docs
description: REST API çözümleyiciler Microsoft Bilişsel hizmetler hizmeti tarafından şu anda desteklenen çözümleyiciler listesini sağlar.
services: cognitive-services
author: RichardSunMS
manager: wkwok
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: article
ms.date: 06/30/2016
ms.author: lesun
ms.openlocfilehash: 3fc243a0da77c5bae9009929f2b82e1353347752
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351461"
---
# <a name="analyzers-method"></a>Çözümleyiciler yöntemi

**Çözümleyiciler** REST API hizmeti tarafından şu anda desteklenen çözümleyiciler listesini sağlar.
Yanıtı içeren kendi [adları](Analyzer-Names.md) ve her (örneğin, İngilizce için "en") tarafından desteklenen diller.

## <a name="request-parameters"></a>İstek parametreleri
None

<br>

## <a name="response-parameters"></a>Yanıt parametreleri
Ad | Tür | Açıklama
-----|------|--------------
Diller | dize listesi | Bu çözümleyici kullanılabilir iki harf ISO dil kodlarının listesi.
id   | dize | Bu çözümleyici benzersiz kimliği
türü | dize | Burada Çözümleyicisi geniş türü
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
