---
title: 'Konumlar ve bağlantı sağlayıcıları: Azure ExpressRoute | Microsoft Docs'
description: Bu makale, sunulan hizmetlerin konumları ve Azure bölgelerine nasıl bağlanılacağı hakkında ayrıntılı bir genel bakış sağlar. Konuma göre sıralanmıştır.
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
ms.assetid: feb67da3-5abc-4acb-bad4-f78e3c541ded
ms.service: expressroute
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/18/2019
ms.author: jaredr80
ms.openlocfilehash: 3a29940c4ef904d813fa7400928448a5c48334a4
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67205962"
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

| **Jeopolitik bölge** | **Bölge** | **Azure bölgeleri** | **ExpressRoute konumları** |
| --- | --- | --- | --- |
| **Australia Government** | 1 | Avustralya Orta, Avustralya Orta 2 |Kanberra, Kanberra2 |
| **Avrupa** | 1 |Fransa Orta, Fransa Güney, Kuzey Avrupa, Batı Avrupa, UK Batı, UK Güney |Amsterdam, Amsterdam2, Dublin, Frankfurt, Londra, London2, Marsilya'daki, newport(Galler), Paris, Zürih |
| **Kuzey Amerika** | 1 |Doğu ABD, Batı ABD, Doğu ABD 2, Batı ABD 2, Orta ABD, Orta Güney ABD, Orta Kuzey ABD, Batı Orta ABD, Orta Kanada, Doğu Kanada |Atlanta, Chicago, Dallas, Denver, Las Vegas, Los Angeles, Miami, New York, San Antonio, Seattle, Silikon vadisi, Silikon Valley2, DC2 Washington DC, Washington, Montreal, Quebec City, Toronto |
| **Asya** | 2 |Doğu Asya, Güneydoğu Asya |Hong Kong ÖİB, Kuala Lumpur, Singapur, singapur2 Taipei |
| **Hindistan** | 2 |Batı Hindistan, Orta Hindistan, Güney Hindistan |Madras, Madras2, Bombay, Bombay2 |
| **Japonya** | 2 |Batı Japonya, Doğu Japonya |Osaka, Tokyo |
| **Okyanusya** | 2 |Güneydoğu Avustralya, Doğu Avustralya |Auckland, Melbourne, Perth, Sidney | 
| **Güney Kore** | 2 |Kore Orta, Kore Güney |Busan, Seul|
| **BAE** | 3 | BAE Orta, BAE Kuzey | Dubai, Dubai2 |
| **Güney Afrika** | 3 |Güney Afrika Batı, Güney Afrika Kuzey |Cape Town, Johannesburg |
| **Güney Amerika** | 3 |Güney Brezilya |Sao Paulo |

### <a name="regions-and-geopolitical-boundaries-for-national-clouds"></a>Ulusal bulutlar için bölgeler ve coğrafi sınırlar
Aşağıdaki tablo ulusal bulutlar için bölgeler ve coğrafi sınırlar hakkında bilgi sağlar.

| **Jeopolitik bölge** | **Azure bölgeleri** | **ExpressRoute konumları** |
| --- | --- | --- |
| **US Government bulutu** |US Gov Arizona, US Gov Iowa, US Gov Teksas, US Gov Virginia, US DoD Orta, US DoD Doğu  |Chicago, Dallas, New York, Phoenix, San Antonio, Seattle, Silikon Vadisi, Washington DC |
| **Çin Doğu** |Çin Doğu, Çin Doğu2 |Şangay, Shanghai2 |
| **Çin Kuzey** |Çin Kuzey, Çin Kuzey2 |Pekin, Beijing2 |
| **Almanya** |Orta Almanya, Doğu Almanya |Berlin, Frankfurt |

Coğrafi bölgeler arasındaki bağlantı standart ExpressRoute SKU’da desteklenmiyor. Genel bağlantıyı desteklemek için ExpressRoute premium eklentisini etkinleştirmeniz gerekir. Ulusal bulut ortamlarına bağlantı desteklenmiyor. Bu tür bir ihtiyaç ortaya çıkarsa bağlantı sağlayıcınız ile çalışabilirsiniz.

