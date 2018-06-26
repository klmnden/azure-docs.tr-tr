---
title: Azure API management ilke örnek - uygulama X-CSRF düzeni | Microsoft Docs
description: Azure API management ilke örnek - birçok API tarafından kullanılan X-CSRF desen uygulamak nasıl gösterir. Bu örnek SAP Ağ Geçidi'ne özgüdür.
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
ms.openlocfilehash: 3a2067836a1488d117dced96f3935f2d1f8b1b48
ms.sourcegitcommit: e34afd967d66aea62e34d912a040c4622a737acb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36946032"
---
# <a name="implement-x-csrf-pattern"></a>Uygulama X-CSRF düzeni

Bu makalede, birçok API tarafından kullanılan X-CSRF desen uygulamak nasıl oluşturulduğunu gösteren bir Azure API management ilke örnek gösterilmektedir. Bu örnek SAP Ağ Geçidi'ne özgüdür. Ayarlamak veya ilke kodu düzenlemek için açıklanan adımları izleyin [ayarlama veya düzenleme bir ilke](../set-edit-policies.md). Diğer örnekleri görmek için bkz: [ilkesi örnekleri](../policy-samples.md).

## <a name="policy"></a>İlke

Koda Yapıştır **gelen** bloğu.

[!code-xml[Main](../../../api-management-policy-samples/examples/Get X-CSRF token from SAP gateway using send request.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APIM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

