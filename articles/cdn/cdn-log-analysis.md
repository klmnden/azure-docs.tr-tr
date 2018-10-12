---
title: Azure CDN kullanım biçimlerini çözümleme | Microsoft Docs
description: Bu makalede, Azure CDN ürünü için kullanılabilir analiz raporları farklı türde açıklanır.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: 95e18b3c-b987-46c2-baa8-a27a029e3076
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: magattus
ms.openlocfilehash: 45b3698dd77bda815218b43405d64819c3e4789f
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49091275"
---
# <a name="analyze-azure-cdn-usage-patterns"></a>Azure CDN kullanım biçimlerini çözümleme

CDN için uygulamanızı etkinleştirdikten sonra CDN kullanımını izlemek, teslimatınızı durumunu denetleyin ve olası sorunları giderme. Azure CDN, bu özellikler aşağıdaki yollarla sağlar: 

## <a name="core-analytics-via-azure-diagnostic-logs"></a>Azure tanılama günlükleri aracılığıyla çekirdek analizi

Çekirdek analizi, tüm fiyatlandırma katmanları için CDN uç noktası için kullanılabilir. Azure tanılama günlükleri, çekirdek analizi, Azure depolama, event hubs'a ve Azure Log Analytics için dışarı aktarılmasına izin verin. Azure Log Analytics, kullanıcı tarafından yapılandırılabilir ve özelleştirilebilir grafikler içeren bir çözüm sunar. Azure tanılama günlükleri hakkında daha fazla bilgi için bkz. [Azure tanılama günlükleri](cdn-azure-diagnostic-logs.md).

## <a name="verizon-core-reports"></a>Verizon'dan alınan çekirdek raporlar

Azure CDN kullanıcı ile bir **verizon'dan Azure CDN standart** veya **verizon'dan Azure CDN Premium** profili Verizon'dan alınan çekirdek raporlar Verizon ek Portalı'nda görüntüleyebilirsiniz. Verizon'dan alınan çekirdek raporlar aracılığıyla erişilebilir **Yönet** seçeneği Azure portalından ve çeşitli grafikleri ve görünümleri sunar. Daha fazla bilgi için [verizon'dan alınan çekirdek raporlar](cdn-analyze-usage-patterns.md).

## <a name="verizon-custom-reports"></a>Verizon özel raporları

Azure CDN kullanıcı ile bir **verizon'dan Azure CDN standart** veya **verizon'dan Azure CDN Premium** profili Verizon ek Portalı'nda Verizon özel raporları görüntüleyebilirsiniz. Verizon özel raporları aracılığıyla erişilebilir **Yönet** Azure portalından seçeneği. Verizon özel raporları sayfa gösterir aktarılan isabet sayısı veya veri sayısı için her bir Azure CDN profiline ait CName kenar. Veri göre HTTP yanıt kodu veya önbellek durumu, bir süre içinde gruplandırılabilir. Daha fazla bilgi için [Verizon özel raporları](cdn-verizon-custom-reports.md).

## <a name="azure-cdn-premium-from-verizon-reports"></a>Verizon'dan Azure CDN Premium raporları

İle **verizon'dan Azure CDN Premium**, aşağıdaki raporları da erişebilirsiniz:
   * [Gelişmiş HTTP raporları](cdn-advanced-http-reports.md)
   * [Gerçek zamanlı istatistikler](cdn-real-time-stats.md)
   * [Uç düğümü performansı](cdn-edge-performance.md)

