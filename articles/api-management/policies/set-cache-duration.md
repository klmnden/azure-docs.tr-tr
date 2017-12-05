---
title: "Azure API management ilke örnek - yanıt önbellek süresini ayarlama | Microsoft Docs"
description: "Azure API management ilke örnek - yanıt önbelleğe alma süresi arka ucu tarafından gönderilen Cache-Control üstbilgisi maxAge değeri kullanılarak nasıl kurulur gösteren..."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: 8471b51999159c8f6233abbbce998b122163efd8
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="set-response-cache-duration"></a>Yanıt önbelleğe alma süresi ayarlayın

Bu makalede arka ucu tarafından gönderilen Cache-Control üstbilgisi maxAge değerini kullanarak yanıt önbelleğe alma süresi ayarlamak nasıl oluşturulduğunu gösteren bir Azure API management ilke örnek gösterilmektedir. Ayarlamak veya ilke kodu düzenlemek için açıklanan adımları izleyin [ayarlama veya düzenleme bir ilke](../set-edit-policies.md). Diğer örnekleri görmek için bkz: [ilkesi örnekleri](../policy-samples.md).

## <a name="policy"></a>İlke

Koda Yapıştır **gelen** bloğu.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Set cache duration using response cache control header.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APIM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

