---
title: Azure kullanılabilirlik bölgeleri nelerdir? | Microsoft Docs
description: Yüksek oranda kullanılabilir ve esnek uygulamaları oluşturmak için fiziksel olarak ayrı konumlardan kaynaklarınızı çalıştırmak için kullanabilirsiniz kullanılabilirlik bölgeleri sağlayın.
services: ''
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: azure
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/08/2018
ms.author: iainfou
ms.custom: mvc I am an ITPro and application developer, and I want to protect (use Availability Zones) my applications and data against data center failure (to build Highly Available applications).
ms.openlocfilehash: 7cfd3fba459f409079cb0c621edab3197070285e
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232539"
---
# <a name="what-are-availability-zones-in-azure"></a>Azure kullanılabilirlik bölgeleri nelerdir?
Kullanılabilirlik bölgeleri, uygulamaları ve verileri datacenter hatalarından korur sunan bir yüksek kullanılabilirlik ' dir. Kullanılabilirlik bölgeleri bir Azure bölgesine benzersiz fiziksel konumlara ' dir. Her bölge soğutma ve ağ bağımsız güç ile donatılmış bir veya daha fazla veri merkezleri oluşur. Dayanıklılık sağlamak için en az üç ayrı bölgelere etkinleştirilmiş tüm bölgelerde yoktur. Kullanılabilirlik bölgeleri fiziksel ayrımı bir bölge içinde uygulamaları ve verileri datacenter hatalarından korur. Bölge olarak yedekli Hizmetleri, uygulamaları ve verileri tek-noktaları-in-arızasına karşı korumak için kullanılabilirlik bölgeler arasında çoğaltılır. Kullanılabilirlik bölgeleri ile sektör en iyi % 99,99 VM çalışma süresi SLA Azure sunar. [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) şartları, Azure’un tamamının kullanılabilirlik garantisini açıklamaktadır.

Bir Azure bölgesi bir kullanılabilirlik bölgede bir arıza hem de bir güncelleştirme etki birleşimidir. Örneğin, bir Azure bölgesi üç bölgeleri arasında üç veya daha fazla sanal makine oluşturursanız, Vm'leriniz etkili bir şekilde üç hata etki alanları ve üç güncelleştirme etki alanları arasında dağıtılır. Azure platformu bu dağıtım farklı bölgelerdeki sanal makineleri aynı anda güncelleştirilmez emin olmak için güncelleştirme alanlarında tanır.

Yüksek kullanılabilirlik, işlem, depolama, ağ ve veri kaynaklarınızın bir bölge içinde birlikte bulunması ve diğer bölgelerde çoğaltmaya göre uygulama Mimarinizi oluşturun. Kullanılabilirlik bölgeleri destekleyen azure Hizmetleri iki kategoriye ayrılır:

- **Zonal Hizmetleri** – belirli bir bölgenin (örneğin, sanal makineler, yönetilen diskler, IP adresleri), kaynağa PIN veya
- **Bölge olarak yedekli Hizmetleri** – Platformu (örneğin, bölge olarak yedekli depolama, SQL veritabanı) dilimlerinde otomatik olarak çoğaltır.

Azure ile ilgili kapsamlı iş sürekliliği elde etmek için uygulama Mimarinizi Azure bölgesi çiftiyle kullanılabilirlik bölgeleri birleşimini kullanarak oluşturun. Uygulamaları ve yüksek kullanılabilirlik için bir Azure bölgesi içinde kullanılabilirlik bölgeleri kullanarak verileri zaman uyumlu olarak çoğaltabilir ve olağanüstü durum kurtarma koruması için Azure bölgeler arasında zaman uyumsuz olarak çoğaltılır.
 
![bir bölgenin bir bölgede giderek kavramsal görünümü](./media/az-overview/az-graphic-two.png)

## <a name="regions-that-support-availability-zones"></a>Kullanılabilirlik bölgeleri destekler bölgeleri

- Orta ABD
- Fransa Orta
- Doğu ABD 2 (Önizleme)
- Batı Avrupa (Önizleme)
- Güneydoğu Asya (Önizleme)


## <a name="services-that-support-availability-zones"></a>Kullanılabilirlik bölgeleri Destek Hizmetleri
Kullanılabilirlik bölgeleri destekler Azure hizmetler şunlardır:

- Linux Sanal Makineleri
- Windows Sanal Makineleri
- Sanal Makine Ölçek Kümeleri
- Yönetilen Diskler
- Load Balancer
- Genel IP adresi
- Bölgesel olarak yedekli depolama
- SQL Veritabanı


## <a name="pricing"></a>Fiyatlandırma
Bir kullanılabilirlik bölgesinde dağıtılan sanal makineleri için ek bir maliyet yoktur. Bir Azure bölgesi içindeki iki veya daha fazla kullanılabilirlik bölgeler arasında iki veya daha fazla VM dağıtıldığında % 99,99 VM çalışma süresi SLA sunulur. Ek arası kullanılabilirlik bölge VM-VM veri aktarımı ücretlerine olacaktır. Daha fazla bilgi için gözden [bant genişliği fiyatlandırma](https://azure.microsoft.com/pricing/details/bandwidth/) sayfası.


## <a name="get-started-with-availability-zones"></a>Kullanılabilirlik bölge ile çalışmaya başlama
- [Bir sanal makine oluşturun](../virtual-machines/windows/create-portal-availability-zone.md)
- [PowerShell kullanarak yönetilen Disk ekleme](../virtual-machines/windows/attach-disk-ps.md#add-an-empty-data-disk-to-a-virtual-machine)
- [Bölge olarak yedekli sanal makine ölçek kümesi oluşturma](../virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones.md)
- [Bölge olarak yedekli bir ön uç ile standart bir yük dengeleyici kullanarak bölgeler arasında Yük Dengeleme VM'ler](../load-balancer/load-balancer-standard-public-zone-redundant-cli.md)
- [Standart bir yük dengeleyici zonal bir ön uç ile kullanarak bir bölgedeki Yük Dengeleme VM'ler](../load-balancer/load-balancer-standard-public-zonal-cli.md)
- [Bölgesel olarak yedekli depolama](../storage/common/storage-redundancy-zrs.md)
- [SQL Database](../sql-database/sql-database-high-availability.md#zone-redundant-configuration-preview)


## <a name="next-steps"></a>Sonraki adımlar
- [Hızlı Başlangıç şablonları](http://aka.ms/azqs)
