---
title: Azure CDN önbelleğe alma İlkesi, Azure Media Services'ı yönetme | Microsoft Docs
description: Azure CDN önbelleğe alma İlkesi, Azure Media Services yönetmeyi öğrenin.
services: media-services,cdn
documentationcenter: .NET
author: juliako
manager: erikre
editor: ''
ms.assetid: be33aecc-6dbe-43d7-a056-10ba911e0e94
ms.service: media-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/04/2017
ms.author: juliako
ms.openlocfilehash: 516df2f6177303987fc0354dde647c1fc26820ef
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58918289"
---
# <a name="manage-azure-cdn-caching-policy-in-azure-media-services"></a>Azure CDN önbelleğe alma İlkesi, Azure Media Services'ı yönetme
Azure Media Services, Uyarlamalı akış ve aşamalı indirme HTTP tabanlı sağlar. Akışa göre HTTP proxy ve Katmanlar CDN önbelleğe alma yanı sıra istemci tarafı önbelleğe alma avantajları ile yüksek düzeyde ölçeklenebilirdir. Genel akış özellikleri ve ayrıca yapılandırma HTTP önbellek üst bilgiler için akış uç noktaları sağlar. Akış uç noktaları, HTTP Cache-Control ayarlar: en fazla yaş ve Expires üst bilgileri. Üst bilgiler HTTP önbelleği için daha fazla bilgi edinebilirsiniz [W3.org](https://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html).

## <a name="default-caching-headers"></a>Varsayılan önbelleğe alma üstbilgileri
Varsayılan akış uç noktalarını isteğe bağlı Akış verilerini (gerçek medya parçasının/öbekleri) ve manifest(playlist) için 3 günlük önbellek üstbilgileri uygulayın. Canlı akış için akış uç noktaları (gerçek medya parçasının/öbekleri) veri için 3 günlük önbellek üstbilgileri uygulamak ve üstbilgi manifest(playlist) istekleri için 2 saniye önbelleğe alır. Canlı program isteğe bağlı olarak (Canlı arşiv) açtığında, ardından talep üzerine akış önbellek üstbilgileri uygulayın.

## <a name="azure-cdn-integration"></a>Azure CDN tümleştirmesi
Azure Media Services sağlar [CDN tümleşik](https://azure.microsoft.com/updates/azure-media-services-now-fully-integrated-with-azure-cdn/) akış uç noktaları için. Cache-control üst bilgilerinin, akış uç noktaları için CDN, akış uç noktaları etkinleştirilmiş olarak aynı şekilde geçerlidir. Akış uç noktası azure CDN kullanır, dahili olarak önbelleğe alınan nesneleri yaşam süresini tanımlamak için önbellek değerleri yapılandırılmış ve ayrıca teslim önbellek üstbilgileri ayarlamak için bu değeri kullanır. CDN kullanarak akış uç noktalarını etkinleştirildiğinde küçük önbellek değerleri ayarlamak için önerilmez. Ayar küçük değerleri performansını azaltır ve CDN avantajı azaltın. Önbellek üstbilgileri 600 saniye CDN için akış uç noktaları etkinleştirilmiş daha küçük olarak ayarlamak için izin verilmiyor.

> [!IMPORTANT]
>Azure Media Services, Azure CDN ile tam bir tümleştirmeye sahiptir. Tek bir tıklamayla, standart ve premium ürünler dahil olmak üzere, akış uç noktası için kullanılabilir tüm Azure CDN sağlayıcıları tümleştirebilirsiniz. Daha fazla bilgi için bkz. Bu [duyuru](https://azure.microsoft.com/blog/standardstreamingendpoint/).
> 
> CDN uç noktasına akışı'ndan veri ücretleri yalnızca devre dışı CDN'yi akış uç noktası API'lerini veya Azure portal'ın akış uç noktası bölümünü kullanarak üzerinden etkinleştirilirse. El ile tümleştirme veya doğrudan portal bir bölümü veya CDN API'lerini kullanarak bir CDN uç noktası oluşturma, veri ücretleri devre dışı değil.

## <a name="configuring-cache-headers-with-azure-media-services"></a>Azure Media Services ile önbellek üst bilgilerini yapılandırma
Önbellek üstbilgi değerlerini yapılandırmak için Azure portal veya Azure Media Services API'leri kullanabilirsiniz.

1. Azure portalını kullanarak bir önbellek üst bilgileri yapılandırmak için başvurmak [nasıl akış uç noktalarını yönetme](../media-services/previous/media-services-portal-manage-streaming-endpoints.md) akış uç noktası yapılandırma bölümü.
2. Azure Media Services REST API [StreamingEndpoint](/rest/api/media/operations/streamingendpoint#StreamingEndpointCacheControl).
3. Azure Media Services .NET SDK'sı [StreamingEndpointCacheControl özellikleri](https://go.microsoft.com/fwlink/?LinkId=615302).

## <a name="cache-configuration-precedence-order"></a>Önbelleği yapılandırma öncelik sırası
1. Azure Media Services'ın yapılandırılmış önbellek değeri varsayılan değerini geçersiz kılar.
2. El ile yapılandırma yok ise, varsayılan değerler geçerlidir.
3. Varsayılan 2 saniye önbelleği tarafından üstbilgileri uygular canlı akış manifest(playlist) bakılmaksızın Azure medya veya Azure depolama yapılandırması ve bu değeri geçersiz kılma mevcut değil.

