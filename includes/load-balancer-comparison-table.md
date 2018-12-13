---
title: include dosyası
description: include dosyası
services: load balancer
author: KumudD
ms.service: load-balancer
ms.topic: include
ms.date: 12/11/2018
ms.author: kumud
ms.custom: include file
ms.openlocfilehash: 61af77897de0ad860eb01ee309bbeedf939e466b
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53326665"
---
| | Standart SKU | Temel SKU |
| --- | --- | --- |
| Arka uç havuzu boyutu | 1000'e kadar örnek destekler | 100 örneğe kadar destekler |
| Arka uç havuzu uç noktaları | blend, sanal makinelerin kullanılabilirlik kümeleri dahil olmak üzere tek bir sanal ağ içindeki herhangi bir sanal makine, sanal makine ölçek kümeleri. | Sanal makineleri tek bir kullanılabilirlik kümesi veya sanal makine ölçek kümesi. |
| [Sistem durumu yoklamaları](../articles/load-balancer/load-balancer-custom-probe-overview.md#types) | TCP VE HTTP, HTTPS | TCP VE HTTP |
| [Sistem durumu araştırma davranışını aşağı](../articles/load-balancer/load-balancer-custom-probe-overview.md#probedown) | TCP bağlantıları örneğini araştırma hakkında Canlı kalmasını __ve__ tüm araştırmalar üzerinde. | TCP bağlantıları örneğini araştırma üzerinde etkin kalır. Tüm TCP bağlantılarını tüm sonlandırmak araştırmaları olan aşağı. |
| Kullanılabilirlik Alanları | Standart SKU, gelen ve giden için bölgesel olarak yedekli ve bölgesel ön uçlar, bölge hatası, bölgeler arası Yük Dengeleme giden akışlar eşlemeleri önceliklidir. | Yok |
| Tanılama | Azure İzleyici, bayt ve paket sayaçları, sistem durumu da dahil olmak üzere çok boyutlu ölçümler araştırma durumu, bağlantı denemeleri (TCP SYN), giden bağlantı durumu (SNAT başarılı ve başarısız akışlar), etkin veri düzlemi ölçümleri | Azure Log Analytics genel Load Balancer için yalnızca SNAT tükenmesi uyarı, arka uç havuzu durumu sayısı. |
| HA bağlantı noktaları | İç Yük Dengeleyici | Kullanılamaz |
| Varsayılan olarak güvenli | İzin verilenler listesinde bir ağ güvenlik grubu tarafından gelen sürece ortak IP ve yük dengeleyici uç noktaları için (genel ve dahili) izin verilmez. | Ağ güvenlik grubu isteğe bağlı varsayılan açın. |
| [Giden bağlantılar](../articles/load-balancer/load-balancer-outbound-connections.md) | Giden NAT havuzu tabanlı ile açıkça tanımlayabileceğiniz [giden kuralları](../articles/load-balancer/load-balancer-outbound-rules-overview.md). Yük Dengeleme kuralı çevirme başına birden çok ön uç ile kullanabilirsiniz. Giden bir senaryo _gerekir_ oluşturulabilir sanal makineyi giden bağlantı kullanmak için.  Sanal ağ hizmet uç noktalarına giden bağlantı erişilebilir ve doğru işlenen veri sayılmaz.  Sanal ağ hizmet uç noktaları kullanılabilir değil Azure PaaS Hizmetleri dahil olmak üzere tüm genel IP adresleri, giden bağlantı ve işlenen veri doğrultusunda sayısı üzerinden erişilmesi gereken. Bir sanal makine yalnızca bir iç yük dengeleyici hizmet veren, varsayılan SNAT üzerinden giden bağlantılara kullanılamaz; kullanma [giden kuralları](../articles/load-balancer/load-balancer-outbound-rules-overview.md) yerine. Giden SNAT programlama gelen Yük Dengeleme kuralı protokolü temel aktarım belirli protokolüdür. | Birden çok ön uç mevcut olduğunda rastgele seçilmiş tek ön uç.  İç Load Balancer bir sanal makine görevi gördüğünden, varsayılan SNAT kullanılır. |
| [Giden kuralları](../articles/load-balancer/load-balancer-outbound-rules-overview.md) | Genel IP adresleri veya ortak IP ön ekleri veya her ikisi de kullanarak, bildirim temelli giden NAT yapılandırma yapılandırılabilir giden boşta kalma zaman aşımı, özel SNAT bağlantı noktası ayırma | Kullanılamaz |
|  [Boşta kalma TCP Sıfırla](../articles/load-balancer/load-balancer-tcp-reset.md) | Herhangi bir kural TCP boşta kalma zaman aşımı üzerinde (TCP k) sıfırlama etkinleştirme | Kullanılamaz |
| [Birden çok ön uç](../articles/load-balancer/load-balancer-multivip-overview.md) | Gelen ve [giden](../articles/load-balancer/load-balancer-outbound-connections.md) | Yalnızca gelen |
| Yönetim işlemleri | Çoğu operations < 30 saniye | 60-90 saniye tipik. |
| SLA | veri yolu ile iki sağlıklı sanal makine için % 99,99 oranında. | [VM SLA nda örtük](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/). | 
| Fiyatlandırma | İşlenen veri kuralları sayısına göre gelen ve giden kaynakla ilişkili ücretlendirilir.  | Ücretsiz |
|  |  |  |
