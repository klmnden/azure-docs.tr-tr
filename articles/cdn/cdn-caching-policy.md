---
title: "Azure Media Services ilkesinde önbelleğe alma Azure CDN yönetme | Microsoft Docs"
description: "Azure Media Services ilkesinde önbelleğe alma Azure CDN yönetmeyi öğrenin."
services: media-services,cdn
documentationcenter: .NET
author: juliako
manager: erikre
editor: 
ms.assetid: be33aecc-6dbe-43d7-a056-10ba911e0e94
ms.service: media-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/04/2017
ms.author: juliako
ms.openlocfilehash: 0c479a58f4158bb1a72dc43432507160f65d2791
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="manage-azure-cdn-caching-policy-in-azure-media-services"></a>Azure Media Services ilkesinde önbelleğe alma Azure CDN yönetme
Azure Media Services, Uyarlamalı akış ve aşamalı indirme HTTP tabanlı sağlar. Akışa göre HTTP proxy ve CDN Katmanlar yanı sıra istemci tarafında önbelleğe alma önbelleğe alma avantajları ile yüksek düzeyde ölçeklenebilirdir. Akış uç noktaları genel akış özellikleri ve ayrıca HTTP önbellek üstbilgileri yapılandırmasını sağlar. Akış uç noktaları ayarlar HTTP Cache-Control: Maksimum yaş ve Expires üstbilgileri. Daha fazla bilgi için HTTP önbellek üstbilgileri almak [W3.org](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html).

## <a name="default-caching-headers"></a>Varsayılan önbelleğe alma üstbilgileri
Varsayılan akış uç noktaları 3 gün önbellek üstbilgileri isteğe bağlı Akış verilerini (gerçek medya parçasının/öbekleri) ve manifest(playlist) için geçerlidir. Canlı akış, akış uç noktalarını 3 gün önbellek üstbilgileri verileri (gerçek medya parçasının/öbekleri) için geçerlidir ve 2 saniye başlığı manifest(playlist) istekleri için önbelleğe al. Canlı program isteğe bağlı için (dinamik arşivi) döndüğünde isteğe bağlı Akış önbellek üstbilgileri uygulayın.

## <a name="azure-cdn-integration"></a>Azure CDN tümleştirme
Azure Media Services sağlar [CDN tümleşik](https://azure.microsoft.com/updates/azure-media-services-now-fully-integrated-with-azure-cdn/) akış uç noktaları için. Cache-control üstbilgileri akış uç noktalarını akış uç noktalarını CDN için etkin olarak aynı şekilde geçerlidir. Akış uç noktası azure CDN kullanan dahili olarak önbelleğe alınan nesnelerin yaşam süresini tanımlamak için önbellek değerleri yapılandırılmış ve aynı zamanda teslim önbellek üstbilgileri ayarlamak için bu değeri kullanır. CDN kullanarak akış uç noktalarını etkinleştirildiğinde küçük önbellek değerlerini ayarlamak için önerilmez. Küçük değerleri ayarlama performansını düşürebilir ve CDN yararı azaltın. Önbellek üstbilgileri akış uç noktalarını CDN 600 saniye etkin daha küçük ayarlamak için izin verilmiyor.

> [!IMPORTANT]
>Azure Media Services, Azure CDN ile tam tümleştirme vardır. Tek bir tıklatmayla CDN standart ve Premium ürünler dahil olmak üzere, akış uç noktası için tüm kullanılabilir Azure CDN sağlayıcıları (Akamai ve Verizon) tümleştirebilirsiniz. Daha fazla bilgi için bkz [duyuru](https://azure.microsoft.com/blog/standardstreamingendpoint/).
> 
> Akış uç noktası için CDN gelen veri ücretlerini yalnızca devre dışı CDN akış uç noktası API'leri veya Azure Yönetim Portalı'nın akış uç noktası bölümünü kullanarak üzerinden etkinleştirilirse. El ile tümleştirme veya doğrudan CDN API'leri veya portal bölüm kullanarak bir CDN uç noktası oluşturma veri ücretlerini devre dışı değil.

## <a name="configuring-cache-headers-with-azure-media-services"></a>Önbellek üstbilgileri Azure Media Services ile yapılandırma
Önbellek üstbilgi değerlerini yapılandırmak için Azure Yönetim Portalı veya Azure Media Services API'lerini kullanın.

1. Önbellek üstbilgileri kullanarak yapılandırmak için Yönetim Portalı Lütfen başvurmak için [akış uç noktalarını yönetme nasıl](../media-services/media-services-portal-manage-streaming-endpoints.md) akış uç noktası yapılandırma bölümü.
2. Azure Media Services REST API [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx#StreamingEndpointCacheControl).
3. Azure Media Services .NET SDK'sı [StreamingEndpointCacheControl özellikleri](http://go.microsoft.com/fwlink/?LinkId=615302).

## <a name="cache-configuration-precedence-order"></a>Önbellek yapılandırma öncelik sırası
1. Azure Media Services yapılandırılmış önbellek değeri varsayılan değerini geçersiz kılar.
2. El ile yapılandırma yoksa, varsayılan değerler geçerlidir.
3. Varsayılan 2 saniye önbelleğinin üstbilgileri uygular canlı akış manifest(playlist) Azure medya veya Azure depolama yapılandırması bağımsız olarak ve bu değeri geçersiz kılma kullanılabilir değil.

