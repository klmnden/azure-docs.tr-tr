---
title: Azure API management ilkesi örneği - erişim JWT taleplere göre yetkilendirme | Microsoft Docs
description: Azure API management ilke örneği - belirli bir HTTP yöntemleri JWT talepleri temel alan bir API üzerinde erişim yetkisi vermek nasıl gösterir.
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
ms.openlocfilehash: 60b36ceeac1cd4578ca81ac908c1a8a03c9d0180
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52869135"
---
# <a name="authorize-access-based-on-jwt-claims"></a>JWT talepleri temel alan erişim yetkisi verme

Bu makalede belirli HTTP yöntemlerini JWT talepleri temel alan bir API üzerinde erişim yetkisi vermek nasıl oluşturulduğunu gösteren bir Azure API management ilke örnek gösterilmektedir. Ayarlama veya ilke kodu düzenleme için açıklanan adımları izleyin [ayarlama veya düzenleme ilke](../set-edit-policies.md). Diğer örnekler için bkz [ilkesi örnekleri](../policy-samples.md).

## <a name="policy"></a>İlke

Kodun içine yapıştırın **gelen** blok.

[!code-xml[Main](../../../api-management-policy-samples/examples/Pre-authorize requests based on HTTP method with validate-jwt.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APIM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

