---
title: "Konumlar ve bağlantı sağlayıcıları: Azure ExpressRoute | Microsoft Docs"
description: "Bu makale, sunulan hizmetlerin konumları ve Azure bölgelerine nasıl bağlanılacağı hakkında ayrıntılı bir genel bakış sağlar. Konuma göre sıralanmıştır."
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
ms.assetid: feb67da3-5abc-4acb-bad4-f78e3c541ded
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/17/2017
ms.author: kaanan
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: a10986ac0b36a3f2065f02533f9e321c755d4cc8
ms.contentlocale: tr-tr
ms.lasthandoff: 08/21/2017

---
# <a name="expressroute-partners-and-peering-locations"></a>ExpressRoute ortakları ve eşleme konumları

> [!div class="op_single_selector"]
> * [Sağlayıcıya Göre Konumlar](expressroute-locations.md)
> * [Konuma Göre Sağlayıcılar](expressroute-locations-providers.md)


Bu makaledeki tablolar ExpressRoute bağlantı sağlayıcıları, ExpressRoute coğrafi kapsamı, ExpressRoute üzerinden desteklenen Microsoft bulut hizmetleri ve ExpressRoute Sistem Tümleştiricileri (SIs) hakkında bilgi sağlar.

## <a name="partners"></a>ExpressRoute bağlantı sağlayıcıları
ExpressRoute tüm Azure bölgeleri ve konumları arasında desteklenir. Aşağıdaki harita Azure bölgeleri ve ExpressRoute konumlarının listesini sağlar. ExpressRoute konumları birkaç hizmet sağlayıcının sahip olduğu Microsoft eşlerine başvurur.

![Konum eşleme][0]

Coğrafi bölge içindeki en az bir ExpressRoute konumuna bağlanırsanız coğrafi bölge içindeki tüm bölgeler arasında Azure hizmetlerine erişebileceksiniz. 

### <a name="azure-regions-to-expressroute-locations-within-a-geopolitical-region"></a>Bir coğrafi bölge içindeki Azure bölgeler ile ExpressRoute konumları arasında eşleme
Aşağıdaki tablo, coğrafi bölge içindeki Azure bölgeler ile ExpressRoute konumları arasında yapılan eşlemeyi sağlar.

| **Jeopolitik bölge** | **Azure bölgeleri** | **ExpressRoute konumları** |
| --- | --- | --- |
| **Kuzey Amerika** |Doğu ABD, Batı ABD, Doğu ABD 2, Batı ABD 2, Orta ABD, Orta Güney ABD, Orta Kuzey ABD, Batı Orta ABD, Orta Kanada, Doğu Kanada |Atlanta, Chicago, Dallas, Denver, Las Vegas, Los Angeles, Miami, New York, San Antonio, Seattle, Silikon Vadisi, Washington, Montreal, Quebec City, Toronto |
| **Güney Amerika** |Güney Brezilya |Sao Paulo |
| **Avrupa** |Kuzey Avrupa, Batı Avrupa, İngiltere Batı, İngiltere Güney |Amsterdam, Dublin, Londra, Newport(Galler), Paris |
| **Asya** |Doğu Asya, Güneydoğu Asya |Hong Kong, Singapur |
| **Japonya** |Batı Japonya, Doğu Japonya |Osaka, Tokyo |
| **Avustralya** |Güneydoğu Avustralya, Doğu Avustralya |Melbourne, Sidney |
| **Hindistan** |Batı Hindistan, Orta Hindistan, Güney Hindistan |Chennai, Mumbai |
| **Güney Kore** |Kore Orta, Kore Güney |Busan, Seul |

### <a name="regions-and-geopolitical-boundaries-for-national-clouds"></a>Ulusal bulutlar için bölgeler ve coğrafi sınırlar
Aşağıdaki tablo ulusal bulutlar için bölgeler ve coğrafi sınırlar hakkında bilgi sağlar.

| **Jeopolitik bölge** | **Azure bölgeleri** | **ExpressRoute konumları** |
| --- | --- | --- |
| **US Government bulutu** |ABD Devleti Iowa, ABD Devleti Virginia, US DoD Orta, US DoD Doğu  |Chicago, Dallas, New York, Seattle, Silikon Vadisi, Washington DC |
| **Çin** |Kuzey Çin, Doğu Çin |Pekin, Şangay |
| **Almanya** |Orta Almanya, Doğu Almanya |Berlin, Frankfurt |

Coğrafi bölgeler arasındaki bağlantı standart ExpressRoute SKU’da desteklenmiyor. Genel bağlantıyı desteklemek için ExpressRoute premium eklentisini etkinleştirmeniz gerekir. Ulusal bulut ortamlarına bağlantı desteklenmiyor. Bu tür bir ihtiyaç ortaya çıkarsa bağlantı sağlayıcınız ile çalışabilirsiniz.

## <a name="locations"></a>Bağlantı sağlayıcı konumları

