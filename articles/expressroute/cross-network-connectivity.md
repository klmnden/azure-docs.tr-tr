---
title: Azure arası ağ bağlantısı | Microsoft Docs
description: Bu sayfa arası ağ bağlantısı ve Azure ağ özelliklerini temel alarak çözüm için bir uygulama senaryosu açıklanmaktadır.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute
ms.topic: article
ms.workload: infrastructure-services
ms.date: 04/03/2019
ms.author: rambala
ms.openlocfilehash: 3bc189cf269084fdb26f141a36755c96554cad7b
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64865996"
---
# <a name="cross-network-connectivity"></a>Ağlar arası bağlantı

Fabrikam Inc. büyük bulunmasını ve Doğu ABD Azure dağıtım var. Fabrikam, şirket içi ve ExpressRoute aracılığıyla Azure dağıtımlar arasında arka uç bağlantısı vardır. Benzer şekilde, Contoso Ltd. varlığı ve Batı ABD Azure dağıtımında sahiptir. Contoso, şirket içi ve ExpressRoute aracılığıyla Azure dağıtımlar arasında arka uç bağlantısı vardır.  

Fabrikam Inc. acquires Contoso Ltd. Ağlar arasında bağlantı kurmak, Fabrikam birleşme istemektedir. Aşağıdaki şekil senaryo gösterilmektedir:

 [![1]][1]

