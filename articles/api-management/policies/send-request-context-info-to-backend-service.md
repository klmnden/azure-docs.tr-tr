---
title: Azure API management ilke örneği - arka uç hizmetine istek bağlamı bilgi gönder | Microsoft Docs
description: Azure API management ilke örneği - arka uç hizmetine istek bağlamı bilgi gönderme işlemini gösterir.
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
ms.openlocfilehash: 3369f3b3349e8daf9ff540b824c10bbd618a1147
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60859962"
---
# <a name="send-request-context-information-to-the-backend-service"></a>İstek bağlamını arka uç hizmetine gönderin

Bu makalede, istek bağlamını arka uç hizmetine göndermek nasıl oluşturulduğunu gösteren bir Azure API Yönetimi İlkesi örnek gösterilmektedir. Ayarlama veya ilke kodu düzenleme için açıklanan adımları izleyin [ayarlama veya düzenleme ilke](../set-edit-policies.md). Diğer örnekler için bkz [ilkesi örnekleri](../policy-samples.md).

## <a name="policy"></a>İlke

Kodun içine yapıştırın **gelen** blok.

[!code-xml[Main](../../../api-management-policy-samples/examples/Send request context information to the backend service.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APIM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

