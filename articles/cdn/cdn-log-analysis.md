---
title: Azure CDN kullanım desenlerini çözümleme | Microsoft Docs
description: Bu makalede Azure CDN ürünü için kullanılabilen analiz raporu farklı türleri açıklanmaktadır.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: 95e18b3c-b987-46c2-baa8-a27a029e3076
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: rli; v-deasim
ms.openlocfilehash: 61fbe6e29df787048a9694138d3c9095f5cba76b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33764902"
---
# <a name="analyze-azure-cdn-usage-patterns"></a>Azure CDN kullanım biçimlerini çözümleme

Uygulamanız için CDN etkinleştirdikten sonra CDN kullanımını izlemek, teslimat durumunu denetleyin ve olası sorunları gidermek. Azure CDN, bu özellikler aşağıdaki yollarla sağlar: 

## <a name="core-analytics-via-azure-diagnostic-logs"></a>Temel analiz aracılığıyla Azure tanılama günlükleri

Temel analiz tüm fiyatlandırma katmanlarına için CDN uç noktaları için kullanılabilir. Azure tanılama günlükleri temel analiz Azure depolama, olay hub'ları veya Azure günlük analizi verilmesine izin verin. Azure günlük analizi kullanıcı tarafından yapılandırılabilir ve özelleştirilebilir grafikleri ile bir çözüm sunar. Azure tanılama günlükleri hakkında daha fazla bilgi için bkz: [Azure tanılama günlükleri](cdn-azure-diagnostic-logs.md).

## <a name="verizon-core-reports"></a>Verizon çekirdek raporları

Bir Azure CDN kullanıcı olarak bir **verizon'dan Azure CDN standart** veya **verizon'dan Azure CDN Premium** profili Verizon ek portalda Verizon çekirdek raporları görüntüleyebilirsiniz. Verizon çekirdek raporları aracılığıyla erişilebilir durumda **Yönet** Azure portalından seçeneği ve çeşitli grafikleri ve görünümler sunar. Daha fazla bilgi için bkz: [çekirdek raporları verizon'dan](cdn-analyze-usage-patterns.md).

## <a name="verizon-custom-reports"></a>Verizon özel raporlar

Bir Azure CDN kullanıcı olarak bir **verizon'dan Azure CDN standart** veya **verizon'dan Azure CDN Premium** profili Verizon ek portalda Verizon özel raporları görüntüleyebilirsiniz. Verizon özel raporlar yoluyla erişilebilir **Yönet** Azure portalından seçeneği. Her biri için aktarılan isabet veya veri sayısı Verizon özel raporlar sayfa gösterir bir Azure CDN profili ait CName kenar. Verileri bir süre boyunca HTTP yanıtı kodu veya önbellek duruma göre gruplandırılabilir. Daha fazla bilgi için bkz: [özel raporlar verizon'dan](cdn-verizon-custom-reports.md).

## <a name="azure-cdn-premium-from-verizon-reports"></a>Verizon'dan Azure CDN Premium raporları

İle **verizon'dan Azure CDN Premium**, aşağıdaki raporları da erişebilirsiniz:
   * [Gelişmiş HTTP raporları](cdn-advanced-http-reports.md)
   * [Gerçek zamanlı istatistikler](cdn-real-time-stats.md)
   * [Uç düğümü performansı](cdn-edge-performance.md)

