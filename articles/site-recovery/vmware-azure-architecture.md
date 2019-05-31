---
title: Vmware'den Azure'a olağanüstü durum kurtarma mimaride Azure Site Recovery | Microsoft Docs
description: Bu makalede Azure'da Azure Site Recovery ile şirket içi VMware vm'lerinin olağanüstü durum kurtarma ayarlama çoğaltırken kullanılan bileşenler ve genel bir bakış sağlar.
author: rayne-wiselman
ms.service: site-recovery
services: site-recovery
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: raynew
ms.openlocfilehash: f1fdbd143093beb9736e86b24b76843ad82b89f2
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66418364"
---
# <a name="vmware-to-azure-disaster-recovery-architecture"></a>Vmware'den Azure'a olağanüstü durum kurtarma mimarisi

Bu mimari ve işlemlerdeki olağanüstü durum kurtarma çoğaltma, yük devretme ve kurtarma VMware sanal makinelerini (VM) dağıtırken kullanılan bir şirket içi VMware sitesi ve Azure kullanarak arasında makalede [Azure Site Recovery](site-recovery-overview.md) hizmeti.


## <a name="architectural-components"></a>Mimari bileşenler

Aşağıdaki tablo ve grafik Vmware'den azure'a olağanüstü durum kurtarma için kullanılan bileşenleri üst düzey bir görünümünü sağlar.

**Bileşen** | **Gereksinim** | **Ayrıntılar**
--- | --- | ---
**Azure** | Bir Azure aboneliği, önbellek, yönetilen Disk ve Azure ağ için Azure depolama hesabı. | Şirket içi VM'den çoğaltılan veriler Azure Depolama'da saklanır. Şirket içinden Azure'a yük devretme çalıştırdığınızda çoğaltılan verilerle azure Vm'leri oluşturulur. Azure VM’leri oluşturulduğunda Azure sanal ağına bağlanır.
**Configuration server makinesi** | Bir tek şirket içi makine. Bu dağıtılabilir bir VMware VM yüklenen bir OVF şablondan çalıştırmanızı öneririz.<br/><br/> Makine yapılandırma sunucusu, işlem sunucusu ve ana hedef sunucusu içeren tüm şirket içi Site Recovery bileşenlerini çalıştırır. | **Yapılandırma sunucusu**: Şirket içi ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir.<br/><br/> **İşlem sunucusu**: Varsayılan olarak yapılandırma sunucusuna yüklenir. Bu çoğaltma verilerini alıp; Bu, önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir; ve Azure depolamaya gönderir. İşlem sunucusu, çoğaltmak istediğiniz Vm'lere de Azure Site Recovery Mobility hizmeti yükler ve şirket içi makineleri otomatik olarak bulunmasını gerçekleştirir. Dağıtımınız büyüdükçe, daha büyük çoğaltma trafiği hacimlerini idare etmek ayrı, ek işlem sunucuları ekleyebilirsiniz.<br/><br/> **Ana hedef sunucusu**: Varsayılan olarak yapılandırma sunucusuna yüklenir. Azure'dan yeniden çalışma sırasında çoğaltma verilerini işler. Büyük dağıtımlar için yeniden çalışma için bir ek, ayrı bir ana hedef sunucusu ekleyebilirsiniz.
**VMware sunucuları** | VMware Vm'leri, şirket içi vSphere ESXi sunucularında barındırılır. Ana bilgisayarları yönetmek için bir vCenter sunucusu öneririz. | Site Recovery dağıtımı sırasında VMware sunucularını kurtarma Hizmetleri Kasası'na ekleyin.
**Çoğaltılan makineler** | Mobility hizmeti Çoğalttığınız her VMware sanal makinede yüklü. | İşlem sunucusundan otomatik yükleme izin öneririz. Alternatif olarak, hizmetini el ile yükleme veya System Center Configuration Manager gibi bir otomatik dağıtım yöntemini kullanın.

**VMware-Azure arası mimari**

![Bileşenler](./media/vmware-azure-architecture/arch-enhanced.png)



## <a name="replication-process"></a>Çoğaltma işlemi

1. Bir sanal makine için çoğaltmayı etkinleştirdiğinizde, belirtilen çoğaltma ilkesini kullanarak Azure depolama için ilk çoğaltma başlar. Şunlara dikkat edin:
    - VMware Vm'leri için çoğaltma blok düzeyinde, yakın sürekli, sanal makinede çalışan Mobility Hizmeti Aracısı kullanılarak yapılır.
    - Herhangi bir çoğaltma ilkesi ayarı uygulanır:
        - **RPO eşiği**. Bu ayar, çoğaltma etkilemez. İzleme ile yardımcı olur. Bir olay tetiklenir ve geçerli RPO, belirttiğiniz eşik sınırını aşarsa, isteğe bağlı olarak bir e-posta, gönderdi.
        - **Kurtarma noktası bekletme**. Bu ayar, bir kesinti oluştuğunda gitmek için istediğiniz zaman içinde ne kadar geriye belirtir. Premium depolama maksimum bekletme 24 saattir. Standart depolama alanında, değer 72 saattir. 
        - **Uygulamayla tutarlı anlık görüntüleri**. Uygulamayla tutarlı anlık görüntü olması gerçekleştirebileceğiniz her 1 uygulama gereksinimlerinize bağlı olarak 12 saat. Anlık görüntü, standart Azure blob anlık görüntüleridir. Bir VM'de çalışan Mobility Aracısı, bu ayar ve -belirli bir noktaya tutarlı bir uygulama olarak çoğaltma akışında noktası yer işaretleri uygun olarak bir VSS anlık görüntüsünün ister.

