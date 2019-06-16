---
title: include dosyası
description: include dosyası
services: load balancer
author: KumudD
ms.service: load-balancer
ms.topic: include
ms.date: 02/08/2018
ms.author: kumud
ms.custom: include file
ms.openlocfilehash: 1d3ce900f7354b31e999c12b8e1eb0e23d391fcb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66111557"
---
| | Standart SKU | Temel SKU |
| --- | --- | --- |
| Arka uç havuzu boyutu | 1000'e kadar örneklerini destekler. | 100 örneğe kadar destekler. |
| Arka uç havuzu uç noktaları | blend, sanal makinelerin kullanılabilirlik kümeleri dahil olmak üzere tek bir sanal ağ içindeki herhangi bir sanal makine, sanal makine ölçek kümeleri. | Sanal makineleri tek bir kullanılabilirlik kümesi veya sanal makine ölçek kümesi. |
| [Sistem durumu yoklamaları](../articles/load-balancer/load-balancer-custom-probe-overview.md#types) | TCP VE HTTP, HTTPS | TCP VE HTTP |
| [Sistem durumu araştırma davranışını aşağı](../articles/load-balancer/load-balancer-custom-probe-overview.md#probedown) | TCP bağlantıları örneğini araştırma hakkında Canlı kalmasını __ve__ tüm araştırmalar üzerinde. | TCP bağlantıları örneğini araştırma üzerinde etkin kalır. Tüm TCP bağlantılarını tüm sonlandırmak araştırmaları olan aşağı. |
| Kullanılabilirlik Alanları | Standart SKU, gelen ve giden için bölgesel olarak yedekli ve bölgesel ön uçlar, bölge hatası, bölgeler arası Yük Dengeleme giden akışlar eşlemeleri önceliklidir. | Kullanılamıyor. |
| Tanılama | Azure İzleyici, bayt ve paket sayaçları, sistem durumu da dahil olmak üzere çok boyutlu ölçümler araştırma durumu, bağlantı denemeleri (TCP SYN), giden bağlantı durumu (SNAT başarılı ve başarısız akışlar), etkin veri düzlemi ölçümleri | Azure Log Analytics genel Load Balancer için yalnızca SNAT tükenmesi uyarı, arka uç havuzu durumu sayısı. |
| HA bağlantı noktaları | İç Yük Dengeleyici | Kullanılamıyor. |
| Varsayılan olarak güvenli | Genel IP, genel yük dengeleyici uç noktaları, uç noktaları olan kapalı olarak gelen akışlar sürece iç Load Balancer izin verilenler listesinde bir ağ güvenlik grubu. | Varsayılan olarak, ağ güvenliği açık grubu isteğe bağlı. |
| [Giden bağlantılar](../articles/load-balancer/load-balancer-outbound-connections.md) | Giden NAT havuzu tabanlı ile açıkça tanımlayabileceğiniz [giden kuralları](../articles/load-balancer/load-balancer-outbound-rules-overview.md). Yük Dengeleme kuralı çevirme başına birden çok ön uç ile kullanabilirsiniz. Giden bir senaryo _gerekir_ oluşturulabilir sanal makine için kullanılabilirlik kümesi, sanal makine ölçek kümesi giden bağlantı kullanmak için.  Sanal ağ hizmet uç noktalarına giden bağlantı tanımlamadan erişilebilir ve doğru işlenen veri sayılmaz.  Sanal ağ hizmet uç noktaları kullanılabilir değil Azure PaaS Hizmetleri dahil olmak üzere tüm genel IP adresleri, giden bağlantı ve işlenen veri doğrultusunda sayısı üzerinden erişilmesi gereken. Bir sanal makine yalnızca bir iç yük dengeleyici hizmet veren, kullanılabilirlik kümesi veya sanal makine ölçek kümesi, giden bağlantılara varsayılan SNAT aracılığıyla kullanılamaz; kullanma [giden kuralları](../articles/load-balancer/load-balancer-outbound-rules-overview.md) yerine. Giden SNAT programlama gelen Yük Dengeleme kuralı protokolü temel aktarım belirli protokolüdür. | Birden çok ön uç mevcut olduğunda rastgele seçilmiş tek ön uç.  İç Load Balancer, bir sanal makine hizmet kullanılabilirlik kümesi veya sanal makine ölçek kümesi, varsayılan SNAT kullanılır. |
| [Giden kuralları](../articles/load-balancer/load-balancer-outbound-rules-overview.md) | Genel IP adresleri veya ortak IP ön ekleri veya her ikisi de kullanarak, bildirim temelli giden NAT yapılandırma yapılandırılabilir giden boşta kalma zaman aşımı (4-120 dakika), özel SNAT bağlantı noktası ayırma | Kullanılamıyor. |
|  [Boşta kalma TCP Sıfırla](../articles/load-balancer/load-balancer-tcp-reset.md) | Herhangi bir kural TCP boşta kalma zaman aşımı üzerinde (TCP k) sıfırlama etkinleştirme | Kullanılamaz |
| [Birden çok ön uç](../articles/load-balancer/load-balancer-multivip-overview.md) | Gelen ve [giden](../articles/load-balancer/load-balancer-outbound-connections.md) | Yalnızca gelen |
| Yönetim işlemleri | Çoğu operations < 30 saniye | 60-90 saniye tipik. |
| SLA | veri yolu ile iki sağlıklı sanal makine için % 99,99 oranında. | Geçerli değildir. | 
| Fiyatlandırma | İşlenen veri kuralları sayısına göre gelen ve giden kaynakla ilişkili ücretlendirilir.  | Ücret alınmaz. |
|  |  |  |
