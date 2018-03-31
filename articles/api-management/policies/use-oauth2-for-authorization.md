---
title: Azure API management ilke örnek - kullanım OAuth2 yetkilendirme ağ geçidi ve arka uç arasında için | Microsoft Docs
description: Azure API management ilke örnek - OAuth2 için yetkilendirme ağ geçidi ve arka uç arasında nasıl kullanıldığını gösterir. AAD bir erişim belirteci edinme ve arka uç için iletme gösterir.
services: api-management
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: fc896656a4725475fc78cadb5bab54a27cfc02a2
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="use-oauth2-for-authorization-between-the-gateway-and-a-backend"></a>Ağ geçidi ve arka uç arasında yetkilendirme için OAuth2 kullanın

Bu makalede ağ geçidi ve arka uç arasında yetkilendirme OAuth2 kullanılmak üzere nasıl oluşturulduğunu gösteren bir Azure API Yönetimi İlkesi örnek gösterilmektedir. AAD bir erişim belirteci edinme ve arka uç için iletme gösterir. 

Ayarlamak veya ilke kodu düzenlemek için açıklanan adımları izleyin [ayarlama veya düzenleme bir ilke](../set-edit-policies.md). Diğer örnekleri görmek için bkz: [ilkesi örnekleri](../policy-samples.md).

Aşağıdaki komut dosyası {{özelliğinde}} görünmesi özelliklerini kullanır. Özellikleri ve API Management ilkeleri kullanma hakkında bilgi edinmek için [bu](../api-management-howto-properties.md) konu.
 
## <a name="policy"></a>İlke

Koda Yapıştır **gelen** bloğu.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Get OAuth2 access token from AAD and forward it to the backend.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APIM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

