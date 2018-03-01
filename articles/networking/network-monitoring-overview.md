---
title: "Günlük analizi ağ izleme hakkında | Microsoft Docs"
description: "Ağ çözümleri bulut, şirket içi ve karma ortamlar genelinde ağlarını yönetmek için NPM dahil olmak üzere, izleme genel bakış."
services: monitoring-and-diagnostics
documentationcenter: na
author: agummadi
manager: 
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2018
ms.author: ajaycode
ms.openlocfilehash: 6d93821b59e1f69a48c3d5eeda96dad2edddb188
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="network-monitoring-solutions"></a>Ağ çözümleri izleme 

Azure ağ varlıklarınızı izlemek için bir konak çözümleri sunar. Azure çözümleri ve bulut ağ trafiğini analiz ve ağ bağlantısı, ExpressRoute bağlantı hatları durumunu izlemek için yardımcı programlar sahiptir.

## <a name="network-performance-monitor-npm"></a>Ağ Performans İzleyicisi'ni (NPM)

Ağ Performans İzleyicisi'ni (NPM) her biri ağ, ağ bağlantısı, uygulamalarınız için sistem durumu izleme doğrultusunda sağlamıştır ve ağ performansını fikir sağlar özellikleri paketidir. NPM, bulut tabanlı olduğundan ve bir karma ağ arasında bağlantı izler çözüm izleme sağlar:
 
* Bulut dağıtımları ve şirket içi konumlarına
* Birden çok veri merkezi ve şube
* Görev kritik çok katmanlı uygulamalar/mikro-hizmetler
* Kullanıcı konumları ve web tabanlı uygulamalara (HTTP/HTTPs) 

## <a name="performance-monitor"></a>Performans İzleyicisi

Performans İzleyicisi'ni NPM bir parçasıdır ve ağ bulut, karma ve şirket içi ortamları için izleme. Uzak şube ve alan ofisleri, depolama konumları, veri merkezleri ve bulut arasında ağ bağlantısı izleyebilirsiniz. Kullanıcılarınızın şikayetçi önce ağ sorunları algılayabilir. Anahtar avantajları şunlardır:

* Çeşitli alt ağları ve Uyarıları Ayarla kaybı ve gecikme izleme
* Ağdaki tüm yolları (yedekli yollar dahil) izleme
* Çoğaltma zor olan geçici ve zaman içinde nokta ağ sorunlarını giderme
* İçin performans sorumlu olduğu ağ, belirli kesiminde belirleme
* SNMP gerek kalmadan ağ durumunu izleyin

![NPM topoloji Haritası](./media/network-monitoring-overview/npm-topology-map.png) 

Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Bir ağ Performans İzleyicisi çözümü günlük analizi yapılandırın](../log-analytics/log-analytics-network-performance-monitor.md) 
* [Kullanım örnekleri](https://blogs.technet.microsoft.com/msoms/2016/08/30/monitor-on-premises-cloud-iaas-and-hybrid-networks-using-oms-network-performance-monitor/)
*  Ürün güncelleştirmeleri: [Şubat 2017](https://blogs.technet.microsoft.com/msoms/2017/02/27/oms-network-performance-monitor-is-now-generally-available/), [Ağustos 2017](https://blogs.technet.microsoft.com/msoms/2017/08/14/improvements-to-oms-network-performance-monitor/)

## <a name="expressroute-monitor"></a>ExpressRoute Monitor

ExpressRoute için NPM ExpressRoute özel eşleme bağlantıları için kapsamlı izleme sunar. ExpressRoute üzerinde E2E bağlantısı ve şube ofislerinde ve Azure arasında performansını izleyebilir. Temel işlevler şunlardır:

* Aboneliğinizle ilişkili ER devreler otomatik algılama
* Şirket içi ağ topolojisinin bulut uygulamalarınıza algılama
* Kapasite planlama, kullanma analizi
* İzleme ve birincil ve ikincil yollarında uyarı
* Sanal ağlara bağlanma düşmesine Algıla

Daha fazla bilgi için aşağıdaki makalelere bakın:

* [ExpressRoute için Ağ Performansı İzleyicisini Yapılandırma](../expressroute/how-to-npm.md)
* [blog gönderisi](https://aka.ms/NPMExRmonitorGA)

## <a name="service-endpoint-monitor"></a>Hizmet uç noktası İzleyicisi

Hizmet uç noktası izleme ile artık ulaşılabilirlik uygulamaların test edin ve şirket içi, taşıyıcı ağlar ve bulut/özel veri merkezleri üzerinden performans sorunları algılar.

* Uçtan uca ağ bağlantısı uygulamaları izleme
* Uygulama teslim ağ performansı ile ilişkilendirmek, düşmesine yol kullanıcı ve uygulama arasında kesin konumunu Algıla
* Dünya çapında uygulama ulaşılabilirlik birden çok kullanıcı konumlardan gelen sınama
* Ağ gecikme süresi ve paket kaybı hattınızın iş ve SaaS uygulamaları için belirleme
* Etkin noktalar zayıf uygulama performans neden olabilecek, ağdaki belirleme
* Microsoft Office 365, Dynamics 365 Skype Kurumsal için yerleşik testleri ve diğer Microsoft hizmetlerini kullanarak Office 365 uygulamalarına ulaşılabilirlik izleme

Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Hizmet uç noktaları izleme için ağ Performans İzleyicisi'ni yapılandırma](https://aka.ms/applicationconnectivitymonitorguide)
* [blog gönderisi](https://aka.ms/svcendptmonitor)

## <a name="next-steps"></a>Sonraki adımlar

* [Ağ Performans İzleyicisi'ni yapılandırma](https://docs.microsoft.com/azure/log-analytics/log-analytics-network-performance-monitor)
* [ExpressRoute için Ağ Performansı İzleyicisini Yapılandırma](../expressroute/how-to-npm.md)
