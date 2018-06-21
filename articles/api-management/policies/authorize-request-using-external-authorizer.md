---
title: Azure API management İlkesi örnek - dış Yetkilendiricisi kullanarak isteği yetkilendirmek | Microsoft Docs
description: Azure API management ilke örnek - gösteren bir özel veya eski kimlik doğrulama/yetkilendirme mantığı Kapsüllenen dış Yetkilendiricisi kullanan isteklerinin nasıl yetkilendirmek.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2018
ms.author: apimpm
ms.openlocfilehash: 7d172a40f2aad65b595026fc656634060a1f7193
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36284881"
---
# <a name="authorize-requests-using-external-authorizer"></a>Dış Yetkilendiricisi kullanarak isteklerini yetkilendirmek

Bu makalede, özel kimlik doğrulama/yetkilendirme mantığı içeren bir dış Yetkilendiricisi kullanarak API erişimini güvenli hale getirmek nasıl oluşturulduğunu gösteren bir Azure API management ilke örnek gösterilmektedir. Ayarlamak veya ilke kodu düzenlemek için açıklanan adımları izleyin [ayarlama veya düzenleme bir ilke](../set-edit-policies.md). Diğer örnekleri görmek için bkz: [ilkesi örnekleri](../policy-samples.md).

## <a name="policy"></a>İlke

Koda Yapıştır **gelen** bloğu.

[!code-xml[Main](../../../api-management-policy-samples/examples/Authorize requests using external authorizer.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APIM ilkeleri hakkında daha fazla bilgi edinin:

+ [Erişim kısıtlamaları ilkeleri](../api-management-access-restriction-policies.md)
+ [İlke örnekleri](../policy-samples.md)