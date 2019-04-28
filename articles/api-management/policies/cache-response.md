---
title: Azure API management ilkesi örneği - bir arka uç hizmetine yetenekleri ekleme | Microsoft Docs
description: Azure API management ilke örneği - özellikleri bir arka uç hizmetine ekleme işlemini gösterir. Örneğin, hava durumu tahmini API'sinde enlem ve boylam yerine bir yer adını kabul edin.
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
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: 7c9edbf4b2d231453cd336521a04ba6b7714b696
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61062216"
---
# <a name="add-capabilities-to-a-backend-service"></a>Bir arka uç hizmetine özellikleri ekleyin

Bu makalede, bir arka uç hizmetine özellikleri eklemek nasıl oluşturulduğunu gösteren bir Azure API management ilke örnek gösterilmektedir. Örneğin, hava durumu tahmini API'sinde enlem ve boylam yerine bir yer adını kabul edin. Ayarlama veya ilke kodu düzenleme için açıklanan adımları izleyin [ayarlama veya düzenleme ilke](../set-edit-policies.md). Diğer örnekler için bkz [ilkesi örnekleri](../policy-samples.md).

## <a name="policy"></a>İlke

Kodun içine yapıştırın **gelen** blok.

[!code-xml[Main](../../../api-management-policy-samples/examples/Call out to an HTTP endpoint and cache the response.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APIM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

