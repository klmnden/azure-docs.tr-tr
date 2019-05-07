---
title: 'Bağlantı sağlayıcıları ve konumları: Azure ExpressRoute | Microsoft Docs'
description: Bu makale, sunulan hizmetlerin konumları ve Azure bölgelerine nasıl bağlanılacağı hakkında ayrıntılı bir genel bakış sağlar. Bağlantı sağlayıcısına göre sıralanır.
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
ms.assetid: c878513a-d594-42ad-8b0e-403efd0c4b25
ms.service: expressroute
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/06/2019
ms.author: pareshmu
ms.openlocfilehash: 96839a42eb9dbd947b36522122a28ef81946a99c
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65191183"
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

| **Jeopolitik bölge** | **Bölge** | **Azure bölgeleri** | **ExpressRoute konumları** |
| --- | --- | --- | --- |
| **Australia Government** | 1 | Avustralya Orta, Avustralya Orta 2 |Kanberra, Kanberra2 |
| **Avrupa** | 1 |Fransa Orta, Fransa Güney, Kuzey Avrupa, Batı Avrupa, UK Batı, UK Güney |Amsterdam, Amsterdam2, Dublin, Frankfurt, Londra, London2, Marsilya'daki, newport(Galler), Paris, Zürih |
| **Kuzey Amerika** | 1 |Doğu ABD, Batı ABD, Doğu ABD 2, Batı ABD 2, Orta ABD, Orta Güney ABD, Orta Kuzey ABD, Batı Orta ABD, Orta Kanada, Doğu Kanada |Atlanta, Chicago, Dallas, Denver, Las Vegas, Los Angeles, Miami, New York, San Antonio, Seattle, Silikon vadisi, Silikon Valley2, DC2 Washington DC, Washington, Montreal, Quebec City, Toronto |
| **Asya** | 2 |Doğu Asya, Güneydoğu Asya |Hong Kong ÖİB, Kuala Lumpur, Singapur, singapur2 Taipei |
| **Avustralya** | 2 |Güneydoğu Avustralya, Doğu Avustralya |Melbourne, Perth, Sidney | 
| **Hindistan** | 2 |Batı Hindistan, Orta Hindistan, Güney Hindistan |Madras, Madras2, Bombay, Bombay2 |
| **Japonya** | 2 |Batı Japonya, Doğu Japonya |Osaka, Tokyo |
| **Güney Kore** | 2 |Kore Orta, Kore Güney |Busan, Seul|
| **BAE** | 3 | BAE Orta, BAE Kuzey | Dubai |
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