## <a name="locations"></a>Bağlantı sağlayıcı konumları

Aşağıdaki tabloda bağlantı konumları ve her konum için hizmet sağlayıcıları gösterilmektedir. Hizmet sağlayıcılarını ve hizmet sunabildikleri konumları görüntülemek istiyorsanız bkz. [Hizmet sağlayıcısına göre konumlar](expressroute-locations.md#locations). 

**Yerel Azure bölgeleri** olanlardır, [ExpressRoute yerel](expressroute-faqs.md) her eşleme konumunda erişebilirsiniz. **yok** ExpressRoute yerel eşleme o konumda kullanılabilir olmadığını gösterir.


### <a name="production-azure"></a>Üretim Azure
| **Location** | **Eşdüzey Hizmet Sağlama Konumu Sahibi** | **Yerel Azure bölgeleri** | **Hizmet Sağlayıcılar** |
| --- | --- | --- | --- |
| **Amsterdam** | Equinix | Batı Avrupa | Aryaka Networks, AT&T NetBond, British Telecom, Colt, Equinix, euNetworks, GÉANT, InterCloud, Interxion, KPN, IX Reach, Level 3 Communications, Megaport, NTT Communications, Orange, Tata Communications, TeleCity Group, Telefonica, Telenor, Telia Carrier, Verizon, Zayo |
| **Amsterdam2** | Interxion | Batı Avrupa | DE-CIX, Interxion, Vodafone |
| **Atlanta** | Equinix | yok | Equinix, Megaport |
| **Auckland** | Vocus | yok | Devoli |
| **Busan** |LG CNS | Kore Güney | LG CNS |
| **Kanberra** | CDC | Avustralya Orta | CDC |
| **Kanberra2** | CDC | Avustralya Orta 2| CDC |
| **Cape Town** | Teraco | Güney Afrika Batı | Internet Solutions - Cloud Connect, Liquid Telecom, Teraco |
| **Chennai** | Tata Communications | Güney Hindistan | Global CloudXchange (GCX), SIFY, Tata Communications |
| **Chennai2** | Airtel | Güney Hindistan | Airtel |
| **Chicago** | Equinix | Orta Kuzey ABD | Aryaka ağları Cologix, Comcast, Coresite, Equinix, InterCloud, Internet2, 3. düzey iletişimleri, Megaport, PacketFabric, genel PCCW Limited, Sprint, Telia taşıyıcı, Verizon, Zayo, AT & T NetBond, CenturyLink bulut bağlanma |
| **Dallas** | Equinix | yok | Aryaka Networks, AT&T NetBond, Cologix, Equinix, Internet2, Level 3 Communications, Megaport, Neutrona Networks, Telmex Uninet, Telia Carrier, Transtelco, Verizon, Zayo|
| **Denver** | CoreSite | Batı Orta ABD | CoreSite, Megaport, Zayo |
| **Dubai** | Etisalat UAE | BAE Kuzey | Etisalat UAE |
| **Dubai2** | DU datamena | BAE Kuzey | DU datamena, Orixcom |
| **Dublin** | Equinix | Kuzey Avrupa | Colt, eir, Equinix, Interxion, Megaport |
| **Frankfurt** | Interxion | yok | DE-CIX Interxion |
| **Hong Kong ÖİB** | Equinix | Doğu Asya | Aryaka Networks, British Telecom, CenturyLink Cloud Connect, Chief Telecom, China Telecom Global, Equinix, Megaport, NTT Communications, Orange, PCCW Global Limited, Tata Communications, Verizon |
| **Johannesburg** | Teraco | Güney Afrika Kuzey | İngiliz Telekom, Internet Solutions - Cloud Connect Liquid Telekom Teraco |
| **Kuala Lumpur** | TIME dotCom | yok | TIME dotCom |
| **Las Vegas** | Anahtar | yok | CenturyLink Cloud Connect, Megaport |
| **Londra** | Equinix | Birleşik Krallık Güney | AT&T NetBond, British Telecom, Colt, Equinix, InterCloud, Internet Solutions - Cloud Connect, Interxion, Jisc, Level 3 Communications, Megaport, MTN, NTT Communications, Orange, PCCW Global Limited, Tata Communications, Telehouse - KDDI, Telenor, Telia Carrier, Verizon, Vodafone, Zayo |
| **London2** | Telehouse | Birleşik Krallık Güney | IX Reach, Equinix |
| **Los Angeles** | CoreSite | yok | CoreSite, Equinix, Megaport, Neutrona Networks, NTT, Zayo |
| **Marsilya** |Interxion | Fransa Güney | Interxion, Jaguar ağ |
| **Melbourne** | NextDC | Avustralya Güneydoğu | AARNet, Devoli, Equinix, Megaport, NEXTDC, Optus, Telstra Corporation, TPG telekomünikasyon |
| **Miami** | Equinix | yok | C3ntro+, Equinix, Megaport, Neutrona Networks |
| **Montreal** | Cologix | yok | Bell Canada, Cologix, Telus, Zayo |
| **Mumbai** | Tata Communications | Batı Hindistan | Genel CloudXchange (GCX) olmasının Jio, Sify Tata iletişimleri, Verizon |
| **Mumbai2** | Airtel | Batı Hindistan | Airtel, Sify, Vodafone Idea |
| **New York** | Equinix | yok | CenturyLink Cloud Connect, Coresite, Equinix, InterCloud, Megaport, Packet, Zayo |
| **Newport(Galler)** | Next Generation Data | Birleşik Krallık Batı | İngiliz Telekom, Colt, 3. düzey iletişimleri, yeni nesil veriler |
| **Osaka** | Equinix | Japonya Batı | Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, NTT SmartConnect, Softbank |
| **Paris** | Interxion | Fransa Orta | Colt, Equinix, Intercloud, Interxion, Orange, Telia Carrier, Zayo |
| **Perth** | NextDC | yok | Megaport, NextDC |
| **Quebec City** | 4Degrees | Doğu Kanada | Bell Canada, Megaport |
| **San Antonio** | CyrusOne | Orta Güney ABD | CenturyLink Cloud Connect, Megaport |
| **Sao Paulo** | Equinix | Güney Brezilya | Aryaka Networks, Ascenty Data Centers, British Telecom, Equinix, Level 3 Communications, Neutrona Networks, Orange, Tata Communications, Telefonica, UOLDIVEO |
| **Seattle** | Equinix | Batı ABD 2 | 3 iletişimleri, Megaport, Telus Zayo Aryaka ağlar Equinix, düzey |
| **Seul** | KINX | Kore Orta | KINX, LG CNS, Sejong Telecom |
| **Silikon Vadisi** | Equinix | Batı ABD | Aryaka ağlar, AT & T NetBond, İngiliz Telekom CenturyLink buluta bağlayın, Comcast, Coresite, Equinix InterCloud, paket, PacketFabric, 3. düzey iletişimleri, Megaport, Orange, Sprint, Tata iletişimleri, Verizon, Zayo IX ulaşın |
| **Silikon Valley2** | Coresite | Batı ABD | Coresite | 
| **Singapur** | Equinix | Güneydoğu Asya | Aryaka Networks, AT&T NetBond, British Telecom, Epsilon Global Communications, Equinix, InterCloud, Level 3 Communications, Megaport, NTT Communications, Orange, SingTel, Tata Communications, Telstra Corporation, Verizon, Vodafone |
| **Singapur2** | Global Switch | Güneydoğu Asya | Colt, Epsilon Megaport, genel iletişim SingTel |
| **Sidney** | Equinix | Avustralya Doğu | AARNet, AT&T NetBond, British Telecom, Devoli, Equinix, Kordia, Megaport, NEXTDC, NTT Communications, Optus, Orange, Telstra Corporation, TPG Telecom, Verizon |
| **Taipei** | Baş telekomünikasyon | yok | Baş Telekom, FarEasTone |
| **Tokyo** | Equinix | Japonya Doğu | Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink Cloud Connect, Colt, Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, NTT EAST, Orange, Softbank, Verizon |
| **Toronto** | Cologix | Orta Kanada | AT&T NetBond, Bell Canada, CenturyLink Cloud Connect, Cologix, Equinix, IX Reach Megaport, Telus, Verizon, Zayo |
| **Washington DC** | Equinix | Doğu ABD, Doğu ABD 2 | Aryaka ağları AT & T NetBond, İngiliz Telekom, CenturyLink bulut bağlanmak, Cologix, Comcast, Coresite, Equinix, Internet2, InterCloud, 3. düzey iletişim, Megaport, Neutrona ağları, NTT iletişimleri, Orange, PacketFabric, Sprint, Tata İletişim, Telia taşıyıcı, Verizon, Zayo |
| **Washington DC2** | Coresite | Doğu ABD, Doğu ABD 2 |Coresite | 
| **Zürih** | Interxion | yok | Interxion |

 **+** çok yakında anlamına geliyor

### <a name="national-cloud-environments"></a>Ulusal bulut ortamları

### <a name="us-government-cloud"></a>US Government bulutu
| **Location** | **Hizmet Sağlayıcılar** |
| --- | --- |
| **Chicago** |AT&T NetBond, Equinix, Level 3 Communications, Verizon |
| **Dallas** |Equinix, Megaport, Verizon |
| **New York** |Equinix, CenturyLink Cloud Connect, Verizon |
| **Phoenix** | AT & T NetBond, CenturyLink bulut bağlanmak, Megaport |
| **San Antonio** | CenturyLink Cloud Connect, Megaport |
| **Silikon Vadisi** | Equinix, Level 3 Communications, Verizon |
| **Seattle** | Equinix, Megaport |
| **Washington DC** |AT&T NetBond, Equinix, Level 3 Communications, Megaport, Verizon |

### <a name="china"></a>Çin
| **Location** | **Hizmet Sağlayıcılar** |
| --- | --- |
| **Pekin** |China Telecom |
| **Beijing2** | Çin Telekom, GDS |
| **Şangay** |China Telecom |
| **Shanghai2** | Çin Telekom, GDS |

Daha fazla bilgi için bkz. [Çin’de ExpressRoute](http://www.windowsazure.cn/home/features/expressroute/)

### <a name="germany"></a>Almanya
| **Konum** | **Hizmet Sağlayıcılar** |
| --- | --- |
| **Berlin** |e-shelter, Megaport+, T-Systems |
| **Frankfurt** |Colt, Equinix, Interxion |

## <a name="c1partners"></a>Exchange Sağlayıcıları Üzerinden Bağlantı
Bağlantı sağlayıcınız önceki bölümlerde listelenmemişse hala bağlantı oluşturabilirsiniz.

* Yukarıdaki tabloda yer alan değişimlerin herhangi birine bağlı olup olmadığını görmek için bağlantı sağlayıcınıza başvurun. Değişim sağlayıcıları tarafından sunulan hizmetler hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kontrol edebilirsiniz. Birkaç bağlantı sağlayıcı Ethernet değişimlerine zaten bağlı.
  * [Cologix](https://www.cologix.com/)
  * [CoreSite](https://www.coresite.com/)
  * [Equinix Cloud Exchange](https://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
  * [InterXion](https://www.interxion.com/)
  * [NextDC](https://www.nextdc.com/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [PacketFabric](https://www.packetfabric.com/packetcor/microsoft-azure/)
  
* Bağlantı sağlayıcınızı, ağınızı seçtiğiniz eşleme konumuna genişletmesini sağlayın.
  * Bağlantı sağlayıcınızın bağlantınızı yüksek oranda kullanılabilir şekilde genişlettiğinden emin olun, böylece hiç tek nokta arızası olmaz.
* Microsoft’a bağlanmak için bağlantı sağlayınız olarak değişime sahip bir ExpressRoute bağlantı hattı sipariş edin.
  * Bağlantı kurmak için [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-classic.md)’daki adımları izleyin.

## <a name="c1partners"></a>Diğer Hizmet Sağlayıcılar Üzerinden Bağlantı
| **Location** | **Exchange** | **Bağlantı Sağlayıcılar** |
| --- | --- | --- |
| **Amsterdam** | Equinix, Telecity | BICS, CloudXpress, Eurofiber, Fastweb S.p.A, Gulf köprüsü uluslararası, MainOne, Nianet, Post, Proximus, TDC Erhverv, Telekom Italia pırıltı, Telia |
| **Atlanta** | Equinix| Castle Dama yapma
| **Cape Town** | Teraco | MTN |
| **Chicago** | Equinix | Dama yapma Castle, Spektrumun Kurumsal Windstream |
| **Dallas** | Equinix, Megaport | Axtel, C3ntro Telekom Cox iş Dama yapma Castle veri Foundry Spektrumun Kurumsal Transtelco |
| **Frankfurt** | Telecity | BICS, Cinia, Nianet, QSC AG |
| **Hamburg** | Equinix | Cinia |
| **Hong Kong ÖİB** | Equinix | Baş, Macroview telekomünikasyon |
| **Johannesburg** | Teraco | MTN |
| **Londra** | BICS, Equinix, euNetworks, Telecity | Bezeq International Ltd, CoreAzure, Epsilon telekomünikasyon Limited, üstel E, HSO, NexGen ağları, Proximus, Tamares Telekom, Zain |
| **Los Angeles** | Equinix |Dama yapma Castle, Spektrumun Kurumsal Transtelco |
| **Madrid** | Level3 | Zertia |
| **Montreal** | Cologix, Equinix | Airgate Technologies, Inc. Cogeco eş Laura, Zirro 1 |
| **New York** |Equinix, Megaport | Altice iş Dama yapma Castle Spektrumun Kurumsal Webair |
| **Paris** | Equinix | Proximus |
| **Quebec City** | Megaport | Fibrenoire |
| **Sao Paula** | Equinix | Venha Pra Nuvem |
| **Seattle** |Equinix | Alaska Communications |
| **Silikon Vadisi** |Coresite, Equinix | Cox Business, Spectrum Enterprise, Windstream, X2nsat Inc. |
| **Singapur** |Equinix |1CLOUDSTAR, BICS, Epsilon Telecommunications Limited, LGA Telecom, United Information Highway (UIH) |
| **Slough** | Equinix | HSO|
| **Sidney** | Megaport | Macquarie Telecom Group|
| **Tokyo** | Equinix | ARTERIA Networks Corporation, BroadBand Tower, Inc. |
| **Toronto** | Equinix, Megaport | Airgate Technologies Inc., Beanfield Metroconnect, Cogeco Peer 1, IVedha Inc, Rogers, Thinktel, Zirro|
| **Washington DC** |Equinix | Altice iş, BICS, Cox iş, dama yapma Castle, Gtt iletişimleri Inc. Epsilon telekomünikasyon Limited, Masergy, Windstream |

## <a name="expressroute-system-integrators"></a>ExpressRoute sistem tümleştiricileri
İhtiyaçlarınıza uyan özel bağlantıyı etkinleştirme ağınızın ölçeğine bağlı olarak zorlu olabilir. ExpressRoute’a yönelik ekleme işleminde size yardımcı olmak üzere aşağıdaki tabloda listelenen herhangi bir sistem tümleştirici ile çalışabilirsiniz.

| **Continent** | **Sistem tümleştiricileri** |
| --- | --- |
| **Asya** |Avanade Inc., OneAs1a |
| **Avustralya** | Ensyst, IT Consultancy, MOQdigital, Vigilant.IT |
| **Avrupa** |Avanade Inc., Altogee, Bright Skies GmbH, Inframon, MSG Services, New Signature, Nelite, Orange Networks, sol-tec |
| **Kuzey Amerika** |Avanade Inc., Equinix Professional Services, FlexManage, Lightstream, Perficient, Presidio |
| **Güney Amerika** |Avanade Inc., Venha Pra Nuvem |
## <a name="next-steps"></a>Sonraki adımlar
* ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
* Tüm önkoşulların sağlandığından emin olun. Bkz. [ExpressRoute önkoşulları](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Konum eşleme"
