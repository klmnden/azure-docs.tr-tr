---
title: Azure kullanılabilirlik alanları nedir? | Microsoft Docs
description: Azure'da yüksek oranda kullanılabilir ve dayanıklı uygulamalar oluşturma için fiziksel olarak ayrı konumlarda kaynaklarınızı çalıştırmak için kullanabileceğiniz kullanılabilirlik sağlayın.
services: ''
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: azure
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2018
ms.author: cynthn
ms.custom: mvc I am an ITPro and application developer, and I want to protect (use Availability Zones) my applications and data against data center failure (to build Highly Available applications).
ms.openlocfilehash: 488f7fa123692a908c36619e17166cee27d17612
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2018
ms.locfileid: "45735875"
---
# <a name="what-are-availability-zones-in-azure"></a>Azure kullanılabilirlik alanları nedir?
Kullanılabilirlik alanları, veri merkezi arızasına karşı uygulamalarınızı ve verilerinizi koruyan sunan bir yüksek kullanılabilirlik olur. Kullanılabilirlik, bir Azure bölgesi içinde benzersiz fiziksel konumlara bölgeleridir. Her bölge, soğutma ve ağ bağımsız güç ile donatılmış bir veya daha fazla veri merkezlerinden oluşur. Dayanıklılık sağlamak için üç ayrı bölge etkinleştirilmiş tüm bölgelerde en az yoktur. Bir bölge içinde kullanılabilirlik alanlarının fiziksel olarak ayrılması, uygulamaları ve verileri veri merkezi arızasına karşı korur. Bölgesel olarak yedekli Hizmetleri, uygulamaları ve verileri tek-noktaları-ın-arızasına karşı korumak için kullanılabilirlik alanları genelinde çoğaltın. Kullanılabilirlik alanları ile Azure, sektördeki en iyi % 99,99 VM çalışma SLA'sı sunar. [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) şartları, Azure’un tamamının kullanılabilirlik garantisini açıklamaktadır.

Bir Azure bölgesi içinde kullanılabilirlik alanı, hata etki alanı ve bir güncelleme etki alanı birleşimidir. Örneğin, bir Azure bölgesinde üç bölgelerindeki üç veya daha fazla VM oluşturursanız, sanal makinelerinizin etkili bir şekilde üç hata etki alanları ve üç güncelleştirme etki alanları arasında dağıtılır. Azure platform güncelleştirme etki alanlarında, farklı bölgelerdeki sanal makineleri aynı anda güncelleştirilmez emin olmak için bu dağıtım tanır.

Yüksek kullanılabilirlik uygulama mimarinizin işlem, depolama, ağ ve veri kaynaklarınızı bir bölge içinde birlikte bulundurmak ve diğer bölgelere çoğaltma oluşturun. Kullanılabilirlik alanlarını destekleyen azure Hizmetleri'nin iki kategoriye ayrılır:

- **Bölgesel Hizmetler** – belirli bir alanı (örneğin, sanal makineler, yönetilen diskler, IP adresleri), kaynak sabitleme veya
- **Bölgesel olarak yedekli Hizmetleri** – Platformu (örneğin, bölgesel olarak yedekli depolama, SQL veritabanı) bölgeler arasında otomatik olarak çoğaltır.

Azure üzerinde kapsamlı iş sürekliliği elde etmek için uygulama mimarinizin kullanılabilirlik birleşimi olan Azure bölge çiftleri kullanarak oluşturun. Uygulamalarınızı ve verilerinizi kullanılabilirlik alanları, yüksek kullanılabilirlik için bir Azure bölgesi içinde kullanarak zaman uyumlu çoğaltma ve olağanüstü durum kurtarma koruması için Azure bölgeleri arasında zaman uyumsuz olarak çoğaltın.
 
![bir bölgede giderek alanlardan birini kavramsal görünümü](./media/az-overview/az-graphic-two.png)

## <a name="regions-that-support-availability-zones"></a>Kullanılabilirlik alanlarını destekleyen bölgeler

- Orta ABD
- Doğu ABD 2 (Önizleme)
- Fransa Orta
- Kuzey Avrupa
- Güneydoğu Asya (Önizleme)
- Batı Avrupa
- Batı ABD 2



## <a name="services-that-support-availability-zones"></a>Kullanılabilirlik alanlarını destekleyen hizmetler
Kullanılabilirlik alanlarını destekleyen Azure Hizmetleri'nin şunlardır:

- Linux Sanal Makineleri
- Windows Sanal Makineleri
- Sanal Makine Ölçek Kümeleri
- Yönetilen Diskler
- Load Balancer
- Genel IP adresi
- Bölgesel olarak yedekli depolama
- SQL Veritabanı
- Event Hubs
- Service Bus
- VPN Gateway
- ExpressRoute


## <a name="pricing"></a>Fiyatlandırma
Bir kullanılabilirlik alanında dağıtılan sanal makineler için hiçbir ek ücret yoktur. Bir Azure bölgesi içinde iki veya daha fazla kullanılabilirlik bölgelerindeki iki veya daha fazla sanal makine dağıtılırken, % 99,99 VM çalışma süresi SLA sunulur. Ek arası kullanılabilirlik bölgesi VM-VM veri aktarım ücretleri olacaktır. Daha fazla bilgi için gözden [bant genişliği fiyatlandırma](https://azure.microsoft.com/pricing/details/bandwidth/) sayfası.


## <a name="get-started-with-availability-zones"></a>Kullanılabilirlik alanları ile çalışmaya başlama
- [Sanal makine oluşturma](../virtual-machines/windows/create-portal-availability-zone.md)
- [PowerShell kullanarak yönetilen Disk ekleme](../virtual-machines/windows/attach-disk-ps.md#add-an-empty-data-disk-to-a-virtual-machine)
- [Bölge yedekli sanal makine ölçek kümesi oluşturma](../virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones.md)
- [Sanal makineleri ile bölgesel olarak yedekli bir ön uç bir Standard Load Balancer'ı kullanarak bölgeler arasında Yük Dengelemesi](../load-balancer/load-balancer-standard-public-zone-redundant-cli.md)
- [Bölgesel bir ön uç ile bir Standard Load Balancer'ı kullanarak bir bölge içerisindeki Vm'lerde Yük Dengeleme](../load-balancer/load-balancer-standard-public-zonal-cli.md)
- [Bölgesel olarak yedekli depolama](../storage/common/storage-redundancy-zrs.md)
- [SQL Database](../sql-database/sql-database-high-availability.md#zone-redundant-configuration-preview)
- [Event Hubs coğrafi olağanüstü durum kurtarma](../event-hubs/event-hubs-geo-dr.md#availability-zones-preview)
- [Service Bus coğrafi olağanüstü durum kurtarma](../service-bus-messaging/service-bus-geo-dr.md#availability-zones-preview)
- [Bölgesel olarak yedekli sanal ağ geçidi oluşturma](../vpn-gateway/create-zone-redundant-vnet-gateway.md)


## <a name="next-steps"></a>Sonraki adımlar
- [Hızlı Başlangıç şablonları](http://aka.ms/azqs)
