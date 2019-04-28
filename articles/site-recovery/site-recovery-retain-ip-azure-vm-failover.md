---
title: Azure Site Recovery ile Azure VM yük devretme sırasında IP adresleri saklamak | Microsoft Docs
description: IP adresleri, Azure Site Recovery ile ikincil bir bölgeye olağanüstü durum kurtarma için Azure Vm'leri üzerinde başarısız olduğunda bekletilecek açıklar
ms.service: site-recovery
ms.date: 4/9/2019
author: mayurigupta13
ms.topic: conceptual
ms.author: mayg
ms.openlocfilehash: 618d60417aa6b582eaef94bf75dcf16c74750f83
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61277333"
---
# <a name="retain-ip-addresses-during-failover"></a>Yük devretme sırasında IP adreslerini koru

[Azure Site Recovery](site-recovery-overview.md) Vm'leri, başka bir Azure bölgesine çoğaltmak, bir kesinti oluşursa yük devretmeyi ve normal olarak şeyler olduğunda birincil bölgeye geri başarısız Azure Vm'leri için olağanüstü durum kurtarma sağlar.

Yük devretme sırasında IP adresi kaynak bölgeye aynı hedef bölgede tutmak isteyebilirsiniz:

- Azure Vm'leri için olağanüstü durum kurtarma etkinleştirdiğinizde, varsayılan olarak, Site Recovery hedef kaynaklar kaynak kaynak ayarları temel alarak oluşturur. Statik IP adresleriyle yapılandırılmış Azure Vm'leri için Site Recovery kullanımda değilse hedef sanal makine, aynı IP adresi sağlamak çalışır. Adresleme, Site Recovery nasıl işleyeceğini bir tam açıklama için [bu makaleyi gözden geçirin](azure-to-azure-network-mapping.md#set-up-ip-addressing-for-target-vms).
- Basit uygulamalar için varsayılan yapılandırma yeterli olur. Daha karmaşık uygulamalar için bağlantı yük devretmeden sonra beklendiği gibi çalıştığından emin olmak için ek kaynak sağlamanız gerekebilir.


Bu makalede, koruma örnek senaryoları daha karmaşık IP adresleri için bazı örnekler sağlar. Örnekleri şunlardır:

- Azure'da çalışan tüm kaynaklar ile şirket için yük devretme
- Karma bir dağıtımda ve hem şirket içinde çalışan kaynakları ve azure'da şirket için yük devretme

## <a name="resources-in-azure-full-failover"></a>Azure'daki kaynakları: tam yük devretme

Bir şirket, Azure'da çalışan tüm uygulamalarına sahiptir.

### <a name="before-failover"></a>Önce yük devretme

Yük devretmeden önce mimari aşağıda verilmiştir.

- Şirket A aynı ağlara ve kaynak ve hedef Azure bölgeleri alt ağlar var.
- Kurtarma süresi hedefi (RTO) azaltmak için SQL Server Always On için etki alanı denetleyicileri vb. yineleme düğümleri şirket kullanır. Kaynak ve hedef bölgeler arasında VPN siteden siteye bağlantı kurarak Bu çoğaltma hedef bölgedeki farklı bir sanal ağ içinde düğümlerdir. Bu, kaynak ve hedef aynı IP adresi alanını kullanılıyorsa mümkün değildir.  
- Yük devretme önce ağ mimarisi aşağıdaki gibidir:
    - Birincil bölgedir Azure Doğu Asya
        - Doğu Asya sahip bir VNet (**kaynak VNet**) ile adres alanı 10.1.0.0/16.
        - Doğu Asya sanal ağ içindeki üç alt ağlar arasındaki bölme iş yükleri vardır:
            - **Alt ağ 1**: 10.1.1.0/24
            - **Alt ağı 2**: 10.1.2.0/24,
            - **Alt ağ 3**: 10.1.3.0/24
    - İkincil (hedef) bölgedir Azure Güneydoğu Asya
        - Güneydoğu Asya sahip bir kurtarma sanal ağ (**kurtarma VNet**) aynı **kaynak VNet**.
        - Güneydoğu Asya, ek bir sanal ağa sahip (**Azure VNet**) ile adres alanı 10.2.0.0/16.
        - **Azure sanal ağı** bir alt ağı içeriyor (**alt 4**) adres alanı 10.2.4.0/24 ile.
        - Yineleme düğümleri için SQL Server Always On, etki alanı denetleyicisi vb. yerleştirilir **alt 4**.
    - **Kaynak VNet** ve **Azure VNet** VPN siteden siteye bağlantı ile bağlı.
    - **Kurtarma VNet** diğer sanal ağ ile bağlı değil.
    - **Bir şirket** çoğaltılan öğeler için hedef IP adresleri atar/doğrular. Hedef IP, her VM için kaynak IP ile aynıdır.

![Tam Yük devretmeden önce Azure kaynakları](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-before-failover2.png)

### <a name="after-failover"></a>Yük devretmeden sonra

Kaynak bölgesel bir kesinti oluşursa, hedef bölge için tüm kaynaklarını şirketi devredebilir.

- Yük devretmeden önce yerinde zaten hedef IP adresi ile şirketi yük devretmelerini düzenlemek ve otomatik olarak arasında yük devretme işleminden sonra bağlantıları kurmak **kurtarma VNet** ve **Azure VNet**. Bu, aşağıdaki diyagramda gösterilmiştir...
- İki sanal ağ arasındaki bağlantıları olan uygulama gereksinimlerine bağlı olarak (**kurtarma VNet** ve **Azure VNet**) hedef bölge olabilir kurulan önce sırasında (ara adım) olarak veya yük devretme işleminden sonra.
  - Şirketin kullanabileceği [kurtarma planları](site-recovery-create-recovery-plans.md) bağlantı kuran zaman belirtmek için.
  - VNet eşlemesi veya siteden siteye VPN kullanarak sanal ağlar arasında birbirine bağlanabilir.
      - VNet eşlemesi bir VPN ağ geçidi kullanmaz ve farklı kısıtlamaları vardır.
      - VNet eşlemesi [fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-network) VNet-VNet VPN ağ geçidi farklı hesaplanır [fiyatlandırma](https://azure.microsoft.com/pricing/details/vpn-gateway). Yük devretme işlemleri için biz genellikle ağ öngörülemeyen olayları en aza indirmek için bağlantı türü de dahil olmak üzere kaynak ağları olarak aynı bağlantı yöntemini kullanmak için önerin.

    ![Kaynakları Azure tam yük devretme](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-full-region-failover2.png)



## <a name="resources-in-azure-isolated-app-failover"></a>Azure'daki kaynakları: yalıtılmış uygulama yük devretme

Uygulama düzeyinde yük devretmek gerekebilir. Örneğin, belirli bir uygulama veya uygulama katmanı üzerinden başarısız bir ayrılmış alt ağında yer alan.

- IP adresleme, tutabilirsiniz ancak bağlantı tutarsızlıklar olasılığını artırır olduğundan bu senaryoda, genel olarak önerilmez. Ayrıca, diğer alt ağlara aynı Azure sanal ağ içindeki alt ağ bağlantısını kaybedersiniz.
- Alt düzey uygulama yük devretme yapmak için daha iyi bir yolu, (kaynak sanal ağ üzerindeki diğer alt ağlara bağlantısı gerekiyorsa) yük devretme için farklı bir hedef IP adresleri kullanmak üzere veya kaynak bölgedeki adanmış kendi sanal ağ içindeki her bir uygulamayı ayırmak istiyorsanız ' dir. İkinci yaklaşımda kaynak bölgedeki ağlar arasında bağlantı kurmak ve hedef bölgeye yük devretme sırasında aynı davranışını taklit.  

Bu örnekte, şirket bir yerde kaynak bölgede uygulamalarında sanal ağlara ayrılmış ve bu sanal ağlar arasında bağlantı kurar. Bu tasarımla, bunlar yalıtılmış uygulama yük devretme gerçekleştirmek ve kaynak özel IP adresleri hedef ağdaki korur.

### <a name="before-failover"></a>Önce yük devretme

Yük devretme önce mimarisi aşağıdaki gibidir:

- Uygulama Vm'leri, Birincil Azure Doğu Asya bölgede barındırılır:
    - **App1** VM'ler Vnet'te bulunan **kaynak VNet 1**: 10.1.0.0/16.
    - **App2** VM'ler Vnet'te bulunan **kaynak VNet 2**: 10.2.0.0/16.
    - **Kaynak VNet 1** iki alt ağa sahip.
    - **Kaynak VNet 2** iki alt ağa sahip.
- (Hedef) ikincil bölgede Azure Güneydoğu Asya - Güneydoğu Asya sahip bir kurtarma sanal ağlar (**kurtarma VNet 1** ve **kurtarma VNet 2**) aynı olan **kaynak VNet 1** ve **Kaynak VNet 2**.
        - **Kurtarma sanal ağ 1** ve **kurtarma VNet 2** her alt ağda eşleşen iki alt ağa sahip **kaynak VNet 1** ve **kaynak VNet 2** -Güneydoğu Asya sahip bir ek sanal ağ (**Azure VNet**) adres alanı 10.3.0.0/16 ile.
        - **Azure sanal ağı** bir alt ağı içeriyor (**alt 4**) adres alanı 10.3.4.0/24 ile.
        -Yineleme düğümleri için SQL Server Always On, etki alanı denetleyicisi vb. yerleştirilir **alt 4**.
- Siteden siteye VPN bağlantılarının sayısı vardır: 
    - **Kaynak VNet 1** ve **Azure sanal ağı**
    - **Kaynak VNet 2** ve **Azure sanal ağı**
    - **Kaynak VNet 1** ve **kaynak VNet 2** VPN-siteler ile bağlı
- **Kurtarma sanal ağ 1** ve **kurtarma VNet 2** herhangi diğer sanal ağlara bağlı değildir.
- **Bir şirket** VPN ağ geçitleri yapılandırır **kurtarma VNet 1** ve **kurtarma VNet 2**RTO azaltmak için.  
- **Kurtarma VNet1** ve **kurtarma vnet2'den** diğer sanal ağ ile bağlı değil.
- Kurtarma süresi hedefi (RTO) azaltmak için VPN ağ geçitleri üzerinde yapılandırılmış **kurtarma VNet1** ve **kurtarma vnet2'den** yük devretme öncesinde.

    ![Uygulama yük devretmeden önce Azure kaynakları](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-isolated-application-before-failover2.png)

### <a name="after-failover"></a>Yük devretmeden sonra

Bir kesinti veya tek bir uygulama etkileyen bir sorun olması durumunda (içinde ** örneğimizde kaynak VNet 2), şirket bir kurtarabilir etkilenen uygulamayı şu şekilde:


- VPN bağlantıları arasında kesin **kaynak VNet1** ve **kaynak vnet2'den**ve arasında **kaynak vnet2'den** ve **Azure VNet** .
- VPN bağlantı arasındaki **kaynak VNet1** ve **kurtarma vnet2'den**ve arasında **kurtarma vnet2'den** ve **Azure VNet**.
- Vm'lerde yük devretme **kaynak VNet2** için **kurtarma vnet2'den**.

![Azure uygulama yük devretme kaynakları](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-isolated-application-after-failover2.png)


- Bu örnek, daha fazla uygulama ve ağ bağlantıları içerecek şekilde genişletilebilir. Kaynaktan hedefe devretmek, mümkün olduğunca uzak onaylamaktan bir gibi benzeri bağlantı modeli izlenmesi önerilir.
- VPN ağ geçitleri, bağlantılar kurmak için genel IP adresleri ve ağ geçidi atlama kullanın. Genel IP adresleri kullanmak istemediğiniz veya ek atlama önlemek istiyorsanız, kullanabileceğiniz [Azure VNet eşlemesi](../virtual-network/virtual-network-peering-overview.md) arasında sanal ağları eşleyebilme [Azure bölgeleri desteklenen](../virtual-network/virtual-network-manage-peering.md#cross-region).

## <a name="hybrid-resources-full-failover"></a>Karma kaynaklarını: tam yük devretme

Bu senaryoda, **Şirket B** Azure ve şirket içinde çalışan Kalan çalışan uygulama altyapısının bir parçası olan bir hibrit iş çalıştırır. 

### <a name="before-failover"></a>Önce yük devretme

Ağ mimarisi önce yük devretme nasıl göründüğünü aşağıda verilmiştir.

- Uygulama Vm'leri Azure Doğu Asya'da barındırılır.
- Doğu Asya sahip bir VNet (**kaynak VNet**) ile adres alanı 10.1.0.0/16.
  - Doğu Asya sahip iş yükleri üç alt ağlar arasındaki bölme **kaynak VNet**:
    - **Alt ağ 1**: 10.1.1.0/24
    - **Alt ağı 2**: 10.1.2.0/24,
    - **Alt ağ 3**: bir Azure sanal ağ adres alanı 10.1.0.0/16 ile 10.1.3.0/24utilizing. Bu sanal ağ adlı **kaynak sanal ağ**
      - Azure Güney Doğu Asya (hedef) ikincil bölgeye şöyledir:
  - Güneydoğu Asya sahip bir kurtarma sanal ağ (**kurtarma VNet**) aynı **kaynak VNet**.
- Doğu Asya Vm'leri Azure ExpressRoute veya VPN sitesi site ile bir şirket içi veri merkezine bağlıdır.
- RTO azaltmak için ağ geçitleri Azure Güneydoğu Asya, Kurtarma VNet üzerinde yük devretme öncesinde Şirket B sağlar.
- Şirket B atar/hedef IP adresleri çoğaltılan VM'ler için doğrular. Hedef IP adresine kaynak IP adresi her VM için aynıdır.


![Yük devretmeden önce şirket içi-Azure'a bağlantısı](./media/site-recovery-retain-ip-azure-vm-failover/on-premises-to-azure-connectivity-before-failover2.png)

### <a name="after-failover"></a>Yük devretmeden sonra


Kaynak bölgesel bir kesinti oluşursa, hedef bölge için tüm kaynaklarını Şirket B devredebilir.

- Yük devretmeden önce yerinde zaten hedef IP adresi ile Şirket B yük devretmelerini düzenlemek ve otomatik olarak arasında yük devretme işleminden sonra bağlantıları kurmak **kurtarma VNet** ve **Azure VNet**.
- İki sanal ağ arasındaki bağlantıları olan uygulama gereksinimlerine bağlı olarak (**kurtarma VNet** ve **Azure VNet**) hedef bölge olabilir kurulan önce sırasında (ara adım) olarak veya yük devretme işleminden sonra. Şirketin kullanabileceği [kurtarma planları](site-recovery-create-recovery-plans.md) bağlantı kuran zaman belirtmek için.
- Azure Güneydoğu Asya ve şirket içi veri merkezi arasında bağlantı kurmadan önce Azure Doğu Asya ve şirket içi veri merkezi arasında özgün bağlantı kesilmesi gerekir.
- Yönlendirme şirket içinde hedef bölgeye işaret edecek şekilde yeniden yapılandırılacak ve Yük Devretme ağ geçitleri yayınlayın.

![Şirket içi-Azure'a yük devretme sonrasında bağlantısı](./media/site-recovery-retain-ip-azure-vm-failover/on-premises-to-azure-connectivity-after-failover2.png)

## <a name="hybrid-resources-isolated-app-failover"></a>Karma kaynaklarını: yalıtılmış uygulama yük devretme

Alt ağ düzeyinde yalıtılmış uygulamalar üzerinde Şirket B çalışma özelliğini kullanamazsınız. Kaynak ve kurtarma sanal ağ adres alanını aynıdır ve şirket içi bağlantı özgün kaynağına etkin olduğundan budur.

 - Uygulama dayanıklılığı için Şirket B her uygulama kendi adanmış bir Azure sanal ağınızda yerleştirmeniz gerekecektir.
 - Ayrı bir sanal ağ içindeki her bir uygulamayla Şirket B yalıtılmış uygulamalar başarısız ve kaynak bağlantıları için hedef bölgede yol.

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [kurtarma planları](site-recovery-create-recovery-plans.md).