2. Trafik, internet üzerinden genel uç noktaları Azure depolama alanına çoğaltır. Alternatif olarak, Azure ExpressRoute ile kullanabileceğiniz [Microsoft eşlemesi](../expressroute/expressroute-circuit-peerings.md#microsoftpeering). Trafiği bir siteden siteye sanal özel ağ (VPN) bir şirket içi siteden Azure'a çoğaltılması desteklenmez.
3. Delta değişikliklerinin azure'a çoğaltılması ilk çoğaltma sonlandırıldıktan sonra başlar. Bir makine için izlenen değişiklikler işlem sunucusuna gönderilir.
4. İletişim şu şekilde olur:

    - Vm'leri şirket içi yapılandırma Sunucusu'ndaki HTTPS 443 numaralı bağlantı noktasında gelen çoğaltma yönetimi için iletişim.
    - Yapılandırma sunucusu Azure çoğaltması HTTPS 443 giden bağlantı noktası üzerinden düzenler.
    - Vm'leri, gelen çoğaltma verilerini HTTPS 9443 numaralı bağlantı noktasında (yapılandırma sunucusu makinesinde çalışan) işlem sunucusuna gönderir. Bu bağlantı noktası değiştirilebilir.
    - İşlem sunucusu çoğaltma verilerini alıp, en iyi duruma getirir ve şifreler ve Azure depolamaya bağlantı noktası 443 üzerinden giden gönderir.
5. Çoğaltma verilerinin bir önbellek depolama hesabına azure'da ilk land kaydeder. Bu günlükler işlenir ve verileri Azure yönetilen diski (asr çekirdek diski olarak adlandırılır) depolanır. Bu diskteki kurtarma noktaları oluşturulur.




**Vmware'den Azure'a çoğaltma işlemi**

![Çoğaltma işlemi](./media/vmware-azure-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Yük devretme ve yeniden çalışma işlemi

Gerektiği şekilde çoğaltma ayarlandıktan sonra her şeyin beklendiği gibi çalıştığını denetlemek için olağanüstü durum kurtarma tatbikatı (yük devretme testi) çalıştırın, yük devretme ve yeniden çalışma çalıştırabilirsiniz.

1. Tek bir makine için yük çalıştırmak veya kurtarma aynı anda birden çok VM'in devredilmesini düzenleyebilirsiniz planları oluşturun. Tek makine yük devretme yerine bir kurtarma planı avantajı şunlardır:
    - Tüm sanal makineleri uygulama bir tek kurtarma planına dahil ederek Uygulama bağımlılıklarını modelleyebilirsiniz.
    - Komut dosyaları, Azure runbook'ları ekleyebilir ve el ile gerçekleştirilecek eylemler duraklatın.
2. İlk yük devretmeyi tetiklemeden sonra Azure VM üzerindeki iş yüküne erişmeye başlamak üzere yürütürsünüz.
3. Birincil yerinde siteniz yeniden kullanılabilir olduğunda geri dönmesini hazırlayabilirsiniz. Yeniden çalışma için bir yeniden çalışma altyapısı ayarlarsınız gerekir dahil olmak üzere:

    * **Azure'da geçici işlem sunucusu**: Azure'dan yeniden çalışma için azure'dan çoğaltmayı düzenlemek için bir işlem sunucusu olarak görev yapacak bir Azure VM'yi ayarlayın. Yeniden çalışma sona erdikten sonra bu VM'yi silebilirsiniz.
    * **VPN bağlantısı**: Yeniden çalışma için bir VPN bağlantısı (veya ExpressRoute) Azure ağından şirket içi siteye gerekir.
    * **Ayrı bir ana hedef sunucusu**: Varsayılan olarak, şirket içi VMware VM üzerindeki yapılandırma sunucusu ile yüklenen ana hedef sunucusu yeniden çalışma işler. Geri büyük hacimli trafikte başarısız gerekiyorsa, bu amaç için ayrı şirket içi ana hedef sunucusu ayarlamanız ayarlayın.
    * **Yeniden çalışma ilkesi**: Şirket içi siteye geri çoğaltılması için bir yeniden çalışma ilkesi gerekir. Şirket içinden Azure'a çoğaltma ilkesi oluşturduğunuzda bu ilke otomatik olarak oluşturulur.
4. Bileşenleri hazır olduktan sonra yeniden çalışma üç eylemler gerçekleşir:

    - 1. Aşama: Böylece bunlar Azure'dan çoğaltma Azure Vm'lerini yeniden koruma şirket içi VMware Vm'lerini geri dönün.
    -  2. Aşama: Şirket içi siteye yük devretme çalıştırın.
    - 3. Aşama: İş yüklerini geri başarısız olduktan sonra şirket içi Vm'leri için çoğaltmayı yeniden etkinleştirin.
    
 
**Azure'dan VMware**

![Yeniden çalışma](./media/vmware-azure-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Sonraki adımlar

İzleyin [Bu öğreticide](vmware-azure-tutorial.md) VMware-Azure arası çoğaltmayı etkinleştirme için.
