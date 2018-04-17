---
title: Azure sanal ağı (VNet) Plan ve Tasarım Kılavuzu | Microsoft Docs
description: Planlama ve tasarım yalıtımı, bağlantı ve konumunu gereksinimlerinize göre azure'da sanal ağlar hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: tysonn
ms.assetid: 3a4a9aea-7608-4d2e-bb3c-40de2e537200
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/08/2016
ms.author: jdial
ms.openlocfilehash: 6e41dae2f4e93fe2e3cef689596612a6a192c844
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="plan-and-design-azure-virtual-networks"></a>Planlama ve Azure sanal ağlar tasarlama
Denemeniz için bir VNet oluşturma yeterince kolay, ancak büyük olasılıkla, birden çok sanal ağlar, kuruluşunuzun üretim gereksinimlerini desteklemek için zaman içinde dağıtır. Bazı planlama ve tasarım ile sanal ağlar dağıtabilmeli ve daha etkili bir şekilde ihtiyacınız olan kaynakları bağlanmak olacaktır. Sanal ağlar ile bilmiyorsanız, önermiştir, [sanal ağlar hakkında bilgi edinin](virtual-networks-overview.md) ve [nasıl dağıtılacağı](quick-create-portal.md) devam etmeden önce bir.

## <a name="plan"></a>Planlama
Azure Abonelikleri, bölgeler ve ağ kaynaklarının kapsamlı olarak anlamayı başarısı için önemlidir. Konuların listesini bir başlangıç noktası olarak kullanabilirsiniz. Bu konuları anladığınızda, ağ tasarımınız için gereksinimler tanımlayabilirsiniz.

### <a name="considerations"></a>Dikkat edilmesi gerekenler
Aşağıdaki sorular planlama yanıtlama önce aşağıdakileri dikkate alın:

