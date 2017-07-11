---
title: "Azure Sanal Ağ eşlemesi | Microsoft Belgeleri"
description: "Azure'daki sanal ağ eşlemesi hakkında bilgi edinin."
services: virtual-network
documentationcenter: na
author: NarayanAnnamalai
manager: jefco
editor: tysonn
ms.assetid: eb0ba07d-5fee-4db0-b1cb-a569b7060d2a
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/06/2017
ms.author: narayan
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: bfba85160cd02baa337881653dbe6582d10ba073
ms.contentlocale: tr-tr
ms.lasthandoff: 06/28/2017


---
<a id="virtual-network-peering" class="xliff"></a>

# Sanal ağ eşleme
Sanal ağ eşlemesi, Azure omurga ağı aracılığıyla aynı bölgedeki iki sanal ağı birbirine bağlamanızı sağlar. Eşleme yapıldıktan sonra, bağlantı açısından iki sanal ağ tek bir sanal ağ gibi görünür. Bu iki sanal ağ ayrı kaynaklar olarak yönetilmeye devam eder, ancak eşlenen sanal ağlardaki sanal makineler özel IP adresleri kullanarak birbirleriyle doğrudan iletişim kurabilir.

Eşlenen sanal ağlarda bulunan sanal makineler arasındaki trafik, Azure altyapısı aracılığıyla aynı sanal ağdaki sanal makineler arasında olduğu gibi yönlendirilir. Sanal ağ eşlemesini kullanmanın bazı avantajları şunlardır:

* Farklı sanal ağlardaki kaynaklar arasında düşük gecikme süresi ve yüksek bant genişlikli bağlantı.
* VPN ağ geçitleri ve ağ sanal gereçleri gibi kaynakları, eşlenmiş sanal ağ içinde geçiş noktaları olarak kullanabilme özelliği.
* Azure Resource Manager dağıtım modeliyle oluşturulan iki sanal ağı eşleyebilme veya Resource Manager ile oluşturulan bir sanal ağı klasik dağıtım modeliyle oluşturulan sanal ağ ile eşleyebilme özelliği. İki Azure dağıtım modeli arasındaki fark hakkında daha fazla bilgi almak için [Azure dağıtım modellerini anlama](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) makalesini okuyun.

Sanal ağ eşlemesi ile ilgili gereksinimler ve önemli noktalar:

