---
title: "Azure API management ilke örnek - kullanım OAuth2 yetkilendirme ağ geçidi ve arka uç arasında için | Microsoft Docs"
description: "Azure API management ilke örnek - OAuth2 için yetkilendirme ağ geçidi ve arka uç arasında nasıl kullanıldığını gösterir. AAD bir erişim belirteci edinme ve arka uç için iletme gösterir."
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
ms.openlocfilehash: e0aeec66f23033f916c782c8a895e725b0735b62
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="use-oauth2-for-authorization-between-the-gateway-and-a-backend"></a>Ağ geçidi ve arka uç arasında yetkilendirme için OAuth2 kullanın

Bu makalede ağ geçidi ve arka uç arasında yetkilendirme OAuth2 kullanılmak üzere nasıl oluşturulduğunu gösteren bir Azure API Yönetimi İlkesi örnek gösterilmektedir. AAD bir erişim belirteci edinme ve arka uç için iletme gösterir. Ayarlamak veya ilke kodu düzenlemek için açıklanan adımları izleyin [ayarlama veya düzenleme bir ilke](../set-edit-policies.md). Diğer örnekleri görmek için bkz: [ilkesi örnekleri](../policy-samples.md).

## <a name="policy"></a>İlke

Koda Yapıştır **gelen** bloğu.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Get OAuth2 access token from AAD and forward it to the backend.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APIM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

