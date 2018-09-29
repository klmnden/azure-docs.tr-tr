---
title: include dosyası
description: include dosyası
services: load balancer
author: KumudD
ms.service: load-balancer
ms.topic: include
ms.date: 9/26/2018
ms.author: kumud
ms.custom: include file
ms.openlocfilehash: 74835734a1dd37e07efa96fd3aa76fc0d349e47f
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47454528"
---
| | Standart SKU | Temel SKU |
| --- | --- | --- |
| Arka uç havuzu boyutu | 1000'e kadar örnek destekler | 100 örneğe kadar destekler |
| Arka uç havuzu uç noktaları | blend, sanal makinelerin kullanılabilirlik kümeleri dahil olmak üzere tek bir sanal ağ içindeki herhangi bir sanal makine, sanal makine ölçek kümeleri. | Sanal makineleri tek bir kullanılabilirlik kümesi veya sanal makine ölçek kümesi. |
| [Sistem durumu araştırmaları](../articles/load-balancer/load-balancer-custom-probe-overview.md#types) | TCP VE HTTP, HTTPS | TCP VE HTTP |
| [Sistem durumu araştırma davranışını aşağı](../articles/load-balancer/load-balancer-custom-probe-overview.md#probedown) | TCP bağlantıları örneğini araştırma hakkında Canlı kalmasını __ve__ tüm araştırmalar üzerinde. | TCP bağlantıları örneğini araştırma üzerinde etkin kalır. Tüm TCP bağlantıları tüm araştırmaları temel aşağı sonlandırın. |
| Kullanılabilirlik Alanları | Temel SKU, bölgesel olarak yedekli ve bölgesel ön uçlar için gelen ve giden, giden akışlar eşlemeleri bölge başarısızlığında varlıklarını, bölgeler arası Yük Dengeleme. | Yok |
| Tanılama | Azure İzleyici, bayt ve paket sayaçları, sistem durumu da dahil olmak üzere çok boyutlu ölçümler araştırma durumu, bağlantı denemeleri (TCP SYN), giden bağlantı durumu (SNAT başarılı ve başarısız akışlar), etkin veri düzlemi ölçümleri | Azure Log Analytics genel Load Balancer için yalnızca SNAT tükenmesi uyarı, arka uç havuzu durumu sayısı. |
| HA bağlantı noktaları | İç Yük Dengeleyici | Kullanılamaz |
| Varsayılan olarak güvenli | Gelen varsayılan genel IP ve yük dengeleyici uç noktaları için kapalı ve ağ güvenlik grubu açıkça güvenilir listeye eklenecek trafik için akış için kullanılmalıdır. | Ağ güvenlik grubu isteğe bağlı varsayılan açın. |
| [Giden bağlantılar](../articles/load-balancer/load-balancer-outbound-connections.md) | Giden NAT havuzu tabanlı ile açıkça tanımlayabileceğiniz [giden kuralları](../articles/load-balancer/load-balancer-outbound-rules-overview.md). Yük Dengeleme kuralı çevirme başına birden çok ön uç ile kullanabilirsiniz. Giden bir senaryo _gerekir_ oluşturulabilir giden bağlantı kullanabilmek sanal makine için.  Sanal ağ hizmet uç noktalarına giden bağlantı erişilebilir ve doğru işlenen veri sayılmaz.  Sanal ağ hizmet uç noktaları kullanılabilir değil Azure PaaS Hizmetleri dahil olmak üzere tüm genel IP adresleri, giden bağlantı ve işlenen veri doğrultusunda sayısı üzerinden erişilmesi gereken. Bir sanal makine yalnızca bir iç yük dengeleyici hizmet veren, varsayılan SNAT üzerinden giden bağlantılara kullanılamaz; kullanma [giden kuralları](../articles/load-balancer/load-balancer-outbound-rules-overview.md) yerine. Giden SNAT programlama gelen Yük Dengeleme kuralı protokolü temel aktarım belirli protokolüdür. | Birden çok ön uç mevcut olduğunda rastgele seçilmiş tek ön uç.  İç Load Balancer bir sanal makine görevi gördüğünden, varsayılan SNAT kullanılır. |
| [Giden kuralları](../articles/load-balancer/load-balancer-outbound-rules-overview.md) | Hangi genel IP adresleri veya ortak IP ön ekler, yapılandırılabilir giden boşta kalma zaman aşımı, özel SNAT dahil olmak üzere, bildirim temelli giden NAT yapılandırma bağlantı noktası ayırma | Kullanılamaz |
|  [Boşta kalma TCP Sıfırla](../articles/load-balancer/load-balancer-tcp-reset.md) | Herhangi bir kural TCP boşta kalma zaman aşımı üzerinde (TCP k) sıfırlama etkinleştirme | Kullanılamaz |
| [Birden çok ön uç](../articles/load-balancer/load-balancer-multivip-overview.md) | Gelen ve [giden](../articles/load-balancer/load-balancer-outbound-connections.md) | Yalnızca gelen |
| Yönetim işlemleri | Çoğu operations < 30 saniye | 60-90 saniye tipik. |
| SLA | veri yolu ile iki sağlıklı sanal makine için % 99,99 oranında. | [VM SLA nda örtük](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/). | 
| Fiyatlandırma | İşlenen veri kuralları sayısına göre gelen veya giden kaynakla ilişkili ücret.  | Ücretsiz |
|  |  |  |
