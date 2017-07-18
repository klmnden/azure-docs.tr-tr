---
title: "Bağlantı sağlayıcıları ve konumları: Azure ExpressRoute | Microsoft Docs"
description: "Bu makale, sunulan hizmetlerin konumları ve Azure bölgelerine nasıl bağlanılacağı hakkında ayrıntılı bir genel bakış sağlar. Bağlantı sağlayıcısına göre sıralanır."
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
ms.assetid: c878513a-d594-42ad-8b0e-403efd0c4b25
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/03/2017
ms.author: kaanan
ms.translationtype: Human Translation
ms.sourcegitcommit: b1d56fcfb472e5eae9d2f01a820f72f8eab9ef08
ms.openlocfilehash: 8fdf343f2d70dce4f9457277affcfd6e5dae3b78
ms.contentlocale: tr-tr
ms.lasthandoff: 07/06/2017


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

### <a name="azure-regions-to-expressroute-locations-within-a-geopolitical-region"></a>Bir coğrafi bölge içindeki Azure bölgeler ile ExpressRoute konumları arasında eşleme.
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

Aşağıdaki tabloda hizmet sağlayıcısına göre konumlar gösterilmektedir. Kullanılabilir sağlayıcıları konuma göre görüntülemek istiyorsanız bkz. [Konuma göre hizmeti sağlayıcılar](expressroute-locations-providers.md#locations).


### <a name="production-azure"></a>Üretim Azure
| **Hizmet sağlayıcı** | **Microsoft Azure** | **Office 365 ve Dynamics 365** | **Konumlar** |
| --- | --- | --- | --- |
| **[AARNet](https://www.aarnet.edu.au/network-and-services/cloud-services-applications/azure-expressroute/)** |Destekleniyor |Destekleniyor |Melbourne, Sidney |
| **[Airtel](http://www.airtel.in/business/connexion)** | Destekleniyor | Destekleniyor | Chennai, Mumbai |
| **[Aryaka Networks](http://www.aryaka.com/)** |Destekleniyor |Destekleniyor |Amsterdam, Dallas, Hong Kong, Silikon Vadisi, Singapur, Tokyo, Washington DC |
| **[Ascenty Data Centers](https://ascenty.com/solucoes/conectividade-e-interconexoes/Microsoft-express-route/)** |Çok yakında |Çok yakında |Sao Paulo |
| **[AT&T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** |Destekleniyor |Destekleniyor |Amsterdam, Chicago, Dallas, Londra, Silikon Vadisi, Singapur, Sidney, Tokyo, Toronto, Washington |
| **[Bell Canada](https://business.bell.ca/shop/enterprise/cloud-connect-access-to-cloud-partner-services)** |Destekleniyor |Destekleniyor |Montreal, Toronto |
| **[British Telecom](http://www.globalservices.bt.com/uk/en/news/bt_to_provide_connectivity_to_microsoft_azure)** |Destekleniyor |Destekleniyor |Amsterdam, Hong Kong, Londra, Silikon Vadisi, Singapur, Sidney, Tokyo, Washington DC |
| **[CenturyLink](http://www.centurylink.com/business/enterprise/services/data-network/mpls-vpn.html)** |Çok yakında |Çok yakında |Silikon Vadisi |
| **China Telecom Global** |Destekleniyor |Desteklenmiyor |Hong Kong |
| **[Cologix](http://www.cologix.com/solutions/cloud-connect/public-clouds/microsoft-cloud/)** |Destekleniyor |Destekleniyor |Dallas, Montreal, Toronto |
| **[Colt](http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** |Destekleniyor |Destekleniyor |Amsterdam, Dublin, Londra, Paris, Tokyo |
| **Comcast** |Destekleniyor |Destekleniyor |Chicago, Silikon Vadisi, Washington DC |
| **[Console](https://www.consoleconnect.com/partners/cloudsaas/)**| Destekleniyor | Destekleniyor |Silicon Valley, Toronto |
| **[CoreSite](http://www.coresite.com/solutions/cloud-services/public-cloud-providers/microsoft-azure-expressroute)** |Destekleniyor |Destekleniyor |Denver, Los Angeles, New York |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Destekleniyor |Destekleniyor |Amsterdam, Atlanta, Chicago, Dallas, Hong Kong, Londra, Los Angeles, Melbourne, New York, Osaka, Paris, Sao Paulo, Seattle, Silikon Vadisi, Singapur, Sidney, Tokyo, Toronto, Washington DC |
| **euNetworks** |Destekleniyor |Destekleniyor |Amsterdam |
| **GÉANT** |Destekleniyor |Destekleniyor |Amsterdam |
| **[Global Cloud Xchange (GCX)] (http://globalcloudxchange.com/cloud-platform/cloud-x-fusion/cloud-x-fusion-for-azure/)** | Destekleniyor| Destekleniyor | Chennai |
| **[InterCloud](https://www.intercloud.com/)** |Destekleniyor |Destekleniyor |Amsterdam, Londra, Singapur, Washington DC |
| **[Internet Initiative Japan Inc. - IIJ](http://www.iij.ad.jp/en/news/pressrelease/2015/1216-2.html)** |Destekleniyor |Destekleniyor |Osaka, Tokyo |
| **Internet Solutions - Cloud Connect** |Destekleniyor |Destekleniyor |Amsterdam, Londra |
| **[Interxion](http://www.interxion.com/why-interxion/colocate-with-the-clouds/Microsoft-Azure/)** |Destekleniyor |Destekleniyor |Amsterdam, Londra, Paris |
| **Jisc** |Destekleniyor |Destekleniyor |Londra |
| **KINX** |Destekleniyor |Destekleniyor |Seul |
| **[KPN](http://www.kpn.com/cloudconnect)** | Destekleniyor | Destekleniyor | Amsterdam | 
| **[Level 3 Communications](http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** |Destekleniyor |Destekleniyor |Amsterdam, Chicago, Dallas, Las Vegas+, Londra, Sao Paulo, Seattle, Silikon Vadisi, Singapur, Washington |
| **LG CNS** |Destekleniyor |Destekleniyor |Busan |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Destekleniyor |Destekleniyor |Amsterdam, Chicago, Dallas, Hong Kong, Las Vegas, Londra, Los Angeles, Melbourne, Miami, New York, Quebec City, San Antonio, Seattle, Silikon Vadisi, Singapur, Sidney, Toronto, Washington |
| **MTN** |Destekleniyor |Destekleniyor |Londra |
| **[Next Generation Data](http://www.nextgenerationdata.co.uk/ngd-cloud-gateway/)** |Destekleniyor |Destekleniyor |Newport(Galler) |
| **NEXTDC** |Destekleniyor |Destekleniyor |Melbourne, Sidney |
| **[NTT Communications](http://www.ntt.com/en/services/network/virtual-private-network.html)** |Destekleniyor |Destekleniyor |Londra, Los Angeles, Osaka, Singapur, Tokyo, Washington DC |
| **[NTT SmartConnect](http://cloud.nttsmc.com/cxc/azure.html)** |Destekleniyor |Destekleniyor |Osaka |
| **[Orange](http://www.orange-business.com/en/products/business-vpn-galerie)** |Destekleniyor |Destekleniyor |Amsterdam, Hong Kong, Londra, Paris+, Silikon Vadisi, Singapur, Sidney, Washington |
| **PCCW Global Limited** |Destekleniyor |Destekleniyor |Hong Kong |
| **Sejong Telecom** |Destekleniyor |Destekleniyor |Seul |
| **[SIFY](http://telecom.sify.com/azure-expressroute.html)** |Destekleniyor |Destekleniyor |Chennai |
| **[SingTel](http://info.singtel.com/about-us/news-releases/singtel-provide-secure-private-access-microsoft-azure-public-cloud)** |Destekleniyor |Destekleniyor |Singapur |
| **[Softbank](http://www.softbank.jp/biz/cloud/cloud_access/direct_access_for_az/)** |Destekleniyor |Destekleniyor |Osaka, Tokyo |
| **[Tata Communications](http://www.tatacommunications.com/lp/izo/azure/azure_index.html)** |Destekleniyor |Destekleniyor |Amsterdam, Chennai, Hong Kong, Londra, Mumbai, Silikon Vadisi, Singapur, Washington DC |
| **[TeleCity Group](http://www.telecitygroup.com/investor-centre/news_details.htm?locid=03100500400b00d&xml)** |Destekleniyor |Destekleniyor |Amsterdam, Dublin, Londra |
| **[Telefonica](https://www.business-solutions.telefonica.com/es/enterprise/solutions/efficient-infrastructure/managed-voice-data-connectivity/)** |Destekleniyor |Destekleniyor |Amsterdam, Sao Paulo |
| **[Telehouse - KDDI](http://www.telehouse.net/solutions/cloud-services/cloud-link)** |Destekleniyor |Destekleniyor |Londra |
| **Telenor** |Destekleniyor |Destekleniyor |Amsterdam, Londra |
| **[Telstra Corporation](http://www.telstra.com.au/business-enterprise/network-services/networks/cloud-direct-connect/)** |Destekleniyor |Destekleniyor |Melbourne, Sidney |
| **[Telus](http://www.telus.com)** |Destekleniyor |Destekleniyor |Toronto |
| **[UOLDIVEO](http://www.uoldiveo.com.br/solucoes/cloud.html#rmcl)** |Çok yakında |Çok yakında |Sao Paulo+ |
| **[Verizon](http://www.verizonenterprise.com/products/networking/secure-cloud-interconnect/)** |Destekleniyor |Destekleniyor |Amsterdam, Chicago, Dallas, Hong Kong, Londra, Silikon Vadisi, Singapur, Sidney, Tokyo, Washington DC |
| **[Vodafone](http://www.vodafone.com/business/global-enterprise/global-connectivity/vodafone-ip-vpn-cloud-connect)** |Destekleniyor |Desteklenmiyor |Londra |
| **[Zayo Group](http://www.zayo.com/solutions/industries/cloud-connectivity/microsoft-expressroute)** |Destekleniyor |Destekleniyor |Amsterdam, Chicago, Dallas+, Londra+, Los Angeles, New York, Silikon Vadisi, Toronto, Washington |

 **+** çok yakında anlamına geliyor

### <a name="national-cloud-environment"></a>Ulusal bulut ortamı

### <a name="us-government-cloud"></a>US Government bulutu
| **Hizmet sağlayıcı** | **Microsoft Azure** | **Office 365** | **Konumlar** |
| --- | --- | --- | --- |
| **[AT&T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** |Destekleniyor |Destekleniyor |Chicago, Washington DC |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Destekleniyor |Destekleniyor |Chicago, Dallas, New York, Seattle, Silikon Vadisi, Washington DC |
| **[Level 3 Communications](http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** |Destekleniyor |Destekleniyor |Chicago, New York+, Silikon Vadisi, Washington |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Destekleniyor | Destekleniyor | Chicago, Dallas |
| **[Verizon](http://news.verizonenterprise.com/2014/04/secure-cloud-interconnect-solutions-enterprise/)** |Destekleniyor |Destekleniyor |Chicago, Dallas, New York, Washington DC |

### <a name="china"></a>Çin
| **Hizmet sağlayıcı** | **Microsoft Azure** | **Office 365** | **Konumlar** |
| --- | --- | --- | --- |
| **China Telecom** |Destekleniyor |Desteklenmiyor |Pekin, Şangay |

Daha fazla öğrenmek için, bkz. [Çin’de ExpressRoute](http://www.windowsazure.cn/home/features/expressroute/).

### <a name="germany"></a>Almanya
| **Hizmet sağlayıcı** | **Microsoft Azure** | **Office 365** | **Konumlar** |
| --- | --- | --- | --- |
| **[Colt](http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** |Destekleniyor |Desteklenmiyor |Berlin+, Frankfurt |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Destekleniyor |Desteklenmiyor |Frankfurt |
| **[e-shelter](https://www.e-shelter.de/en/microsoft-expressroutetm)** |Destekleniyor |Desteklenmiyor |Berlin |
| **Interxion** |Destekleniyor |Desteklenmiyor |Frankfurt |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Destekleniyor  | Desteklenmiyor | Berlin |

## <a name="connectivity-through-exchange-providers"></a>Exchange Sağlayıcıları Üzerinden Bağlantı

Bağlantı sağlayıcınız önceki bölümlerde listelenmemişse hala bağlantı oluşturabilirsiniz.

* Yukarıdaki tabloda yer alan değişimlerin herhangi birine bağlı olup olmadığını görmek için bağlantı sağlayıcınıza başvurun. Değişim sağlayıcıları tarafından sunulan hizmetler hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kontrol edebilirsiniz. Birkaç bağlantı sağlayıcı Ethernet değişimlerine zaten bağlı.
  * [Cologix](http://www.cologix.com/)
  * [Console](https://www.consoleconnect.com/partners/cloudsaas/)
  * [CoreSite](http://www.coresite.com/)
  * [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
  * [Interxion](http://www.interxion.com/products/interconnection/cloud-connect/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [NextDC](http://www.nextdc.com/)
  * [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
* Bağlantı sağlayıcınızı, ağınızı seçtiğiniz eşleme konumuna genişletmesini sağlayın.
  * Bağlantı sağlayıcınızın bağlantınızı yüksek oranda kullanılabilir şekilde genişlettiğinden emin olun, böylece hiç tek nokta arızası olmaz.
* Microsoft’a bağlanmak için bağlantı sağlayınız olarak değişime sahip bir ExpressRoute bağlantı hattı sipariş edin.
  * Bağlantı kurmak için [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-classic.md)’daki adımları izleyin.

## <a name="connectivity-through-additional-service-providers"></a>Diğer Hizmet Sağlayıcılar Üzerinden Bağlantı

| **Bağlantı sağlayıcı** | **Exchange** | **Konumlar** |
| --- | --- | --- |
| **[1CLOUDSTAR](http://www.1cloudstar.com/service/cloudconnect-azure-expressroute/)** |Equinix |Singapur |
| **[Airgate Technologies, Inc.](http://airgate.ca/cloud-express/)** | Equinix, Cologix | Toronto, Montreal |
| **[Alaska Communications](http://www.alaskacommunications.com/For-Your-Business/Direct-Cloud-Service)** |Equinix |Seattle |
| **[Altice Business](https://golightpath.com/transport)** |Equinix |New York, Washington DC |
| **[Arteria Networks Corporation](https://arteria-net.com/business/service/cloud_access/sca/)** |Equinix |Tokyo |
| **[Bezeq International Ltd.](https://www.bezeqint.net/english)** | euNetworks | Londra |
| **[BroadBand Tower, Inc.](http://www.bbtower.co.jp/product-service/data-center/network/dcconnect-for-azure/)** | Equinix | Tokyo |
| **[C3ntro Telecom](http://www.c3ntro.com/data/cloud-conectivity/)** | Equinix, Megaport | Dallas |
| **[Cogeco Peer 1](https://www.cogecopeer1.com/en/)**| Equinix | Montreal, Toronto |
| **[Cox Business](https://www.cox.com/business/networking/cloud-connectivity.html)** | Equinix | Dallas, Silikon Vadisi, Washington DC | 
| **[Data Foundry](https://www.datafoundry.com/services/cloud-connect)** | Megaport | Dallas |
| **[Epsilon Telecommunications Limited](http://www.epsilontel.com/data-connectivity/cloud-access/)** | Equinix | Londra, Singapur, Washington DC |
| **[Eurofiber](https://eurofiber.nl/microsoft-azure/)** | Equinix | Amsterdam |
| **[Exponential E](http://www.exponential-e.com/services/connectivity-services/cloud-connect-exchange)** | Equinix | Londra |
| **[Fastweb S.p.A](http://www.fastweb.it/grandi-aziende/connessione-voce-e-wifi/scheda-prodotto/rete-privata-virtuale/)** | Equinix | Amsterdam |
| **[Gtt Communications Inc](https://www.gtt.net/wp-content/uploads/2017/04/EtherCloud-Data-Sheet.pdf)** |Equinix | Washington DC |
| **[HSO](http://www.hso.co.uk/products/cloud-direct)** |Equinix | Londra, Slough |
| **LGA Telecom** |Equinix |Singapur|
| **[Lightower](http://www.lightower.com/network-solutions/cloud-connect/#microsoft-azure)** |Equinix | Chicago, New York, Washington DC |
| **[Macroview Telecom](http://www.macroview.com/en/scripts/catitem.php?catid=solution&sectionid=expressroute)** |Equinix |Hong Kong |
| **[Macquarie Telecom Group](https://macquariegovernment.com/secure-cloud/secure-cloud-exchange/)** | Megaport | Sidney |
| **[Masergy](https://www.masergy.com/solutions/hybrid-networking/cloud-marketplace/microsoft-azure)** | Equinix | Washington DC |
| **[NexGen Networks](http://www.nexgen-net.com/nexgen-networks-direct-connect-microsoft-azure-expressroute.html)** | Interxion | Londra |
| **[Nianet](https://nianet.dk/produkter/internet/microsoft-expressroute)** |Telecity | Amsterdam, Frankfurt |  
| **[QSC AG](https://www.qsc.de/de/produkte-loesungen/cloud-services-und-it-outsourcing/pure-enterprise-cloud/multi-cloud-management/azure-expressroute/)** |Interxion | Frankfurt |  
| **Rogers** | Cologix, Equinix | Montreal, Toronto |
| **[Tamares Telecom](http://www.tamarestelecom.com/our-services/#Connectivity)** | Telecity | Londra | 
| **[ThinkTel](http://www.thinktel.ca/services/agile-ix-data/expressroute/)** | Equinix | Toronto | 
| **[Transtelco](http://www.transtelco.net/tcloud/microsoft)** |Equinix | Dallas, Los Angeles |  
| **[United Information Highway (UIH)](https://www.uih.co.th/en/internet-solution/cloud-direct/uih-cloud-direct-for-microsoft-azure-expressroute)**| Equinix | Singapur |
| **[Webair](https://www.webair.com/microsoft-express-route-partnership/)**| Megaport | New York |
| **[Windstream](http://www.windstreambusiness.com/solutions/cloud-services/cloud-and-managed-hosting-services)**| Equinix | Chicago, Silikon Vadisi, Washington DC |
| **Zain** |Equinix |Londra|
| **[Zertia](http://www.zertia.es/index.php/novedades)**| Level 3 | Madrid |
| **[Zirro](https://zirro.com/services/)**| Equinix | Montreal, Toronto |

## <a name="connectivity-through-datacenter-providers"></a>Veri Merkezi Sağlayıcıları Üzerinden Bağlantı
| **Sağlayıcı** | **Exchange** |
| --- | --- |
| **[Cyrus One](https://cyrusone.com/enterprise-data-center-services/connectivity-and-interconnection/cloud-connectivity-reaching-amazon-microsoft-google-and-more/microsoft-azure-expressroute/?doing_wp_cron=1498512235.6733090877532958984375)** | Megaport |
| **[Digital Realty](https://www.digitalrealty.com/services/interconnection/service-exchange/)** | Megaport |
| **[EdgeConnex](http://www.edgeconnex.com/services/edge-data-centers-proximity-matters/)** | Megaport |
| **[RagingWire Data Centers](http://www.ragingwire.com/wholesale/wholesale-data-centers-worldwide-nexcenters)** | Konsol |
| **[T5 Datacenters](http://t5datacenters.com/network-cloud-connect/)** | Konsol |

## <a name="connectivity-through-national-research-and-education-networks-nren"></a>Ulusal Araştırma ve Eğitim Ağları (NREN) Üzerinden Bağlantı

| **Sağlayıcı**|
| --- |
| **AARNET**| 
| **DeIC, GÉANT üzerinden**|
| **GARR, GÉANT üzerinden**|
| **GÉANT**|
| **HEAnet, GÉANT üzerinden**|
| **JISC**|
| **RedIRIS, GÉANT üzerinden**|
| **SINET**|
| **Surfnet, GÉANT üzerinden**|

* Bağlantı sağlayıcınız bu listede yoksa, lütfen yukarıda listelenen ExpressRoute Exchange İş Ortaklarından herhangi birine bağlı olup olmadığınızı denetleyin.

## <a name="expressroute-system-integrators"></a>ExpressRoute sistem tümleştiricileri
İhtiyaçlarınıza uyan özel bağlantıyı etkinleştirme ağınızın ölçeğine bağlı olarak zorlu olabilir. ExpressRoute’a yönelik ekleme işleminde size yardımcı olmak üzere aşağıdaki tabloda listelenen herhangi bir sistem tümleştirici ile çalışabilirsiniz.

| **Sistem tümleştirici** | **Continent** |
| --- | --- |
| **[Altogee](http://www.altogee.be/expressroute)** | Avrupa |
| **[Avanade Inc.](http://www.avanade.com/)** | Asya, Avrupa, Kuzey Amerika, Güney Amerika |
| **[Bright Skies GmbH](http://bskies.io/expressroute)** | Avrupa
| **[Ensyst](http://www.ensyst.com.au)** | Asya
| **[Equinix Professional Services](http://www.equinix.com/services/consulting/)** | Kuzey Amerika |
| **[FlexManage](http://www.flexmanage.com/cloud)** | Kuzey Amerika |
| **[Inframon](http://www.inframon.com/partner/microsoft/)** | Avrupa |
| **[The IT Consultancy Group](http://itconsult.com.au/microsoft-expressroute)** | Avustralya |
| **[MOQdigital](http://www.moqdigital.com.au/insights/technical/network-connectivity-options-for-azure)** | Avustralya |
| **[MSG Services](https://www.msg-services.de/it-services/managed-services/cloud-outsourcing/)** | Avrupa (Almanya) |
| **[Nelite](http://nelite.com/)** | Avrupa |
| **[New Signature](https://www.newsignature.co.uk/)** | Avrupa |
| **[OneAs1a](http://www.oneas1a.com/express-connect-any-cloud-ecac)** | Asya |
| **[Orange Networks](https://orange-networks.com/blog/88-azureexpressroute)** | Avrupa |
| **[Perficient](http://www.perficient.com/Partners/Microsoft/Cloud/Azure-ExpressRoute)** | Kuzey Amerika |
| **[Presidio](http://info.presidio.com/microsoft-azure-expressroute)** | Kuzey Amerika |
| **[sol-tec](http://www.sol-tec.com/Technologies)** | Avrupa |
| **[Vigilant.IT](https://vigilant.it/expressroute)** | Avustralya |


## <a name="next-steps"></a>Sonraki adımlar
* ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
* Tüm önkoşulların sağlandığından emin olun. Bkz. [ExpressRoute önkoşulları](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Konum eşleme"

