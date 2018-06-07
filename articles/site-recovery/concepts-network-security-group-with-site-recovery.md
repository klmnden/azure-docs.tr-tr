---
title: Ağ güvenlik grupları Azure Site Recovery ile | Microsoft Docs
description: Ağ güvenlik grupları Azure Site Recovery ile olağanüstü durum kurtarma ve geçiş için nasıl kullanılacağını açıklar
services: site-recovery
documentationcenter: ''
author: mayanknayar
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 05/21/2018
ms.author: manayar
ms.openlocfilehash: 2b35578e2dc49cd47689f62fc47c5341ea8767a4
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655826"
---
# <a name="network-security-groups-with-azure-site-recovery"></a>Azure Site Recovery ile ağ güvenlik grupları

Ağ güvenlik grupları, ağ trafiği sanal ağınızdaki kaynaklara sınırlamak için kullanılır. A [ağ güvenlik grubu (NSG)](../virtual-network/security-overview.md#network-security-groups) izin veren veya reddeden kaynak veya hedef IP adresi, bağlantı noktası ve protokol göre gelen veya giden ağ trafiği güvenlik kuralları listesini içerir.

Resource Manager dağıtım modeli altında Nsg'ler alt ağları veya tek tek ağ arabirimleri için ilişkili olabilir. Bir NSG bir alt ağ ile ilişkilendirildiğinde kurallar alt ağa bağlı tüm kaynaklar için geçerli olur. Trafik daha da bir NSG tek tek ağ arabirimlerine zaten ilişkilendirilmiş bir NSG bir alt ağ içindeki ilişkilendirerek kısıtlanabilir.

Bu makalede, Azure Site Recovery ile ağ güvenlik grupları nasıl kullanabileceğinizi açıklar.

## <a name="using-network-security-groups"></a>Ağ güvenlik gruplarını kullanma

Tek bir alt ağa sahip sıfır veya bir ilişkili NSG. Bir ağ arabirimine de sağlayabilirsiniz sıfır veya bir ilişkili NSG. Bu nedenle, bir NSG bir alt ağ için önce ve sonra başka bir NSG VM ağ arabirimine ilişkilendirerek bir sanal makine için çift trafiği kısıtlama etkili olabilir. NSG kuralları uygulama bu durumda trafiğin yönü ve uygulanan güvenlik kuralların önceliğini bağlıdır.

Bir sanal makine ile basit bir örnek gibi göz önünde bulundurun:
-   Sanal makine içinde yerleştirilir **Contoso alt**.
-   **Contoso alt** ile ilişkili **alt NSG**.
-   VM ağ arabirimine ayrıca ilişkilidir **VM NSG**.

![Site Recovery ile NSG](./media/concepts-network-security-group-with-site-recovery/site-recovery-with-network-security-group.png)

Bu örnekte, gelen trafik için alt ağ NSG önce değerlendirilir. Alt ağ NSG izin verilen trafiği sonra VM NSG tarafından değerlendirilir. Ters VM ilk değerlendirilen NSG ile giden trafik için geçerlidir. VM NSG izin verilen trafiği ardından alt ağ NSG tarafından değerlendirilir.

Bu ayrıntılı güvenlik kuralı uygulaması için sağlar. Örneğin, birkaç uygulama altında yer alan VM'ler (örneğin, ön uç VM'ler) bir alt ağ için gelen internet erişimi ver, ancak diğer VM'ler (örneğin, veritabanı ve diğer arka uç VM'ler) gelen internet erişimi sınırlamak isteyebilirsiniz. Bu durumda alt ağ Internet trafiğe izin NSG üzerinde daha esnek bir kuralı varsa ve VM NSG erişimi vermeyerek belirli VM'ler için erişimi kısıtlamak. Aynı giden trafik için uygulanabilir.

