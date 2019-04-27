---
title: Azure API management ilke örneği - Stackify'i gönderme hataları günlüğe kaydetme için | Microsoft Docs
description: Azure API management ilke örneği - gösteren hatalar için günlüğü için Stackify'i göndermek için bir hata günlük kaydı ilkesi ekleme...
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
ms.openlocfilehash: 07cc83830fe2d467c611622bb66dfbb8c9429c2d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60860547"
---
# <a name="send-errors-to-stackify-for-logging"></a>Hataları için günlüğü Stackify'i için gönderme

Bu makalede, hatalar için günlüğü için Stackify'i göndermek için bir hata günlük kaydı ilkesi ekleme gösteren bir Azure API management ilke örneği gösterilmektedir. Ayarlama veya ilke kodu düzenleme için açıklanan adımları izleyin [ayarlama veya düzenleme ilke](../set-edit-policies.md). Diğer örnekler için bkz [ilkesi örnekleri](../policy-samples.md).

## <a name="policy"></a>İlke

Kodun içine yapıştırın **hata** blok.

[!code-xml[Main](../../../api-management-policy-samples/examples/Log errors to Stackify.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APIM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

