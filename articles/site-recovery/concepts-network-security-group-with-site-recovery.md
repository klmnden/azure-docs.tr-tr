---
title: Ağ güvenlik grupları, Azure Site Recovery ile | Microsoft Docs
description: Ağ güvenlik grupları Azure Site Recovery ile olağanüstü durum kurtarma ve geçiş için kullanmayı açıklar
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: mayg
ms.openlocfilehash: 0c06283080a4ee51f863714e4c515672299b420d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60773028"
---
# <a name="network-security-groups-with-azure-site-recovery"></a>Azure Site Recovery ile ağ güvenlik grupları

Ağ güvenlik grupları, ağ trafiğini bir sanal ağ içindeki kaynaklarla sınırlama için kullanılır. A [ağ güvenlik grubu (NSG)](../virtual-network/security-overview.md#network-security-groups) izin veren veya kaynak veya hedef IP adresi, bağlantı noktası ve protokole göre gelen veya giden ağ trafiği reddeden güvenlik kurallarının bir listesini içerir.

Resource Manager dağıtım modelinde Nsg'leri alt ağları veya tek tek Ağ arabirimleriyle ilişkilendirilebilir. Bir NSG bir alt ağ ile ilişkilendirildiğinde kurallar alt ağa bağlı tüm kaynaklar için geçerli olur. Trafik de NSG'yi ağ arabirimine zaten ilişkili bir NSG'si sahip bir alt ağ ile ilişkilendirilmesi yoluyla daha da kısıtlanabilir.

Bu makalede, ağ güvenlik grupları Azure Site Recovery ile nasıl kullanabileceğiniz açıklanır.

## <a name="using-network-security-groups"></a>Ağ güvenlik gruplarını kullanma

Tek bir alt ağı olabilir sıfır veya bir ilişkili NSG. Bir ağ arabirimine bulundurabilirsiniz sıfır veya bir ilişkili NSG. Bu nedenle, bir NSG bir alt ağ için önce ve sonra başka bir NSG sanal makinenin ağ arabirimine ilişkilendirerek bir sanal makine için çift trafiği kısıtlama etkin olabilir. NSG kuralları uygulama bu durumda trafik yönü ve uygulanan güvenlik kuralları önceliğini bağlıdır.

Bir sanal makine ile basit bir örnek gibi göz önünde bulundurun:
-   Sanal makine içine yerleştirilir **Contoso alt**.
-   **Contoso alt** ilişkili olduğu **alt ağın NSG**.
-   VM ağ arabirimi ile ilişkili ayrıca **VM NSG**.

![Site Recovery ile NSG](./media/concepts-network-security-group-with-site-recovery/site-recovery-with-network-security-group.png)

Bu örnekte, gelen trafik için alt ağın NSG önce değerlendirilir. Alt ağ NSG'de izin verdiği trafik sonrasında VM NSG tarafından değerlendirilir. Geriye doğru VM ilk değerlendirilen NSG ile giden trafik için geçerlidir. VM NSG'de izin verdiği trafik sonrasında alt ağ NSG tarafından değerlendirilir.

Bu, ayrıntılı bir güvenlik kuralı uygulama için sağlar. Örneğin, gelen internet erişimi birkaç uygulama altında yer alan VM'ler (örneğin, ön uç Vm'leri) bir alt ağ için izin ver, ancak diğer vm'lere (örneğin, veritabanı ve diğer arka uç VM) gelen internet erişimi kısıtlamak isteyebilirsiniz. Bu durumda alt ağ internet trafiğe izin veren NSG üzerinde daha esnek bir kuralı vardır ve VM NSG erişimi vermeyerek belirli VM erişimini kısıtlamak. Aynı giden trafik için uygulanabilir.

