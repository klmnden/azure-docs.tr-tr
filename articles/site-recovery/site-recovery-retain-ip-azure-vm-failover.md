---
title: "Azure sanal makineleri başka bir Azure bölgesine üzerinden başarısız olduğunda IP adreslerini korumak | Microsoft Docs"
description: "Azure Site Recovery ile Azure için Azure yük devretme senaryoları için IP adreslerini korumak açıklar"
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2018
ms.author: manayar
ms.openlocfilehash: 28d772df384e620c7e82812adfa2bfa148401132
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="ip-address-retention-for-azure-virtual-machine-failover"></a>Azure sanal makine yük devretme için IP adresi bekletme

Azure Site Recovery Azure VM'ler için olağanüstü durum kurtarma sağlar. Üzerinden bir Azure bölgesinden diğerine başarısız olduğunda, müşteri genellikle IP yapılandırmalarını bekletme gerektirir. Site Recovery, varsayılan olarak, kaynak sanal ağ ve alt ağ yapısı bu kaynaklar hedef bölgesi oluşturulurken taklit eder. Özel statik IP adresleriyle yapılandırılmış Azure VM'ler için Site Recovery bu IP zaten bir Azure kaynağı veya çoğaltılmış bir VM tarafından engellenirse değil, aynı özel IP VM, hedefte sağlamak girişimi en iyi çaba de sağlar.

Basit uygulamalar için yukarıdaki varsayılan gereken tüm yapılandırmadır. Daha karmaşık kurumsal uygulamalar için müşteriler kendi altyapısının diğer bileşenlerle yük devretme sonrasında bağlantı sağlamak için ek ağ kaynakları sağlamak üzere gerekebilir. Bu makalede başarısız olan ağ gereksinimleri Azure Vm'leri üzerinde bir bölgesinden diğerine VM IP adreslerini korurken açıklanmaktadır.

## <a name="azure-to-azure-connectivity"></a>Azure Azure bağlantısı

İlk senaryo için biz göz önünde bulundurun **Şirket A** Azure'da çalışan kendi uygulama altyapısı vardır. İş devamlılığı ve uyumluluk nedenleriyle **Şirket A** uygulamaları korumak için Azure Site Recovery kullanmaya karar verir.

(Uygulama bağlamaları için olduğu gibi gibi) IP bekletme gereksinimi verildiğinde, Şirket A hedef bölgesi üzerinde aynı sanal ağ ve alt yapısını sahiptir. Daha fazla kurtarma süresi hedefi (RTO) azaltmak için **Şirket A** SQL Always ON, etki alanı denetleyicileri, vb. için çoğaltma düğümleri ve düğümlerin hedef bölgesi farklı bir sanal ağda yerleştirilir Bu çoğaltma kullanır. Farklı bir adres alanı için çoğaltma düğümleri kullanarak sağlayan **Şirket A** kaynak ve hedef bölgeler arasında VPN siteden siteye bağlantı oluşturmak için Aksi takdirde aynı adres alanı, mümkün olmayacaktır her iki uçta kullanılır .

Ağ mimarisi nasıl önce yük devretme göründüğünü aşağıda verilmiştir:
- Uygulama VM'ler içinde Azure adres alanı 10.1.0.0/16 sahip Azure sanal ağı kullanan Asya, barındırılır. Bu sanal ağ adlı **kaynak VNet**.
- Uygulama iş yükleri üç alt ağlar arasında – 10.1.0.0/24, 10.1.1.0/24, sırasıyla adlı 10.1.2.0/24 bölme **alt ağ 1**, **alt ağı 2**, **alt ağ 3**.
- Azure Güneydoğu Asya hedef bölgesi ve kaynak adres alanı ve alt ağ yapılandırması taklit eden bir kurtarma sanal ağ vardır. Bu sanal ağ adlı **kurtarma VNet**.
- Çoğaltma düğümleri her zaman açık, etki alanı denetleyicisi, vb. için gerekenler gibi bir sanal ağ adres 20.1.0.0/24 ile adres alanı 20.1.0.0/16 alt 4 içinde ile yerleştirilir. Sanal ağ adlı **Azure VNet** ve Azure Güneydoğu Asya üzerinde.
- **Kaynak VNet** ve **Azure VNet** VPN siteden siteye bağlantı bağlanır.
- **Kurtarma VNet** diğer sanal ağ ile bağlı değil.
- **Şirket A** çoğaltılan öğeler için hedef IP adresi atar/doğrular. Bu örnekte, hedef IP her VM için kaynak IP ile aynıdır.

