---
title: Azure API management ilke örneği - uygulama X-CSRF düzeni | Microsoft Docs
description: Azure API management ilke örneği - çok sayıda API tarafından kullanılan X-CSRF düzeni nasıl uygulayacağınıza karar gösterir. Bu örnek SAP Ağ Geçidi'ne özgüdür.
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
ms.openlocfilehash: 2f4d26702443ef3113dad98cde1d13b292fe657a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60306708"
---
# <a name="implement-x-csrf-pattern"></a>Uygulama X-CSRF düzeni

Bu makalede, çok sayıda API tarafından kullanılan X-CSRF düzeni nasıl uygulayacağınıza karar gösteren bir Azure API management ilke örneği gösterilmektedir. Bu örnek SAP Ağ Geçidi'ne özgüdür. Ayarlama veya ilke kodu düzenleme için açıklanan adımları izleyin [ayarlama veya düzenleme ilke](../set-edit-policies.md). Diğer örnekler için bkz [ilkesi örnekleri](../policy-samples.md).

## <a name="policy"></a>İlke

Kodun içine yapıştırın **gelen** blok.

[!code-xml[Main](../../../api-management-policy-samples/examples/Get X-CSRF token from SAP gateway using send request.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APIM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

