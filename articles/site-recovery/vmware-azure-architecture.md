---
title: VMware-Azure Site recovery'de Azure'a çoğaltma mimarisi | Microsoft Docs
description: Bu makalede şirket içi VMware Vm'leri, Azure Site Recovery ile azure'a çoğaltırken kullanılan bileşenlere ve genel bir bakış sağlar.
author: rayne-wiselman
ms.service: site-recovery
ms.date: 08/29/2018
ms.author: raynew
ms.openlocfilehash: 4a97c44226d875a08f81a6306fc9ddd4ee29c409
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43288150"
---
# <a name="vmware-to-azure-replication-architecture"></a>Vmware'den Azure'a çoğaltma mimarisi

Bu mimari ve işlemlerdeki çoğaltma, yük devretme ve kurtarma VMware sanal makinelerini (VM) olduğunda kullanılan bir şirket içi VMware sitesi ile Azure arasında kullanarak makalede [Azure Site Recovery](site-recovery-overview.md).


## <a name="architectural-components"></a>Mimari bileşenler

Aşağıdaki tablo ve grafik azure'a VMware çoğaltması için kullanılan bileşenleri üst düzey bir görünümünü sağlar.

**Bileşen** | **Gereksinim** | **Ayrıntılar**
--- | --- | ---
**Azure** | Bir Azure aboneliği, Azure depolama hesabını ve Azure ağı. | Şirket içi vm'lerden çoğaltılan veriler depolama hesabında depolanır. Şirket içinden Azure'a yük devretme çalıştırdığınızda çoğaltılan verilerle azure Vm'leri oluşturulur. Azure VM’leri oluşturulduğunda Azure sanal ağına bağlanır.
**Configuration server makinesi** | Bir tek şirket içi makine. Bu dağıtılabilir bir VMware VM yüklenen bir OVF şablondan çalıştırmanızı öneririz.<br/><br/> Makine yapılandırma sunucusu, işlem sunucusu ve ana hedef sunucusu içeren tüm şirket içi Site Recovery bileşenlerini çalıştırır. | **Yapılandırma sunucusu**: şirket içi ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir.<br/><br/> **İşlem sunucusu**: yapılandırma sunucusu üzerinde varsayılan olarak yüklüdür. Bu çoğaltma verilerini alıp; Bu, önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir; ve Azure depolamaya gönderir. İşlem sunucusu, çoğaltmak istediğiniz Vm'lere de Azure Site Recovery Mobility hizmeti yükler ve şirket içi makineleri otomatik olarak bulunmasını gerçekleştirir. Dağıtımınız büyüdükçe, daha büyük çoğaltma trafiği hacimlerini idare etmek ayrı, ek işlem sunucuları ekleyebilirsiniz.<br/><br/> **Ana hedef sunucusu**: yapılandırma sunucusu üzerinde varsayılan olarak yüklüdür. Azure'dan yeniden çalışma sırasında çoğaltma verilerini işler. Büyük dağıtımlar için yeniden çalışma için bir ek, ayrı bir ana hedef sunucusu ekleyebilirsiniz.
**VMware sunucuları** | VMware Vm'leri, şirket içi vSphere ESXi sunucularında barındırılır. Ana bilgisayarları yönetmek için bir vCenter sunucusu öneririz. | Site Recovery dağıtımı sırasında VMware sunucularını kurtarma Hizmetleri Kasası'na ekleyin.
**Çoğaltılan makineler** | Mobility hizmeti Çoğalttığınız her VMware sanal makinede yüklü. | İşlem sunucusundan otomatik yükleme izin öneririz. Alternatif olarak, hizmetini el ile yükleme veya System Center Configuration Manager gibi bir otomatik dağıtım yöntemini kullanın.

**VMware-Azure arası mimari**

![Bileşenler](./media/vmware-azure-architecture/arch-enhanced.png)



## <a name="replication-process"></a>Çoğaltma işlemi

