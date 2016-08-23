<properties
   pageTitle="ExpressRoute konumları | Microsoft Azure"
   description="Bu makale, sunulan hizmetlerin konumları ve Azure bölgelerine nasıl bağlanılacağı hakkında ayrıntılı bir genel bakış sağlar."
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/28/2016"
   ms.author="cherylmc" />

# ExpressRoute ortakları ve eşleme konumları

Bu makaledeki tablolar ExpressRoute bağlantı sağlayıcıları, ExpressRoute coğrafi kapsamı, ExpressRoute üzerinden desteklenen Microsoft bulut hizmetleri ve ExpressRoute Sistem Tümleştiricileri (SIs) hakkında bilgi sağlar.

## <a name="partners"></a>ExpressRoute bağlantı sağlayıcıları

ExpressRoute tüm Azure bölgeleri ve konumları arasında desteklenir. Aşağıdaki harita Azure bölgeleri ve ExpressRoute konumlarının listesini sağlar. ExpressRoute konumları birkaç hizmet sağlayıcının sahip olduğu Microsoft eşlerine başvurur.

![Konum eşleme][0]

Coğrafi bölge içindeki en az bir ExpressRoute konumuna bağlanırsanız coğrafi bölge içindeki tüm bölgeler arasında Azure hizmetlerine erişebileceksiniz. Aşağıdaki tablo, coğrafi bölge içindeki Azure bölgeler ile ExpressRoute konumları arasında yapılan eşlemeyi sağlar.

|**Coğrafi bölge**|**Azure bölgeleri**|**ExpressRoute konumları**|
|---|---|---|
|**Kuzey Amerika**|Doğu ABD, Batı ABD, Doğu ABD 2, Orta ABD, Güney Orta ABD, Kuzey Orta ABD, Orta Kanada, Doğu Kanada|Atlanta, Chicago, Dallas, Las Vegas+, Los Angeles, New York, Seattle, Silikon Vadisi, Washington DC, Montreal+, Quebec City+, Toronto|
|**Güney Amerika**|Güney Brezilya|Sao Paulo|
|**Avrupa**|Kuzey Avrupa, Batı Avrupa|Amsterdam, Dublin, Londra, Newport(Galler)+, Paris|
|**Asya**|Doğu Asya, Güneydoğu Asya|Hong Kong, Singapur|
|**Japonya**|Batı Japonya, Doğu Japonya|Osaka, Tokyo|
|**Avustralya**|Güneydoğu Avustralya, Doğu Avustralya|Melbourne, Sidney|
|**Hindistan**|Batı Hindistan, Orta Hindistan, Güney Hindistan|Chennai, Mumbai|



Aşağıdaki tablo ulusal bulutlar için bölgeler ve coğrafi sınırlar hakkında bilgi sağlar.

|**Coğrafi bölge**|**Azure bölgeleri**|**ExpressRoute konumları**|
|---|---|---|---|
|**ABD bulutu**|ABD Iowa, ABD Virginia|Chicago, Dallas+, New York, Washington DC|
|**Çin**|Kuzey Çin, Doğu Çin|Pekin, Şangay|
|**Almanya**|Orta Almanya, Doğu Almanya|Berlin, Frankfurt|


Coğrafi bölgeler arasındaki bağlantı standart ExpressRoute SKU’da desteklenmiyor. Genel bağlantıyı desteklemek için ExpressRoute premium eklentisini etkinleştirmeniz gerekir. Ulusal bulut ortamlarına bağlantı desteklenmiyor. Bu tür bir ihtiyaç ortaya çıkarsa bağlantı sağlayıcınız ile çalışabilirsiniz.


## Bağlantı sağlayıcı konumları