Yukarıdaki şekilde ortasında kesikli oklar, istenen ağ Connect gösterir. Özellikle, çapraz bağlantıları istenen üç tür vardır: (1) Fabrikam ve Contoso sanal ağlar arası bağlantı, 2) arası (yani, Fabrikam şirket içi ağı Contoso sanal ağa bağlamak ve Fabrikam Vnet'e Contoso şirket içi ağa bağlanma), bölgesel şirket içinde ve sanal ağlar bağlanır ve Contoso ve Fabrikam 3) Çapraz şirket içi ağınıza bağlayın. 

ExpressRoute, Contoso Ltd., birleşme önce özel eşlemesi yol tablosu aşağıdaki tabloda gösterilmektedir.

[![2]][2]

Aşağıdaki tabloda, Contoso aboneliği birleşme önce sanal makinenin geçerli rotalar gösterir. Tablo VNet adres alanı ve varsayılan değerleri dışında Contoso şirket içi ağ, VM sanal ağ üzerinde bilmektedir. 

[![4]][4]

ExpressRoute, Fabrikam, Inc.'in, birleşme önce özel eşlemesi yol tablosu aşağıdaki tabloda gösterilmektedir.

[![3]][3]

Aşağıdaki tabloda, Fabrikam Abonelikteki birleşme önce sanal makinenin geçerli rotalar gösterir. Tablo VNet adres alanı ve varsayılan değerleri dışında Fabrikam şirket içi ağ, VM sanal ağ üzerinde bilmektedir.

[![5]][5]

Bu makalede, aşağıdaki Azure ağ özellikleri kullanarak istenen çapraz bağlantıları elde etmek nasıl tartışmak ve üzerinden adım adım edelim:

* [Sanal Ağ eşlemesi][Virtual network peering] 
* [Sanal ağ ExpressRoute bağlantısı][connection]
* [Global erişim][Global Reach] 

## <a name="cross-connecting-vnets"></a>Çapraz sanal ağları bağlama

Sanal ağ (VNet eşlemesi) eşlemesi en iyi ve en iyi ağ performansını iki sanal ağ bağlanırken sağlar. VNet eşlemesi eşlemesi iki sanal ağ (VNet eşlemesi yaygın olarak adlandırılır) için aynı Azure bölgesindeki hem de iki farklı Azure bölgelerinde (genel sanal ağ eşleme yaygın olarak adlandırılır) destekler. 

Contoso ve Fabrikam Azure aboneliklerinin içindeki sanal ağlar arasında eşleme genel sanal ağ yapılandıralım. Sanal ağ arasında iki sanal ağları eşleme oluşturmak nasıl [bir sanal ağ eşlemesi oluşturma] [ Configure VNet peering] makalesi.

Aşağıdaki resimde, genel sanal ağ eşleme yapılandırdıktan sonra ağ mimarisi gösterilmektedir.

[![6]][6]

Aşağıdaki tabloda, Contoso aboneliği VM bilinen yolları gösterir. Tablonun son giriş dikkat edin. Bu giriş sanal ağları bağlama çapraz sonucudur.

[![7]][7]

Aşağıdaki tabloda yolları Fabrikam aboneliğe VM bilinen gösterilmektedir. Tablonun son giriş dikkat edin. Bu giriş sanal ağları bağlama çapraz sonucudur.

[![8]][8]

VNet eşlemesi doğrudan bağlantılar iki sanal ağ (bkz: hiçbir sonraki atlama için vardır *VNetGlobalPeering* Yukarıdaki iki tablo girişi)

## <a name="cross-connecting-vnets-to-the-on-premises-networks"></a>Çapraz şirket içi ağlara sanal ağları bağlama

Biz, bir ExpressRoute bağlantı hattı birden çok sanal ağlara bağlanabilirsiniz. Bkz: [abonelik ve hizmet sınırlamaları] [ Subscription limits] bir ExpressRoute bağlantı hattına bağlı sanal ağlar maksimum sayısı. 

Şimdi Fabrikam abonelik sanal ağları ile şirket içi ağlar arası bağlantıyı etkinleştirmek için sanal ağ için Contoso aboneliği VNet Fabrikam ExpressRoute bağlantı hattına ve benzer şekilde Contoso ExpressRoute bağlantı hattına bağlayın. Bir ExpressRoute bağlantı hattı farklı bir abonelikte bir sanal ağa bağlanmak için oluşturma ve bir yetkilendirme kullanma ihtiyacımız var.  Makaleye bakın: [Bir sanal ağı ExpressRoute devresine bağlama][Connect-ER-VNet].

Aşağıdaki resimde, ExpressRoute yapılandırdıktan sonra ağ mimarisi gösterilmiştir için sanal ağlar arası bağlantı.

[![9]][9]

Aşağıdaki tabloda, özel ExpressRoute, Contoso Ltd. sonra sanal ağları ExpressRoute aracılığıyla şirket içi ağlara bağlanma arası eşleme rota tablosu gösterilmektedir. Yol tablosu yolları her iki sanal ağlara ait olduğunu görürsünüz.

[![10]][10]

Aşağıdaki tabloda, özel ExpressRoute, Fabrikam Inc.'ın, sonra sanal ağları ExpressRoute aracılığıyla şirket içi ağlara bağlanma arası eşleme rota tablosu gösterilmektedir. Yol tablosu yolları her iki sanal ağlara ait olduğunu görürsünüz.

[![11]][11]

Aşağıdaki tabloda, Contoso aboneliği VM bilinen yolları gösterir. Dikkat *sanal ağ geçidi* tablo girişleri. VM, her iki şirket içi ağ için rota görür.

[![12]][12]

Aşağıdaki tabloda yolları Fabrikam aboneliğe VM bilinen gösterilmektedir. Dikkat *sanal ağ geçidi* tablo girişleri. VM, her iki şirket içi ağ için rota görür.

[![13]][13]

>[!NOTE]
>Ya da bulundurabilirsiniz Fabrikam ve/veya Contoso abonelikleri uç sanal ağları ilgili merkez sanal ağa (bir hub ve bağlı bileşen tasarım mimari diyagramları bu makalede gösterilmiştir değil). ExpressRoute hub VNet ağ geçitleri arasında çapraz bağlantılar Doğu ve Batı hub ve bağlı bileşenler arasındaki iletişim de izin verir.
>

## <a name="cross-connecting-on-premises-networks"></a>Çapraz şirket içi ağları bağlama

ExpressRoute Global erişim farklı ExpressRoute bağlantı hatlarına bağlı şirket içi ağlar arasında bağlantı sağlar. Global erişim arasında Contoso ve Fabrikam ExpressRoute bağlantı hatları yapılandıralım. ExpressRoute bağlantı hatları farklı Aboneliklerde olduğundan, oluşturma ve bir yetkilendirme kullanma ihtiyacımız var. Bkz: [yapılandırma ExpressRoute Global erişim] [ Configure Global Reach] makale adım adım yönergeler için.

Aşağıdaki resimde, Global erişim yapılandırdıktan sonra ağ mimarisi gösterilmektedir.

[![14]][14]

Global erişim yapılandırdıktan sonra eşlemesi, özel ExpressRoute, Contoso Ltd. yol tablosu aşağıdaki tabloda gösterilmektedir. Yol tablosu yolları hem şirket içi ağlara ait olduğunu görürsünüz. 

[![15]][15]

Global erişim yapılandırdıktan sonra eşlemesi, özel ExpressRoute, Fabrikam Inc., yol tablosu aşağıdaki tabloda gösterilmektedir. Yol tablosu yolları hem şirket içi ağlara ait olduğunu görürsünüz.

[![16]][16]

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [virtual network SSS][VNet-FAQ]daha ayrıntılı sorular için sanal ağ ve VNet eşlemesi. Bkz: [ExpressRoute SSS] [ ER-FAQ] başka sorunuz ExpressRoute ve sanal ağ bağlantısını için.

Küresel erişim ülke/bölge tarafından ülke/bölge olarak alınır. Global erişim ülkede/istediğiniz bölgede kullanılabilir olup olmadığını görmek için bkz: [ExpressRoute Global erişim][Global Reach].

<!--Image References-->
[1]: ./media/cross-network-connectivity/premergerscenario.png "uygulama senaryosu"
[2]: ./media/cross-network-connectivity/contosoexr-rt-premerger.png "birleşme önce Contoso ExpressRoute yol tablosu"
[3]: ./media/cross-network-connectivity/fabrikamexr-rt-premerger.png "birleşme önce Fabrikam ExpressRoute yol tablosu"
[4]: ./media/cross-network-connectivity/contosovm-routes-premerger.png "Contoso VM önce birleşme yönlendirir"
[5]: ./media/cross-network-connectivity/fabrikamvm-routes-premerger.png "Fabrikam VM önce birleşme yönlendirir"
[6]: ./media/cross-network-connectivity/vnet-peering.png "VNet eşlemesi sonra mimarisi"
[7]: ./media/cross-network-connectivity/contosovm-routes-peering.png "Contoso VM yolları sonra VNet eşlemesi"
[8]: ./media/cross-network-connectivity/fabrikamvm-routes-peering.png "Fabrikam VM yolları sonra VNet eşlemesi"
[9]: ./media/cross-network-connectivity/exr-x-connect.png "ExpressRoutes bağlantıdan sonra mimarisi"
[10]: ./media/cross-network-connectivity/contosoexr-rt-xconnect.png "bağlanan ExR ve sanal ağlar arası sonra Contoso ExpressRoute yol tablosu"
[11]: ./media/cross-network-connectivity/fabrikamexr-rt-xconnect.png "bağlanan ExR ve sanal ağlar arası sonra Fabrikam ExpressRoute yol tablosu"
[12]: ./media/cross-network-connectivity/contosovm-routes-xconnect.png "Contoso VM yolları sonra bağlantı ExR ve sanal ağlar arası"
[13]: ./media/cross-network-connectivity/fabrikamvm-routes-xconnect.png "Fabrikam VM yolları sonra bağlantı ExR ve sanal ağlar arası"
[14]: ./media/cross-network-connectivity/globalreach.png "Global erişim yapılandırdıktan sonra mimarisi"
[15]: ./media/cross-network-connectivity/contosoexr-rt-gr.png "Global erişim sonra Contoso ExpressRoute yol tablosu"
[16]: ./media/cross-network-connectivity/fabrikamexr-rt-gr.png "Global erişim sonra Fabrikam ExpressRoute yol tablosu"

<!--Link References-->
[Virtual network peering]: https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview
[connection]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-linkvnet-portal-resource-manager
[Global Reach]: https://docs.microsoft.com/azure/expressroute/expressroute-global-reach
[Configure VNet peering]: https://docs.microsoft.com/azure/virtual-network/create-peering-different-subscriptions
[Configure Global Reach]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-set-global-reach
[Subscription limits]: https://docs.microsoft.com/azure/azure-subscription-service-limits#networking-limits
[Connect-ER-VNet]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-linkvnet-portal-resource-manager
[ER-FAQ]: https://docs.microsoft.com/azure/expressroute/expressroute-faqs
[VNet-FAQ]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-faq