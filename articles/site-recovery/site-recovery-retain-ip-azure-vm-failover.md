---
title: Azure VM yük devretmesi için IP adreslerini korumak | Microsoft Docs
description: IP adresleri, Azure Site Recovery ile ikincil bir bölgeye olağanüstü durum kurtarma için Azure Vm'leri üzerinde başarısız olduğunda bekletilecek açıklar
ms.service: site-recovery
ms.date: 10/16/2018
author: mayurigupta13
ms.topic: conceptual
ms.author: mayg
ms.openlocfilehash: 86adaa21a069c168b512231ba231940bfa2ef9e8
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50213041"
---
# <a name="ip-address-retention-for-azure-vm-failover"></a>Azure VM yük devretmesi için IP adresi bekletme

Azure Site Recovery, Azure Vm'leri için olağanüstü durum kurtarma sağlar. Üzerinden bir Azure bölgesinden diğerine başarısız olduğunda, müşteriler genellikle IP yapılandırmalarına saklanmasını gerektirir. Site Recovery varsayılan olarak, kaynak sanal ağ ve alt ağ yapısı hedef bölge üzerinde bu kaynakları oluşturulurken taklit eder. Statik özel IP adresleriyle yapılandırılmış Azure Vm'leri için Site Recovery de bu IP zaten bir Azure kaynağı veya çoğaltılan bir sanal makine tarafından engellenirse değil, aynı özel IP hedef VM sağlama girişimi bir en iyi hale getirir.

Basit uygulamalar için yukarıdaki varsayılan yapılandırma gereken şey. Daha karmaşık kurumsal uygulamalar için müşteriler kendi altyapısının diğer bileşenleri ile yük devretme sonrası bağlantı sağlamak için ek ağ kaynakları sağlaması gerekebilir. Bu makalede başarısız için ağ gereksinimleri Azure Vm'leri bir bölgeden diğerine VM IP adresleri korurken açıklanmaktadır.

## <a name="azure-to-azure-connectivity"></a>Azure'dan Azure'a bağlantısı

İlk senaryo için biz göz önünde bulundurun **şirketi** Azure'da çalışan kendi uygulama altyapısı vardır. İş sürekliliği ve uyumluluk nedenleriyle **şirketi** uygulamaları korumak için Azure Site Recovery kullanmaya karar verir.

(Uygulama bağlamaları olduğu gibi) IP bekletme gereksinimi göz önünde bulundurulduğunda, şirket bir hedef bölge üzerinde aynı sanal ağ ve alt ağ yapısı vardır. Kurtarma süresi hedefi (RTO) daha da azaltmak için **şirketi** SQL Always ON, etki alanı denetleyicileri vb. için yineleme düğümlerinin düğümleri hedef bölge farklı bir sanal ağda yerleştirildiğinde Bu çoğaltma yararlanır. Yineleme düğümleri için farklı adres alanı kullanarak **şirketi** kaynak ve hedef bölgeler arasında VPN siteden siteye bağlantı oluşturmak için Aksi takdirde aynı adres alanı, mümkün olmayacak her iki uçta kullanılır .

Ağ mimarisi önce yük devretme nasıl göründüğünü aşağıda verilmiştir:
- Uygulama Vm'leri, Azure Doğu adres alanı 10.1.0.0/16 ile bir Azure sanal ağı kullanan Asya, barındırılır. Bu sanal ağ adlı **kaynak VNet**.
- Uygulama iş yüklerini üç alt ağlar arasında – 10.1.1.0/24 10.1.2.0/24, sırasıyla adlı 10.1.3.0/24, bölme **alt ağ 1**, **alt ağı 2**, **alt 3**.
- Azure Güneydoğu Asya, hedef bölge ve kaynak adres alanı ve alt ağ yapılandırması taklit eden bir kurtarma sanal ağı vardır. Bu sanal ağ adlı **kurtarma VNet**.
- Yineleme düğümleri her zaman açık, etki alanı denetleyicisi, vb. için gerekli olanlar gibi bir sanal ağ içindeki alt ağ 4 adresi alanı 10.2.0.0/16 adres 10.2.4.0/24 ile yerleştirilir. Sanal ağ adlı **Azure VNet** ve Azure üzerinde Güneydoğu Asya.
- **Kaynak VNet** ve **Azure VNet** VPN siteden siteye bağlantı bağlanır.
- **Kurtarma VNet** diğer sanal ağ ile bağlı değil.
- **Bir şirket** çoğaltılan öğeler için hedef IP adresi atar/doğrular. Bu örnekte, hedef IP her VM için kaynak IP ile aynıdır.

