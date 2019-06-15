---
title: Azure API management ilke örneği - istek gövdesinde boyutuna göre yönlendirme | Microsoft Docs
description: Azure API management ilke örneği - kendi gövdeleri boyutuna göre istekleri yönlendirmeyi nasıl gösterir.
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
ms.openlocfilehash: 7656401115cc370d6eee60fb9bddb9bcd92e4201
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60860953"
---
# <a name="route-the-request-based-on-the-size-of-its-body"></a>İstek gövdesinde boyutuna göre yönlendirme

Bu makalede, kendi gövdeleri boyutuna göre istekleri yönlendirmek nasıl oluşturulduğunu gösteren bir Azure API management ilke örnek gösterilmektedir. Ayarlama veya ilke kodu düzenleme için açıklanan adımları izleyin [ayarlama veya düzenleme ilke](../set-edit-policies.md). Diğer örnekler için bkz [ilkesi örnekleri](../policy-samples.md).

## <a name="policy"></a>İlke

Kodun içine yapıştırın **gelen** blok.

[!code-xml[Main](../../../api-management-policy-samples/examples/Route requests based on size.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APIM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

