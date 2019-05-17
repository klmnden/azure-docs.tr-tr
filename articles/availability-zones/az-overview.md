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
ms.date: 05/06/2019
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 92ef494efa62a7f4e2d0a7c88ef8efd950dd2472
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65786098"
---
# <a name="what-are-availability-zones-in-azure"></a>Azure kullanılabilirlik alanları nedir?
Kullanılabilirlik alanları, veri merkezi arızasına karşı uygulamalarınızı ve verilerinizi koruyan sunan bir yüksek kullanılabilirlik olur. Kullanılabilirlik, bir Azure bölgesi içinde benzersiz fiziksel konumlara bölgeleridir. Her bölge, soğutma ve ağ bağımsız güç ile donatılmış bir veya daha fazla veri merkezlerinden oluşur. Dayanıklılık sağlamak için üç ayrı bölge etkinleştirilmiş tüm bölgelerde en az yoktur. Bir bölge içinde kullanılabilirlik alanlarının fiziksel olarak ayrılması, uygulamaları ve verileri veri merkezi arızasına karşı korur. Bölgesel olarak yedekli Hizmetleri, uygulamaları ve verileri tek-noktaları-ın-arızasına karşı korumak için kullanılabilirlik alanları genelinde çoğaltın. Kullanılabilirlik alanları ile Azure, sektördeki en iyi % 99,99 VM çalışma SLA'sı sunar. [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) şartları, Azure’un tamamının kullanılabilirlik garantisini açıklamaktadır.

Bir Azure bölgesi içinde kullanılabilirlik alanı, hata etki alanı ve bir güncelleme etki alanı birleşimidir. Örneğin, bir Azure bölgesinde üç bölgelerindeki üç veya daha fazla VM oluşturursanız, sanal makinelerinizin etkili bir şekilde üç hata etki alanları ve üç güncelleştirme etki alanları arasında dağıtılır. Azure platform güncelleştirme etki alanlarında, farklı bölgelerdeki sanal makineleri aynı anda güncelleştirilmez emin olmak için bu dağıtım tanır.

Yüksek kullanılabilirlik uygulama mimarinizin işlem, depolama, ağ ve veri kaynaklarınızı bir bölge içinde birlikte bulundurmak ve diğer bölgelere çoğaltma oluşturun. Kullanılabilirlik alanlarını destekleyen azure Hizmetleri'nin iki kategoriye ayrılır:

- **Bölgesel Hizmetler** – belirli bir alanı (örneğin, sanal makineler, yönetilen diskler, IP adresleri), kaynak sabitleme veya
- **Bölgesel olarak yedekli Hizmetleri** – Platformu (örneğin, bölgesel olarak yedekli depolama, SQL veritabanı) bölgeler arasında otomatik olarak çoğaltır.

Azure üzerinde kapsamlı iş sürekliliği elde etmek için uygulama mimarinizin kullanılabilirlik birleşimi olan Azure bölge çiftleri kullanarak oluşturun. Uygulamalarınızı ve verilerinizi kullanılabilirlik alanları, yüksek kullanılabilirlik için bir Azure bölgesi içinde kullanarak zaman uyumlu çoğaltma ve olağanüstü durum kurtarma koruması için Azure bölgeleri arasında zaman uyumsuz olarak çoğaltın.
 
![bir bölgede giderek alanlardan birini kavramsal görünümü](./media/az-overview/az-graphic-two.png)

## <a name="services-support-by-region"></a>Bölgeye göre Destek Hizmetleri

Azure Hizmetleri ve kullanılabilirlik alanlarını destekleyen bölgeleri birleşimlerini şunlardır:


