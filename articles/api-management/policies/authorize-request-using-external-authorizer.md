---
title: Azure API management ilkesi örneği - dış yetkilendirici kullanarak isteği yetkilendirmek | Microsoft Docs
description: Azure API management ilke örneği - gösterir kullanan özel veya eski kimlik doğrulama/yetkilendirme mantığı içeren dış yetkilendirici isteklerinin nasıl yetkilendirin.
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
ms.date: 06/06/2018
ms.author: apimpm
ms.openlocfilehash: 70f43a058cfd3818dae1ccfa4b222a7d0d740aee
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46979412"
---
# <a name="authorize-requests-using-external-authorizer"></a>Dış yetkilendirici kullanarak istekleri Yetkilendir

Bu makalede, özel kimlik doğrulama/yetkilendirme mantığı içeren bir dış yetkilendirici kullanarak API erişim güvenliğinin nasıl sağlanacağını gösterir bir Azure API management ilke örneği gösterilmektedir. Ayarlama veya ilke kodu düzenleme için açıklanan adımları izleyin [ayarlama veya düzenleme ilke](../set-edit-policies.md). Diğer örnekler için bkz [ilkesi örnekleri](../policy-samples.md).

## <a name="policy"></a>İlke

Kodun içine yapıştırın **gelen** blok.

[!code-xml[Main](../../../api-management-policy-samples/examples/Authorize requests using external authorizer.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APIM ilkeleri hakkında daha fazla bilgi edinin:

+ [Erişimi kısıtlama ilkeleri](../api-management-access-restriction-policies.md)
+ [İlke örnekleri](../policy-samples.md)