Bu tür NSG yapılandırmalarını ayarlama, doğru öncelikleri için uygulandığından emin olun [güvenlik kuralları](../virtual-network/security-overview.md#security-rules). Kurallar öncelik sırasına göre işleme alınır ve düşük rakamlı kurallar daha yüksek önceliğe sahip olduğundan yüksek rakamlı kurallardan önce uygulanır. Trafik bir kuralla eşleştiğinde işlem durur. Bunun sonucunda yüksek önceliğe sahip olan kurallarla aynı özniteliklere sahip olan önceliği daha düşük olan (yüksek rakamlı) kurallar işleme alınmaz.

Her zaman ağ güvenlik gruplarının hem bir ağ arabirimine hem de alt ağa uygulandığının farkında olmayabilirsiniz. Görüntüleyerek bir ağ arabirimine uygulanmış olan toplu kuralları doğrulayabilirsiniz [geçerli güvenlik kuralları](../virtual-network/virtual-network-network-interface.md#view-effective-security-rules) bir ağ arabirimi. Ayrıca [IP akışı doğrulama](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md) özelliği [Azure Ağ İzleyicisi](../network-watcher/network-watcher-monitoring-overview.md) iletişim bir ağ arabirimine gelen veya izin verilip verilmeyeceğini belirlemek için. Bu araç iletişime izin verilip verilmediğini ve trafiğe izin veren veya onu reddeden ağ güvenlik kuralının hangisi olduğunu belirler.

## <a name="on-premises-to-azure-replication-with-nsg"></a>Şirket içi NSG ile Azure'a çoğaltma

Azure Site kurtarma sağlayan olağanüstü durum kurtarma ve azure'a geçiş için şirket içi [Hyper-V sanal makinelerini](hyper-v-azure-architecture.md), [VMware sanal makinelerini](vmware-azure-architecture.md), ve [fiziksel sunucuları](physical-azure-architecture.md). Tüm şirket içi Azure senaryoları için çoğaltma verileri gönderilen ve bir Azure depolama hesabında depolanır. Çoğaltma sırasında herhangi bir sanal makine ücreti ödeme yapmayın. Azure'a yük devretme çalıştırdığınızda, Site Recovery, Azure Iaas sanal makineleri otomatik olarak oluşturur.

Azure'a yük devretmenin ardından VM'ler oluşturulduktan sonra Nsg'ler sanal ağ ve VM ağ trafiği sınırlamak için kullanılabilir. Site Recovery, yük devretme işleminin bir parçası olarak Nsg'ler oluşturmaz. Yük devretmeyi başlatmadan önce gerekli Azure Nsg'ler oluşturmanızı öneririz. Daha sonra Nsg'leri VM'lerin yükünü otomatik olarak yük devretme sırasında başarısız Otomasyon betikleri ile Site Recovery'nin güçlü kullanarak ilişkilendirebilirsiniz [kurtarma planları](site-recovery-create-recovery-plans.md).

Örneğin, yük devretme sonrasında VM yapılandırmasına benzer [Örnek senaryo](concepts-network-security-group-with-site-recovery.md#using-network-security-groups) yukarıda ayrıntılı:
-   Oluşturabileceğiniz **Contoso VNet** ve **Contoso alt** hedef Azure bölgesi planlama DR bir parçası olarak.
-   Ayrıca oluşturabilir ve her ikisi de yapılandırma **alt ağın NSG** yanı **VM NSG** bölümü aynı DR planlama olarak.
-   **Alt ağ NSG'SİNDE** ardından hemen ile ilişkilendirilebilir **Contoso alt**gibi hem NSG hem de alt ağ zaten kullanılabilir.
-   **VM NSG** kurtarma planlarını kullanarak yük devretme sırasında VM ile ilişkili olabilir.

Nsg'ler oluşturulup yapılandırıldıktan sonra çalışmasını öneririz. bir [yük devretme testi](site-recovery-test-failover-to-azure.md) doğrulamak için komut dosyası NSG ilişkileri ve yük devretme sonrasında VM bağlantısı.

## <a name="azure-to-azure-replication-with-nsg"></a>NSG ile azure'dan Azure'a çoğaltma

Azure Site kurtarma sağlayan olağanüstü durum kurtarma [Azure sanal makineleri](azure-to-azure-architecture.md). Azure Vm'leri için çoğaltmayı etkinleştirdiğinizde Site Recovery çoğaltma üzerinde hedef bölge (alt ağlar ve ağ geçidi alt ağları dahil) sanal ağlar oluşturma ve kaynak arasında gerekli eşlemeleri oluşturabilir ve hedef sanal ağları. Ayrıca, önceden hedef tarafı ağlar ve alt ağlar oluşturmak ve aynı çoğaltmayı etkinleştirirken kullanabilirsiniz. Site Recovery oluşturmaz herhangi bir VM üzerinde hedef Azure bölgesi öncesinde [yük devretme](azure-to-azure-tutorial-failover-failback.md).

Azure VM çoğaltma için NSG kurallarını Azure bölgesi kaynağı üzerinde izin verdiğinden emin olun [giden bağlantı](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges) çoğaltma trafiği için. Ayrıca test ve gerekli kurallar bu doğrulayabilirsiniz [örnek NSG yapılandırma](azure-to-azure-about-networking.md#example-nsg-configuration).

Site Recovery oluşturma veya yük devretme işleminin bir parçası olarak Nsg'ler çoğaltmak desteklemez. Yük devretmeyi başlatmadan önce hedef Azure bölgeniz gerekli Nsg oluşturmanızı öneririz. Daha sonra Nsg'leri VM'lerin yükünü otomatik olarak yük devretme sırasında başarısız Otomasyon betikleri ile Site Recovery'nin güçlü kullanarak ilişkilendirebilirsiniz [kurtarma planları](site-recovery-create-recovery-plans.md).

Dikkate [Örnek senaryo](concepts-network-security-group-with-site-recovery.md#using-network-security-groups) daha önce açıklanan:
-   Site Recovery kopyaları oluşturabilir **Contoso VNet** ve **Contoso alt** hedef VM için çoğaltma etkinleştirildiğinde Azure bölgesi.
-   İstenen kopyalarını oluşturabilirsiniz **alt ağın NSG** ve **VM NSG** (adlandırılmış, örneğin, **hedef alt ağın NSG** ve **hedef VM NSG**, sırasıyla) için hedef bölgede yapılması gereken herhangi bir ek kuralın verme hedef Azure bölgesi.
-   **Hedef alt ağın NSG** ardından olabilir hem NSG hem de alt ağ zaten kullanılabildiğinden azureresourcemanager hemen hedef bölge alt ağ ile ilişkili.
-   **Hedef VM NSG** kurtarma planlarını kullanarak yük devretme sırasında VM ile ilişkili olabilir.

Nsg'ler oluşturulup yapılandırıldıktan sonra çalışmasını öneririz. bir [yük devretme testi](azure-to-azure-tutorial-dr-drill.md) doğrulamak için komut dosyası NSG ilişkileri ve yük devretme sonrasında VM bağlantısı.

## <a name="next-steps"></a>Sonraki adımlar
-   Daha fazla bilgi edinin [ağ güvenlik grupları](../virtual-network/security-overview.md#network-security-groups).
-   NSG hakkında daha fazla bilgi [güvenlik kuralları](../virtual-network/security-overview.md#security-rules).
-   Daha fazla bilgi edinin [geçerli güvenlik kuralları](../virtual-network/diagnose-network-traffic-filter-problem.md) için bir NSG.
-   Daha fazla bilgi edinin [kurtarma planları](site-recovery-create-recovery-plans.md) uygulama yük devretme otomatikleştirmek için.
