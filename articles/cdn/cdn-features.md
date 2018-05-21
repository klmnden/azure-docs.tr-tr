---
title: Compare Azure Content Delivery Network (CDN) ürün özelliklerini karşılaştırın | Microsoft Docs
description: Her Azure Content Delivery Network (CDN) ürününün desteklediği özellikler hakkında bilgi edinin.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 05/09/2018
ms.author: v-deasim
ms.custom: mvc
ms.openlocfilehash: 3368a8a14a3d1314e4c7ecae9256071f1fe646f9
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="compare-azure-cdn-product-features"></a>Azure CDN ürün özelliklerini karşılaştırın

Azure Content Delivery Network (CDN), dört ürün içerir: **Microsoft’tan Azure CDN Standart** (önizlemede), **Akamai’den Azure CDN Standart**, **Verizon’dan Azure CDN Standart** ve **Verizon’dan Azure CDN Premium**. 

Aşağıdaki tabloda her ürünle birlikte sunulan özellikler karşılaştırılmaktadır.

| **Performans özellikleri ve iyileştirmeler** | **Standart Microsoft (önizleme)** | **Standart Akamai** | **Standart Verizon** | **Premium Verizon** |
| --- | --- | --- | --- | --- |
| [Dinamik site hızlandırma](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration)  |  | **&#x2713;**  | **&#x2713;** | **&#x2713;** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Dinamik site hızlandırma - uyarlamalı görüntü sıkıştırma](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#adaptive-image-compression-akamai-only)  |  | **&#x2713;**  |  |  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Dinamik site hızlandırma - nesneleri önceden getirme](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#object-prefetch-akamai-only)  |  | **&#x2713;**  |  |  |
| [Video akışı iyileştirmesi](https://docs.microsoft.com/azure/cdn/cdn-media-streaming-optimization)  | \* | **&#x2713;**  | \* |  \* |
| [Büyük dosyaları iyileştirme](https://docs.microsoft.com/azure/cdn/cdn-large-file-optimization)  | \* | **&#x2713;**  | \* |  \* |
| [Genel sunucu yük dengelemesi (GSLB)](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-load-balancing-azure)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Hızlı temizleme](cdn-purge-endpoint.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Varlık önceden yükleme](cdn-preload-endpoint.md)  |  | |**&#x2713;** |**&#x2713;** |
| Önbellek/üst bilgi ayarları ([önbelleğe alma](cdn-caching-rules.md) kullanılarak)  |  |**&#x2713;** |**&#x2713;** | |
| Önbellek/üstbilgi ayarları ([kurallar altyapısı](cdn-rules-engine.md) kullanılarak)  |  | | |**&#x2713;** |
| [Sorgu dizesini önbelleğe alma](cdn-query-string.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| Bölgesel önbelleğe alma  |**&#x2713;** |  |  |  |
| IPv4/IPv6 ikili yığını | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [HTTP/2 desteği](cdn-http2.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
||||
 **Güvenlik** | **Standart Microsoft (önizleme)** | **Standart Akamai** | **Standart Verizon** | **Premium Verizon** | 
| CDN uç noktasıyla HTTPS desteği | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Özel etki alanı HTTPS](cdn-custom-ssl.md)  | **&#x2713;** | |**&#x2713;** |**&#x2713;** |
| [Özel etki alanı adı desteği](cdn-map-content-to-custom-domain.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Coğrafi filtreleme](cdn-restrict-access-by-country.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Belirteç kimlik doğrulaması](cdn-token-auth.md)  |  |  |  |**&#x2713;**| 
| [DDOS koruması](https://www.us-cert.gov/ncas/tips/ST04-015)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Kendi sertifikanızı getirme](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#ssl-certificates) |**&#x2713;** |  |  |  |
||||
| **Analiz ve raporlama** | **Standart Microsoft (önizleme)** | **Standart Akamai** | **Standart Verizon** | **Premium Verizon** | 
| [Azure tanılama günlükleri](cdn-azure-diagnostic-logs.md)  | **&#x2713;** | **&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Verizon'dan alınan çekirdek raporlar](cdn-analyze-usage-patterns.md)  |  | |**&#x2713;** |**&#x2713;** |
| [Verizon özel raporları](cdn-verizon-custom-reports.md)  |  | |**&#x2713;** |**&#x2713;** |
| [Gelişmiş HTTP raporları](cdn-advanced-http-reports.md)  |  | | |**&#x2713;** |
| [Gerçek zamanlı istatistikler](cdn-real-time-stats.md)  |  | | |**&#x2713;** |
| [Uç düğümü performansı](cdn-edge-performance.md)  |  | | |**&#x2713;** |
| [Gerçek zamanlı uyarılar](cdn-real-time-alerts.md)  |  | | |**&#x2713;** |
||||
| **Kullanım kolaylığı** | **Standart Microsoft (önizleme)** | **Standart Akamai** | **Standart Verizon** | **Premium Verizon** | 
| [Depolama](cdn-create-a-storage-account-with-cdn.md), [Web Apps](cdn-add-to-web-app.md) ve [Media Services](../media-services/media-services-portal-manage-streaming-endpoints.md) gibi Azure hizmetleriyle kolay tümleştirme  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [REST API](https://msdn.microsoft.com/library/mt634456.aspx), [.NET](cdn-app-dev-net.md), [Node.js](cdn-app-dev-node.md), veya [PowerShell](cdn-manage-powershell.md) aracılığıyla yönetim  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Özelleştirilebilir, kural tabanlı içerik teslim altyapısı](cdn-rules-engine.md)  |  | | |**&#x2713;** |
| URL yeniden yönlendirme/yeniden yazma ([kurallar altyapısı](cdn-rules-engine.md) kullanılarak)  |  | | |**&#x2713;** |
| Mobil cihaz kuralları ([kurallar altyapısı](cdn-rules-engine.md) kullanılarak)  |  | | |**&#x2713;** |

\* Microsoft ve Verizon, doğrudan genel web teslimatı iyileştirmesi aracılığıyla büyük dosya ve medya göndermeyi destekler.



