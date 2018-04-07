---
title: Günlük analizi ağ izleme hakkında | Microsoft Docs
description: Ağ çözümleri bulut, şirket içi ve karma ortamlar genelinde ağlarını yönetmek için NPM dahil olmak üzere, izleme genel bakış.
services: monitoring-and-diagnostics
documentationcenter: na
author: agummadi
manager: ''
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2018
ms.author: ajaycode
ms.openlocfilehash: 306d0e57449de41080d5473034e585f772771d51
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="network-monitoring-solutions"></a>Ağ çözümleri izleme 

Azure ağ varlıklarınızı izlemek için bir konak çözümleri sunar. Azure çözümleri ve bulut ağ trafiğini analiz ve ağ bağlantısı, ExpressRoute bağlantı hatları durumunu izlemek için yardımcı programlar sahiptir.

## <a name="network-performance-monitor-npm"></a>Ağ Performans İzleyicisi'ni (NPM)

Ağ Performans İzleyicisi'ni (NPM) her biri ağ, ağ bağlantısı, uygulamalarınız için sistem durumu izleme doğrultusunda sağlamıştır ve ağ performansını fikir sağlar özellikleri paketidir. NPM, bulut tabanlı olduğundan ve bir karma ağ arasında bağlantı izler çözüm izleme sağlar:
 
* Bulut dağıtımları ve şirket içi konumlarına
* Birden çok veri merkezi ve şube
* Görev kritik çok katmanlı uygulamalar/mikro-hizmetler
* Kullanıcı konumları ve web tabanlı uygulamalara (HTTP/HTTPs) 

Performans İzleyicisi, ExpressRoute İzleyici ve hizmet uç noktası İzleyicisi NPM içinde özellikleri izleme ve aşağıda açıklanmıştır.

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
* Kapasite, kullanım analizi, sanal ağ başına bant genişliği kullanımını planlama
* İzleme ve birincil ve ikincil yollarında uyarı
* Sanal ağlara bağlanma düşmesine Algıla

![Coğrafi harita gösteren trafiği bölgeler arasında](./media/network-monitoring-overview/expressroute-topology-map.png) 

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

## <a name="traffic-analytics"></a>Trafik Analizi
Trafik analizi, bulut ağlarda kullanıcı ve uygulama etkinlik görünürlük sağlayan bir bulut tabanlı bir çözümdür. NSG akış günlükleri Öngörüler sağlamak için analiz edilir:

* Ağlar Azure ile Internet, genel bulut bölgeleri, sanal ağlar ve alt ağlar arasındaki trafik akışı
* Uygulamalar ve algılayıcılar veya ayrılmış akış Toplayıcı uygulamaları gerek kalmadan, ağınızdaki protokolleri
* Etkin noktalarına üst talkers, chatty uygulamaları, bulut VM görüşmeleri trafiği
* Kaynakları ve sanal ağlar, kritik iş Hizmetleri ve uygulamaları arasındaki arası ilişkileri üzerinden trafik hedefleri
* Güvenlik – kötü amaçlı trafiği, bağlantı noktalarını Internet, uygulama veya Internet erişmeye VM'ler Aç...
* Kapasite kullanımı - VPN ağ geçitleri ve diğer hizmetleri kullanım eğilimleri izleme tarafından aşırı sağlama veya yetersiz kullanım sorunları ortadan kaldırmanıza yardımcı olur

Trafik Analytics yardımcı olur, kuruluşunuzun ağ etkinliği, güvenli uygulamaları ve verileri, Denetim iş yükü performansını iyileştirmek ve uyumlu kalın tıklatılabilir bilgilerle donatır.

![Coğrafi harita gösteren trafiği bölgeler arasında](../network-watcher/media/traffic-analytics/geo-map-view-showcasing-traffic-distribution-to-countries-and-continents.png) 

İlgili bağlantılar:
* [Blog gönderisi](https://aka.ms/trafficanalytics), [belgelerine](https://aka.ms/trafficanalyticsdocs), [SSS](https://docs.microsoft.com/azure/network-watcher/traffic-analytics-faq)

## <a name="dns-analytics"></a>DNS Analizi
DNS yöneticileri için yerleşik, bu çözüm toplar, çözümler ve güvenlik, işlemler ve performans ile ilgili Öngörüler sağlamak için DNS günlükleri karşılık gelen.  Özelliklerden bazıları şunlardır:

* Kötü amaçlı etki alanlarına çözümlemeye istemcileri tanımlaması
* Eski kaynak kayıtlarının tanımlama
* Sık sık sorgulanan etki alanı adları ve talkative DNS istemcileri görünürlük
* DNS sunucularında istek yükünü görünürlük
* Dinamik DNS kayıt hatalarını izleme

![DNS Analytics Panosu](./media/network-monitoring-overview/dns-analytics-overview.png) 

İlgili bağlantılar:
* [Blog gönderisi](https://blogs.technet.microsoft.com/msoms/2017/04/19/introducing-oms-dns-analytics/), [belgeleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-dns)

## <a name="miscellaneous"></a>Muhtelif Hükümler

* [Yeni fiyatlandırma](https://docs.microsoft.com/azure/log-analytics/log-analytics-network-performance-monitor-pricing-faq)