Aşağıdaki tabloda bağlantı konumları ve her konum için hizmet sağlayıcıları gösterilmektedir. Hizmet sağlayıcılarını ve hizmet sunabildikleri konumları görüntülemek istiyorsanız bkz. [Hizmet sağlayıcısına göre konumlar](expressroute-locations.md#locations). 


### <a name="production-azure"></a>Üretim Azure
| **Konum** | **Hizmet Sağlayıcılar** |
| --- | --- |
| **Amsterdam** |Aryaka Networks, AT&T NetBond, British Telecom, Colt, Equinix, euNetworks, GÉANT, InterCloud, Internet Solutions - Cloud Connect, Interxion, KPN, Level 3 Communications, Megaport, Orange, Tata Communications, TeleCity Group, Telefonica, Telenor, Verizon, Zayo Group |
| **Atlanta** |Equinix |
| **Busan** |LG CNS |
| **Chennai** | Airtel+, Global CloudXchange (GCX), SIFY, Tata Communications |
| **Chicago** |AT&T NetBond, Comcast, Equinix, Level 3 Communications, Megaport, Verizon, Zayo Group |
| **Dallas** |Aryaka Networks, AT&T NetBond, Cologix, Equinix, Level 3 Communications, Megaport, Verizon, Zayo Group+ |
| **Denver** |CoreSite |
| **Dublin** |Colt, Interxion, Telecity Group |
| **Hong Kong** |Aryaka Networks, British Telecom, China Telecom Global, Equinix, Megaport, Orange, PCCW Global Limited, Tata Communications, Verizon |
| **Las Vegas** |Level 3 Communications, Megaport |
| **Londra** |AT&T NetBond, British Telecom, Colt, Equinix, InterCloud, Internet Solutions - Cloud Connect, Interxion, Jisc, Level 3 Communications, Megaport, MTN, NTT Communications, Orange, Tata Communications, Telecity Group, Telehouse - KDDI, Telenor, Verizon, Vodafone, Zayo Group+ |
| **Los Angeles** |CoreSite, Equinix, Megaport, NTT, Zayo Group |
| **Melbourne** |AARNet, Equinix, Megaport, NEXTDC, Telstra Corporation |
| **Miami** |C3ntro+, Megaport, Neutrona Networks |
| **Montreal** |Bell Canada, Cologix |
| **Mumbai** |Airtel+, Sify, Tata Communications |
| **New York** |Coresite, Equinix, Megaport, Zayo Group |
| **Newport(Galler)** |Next Generation Data |
| **Osaka** |Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, NTT SmartConnect, Softbank |
| **Paris** |Colt, Interxion, Equinix, Orange |
| **Quebec City** | Megaport |
| **San Antonio** |Megaport |
| **Sao Paulo** |Ascenty Data Centers+, Equinix, Level 3 Communications, Neutrona Networks, Telefonica, UOLDIVEO |
| **Seattle** |Equinix, Level 3 Communications, Megaport |
| **Seul** |KINX, LG CNS, Sejong Telecom |
| **Silikon Vadisi** |Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink+, Comcast, Console, Equinix, Level 3 Communications, Megaport, Orange, Tata Communications, Verizon, Zayo Group |
| **Singapur** |Aryaka Networks, AT&T NetBond, British Telecom, Equinix, InterCloud, Level 3 Communications, Megaport, NTT Communications, Orange, SingTel, Tata Communications, Verizon |
| **Sidney** |AARNet, AT&T NetBond, British Telecom, Equinix, Megaport, NEXTDC, Orange, Telstra Corporation, Verizon |
| **Tokyo** |Aryaka Networks, AT&T NetBond, British Telecom, Colt, Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, Softbank, Verizon |
| **Toronto** |AT&T NetBond, Bell Canada, Cologix, Console, Equinix, Megaport, Telus, Zayo Group |
| **Washington DC** |Aryaka Networks, AT&T NetBond, British Telecom, Comcast, Equinix, InterCloud, Level 3 Communications, Megaport, NTT Communications, Orange, Tata Communications, Verizon, Zayo Group |

 **+** çok yakında anlamına geliyor

### <a name="national-cloud-environments"></a>Ulusal bulut ortamları

### <a name="us-government-cloud"></a>US Government bulutu
| **Konum** | **Hizmet Sağlayıcılar** |
| --- | --- |
| **Chicago** |AT&T NetBond, Equinix, Level 3 Communications, Verizon |
| **Dallas** |Equinix, Megaport, Verizon |
| **New York** |Equinix, Level 3 Communications+, Verizon |
| **Silikon Vadisi** | Equinix, Level 3 Communications |
| **Seattle** | Equinix |
| **Washington DC** |AT&T NetBond, Equinix, Level 3 Communications, Verizon |

### <a name="china"></a>Çin
| **Konum** | **Hizmet Sağlayıcılar** |
| --- | --- |
| **Pekin** |China Telecom |
| **Şangay** |China Telecom |

Daha fazla bilgi için bkz. [Çin’de ExpressRoute](http://www.windowsazure.cn/home/features/expressroute/)

### <a name="germany"></a>Almanya
| **Konum** | **Hizmet Sağlayıcılar** |
| --- | --- |
| **Berlin** |Colt+, e-shelter, Megaport+, T-Systems |
| **Frankfurt** |Colt, Equinix, Interxion |

## <a name="c1partners"></a>Exchange Sağlayıcıları Üzerinden Bağlantı
Bağlantı sağlayıcınız önceki bölümlerde listelenmemişse hala bağlantı oluşturabilirsiniz.

* Yukarıdaki tabloda yer alan değişimlerin herhangi birine bağlı olup olmadığını görmek için bağlantı sağlayıcınıza başvurun. Değişim sağlayıcıları tarafından sunulan hizmetler hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kontrol edebilirsiniz. Birkaç bağlantı sağlayıcı Ethernet değişimlerine zaten bağlı.
  * [Cologix](http://www.cologix.com/)
  * [Console](https://www.consoleconnect.com/partners/cloudsaas/)
  * [CoreSite](http://www.coresite.com/)
  * [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
  * [InterXion](http://www.interxion.com/)
  * [NextDC](http://www.nextdc.com/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
* Bağlantı sağlayıcınızı, ağınızı seçtiğiniz eşleme konumuna genişletmesini sağlayın.
  * Bağlantı sağlayıcınızın bağlantınızı yüksek oranda kullanılabilir şekilde genişlettiğinden emin olun, böylece hiç tek nokta arızası olmaz.
* Microsoft’a bağlanmak için bağlantı sağlayınız olarak değişime sahip bir ExpressRoute bağlantı hattı sipariş edin.
  * Bağlantı kurmak için [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-classic.md)’daki adımları izleyin.

## <a name="c1partners"></a>Diğer Hizmet Sağlayıcılar Üzerinden Bağlantı
| **Konum** | **Exchange** | **Bağlantı Sağlayıcılar** |
| --- | --- | --- |
| **Amsterdam** | Equinix, Telecity | Eurofiber , Fastweb S.p.A, Nianet |
| **Chicago** | Equinix | Lightower, Windstream |
| **Dallas** | Equinix, Megaport | C3ntro Telecom, Cox Business, Data Foundry, Transtelco |
| **Frankfurt** | Telecity | Nianet, QSC AG |
| **Hong Kong** | Equinix | Macroview Telecom |
| **Londra** | Equinix, euNetworks, Telecity | Bezeq International Ltd., Epsilon, Exponential E, HSO, NexGen Networks, Tamares Telecom, Zain |
| **Los Angeles** | Equinix |Transtelco |
| **Madrid** | Level3 | Zertia |
| **Montreal** | Cologix, Equinix | Airgate Technologies. Inc, Cogeco Peer 1, Rogers, Zirro |
| **New York** |Equinix, Megaport | Altice Business, Lightower, Webair |
| **Seattle** |Equinix | Alaska Communications |
| **Silikon Vadisi** |Equinix | Cox Business, Windstream |
| **Singapur** |Equinix |1CLOUDSTAR, Epsilon Telecommunications Limited, LGA Telecom, United Information Highway (UIH) |
| **Slough** | Equinix | HSO|
| **Sidney** | Megaport | Macquarie Telecom Group|
| **Tokyo** | Equinix | ARTERIA Networks Corporation, BroadBand Tower, Inc. |
| **Toronto** | Equinix | Airgate Technologies. Inc, Cogeco Peer 1, Rogers, Thinktel, Zirro|
| **Washington DC** |Equinix | Altice Business, Gtt Communications Inc, Epsilon, Lightower, Masergy, Windstream |

## <a name="expressroute-system-integrators"></a>ExpressRoute sistem tümleştiricileri
İhtiyaçlarınıza uyan özel bağlantıyı etkinleştirme ağınızın ölçeğine bağlı olarak zorlu olabilir. ExpressRoute’a yönelik ekleme işleminde size yardımcı olmak üzere aşağıdaki tabloda listelenen herhangi bir sistem tümleştirici ile çalışabilirsiniz.

| **Continent** | **Sistem tümleştiricileri** |
| --- | --- |
| **Asya** |Avanade Inc., OneAs1a |
| **Avustralya** | Ensyst, IT Consultancy, MOQdigital, Vigilant.IT |
| **Avrupa** |Avanade Inc., Altogee, Bright Skies GmbH, Inframon, MSG Services, New Signature, Nelite, Orange Networks, sol-tec |
| **Kuzey Amerika** |Avanade Inc., Equinix Professional Services, FlexManage, Perficient, Presidio |
| **Güney Amerika** |Avanade Inc. |
## <a name="next-steps"></a>Sonraki adımlar
* ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
* Tüm önkoşulların sağlandığından emin olun. Bkz. [ExpressRoute önkoşulları](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Konum eşleme"