|                                 |Kuzey ve Güney Amerika |              |           |           | Avrupa |              |          |              | Asya Pasifik |                 |
|----------------------------|----------|----------|---------|---------|--------------|------------|--------|----------|----------|-------------|
|          |Orta ABD|Doğu ABD|Doğu ABD 2|Batı ABD 2|Fransa Orta|Kuzey Avrupa|BK Güney|Batı Avrupa|Japonya Doğu|Güneydoğu Asya|
| **İşlem**                         |            |              |           |           |                |              |          |             |            |                |
| Linux Sanal Makineleri          | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |
| Windows Sanal Makineleri        | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |
| Sanal Makine Ölçek Kümeleri      | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |
| **Depolama**   |            |              |           |           |                |              |          |             |            |                |
| Yönetilen Diskler                   | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |
| Bölgesel olarak yedekli depolama          | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |
| **Ağ**                     |            |              |           |           |                |              |          |             |            |                |
| Standart IP adresi        | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |
| Standart Load Balancer     | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |
| VPN Ağ Geçidi                     | &#10003;   |              | &#10003;  | &#10003;  | &#10003;       | &#10003;     |          | &#10003;    |            | &#10003;       |
| ExpressRoute                    | &#10003;   |              | &#10003;  | &#10003;  | &#10003;       | &#10003;     |          | &#10003;    |            | &#10003;       |
| Application Gateway   | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     |          | &#10003;    | &#10003;       | &#10003;       |
| **Veritabanları**                     |            |              |           |           |                |              |          |             |            |                |
| SQL Veritabanı                    | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    |            | &#10003;       |
| **Analizler**                       |            |              |           |           |                |              |          |             |            |                |
| Event Hubs                      | &#10003;   |              | &#10003;  | &#10003;  | &#10003;       | &#10003;     |          | &#10003;    |            | &#10003;       |
| **Tümleştirme**                     |            |              |           |           |                |              |          |             |            |                |
| Hizmet veri yolu (yalnızca Premium katman için) | &#10003;   |              | &#10003;  | &#10003;  | &#10003;       | &#10003;     |          | &#10003;    |            | &#10003;       |



## <a name="services-resiliency"></a>Hizmetleri dayanıklılık
Tüm Azure Yönetim Hizmetleri bölge düzeyinde hatalardan dayanıklı olacak şekilde tasarlanmış. Bir bölgenin tamamını hatasına karşılaştırıldığında daha küçük bir hata RADIUS hataları spektrumu içinde bir bölgede bir veya daha fazla kullanılabilirlik alanı hataları var. Azure Yönetim Hizmetleri bölge içinde bölge düzeyinde bir hata veya başka bir Azure bölgesine geri alabilirsiniz. Azure kullanılabilirlik alanları genelinde bir bölgede dağıtılan müşteri kaynakların etkileyen hataları önlemek için bir bölge içinde bir anda alanlardan birini kritik bakım gerçekleştirir.

## <a name="pricing"></a>Fiyatlandırma
Bir kullanılabilirlik alanında dağıtılan sanal makineler için hiçbir ek ücret yoktur. Bir Azure bölgesi içinde iki veya daha fazla kullanılabilirlik bölgelerindeki iki veya daha fazla sanal makine dağıtılırken, % 99,99 VM çalışma süresi SLA sunulur. Ek arası kullanılabilirlik bölgesi VM-VM veri aktarım ücretleri olacaktır. Daha fazla bilgi için gözden [bant genişliği fiyatlandırma](https://azure.microsoft.com/pricing/details/bandwidth/) sayfası.


## <a name="get-started-with-availability-zones"></a>Kullanılabilirlik alanları ile çalışmaya başlama
- [Sanal makine oluşturun](../virtual-machines/windows/create-portal-availability-zone.md)
- [PowerShell kullanarak yönetilen Disk ekleme](../virtual-machines/windows/attach-disk-ps.md#add-an-empty-data-disk-to-a-virtual-machine)
- [Bölge yedekli sanal makine ölçek kümesi oluşturma](../virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones.md)
- [Sanal makineleri ile bölgesel olarak yedekli bir ön uç bir Standard Load Balancer'ı kullanarak bölgeler arasında Yük Dengelemesi](../load-balancer/load-balancer-standard-public-zone-redundant-cli.md)
- [Bölgesel bir ön uç ile bir Standard Load Balancer'ı kullanarak bir bölge içerisindeki Vm'lerde Yük Dengeleme](../load-balancer/load-balancer-standard-public-zonal-cli.md)
- [Alanlar arası yedekli depolama](../storage/common/storage-redundancy-zrs.md)
- [SQL Veritabanı](../sql-database/sql-database-high-availability.md#zone-redundant-configuration)
- [Event Hubs coğrafi olağanüstü durum kurtarma](../event-hubs/event-hubs-geo-dr.md#availability-zones)
- [Service Bus coğrafi olağanüstü durum kurtarma](../service-bus-messaging/service-bus-geo-dr.md#availability-zones)
- [Alanlar arası yedekli sanal ağ geçidi oluşturma](../vpn-gateway/create-zone-redundant-vnet-gateway.md)


## <a name="next-steps"></a>Sonraki adımlar
- [Hızlı Başlangıç şablonları](https://aka.ms/azqs)
