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
ms.openlocfilehash: e47a3736e814229e881a314c7cb05054369db1f0
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="send-errors-to-stackify-for-logging"></a>Hatalar için günlüğü Stackify için Gönder

Bu makalede, hatalar için günlüğü için Stackify göndermek için bir hata günlük kaydı ilkesi ekleme gösteren bir Azure API management ilke örnek gösterilmektedir. Ayarlamak veya ilke kodu düzenlemek için açıklanan adımları izleyin [ayarlama veya düzenleme bir ilke](../set-edit-policies.md). Diğer örnekleri görmek için bkz: [ilkesi örnekleri](../policy-samples.md).

## <a name="policy"></a>İlke

Koda Yapıştır **hata** bloğu.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Log errors to Stackify.policy.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APIM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

