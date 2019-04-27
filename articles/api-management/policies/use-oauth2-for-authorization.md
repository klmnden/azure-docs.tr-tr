---
title: Azure API management ilke örneği - kullanım OAuth2 yetkilendirme arasında ağ geçidi ve arka uç için | Microsoft Docs
description: Azure API management ilke örneği - ağ geçidi ve arka ucu arasında yetkilendirme için OAuth2 kullanmayı gösterir. AAD'den erişim belirtecini alma ve bunu arka uca iletme işlemini de gösterir.
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
ms.openlocfilehash: 519233cb9e77bf48f67d869a54af771c17c7827e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60859099"
---
# <a name="use-oauth2-for-authorization-between-the-gateway-and-a-backend"></a>OAuth2 yetkilendirme arasında ağ geçidi ve arka ucu kullanın

Bu makalede, OAuth2 yetkilendirme arasında ağ geçidi ve arka ucu için kullanılacak nasıl oluşturulduğunu gösteren bir Azure API management ilke örnek gösterilmektedir. AAD'den erişim belirtecini alma ve bunu arka uca iletme işlemini de gösterir. 

Ayarlama veya ilke kodu düzenleme için açıklanan adımları izleyin [ayarlama veya düzenleme ilke](../set-edit-policies.md). Diğer örnekler için bkz [ilkesi örnekleri](../policy-samples.md).

Aşağıdaki komut dosyası, {{özelliği}} özelliklerini kullanır. Özellikleri ve bunları API Management ilkeleri kullanma hakkında bilgi edinmek için bkz. [bu](../api-management-howto-properties.md) konu.
 
## <a name="policy"></a>İlke

Kodun içine yapıştırın **gelen** blok.

[!code-xml[Main](../../../api-management-policy-samples/examples/Get OAuth2 access token from AAD and forward it to the backend.policy.xml)]
  
## <a name="next-steps"></a>Sonraki adımlar

APIM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

