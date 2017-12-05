---
title: "Azure API management ilke örnek - gönderme isteği bağlamı bilgileri arka uç hizmetine | Microsoft Docs"
description: "Azure API management ilke örnek - arka uç hizmetine istek bağlamı bilgilerini göndermek nasıl gösterir."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: af4d0a8aa3705b290fa5e4801eaa25264229696e
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="send-request-context-information-to-the-backend-service"></a>Arka uç hizmetine istek bağlamı bilgi gönder

Bu makalede arka uç hizmetine istek bağlamı bilgilerini göndermek nasıl oluşturulduğunu gösteren bir Azure API management ilke örnek gösterilmektedir. Ayarlamak veya ilke kodu düzenlemek için açıklanan adımları izleyin [ayarlama veya düzenleme bir ilke](../set-edit-policies.md). Diğer örnekleri görmek için bkz: [ilkesi örnekleri](../policy-samples.md).

## <a name="policy"></a>İlke

Koda Yapıştır **gelen** bloğu.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Send request context information to the backend service.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APIM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

