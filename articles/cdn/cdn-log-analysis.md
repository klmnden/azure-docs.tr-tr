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
ms.openlocfilehash: 3f475c5cc9b766ea9aa5bd39d4a378e8deed5e35
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="analyze-azure-cdn-usage-patterns"></a>Azure CDN kullanım biçimlerini çözümleme

Uygulamanız için CDN etkinleştirdikten sonra CDN kullanımını izlemek, teslimat durumunu denetleyin ve olası sorunları gidermek. Azure CDN, bu özellikler aşağıdaki yollarla sağlar: 

## <a name="core-analytics-via-azure-diagnostic-logs"></a>Temel analiz aracılığıyla Azure tanılama günlükleri

Temel analiz Verizon (standart ve Premium) ve Akamai (standart) CDN profilleri ait tüm CDN uç noktası için kullanılabilir. Azure tanılama günlükleri temel analiz Azure depolama, olay hub'ları veya günlük analizi verilmesine izin verin. Günlük analizi kullanıcı tarafından yapılandırılabilir ve özelleştirilebilir grafikleri ile bir çözüm sunar. Daha fazla bilgi için bkz: [Azure tanılama günlükleri](cdn-azure-diagnostic-logs.md).

## <a name="verizon-core-reports"></a>Verizon çekirdek raporları

Bir Azure CDN kullanıcı olarak bir **verizon'dan Azure CDN standart** veya **verizon'dan Azure CDN Premium** profili Verizon ek portalda Verizon çekirdek raporları görüntüleyebilirsiniz. Verizon çekirdek raporları aracılığıyla erişilebilir durumda **Yönet** Azure portalından seçeneği ve çeşitli grafikleri ve görünümler sunar. Daha fazla bilgi için bkz: [çekirdek raporları verizon'dan](cdn-analyze-usage-patterns.md).

## <a name="verizon-custom-reports"></a>Verizon özel raporlar

Bir Azure CDN kullanıcı olarak bir **verizon'dan Azure CDN standart** veya **verizon'dan Azure CDN Premium** profili Verizon ek portalda Verizon özel raporları görüntüleyebilirsiniz. Verizon özel raporlar yoluyla erişilebilir **Yönet** Azure portalından seçeneği. Her biri için aktarılan isabet veya veri sayısı Verizon özel raporlar sayfa gösterir bir Azure CDN profili ait CName kenar. Verileri bir süre boyunca HTTP yanıtı kodu veya önbellek duruma göre gruplandırılabilir. Daha fazla bilgi için bkz: [özel raporlar verizon'dan](cdn-verizon-custom-reports.md).

## <a name="verizon-premium-reports"></a>Verizon premium raporları

İle **verizon'dan Azure CDN Premium**, aşağıdaki raporları da erişebilirsiniz:
   * [Gelişmiş HTTP raporları](cdn-advanced-http-reports.md)
   * [Gerçek zamanlı istatistikler](cdn-real-time-stats.md)
   * [Uç düğümü performansı](cdn-edge-performance.md)

