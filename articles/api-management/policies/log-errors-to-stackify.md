---
title: Azure API management ilke örnek - Stackify gönderme hataları günlüğe kaydetme için | Microsoft Docs
description: Azure API management ilke örnek - gösteren hatalar için günlüğü için Stackify göndermek için bir hata günlük kaydı ilkesi ekleme...
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
ms.openlocfilehash: 5daf21cb55289c874d56910b1240e1433f3d55d0
ms.sourcegitcommit: e34afd967d66aea62e34d912a040c4622a737acb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36945964"
---
# <a name="send-errors-to-stackify-for-logging"></a>Hatalar için günlüğü Stackify için Gönder

Bu makalede, hatalar için günlüğü için Stackify göndermek için bir hata günlük kaydı ilkesi ekleme gösteren bir Azure API management ilke örnek gösterilmektedir. Ayarlamak veya ilke kodu düzenlemek için açıklanan adımları izleyin [ayarlama veya düzenleme bir ilke](../set-edit-policies.md). Diğer örnekleri görmek için bkz: [ilkesi örnekleri](../policy-samples.md).

## <a name="policy"></a>İlke

Koda Yapıştır **hata** bloğu.

[!code-xml[Main](../../../api-management-policy-samples/examples/Log errors to Stackify.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APIM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