Aşağıdaki tabloda hizmet sağlayıcısına göre konumlar gösterilmektedir. Kullanılabilir sağlayıcıları konuma göre görüntülemek istiyorsanız bkz. [Konuma göre hizmeti sağlayıcılar](expressroute-locations-providers.md#locations).


### <a name="production-azure"></a>Üretim Azure

| **Hizmet sağlayıcı** | **Microsoft Azure** | **Office 365 ve Dynamics 365** | **Konumlar** |
| --- | --- | --- | --- |
| **[AARNet](https://www.aarnet.edu.au/network-and-services/cloud-services-applications/azure-expressroute/)** |Desteklenen |Desteklenen |Melbourne, Sidney |
| **[Airtel](https://www.airtel.in/creatingsmiles/)** | Desteklenen | Desteklenen | Madras2, Madras2 |
| **[Aryaka Networks](https://www.aryaka.com/)** |Desteklenen |Desteklenen |Amsterdam, Chicago, Dallas, Hong Kong ÖİB, Sao Paulo, Seattle, Silikon vadisi, Singapur, Tokyo, Washington DC |
| **[Ascenty Data Centers](https://ascenty.com/servicos/cloud-connect/microsoft-expressroute/)** |Desteklenen |Desteklenen |Sao Paulo |
| **[AT&T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** |Desteklenen |Desteklenen |Amsterdam, Chicago, Dallas, Londra, Silikon Vadisi, Singapur, Sidney, Tokyo, Toronto, Washington DC |
| **[Bell Canada](https://business.bell.ca/shop/enterprise/cloud-connect-access-to-cloud-partner-services)** |Desteklenen |Desteklenen |Montreal, Toronto, Quebec City |
| **[British Telecom](https://www.globalservices.bt.com/en/solutions/products/bt-compute-for-microsoft-azure)** |Desteklenen |Desteklenen |Amsterdam, Hong Kong ÖİB, Londra, newport(Galler), Sao Paulo, Silikon vadisi, Singapur, Sidney, Tokyo, Washington DC |
| **C3ntro** |Çok yakında |Çok yakında |Miami |
| **CDC** | Desteklenen | Desteklenen | Kanberra, Kanberra2 |
| **[CenturyLink Cloud Connect](https://www.centurylink.com/cloudconnect)** |Desteklenen |Desteklenen |Las Vegas, New York, San Antonio, Silikon Vadisi, Tokyo, Toronto |
| **Baş telekomünikasyon** |Desteklenen |Desteklenen |Taipei |
| **China Telecom Global** |Desteklenen |Desteklenmiyor |Hong Kong Çin ÖİB |
| **[Cologix](https://www.cologix.com/solutions/cloud-connect/public-clouds/microsoft-cloud/)** |Desteklenen |Desteklenen |Chicago, Dallas, Montreal, Toronto, Washington DC |
| **[Colt](https://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** |Desteklenen |Desteklenen |Amsterdam, Dublin, Londra, Paris, singapur2, Tokyo |
| **[Comcast](https://business.comcast.com/landingpage/microsoft-azure)** |Desteklenen |Desteklenen |Chicago, Silikon Vadisi, Washington DC |
| **[CoreSite](https://www.coresite.com/solutions/cloud-services/public-cloud-providers/microsoft-azure-expressroute)** |Desteklenen |Desteklenen |Chicago, Denver, Los Angeles, New York, Silikon Valley2, Silikon vadisi, Washington DC, Washington DC2 |
| **[DE-CIX](https://www.de-cix.net/en/de-cix-service-world/directcloud/find-a-cloud-service/detail/microsoft-azure)** | Desteklenen |Desteklenen |Amsterdam2|
| **eir** |Desteklenen |Desteklenen |Dublin|
| **Epsilon Global Communications** |Desteklenen |Desteklenen |Singapur, Singapur2 |
| **[Equinix](https://www.equinix.com/partners/microsoft-azure/)** |Desteklenen |Desteklenen |Amsterdam, Atlanta, Chicago, Dallas, Dublin, Hong Kong ÖİB, Londra, London2, Los Angeles, Melbourne, Miami, New York, Osaka, Paris, Sao Paulo, Seattle, Silikon vadisi, Singapur, Sidney, Tokyo, Toronto, Washington DC |
| **Etisalat BAE** |Desteklenen |Desteklenen |Dubai|
| **euNetworks** |Desteklenen |Desteklenen |Amsterdam, Dublin, Londra |
| **FarEasTone** |Desteklenen |Desteklenen |Taipei|
| **GÉANT** |Desteklenen |Desteklenen |Amsterdam |
| **[Genel Bulut Değişimi (GCX)](https://globalcloudxchange.com/cloud-platform/cloud-x-fusion/)** | Desteklenen| Desteklenen | Chennai, Mumbai |
| **[InterCloud](https://www.intercloud.com/)** |Desteklenen |Desteklenen |Amsterdam, Chicago, Londra, New York, Paris, Silikon vadisi, Singapur, Washington DC |
| **Internet2** |Desteklenen |Desteklenen |Chicago, Dallas, Washington DC |
| **[Internet Initiative Japan Inc. - IIJ](https://www.iij.ad.jp/en/news/pressrelease/2015/1216-2.html)** |Desteklenen |Desteklenen |Osaka, Tokyo |
| **[Internet Solutions - Cloud Connect](https://www.is.co.za/solution/cloud-connect/)** |Desteklenen |Desteklenen |Cape Town, Johannesburg, Londra |
| **[Interxion](https://www.interxion.com/why-interxion/colocate-with-the-clouds/Microsoft-Azure/)** |Desteklenen |Desteklenen |Amsterdam, Amsterdam2, Dublin, Frankfurt, Londra, Marsilya'daki, Paris, Zürih |
| **[IX Reach](https://www.ixreach.com/partners/cloud-partners/microsoft-azure/)**|Desteklenen |Desteklenen | Amsterdam, London2, Silikon vadisi, Toronto |
| **Jisc** |Desteklenen |Desteklenen |Londra |
| **[KINX](https://www.kinx.net/service/network/cloudhub/ms-expressroute/?lang=en)** |Desteklenen |Desteklenen |Seul |
| **[Kordia](https://www.kordia.co.nz/cloudconnect)** | Desteklenen |Desteklenen |Sidney |
| **[KPN](https://www.kpn.com/zakelijk/cloud/connect.htm)** | Desteklenen | Desteklenen | Amsterdam | 
| **[Level 3 Communications](http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** |Desteklenen |Desteklenen |Amsterdam, Chicago, Dallas, Londra, Newport (Galler), Sao Paulo, Seattle, Silikon vadisi, Singapur, Washington DC |
| **LG CNS** |Desteklenen |Desteklenen |Busan, Seul |
| **[Liquid Telecom](https://www.liquidtelecom.com/products-and-services/cloud.html)** |Desteklenen |Desteklenen |Cape Town, Johannesburg |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Desteklenen |Desteklenen |Amsterdam, Atlanta, Chicago, Dallas, Denver, Dublin, Hong Kong ÖİB, Las Vegas, Londra, Los Angeles, Melbourne, Miami, New York, Perth, Quebec City, San Antonio, Seattle, Silikon vadisi, Singapur, singapur2, Sidney, Toronto, Washington DC |
| **[MTN](https://www.mtnbusiness.com/en/enterprise/Pages/microsoft-express-route.aspx)** |Desteklenen |Desteklenen |Londra |
| **[Neutrona Networks](http://www.neutrona.com/index.php/azure-expressroute/)** |Desteklenen |Desteklenen |Dallas, Los Angeles, Miami, Sao Paulo |
| **[Next Generation Data](https://www.nextgenerationdata.co.uk/ngd-cloud-gateway/)** |Desteklenen |Desteklenen |Newport(Galler) |
| **[NEXTDC](https://www.nextdc.com/services/axon-ethernet/microsoft-expressroute)** |Desteklenen |Desteklenen |Melbourne, Perth, Sidney |
| **[NTT Communications](https://www.ntt.com/en/services/network/virtual-private-network.html)** |Desteklenen |Desteklenen |Amsterdam, Hong Kong ÖİB, Londra, Los Angeles, Osaka, Singapur, Sidney, Tokyo, Washington DC |
| **[NTT EAST](https://flets.com/cloudgateway/crossconnect/)** |Desteklenen |Desteklenen |Tokyo |
| **[NTT SmartConnect](https://cloud.nttsmc.com/cxc/azure.html)** |Desteklenen |Desteklenen |Osaka |
| **[Optus](https://www.optus.com.au/enterprise/networking/network-connectivity/express-link)** |Desteklenen |Desteklenen |Melbourne, Sidney |
| **[Orange](https://www.orange-business.com/en/products/business-vpn-galerie)** |Desteklenen |Desteklenen |Amsterdam, Hong Kong ÖİB, Londra, Paris, Sao Paulo, Silikon vadisi, Singapur, Sidney, Tokyo, Washington DC |
| **[PacketFabric](https://www.packetfabric.com/packetcor/microsoft-azure/)** |Desteklenen |Desteklenen |Chicago, Silikon Vadisi, Washington DC |
| **[PCCW Global Limited](https://consoleconnect.com/clouds/#azureRegions)** |Desteklenen |Desteklenen |Chicago, Hong Kong ÖİB, Londra |
| **[Sejong Telecom](https://www.sejongtelecom.net/en/pages/service/cloud_ms)** |Desteklenen |Desteklenen |Seul |
| **[SIFY](http://telecom.sify.com/azure-expressroute.html)** |Desteklenen |Desteklenen |Madras, Bombay2 |
| **[SingTel](http://info.singtel.com/about-us/news-releases/singtel-provide-secure-private-access-microsoft-azure-public-cloud)** |Desteklenen |Desteklenen |Singapur, Singapur2 |
| **[Softbank](https://www.softbank.jp/biz/cloud/cloud_access/direct_access_for_az/)** |Desteklenen |Desteklenen |Osaka, Tokyo |
| **[Sprint](https://business.sprint.com/solutions/cloud-networking/)** |Desteklenen |Desteklenen |Chicago, Silikon Vadisi, Washington DC |
| **[Tata Communications](https://www.tatacommunications.com/lp/izo/azure/azure_index.html)** |Desteklenen |Desteklenen |Amsterdam, Chennai, Hong Kong ÖİB, Londra, Mumbai, Sao Paulo, Silikon vadisi, Singapur, Washington DC |
| **Telecity Group** |Desteklenen |Desteklenen |Amsterdam |
| **[Telefonica](https://www.business-solutions.telefonica.com/es/enterprise/solutions/efficient-infrastructure/managed-voice-data-connectivity/)** |Desteklenen |Desteklenen |Amsterdam, Sao Paulo |
| **[Telehouse - KDDI](https://www.telehouse.net/solutions/cloud-services/cloud-link)** |Desteklenen |Desteklenen |Londra |
| **Telenor** |Desteklenen |Desteklenen |Amsterdam, Londra |
| **[Telia Carrier](https://teliacarrier.com/our-services/connectivity/cloud-connect.html?title=Cloud%20Connect)** | Desteklenen | Desteklenen |Amsterdam, Chicago, Dallas, Londra, Washington DC |
| **Telmex Uninet**| Desteklenen | Desteklenen | Dallas |
| **[Telstra Corporation](https://www.telstra.com.au/business-enterprise/network-services/networks/cloud-direct-connect/)** |Desteklenen |Desteklenen |Melbourne, Singapur, Sidney |
| **[Telus](https://www.telus.com)** |Desteklenen |Desteklenen |Montreal, Toronto |
| **[Teraco](https://www.teraco.co.za/services/africa-cloud-exchange/)** |Desteklenen |Desteklenen |Cape Town, Johannesburg |
| **[ZAMAN dotCom](https://www.time.com.my/enterprise/connectivity/cloud-interconnect)** | Desteklenen | Desteklenen | Kuala Lumpur |
| **[UOLDIVEO](https://www.uoldiveo.com.br/)** |Desteklenen |Desteklenen |Sao Paulo |
| **[Verizon](https://enterprise.verizon.com/products/network/application-enablement/secure-cloud-interconnect/)** |Desteklenen |Desteklenen |Amsterdam, Chicago, Dallas, Hong Kong ÖİB, Londra, Silikon vadisi, Singapur, Sidney, Tokyo, Washington DC |
| **[Vodafone](https://www.vodafone.com/business/global-enterprise/global-connectivity/vodafone-ip-vpn-cloud-connect)** |Desteklenen |Desteklenen |London, Singapur |
| **Vodafone Idea** | Desteklenen | Desteklenen | Mumbai, Mumbai2 |
| **[Zayo](https://www.zayo.com/solutions/industries/cloud-connectivity/microsoft-expressroute)** |Desteklenen |Desteklenen |Amsterdam, Chicago, Dallas, Denver, Londra, Los Angeles, Montreal, New York, Paris, Seattle, Silikon vadisi, Toronto, Washington DC |

 **+** çok yakında anlamına geliyor

### <a name="national-cloud-environment"></a>Ulusal bulut ortamı

### <a name="us-government-cloud"></a>US Government bulutu

| **Hizmet sağlayıcı** | **Microsoft Azure** | **Office 365** | **Konumlar** |
| --- | --- | --- | --- |
| **[AT&T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** |Desteklenen |Desteklenen |Chicago, Phoenix,  Washington DC |
| **[CenturyLink Cloud Connect](https://www.centurylink.com/cloudconnect)** |Desteklenen |Desteklenen |New York, Phoenix, San Antonio |
| **[Equinix](https://www.equinix.com/partners/microsoft-azure/)** |Desteklenen |Desteklenen |Chicago, Dallas, New York, Seattle, Silikon Vadisi, Washington DC |
| **[Level 3 Communications](http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** |Desteklenen |Desteklenen |Chicago, Silikon Vadisi, Washington DC |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Desteklenen | Desteklenen | Chicago, Dallas, San Antonio, Seattle, Washington DC |
| **[Verizon](http://news.verizonenterprise.com/2014/04/secure-cloud-interconnect-solutions-enterprise/)** |Desteklenen |Desteklenen |Chicago, Dallas, New York, Silikon Vadisi, Washington DC |

### <a name="china"></a>Çin

| **Hizmet sağlayıcı** | **Microsoft Azure** | **Office 365** | **Konumlar** |
| --- | --- | --- | --- |
| **China Telecom** |Desteklenen |Desteklenmiyor |Pekin, Şangay |
| **[GDS](http://en.gds-services.com/news_detail/newsId=21.html)** |Desteklenen |Desteklenmiyor |Beijing2, Shanghai2 |

Daha fazla öğrenmek için, bkz. [Çin’de ExpressRoute](http://www.windowsazure.cn/home/features/expressroute/).

### <a name="germany"></a>Almanya

| **Hizmet sağlayıcı** | **Microsoft Azure** | **Office 365** | **Konumlar** |
| --- | --- | --- | --- |
| **[Colt](https://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** |Desteklenen |Desteklenmiyor |Frankfurt |
| **[Equinix](https://www.equinix.com/partners/microsoft-azure/)** |Desteklenen |Desteklenmiyor |Frankfurt |
| **[e-shelter](https://www.e-shelter.de/en/microsoft-expressroutetm)** |Desteklenen |Desteklenmiyor |Berlin |
| **Interxion** |Desteklenen |Desteklenmiyor |Frankfurt |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Desteklenen  | Desteklenmiyor | Berlin |
| **T-Systems** |Desteklenen |Desteklenmiyor |Berlin |

## <a name="connectivity-through-exchange-providers"></a>Exchange Sağlayıcıları Üzerinden Bağlantı

Bağlantı sağlayıcınız önceki bölümlerde listelenmemişse hala bağlantı oluşturabilirsiniz.

* Yukarıdaki tabloda yer alan değişimlerin herhangi birine bağlı olup olmadığını görmek için bağlantı sağlayıcınıza başvurun. Değişim sağlayıcıları tarafından sunulan hizmetler hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kontrol edebilirsiniz. Birkaç bağlantı sağlayıcı Ethernet değişimlerine zaten bağlı.
  * [Cologix](https://www.cologix.com/)
  * [CoreSite](https://www.coresite.com/)
  * [Equinix Cloud Exchange](https://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
  * [Interxion](https://www.interxion.com/products/interconnection/cloud-connect/)
  * [IX Reach](https://www.ixreach.com/partners/cloud-partners/microsoft-azure/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [NextDC](https://www.nextdc.com/)
  * [PacketFabric](https://www.packetfabric.com/packetcor/microsoft-azure/) 
* Bağlantı sağlayıcınızı, ağınızı seçtiğiniz eşleme konumuna genişletmesini sağlayın.
  * Bağlantı sağlayıcınızın bağlantınızı yüksek oranda kullanılabilir şekilde genişlettiğinden emin olun, böylece hiç tek nokta arızası olmaz.
* Microsoft’a bağlanmak için bağlantı sağlayınız olarak değişime sahip bir ExpressRoute bağlantı hattı sipariş edin.
  * Bağlantı kurmak için [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-classic.md)’daki adımları izleyin.

## <a name="connectivity-through-additional-service-providers"></a>Diğer Hizmet Sağlayıcılar Üzerinden Bağlantı

| **Bağlantı sağlayıcı** | **Exchange** | **Konumlar** |
| --- | --- | --- |
| **[1CLOUDSTAR](https://www.1cloudstar.com/service/cloudconnect-azure-expressroute/)** |Equinix |Singapur |
| **[Airgate Technologies, Inc.](https://www.airgate.ca/expressroute)** | Equinix, Cologix | Toronto, Montreal |
| **[Alaska Communications](https://www.alaskacommunications.com/For-Your-Business/Direct-Cloud-Service)** |Equinix |Seattle |
| **[Altice Business](https://golightpath.com/transport)** |Equinix |New York, Washington DC |
| **[Arteria Networks Corporation](https://www.arteria-net.com/business/service/cloud/sca/)** |Equinix |Tokyo |
| **[Axtel](https://alestra.mx/landing/expressrouteazure/)** |Equinix |Dallas|
| **[Bezeq International Ltd.](https://www.bezeqint.net/english)** | euNetworks | Londra |
| **[BICS](https://bics.com/bics-solutions-suite/cloud-connect/)** | Equinix | Amsterdam, Frankfurt, Londra, Singapur, Washington DC |
| **[BroadBand Tower, Inc.](https://www.bbtower.co.jp/product-service/data-center/network/dcconnect-for-azure/)** | Equinix | Tokyo |
| **[C3ntro Telecom](https://www.c3ntro.com/data/express-route)** | Equinix, Megaport | Dallas |
| **[Chief](https://www.chief.com.tw/dispPageBox/ct.aspx?ddsPageID=ENCLOUDSERVICE&dbid=4852937342)** | Equinix | Hong Kong Çin ÖİB |
| **[Cinia](https://www.cinia.fi/en/services/connectivity-services/direct-public-cloud-connection.html)** | Equinix, Megaport | Frankfurt, Hamburg |
| **[CloudXpress](https://www2.telenet.be/fr/business/produits-services/internet/cloudxpress/)** | Equinix | Amsterdam | 
| **[Cogeco Peer 1](https://www.cogecopeer1.com/en/)**| Equinix | Montreal, Toronto |
| **[CoreAzure](http://coreazure.com/expressroute)**| Equinix | Londra |
| **[Cox Business](https://www.cox.com/business/networking/cloud-connectivity.html)** | Equinix | Dallas, Silikon Vadisi, Washington DC | 
| **[Data Foundry](https://www.datafoundry.com/services/cloud-connect)** | Megaport | Dallas |
| **[Epsilon Telecommunications Limited](https://www.epsilontel.com/data-connectivity/cloud-access/)** | Equinix | Londra, Singapur, Washington DC |
| **[Eurofiber](https://eurofiber.nl/microsoft-azure/)** | Equinix | Amsterdam |
| **[Exponential E](https://www.exponential-e.com/services/connectivity-services/cloud-connect-exchange)** | Equinix | Londra |
| **[Fastweb S.p.A](https://www.fastweb.it/grandi-aziende/connessione-voce-e-wifi/scheda-prodotto/rete-privata-virtuale/)** | Equinix | Amsterdam |
| **[Fibrenoire](https://www.fibrenoire.ca/en/cloudextn)** | Megaport | Quebec City |
| **[Gtt Communications Inc](https://www.gtt.net)** |Equinix | Washington DC |
| **[Uluslararası Gulf köprüsü](https://www.gbiinc.com/microsoft-azure-expressroute/)** | Equinix | Amsterdam |
| **[HSO](https://www.hso.co.uk/products/cloud-direct)** |Equinix | Londra, Slough |
| **[IVedha Inc](http://www.ivedha.com/cloud/manage-azure-cloud/express-route-4/)**| Equinix | Toronto |
| **LGA Telecom** |Equinix |Singapur|
| **[Lightower](http://www.lightower.com/network-solutions/cloud-connect/#microsoft-azure)** |Equinix | Chicago, New York, Washington DC |
| **[Macroview Telecom](http://www.macroview.com/en/scripts/catitem.php?catid=solution&sectionid=expressroute)** |Equinix |Hong Kong Çin ÖİB |
| **[Macquarie Telecom Group](https://macquariegovernment.com/secure-cloud/secure-cloud-exchange/)** | Megaport | Sidney |
| **[MainOne](https://www.mainone.net/services/connectivity/cloud-connect/)** |Equinix | Amsterdam |
| **[Masergy](https://www.masergy.com/solutions/hybrid-networking/cloud-marketplace/microsoft-azure)** | Equinix | Washington DC |
| **[MTN](https://www.mtnbusiness.com/en/enterprise/Pages/microsoft-express-route.aspx)** | Teraco | Cape Town, Johannesburg |
| **[NexGen Networks](https://www.nexgen-net.com/nexgen-networks-direct-connect-microsoft-azure-expressroute.html)** | Interxion | Londra |
| **[Nianet](https://nianet.dk/produkter/internet/microsoft-expressroute)** |Telecity | Amsterdam, Frankfurt |
| **[Post](https://www.teralinksolutions.com/cloud-connectivity/cloudbridge-to-azure-expressroute/)**|Equinix | Amsterdam |
| **[Proximus](https://www.proximus.be/en/id_b_cl_proximus_external_cloud_connect/companies-and-public-sector/discover/magazines/expert-blog/proximus-external-cloud-connect.html)**|Equinix | Amsterdam, Dublin, Londra, Paris |
| **[QSC AG](https://www.qsc.de/de/produkte-loesungen/cloud-services-und-it-outsourcing/pure-enterprise-cloud/multi-cloud-management/azure-expressroute/)** |Interxion | Frankfurt |  
| **Rogers** | Cologix, Equinix | Montreal, Toronto |
| **[Tamares Telecom](http://www.tamarestelecom.com/our-services/#Connectivity)** | Telecity | Londra | 
| **[TDC Erhverv](https://tdc.dk/Produkter/cloudaccessplus)** | Equinix | Amsterdam | 
| **[Telecom Italia Sparkle](https://www.tisparkle.com/our-platform/corporate-platform/sparkle-cloud-connect#catalogue)**| Equinix | Amsterdam |
| **[Telia](https://www.telia.se/foretag/losningar/produkter-tjanster/datanet)** | Equinix | Amsterdam |
| **[ThinkTel](https://www.thinktel.ca/services/agile-ix-data/expressroute/)** | Equinix | Toronto | 
| **[Transtelco](https://www.transtelco.net/tcloud/microsoft)** |Equinix | Dallas, Los Angeles |
| **[United Information Highway (UIH)](https://www.uih.co.th/en/internet-solution/cloud-direct/uih-cloud-direct-for-microsoft-azure-expressroute)**| Equinix | Singapur |
| **[Venha Pra Nuvem](https://venhapranuvem.com.br/)** | Equinix | Sao Paulo |
| **[Webair](https://www.webair.com/microsoft-express-route-partnership/)**| Megaport | New York |
| **[Windstream](https://www.windstreambusiness.com/solutions/cloud-services/cloud-and-managed-hosting-services)**| Equinix | Chicago, Silikon Vadisi, Washington DC |
| **Zain** |Equinix |Londra|
| **[Zertia](https://www.zertia.es)**| Level 3 | Madrid |
| **[Zirro](https://zirro.com/services/)**| Equinix | Montreal, Toronto |

## <a name="connectivity-through-datacenter-providers"></a>Veri Merkezi Sağlayıcıları Üzerinden Bağlantı

| **Sağlayıcı** | **Exchange** |
| --- | --- |
| **[Cyrus One](https://cyrusone.com/enterprise-data-center-services/connectivity-and-interconnection/cloud-connectivity-reaching-amazon-microsoft-google-and-more/microsoft-azure-expressroute/?doing_wp_cron=1498512235.6733090877532958984375)** | Megaport |
| **[Cyxtera](https://www.cyxtera.com/data-center-services/interconnection)** | Megaport |
| **[Digital Realty](https://www.digitalrealty.com/services/interconnection/service-exchange/)** | Megaport |
| **[EdgeConnex](https://www.edgeconnex.com/services/edge-data-centers-proximity-matters/)** | Megaport |
| **[RagingWire Data Centers](https://www.ragingwire.com/wholesale/wholesale-data-centers-worldwide-nexcenters)** | IX Reach |
| **[T5 Datacenters](https://t5datacenters.com/network-cloud-connect/)** | IX Reach |

## <a name="connectivity-through-national-research-and-education-networks-nren"></a>Ulusal Araştırma ve Eğitim Ağları (NREN) Üzerinden Bağlantı

| **Sağlayıcı**|
| --- |
| **AARNET**| 
| **GÉANT aracılığıyla DeIC**|
| **GÉANT aracılığıyla GARR**|
| **GÉANT**|
| **GÉANT aracılığıyla HEAnet**|
| **Internet2**|
| **JISC**|
| **GÉANT aracılığıyla RedIRIS**|
| **SINET**|
| **GÉANT aracılığıyla Surfnet**|

* Bağlantı sağlayıcınız bu listede yoksa, lütfen yukarıda listelenen ExpressRoute Exchange İş Ortaklarından herhangi birine bağlı olup olmadığınızı denetleyin.

## <a name="expressroute-system-integrators"></a>ExpressRoute sistem tümleştiricileri
İhtiyaçlarınıza uyan özel bağlantıyı etkinleştirme ağınızın ölçeğine bağlı olarak zorlu olabilir. ExpressRoute’a yönelik ekleme işleminde size yardımcı olmak üzere aşağıdaki tabloda listelenen herhangi bir sistem tümleştirici ile çalışabilirsiniz.

| **Sistem tümleştirici** | **Continent** |
| --- | --- |
| **[Altogee](https://altogee.be/diensten/express-route/)** | Avrupa |
| **[Avanade Inc.](https://www.avanade.com/)** | Asya, Avrupa, Kuzey Amerika, Güney Amerika |
| **[Bright Skies GmbH](https://bskies.io/expressroute)** | Avrupa
| **[Ensyst](https://www.ensyst.com.au)** | Asya
| **[Equinix Professional Services](https://www.equinix.com/services/consulting/)** | Kuzey Amerika |
| **[FlexManage](https://www.flexmanage.com/cloud)** | Kuzey Amerika |
| **[Lightstream](https://www.lightstream.tech/partners/microsoft-azure/)** | Kuzey Amerika |
| **[The IT Consultancy Group](https://itconsult.com.au/)** | Avustralya |
| **[MOQdigital](https://www.moqdigital.com.au/insights/technical/network-connectivity-options-for-azure)** | Avustralya |
| **[MSG Services](https://www.msg-services.de/it-services/managed-services/cloud-outsourcing/)** | Avrupa (Almanya) |
| **[Nelite](https://www.nelite.com/offres-services/)** | Avrupa |
| **[New Signature](https://newsignature.com/technologies/express-route/)** | Avrupa |
| **[OneAs1a](https://www.oneas1a.com/connectivity.html)** | Asya |
| **[Orange Networks](https://orange-networks.com/blog/88-azureexpressroute)** | Avrupa |
| **[Perficient](https://www.perficient.com/Partners/Microsoft/Cloud/Azure-ExpressRoute)** | Kuzey Amerika |
| **[Presidio](https://info.presidio.com/microsoft-azure-expressroute)** | Kuzey Amerika |
| **[sol-tec](https://www.sol-tec.com/services)** | Avrupa |
| **[Venha Pra Nuvem](https://venhapranuvem.com.br/)** | Güney Amerika |
| **[Vigilant.IT](https://vigilant.it/expressroute)** | Avustralya |

## <a name="next-steps"></a>Sonraki adımlar
* ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
* Tüm önkoşulların sağlandığından emin olun. Bkz. [ExpressRoute önkoşulları](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Konum eşleme"
