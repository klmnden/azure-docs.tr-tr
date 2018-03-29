---
title: Azure iç yük dengeleyici genel bakış | Microsoft Docs
description: Azure ve senaryoları iç uç yapılandırma için bir iç yük dengeleyicisi nasıl çalışır.
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: tysonn
ms.assetid: 36065bfe-0ef1-46f9-a9e1-80b229105c85
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 0511165225f5a336291e86e0c504e60989933f3c
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="overview-of-azure-internal-load-balancer"></a>Azure iç yük dengeleyici genel bakış



Azure iç yük dengeleyici (ILB) yalnızca içinde bir bulut hizmeti olan veya Azure altyapı erişmek için bir VPN kullanan kaynaklara trafiğini yönlendirir. Bu bakımdan, ILB bir internet'e yönelik Yük dengeleyiciden farklıdır. Azure altyapı yük dengeli sanal IP (VIP) adresleri bir bulut hizmeti veya bir sanal ağ erişimi sınırlandırır. VIP adresleri ve sanal ağlar hiçbir zaman doğrudan bir Internet uç noktasına sunulur. İç iş kolu satır uygulama Azure'da çalıştırın ve Azure içinde veya şirket içi kaynaklardan gelen sonuna erişilir.

## <a name="why-you-might-need-an-internal-load-balancer"></a>Bir iç yük dengeleyici neden ihtiyacınız

İç yük dengeleyici Yük Dengeleme bir bulut hizmeti veya bölgesel kapsama sahip bir sanal ağ içinde duran sanal makineleri (VM'ler) arasında sağlar. Bölgesel bir kapsamla sanal ağlar hakkında daha fazla bilgi için bkz: [bölgesel sanal ağlar](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/) Azure Web günlüğündeki. Bir benzeşim grubu için yapılandırılmış varolan sanal ağları ILB kullanamazsınız.

ILB Yük Dengeleme aşağıdaki türleri sağlar:

* Bir bulut hizmeti içinde: yük aynı bulut hizmetinde bulunan sanal makineleri bir dizi Vm'lerden Dengeleme. Bu bkz <a href="#figure1">örnek</a>.
* Bir sanal ağ içinde: yük VM'lerin sanal ağ aynı bulut hizmetinde bulunan sanal makineleri bir dizi sanal ağında Dengeleme. Bu bkz <a href="#figure2">örnek</a>.
* Bir şirket içi sanal ağ için: yük sanal ağ aynı bulut hizmetinde bulunan sanal makineleri bir dizi şirket içi bilgisayarlardan Dengeleme. Bu bkz <a href="#figure3">örnek</a>.
* Çok katmanlı uygulamalar için: Yük Dengeleme uç katmanları nerede değil Internet'e Internet'e çok katmanlı uygulamalar için. Arka uç katmanları trafik Yük Dengeleme Internet'e katmanından gerektirir.
* Satır iş kolu uygulamaları için: Yük Dengeleme ek yük dengeleyici donanım veya yazılım Azure üzerinde barındırılan satır iş kolu uygulamaları için. Bu senaryo, yük dengelemesi, trafik olduğu bilgisayarları kümesinde yer alan şirket içi sunucuları içerir.

## <a name="load-balancing-for-internet-facing-multi-tier-applications"></a>Yük Dengeleme için İnternet'e yönelik çok katmanlı uygulamalar

Web katmanı Internet istemciler için internet'e yönelik uç noktalar vardır ve Yük Dengelemesi kümesinin bir parçası olur. ILB web sunucularına TCP bağlantı noktası 443 (HTTPS) için web istemcilerinden gelen trafiği dağıtır.

Web sunucuları için depolama kullanan bir ILB uç nokta arkasında veritabanı sunucularıdır. ILB uç noktası bir veritabanı hizmeti yük dengeli uç noktadır. ILB kümesindeki veritabanı sunucuları arasında Yük Dengelemesi trafiğidir.

Aşağıdaki resimde iç Yük Dengeleme aynı bulut hizmetinde Internet'e çok katmanlı uygulama için gösterilir.

<a name="figure1"></a>
![Internet'e yönelik çok katmanlı uygulama](./media/load-balancer-internal-overview/IC736321.png)

Başka bir senaryo, çok katmanlı uygulamalar için kullanılabilir. Yük Dengeleyici farklı bir bulut hizmeti için hizmet ILB için kullandığı bir dağıtılır.

Aynı sanal ağı kullanan bulut Hizmetleri ILB uç noktasına erişebilirsiniz. Aşağıdaki resimde bir veritabanı arka uç farklı bir bulut hizmetinden bulunan ön uç web sunucuları gösterir. Ön uç sunucuları aynı sanal ağ içinde ILB uç arka uç kullanın.

<a name="figure2"></a>
![Farklı bir bulut hizmeti ön uç sunucuları](./media/load-balancer-internal-overview/IC744147.png)

## <a name="load-balancing-for-intranet-line-of-business-applications"></a>Yük Dengeleme için intranet satır iş kolu uygulamaları

Şirket içi ağ üzerindeki istemcilerden Azure ağı için VPN bağlantısı kullan satır iş kolu sunucuları arasında Yük Dengelemesi trafiğidir.

İstemci Makine IP adresi Azure VPN hizmetinden bir noktadan siteye VPN kullanarak erişebilirsiniz. İş kolu satır uygulama ILB uç nokta barındırılabilir.

<a name="figure3"></a>
![ILB uç nokta barındırılan iş kolu satır uygulama](./media/load-balancer-internal-overview/IC744148.png)

Başka bir satır iş kolu uygulamaları için bir siteden siteye VPN ILB uç nokta yapılandırıldığı sanal ağ için bir senaryodur. Şirket içi ağ trafiğini ILB uç noktasına yönlendirilir.

<a name="figure4"></a>
![Şirket içi ağ ILB uç noktasına yönlendirilen trafiği](./media/load-balancer-internal-overview/IC744150.png)

## <a name="limitations"></a>Sınırlamalar

İç yük dengeleyici yapılandırmalarının SNAT desteklemez. Bu makalede, bağlantı noktası maskelemeyi kaynak ağ adresi çevirisi ilgili senaryolar için SNAT başvuruyor. VM yük dengeleyici havuzu ile ilgili iç yük dengeleyici ön uç IP adresi ulaşması gerekir. Akış yükü dengelenmiş akış kaynaklanan VM için bağlantı hataları meydana gelir. Bu senaryolar, ILB için desteklenmez. Bunun yerine bir proxy stili yük dengeleyicinin kullanılması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure yük dengeleyici için Azure Resource Manager desteği](load-balancer-arm.md)
* [Bir internet'e yönelik Yük Dengeleyici yapılandırma ile çalışmaya başlama](load-balancer-get-started-internet-arm-ps.md)
* [Bir iç yük dengeleyici yapılandırma ile çalışmaya başlama](load-balancer-get-started-ilb-arm-ps.md)
* [Yük dengeleyici dağıtım modunu yapılandırma](load-balancer-distribution-mode.md)
* [Yük dengeleyiciniz için boşta TCP zaman aşımı ayarlarını yapılandırma](load-balancer-tcp-idle-timeout.md)
