---
title: "Azure CDN’ye Genel Bakış | Microsoft Belgeleri"
description: "Azure İçerik Teslim Ağı&quot;nın (CDN) ne olduğunu, blobları ve statik içeriği önbelleğe alarak yüksek bant genişliği içeriği teslimi gerçekleştirmek üzere nasıl kullanılacağını öğrenin."
services: cdn
documentationcenter: 
author: lichard
manager: akucer
editor: 
ms.assetid: 866e0c30-1f33-43a5-91f0-d22f033b16c6
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 09/30/2016
ms.author: rli
translationtype: Human Translation
ms.sourcegitcommit: 9a96e297711a949ac6bc464ad9154b4ad924666d
ms.openlocfilehash: d6f56ed485eedd1e8250448c2c0794a066b11dc4


---
# <a name="overview-of-the-azure-content-delivery-network-cdn"></a>Azure İçerik Teslim Ağı'na (CDN) genel bakış
> [!NOTE]
> Bu belgede, Azure İçerik Teslim Ağı'nın (CDN) ne olduğu, nasıl çalıştığı ve her bir Azure CDN ürününün özellikleri açıklanmaktadır.  Bu bilgileri atlayıp doğrudan CDN uç noktası oluşturmaya yönelik öğreticiye gitmek istiyorsanız bkz. [Azure CDN'yi kullanma](cdn-create-new-endpoint.md).  Geçerli CDN düğümü konumlarının listesini görmek istiyorsanız bkz. [Azure CDN POP Konumları](cdn-pop-locations.md).
> 
> 

Azure İçerik Teslim Ağı (CDN), kullanıcılara içerik teslim etmek için en yüksek verimliliği sağlamak üzere stratejik olarak yerleştirilmiş konumlardaki statik web içeriğini önbelleğe alır.  CDN, dünya genelindeki fiziksel düğümlerde bulunan içeriği önbelleğe alarak, yüksek bant genişliği içeriğinin teslimi konusunda geliştiricilere genel bir çözüm sunar. 

Web sitesi varlıklarını önbelleğe almak için CDN kullanmanın avantajları şunlardır:

* Özellikle içeriğin yüklenmesi için birden çok gidiş dönüş gerektiren uygulamaların kullanımı sırasında, son kullanıcılar için daha iyi performans ve kullanıcı deneyimi.
* Bir ürün sunumu etkinliğinin başlangıcında olduğu gibi, anlık yüksek düzeyde yükü daha iyi işleyebilmek için büyük ölçeklendirme.
* Kullanıcı isteklerinin dağıtımı ve uç sunuculardan içerik sunulması yoluyla kaynağa daha az trafik gönderilir.