* Her şeyi Azure içinde oluşturduğunuz bir veya daha fazla kaynak oluşur. Bir sanal makine (VM) bir kaynaktır bir VM tarafından kullanılan ağ bağdaştırıcısı arabirimi (NIC) bir kaynak, bir NIC tarafından kullanılan genel IP adresini bir kaynaktır, NIC bağlı VNet bir kaynaktır.
* Kaynakları içinde oluşturduğunuz bir [Azure bölgesi](https://azure.microsoft.com/regions/#services) ve abonelik. Kaynaklar yalnızca aynı bölgede bulunan bir sanal ağa bağlanabilir ve abonelik kaynak bulunmaktadır.
* Sanal ağlar birbirlerine kullanarak bağlayabilirsiniz:
    * **[Sanal Ağ eşlemesi](virtual-network-peering-overview.md)**: sanal ağlar aynı Azure bölgesinde bulunması gerekir. Kaynakları aynı sanal ağa bağlıymış gibi eşlenen sanal ağlarda bulunan kaynaklar arasındaki bant genişliği aynıdır.
    * **Bir Azure [VPN ağ geçidi](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)**: sanal ağlar aynı ya da farklı Azure bölgelerinde bulunabilir. Bir VPN ağ geçidi üzerinden bağlı sanal ağlarda bulunan kaynaklar arasındaki bant genişliği, VPN ağ geçidi bant genişliği ile sınırlıdır.
* Aşağıdakilerden birini kullanarak sanal ağlar, şirket içi ağınıza bağlayabilirsiniz [bağlantı seçenekleri](../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti) Azure içinde kullanılabilir.
* Farklı kaynaklar gruplandırılabilir birlikte [kaynak grupları](../azure-resource-manager/resource-group-overview.md#resource-groups), bir birim olarak kaynak yönetmenizi kolaylaştırır. Kaynaklar aynı aboneliğe ait olduğu sürece, bir kaynak grubu birden çok bölgelerdeki kaynakları içerebilir.

### <a name="define-requirements"></a>Gereksinimleri tanımlayın
Aşağıdaki sorular Azure ağ tasarımınız için başlangıç noktası olarak kullanın.    

1. Hangi Azure konumları ana bilgisayar sanal ağlar kullanacak mısınız?
2. Bu Azure konumlar arasında iletişim sağlamak üzere gerekiyor mu?
3. Azure VNet(s) ve şirket içi inizdeki arasındaki iletişimi sağlamanız gerekiyor mu?
4. Bir hizmet (Iaas) sanal olarak kaç altyapı bulut rolleri ve web uygulamaları çözümünüz için gereken hizmetleri?
5. Sanal makineleri (yani ön uç web sunucuları ve arka uç veritabanı sunucuları) gruplarını temel alan trafiğini yalıtmak gerekiyor mu?
6. Sanal gereçler kullanma trafik akışını denetlemenizi gerekiyor mu?
7. Kullanıcıların farklı farklı Azure kaynakları için izin kümeleri gerekiyor mu?

### <a name="understand-vnet-and-subnet-properties"></a>VNet ve alt ağ özelliklerini anlama
VNet ve alt kaynakları Azure'da çalışan iş yükleri için güvenlik sınırı tanımlamaya yardımcı olacak. Bir sanal ağ adres alanları, CIDR bloğu tanımlı bir koleksiyon tarafından belirlenir.

> [!NOTE]
> Ağ yöneticileri ile CIDR gösteriminde biliyorsunuzdur. CIDR ile bilmiyorsanız [hakkında daha fazla bilgi](http://whatismyipaddress.com/cidr).
>
>

Sanal ağlar aşağıdaki özellikleri içerir.

| Özellik | Açıklama | Kısıtlamalar |
| --- | --- | --- |
| **Adı** |VNet adı |En fazla 80 karakter dizesi. Harf, rakam, alt çizgi, nokta veya kısa çizgi içerebilir. Bir harf veya sayı ile başlamalıdır. Bir harf, sayı veya alt çizgi ile bitmelidir. Üst veya küçük harf içerir. |
| **konum** |Azure konum (bölge da bilinir). |Geçerli Azure konumlardan birinde olmalıdır. |
| **addressSpace** |CIDR gösteriminde VNet oluşturan adres öneklerini koleksiyonu. |Genel IP adresi aralıklarının da dahil olmak üzere, geçerli CIDR adres bloklarını bir dizi olmalıdır. |
| **Alt ağlar** |VNet yapmak alt koleksiyonu |Aşağıdaki alt ağ özelliklerini tabloya bakın. |
| **dhcpOptions** |Adlı tek gerekli bir özellik içeren nesne **dnsServers**. | |
| **dnsServers** |Sanal ağ tarafından kullanılan DNS sunucuları dizisi. Hiçbir sunucu belirtilirse, Azure dahili ad çözümlemesi kullanılır. |Bir dizi IP adresine göre en fazla 10 DNS sunucuları olması gerekir. |

Bir alt ağ alt bir vnet'in kaynaktır ve adres alanları IP adresi öneklerini kullanarak bir CIDR bloğu içinde kesimleri yardımcı tanımlayın. NIC alt ağlara eklendi ve çeşitli iş yükleri için bağlantı sağlama VM'ler bağlı.

Alt ağları aşağıdaki özellikleri içerir.

| Özellik | Açıklama | Kısıtlamalar |
| --- | --- | --- |
| **Adı** |Alt ağ adı |En fazla 80 karakter dizesi. Harf, rakam, alt çizgi, nokta veya kısa çizgi içerebilir. Bir harf veya sayı ile başlamalıdır. Bir harf, sayı veya alt çizgi ile bitmelidir. Üst veya küçük harf içerir. |
| **konum** |Azure konum (bölge da bilinir). |Geçerli Azure konumlardan birinde olmalıdır. |
| **addressPrefix** |Alt ağ CIDR gösteriminde oluşturan tek adresi öneki |Sanal ağınızın adres alanlarından birini parçası olan tek bir CIDR bloğu olmalıdır. |
| **networkSecurityGroup** |NSG alt ağına uygulanır | |
| **routeTable** |Alt ağa uygulanan yol tablosu | |
| **Ipconfigurations** |Alt ağına bağlı NIC tarafından kullanılan IP yapılandırma nesneleri koleksiyonu | |

### <a name="name-resolution"></a>Ad çözümlemesi
Varsayılan olarak, sanal ağınızı kullanır [Azure tarafından sağlanan ad çözümlemesi](virtual-networks-name-resolution-for-vms-and-role-instances.md) VNet içindeki ve ortak Internet'te adları çözümlemek için. Şirket içi veri merkezleri, sanal ağlar bağlanıyorsanız, ancak sağlamanız gerekir [kendi DNS sunucusu](virtual-networks-name-resolution-for-vms-and-role-instances.md) ağlarınız arasında adları çözümlemek için.  

### <a name="limits"></a>Sınırlar
Ağ sınırları gözden [Azure sınırlar](../azure-subscription-service-limits.md#networking-limits) tasarımınızı herhangi bir sınırları ile çakışan değil emin olmak için makale. Bazı sınırlar bir destek bileti açılarak artırılabilir.

### <a name="role-based-access-control-rbac"></a>Rol Tabanlı Erişim Denetimi (RBAC)
Kullanabileceğiniz [Azure RBAC](../role-based-access-control/built-in-roles.md) farklı kullanıcıların farklı kaynaklara sahip olabilir erişim düzeyini denetlemek için. Bu şekilde kendi ihtiyaçlarına göre ekibiniz tarafından çalışmanın ayırabilirsiniz.

Sanal ağlar olarak uzak kaygı, kullanıcılar **ağ Katılımcısı** rol Azure Resource Manager sanal ağ kaynaklarına üzerinde tam denetime sahiptir. Benzer şekilde, kullanıcılar **Klasik ağ Katılımcısı** rol Klasik sanal ağ kaynaklarına üzerinde tam denetime sahiptir.

> [!NOTE]
> Ayrıca [kendi rolleri oluşturma](../role-based-access-control/role-assignments-portal.md) yönetim gereksinimlerinizi ayırmak için.
>
>

## <a name="design"></a>Tasarım
Soruların yanıtlarını öğrendikten sonra [planlama](#Plan) bölümünde, sanal ağlar tanımlama önce aşağıdakileri gözden geçirin.

### <a name="number-of-subscriptions-and-vnets"></a>Abonelikler ve sanal ağlar sayısı
Birden çok sanal ağlar aşağıdaki senaryolarda oluşturmayı düşünebilirsiniz:

* **Azure farklı konumlarda yerleştirilmesi gereken sanal makineleri**. Azure sanal ağları bölgesel. Bunlar konumları yayılamaz. Bu nedenle konak vm'lerinin istediğiniz her Azure konumu için en az bir VNet gerekir.
* **Birbirinden tamamen yalıtılmış olması gereken iş yükleri**. Kullanan bile aynı IP adresi alanlarını, farklı iş yükleri birbirinden ayırmak için ayrı sanal ağlar oluşturabilirsiniz.

Yukarıda gördüğünüz her bölge, abonelik başına kısıtlamalardır aklınızda bulundurun. Birden çok abonelik Azure'da koruyabilirsiniz kaynakları sınırını artırmak için kullanabileceğiniz anlamına gelir. Farklı Aboneliklerde sanal ağlara bağlanmak için siteden siteye VPN ya da bir expressroute bağlantı hattı kullanabilirsiniz.

### <a name="subscription-and-vnet-design-patterns"></a>Abonelik ve VNet tasarım desenleri
Aşağıdaki tabloda, abonelikleri ve sanal ağlar kullanmak için bazı ortak tasarım desenleri gösterilmektedir.

| Senaryo | Diyagram | Uzmanları | Simgeler |
| --- | --- | --- | --- |
| Tek bir abonelik, uygulama başına iki sanal ağlar |![Tek abonelik](./media/virtual-network-vnet-plan-design-arm/figure1.png) |Yönetmek için yalnızca bir abonelik. |Sanal ağlar maksimum sayısı her Azure bölgesi. Daha fazla abonelik bundan sonra gerekir. Gözden geçirme [Azure sınırlar](../azure-subscription-service-limits.md#networking-limits) Ayrıntılar için makale. |
| Uygulama, uygulama başına iki Vnet başına tek abonelikle |![Tek abonelik](./media/virtual-network-vnet-plan-design-arm/figure2.png) |Abonelik başına yalnızca iki Vnet kullanır. |Çok fazla uygulama olduğunda yönetmek daha zor. |
| Departman, uygulama başına iki Vnet başına tek abonelikle. |![Tek abonelik](./media/virtual-network-vnet-plan-design-arm/figure3.png) |Abonelik sayısı ve sanal ağlar arasında dengeleyin. |Sanal ağlar maksimum sayısı her iş birimi (abonelik). Gözden geçirme [Azure sınırlar](../azure-subscription-service-limits.md#networking-limits) Ayrıntılar için makale. |
| Departman, uygulama grubu başına iki Vnet başına tek abonelikle. |![Tek abonelik](./media/virtual-network-vnet-plan-design-arm/figure4.png) |Abonelik sayısı ve sanal ağlar arasında dengeleyin. |Alt ağları ve Nsg'ler kullanarak uygulamaları yalıtılmış gerekir. |

### <a name="number-of-subnets"></a>Alt ağların sayısı
Aşağıdaki senaryolarda bir VNet içindeki birden çok alt ağı dikkate almanız gerekir:

* **Bir alt ağdaki tüm NIC'ler için yeterli özel IP adresleri**. Alt ağ adresi alanınızı NIC sayısı alt ağda yeterli IP adresine içermiyorsa, birden çok alt ağı oluşturmanız gerekir. Azure kullanılamaz her alt ağdan 5 özel IP adresleri ayırır göz önünde bulundurun: adres alanı (alt ağ adresi ve çok noktaya yayın) ilk ve son adreslerini ve 3 adreslerini dahili (DHCP ve DNS amaçları için).
* **Güvenlik**. Sanal makineleri grupları yapısı ve farklı uygulamak çok katmanlı olan iş yükleri için birbirinden ayırmak için alt ağları kullanın [güvenlik gruplarını (Nsg'ler) ağ](virtual-networks-nsg.md#subnets) bu alt ağlardan için.
* **Karma bağlantı**. VPN ağ geçitleri ve ExpressRoute bağlantı hatları için kullanabileceğiniz [bağlanmak](../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti) , sanal ağlar birbirlerine ve şirket içi veri center(s). VPN ağ geçitleri ve ExpressRoute bağlantı hatları oluşturulacak kendi alt ağı gerektirir.
* **Sanal gereçler**. Güvenlik Duvarı, WAN Hızlandırıcı veya VPN ağ geçidi gibi bir sanal gereç bir Azure VNet kullanabilirsiniz. Bunu yaptığınızda, gerek [trafiği yönlendirmek](virtual-networks-udr-overview.md) bu cihazları için ve kendi alt ağda yalıtır.

### <a name="subnet-and-nsg-design-patterns"></a>Alt ağı ve NSG tasarım desenleri
Aşağıdaki tabloda, alt ağları kullanarak bazı ortak tasarım desenleri gösterilmektedir.

| Senaryo | Diyagram | Uzmanları | Simgeler |
| --- | --- | --- | --- |
| Tek alt ağ, uygulama başına uygulama katmanı başına Nsg'ler |![Tek alt ağ](./media/virtual-network-vnet-plan-design-arm/figure5.png) |Yönetmek için yalnızca bir alt ağ'ı seçin. |Birden çok Nsg'ler her uygulama yalıtmak gerekli. |
| Uygulama, uygulama katmanı başına Nsg'ler başına bir alt ağ |![Uygulama başına alt ağ](./media/virtual-network-vnet-plan-design-arm/figure6.png) |Yönetmek için daha az Nsg'ler. |Yönetmek için birden fazla alt ağ'ı seçin. |
| Uygulama katmanı, uygulama başına Nsg'ler başına bir alt ağ. |![Katman her alt ağ](./media/virtual-network-vnet-plan-design-arm/figure7.png) |Nsg'ler alt ağların sayısı arasında dengeleyin. |Abonelik başına Nsg'ler maksimum sayısı. Gözden geçirme [Azure sınırlar](../azure-subscription-service-limits.md#networking-limits) Ayrıntılar için makale. |
| Uygulama, her alt ağ Nsg'ler başına uygulama katmanı başına bir alt ağ |![Alt ağına göre uygulama başına katmanı](./media/virtual-network-vnet-plan-design-arm/figure8.png) |Büyük olasılıkla daha az sayıda Nsg'ler. |Yönetmek için birden fazla alt ağ'ı seçin. |

## <a name="sample-design"></a>Örnek tasarımı
Bu makaledeki bilgiler uygulama göstermek için aşağıdaki senaryoyu göz önünde bulundurun.

Kuzey Amerika 2 veri merkezlerinde ve iki veri merkezleri Avrupa sahip bir şirket için çalışır. 2 ile farklı tutulan 6 farklı müşteri yan yana uygulamalar tanımlanan bir pilot olarak azure'a geçirmek istediğiniz iş birimleri. Uygulamalar için temel mimari aşağıdaki gibidir:

* App1 ve App2, App3 ve App4 Ubuntu Linux sunucuları üzerinde barındırılan web uygulamalardır. Her uygulama barındıran RESTful hizmetlerini Linux sunuculara ayrı bir uygulama sunucusuna bağlanır. RESTful hizmetlerini bir arka uç MySQL veritabanına bağlanın.
* App5 ve App6 Windows Server 2012 R2 çalıştıran Windows sunucularında barındırılan web uygulamalardır. Her bir uygulama bir arka uç SQL Server veritabanına bağlar.
* Tüm uygulamalar, şu anda Kuzey Amerika şirketin veri merkezlerinde birinde barındırılır.
* Şirket içi veri merkezleri 10.0.0.0/8 adres alanı kullanın.

Aşağıdaki gereksinimleri karşılayan bir sanal ağ çözümü tasarlamanız gerekir:

* Her iş birimi diğer iş birimleri kaynak tüketimini tarafından etkilenmez.
* Sanal ağlar ve alt ağları yönetimini kolaylaştırmak için miktarı en aza indirmeniz gerekir.
* Her iş birimi bir tek test/geliştirme tüm uygulamalar için kullanılan VNet olması gerekir.
* Her uygulama kıtada (Kuzey Amerika ve Avrupa'da) başına 2 farklı Azure veri merkezleri içinde barındırılır.
* Her uygulama birbirinden tamamen ayrı tutulur.
* Her uygulama müşteriler tarafından HTTP kullanarak Internet üzerinden erişilebilir.
* Her uygulama için şirket içi veri merkezleri şifrelenmiş tüneli kullanarak bağlı kullanıcılar tarafından erişilebilir.
* Şirket içi veri merkezleri bağlantı mevcut VPN aygıtları kullanmanız gerekir.
* Şirketin ağ grubun VNet yapılandırmasını üzerinde tam denetimi olmalıdır.
* Her iş birimi geliştiriciler yalnızca varolan alt ağlara sanal makineleri dağıtamaz olmalıdır.
* Tüm uygulamaları, Azure (yükseltme ve shift) olarak geçirilecektir.
* Her konum veritabanlarında diğer Azure konumlara günde bir kez çoğaltılması.
* Her uygulama 5 ön uç web sunucuları, 2 uygulama sunucuları (gerektiğinde) ve 2 veritabanı sunucuları kullanmanız gerekir.

### <a name="plan"></a>Planlama
Tasarımınızın soruyu yanıtlayarak planlama başlamalıdır [gereksinimlerini tanımlamanıza](#Define-requirements) bölümünde aşağıda gösterildiği gibi.

1. Hangi Azure konumları ana bilgisayar sanal ağlar kullanacak mısınız?

    2 konumlarda Kuzey Amerika ve Avrupa'da 2 konumları. Bu, mevcut şirket içi veri merkezleri fiziksel konuma göre seçmeniz gerekir. Bu şekilde, fiziksel konumlardan bağlantınızı Azure daha iyi bir gecikme süresi gerekir.
2. Bu Azure konumlar arasında iletişim sağlamak üzere gerekiyor mu?

    Evet. Bu yana veritabanlarını tüm konumlara çoğaltılması gerekir.
3. Veri center(s) Azure VNet(s) ve şirket içi arasındaki iletişimi sağlamanız gerekiyor mu?

    Evet. Kullanıcıların bağlı olduğundan şirket içi veri merkezleri uygulamalar şifrelenmiş bir tünel üzerinden erişebilir olması gerekir.
4. Kaç tane Iaas VM'ler çözümünüz için gerekiyor mu?

    200 Iaas VM'ler. App1 ve App2, App3 ve App4 her 5 web sunucularının her, 2 uygulama sunucularının her ve 2 veritabanı sunucuları gerektirir. Uygulama başına 9 Iaas VM'ler veya 36 Iaas Vm'leri toplam olmasıdır. App5 ve App6 5 web sunucuları ve 2 veritabanı sunucuları gerektirir. Uygulama başına 7 Iaas VM'ler veya 14 Iaas Vm'leri toplam olmasıdır. Bu nedenle, her bir Azure bölgesine tüm uygulamalar için 50 Iaas Vm'leri gerekir. 4 bölgeleri kullanmak ihtiyacımız olduğundan, 200 Iaas Vm'leri olacaktır.

    DNS sunucularının her VNet ya da Azure Iaas Vm'leri ve şirket içi ağınız arasında adı çözümlemek için şirket içi veri merkezleri sağlamak gerekir.
5. Sanal makineleri (yani ön uç web sunucuları ve arka uç veritabanı sunucuları) gruplarını temel alan trafiğini yalıtmak gerekiyor mu?

    Evet. Her uygulama birbirinden tamamen yalıtılmış olması gerekir ve her uygulama katmanı da yalıtılmış olması gerekir.
6. Sanal gereçler kullanma trafik akışını denetlemenizi gerekiyor mu?

    Hayır. Sanal Gereçleri, ayrıntılı veri düzlemi günlükleri dahil olmak üzere, trafik akışı üzerinde daha fazla denetim sağlamak için kullanılabilir.
7. Kullanıcıların farklı farklı Azure kaynakları için izin kümeleri gerekiyor mu?

    Evet. Geliştiriciler yalnızca Vm'leri önceden var olan alt ağlarınıza dağıtmanız gerekir sırasında ağ takım sanal ağ ayarları üzerinde tam denetim gerekir.

### <a name="design"></a>Tasarım
Abonelikler, sanal ağlar, alt ağları ve Nsg'ler belirtme tasarım izlemelisiniz. Nsg'ler burada ele alınacaktır, ancak, daha fazla bilgi edinmek [Nsg'ler](virtual-networks-nsg.md) tasarımınızı tamamlamadan önce.

**Abonelikler ve sanal ağlar sayısı**

Aşağıdaki gereksinimleri abonelikleri ve sanal ağlar ilgili:

* Her iş birimi diğer iş birimleri kaynak tüketimini tarafından etkilenmez.
* Sanal ağlar ve alt ağları miktarı en aza indirmeniz gerekir.
* Her iş birimi bir tek test/geliştirme tüm uygulamalar için kullanılan VNet olması gerekir.
* Her uygulama kıtada (Kuzey Amerika ve Avrupa'da) başına 2 farklı Azure veri merkezleri içinde barındırılır.

Bu gereksinimlerine bağlı olarak, her iş birimi için bir abonelik gerekiyor. Bu şekilde, bir iş biriminin kaynakları tüketiminin sınırları diğer iş birimleri için sayar değil. Ve sanal ağlar sayısını en aza indirmek istediğiniz beri kullanmayı düşünmelisiniz **departman, uygulama grubu başına iki Vnet başına tek abonelikle** desen aşağıda görüldüğü gibi.

![Tek abonelik](./media/virtual-network-vnet-plan-design-arm/figure9.png)

Ayrıca her sanal ağ için adres alanı belirtmeniz gerekir. Gereksinim duyduğunuz beri şirket içi veri arasında bağlantı merkezleri ve Azure bölgeleri Azure sanal ağlar için kullanılan adres alanı ile şirket içi ağ artar olamaz ve her sanal ağ tarafından kullanılan adres alanı ile var olan diğer sanal ağlardan artar değil. Bu gereksinimleri karşılamak için aşağıdaki tabloda adres alanlarını kullanabilirsiniz.  

| **Abonelik** | **Sanal ağ** | **Azure bölgesi** | **Adres alanı** |
| --- | --- | --- | --- |
| BU1 |ProdBU1US1 |Batı ABD |172.16.0.0/16 |
| BU1 |ProdBU1US2 |Doğu ABD |172.17.0.0/16 |
| BU1 |ProdBU1EU1 |Kuzey Avrupa |172.18.0.0/16 |
| BU1 |ProdBU1EU2 |Batı Avrupa |172.19.0.0/16 |
| BU1 |TestDevBU1 |Batı ABD |172.20.0.0/16 |
| BU2 |TestDevBU2 |Batı ABD |172.21.0.0/16 |
| BU2 |ProdBU2US1 |Batı ABD |172.22.0.0/16 |
| BU2 |ProdBU2US2 |Doğu ABD |172.23.0.0/16 |
| BU2 |ProdBU2EU1 |Kuzey Avrupa |172.24.0.0/16 |
| BU2 |ProdBU2EU2 |Batı Avrupa |172.25.0.0/16 |

**Alt ağları ve Nsg'ler sayısı**

Aşağıdaki gereksinimleri alt ağları ve Nsg'ler ilgili:

* Sanal ağlar ve alt ağları miktarı en aza indirmeniz gerekir.
* Her uygulama birbirinden tamamen ayrı tutulur.
* Her uygulama müşteriler tarafından HTTP kullanarak Internet üzerinden erişilebilir.
* Her uygulama için şirket içi veri merkezleri şifrelenmiş tüneli kullanarak bağlı kullanıcılar tarafından erişilebilir.
* Şirket içi veri merkezleri bağlantı mevcut VPN aygıtları kullanmanız gerekir.
* Her konum veritabanlarında diğer Azure konumlara günde bir kez çoğaltılması.

Bu gereksinimlerine bağlı olarak, size uygulama katmanı başına tek bir alt ağda kullanmak ve Nsg'ler uygulama başına trafiğine filtre uygulamak için kullanın. Bu şekilde, yalnızca 3 alt ağların her bir Vnet'teki (ön uç, uygulama katmanı ve veri katmanı) ve her alt ağ uygulaması başına bir NSG gerekir. Bu durumda, kullanmayı düşünmelisiniz **uygulama katmanı, uygulama başına Nsg'ler başına tek bir alt ağda** tasarım deseni. Aşağıdaki şekilde tasarım deseni temsil eden kullanımını gösterilmektedir **ProdBU1US1** VNet.

![Her katman, uygulama katmanı başına başına bir NSG bir alt ağ](./media/virtual-network-vnet-plan-design-arm/figure11.png)

Ancak, aynı zamanda sanal ağlar ve şirket içi veri merkezleri arasında VPN bağlantısı için fazladan bir alt ağı oluşturmanız gerekir. Ve her alt ağ için adres alanı belirtmeniz gerekir. Örnek bir çözüm için aşağıdaki şekilde gösterilmektedir **ProdBU1US1** VNet. Bu senaryo için her bir Vnet'teki çoğaltılır. Her renk farklı bir uygulama temsil eder.

![Örnek VNet](./media/virtual-network-vnet-plan-design-arm/figure10.png)

**Erişim denetimi**

Erişim denetimi için aşağıdaki gereksinimleri ilişkili:

* Şirketin ağ grubun VNet yapılandırmasını üzerinde tam denetimi olmalıdır.
* Her iş birimi geliştiriciler yalnızca varolan alt ağlara sanal makineleri dağıtamaz olmalıdır.

Bu gereksinimlerine bağlı olarak, kullanıcıların ağ ekibinden yerleşik ekleyebilirsiniz **ağ Katılımcısı** her abonelik; rolünde ve hakları vermek her Abonelikteki uygulama geliştiricileri için özel bir rol oluşturun VM'ler için var olan alt ağlar eklemek için.

## <a name="next-steps"></a>Sonraki adımlar
* [Bir sanal ağı dağıtmak](quick-create-portal.md).
* Anlamak nasıl [Yük Dengelemesi](../load-balancer/load-balancer-overview.md) Iaas Vm'leri ve [üzerinde birden fazla Azure bölgesine yönlendirmesi yönetmek](../traffic-manager/traffic-manager-overview.md).
* Daha fazla bilgi edinmek [ağ güvenlik grubu](security-overview.md) NSG çözümünü.
* Daha fazla bilgi edinmek, [şirket içi ve sanal ağ bağlantısı seçenekleri](../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti).
