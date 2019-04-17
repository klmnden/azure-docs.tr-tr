---
title: Azure ExpressRoute, Azure Site Recovery hizmetini kullanarak Azure Vm'leri için olağanüstü durum kurtarma ile tümleştirme | Microsoft Docs
description: Azure Site Recovery ve Azure ExpressRoute kullanarak Azure Vm'leri için olağanüstü durum kurtarma ayarlama işlemi açıklanmaktadır
services: site-recovery
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: mayg
ms.openlocfilehash: 90388d570d027aea3c897f7306a1714fd7e847b3
ms.sourcegitcommit: fec96500757e55e7716892ddff9a187f61ae81f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59618108"
---
# <a name="integrate-azure-expressroute-with-disaster-recovery-for-azure-vms"></a>Azure ExpressRoute, Azure Vm'leri için olağanüstü durum kurtarma ile tümleştirin


Bu makale, Azure ExpressRoute ile tümleştirmeyi açıklamaktadır [Azure Site Recovery](site-recovery-overview.md), Azure Vm'leri için ikincil bir Azure bölgesine olağanüstü durum kurtarmayı ayarlayın.

Site Recovery, Azure sanal makine verilerini Azure'a çoğaltma yaparak Azure Vm'leri olağanüstü durum kurtarma sağlar.

- Azure Vm'leri kullanıyorsanız [Azure yönetilen diskler](../virtual-machines/windows/managed-disks-overview.md), VM veri, ikincil bölgede çoğaltılan bir yönetilen disk için çoğaltılır.
- Azure Vm'leri yönetilen diskleri kullanmıyorsanız, VM verileri bir Azure depolama hesabına çoğaltılır.
- Çoğaltma uç noktaları herkese açık, ancak Azure Vm'leri için çoğaltma trafiğini internet çapraz değil.

ExpressRoute, bağlantı sağlayıcı tarafından kolaylaştırılan özel bağlantı üzerinden, şirket içi ağlarınızı Microsoft Azure bulutuna genişletmenizi sağlar. Yapılandırılmış ExpressRoute varsa, Site Recovery ile şu şekilde tümleşir:

- **Azure bölgeleri arasında çoğaltma gerçekleştiği sırada**: Çoğaltma trafiği Azure VM'LERİNDE olağanüstü durum kurtarma için yalnızca Azure içinde geçerlidir ve ExpressRoute gerekli değil veya çoğaltma için kullanılan. Ancak, bir şirket içi siteden Azure birincil sitede Azure vm'lerine bağlanıyorsanız, birkaç sorun ne zaman, olağanüstü durum kurtarma işlemini söz konusu Azure Vm'leri için ayarladığınız dikkat etmeniz vardır.
- **Azure bölgeleri arasında yük devretme**: Kesintiler meydana geldiğinde Azure Vm'leri için ikincil Azure bölgesine birincil başarısız. İkincil bir bölgeye sonra Yük devretme, bir dizi ExpressRoute kullanarak ikincil bölgedeki Azure Vm'lerini erişmek için gereken adımlar vardır.


## <a name="before-you-begin"></a>Başlamadan önce

Başlamadan önce aşağıdaki kavramları anladığınızdan emin olun:

