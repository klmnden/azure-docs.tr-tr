---
title: Azure API management ilke örnek - kullanım OAuth2 yetkilendirme ağ geçidi ve arka uç arasında için | Microsoft Docs
description: Azure API management ilke örnek - OAuth2 için yetkilendirme ağ geçidi ve arka uç arasında nasıl kullanıldığını gösterir. AAD'den erişim belirtecini alma ve bunu arka uca iletme işlemini de gösterir.
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
ms.openlocfilehash: d064e918d514b9be1b9fa2dbf30c10edf5167908
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36287829"
---
# <a name="use-oauth2-for-authorization-between-the-gateway-and-a-backend"></a>Ağ geçidi ve arka uç arasında yetkilendirme için OAuth2 kullanın

Bu makalede ağ geçidi ve arka uç arasında yetkilendirme OAuth2 kullanılmak üzere nasıl oluşturulduğunu gösteren bir Azure API Yönetimi İlkesi örnek gösterilmektedir. AAD'den erişim belirtecini alma ve bunu arka uca iletme işlemini de gösterir. 

Ayarlamak veya ilke kodu düzenlemek için açıklanan adımları izleyin [ayarlama veya düzenleme bir ilke](../set-edit-policies.md). Diğer örnekleri görmek için bkz: [ilkesi örnekleri](../policy-samples.md).

Aşağıdaki komut dosyası {{özelliğinde}} görünmesi özelliklerini kullanır. Özellikleri ve API Management ilkeleri kullanma hakkında bilgi edinmek için [bu](../api-management-howto-properties.md) konu.
 
## <a name="policy"></a>İlke

Koda Yapıştır **gelen** bloğu.

[!code-xml[Main](../../../api-management-policy-samples/examples/Get OAuth2 access token from AAD and forward it to the backend.policy.xml)]
  
## <a name="next-steps"></a>Sonraki adımlar

APIM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