![Yük devretmeden önce Azure-Azure bağlantısı](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-before-failover2.png)

### <a name="full-region-failover"></a>Tam bir bölgeye yük devretme

Bölgesel bir kesinti durumunda **şirketi** hızlı ve kolay bir şekilde Azure Site Recovery'nin güçlü kullanarak tüm dağıtımı kurtarabilirsiniz [kurtarma planları](site-recovery-create-recovery-plans.md). Yük devretme öncesinde her VM için hedef IP adresi zaten ayarlanmış **şirketi** yük devretmelerini düzenlemek ve kurtarma sanal ağ ve Azure sanal ağı arasında bağlantı kurma gösterildiği otomatikleştirmek diyagram aşağıda.

![Azure'dan Azure'a bağlantı tam bölgeye yük devretme](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-full-region-failover2.png)

Uygulama gereksinimlerine bağlı olarak, hedef bölge üzerinde iki Vnet arasında olabilen kurulan önce sırasında (ara adım) olarak veya yük devretme işleminden sonra. Kullanım [kurtarma planları](site-recovery-create-recovery-plans.md) komut dosyalarını ekleyin ve yük devretme sırasını tanımlar.

Şirket bir kurtarma sanal ağ ve Azure sanal ağı arasında bağlantı kurmak için sanal ağ eşlemesi veya siteden siteye VPN kullanarak seçeneğiniz de vardır. Sanal ağ eşleme, bir VPN gateway kullanmadığından farklı kısıtlamaları vardır. Ayrıca, [sanal ağ eşleme fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-network), [Sanal Ağlar Arası VPN Gateway fiyatlandırmasından](https://azure.microsoft.com/pricing/details/vpn-gateway) farklı olarak hesaplanır. Yük devretme işlemleri için ağ değişikliklerini dışında kaynaklanan beklenmedik olaylar en aza indirmek için bağlantı türü de dahil olmak üzere, kaynak bağlantı taklit etmek için genellikle tavsiye edilir.

### <a name="isolated-application-failover"></a>Yalıtılmış uygulama yük devretme

Belirli koşullar altında kullanıcılar uygulama altyapılarını yük devretme bölümlerine gerekebilir. Bu tür bir örnek, belirli bir uygulama veya ayrılmış bir alt ağ içinde barındırılır katmanı üzerinden başarısız oluyor. Bir alt ağ yük devretme IP saklama süresi mümkün olsa da önemli ölçüde bağlantı tutarsızlıklar arttıkça, çoğu durum için önerilmez. Ayrıca, diğer alt ağlara aynı Azure sanal ağ içindeki alt ağ bağlantısını kaybedersiniz.

(Bağlantı kaynak sanal ağ üzerindeki diğer alt ağlara gerekli ise), yük devretme için farklı bir hedef IP adresleri kullanın veya her iki uygulamada, kendi özel sanal yalıtmak için hesap alt düzey uygulama yük devretme gereksinimleri için daha iyi bir yolu olan kaynak üzerinde ağ. İkinci yaklaşımda kaynak ağlar arası bağlantı kurmak ve aynı hedef bölgeye yük devretmeyi olduğunda öykünmek.

Dayanıklılık için tek tek uygulamalar oluşturmak için uygulamanın kendi özel sanal ağında barındırmak ve gerektiğinde bu sanal ağlar arasında bağlantı kurmak için önerilir. Bu, yalıtılmış uygulama yük devretme için orijinal özel IP adresleri korurken sağlar.

Yük devretme öncesi yapılandırmanın ardından aşağıdaki gibi görünür:
- Uygulama Vm'leri, Azure Doğu ilk uygulamaya ilişkin adres alanı 10.1.0.0/16 ve 10.2.0.0/16 ikinci uygulama ile bir Azure sanal ağı kullanan Asya, barındırılır. Sanal ağlar adlı **kaynak VNet1** ve **kaynak vnet2'den** birinci ve ikinci uygulama için sırasıyla.
- Her sanal ağ başka iki alt ağına her bölünür.
- Azure Güneydoğu Asya, hedef bölge ve kurtarma sanal ağlar kurtarma vnet1'e ve kurtarma vnet2'den vardır.
- Always On için etki alanı denetleyicisi, vb. gerekenler gibi yineleme düğümlerini sanal ağ adres alanı 10.3.0.0/16 ile yerleştirildiğinde **alt 4** adresi 10.3.4.0/24 ile. Sanal ağ, Azure sanal ağı olarak adlandırılır ve Azure Güneydoğu Asya üzerinde olduğu.
- **Kaynak VNet1** ve **Azure VNet** VPN siteden siteye bağlantı bağlanır. Benzer şekilde, **kaynak vnet2'den** ve **Azure VNet** ayrıca VPN siteden siteye bağlantı bağlanır.
- **Kaynak VNet1** ve **kaynak vnet2'den** Ayrıca bu örnekte S2S VPN üzerinden bağlı. Aynı bölgedeki iki sanal ağ olduğundan, sanal ağ eşlemesi de S2S VPN yerine kullanılabilir.
- **Kurtarma VNet1** ve **kurtarma vnet2'den** diğer sanal ağ ile bağlı değil.
- Kurtarma süresi hedefi (RTO) azaltmak için VPN ağ geçitleri üzerinde yapılandırılmış **kurtarma VNet1** ve **kurtarma vnet2'den** yük devretme öncesinde.

![Yük devretmeden önce azure-Azure bağlantısı yalıtılmış uygulama](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-isolated-application-before-failover2.png)

Tek bir uygulamada (Bu örnekte, kaynak VNet2 içinde barındırılan) etkileyen bir olağanüstü durum durum olması durumunda Şirket A etkilenen uygulama aşağıdaki gibi kurtarabilirsiniz:
- VPN bağlantıları arasında **kaynak VNet1** ve **kaynak vnet2'den**ve arasında **kaynak vnet2'den** ve **Azure VNet** kesilir.
- VPN bağlantıları arasında belirlenir **kaynak VNet1** ve **kurtarma vnet2'den**ve arasında **kurtarma vnet2'den** ve **Azure VNet**.
- Vm'lerden **kaynak vnet2'den** için yük devredildi **kurtarma vnet2'den**.

![Yük devretmeden sonra azure'dan Azure'a bağlantı yalıtılmış uygulama](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-isolated-application-after-failover2.png)

Daha fazla uygulama içerir ve ağ bağlantıları için örnek yalıtılmış yük devretme genişletilebilir. Kaynaktan hedefe devretmek, mümkün olduğunca uzak onaylamaktan bir gibi benzeri bağlantı modeli izlenmesi önerilir.

### <a name="further-considerations"></a>Dikkat edilecek diğer konular

VPN ağ geçidi genel IP adresleri ve bağlantı kurmak için ağ geçidi atlama kullanır. Genel IP kullanın ve/veya ek atlama engellemek istiyorsanız istemiyorsanız, Azure kullanabileceğiniz [sanal ağ eşlemesi](../virtual-network/virtual-network-peering-overview.md) arasında sanal ağları eşleyebilme [Azure bölgeleri desteklenen](../virtual-network/virtual-network-manage-peering.md#cross-region).

## <a name="on-premises-to-azure-connectivity"></a>Şirket içi-Azure'a bağlantısı

İkinci senaryo için biz göz önünde bulundurun **Şirket B** Azure ve şirket içinde çalışan Kalan çalışan kendi uygulama altyapısının bir parçası olan. İş sürekliliği ve uyumluluk nedenleriyle **Şirket B** Azure'da çalışan uygulamaları korumak için Azure Site Recovery kullanmaya karar verir.

Ağ mimarisi önce yük devretme nasıl göründüğünü aşağıda verilmiştir:
- Uygulama Vm'leri, Azure Doğu adres alanı 10.1.0.0/16 ile bir Azure sanal ağı kullanan Asya, barındırılır. Bu sanal ağ adlı **kaynak VNet**.
- Uygulama iş yüklerini üç alt ağlar arasında – 10.1.1.0/24 10.1.2.0/24, sırasıyla adlı 10.1.3.0/24, bölme **alt ağ 1**, **alt ağı 2**, **alt 3**.
- Azure Güneydoğu Asya, hedef bölge ve kaynak adres alanı ve alt ağ yapılandırması taklit eden bir kurtarma sanal ağı vardır. Bu sanal ağ adlı **kurtarma VNet**.
- VM'ler Azure Doğu Asya, şirket içi veri merkezine ExpressRoute veya siteden siteye VPN üzerinden bağlanır.
- Kurtarma süresi hedefi (RTO) azaltmak için ağ geçitleri Azure Güneydoğu Asya, Kurtarma VNet üzerinde yük devretme öncesinde Şirket B sağlar.
- **Şirket B** çoğaltılan öğeler için hedef IP adresi atar/doğrular. Bu örnekte her VM için kaynak IP olarak aynı hedef IP

![Yük devretmeden önce şirket içi-Azure'a bağlantısı](./media/site-recovery-retain-ip-azure-vm-failover/on-premises-to-azure-connectivity-before-failover2.png)

### <a name="full-region-failover"></a>Tam bir bölgeye yük devretme

Bölgesel bir kesinti durumunda **Şirket B** hızlı ve kolay bir şekilde Azure Site Recovery'nin güçlü kullanarak tüm dağıtımı kurtarabilirsiniz [kurtarma planları](site-recovery-create-recovery-plans.md). Yük devretme öncesinde her VM için hedef IP adresi zaten ayarlanmış **Şirket B** yük devretmelerini düzenlemek ve kurtarma VNet ve şirket içi veri merkezi arasında bağlantı kurma gösterildiği otomatikleştirmek diyagram aşağıda.

Azure Güneydoğu Asya ve şirket içi veri merkezi arasında bağlantı kurmadan önce Azure Doğu Asya ve şirket içi veri merkezi arasında özgün bağlantı kesilmesi gerekir. Yönlendirme şirket içinde hedef bölgeye işaret edecek şekilde yeniden ayrıca ve Yük Devretme ağ geçitleri yayınlayın.

![Şirket içi-Azure'a yük devretme sonrasında bağlantısı](./media/site-recovery-retain-ip-azure-vm-failover/on-premises-to-azure-connectivity-after-failover2.png)

### <a name="subnet-failover"></a>Alt ağ yük devretme

İçin Azure'dan Azure'a senaryoyu aksine **şirketi**, bir alt ağ düzeyinde yük devretme bu durumda için mümkün değildir **Şirket B**. Kaynak ve kurtarma sanal ağ adres alanını aynıdır ve şirket içi bağlantı özgün kaynağına etkin olduğundan budur.

Uygulama dayanıklılığı sağlamak için her uygulamanın kendi Azure ayrılmış sanal ağda barındırılır önerilir. Uygulamaları ardından yerine yalıtılmış olarak çalışılabilir ve gerekli şirket içi kaynak bağlantıları için hedef bölgede yukarıda açıklandığı gibi yönlendirilebilir.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [kurtarma planları](site-recovery-create-recovery-plans.md).
