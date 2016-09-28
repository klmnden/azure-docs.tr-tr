
<properties
   pageTitle="Azure sanal ağ eşlemesi | Microsoft Azure"
   description="Azure'da VNet eşlemesi hakkında bilgi edinin."
   services="virtual-network"
   documentationCenter="na"
   authors="NarayanAnnamalai"
   manager="jefco"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/28/2016"
   ms.author="narayan" />


# VNet eşlemesi

VNet eşlemesi, aynı bölgedeki iki sanal ağı Azure omurga ağı aracılığıyla birbirine bağlayan bir mekanizmadır. Eşleme yapıldıktan sonra, iki sanal ağ tüm bağlantılarda tek bir sanal ağ gibi görünür. Bunlar ayrı kaynaklar olarak yönetilmeye devam eder, ancak bu sanal ağlardaki sanal makineler özel IP adresleri kullanarak birbirleriyle doğrudan iletişim kurabilir.

Eşlenen sanal ağlardaki sanal makineler arasındaki trafik, Azure altyapısı aracılığıyla aynı sanal ağ üzerindeki VM'ler arasında olduğu gibi yönlendirilir. VNet eşlemesini kullanmanın bazı avantajları şunlardır:

- Farklı sanal ağlardaki kaynaklar arasında düşük gecikme süresi ve yüksek bant genişlikli bağlantı.
- VPN ağ geçitleri ve ağ sanal gereçleri gibi kaynakları, eşlenmiş VNet içinde geçiş noktaları olarak kullanabilme.
- Azure Resource Manager modelini kullanan bir sanal ağı, klasik modeli kullanan bir sanal ağa bağlayabilme ve bu iki sanal ağ arasında tam bağlantıya olanak tanıma.

VNet eşlemesi ile ilgili gereksinimler ve önemli noktalar:

- Eşlenen iki sanal ağ, aynı Azure bölgesinde olmalıdır.
- Eşlenen sanal ağların IP Adresi alanları çakışmamalıdır.
- VNet eşlemesi iki sanal ağ arasında gerçekleşir ve türetilmiş geçişli bir ilişki yoktur. Örneğin, A sanal ağı B sanal ağıyla, B sanal ağı da C sanal ağıyla eşlenirse bu, A sanal ağının C sanal ağıyla eşlendiği anlamına gelmez.
- İki farklı abonelikteki sanal ağlar arasında eşleme yapılabilmesi için, eşlemenin her iki aboneliğin de ayrıcalıklı bir kullanıcısı tarafından yetkilendirilmiş olması gerekir.
- Resource Manager dağıtım modelini kullanan bir sanal ağ, bu modeli kullanan başka bir sanal ağ ile veya klasik dağıtım modelini kullanan bir sanal ağ ile eşlenebilir. Ancak, klasik dağıtım modelini kullanan sanal ağlar birbiriyle eşlenemez.
- Eşlenmiş sanal ağlarda bulunan sanal makineler arasındaki iletişim başka bir bant genişliği kısıtlaması içermese de VM boyutunu temel alan bant genişliği sınırı geçerli olmaya devam eder.


![Temel VNet eşlemesi](./media/virtual-networks-peering-overview/figure01.png)

## Bağlantı
İki sanal ağ eşlendikten sonra sanal ağ üzerindeki bir sanal makine (web/çalışan rolü), eşlenen sanal ağ üzerindeki diğer sanal makinelerle doğrudan bağlantı kurabilir. Bu iki ağ da tam IP düzeyinde bağlantıya sahip olur.

Eşlenen ağlardaki iki sanal makine arasındaki bir gidiş dönüşe ilişkin ağ gecikmesi, yerel bir sanal ağ üzerindeki bir gidiş dönüş için olan ağ gecikmesiyle aynıdır. Ağ verimi, büyüklüğüne orantılı olarak sanal makine için izin verilen bant genişliğine bağlıdır. Bant genişliği ile ilgili herhangi bir ek kısıtlama yoktur.

Eşlenmiş sanal ağlarda bulunan sanal makineler arasındaki trafik bir ağ geçidi üzerinden değil, doğrudan Azure arka uç altyapısı aracılığıyla yönlendirilir.

