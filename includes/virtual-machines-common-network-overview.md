---
title: include dosyası
description: include dosyası
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 11/01/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: cd3b7d6cc75afc5d83ff02a15b920d9f8b05f608
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66391390"
---
Azure sanal makinesi (VM) oluştururken bir [sanal ağ](../articles/virtual-network/virtual-networks-overview.md) (VNet) oluşturmanız ya da mevcut bir VNet’i kullanmanız gerekir. Sanal ağda VM’lerinize nasıl erişilmesini istediğinize de karar vermeniz gerekir. [Kaynakları oluşturmadan önce planlama yapmak](../articles/virtual-network/virtual-network-vnet-plan-design-arm.md) ve [ağ kaynaklarının sınırlarını](../articles/azure-subscription-service-limits.md#networking-limits) anladığınızdan emin olmak önemlidir.

Aşağıdaki şekilde VM’ler web sunucuları ve veritabanı sunucuları olarak temsil edilmektedir. Her VM kümesi, sanal ağdaki farklı alt ağlara atanmıştır.

![Azure sanal ağı](./media/virtual-machines-common-network-overview/vnetoverview.png)

Bir VM oluştururken kullanabilirsiniz ya da bir VM oluşturmadan önce bir sanal ağ oluşturabilirsiniz. Bir VM ile iletişimi desteklemek için şu kaynakları oluşturursunuz:

- Ağ arabirimleri
- IP adresleri
- Sanal ağ ve alt ağlar

Bu temel kaynaklara ek olarak şu isteğe bağlı kaynakları da göz önünde bulundurmalısınız:

- Ağ güvenlik grupları
- Yük dengeleyiciler 

[!INCLUDE [updated-for-az](./updated-for-az.md)]

## <a name="network-interfaces"></a>Ağ arabirimleri

[Ağ arabirimi (NIC)](../articles/virtual-network/virtual-network-network-interface.md), bir VM ile bir sanal ağ (VNet) arasındaki çift yönlü bağlantıdır. Bir VM en az bir NIC içermelidir, ancak oluşturduğunuz VM’nin boyutuna bağlı olarak birden fazla NIC içerebilir. Her VM kaç NIC hakkında bilgi edinmek için boyutu destekler [Windows](../articles/virtual-machines/windows/sizes.md) veya [Linux](../articles/virtual-machines/linux/sizes.md).

Birden çok NIC ile VM oluşturma ve ekleyebilir veya NIC VM yaşam döngüsü ile kaldırabilirsiniz. Birden çok NIC, farklı alt ağlara bağlanmak ve gönderme veya en uygun arabirimi üzerinden trafiği almak için bir VM izin verir. Herhangi bir sayıda ağ arabirimine sahip VM'ler aynı kullanılabilirlik kümesinde en fazla sanal makine boyutu tarafından desteklenen sayıyı bulunabilir. 

Bir VM’ye bağlı her NIC’nin VM ile aynı konum ve abonelikte bulunması gerekir. Her NIC’nin NIC ile aynı Azure konumunda ve aboneliğinde bulunan bir VNet’e bağlanması gerekir. Oluşturulduktan sonra bir VM'nin bağlı olduğu alt ağ değiştirebilir, ancak Vnet'i değiştiremezsiniz. Bir VM’ye bağlı her NIC’ye VM silinene kadar değişmeyen bir MAC adresi atanır.

Bu tabloda bir ağ arabirimi oluşturmak için kullanabileceğiniz yöntemler listelenmiştir.

| Yöntem | Açıklama |
| ------ | ----------- |
| Azure portal | Azure portalında bir VM oluşturduğunuzda, sizin için otomatik olarak bir ağ arabirimi oluşturulur (ayrı olarak oluşturduğunuz bir NIC’yi kullanamazsınız). Portal yalnızca tek NIC’li bir VM oluşturur. Birden fazla NIC içeren bir VM oluşturmak istiyorsanız VM’yi farklı bir yöntemle oluşturmanız gerekir. |
| [Azure PowerShell](../articles/virtual-machines/windows/multiple-nics.md) | Kullanım [yeni AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/new-aznetworkinterface) ile **- Publicıpaddressıd** daha önce oluşturduğunuz genel IP adresinin tanımlayıcısını sağlamak için parametre. |
| [Azure CLI](../articles/virtual-machines/linux/multiple-nics.md) | Daha önce oluşturduğunuz genel IP adresinin tanımlayıcısını sağlamak için kullanın [az ağ NIC oluşturup](https://docs.microsoft.com/cli/azure/network/nic) ile **--public-ip-address** parametresi. |
| [Şablon](../articles/virtual-network/template-samples.md) | Bir şablon kullanarak ağ arabirimi dağıtmak için [Genel IP Adresli bir Sanal Ağda Ağ Arabirimi](https://github.com/Azure/azure-quickstart-templates/tree/master/101-nic-publicip-dns-vnet) konusunu bir kılavuz olarak kullanın. |

## <a name="ip-addresses"></a>IP adresleri 

Azure’daki bir NIC’ye şu tür [IP adresleri](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md) atayabilirsiniz:

- **Genel IP adresleri**: İnternet’le ve bir VNet’e bağlı olmayan diğer Azure kaynaklarıyla gelen ve giden (ağ adresi çevirisi (NAT) olmadan) iletişimleri gerçekleştirmek için kullanılır. Bir NIC’ye genel IP adresi atanıp atanmayacağı isteğe bağlıdır. Genel IP adreslerinin nominal bir ücret vardır ve abonelik başına kullanılabilecek bir sayısı yoktur.
- **Özel IP adresleri**: Bir VNet, şirket içi ağınız ve İnternet (NAT ile) içerisinde iletişim için kullanılır. VM’ye en az bir özel IP adresi atamalısınız. Azure’da NAT ile ilgili bilgi edinmek için [Azure’da giden bağlantıları anlama](../articles/load-balancer/load-balancer-outbound-connections.md) konusunu okuyun.

VM’lere veya internet’e yönelik yük dengeleyicilere genel IP adresleri atayabilirsiniz. VM’lere ve iç yük dengeleyicilere özel IP adresleri atayabilirsiniz. VM’ye IP adresleri atamak için bir ağ arabirimi kullanılır.

Bir kaynağa IP adresi ayırmak için iki yöntem vardır: Dinamik veya statik. Varsayılan ayırma yöntemi, bir IP adresinin oluşturulduğu sırada ayrılmadığı yöntem olan dinamik yöntemdir. Bunun yerine, IP adresi bir VM oluşturduğunuzda bir VM’yi durdurduğunuzda ayrılır. VM’yi durdurduğunuzda veya sildiğinizde IP adresi serbest kalır. 

VM’nin IP adresinin aynı kalmasını sağlamak için ayırma yöntemini açıkça statik olarak ayarlayabilirsiniz. Bu durumda, anında bir IP adresi atanır. Adres, yalnızca VM’yi sildiğinizde veya ayırma yöntemini dinamik olarak değiştirdiğinizde serbest kalır.
    
Bu tabloda bir IP adresi oluşturmak için kullanabileceğiniz yöntemler listelenmiştir.

| Yöntem | Açıklama |
| ------ | ----------- |
| [Azure portal](../articles/virtual-network/virtual-network-deploy-static-pip-arm-portal.md) | Varsayılan olarak, genel IP adresleri dinamiktir ve VM durdurulduğunda ya da silindiğinde VM ile ilişkili adres değişebilir. VM’nin her zaman aynı genel IP adresini kullanmasını sağlamak için statik bir genel IP adresi oluşturun. Varsayılan olarak, bir VM oluşturulurken NIC’ye portal tarafından dinamik özel IP adresi atanır. VM oluşturulduktan sonra bu IP adresi statik olarak değiştirebilirsiniz.|
| [Azure PowerShell](../articles/virtual-network/virtual-network-deploy-static-pip-arm-ps.md) | Kullandığınız [yeni AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress) ile **- AllocationMethod** parametresi dinamik veya statik olarak. |
| [Azure CLI](../articles/virtual-network/virtual-network-deploy-static-pip-arm-cli.md) | [az network public-ip create](https://docs.microsoft.com/cli/azure/network/public-ip) komutunu **--allocation-method** parametresi Dinamik veya Statik olacak şekilde kullanırsınız. |
| [Şablon](../articles/virtual-network/template-samples.md) | Bir şablon kullanarak genel IP adresi dağıtmak için [Genel IP Adresli bir Sanal Ağda Ağ Arabirimi](https://github.com/Azure/azure-quickstart-templates/tree/master/101-nic-publicip-dns-vnet) konusunu bir kılavuz olarak kullanın. |

Genel bir IP adresi oluşturduktan sonra bu adresi bir NIC’ye atayarak bir VM ile ilişkilendirebilirsiniz.

## <a name="virtual-network-and-subnets"></a>Sanal ağ ve alt ağlar

Alt ağ, sanal ağ içindeki bir IP adresleri aralığıdır. Bir sanal ağı organizasyon ve güvenlik için birden çok alt ağa bölebilirsiniz. Bir VM’deki her NIC, bir VNet’teki bir alt ağa bağlanır. Bir VNet içindeki alt ağlara (aynı veya farklı) bağlı NIC’ler, ek bir yapılandırma gerektirmeden birbirleriyle iletişim kurabilir.

Bir VNet ayarlarken kullanılabilir adres alanları ve alt ağlar da dahil olmak üzere ağın topolojisini siz belirtirsiniz. VNet diğer VNet’lere veya şirket içi ağlara bağlanacaksa birbiriyle çakışmayan adres aralıkları seçmeniz gerekir. IP adresleri özeldir ve yalnızca 10.0.0.0/8, 172.16.0.0/12 veya 192.168.0.0/16 gibi yönlendirilemeyen IP adresleri için true Internet'ten erişilemez. Azure artık tüm adres aralıklarını yalnızca VNet içerisinden, birbirine bağlı VNet’lerden ve şirket içi konumunuzdan erişilebilen özel VNet IP adresi alanının bir parçası olarak görür. 

İç ağlardan başka birinin sorumlu olduğu bir kurumda çalışıyorsanız adres alanınızı seçmeden önce bu kişiyle konuşmalısınız. Herhangi bir çakışma olmadığından emin olun ve sorumlu kişinin aynı IP adresi aralığını kullanmaya çalışmaması için bu kişiye hangi alanı kullanmak istediğinizi bildirin. 

Varsayılan olarak alt ağlar arasında herhangi bir güvenlik sınırı olmadığından, bu alt ağların her birindeki VM’ler birbiriyle iletişim kurabilir. Bununla birlikte, alt ağların ve VM’lerin birbirleri arasındaki trafik akışını denetlemenize imkan sağlayan Ağ Güvenlik Grupları (NSG) ayarlayabilirsiniz. 

Bu tabloda, bir VNet ve alt ağlar oluşturmak için kullanabileceğiniz yöntemler listelenmiştir. 

| Yöntem | Açıklama |
| ------ | ----------- |
| [Azure portal](../articles/virtual-network/quick-create-portal.md) | Bir VM oluştururken VNet oluşturma işlemini Azure’a bırakırsanız, ad VNet’i ve **-vnet**’i içeren kaynak grubunun adının bir birleşimi olur. Adres alanı 10.0.0.0/24, gerekli alt ağ adı **default** ve alt ağ adres aralığı 10.0.0.0/24’tür. |
| [Azure PowerShell](../articles/virtual-network/quick-create-powershell.md) | Kullandığınız [yeni AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworkSubnetConfig) ve [yeni AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork) bir alt ağ ve VNet oluşturmak için. Ayrıca [Ekle AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/Az.Network/Add-AzVirtualNetworkSubnetConfig) bir alt ağı mevcut bir Vnet'e eklemek için. |
| [Azure CLI](../articles/virtual-network/quick-create-cli.md) | Alt ağ ve VNet aynı anda oluşturulur. [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet) komutuna alt ağın adını içeren bir **--subnet-name** parametresi sağlayın. |
| Şablon | Mevcut bir şablonu gibi indirmek için bir VNet ve alt ağlar oluşturmanın en kolay yolu olan [iki alt ağlı sanal ağ](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets)ve gereksinimlerinize göre değiştirebilirsiniz. |

## <a name="network-security-groups"></a>Ağ güvenlik grupları

[Ağ güvenlik grubu (NSG)](../articles/virtual-network/virtual-network-vnet-plan-design-arm.md), alt ağlara, NIC’lere veya her ikisine yönelik ağ trafiğine izin veren veya trafiği reddeden Erişim Denetimi Listesi (ACL) kurallarının bir listesini içerir. NSG’ler alt ağlarla veya bir alt ağa bağlı tekil NIC’lerle örnekleriyle ilişkili olabilir. NSG bir alt ağ ile ilişkili olduğunda ACL kuralları bu alt ağdaki tüm VM’ler için geçerli olur. Ayrıca, tekil bir NIC’ye yönelik trafik, bir NSG’nin doğrudan bu NIC ile ilişkilendirilmesi yoluyla sınırlanabilir.

NSG'ler iki kural kümesi içerir: gelen ve giden. Bir kurala ait öncelik her küme içinde benzersiz olmalıdır. Her kuralın protokol, kaynak ve hedef bağlantı noktası aralıkları, adres ön ekleri, trafik yönü, öncelik ve erişim türü özellikleri vardır. 

Tüm NSG'ler bir varsayılan kurallar kümesini içerir. Varsayılan kurallar silinemez ancak en düşük önceliğe atanmış oldukları için sizin oluşturduğunuz kurallar tarafından geçersiz kılınabilirler. 

Bir NSG'yi bir NIC ile ilişkilendirdiğinizde, NSG'deki ağ erişim kuralları yalnızca bu NIC'ye uygulanır. Çok NIC’li bir VM’de bir NSG tek bir NIC’ye uygulandığı zaman diğer NIC’lere giden trafik etkilenmez. Farklı NSG’leri bir NIC ile (veya dağıtım modeline bağlı olarak VM ile) ve NIC’nin veya VM’nin bağlı olduğu ağ ile ilişkilendirebilirsiniz. Trafiğin yönüne bağlı olarak öncelik verilir.

VM’leriniz ve VNet’inizle ilgili plan yaptığınız sırada NSG’lerinizi de mutlaka [planlayın](../articles/virtual-network/virtual-network-vnet-plan-design-arm.md).

Bu tabloda bir ağ güvenlik grubu oluşturmak için kullanabileceğiniz yöntemler listelenmiştir.

| Yöntem | Açıklama |
| ------ | ----------- |
| [Azure portal](../articles/virtual-network/tutorial-filter-network-traffic.md) | Azure portalında bir VM oluşturduğunuzda otomatik olarak bir NSG oluşturulur ve portalın oluşturduğu NIC ile ilişkilendirilir. NSG’nin adı, VM’nin adı ile **-nsg**’nin birleşiminde oluşur. Bu NSG, önceliği 1000 olan, hizmeti RDP olarak, protokolü TCP olarak, bağlantı noktası 3389 olarak ve eylemi İzin ver olarak ayarlanmış bir gelen kural içerir. VM’ye yönelik başka bir gelen trafiğe izin vermek istiyorsanız NSG’ye ek kurallar eklemeniz gerekir. |
| [Azure PowerShell](../articles/virtual-network/tutorial-filter-network-traffic.md) | Kullanım [yeni AzNetworkSecurityRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecurityruleconfig) ve gerekli kural bilgilerini sağlayın. Kullanım [yeni AzNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecuritygroup) NSG'yi oluşturmak için. Kullanım [kümesi AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/set-azvirtualnetworksubnetconfig) alt ağı için NSG yapılandırmak için. Kullanım [kümesi AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/set-azvirtualnetwork) NSG'yi Vnet'e eklemek için. |
| [Azure CLI](../articles/virtual-network/tutorial-filter-network-traffic-cli.md) | NSG’yi ilk olarak oluşturmak için [az network nsg create](https://docs.microsoft.com/cli/azure/network/nsg) komutunu kullanın. NSG’ye kural eklemek için [az network nsg rule create](https://docs.microsoft.com/cli/azure/network/nsg/rule) komutunu kullanın. NSG’yi alt ağa eklemek için [az network vnet subnet update](https://docs.microsoft.com/cli/azure/network/vnet/subnet) komutunu kullanın. |
| [Şablon](../articles/virtual-network/template-samples.md) | Bir şablon kullanarak ağ güvenlik grubu dağıtmak için [Ağ Güvenlik Grubu Oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-security-group-create) konusunu kılavuz olarak kullanın. |

## <a name="load-balancers"></a>Yük dengeleyiciler

[Azure Load Balancer](../articles/load-balancer/load-balancer-overview.md), uygulamalarınıza yüksek düzeyde kullanılabilirlik ve ağ performansı sağlar. Bir yük dengeleyici, VM’lere yönelik [gelen İnternet trafiğini dengeleme](../articles/load-balancer/load-balancer-internet-overview.md) veya [bir VNet’teki VM’ler arasında trafiği dengeleme](../articles/load-balancer/load-balancer-internal-overview.md) rolünü üstlenecek şekilde yapılandırılabilir. Ayrıca, bir yük dengeleyici şirket içi bilgisayarlar ile şirketler arası VM’ler arasındaki trafiği dengeleme ya da dış trafiği belirli bir VM’ye yönlendirme rollerini üstlenebilir.

Yük dengeleyici, genel IP adresi ile yük dengeleyicideki bağlantı noktası arasında ve özel IP adresi ile VM’nin bağlantı noktası arasında gelen ve giden trafiği eşler.

Bir yük dengeleyici oluştururken şu yapılandırma öğelerini de dikkate almanız gerekir:

- **Ön uç IP yapılandırması**: Bir yük dengeleyici, sanal IP’ler (VIP) olarak da bilinen bir veya daha fazla ön uç IP adresi içerebilir. Bu IP adresleri trafik için bir giriş işlevi görür.
- **Arka uç adres havuzu**: Yükün dağıtıldığı NIC ile ilişkili IP adresleri.
- **NAT kuralları**: Gelen trafik akışının ön uç IP’sinden geçip arka uç IP’sine nasıl dağıtılacağını tanımlar.
- **Yük dengeleyici kuralları**: Belirli bir ön uç IP’si ve bağlantı noktası birleşimini bir dizi arka uç IP adresi ve bağlantı noktası bileşimiyle eşler. Tek bir yük dengeleyici birden çok dengeleme kuralına sahip olabilir. Her kural, bir ön uç IP’si ile bağlantı noktasının ve arka uç IP’si ile VM’lerle ilişkilendirilmiş bağlantı noktasının birleşiminden oluşur.
- **[Araştırmalar](../articles/load-balancer/load-balancer-custom-probe-overview.md)** : VM’lerin durumunu izler. Bir araştırma yanıt veremediğinde, yük dengeleyici sağlıksız VM’ye yeni bağlantı göndermeyi durdurur. Mevcut bağlantılar bu durumdan etkilenmez ve yeni bağlantılar sağlıklı VM’lere gönderilir.

Bu tabloda İnternet’e yönelik bir yük dengeleyici oluşturmak için kullanabileceğiniz yöntemler listelenmiştir.

| Yöntem | Açıklama |
| ------ | ----------- |
| Azure portal |  Yapabilecekleriniz [Yük Dengelemesi Azure portalını kullanarak sanal makineleri internet trafiğini](../articles/load-balancer/tutorial-load-balancer-standard-manage-portal.md). |
| [Azure PowerShell](../articles/load-balancer/load-balancer-get-started-internet-arm-ps.md) | Daha önce oluşturduğunuz genel IP adresinin tanımlayıcısını sağlamak için kullanın [yeni AzLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerfrontendipconfig) ile **- Publicıpaddress** parametresi. Kullanım [yeni AzLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig) arka uç adres havuzunun yapılandırmasını oluşturmak için. Kullanım [yeni AzLoadBalancerInboundNatRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerinboundnatruleconfig) oluşturduğunuz ön uç IP yapılandırmasıyla ilişkili gelen NAT kuralları oluşturmak için. Kullanım [yeni AzLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerprobeconfig) ihtiyacınız olan araştırmaları oluşturmak için. Kullanım [yeni AzLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerruleconfig) yük dengeleyici yapılandırmasını oluşturmak için. Kullanım [yeni AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancer) yük dengeleyici oluşturmanız gerekir.|
| [Azure CLI](../articles/load-balancer/load-balancer-get-started-internet-arm-cli.md) | İlk yük dengeleyici yapılandırmasını oluşturmak için [az network lb create](https://docs.microsoft.com/cli/azure/network/lb) komutunu kullanın. Daha önce oluşturduğunuz genel IP adresini eklemek için [az network lb frontend-ip create](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip) komutunu kullanın. Arka uç adres havuzunun yapılandırmasını eklemek için [az network lb address-pool create](https://docs.microsoft.com/cli/azure/network/lb/address-pool) komutunu kullanın. NAT kuralları eklemek için [az network lb inbound-nat-rule create](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule) komutunu kullanın. Yük dengeleyici kurallarını eklemek için [az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule) komutunu kullanın. Araştırmaları eklemek için [az network lb probe create](https://docs.microsoft.com/cli/azure/network/lb/probe) komutunu kullanın. |
| [Şablon](../articles/load-balancer/load-balancer-get-started-internet-arm-template.md) | Bir şablon kullanarak yük dengeleyici dağıtmak için [Bir Yük Dengeleyici’de 2 VM ve LB’de NAT kuralları yapılandırma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-natrules) konusunu kılavuz olarak kullanın. |
    
Bu tabloda bir iç yük dengeleyici oluşturmak için kullanabileceğiniz yöntemler listelenmiştir.

| Yöntem | Açıklama |
| ------ | ----------- |
| Azure portal | Yapabilecekleriniz [Azure portalında bir temel yük dengeleyici ile iç trafik Yük Dengeleme](../articles/load-balancer/tutorial-load-balancer-basic-internal-portal.md). |
| [Azure PowerShell](../articles/load-balancer/load-balancer-get-started-ilb-arm-ps.md) | Ağ alt ağında özel bir IP adresi sağlamak için kullanın [yeni AzLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerfrontendipconfig) ile **- Privateıpaddress** parametresi. Kullanım [yeni AzLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig) arka uç adres havuzunun yapılandırmasını oluşturmak için. Kullanım [yeni AzLoadBalancerInboundNatRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerinboundnatruleconfig) oluşturduğunuz ön uç IP yapılandırmasıyla ilişkili gelen NAT kuralları oluşturmak için. Kullanım [yeni AzLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerprobeconfig) ihtiyacınız olan araştırmaları oluşturmak için. Kullanım [yeni AzLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerruleconfig) yük dengeleyici yapılandırmasını oluşturmak için. Kullanım [yeni AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancer) yük dengeleyici oluşturmanız gerekir.|
| [Azure CLI](../articles/load-balancer/load-balancer-get-started-ilb-arm-cli.md) | İlk yük dengeleyici yapılandırmasını oluşturmak için [az network lb create](https://docs.microsoft.com/cli/azure/network/lb) komutunu kullanın. Özel IP adresini tanımlamak için [az network lb frontend-ip create](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip) komutunu **--private-ip-address** parametresiyle birlikte kullanın. Arka uç adres havuzunun yapılandırmasını eklemek için [az network lb address-pool create](https://docs.microsoft.com/cli/azure/network/lb/address-pool) komutunu kullanın. NAT kuralları eklemek için [az network lb inbound-nat-rule create](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule) komutunu kullanın. Yük dengeleyici kurallarını eklemek için [az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule) komutunu kullanın. Araştırmaları eklemek için [az network lb probe create](https://docs.microsoft.com/cli/azure/network/lb/probe) komutunu kullanın.|
| [Şablon](../articles/load-balancer/load-balancer-get-started-ilb-arm-template.md) | Bir şablon kullanarak yük dengeleyici dağıtmak için [Bir Yük Dengeleyici’de 2 VM ve LB’de NAT kuralları yapılandırma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer) konusunu kılavuz olarak kullanın. |

## <a name="vms"></a>VM'ler

VM’ler aynı sanal ağda oluşturulabilir ve özel IP adreslerini kullanarak birbirine bağlanabilir. VM’ler, farklı alt ağlarda olsalar bile bir ağ geçidi yapılandırma ya da genel IP adresleri yapılandırma gerekstirmeden birbirine bağlanabilir. VM’leri bir VNet’e eklemek için VNet’i oluşturursunuz ve her VM’yi oluşturduğunuz sırada VNet’e ve alt ağa atarsınız. VM’ler ağ ayarlarını dağıtım veya başlatma sırasında alır.  

VM’ler dağıtıldığı sırada VM’lere bir IP adresi atanır. Bir VNet veya alt ağa birden çok VM dağıtırsanız, bunlara başlatıldıkları sırada IP adresleri atanır. Ayrıca, bir VM'ye statik bir IP ayırabilirsiniz. Statik IP ayırdığınızda, bir statik IP başka bir VM için yanlışlıkla kaçınmak için belirli bir alt ağ kullanmayı düşünmeniz gerekir.  

Bir VM oluşturur ve daha sonra bu VM’yi bir VNet’e geçirmek isterseniz bu işlem basit bir yapılandırma değişikliği değildir. VM’yi yeniden VNet’e dağıtmanız gerekir. Yeniden dağıtmanın en kolay yolu VM’ye bağlı diskleri silmeden VM’yi silmek ve özgün diskleri kullanarak VM’yi VNet’te yeniden oluşturmaktır. 

Bu tabloda, bir VNet’te VM oluşturmak için kullanabileceğiniz yöntemler listelenmiştir.

| Yöntem | Açıklama |
| ------ | ----------- |
| [Azure portal](../articles/virtual-machines/windows/quick-create-portal.md) | Daha önce bahsedilen varsayılan ayarlarını kullanarak tek NIC’li bir VM oluşturur. Birden çok NIC içeren bir VM oluşturmak için farklı bir yöntem kullanmalısınız. |
| [Azure PowerShell](../articles/virtual-machines/windows/tutorial-manage-vm.md) | Kullanılmasını [Ekle AzVMNetworkInterface](https://docs.microsoft.com/powershell/module/az.compute/add-azvmnetworkinterface) daha önce oluşturduğunuz NIC'nin VM yapılandırmasına eklemek için. |
| [Azure CLI](../articles/virtual-machines/linux/create-cli-complete.md) | Oluşturun ve bir Vnet, alt ağ ve her bir adım derleme NIC VM bağlanın. |
| [Şablon](../articles/virtual-machines/windows/ps-template.md) | Bir şablon kullanarak VM dağıtmak için [Çok basit Windows VM dağıtımı](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows) konusunu kılavuz olarak kullanın. |

## <a name="next-steps"></a>Sonraki adımlar
VM'ler için Azure sanal ağları yönetme hakkında sanal Makineye özgü adımlar için bkz: [Windows](../articles/virtual-machines/windows/tutorial-virtual-network.md) veya [Linux](../articles/virtual-machines/linux/tutorial-virtual-network.md) öğreticiler.

Ayrıca vardır öğreticiler Vm'lerde Yük Dengeleme ve yüksek oranda kullanılabilir uygulamalar için oluşturma konusunda [Windows](../articles/virtual-machines/windows/tutorial-load-balancer.md) veya [Linux](../articles/virtual-machines/linux/tutorial-load-balancer.md).

- [Kullanıcı tanımlı yollar ve IP iletimi](../articles/virtual-network/virtual-networks-udr-overview.md) yapılandırma hakkında bilgi edinin. 
- [VNet’ten VNet’e bağlantılar](../articles/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) yapılandırma hakkında bilgi edinin.
- [Yollarla ilgili sorunları giderme](../articles/virtual-network/diagnose-network-routing-problem.md) hakkında bilgi edinin.