Bu tür NSG yapılandırmalarını ayarlama, doğru öncelikleri için uygulandığından emin olmak [güvenlik kuralları](../virtual-network/security-overview.md#security-rules). Kurallar öncelik sırasına göre işleme alınır ve düşük rakamlı kurallar daha yüksek önceliğe sahip olduğundan yüksek rakamlı kurallardan önce uygulanır. Trafik bir kuralla eşleştiğinde işlem durur. Bunun sonucunda yüksek önceliğe sahip olan kurallarla aynı özniteliklere sahip olan önceliği daha düşük olan (yüksek rakamlı) kurallar işleme alınmaz.

Her zaman ağ güvenlik gruplarının hem bir ağ arabirimine hem de alt ağa uygulandığının farkında olmayabilirsiniz. Bir ağ arabirimine görüntüleyerek uygulanan toplama kuralları doğrulayabilir [etkin güvenlik kuralları](../virtual-network/virtual-network-network-interface.md#view-effective-security-rules) bir ağ arabirimi için. Aynı zamanda [IP akış doğrulayın](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md) özelliği [Azure Ağ İzleyicisi](../network-watcher/network-watcher-monitoring-overview.md) iletişimi için veya bir ağ arabirimi izin verilip verilmediğini belirlemek için. Bu araç iletişime izin verilip verilmediğini ve trafiğe izin veren veya onu reddeden ağ güvenlik kuralının hangisi olduğunu belirler.

## <a name="on-premises-to-azure-replication-with-nsg"></a>Şirket içi NSG ile Azure çoğaltma

Azure Site kurtarma sağlayan olağanüstü durum kurtarma ve şirket içi Azure geçiş [Hyper-V sanal makineleri](hyper-v-azure-architecture.md), [VMware sanal makineleri](vmware-azure-architecture.md), ve [fiziksel sunucuları](physical-azure-architecture.md). Tüm şirket için Azure senaryoları için çoğaltma verilerinin gönderilen ve bir Azure depolama hesabında depolanır. Çoğaltma sırasında herhangi bir sanal makine ücret ödemeniz gerekmez. Azure'a yük devretme çalıştırdığınızda, Site Recovery Azure Iaas sanal makineleri otomatik olarak oluşturur.

Azure'a yük devretme sonrasında VM'ler oluşturulduktan sonra Nsg'ler sanal ağ ve sanal makineleri için ağ trafiğini sınırlandırmak için kullanılabilir. Site Recovery, yük devretme işleminin bir parçası olarak Nsg'ler oluşturmaz. Yük devretmeyi başlatmadan önce gerekli Azure NGSs oluşturmanızı öneririz. Daha sonra Nsg'leri otomatik olarak yük devretme sırasında vm'lerinde Otomasyon betikleri Site Recovery'nin ile güçlü kullanarak ilişkilendirebilirsiniz [kurtarma planlarına](site-recovery-create-recovery-plans.md).

Örneğin, yük devretme sonrasında VM yapılandırmasına benzer [Örnek senaryo](concepts-network-security-group-with-site-recovery.md#using-network-security-groups) yukarıdaki ayrıntılı:
-   Oluşturabileceğiniz **Contoso VNet** ve **Contoso alt** hedef Azure bölgesi planlama DR bir parçası olarak.
-   Ayrıca oluşturabilir ve her ikisi de yapılandırma **alt NSG** yanı **VM NSG** bölümü aynı DR planlama olarak.
-   **Alt ağ NSG** sonra hemen ilişkilendirilebilir **Contoso alt**, NSG ve alt ağ zaten mevcut olduğundan.
-   **VM NSG** kurtarma planlarına kullanarak yük devretme sırasında VM'ler ile ilişkili olabilir.

Nsg'ler oluşturup yapılandırılmış sonra çalışmasını öneririz. bir [yük devretme sınamasını](site-recovery-test-failover-to-azure.md) doğrulamak için NSG ilişkileri ve yük devretme sonrasında VM bağlantı eşitlenebilir.

## <a name="azure-to-azure-replication-with-nsg"></a>NSG ile Azure için Azure'a çoğaltma

Azure Site kurtarma sağlayan olağanüstü durum kurtarma [Azure sanal makineleri](azure-to-azure-architecture.md). Azure VM'ler için çoğaltma işlemini etkinleştirdiğinizde, Site Recovery çoğaltma üzerinde hedef bölgesi (alt ağlar ve ağ geçidi alt ağları dahil) sanal ağlar oluşturmak ve kaynak arasında gerekli eşlemeleri oluşturabilir ve hedef sanal ağları. Ayrıca, önceden hedef tarafı ağları ve alt ağlar oluştur ve aynı çoğaltma etkinleştirirken kullanabilirsiniz. Site Recovery oluşturmaz tüm VM'ler hedef Azure bölgesi öncesinde üzerinde [yük devretme](azure-to-azure-tutorial-failover-failback.md).

Azure VM çoğaltma için Azure bölgesi kaynağındaki NSG kuralları izin verdiğinden emin olun [giden bağlantı](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges) çoğaltma trafiği. Ayrıca test ve bu gerekli bu kurallarla doğrulayın [örnek NSG yapılandırma](azure-to-azure-about-networking.md#example-nsg-configuration).

Site Recovery oluşturmaz veya Nsg'ler yük devretme işleminin bir parçası olarak çoğaltılır. Yük devretmeyi başlatmadan önce hedef Azure bölgesi gerekli NGSs oluşturmanızı öneririz. Daha sonra Nsg'leri otomatik olarak yük devretme sırasında vm'lerinde Otomasyon betikleri Site Recovery'nin ile güçlü kullanarak ilişkilendirebilirsiniz [kurtarma planlarına](site-recovery-create-recovery-plans.md).

Dikkate [Örnek senaryo](concepts-network-security-group-with-site-recovery.md#using-network-security-groups) daha önce açıklanan:
-   Site kurtarma kopyaları oluşturabilir **Contoso VNet** ve **Contoso alt** hedef çoğaltma VM için etkinleştirildiğinde, Azure bölgesi.
-   İstenen çoğaltmalarının oluşturabilirsiniz **alt NSG** ve **VM NSG** (adlandırılmış, örneğin, **hedef alt ağ NSG** ve **hedef VM NSG**, sırasıyla) hedef bölgesi üzerinde gerekli herhangi bir ek kuralın izin vermeyi hedef Azure bölgesi.
-   **Hedef alt ağ NSG** sonra olabilir NSG ve alt ağ zaten mevcut olduğundan hemen hedef bölge alt ağ ile ilişkilendirilmiş.
-   **VM NSG hedef** kurtarma planlarına kullanarak yük devretme sırasında VM'ler ile ilişkili olabilir.

Nsg'ler oluşturup yapılandırılmış sonra çalışmasını öneririz. bir [yük devretme sınamasını](azure-to-azure-tutorial-dr-drill.md) doğrulamak için NSG ilişkileri ve yük devretme sonrasında VM bağlantı eşitlenebilir.

## <a name="next-steps"></a>Sonraki adımlar
-   Daha fazla bilgi edinmek [ağ güvenlik grupları](../virtual-network/security-overview.md#network-security-groups).
-   NSG hakkında daha fazla bilgi [güvenlik kuralları](../virtual-network/security-overview.md#security-rules).
-   Daha fazla bilgi edinmek [etkin güvenlik kuralları](../virtual-network/diagnose-network-traffic-filter-problem.md) bir NSG için.
-   Daha fazla bilgi edinmek [kurtarma planlarına](site-recovery-create-recovery-plans.md) uygulama yük devretmeyi otomatikleştirmek için.