Bir sanal ağ üzerindeki sanal makineler, eşlenen sanal ağ üzerindeki iç yükü dengelenmiş (ILB) uç noktalara erişebilir. İstendiğinde, diğer sanal ağlara veya alt ağlara erişimi engellemek için her bir sanal ağ üzerinde ağ güvenlik grupları (NSG'ler) kullanılabilir.

Kullanıcılar eşlemeyi yapılandırırken sanal ağlar arasındaki NSG kurallarını açıp kapatabilirler. Kullanıcı, eşlenen sanal ağlar arasında tam bağlantı açmayı seçerse (varsayılan seçenek) belirli alt ağlarda veya sanal makinelerde belirli erişimleri engellemek ya da reddetmek için NSG'leri kullanabilir.

Sanal makinelere yönelik Azure tarafından sağlanan iç DNS adı çözümlemesi, eşlenen sanal ağlarda kullanılamaz. Sanal makinelerin yalnızca yerel sanal ağ üzerinde çözümlenebilen iç DNS adları vardır. Ancak kullanıcılar, eşlenen sanal ağlarda çalışan sanal makineleri bir sanal ağ için DNS sunucuları olarak yapılandırabilir.

## Hizmet zinciri
Kullanıcılar, bu makalenin ilerleyen kısımlarındaki diyagramda gösterildiği şekilde; "sonraki atlama" IP adresi olarak, eşlenen sanal ağlardaki sanal makineleri işaret eden kullanıcı tanımlı yol tabloları yapılandırabilir. Bu sayede kullanıcılar, kullanıcı tanımlı yol tabloları aracılığıyla bir sanal ağ üzerindeki trafiği, eşlenen ağ üzerinde çalışan bir sanal gerece yönlendirebilecekleri bir hizmet zinciri oluşturabilir.

Ayrıca kullanıcılar, hub ve bağlı bileşen türündeki ortamları da verimli bir şekilde oluşturabilir. Bu ortamlarda hub, ağ sanal gereci gibi altyapı bileşenlerini barındırabilir. Böylece tüm bağlı sanal ağlar, bunun gibi altyapı bileşenlerinin yanı sıra hub sanal ağında çalışan gereçlere giden trafiğin bir alt ağıyla da eşlenebilir. Kısacası, VNet eşlemesi sayesinde "Kullanıcı tanımlı yol tablosundaki" sonraki atlama IP adresi, eşlenen sanal ağ üzerindeki bir sanal makinenin IP adresi olabilir.

## Ağ geçitleri ve şirket içi bağlantı
Başka bir sanal ağ ile eşlenip eşlenmediğine bakılmaksızın her sanal ağ kendi ağ geçidine sahip olabilir ve bu sanal ağ geçidini şirket içine bağlanmak için kullanılabilir. Ayrıca kullanıcılar, sanal ağlar eşlenmiş olsa bile ağ geçitlerini kullanarak [VNet'ler arası bağlantıları](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) yapılandırabilir.

Sanal ağlar arası bağlantı için her iki seçenek de yapılandırıldığında, sanal ağlar arasındaki trafik, eşleme yapılandırması (Azure omurgası) üzerinden akış gerçekleştirir.

Sanal ağlar eşlendiğinde kullanıcılar, eşlenmiş sanal ağ üzerindeki ağ geçidini şirket içine geçiş noktası olarak kullanılacak şekilde de yapılandırabilir. Bu durumda, uzak ağ geçidi kullanan sanal ağın kendi ağ geçidi olamaz. Bir sanal ağın yalnızca bir ağ geçidi olabilir. Aşağıdaki resimde gösterildiği şekilde bu, yerel bir ağ geçidi veya uzak bir ağ geçidi (eşlenen sanal ağ üzerinde) olabilir.

Resource Manager modelini ve klasik dağıtım modelini kullanan sanal ağlar arasındaki eşleme ilişkisinde ağ geçidi geçişi desteklenmez. Ağ geçidi geçişinin gerçekleşebilmesi için, eşleme ilişkisindeki her iki sanal ağ da Resource Manager dağıtım modelini kullanmalıdır.

Tek bir Azure ExpressRoute bağlantısını kullanan sanal ağlar eşlendiğinde, bu iki sanal ağ arasındaki trafik, eşleme ilişkisi (Azure omurga ağı) üzerinden akış gerçekleştirir. Kullanıcılar, şirket içi devreye bağlanmak için her bir sanal ağ üzerindeki yerel ağ geçitlerini kullanmaya devam edebilir. Alternatif olarak, paylaşılan bir ağ geçidini kullanıp şirket içi bağlantı için bir geçiş yapılandırabilirler.

![VNet eşleme geçişi](./media/virtual-networks-peering-overview/figure02.png)

## Sağlama
VNet eşlemesi ayrıcalıklı bir işlemdir. VirtualNetworks ad alanı altında yer alan ayrı bir işlevdir. Bir kullanıcıya eşlemeyi yetkilendirmesi için belirli haklar verilebilir. Sanal ağa yönelik okuma/yazma erişimi olan bir kullanıcı bu haklara otomatik olarak sahip olur.

Eşleme özelliğinin yönetici ya da ayrıcalıklı kullanıcısı olan bir kullanıcı, başka bir VNet üzerinde eşleme işlemi başlatabilir. Diğer tarafta eşleme için eşleşen bir istek varsa ve diğer gereksinimler karşılanırsa eşleme gerçekleştirilir.

İki sanal ağ arasında VNet eşlemesinin nasıl gerçekleştirileceğiyle ilgili daha fazla bilgi edinmek için "Sonraki adımlar" bölümünde yer alan makalelere göz atın.

## Sınırlar
Tek bir sanal ağ için izin verilen eşleme sayısı sınırlıdır. Daha fazla bilgi edinmek için bkz. [Azure ağ sınırları](../azure-subscription-service-limits.md#networking-limits).

## Fiyatlandırma
VNet eşlemesi, gözden geçirme süresi boyunca ücretsiz olacaktır. Piyasaya sürüldükten sonra, eşlemeyi kullanan giriş ve çıkış trafiği için düşük miktarda bir ücret alınacaktır. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/virtual-network).


## Sonraki adımlar
- [Sanal ağlar arasında eşlemeyi ayarlama](virtual-networks-create-vnetpeering-arm-portal.md).
- [NSG'ler](virtual-networks-nsg.md) hakkında bilgi edinin.
- [Kullanıcı tanımlı yollar ve IP iletimi](virtual-networks-udr-overview.md) hakkında bilgi edinin.



<!--HONumber=Sep16_HO3-->


