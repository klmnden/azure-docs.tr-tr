---
title: "Azure API Management ilkesi örnekleri | Microsoft Docs"
description: "Azure API Management'te kullanıma ilkeleri hakkında bilgi edinin."
services: api-management
documentationcenter: 
author: Juliako
manager: cflower
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2017
ms.author: apimpm
ms.openlocfilehash: ae62638fd1d325822b15b7de998861d4df67bd8e
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="api-management-policy-samples"></a>API Management ilkesi örnekleri

[İlkeleri](api-management-howto-policies.md) yapılandırma yoluyla API'nin davranışını değiştirmek yayımcı sisteminin güçlü bir özellik olan. İlkeler, bir API isteği veya yanıtı üzerinde sırayla yürütülen deyimlerin bir koleksiyonudur. Aşağıdaki tabloda, örnekleri bağlantılarını içerir ve her örnek kısa bir açıklamasını sağlar.

|||
|---|---|
|**Gelen ilkeleri**||
|[Arka uç uygun URL'ler oluşturmak için API izin vermek için iletilen üstbilgisi Ekle](./policies/set-header-to-enable-backend-to-construct-urls.md?toc=api-management/toc.json) |Arka uç uygun URL'ler oluşturmak için API izin vermek için gelen istekte iletilen üstbilgi ekleneceği gösterilmektedir.|
|[Bağıntı kimliğini içeren bir üst bilgi Ekle](./policies/add-correlation-id.md?toc=api-management/toc.json) |Gelen istek için bir bağıntı kimliği içeren üstbilgi ekleneceği gösterilmektedir.|
|[Bir arka uç hizmetine özellikleri ekleyin ve yanıt önbelleğe alma](./policies/cache-response.md?toc=api-management/toc.json) |Bir arka uç hizmetine yetenekleri ekleme gösterir. Örneğin, enlem ve boylam hava tahmini API içinde yerine yer adını kabul edin.|
|[JWT talepleri temelinde erişim yetkisi](./policies/authorize-request-based-on-jwt-claims.md?toc=api-management/toc.json) |JWT talepleri temelinde bir API belirli HTTP yöntemleri erişim yetkisi vermek gösterilmiştir.|
|[Google OAuth belirtecini kullanarak erişim yetkisi](./policies/use-google-as-oauth-token-provider.md?toc=api-management/toc.json) |Google OAuth belirteci sağlayıcısı olarak kullanarak noktalarınızı erişim yetkisi vermek gösterilmiştir.|
|[Paylaşılan erişim imzası ve Azure depolama iletme isteği oluştur](./policies/generate-shared-access-signature.md?toc=api-management/toc.json) |Nasıl oluşturulacağını gösterir [paylaşılan erişim imzası](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) ifadeler kullanarak ve isteği yeniden yazma-URI İlkesi ile Azure depolama iletebilir. |
|[AAD OAuth2 erişim belirteci almak ve arka uç için iletme](./policies/use-oauth2-for-authorization.md?toc=api-management/toc.json) |Sağlar ve ağ geçidi ve arka uç arasında yetkilendirme için OAuth2 kullanma örneği. AAD bir erişim belirteci edinme ve arka uç için iletme gösterir.|
|[Gönderme İsteği İlkesi'ni kullanarak SAP ağ geçidine belirtecinden X-CSRF Al](./policies/get-x-csrf-token-from-sap-gateway.md?toc=api-management/toc.json) |Çok sayıda API tarafından kullanılan X-CSRF desen uygulamak gösterilmiştir. Bu örnek, SAP ağ geçidine özeldir. |
|[Yol, gövde boyutuna göre isteği](./policies/route-requests-based-on-size.md?toc=api-management/toc.json) |Kendi gövdeleri boyutuna göre istekleri yönlendirmek gösterilmiştir.|
|[Arka uç hizmetine istek bağlamı bilgi gönder](./policies/send-request-context-info-to-backend-service.md?toc=api-management/toc.json) |Bazı içerik bilgilerini günlüğe kaydetme veya işlemek için arka uç hizmetine göndermek nasıl gösterir.|
|[Yanıt önbelleğe alma süresi ayarlayın](./policies/set-cache-duration.md?toc=api-management/toc.json) |Yanıt önbelleğe alma süresi arka ucu tarafından gönderilen Cache-Control üstbilgisi maxAge değerini kullanarak ayarlanacağı gösterilmiştir.|
|**Giden ilkeleri**||
|[Yanıt içeriği filtrele](./policies/filter-response-content.md?toc=api-management/toc.json) | İstekle ilişkili ürün göre yanıt yükü veri öğeleri filtrelemek gösterilmiştir.|
|**Hata ilkeleri**||
|[Stackify hataları](./policies/log-errors-to-stackify.md?toc=api-management/toc.json) |Hatalar için günlüğü için Stackify göndermek için bir hata günlük kaydı ilkesi ekleme gösterir.|