## <a name="how-it-works"></a>Nasıl çalışır?
![CDN'ye Genel Bakış](./media/cdn-overview/cdn-overview.png)

1. Bir kullanıcı (Alice), `<endpointname>.azureedge.net` gibi özel bir etki alanı adına sahip olan bir URL'yi kullanarak bir dosya (varlık olarak da adlandırılır) isteğinde bulunur.  DNS, isteği en iyi performans gösteren Bulunma Noktası (POP) konumuna yönlendirir.  Bu, genellikle kullanıcıya coğrafi olarak en yakın konumdaki POP'dir.
2. POP'deki uç sunucuların önbelleğinde dosya mevcut değilse uç sunucu, dosyayı kaynaktan ister.  Kaynak bir Azure Web Uygulaması, Azure Bulut Hizmeti, Azure Storage hesabı veya genel olarak erişilebilen herhangi bir web sunucusu olabilir.
3. Kaynak, dosyanın Yaşam Süresi'ni (TTL) açıklayan isteğe bağlı HTTP üst bilgileri de dahil olmak üzere, dosyayı uç sunucuya döndürür.
4. Uç sunucu, dosyayı önbelleğe alır ve dosyayı özgün istek sahibine (Alice) döndürür.  Dosya, TTL süresi dolana kadar uç sunucuda önbelleğe alınmış olarak kalır.  Kaynak, bir TTL belirtmemişse varsayılan TTL yedi gündür.
5. Bu durumda ek kullanıcılar aynı URL'yi kullanarak aynı dosyayı isteyebilir ve ayrıca, aynı POP'ye yönlendirilebilir.
6. Dosya için TTL'nin süresi dolmamışsa uç sunucu dosyayı önbellekten döndürür.  Bu, daha hızlı ve daha duyarlı bir kullanıcı deneyimi sağlar.

## <a name="azure-cdn-features"></a>Azure CDN'nin Özellikleri
Üç Azure CDN ürünü mevcuttur: **Akamai'den Azure CDN Standart**, **Verizon'dan Azure CDN Standart** ve **Verizon'dan Azure CDN Premium**.  Aşağıdaki tabloda her ürünle birlikte sunulan özellikler listelenmiştir.

|  | Standart Akamai | Standart Verizon | Premium Verizon |
| --- | --- | --- | --- |
| [Storage](cdn-create-a-storage-account-with-cdn.md), [Cloud Services](cdn-cloud-service-with-cdn.md), [Web Apps](../app-service-web/cdn-websites-with-cdn.md) ve [Media Services](../media-services/media-services-portal-manage-streaming-endpoints.md) gibi Azure hizmetleriyle kolay tümleştirme |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [REST API](https://msdn.microsoft.com/library/mt634456.aspx), [.NET](cdn-app-dev-net.md), [Node.js](cdn-app-dev-node.md), veya [PowerShell](cdn-manage-powershell.md) aracılığıyla yönetim. |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| HTTPS desteği |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| Yük dengeleme |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [DDOS](https://www.us-cert.gov/ncas/tips/ST04-015) koruması |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| IPv4/IPv6 ikili yığını |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Özel etki alanı adı desteği](cdn-map-content-to-custom-domain.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Sorgu dizesini önbelleğe alma](cdn-query-string.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Coğrafi filtreleme](cdn-restrict-access-by-country.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Hızlı temizleme](cdn-purge-endpoint.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Varlık önceden yükleme](cdn-preload-endpoint.md) | |**&#x2713;** |**&#x2713;** |
| [Temel analiz](cdn-analyze-usage-patterns.md) | |**&#x2713;** |**&#x2713;** |
| [HTTP/2 desteği](https://msdn.microsoft.com/library/mt762901.aspx) |**&#x2713;** | | |
| [Gelişmiş HTTP raporları](cdn-advanced-http-reports.md) | | |**&#x2713;** |
| [Gerçek zamanlı istatistikler](cdn-real-time-stats.md) | | |**&#x2713;** |
| [Gerçek zamanlı uyarılar](cdn-real-time-alerts.md) | | |**&#x2713;** |
| [Özelleştirilebilir, kural tabanlı içerik teslim altyapısı](cdn-rules-engine.md) | | |**&#x2713;** |
| Önbellek/üstbilgi ayarları ([kurallar altyapısı](cdn-rules-engine.md) kullanılarak) | | |**&#x2713;** |
| URL yeniden yönlendirme/yeniden yazma ([kurallar altyapısı](cdn-rules-engine.md) kullanılarak) | | |**&#x2713;** |
| Mobil cihaz kuralları ([kurallar altyapısı](cdn-rules-engine.md) kullanılarak) | | |**&#x2713;** |
| [Belirteç kimlik doğrulaması](cdn-token-auth.md)|  |  |**&#x2713;**| 


> [!TIP]
> Azure CDN'de görmek istediğiniz bir özellik mi var?  [Bize geri bildirim sağlayın](https://feedback.azure.com/forums/169397-cdn)! 
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
CDN ile çalışmaya başlamak için bkz. [Azure CDN'yi kullanma](cdn-create-new-endpoint.md).

Zaten bir CDN müşterisiyseniz artık CDN uç noktalarınızı [Microsoft Azure Portal](https://portal.azure.com) üzerinden veya [PowerShell](cdn-manage-powershell.md) ile yönetebilirsiniz.

CDN'yi uygulamalı olarak görmek için [Build 2016 oturumu videomuza](https://azure.microsoft.com/documentation/videos/build-2016-leveraging-the-new-azure-cdn-apis-to-build-wicked-fast-applications/) bakın.

Azure CDN’yi [.NET](cdn-app-dev-net.md) veya [Node.js](cdn-app-dev-node.md) ile nasıl otomatik hale getireceğinizi öğrenin.

Fiyatlandırma bilgileri için bkz. [CDN Fiyatlandırması](https://azure.microsoft.com/pricing/details/cdn/).




<!--HONumber=Nov16_HO3-->