* Eşlenmiş sanal ağlar aynı Azure bölgesinde bulunmalıdır. Farklı Azure bölgelerindeki sanal ağları bir [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) kullanarak birbirine bağlayabilirsiniz.
* Eşlenmiş sanal ağların IP adresi alanları çakışmamalıdır.
* Bir sanal ağ başka bir sanal ağla eşlendikten sonra sanal ağa adres alanı eklenemez veya ağdaki bir adres alanı silinemez.
* Sanal ağ eşlemesi iki sanal ağ arasında gerçekleşir. Eşlemeler arasında türetilmiş geçişli bir ilişki yoktur. Örneğin, virtualNetworkA ile virtualNetworkB; virtualNetworkB ile de virtualNetworkC eşlenirse, virtualNetwork A ile virtualNetworkC arasında eşleme *yoktur*.
* Eşlemenin her iki aboneliğin de ayrıcalıklı bir kullanıcısı tarafından yetkilendirilmiş olması ve aboneliklerin aynı Azure Active Directory kiracısı ile ilişkilendirilmesi şartıyla, iki farklı abonelikte mevcut olan sanal ağları eşleyebilirsiniz. Farklı Active Directory kiracılarıyla ilişkili aboneliklerdeki sanal ağları bağlamak için bir [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) kullanabilirsiniz.
* Her iki sanal ağ da Resource Manager dağıtım modeliyle oluşturulursa veya biri Resource Manager dağıtım modeliyle, diğeri klasik dağıtım modeliyle oluşturulursa sanal ağlar eşlenebilir. Ancak, klasik dağıtım modeliyle oluşturulan iki sanal ağ birbiriyle eşlenemez. Farklı dağıtım modelleriyle oluşturulan sanal ağlar eşlenirken, sanal ağların her ikisi de *aynı* abonelikte olmalıdır. *Farklı* aboneliklerde mevcut olan ve farklı dağıtım modelleriyle oluşturulmuş sanal ağları eşleyebilme özelliği, **önizleme** sürümündedir. Daha fazla bilgi edinmek için [Sanal ağ eşlemesi oluşturma](virtual-network-create-peering.md#different-subscriptions-different-deployment-models) makalesini okuyun.
* Eşlenmiş sanal ağlardaki sanal makineler arasında kurulan iletişim için ek bant genişliği kısıtlamaları olmasa da, sanal makine boyutuna bağlı olarak hala geçerli olan bir ağ bant genişliği üst sınırı vardır. Farklı sanal makine boyutlarına yönelik ağ bant genişliği üst sınırları hakkında daha fazla bilgi edinmek için [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makine boyutları makalelerini okuyun.

![Temel sanal ağ eşleme](./media/virtual-networks-peering-overview/figure01.png)

<a id="connectivity" class="xliff"></a>

## Bağlantı
İki sanal ağ eşlendikten sonra, sanal ağdaki sanal makineler veya Cloud Services rolleri, eşlenen sanal ağdaki diğer kaynaklarla doğrudan bağlantı kurabilir. İki sanal ağ da tam IP düzeyinde bağlantıya sahip olur.

Eşlenen ağlardaki iki sanal makine arasındaki bir gidiş dönüşe ilişkin ağ gecikme süresi, tek bir sanal ağdaki bir gidiş dönüş için olan ağ gecikme süresiyle aynıdır. Ağ verimi, büyüklüğüne orantılı olarak sanal makine için izin verilen bant genişliğine bağlıdır. Eşleme içindeki bant genişliği ile ilgili herhangi bir ek kısıtlama yoktur.

Eşlenmiş sanal ağlarda bulunan sanal makineler arasındaki trafik bir ağ geçidi üzerinden değil, doğrudan Azure arka uç altyapısı aracılığıyla yönlendirilir.

Bir sanal ağa bağlı sanal makineler, eşlenen sanal ağdaki iç yükü dengelenmiş uç noktalara erişebilir. İstendiğinde, diğer sanal ağlara veya alt ağlara erişimi engellemek için her bir sanal ağda ağ güvenlik grupları uygulanabilir.

Sanal ağ eşlemesi yapılandırırken, sanal ağlar arasındaki ağ güvenlik grubu kurallarını açabilir veya kapatabilirsiniz. Eşlenen sanal ağlar arasında tam bağlantıyı (varsayılan seçenek) açarsanız, belirli erişimleri engellemek ya da reddetmek için belirli alt ağlara veya sanal makinelere ağ güvenlik grupları uygulayabilirsiniz. Ağ güvenlik grupları hakkında daha fazla bilgi edinmek için [Ağ güvenlik gruplarına genel bakış](virtual-networks-nsg.md) makalesini okuyun.

Sanal makinelere yönelik Azure tarafından sağlanan iç DNS adı çözümlemesi, eşlenen sanal ağlarda kullanılamaz. Sanal makinelerin yalnızca yerel sanal ağ üzerinde çözümlenebilen iç DNS adları vardır. Bununla birlikte, eşlenen sanal ağlara bağlı sanal makineleri bir sanal ağ için DNS sunucuları olarak yapılandırabilirsiniz. Daha ayrıntılı bilgi edinmek için [Kendi DNS sunucunuzu kullanarak ad çözümlemesi](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) makalesini okuyun.

<a id="service-chaining" class="xliff"></a>

## Hizmet zinciri
Hizmet zinciri oluşturmayı etkinleştirmek için işlenen sanal ağlardaki sanal makineleri "sonraki atlama" IP adresi olarak işaret eden kullanıcı tanımlı yollar yapılandırabilirsiniz. Hizmet zinciri oluşturma, kullanıcı tanımlı yollar aracılığıyla trafiği bir sanal ağdan eşlenmiş bir sanal ağdaki bir sanal gerece yönlendirmenize imkan tanır.

Ayrıca, hub ve bağlı bileşen türündeki ortamları da verimli bir şekilde oluşturabilirsiniz. Bu ortamlarda hub, ağ sanal gereci gibi altyapı bileşenlerini barındırabilir. Daha sonra, tüm ağlı sanal ağlar merkezi sanal ağla eşlenebilir. Trafik, merkezi sanal ağda çalışan ağ sanal gereçleri üzerinden akabilir. Kısacası, sanal ağ eşlemesi sayesinde, kullanıcı tanımlı yolda bir sonraki atlama IP adresi, eşlenen sanal ağdaki bir sanal makinenin IP adresi olabilir. Kullanıcı tanımlı yollar hakkında daha fazla bilgi için, [kullanıcı tanımlı yollara genel bakış](virtual-networks-udr-overview.md) makalesini okuyun.

<a id="gateways-and-on-premises-connectivity" class="xliff"></a>

## Ağ geçitleri ve şirket içi bağlantı
Her sanal ağ başka bir sanal ağ ile eşlenip eşlenmediğine bakılmaksızın kendi ağ geçidine sahip olabilir ve bu sanal ağ geçidini şirket içi bir ağa bağlanmak için kullanabilir. Ayrıca, sanal ağlar eşlenmiş olsa bile ağ geçitlerini kullanarak [Sanal ağlar arası bağlantılar](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) yapılandırabilirsiniz.

Sanal ağlar arası bağlantı için her iki seçenek de yapılandırıldığında, sanal ağlar arasındaki trafik, eşleme yapılandırması (Azure omurgası) üzerinden akış gerçekleştirir.

Sanal ağlar eşlendiğinde, eşlenmiş sanal ağdaki ağ geçidini şirket içi bir ağa geçiş noktası olarak da yapılandırabilirsiniz. Bu durumda, uzak ağ geçidi kullanan sanal ağın kendi ağ geçidi olamaz. Bir sanal ağın yalnızca bir ağ geçidi olabilir. Ağ geçidi, aşağıdaki resimde gösterildiği gibi yerel veya uzak bir ağ geçidi (eşlenen sanal ağda) olabilir:

![VNet eşleme geçişi](./media/virtual-networks-peering-overview/figure02.png)

Farklı dağıtım modelleriyle oluşturulmuş sanal ağlar arasındaki eşleme ilişkisinde ağ geçidi geçişi desteklenmez. Bir ağ geçidi geçişinin çalışması için eşleme ilişkisindeki her iki sanal ağ da Resource Manager ile oluşturulmuş olmalıdır.

Tek bir Azure ExpressRoute bağlantısını kullanan sanal ağlar eşlendiğinde, bu iki sanal ağ arasındaki trafik, eşleme ilişkisi (Azure omurga ağı) üzerinden akış gerçekleştirir. Şirket içi devreye bağlanmak için her bir sanal ağ üzerindeki yerel ağ geçitlerini kullanmaya devam edebilirsiniz. Alternatif olarak, paylaşılan bir ağ geçidini kullanıp şirket içi bağlantı için bir geçiş yapılandırabilirsiniz.

<a id="provisioning" class="xliff"></a>

## Sağlama
Sanal ağ eşlemesi ayrıcalıklı bir işlemdir. VirtualNetworks ad alanı altında yer alan ayrı bir işlevdir. Bir kullanıcıya eşlemeyi yetkilendirmesi için belirli haklar verilebilir. Sanal ağa yönelik okuma/yazma erişimi olan bir kullanıcı bu haklara otomatik olarak sahip olur.

Eşleme özelliğinin yöneticisi ya da ayrıcalıklı kullanıcısı olan bir kullanıcı, başka bir sanal ağ üzerinde eşleme işlemi başlatabilir. Diğer tarafta eşleme için eşleşen bir istek varsa ve diğer gereksinimler karşılanırsa eşleme gerçekleştirilir.

<a id="limits" class="xliff"></a>

## Sınırlar
Tek bir sanal ağ için izin verilen eşleme sayısı sınırlıdır. Daha fazla bilgi edinmek için [Azure ağ sınırlarını](../azure-subscription-service-limits.md#networking-limits) gözden geçirin.

<a id="pricing" class="xliff"></a>

## Fiyatlandırma
Sanal ağ eşlemesi kullanan girdi ve çıktı trafiği için nominal bir ücret uygulanır. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/virtual-network).

## <a name="next-steps"></a>Sonraki adımlar

* [Sanal ağ eşleme öğreticisini](virtual-network-create-peering.md) tamamlayın
* Tüm [sanal ağ eşleme ayarları ve ayarların nasıl değiştirileceği](virtual-network-manage-peering.md) hakkında bilgi edinin.