> [AZURE.SELECTOR]
[Sağlayıcıya Göre Konumlar](expressroute-locations.md#connectivity-provider-locations)
[Konuma Göre Sağlayıcılar](expressroute-locations-providers.md#connectivity-provider-locations)

### Üretim Azure
| **Konum**  | **Hizmet Sağlayıcılar** |
|---------------|-----------------------|
| **Amsterdam** | Aryaka Networks, AT&T NetBond, British Telecom, Colt, Equinix, euNetworks, GÉANT+, InterCloud, Internet Solutions - Cloud Connect, Interxion, Level 3 Communications, Orange, Tata Communications, TeleCity Group, Telenor, Verizon |
| **Atlanta** | Equinix |
| **Chennai** | Tata Communications |
| **Chicago** | AT&T NetBond, Comcast, Equinix, Level 3 Communications, Zayo Group |
| **Dallas** | AT&T NetBond, Equinix, Level 3 Communications, Megaport |
| **Dublin** | Colt |
| **Hong Kong** | British Telecom, China Telecom Global, Equinix, Megaport, Orange, PCCW Global Limited, Tata Communications, Verizon |
| **Londra** | AT&T NetBond, British Telecom, Colt, Equinix, InterCloud, Internet Solutions - Cloud Connect, Interxion, Jisc+, Level 3 Communications, MTN, NTT Communications, Orange, Tata Communications, Telecity Group, Telenor, Verizon, Vodafone |
| **Las Vegas** | Level 3 Communications+, Megaport+
| **Los Angeles** | CoreSite, Equinix, Megaport, Zayo Group |
| **Melbourne** | Equinix, Megaport, NEXTDC, Telstra Corporation |
| **New York** | Equinix, Megaport, Zayo Group |
| **Montreal** | Cologix+ |
| **Mumbai** | Tata Communications |
| **Osaka** | Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, Softbank |
| **Paris** | Interxion |
| **Sao Paulo** | Equinix, Telefonica+ |
| **Seattle** | Equinix, Level 3 Communications, Megaport |
| **Silikon Vadisi** | Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink+, Comcast, Equinix, Level 3 Communications, Orange, Tata Communications, Verizon, Zayo Group |
| **Singapur** | Aryaka Networks, AT&T NetBond, British Telecom, Equinix, InterCloud, Megaport, Orange, SingTel, Tata Communications, Verizon |
| **Sidney** | AT&T NetBond, British Telecom, Equinix, Megaport, NEXTDC, Telstra Corporation, Verizon |
| **Tokyo** | Aryaka Networks, British Telecom, Colt, Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, Softbank, Verizon |
| **Toronto** | Cologix, Equinix, Zayo Group |
| **Washington DC** | Aryaka Networks, AT&T NetBond, British Telecom, Comcast, Equinix, InterCloud, Level 3 Communications, Megaport, Orange, Tata Communications, Verizon, Zayo Group |

 **+** çok yakında anlamına geliyor

### Ulusal bulut ortamları

#### ABD bulutu

| **Konum**  |**Hizmet Sağlayıcılar** |
|---------------|--------------------|
| **Chicago** | AT&T NetBond, Equinix, Level 3 Communications, Verizon |
| **Dallas** |  Equinix+, Verizon+ |
| **New York** | Equinix, Level 3 Communications+, Verizon |
| **Washington DC** | AT&T NetBond, Equinix, Level 3 Communications, Verizon |

#### Çin

| **Konum**  | **Hizmet Sağlayıcılar** |
|---------------|-----------------------|
| **Pekin** | China Telecom |
| **Shanghai** |  China Telecom |
Daha fazla bilgi için bkz. [Çin’de ExpressRoute](http://www.windowsazure.cn/home/features/expressroute/)

#### Almanya

| **Konum**  | **Hizmet Sağlayıcılar** |
|---------------|-----------------------|
| **Berlin** | Colt+, e-shelter+ |
| **Frankfurt** | Colt, Equinix+, Interxion |

## <a name="nonpartners"></a>Listelenmeyen hizmet sağlayıcıları üzerinden bağlantı

Bağlantı sağlayıcınız önceki bölümlerde listelenmemişse hala bağlantı oluşturabilirsiniz.

- Yukarıdaki tabloda yer alan değişimlerin herhangi birine bağlı olup olmadığını görmek için bağlantı sağlayıcınıza başvurun. Değişim sağlayıcıları tarafından sunulan hizmetler hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kontrol edebilirsiniz. Birkaç bağlantı sağlayıcı Ethernet değişimlerine zaten bağlı.

    - [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
    - [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
    - [InterXion](http://www.interxion.com/)
    - [NextDC](http://www.nextdc.com/)
    - [CoreSite](http://www.coresite.com/)
    - [Cologix](http://www.cologix.com/)
- Bağlantı sağlayıcınızı, ağınızı seçtiğiniz eşleme konumuna genişletmesini sağlayın.
    - Bağlantı sağlayıcınızın bağlantınızı yüksek oranda kullanılabilir şekilde genişlettiğinden emin olun, böylece hiç tek nokta arızası olmaz.
- Microsoft’a bağlanmak için bağlantı sağlayınız olarak değişime sahip bir ExpressRoute bağlantı hattı sipariş edin.
    - Bağlantı kurmak için [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-classic.md)’daki adımları izleyin.

|**Konum**|**Exchange**|**Bağlantı Sağlayıcılar**|
|-------------|------------|-------------------------|
| **New York** | Equinix | Lightower |
| **Seattle** | Equinix | Alaska Communications |
| **Silikon Vadisi** | Equinix | XO Communications |
| **Singapur** | Equinix | 1CLOUDSTAR |
| **Washington DC** | Equinix | Lightower |

## ExpressRoute sistem tümleştiricileri

İhtiyaçlarınıza uyan özel bağlantıyı etkinleştirme ağınızın ölçeğine bağlı olarak zorlu olabilir. ExpressRoute’a yönelik ekleme işleminde size yardımcı olmak üzere aşağıdaki tabloda listelenen herhangi bir sistem tümleştirici ile çalışabilirsiniz.

|**Kıta**|**Sistem tümleştiriciler**|
|-------------|---------------------|
| **Asya** | Avanade Inc., OneAs1a|
| **Avrupa** | Avanade Inc., Dotnet Solutions|
| **ABD** | Avanade Inc., Equinix Professional Services, Perficient, Project Leadership|

## Sonraki adımlar

- ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
- Tüm önkoşulların sağlandığından emin olun. Bkz. [ExpressRoute önkoşulları](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Konum eşleme"



<!--HONumber=Aug16_HO1-->