![Yük devretme önce Azure Azure bağlantısını](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-before-failover.png)

### <a name="full-region-failover"></a>Tam bölge yük devretme

Bölgesel bir kesinti durumunda **Şirket A** hızlı ve kolay bir şekilde Azure Site Recovery'nin güçlü kullanarak, tüm dağıtım kurtarabilirsiniz [kurtarma planlarına](site-recovery-create-recovery-plans.md). Yük devretme önce her bir VM için hedef IP adresi zaten ayarlanmış **Şirket A** yük devretme düzenlemek ve kurtarma VNet ve Azure Vnet arasında bağlantı kurma gösterildiği gibi otomatikleştirmek diyagramı aşağıda.

![Azure Azure bağlantısı tam bölge yük devretme](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-full-region-failover.png)

Uygulama gereksinimlerine bağlı olarak, hedef bölge iki Vnet arasında bağlantılar olabilir kurulan önce sırasında (bir ara adım) olarak veya yük devretme sonrasında. Kullanım [kurtarma planlarına](site-recovery-create-recovery-plans.md) komut dosyalarını ekleyin ve yük devretme sırası tanımlamak için.

Şirket A kurtarma VNet ve Azure VNet arasında bağlantı kurmak için VNet eşlemesi veya siteden siteye VPN kullanma seçeneğiniz de vardır. Sanal ağ eşleme, bir VPN gateway kullanmadığından farklı kısıtlamaları vardır. Ayrıca, [sanal ağ eşleme fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-network), [Sanal Ağlar Arası VPN Gateway fiyatlandırmasından](https://azure.microsoft.com/pricing/details/vpn-gateway) farklı olarak hesaplanır. Yük devretme işlemleri için kaynak bağlantısı, ağ değişikliklerine dışında doğan öngörülemeyen olaylar en aza indirmek için bağlantı türü de dahil olmak üzere taklit genellikle tavsiye edilir.

### <a name="isolated-application-failover"></a>Yalıtılmış uygulama yük devretme

Belirli koşullar altında kullanıcılar uygulama altyapılarını yük devretme bölümlerine gerekebilir. Bu tür bir örnek, belirli bir uygulama veya ayrılmış bir alt ağ içinde yerleştirilebilir katmanı üzerinden başarısız oluyor. Bir alt ağ yük devretme IP bekletme ile mümkün olsa da, önemli ölçüde bağlantı tutarsızlıklar arttıkça, çoğu durumlar için önerilmez. Ayrıca, aynı Azure sanal ağı içindeki diğer alt ağlara alt ağ bağlantısını kaybedersiniz.

Alt düzey uygulama yük devretme gereksinimleri için hesap için daha iyi (bağlantı kaynak sanal ağ üzerindeki diğer alt ağlara gerekiyorsa) farklı bir hedef IP adresleri için yük devretme kullanın veya her iki uygulamada kendi özel sanal yalıtmak için yoludur kaynak üzerinde ağ. İkinci yaklaşımda kaynağında arası ağ bağlantısını kurmak ve aynı üzerinden hedef bölgesine başarısız olduğunda öykünmek.

Tek tek uygulamalar dayanıklılık için mimari için uygulamanın kendi özel sanal ağ içinde barındırmak ve gerektiği gibi bu sanal ağlar arasında bağlantı kurmak için önerilir. Bu, yalıtılmış uygulama yük devretme için orijinal özel IP adresleri korurken sağlar.

Pre-yük devretme yapılandırması sonra şu şekilde görünür:
- Uygulama VM'ler içinde Azure ilk uygulamaya ilişkin adres alanı 10.1.0.0/16 ve ikinci uygulamanın 15.1.0.0/16 sahip Azure sanal ağı kullanan Asya, barındırılır. Sanal ağlar adlı **kaynak VNet1** ve **kaynak vnet2'yi** birinci ve ikinci uygulama için sırasıyla.
- Her sanal ağ, daha fazla iki alt ağa her ayrılır.
- Azure Güneydoğu Asya hedef bölgesi ve kurtarma sanal ağlar kurtarma VNet1 ve kurtarma vnet2'yi vardır.
- Çoğaltma düğümleri her zaman açık için etki alanı denetleyicisi, vb. gerekenler gibi bir sanal ağ adres alanı 20.1.0.0/16 içinde ile yerleştirilir **alt 4** adresi 20.1.0.0/24 ile. Sanal ağ Azure VNet adı verilir ve Azure Güneydoğu Asya üzerinde değil.
- **Kaynak VNet1** ve **Azure VNet** VPN siteden siteye bağlantı bağlanır. Benzer şekilde, **kaynak vnet2'yi** ve **Azure VNet** ayrıca VPN siteden siteye bağlantı bağlanır.
- **Kaynak VNet1** ve **kaynak vnet2'yi** S2S VPN üzerinden bu örnekte de bağlanır. İki sanal ağlar aynı bölgede olduğundan, VNet eşlemesi de S2S VPN yerine kullanılabilir.
- **Kurtarma VNet1** ve **kurtarma vnet2'yi** diğer sanal ağ ile bağlı değil.
- Kurtarma süresi hedefi (RTO) azaltmak için VPN ağ geçitleri yapılandırılmış **kurtarma VNet1** ve **kurtarma vnet2'yi** yük devretme önce.

![Yük devretmeden önce azure Azure bağlantısı yalıtılmış uygulama](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-isolated-application-before-failover.png)

(Kaynak VNet2 önce adprep.exe'de Bu örnekte) yalnızca bir uygulama etkiler bir olağanüstü durum durumunda Şirket A etkilenen uygulamanın aşağıdaki şekilde kurtarabilirsiniz:
- VPN bağlantıları arasında **kaynak VNet1** ve **kaynak vnet2'yi**, arasındaki **kaynak vnet2'yi** ve **Azure VNet** kesilir.
- VPN bağlantıları arasında kurulan **kaynak VNet1** ve **kurtarma vnet2'yi**, arasındaki **kurtarma vnet2'yi** ve **Azure VNet**.
- Vm'lerden **kaynak vnet2'yi** için yük devredildi **kurtarma vnet2'yi**.

![Yük devretme sonrasında azure Azure bağlantısı yalıtılmış uygulama](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-isolated-application-after-failover.png)

Daha fazla uygulama içerir ve ağ bağlantıları için örnek yalıtılmış yük devretme genişletilebilir. Kaynak sunucudan hedef devretmek mümkün olduğunca bir gibi benzeri bağlantı modeli izlemeniz önerilir.

### <a name="further-considerations"></a>Dikkat edilecek diğer konular

VPN ağ geçitleri, genel IP adresleri ve ağ geçidi atlama bağlantıları kurmak için kullanır. Genel IP kullanın ve/veya ek atlama önlemek istiyorsanız istemiyorsanız, artık genel VNet eşlemesi Azure bölgeler arasında sanal ağlar arası kullanabilirsiniz.

Bu özellik şu anda genel önizlemede ve daha bölgesini desteklemek için genişletilen — tüm ortak Internet katılımı veya herhangi bir ek atlama olmadan doğrudan VM VM bağlantıyı etkinleştirme.

Daha fazla bilgi için bkz [eşleme belgelerine](../virtual-network/virtual-network-create-peering.md#register) ve [fiyatlandırma](https://azure.microsoft.com/en-us/pricing/details/virtual-network/).

## <a name="on-premises-to-azure-connectivity"></a>Şirket içi-Azure'a bağlantısı

İkinci senaryo için biz göz önünde bulundurun **Şirket B** Azure ve şirket içi çalışan Kalan çalışan kendi uygulama altyapısının bir parçası olan. İş devamlılığı ve uyumluluk nedenleriyle **Şirket B** Azure'da çalışan kendi uygulamalarını korumak için Azure Site Recovery kullanmaya karar verir.

Ağ mimarisi nasıl önce yük devretme göründüğünü aşağıda verilmiştir:
- Uygulama VM'ler içinde Azure adres alanı 10.1.0.0/16 sahip Azure sanal ağı kullanan Asya, barındırılır. Bu sanal ağ adlı **kaynak VNet**.
- Uygulama iş yükleri üç alt ağlar arasında – 10.1.0.0/24, 10.1.1.0/24, sırasıyla adlı 10.1.2.0/24 bölme **alt ağ 1**, **alt ağı 2**, **alt ağ 3**.
- Azure Güneydoğu Asya hedef bölgesi ve kaynak adres alanı ve alt ağ yapılandırması taklit eden bir kurtarma sanal ağ vardır. Bu sanal ağ adlı **kurtarma VNet**.
- VM'ler içinde Azure Doğu Asya, şirket içi veri merkezine ExpressRoute veya siteden siteye VPN üzerinden bağlanır.
- Kurtarma süresi hedefi (RTO) azaltmak için yük devretme öncesinde kurtarma VNet içindeki Azure Güneydoğu Asya üzerindeki ağ geçitlerini Şirket B sağlar.
- **Şirket B** çoğaltılan öğeler için hedef IP adresi atar/doğrular. Bu örnekte, hedef IP her VM için kaynak IP aynıdır

![Yük devretmeden önce şirket içi-Azure'a bağlantısı](./media/site-recovery-retain-ip-azure-vm-failover/on-premises-to-azure-connectivity-before-failover.png)

### <a name="full-region-failover"></a>Tam bölge yük devretme

Bölgesel bir kesinti durumunda **Şirket B** hızlı ve kolay bir şekilde Azure Site Recovery'nin güçlü kullanarak, tüm dağıtım kurtarabilirsiniz [kurtarma planlarına](site-recovery-create-recovery-plans.md). Yük devretme önce her bir VM için hedef IP adresi zaten ayarlanmış **Şirket B** yük devretme düzenlemek ve kurtarma VNet ve şirket içi veri merkezi arasında bağlantı kurma gösterildiği gibi otomatikleştirmek diyagramı aşağıda.

Azure Güneydoğu Asya ve şirket içi veri merkezi arasında bağlantı kurmadan önce Azure Doğu Asya ve şirket içi veri merkezi arasında özgün bağlantı kesilmesi gerekir. Yönlendirme içi hedef bölgesine işaret edecek şekilde yeniden ayrıca ve Yük Devretme ağ geçitleri gönderin.

![Yük devretme sonrasında şirket içi-Azure'a bağlantısı](./media/site-recovery-retain-ip-azure-vm-failover/on-premises-to-azure-connectivity-after-failover.png)

### <a name="subnet-failover"></a>Alt ağ yük devretme

İçin açıklanan Azure Azure senaryosu aksine **Şirket A**, bir alt ağ düzeyinde yük devretme bu durumda olası değil **Şirket B**. Kaynak ve kurtarma sanal ağ adres alanı aynı ve şirket içi bağlantı özgün kaynak etkin olduğundan budur.

Uygulama dayanıklılığı elde etmek için her uygulamanın kendi ayrılmış Azure sanal ağında önce adprep.exe'de önerilir. Uygulamaları sonra Yük yalıtım modunda Devredilebilen ve yukarıda açıklandığı gibi hedef bölgesine gerekli şirket içi kaynağı bağlantıları için yönlendirilebilir.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinmek [kurtarma planlarına](site-recovery-create-recovery-plans.md).