1. Bir sanal makine için çoğaltmayı etkinleştirdiğinizde, bu çoğaltma ilkesine uygun olarak çoğaltmaya başlar. 
2. Trafik, internet üzerinden genel uç noktaları Azure depolama alanına çoğaltır. Alternatif olarak, Azure ExpressRoute ile kullanabileceğiniz [genel eşdüzey hizmet sağlama](../expressroute/expressroute-circuit-peerings.md#azure-public-peering). Trafiği bir siteden siteye sanal özel ağ (VPN) bir şirket içi siteden Azure'a çoğaltılması desteklenmez.
3. VM verilerin ilk bir kopyası Azure depolamaya çoğaltılır.
4. Delta değişikliklerinin azure'a çoğaltılması ilk çoğaltma sonlandırıldıktan sonra başlar. Bir makine için izlenen değişiklikler bir .hrl dosyasında saklanır.
5. İletişim şu şekilde olur:

    - Vm'leri şirket içi yapılandırma Sunucusu'ndaki HTTPS 443 numaralı bağlantı noktasında gelen çoğaltma yönetimi için iletişim.
    - Yapılandırma sunucusu Azure çoğaltması HTTPS 443 giden bağlantı noktası üzerinden düzenler.
    - Vm'leri, gelen çoğaltma verilerini HTTPS 9443 numaralı bağlantı noktasında (yapılandırma sunucusu makinesinde çalışan) işlem sunucusuna gönderir. Bu bağlantı noktası değiştirilebilir.
    - İşlem sunucusu çoğaltma verilerini alıp, en iyi duruma getirir ve şifreler ve Azure depolamaya bağlantı noktası 443 üzerinden giden gönderir.


**Vmware'den Azure'a çoğaltma işlemi**

![Çoğaltma işlemi](./media/vmware-azure-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Yük devretme ve yeniden çalışma işlemi

Gerektiği şekilde çoğaltma ayarlandıktan sonra her şeyin beklendiği gibi çalıştığını denetlemek için olağanüstü durum kurtarma tatbikatı (yük devretme testi) çalıştırın, yük devretme ve yeniden çalışma çalıştırabilirsiniz.

1. Tek bir makine için yük çalıştırmak veya kurtarma aynı anda birden çok VM'in devredilmesini düzenleyebilirsiniz planları oluşturun. Tek makine yük devretme yerine bir kurtarma planı avantajı şunlardır:
    - Tüm sanal makineleri uygulama bir tek kurtarma planına dahil ederek Uygulama bağımlılıklarını modelleyebilirsiniz.
    - Komut dosyaları, Azure runbook'ları ekleyebilir ve el ile gerçekleştirilecek eylemler duraklatın.
2. İlk yük devretmeyi tetiklemeden sonra Azure VM üzerindeki iş yüküne erişmeye başlamak üzere yürütürsünüz.
3. Birincil yerinde siteniz yeniden kullanılabilir olduğunda geri dönmesini hazırlayabilirsiniz. Yeniden çalışma için bir yeniden çalışma altyapısı ayarlarsınız gerekir dahil olmak üzere:

    * **Azure'da geçici işlem sunucusu**: Azure'dan yeniden çalışma için bir Azure VM'yi azure'dan çoğaltmayı düzenlemek için bir işlem sunucusu olarak davranacak şekilde ayarlarsınız. Yeniden çalışma sona erdikten sonra bu VM'yi silebilirsiniz.
    * **VPN bağlantısı**: yeniden çalışma için bir VPN bağlantısı (veya ExpressRoute) Azure ağından şirket içi siteye gerekir.
    * **Ayrı bir ana hedef sunucusu**: varsayılan olarak, şirket içi VMware VM üzerindeki yapılandırma sunucusu ile yüklenen ana hedef sunucusu yeniden çalışma işler. Geri büyük hacimli trafikte başarısız gerekiyorsa, bu amaç için ayrı şirket içi ana hedef sunucusu ayarlamanız ayarlayın.
    * **Yeniden çalışma ilkesi**: Şirket içi sitenize geri çoğaltmak için bir yeniden çalışma ilkeniz olmalıdır. Şirket içinden Azure'a çoğaltma ilkenizi oluşturduğunuzda bu ilke otomatik olarak oluşturulur.
4. Bileşenleri hazır olduktan sonra yeniden çalışma üç eylemler gerçekleşir:

    - 1. Aşama: Azure Vm'lerini yeniden koruma, böylece bunlar Azure'dan çoğaltmak şirket içi VMware Vm'lerini geri dönün.
    -  2. Aşama: şirket içi siteye yük devretme çalıştırın.
    - 3. Aşama: iş yüklerini geri başarısız olduktan sonra şirket içi Vm'leri için çoğaltmayı yeniden etkinleştirin.
    
 
**Azure'dan VMware**

![Yeniden çalışma](./media/vmware-azure-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Sonraki adımlar

İzleyin [Bu öğreticide](vmware-azure-tutorial.md) VMware-Azure arası çoğaltmayı etkinleştirme için.
