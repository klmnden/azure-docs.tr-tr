---
title: Azure API management ilke örnek - uygulama X-CSRF düzeni | Microsoft Docs
description: Azure API management ilke örnek - birçok API tarafından kullanılan X-CSRF desen uygulamak nasıl gösterir. Bu örnek, SAP ağ geçidine özeldir.
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
ms.openlocfilehash: 71e76b234b614f5935775d5c387c9897e1dcb968
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33937681"
---
# <a name="implement-x-csrf-pattern"></a>Uygulama X-CSRF düzeni

Bu makalede, birçok API tarafından kullanılan X-CSRF desen uygulamak nasıl oluşturulduğunu gösteren bir Azure API management ilke örnek gösterilmektedir. Bu örnek, SAP ağ geçidine özeldir. Ayarlamak veya ilke kodu düzenlemek için açıklanan adımları izleyin [ayarlama veya düzenleme bir ilke](../set-edit-policies.md). Diğer örnekleri görmek için bkz: [ilkesi örnekleri](../policy-samples.md).

## <a name="policy"></a>İlke

Koda Yapıştır **gelen** bloğu.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Get X-CSRF token from SAP gateway using send request policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APIM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

