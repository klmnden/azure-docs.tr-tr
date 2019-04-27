---
title: Azure API management ilkesi örneği - bir iletilen üstbilgi ekleme | Microsoft Docs
description: Azure API management ilke örneği - arka uç API'si uygun URL'ler oluşturmasına izin vermek için gelen istekte iletilen üstbilgi ekleme gösterir.
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
ms.openlocfilehash: b857d1780e9734ce891ce2c0ce4bedf50dfe13e9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60859504"
---
# <a name="add-a-forwarded-header"></a>İletilen üstbilgisi Ekle

Bu makalede, arka uç API'si uygun URL'ler oluşturmasına izin vermek için gelen istekte iletilen üstbilgi ekleme gösteren bir Azure API yönetimi ilke örneği gösterilmektedir. Ayarlama veya ilke kodu düzenleme için açıklanan adımları izleyin [ayarlama veya düzenleme ilke](../set-edit-policies.md). Diğer örnekler için bkz [ilkesi örnekleri](../policy-samples.md).

## <a name="code"></a>Kod

Kodun içine yapıştırın **gelen** blok.

[!code-xml[Main](../../../api-management-policy-samples/examples/Forward gateway hostname to backend for generating correct urls in responses.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APIM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)
