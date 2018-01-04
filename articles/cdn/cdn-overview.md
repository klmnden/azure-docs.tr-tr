---
title: "Azure CDN’ye Genel Bakış | Microsoft Belgeleri"
description: "Azure Content Delivery Network’ün (CDN) ne olduğunu, blobları ve statik içeriği önbelleğe alarak yüksek bant genişliği içeriği teslimi gerçekleştirmek üzere nasıl kullanılacağını öğrenin."
services: cdn
documentationcenter: 
author: dksimpson
manager: akucer
editor: 
ms.assetid: 866e0c30-1f33-43a5-91f0-d22f033b16c6
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 11/10/2017
ms.author: v-semcev
ms.openlocfilehash: cdcf07b6af2bd915345361c0bda2dcd9abe5486e
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
---
# <a name="overview-of-the-azure-content-delivery-network"></a>Azure Content Delivery Network’e Genel Bakış

Azure Content Delivery Network (CDN), kullanıcılara güvenli bir şekilde içerik teslim etmek için en yüksek aktarım hızını sağlamak üzere stratejik olarak yerleştirilmiş konumlardaki statik web içeriğini önbelleğe alır. CDN, dünya genelindeki fiziksel düğümlerde bulunan içeriği önbelleğe alarak, yüksek bant genişliği içeriğinin hızlı bir şekilde teslimi konusunda geliştiricilere genel bir çözüm sunar. 

> [!NOTE]
> Bu makalede Azure CDN, nasıl çalıştığı ve her bir Azure CDN ürününün özellikleri açıklanmaktadır. Bu bilgileri atlayıp bir CDN uç noktası oluşturmaya ilişkin öğreticiyi görüntülemek için bkz. [Azure CDN ile çalışmaya başlama](cdn-create-new-endpoint.md). Geçerli CDN düğümü konumlarının listesini görmek için bkz. [Azure CDN POP Konumları](cdn-pop-locations.md).

Web sitesi varlıklarını önbelleğe almak için CDN kullanmanın avantajları şunlardır:

* Özellikle içeriğin yüklenmesi için birden çok gidiş dönüş gerektiren uygulamaların kullanımı sırasında, son kullanıcılar için daha iyi performans ve geliştirilmiş kullanıcı deneyimi.
* Bir ürün sunumu etkinliğinin başlangıcında olduğu gibi, anlık yüksek düzeyde yükü daha iyi işleyebilmek için büyük ölçeklendirme.
* Kaynağa daha az trafik gönderilmesi için kullanıcı isteklerinin dağıtımı ve uç sunuculardan içerik sunulması.

