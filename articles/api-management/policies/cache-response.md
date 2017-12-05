---
title: "Azure API management İlkesi örnek - bir arka uç hizmetine yetenekleri ekleme | Microsoft Docs"
description: "Azure API management ilke örnek - bir arka uç hizmetine yetenekleri ekleme gösterir. Örneğin, enlem ve boylam hava tahmini API içinde yerine yer adını kabul edin."
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
ms.openlocfilehash: d500df0f0e48134ac9c1397795d65706d2e56ff9
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="add-capabilities-to-a-backend-service"></a>Bir arka uç hizmetine yetenekleri ekleme

Bu makalede bir arka uç hizmetine özellikleri eklemek nasıl oluşturulduğunu gösteren bir Azure API management ilke örnek gösterilmektedir. Örneğin, enlem ve boylam hava tahmini API içinde yerine yer adını kabul edin. Ayarlamak veya ilke kodu düzenlemek için açıklanan adımları izleyin [ayarlama veya düzenleme bir ilke](../set-edit-policies.md). Diğer örnekleri görmek için bkz: [ilkesi örnekleri](../policy-samples.md).

## <a name="policy"></a>İlke

Koda Yapıştır **gelen** bloğu.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Call out to an HTTP endpoint and cache the response.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APIM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