- ExpressRoute [devreler](../expressroute/expressroute-circuit-peerings.md)
- ExpressRoute [Yönlendirme etki alanları](../expressroute/expressroute-circuit-peerings.md#routingdomains)
- ExpressRoute [konumları](../expressroute/expressroute-locations.md).
- Azure VM [çoğaltma mimarisi](azure-to-azure-architecture.md)
- Nasıl yapılır [çoğaltmayı ayarlama](azure-to-azure-tutorial-enable-replication.md) Azure Vm'leri için.
- Nasıl yapılır [yük devretme](azure-to-azure-tutorial-failover-failback.md) Azure Vm'leri.


## <a name="general-recommendations"></a>Genel öneriler

En iyi uygulama ve olağanüstü durum kurtarma için verimli kurtarma zamanı hedeflerine (Rto'lar) sağlamak için ExpressRoute ile tümleştirmek için Site RECOVERY'yi ayarladığınızda şunları öneririz:

- İkincil bir bölgeye yük devretmeden önce ağ bileşenleri sağlayın:
    - Azure Vm'leri için çoğaltmayı etkinleştirdiğinizde Site Recovery, kaynak ağ ayarları temel alarak Azure bölgesini hedef ağ geçitleri ağlar ve alt ağları gibi ağ kaynakları otomatik olarak dağıtabilirsiniz.
    - Site Recovery, sanal ağ geçitleri gibi ağ kaynakları otomatik olarak ayarlayamazsınız.
    - Yük devretmeden önce bu ek ağ kaynakları sağlama öneririz. Küçük bir kapalı kalma süresi, bu dağıtımla ilişkilendirilen ve sizin için dağıtım planlaması sırasında hesabı yaramadı genel kurtarma süresini etkileyebilir.
- Normal bir olağanüstü durum kurtarma tatbikatı çalıştırma:
    - Tatbikat, çoğaltma stratejinizi veri kaybı veya kesinti süresi olmadan doğrular ve üretim ortamınızı etkilemez. Bu durum, RTO olumsuz yönde etkileyebilir, geçen dakikada yapılandırma sorunları önlemeye yardımcı olur.
    - Yük devretme testi için ayrıntıya çalıştırdığınızda, çoğaltmayı etkinleştirdiğinizde ayarlanmış varsayılan ağ yerine ayrı bir Azure VM ağını kullanmanızı öneririz.
- Tek bir ExpressRoute bağlantı hattı varsa farklı IP adresi alanları kullanın.
    - Hedef sanal ağ için farklı bir IP adresi alanı kullanmanızı öneririz. Bölgesel kesintiler sırasında bağlantı kurarken sorunları önler.
    - Ayrı adres alanı kullanamıyorsanız, IP adresleri farklı olan ayrı bir test ağı olağanüstü durum kurtarma tatbikatı yük devretme testi çalıştırmak emin olun. İki sanal ağ, çakışan IP adres alanı aynı ExpressRoute bağlantı hattına ile bağlantı kurulamıyor.

## <a name="replicate-azure-vms-when-using-expressroute"></a>Expressroute'u kullanırken Azure Vm'lerini çoğaltma


Bir birincil sitede Azure Vm'leri için çoğaltmayı ayarlamak istediğiniz ve şirket içi sitenizden bu Vm'lere ExpressRoute üzerinden bağlantı kurduğunuz, işte için yapmanız gerekenler:

1. [Çoğaltmayı etkinleştirme](azure-to-azure-tutorial-enable-replication.md) her bir Azure VM için.
2. Site Recovery, isteğe bağlı olarak izin ağ ayarlayın:
    - Yapılandırma ve çoğaltmayı etkinleştirin, Site Recovery hedef ağlar, alt ağları ve ağ geçidi alt ağları eşleşen kaynak bölgedeki Azure bölgesinin, ayarlar. Site kurtarma Ayrıca kaynak ve hedef sanal ağlar arasında eşler.
    - Site Recovery'nin otomatik olarak bunu istemiyorsanız, çoğaltmayı etkinleştirmeden önce hedef tarafı ağ kaynakları oluşturma.
3. Diğer ağ öğeleriyle oluşturun:
    - Site kurtarma ikincil bölgede rota tabloları, VNet ağ geçitleri, sanal ağ geçidi bağlantılarınızı, VNet eşlemesi, veya diğer ağ kaynakları ve bağlantıları oluşturmaz.
    - Bu ek öğeler ikincil bölgede birincil bölgeden bir yük devretme çalıştırmadan önce her zaman oluşturmak için ihtiyacınız.
    - Kullanabileceğiniz [kurtarma planları](site-recovery-create-recovery-plans.md) ve Otomasyon betikleri oluşturmak ve bunlara bağlanmak için ağ kaynakları.
1. Dağıtılan ağ trafiği akışını denetlemek için bir ağ sanal Gereci (NVA) varsa, dikkat edin:
    - Azure VM çoğaltması için Azure'nın varsayılan sistem yolunu 0.0.0.0/0 ' dir.
    - Genellikle, NVA dağıtımları da NVA üzerinden akmasını giden Internet trafiğini zorlar varsayılan yolun (0.0.0.0/0) tanımlayın. Diğer bir özel yol yapılandırması bulunamadığında varsayılan yol kullanılır.
    - Bu durumda, tüm çoğaltma trafiği NVA üzerinden geçerse NVA aşırı yüklenmiş olabilir.
    - Aynı sınırlama, varsayılan yollar kullanırken şirket içi dağıtımlar için tüm Azure VM trafik yönlendirme için de geçerlidir.
    - Bu senaryoda olmasını öneririz, [bir ağ hizmet uç noktası oluşturma](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage) sanal ağınızda Microsoft.Storage hizmeti için çoğaltma trafiğini Azure sınır bırakmamasını böylece.

## <a name="replication-example"></a>Çoğaltma örneği

Kuruluş dağıtımları genellikle iş yükleri merkezi bağlantı hub için İnternet'e ve şirket içi sitelere dış bağlantısı ile birden fazla Azure Vnet arasında bölme vardır. Bir hub ve bağlı bileşen topolojisi genellikle ExpressRoute ile birlikte kullanılır.

![Şirket içi-Azure'a ExpressRoute ile yük devretmeden önce](./media/azure-vm-disaster-recovery-with-expressroute/site-recovery-with-expressroute-before-failover.png)

- **Bölge**. Uygulamaları Azure Doğu Asya bölgesinde dağıtılır.
- **Uç sanal ağları**. Uygulamaları iki uç sanal ağlarda dağıtılır:
    - **Kaynak vNet1**: 10.1.0.0/24.
    - **Kaynak vnet2'den**: 10.2.0.0/24.
    - Her bir uç sanal ağ bağlı **Hub vNet**.
- **Hub vNet**. Bir hub vNet yok **kaynak Hub vNet**: 10.10.10.0/24.
  - Bu hub sanal ağ geçidi davranır.
  - Bu hub'ı aracılığıyla alt ağlar arasındaki tüm iletişimler gidin.
    - **Merkez sanal ağ alt ağları**. Hub sanal ağı iki alt ağa sahip:
    - **NVA alt ağı**: 10.10.10.0/25. Bu alt ağ bir NVA (10.10.10.10) içerir.
    - **Ağ geçidi alt ağı**: 10.10.10.128/25. Bu alt ağı bir ExpressRoute ağ geçidi aracılığıyla özel bir eşleme Yönlendirme etki alanı şirket içi siteye yönlendiren bir ExpressRoute bağlantısı bağlı içerir.
- Şirket içi veri merkezi, Hong Kong iş ortağı edge'de aracılığıyla bir ExpressRoute bağlantı hattı bağlantısı vardır.
- Tüm yönlendirme (UDR) Azure yol tabloları denetlenir.
- NVA üzerinden giden tüm trafiği sanal ağlar arası veya şirket içi veri merkezine yönlendirilir.

### <a name="hub-and-spoke-peering-settings"></a>Hub ve bağlı bileşen ayarları eşleme

#### <a name="spoke-to-hub"></a>Uçtan merkeze

**Yön** | **Ayar** | **State**
--- | --- | ---
Uçtan merkeze | Sanal ağ adresi izin ver | Etkin
Uçtan merkeze | Yönlendirilen trafiğe izin ver | Etkin
Uçtan merkeze | Ağ geçidi aktarımına izin ver | Devre dışı
Uçtan merkeze | Remove-ağ geçitlerini kullan | Etkin

 ![Hub için eşleme yapılandırma uç](./media/azure-vm-disaster-recovery-with-expressroute/spoke-to-hub-peering-configuration.png)

#### <a name="hub-to-spoke"></a>Merkezden uca

**Yön** | **Ayar** | **State**
--- | --- | ---
Merkezden uca | Sanal ağ adresi izin ver | Etkin
Merkezden uca | Yönlendirilen trafiğe izin ver | Etkin
Merkezden uca | Ağ geçidi aktarımına izin ver | Etkin
Merkezden uca | Remove-ağ geçitlerini kullan | Devre dışı

 ![Hub'ı uç eşleme yapılandırması](./media/azure-vm-disaster-recovery-with-expressroute/hub-to-spoke-peering-configuration.png)

### <a name="example-steps"></a>Örnek adımlar

Bizim örneğimizde kaynak ağdaki Azure Vm'leri için çoğaltmayı etkinleştirirken aşağıdakiler:

1. [Çoğaltmayı etkinleştirme](azure-to-azure-tutorial-enable-replication.md) bir VM için.
2. Site Recovery çoğaltma sanal ağlar, alt ağları ve ağ geçidi alt ağları hedef bölgede oluşturun.
3. Site Recovery, kaynak ağların ve oluşturduğu çoğaltma hedef ağlar arasında eşleme oluşturur.
4. El ile sanal ağ geçitleri, sanal ağ geçidi bağlantısı, sanal ağ eşlemesi, veya tüm diğer ağ kaynakları veya bağlantıları oluşturun.


## <a name="fail-over-azure-vms-when-using-expressroute"></a>ExpressRoute kullanarak Azure Vm'leri üzerinde başarısız

Azure sanal makinelerini Site Recovery kullanarak Azure bölgesini hedef için yük devretme sonra erişebilirsiniz ExpressRoute kullanarak [özel eşdüzey hizmet sağlama](../expressroute/expressroute-circuit-peerings.md#privatepeering).

- Yeni bir bağlantıyla hedefi vNet için ExpressRoute bağlanmanız gerekmez. Mevcut bir ExpressRoute bağlantısı otomatik olarak aktarılmaz.
- Hedef vNet için ExpressRoute bağlantınızı kurmak ayarlama şekliyle ExpressRoute topolojinize bağlıdır.


### <a name="access-with-two-circuits"></a>İki bağlantı hatları ile erişim

#### <a name="two-circuits-with-two-peering-locations"></a>İki bağlantı hatları ile iki eşleme konumları

Bu yapılandırma yardımcı olur, ExpressRoute devreleri bölgesel bir olağanüstü durum karşı korur. Bağlantılar, birincil eşleme konumunuz kalırsa, farklı bir konumdan devam edebilirsiniz.

- Üretim ortamına bağlı devre genellikle birincil değil. İkincil bağlantı hattını genellikle bir olağanüstü durum oluşursa, artırılabilir daha düşük bant genişliğine sahip.
- Yük devretmeden sonra ikincil ExpressRoute bağlantı hattı'den hedef sanal ağdan sanal ağa bağlantı kurabilirsiniz. Alternatif olarak, genel kurtarma süresini azaltmak için ayarlayabilir ve hazır durumunda olağanüstü durum, bağlantıları olabilir.
- Hem birincil hem de eşzamanlı bağlantı ve hedef sanal ağlar ile yalnızca Yönlendirme şirket yük devretmeden sonra bağlantı ve ikincil bağlantı hattını kullandığından emin olun.
- Kaynak ve hedef sanal ağlar, yeni IP adresleri alır veya, yük devretme sonrasında aynı bağlantı noktalarını saklayabilirsiniz. Her iki durumda da, yük devretme öncesinde ikincil bağlantı kurulabilir.


#### <a name="two-circuits-with-single-peering-location"></a>Tek bir eşdüzey hizmet sağlama konumu ile iki bağlantı hatları

Bu yapılandırma birincil ise ExpressRoute bağlantı hattının hatasına karşı korur ancak tek ExpressRoute eşleme konumu değil kalırsa, her iki bağlantı hatları etkiliyor.

- Eşzamanlı bağlantı şirket içi veri merkezinden birincil bağlantı hattını ile vNEt kaynak ve hedef sanal ağ ile ikincil bağlantı hattını olabilir.
- Birincil eşzamanlı bağlantı ve hedef ile yalnızca Yönlendirme kullanır ve ikincil bağlantı hattını bağlantı yük devretmeden sonra şirket emin olun.
-   Eşleme aynı konumda devreler oluşturulduğunda her iki bağlantı hatları aynı sanal ağa bağlanamıyor.

### <a name="access-with-a-single-circuit"></a>Tek bir bağlantı hattı ile erişim

Bu yapılandırmada yalnızca bir Expressroute bağlantı hattı yoktur. Bir arıza durumunda devre yedekli bağlantı sahip olsa da, bir tek route bağlantı hattı eşleme bölgenizi kalırsa dayanıklılık sağlamaz. Şunlara dikkat edin:

- Azure Vm'leri, herhangi bir Azure bölgesine çoğaltabilirsiniz [aynı coğrafi konumda](azure-to-azure-support-matrix.md#region-support). ' % S'hedef Azure bölgeniz kaynak ile aynı konumda değilse, tek bir ExpressRoute bağlantı hattı kullanıyorsanız, ExpressRoute Premium etkinleştirmeniz gerekir. Hakkında bilgi edinin [ExpressRoute konumları](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) ve [ExpressRoute fiyatlandırması](https://azure.microsoft.com/pricing/details/expressroute/).
- Hedef bölge üzerinde aynı IP adresi alanını kullanılıyorsa, kaynak ve hedef sanal ağlar bağlantı hattına aynı anda bağlanamıyor. Bu senaryoda:    
    -  Kaynak tarafı bağlantısını kesmek ve ardından hedef tarafı bağlantısı oluşturmalıdır. Site Recovery kurtarma planının bir parçası Bu bağlantı değişiklik yazılabilir. Şunlara dikkat edin:
        - Birincil bölge erişilemiyorsa, bölgesel bir hata, bağlantıyı kesme işlemi başarısız olabilir. Bu bağlantı oluşturma için hedef bölgede etkileyebilir.
        - İki eşzamanlı bağlantı aynı adres alanına bağlanmaya çalışırsanız bağlantı hedef bölgede oluşturulur ve birincil bölgenin daha sonra kurtarır, paket düşme karşılaşabilirsiniz.
        - Bunu önlemek için birincil bağlantı hemen sonlandırır.
        - İkincil bağlantı bağlantısını kestikten sonra birincil bölgeye VM yeniden çalışma sonra birincil bağlantı yeniden, kurulur.
-   Aynı anda farklı adres alanları hedef vNet üzerinde kullanıldığında, aynı ExpressRoute bağlantı kaynak ve hedef sanal ağlara bağlanabilirsiniz.


## <a name="failover-example"></a>Yük devretme örneği

Bizim örneğimizde, aşağıdaki topoloji kullanıyoruz:

- İki farklı ExpressRoute devreleri eşleme iki farklı konumlarda.
- Özel IP adresleri Azure Vm'leri için yük devretme sonrasında korur.
- Hedef kurtarma Azure Güneydoğu Asya bölgedir.
- Bir iş ortağı uç Singapur aracılığıyla ikincil bir ExpressRoute bağlantı hattı bağlantı kurulur.

Yük devretme sonrasında aynı IP adresine sahip tek bir ExpressRoute bağlantı hattı kullanan basit bir topolojisi için [bu makaleyi gözden geçirin](site-recovery-retain-ip-azure-vm-failover.md#hybrid-resources-full-failover).

### <a name="example-steps"></a>Örnek adımlar
Bu örnekte, burada ait kurtarma otomatik hale getirmek için yapmanız gerekenler:

1. Çoğaltmayı ayarlama adımlarını izleyin.
2. [Azure Vm'leri üzerinde başarısız](azure-to-azure-tutorial-failover-failback.md), aşağıdaki ek adımları sırasında veya sonrasında ile.

    a. Azure ExpressRoute ağ geçidi, hedef bölge merkez sanal ağda oluşturun. Bu hedef hub sanal ağı ExpressRoute devresine bağlama gereklidir.

    b. Bağlantı hedef hub sanal ağdan hedef ExpressRoute bağlantı hattı oluşturun.

    c. Hedef bölgenin hub ve bağlı sanal ağlar arasında VNet eşlemesi ayarlayın. Hedef bölge eşleme özellikleri bulunan kaynak bölge ile aynı olacaktır.

    d. Merkez sanal ağdaki Udr'leri ve iki uç sanal ağları ayarlayın.

    - Özellikleri kullanarak aynı IP adreslerini hedef tarafı Udr'ler kaynak tarafında aynıdır.
    - Farklı bir hedef IP adresi ile Udr uygun şekilde değiştirilmesi gerekir.


Yukarıdaki adımları bir parçası olarak yazılabilir bir [kurtarma planı](site-recovery-create-recovery-plans.md). Uygulama bağlantı ve kurtarma zamanı gereksinimlerinize bağlı olarak yukarıdaki adımları da yük devretme başlatılmadan önce tamamlanabilir.

#### <a name="after-recovery"></a>Kurtarma işleminden sonra

Vm'leri kurtarma ve bağlantı tamamladıktan sonra Kurtarma Ortamı'nı aşağıdaki gibidir.

![Şirket içi-Azure'a yük devretme işleminden sonra ExpressRoute ile](./media/azure-vm-disaster-recovery-with-expressroute/site-recovery-with-expressroute-after-failover.png)



## <a name="next-steps"></a>Sonraki adımlar

Kullanma hakkında daha fazla bilgi edinin [kurtarma planları](site-recovery-create-recovery-plans.md) uygulama yük devretme otomatikleştirmek için.