## <a name="how-it-works"></a>Nasıl çalışır?
![CDN'ye Genel Bakış](./media/cdn-overview/cdn-overview.png)

1. Bir kullanıcı (Alice), `<endpointname>.azureedge.net` gibi özel bir etki alanı adına sahip olan bir URL'yi kullanarak bir dosya (varlık olarak da adlandırılır) isteğinde bulunur. DNS, isteği, genellikle coğrafi olarak kullanıcıya en yakın POP olan en iyi performansa sahip Bulunma Noktası (POP) konumuna yönlendirir.
2. POP'deki uç sunucuların önbelleğinde dosya mevcut değilse uç sunucu, dosyayı kaynaktan ister.  Kaynak bir Azure Web Uygulaması, Azure Bulut Hizmeti, Azure Storage hesabı veya genel olarak erişilebilen herhangi bir web sunucusu olabilir.
3. Kaynak, dosyanın Yaşam Süresi'ni (TTL) açıklayan isteğe bağlı HTTP üst bilgileri de dahil olmak üzere, dosyayı uç sunucuya döndürür.
4. Uç sunucu, dosyayı önbelleğe alır ve dosyayı özgün istek sahibine (Alice) döndürür.  Dosya, TTL süresi dolana kadar uç sunucuda önbelleğe alınmış olarak kalır.  Kaynak, bir TTL belirtmemişse varsayılan TTL yedi gündür.
5. Bu durumda ek kullanıcılar aynı URL'yi kullanarak aynı dosyayı isteyebilir ve ayrıca, aynı POP'ye yönlendirilebilir.
6. Dosya için TTL'nin süresi dolmamışsa uç sunucu dosyayı önbellekten döndürür. Bu işlem, daha hızlı ve daha duyarlı bir kullanıcı deneyimiyle sonuçlanır.

## <a name="azure-cdn-features"></a>Azure CDN'nin Özellikleri
Üç Azure CDN ürünü mevcuttur: **Akamai'den Azure CDN Standart**, **Verizon'dan Azure CDN Standart** ve **Verizon'dan Azure CDN Premium**.  Aşağıdaki tabloda her ürünle birlikte sunulan özellikler listelenmiştir.

|  | Standart Akamai | Standart Verizon | Premium Verizon |
| --- | --- | --- | --- |
| __Performans Özellikleri ve İyileştirmeler__ |
| [Dinamik Site Hızlandırma](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration) | **&#x2713;**  | **&#x2713;** | **&#x2713;** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Dinamik Site Hızlandırma - Uyarlamalı Görüntü Sıkıştırma](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#adaptive-image-compression-akamai-only) | **&#x2713;**  |  |  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Dinamik Site Hızlandırma - Nesneleri Önceden Getirme](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#object-prefetch-akamai-only) | **&#x2713;**  |  |  |
| [Video akışı iyileştirmesi](https://docs.microsoft.com/azure/cdn/cdn-media-streaming-optimization) | **&#x2713;**  | \* |  \* |
| [Büyük dosyaları iyileştirme](https://docs.microsoft.com/azure/cdn/cdn-large-file-optimization) | **&#x2713;**  | \* |  \* |
| [Genel Sunucu Yük Dengelemesi (GSLB)](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-load-balancing-azure) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Hızlı temizleme](cdn-purge-endpoint.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Varlık önceden yükleme](cdn-preload-endpoint.md) | |**&#x2713;** |**&#x2713;** |
| Önbellek/üst bilgi ayarları ([önbelleğe alma](cdn-caching-rules.md) kullanılarak) |**&#x2713;** |**&#x2713;** | |
| Önbellek/üstbilgi ayarları ([kurallar altyapısı](cdn-rules-engine.md) kullanılarak) | | |**&#x2713;** |
| [Sorgu dizesini önbelleğe alma](cdn-query-string.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| IPv4/IPv6 ikili yığını |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [HTTP/2 desteği](cdn-http2.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| __Güvenlik__ |
| CDN uç noktasıyla HTTPS desteği |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Özel etki alanı HTTPS](cdn-custom-ssl.md) | |**&#x2713;** |**&#x2713;** |
| [Özel etki alanı adı desteği](cdn-map-content-to-custom-domain.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Coğrafi filtreleme](cdn-restrict-access-by-country.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Belirteç kimlik doğrulaması](cdn-token-auth.md)|  |  |**&#x2713;**| 
| [DDOS koruması](https://www.us-cert.gov/ncas/tips/ST04-015) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| __Analiz ve Raporlama__ |
| [Azure tanılama günlükleri](cdn-azure-diagnostic-logs.md) | **&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Verizon'dan alınan çekirdek raporlar](cdn-analyze-usage-patterns.md) | |**&#x2713;** |**&#x2713;** |
| [Verizon özel raporları](cdn-verizon-custom-reports.md) | |**&#x2713;** |**&#x2713;** |
| [Gelişmiş HTTP raporları](cdn-advanced-http-reports.md) | | |**&#x2713;** |
| [Gerçek zamanlı istatistikler](cdn-real-time-stats.md) | | |**&#x2713;** |
| [Uç düğümü performansı](cdn-edge-performance.md) | | |**&#x2713;** |
| [Gerçek zamanlı uyarılar](cdn-real-time-alerts.md) | | |**&#x2713;** |
| __Kullanım Kolaylığı__ |
| [Storage](cdn-create-a-storage-account-with-cdn.md), [Cloud Services](cdn-cloud-service-with-cdn.md), [Web Apps](../app-service/app-service-web-tutorial-content-delivery-network.md) ve [Media Services](../media-services/media-services-portal-manage-streaming-endpoints.md) gibi Azure hizmetleriyle kolay tümleştirme |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [REST API](https://msdn.microsoft.com/library/mt634456.aspx), [.NET](cdn-app-dev-net.md), [Node.js](cdn-app-dev-node.md), veya [PowerShell](cdn-manage-powershell.md) aracılığıyla yönetim. |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Özelleştirilebilir, kural tabanlı içerik teslim altyapısı](cdn-rules-engine.md) | | |**&#x2713;** |
| URL yeniden yönlendirme/yeniden yazma ([kurallar altyapısı](cdn-rules-engine.md) kullanılarak) | | |**&#x2713;** |
| Mobil cihaz kuralları ([kurallar altyapısı](cdn-rules-engine.md) kullanılarak) | | |**&#x2713;** |

\* Verizon, doğrudan bir Genel Web Teslimatı aracılığıyla büyük dosya ve medya göndermeyi destekler.


> [!TIP]
> Azure CDN'de görmek istediğiniz bir özellik mi var?  [Bize geri bildirim sağlayın](https://feedback.azure.com/forums/169397-cdn)! 
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
CDN ile çalışmaya başlamak için bkz. [Azure CDN ile çalışmaya başlama](cdn-create-new-endpoint.md).

Zaten bir CDN müşterisiyseniz artık CDN uç noktalarınızı [Microsoft Azure Portal](https://portal.azure.com) üzerinden veya [PowerShell](cdn-manage-powershell.md) ile yönetebilirsiniz.

CDN’yi uygulamalı olarak görmek için [Build 2016 oturumu videosuna](https://azure.microsoft.com/documentation/videos/build-2016-leveraging-the-new-azure-cdn-apis-to-build-wicked-fast-applications/) bakın.

Azure CDN’yi [.NET](cdn-app-dev-net.md) veya [Node.js](cdn-app-dev-node.md) ile nasıl otomatik hale getireceğinizi öğrenin.

Fiyatlandırma bilgileri için bkz. [Content Delivery Network fiyatlandırması](https://azure.microsoft.com/pricing/details/cdn/).

