---
title: Şirket içi Azure bağlantıları için VPN cihazları hakkında | Microsoft Docs
description: Bu makalede VPN cihazları ve S2S VPN Gateway şirketler arası bağlantılar için IPsec parametreleri ele alınmaktadır. Yapılandırma yönergeleri ve örnekler için bağlantılar verilmektedir.
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: ''
tags: azure-resource-manager, azure-service-management
ms.assetid: ba449333-2716-4b7f-9889-ecc521e4d616
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/29/2018
ms.author: yushwang
ms.openlocfilehash: b3d9d45da0fb62445867d13c9dff7502af77e8a8
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="about-vpn-devices-and-ipsecike-parameters-for-site-to-site-vpn-gateway-connections"></a>Siteden Siteye VPN Gateway bağlantıları için VPN cihazları ve IPsec/IKE parametreleri hakkında

Bir VPN ağ geçidi kullanılarak Siteden Siteye (S2S) şirketler arası VPN bağlantısı yapılandırmak için bir VPN cihazı gereklidir. Siteden siteye bağlantılar karma çözüm oluşturmak amacıyla ya da şirket içi ağlarınız ile sanal ağlarınız arasında güvenli bağlantılar istediğinizde kullanılabilir. Bu makalede, doğrulanmış VPN cihazlarının listesi ve VPN ağ geçitleri için IPsec/IKE parametrelerinin listesi verilmektedir.

> [!IMPORTANT]
> Şirket içi VPN cihazlarınızla VPN ağ geçitleri arasında bağlantı sorunları yaşıyorsanız lütfen [Bilinen cihaz uyumluluk sorunları](#known) konusuna başvurun.
>

### <a name="items-to-note-when-viewing-the-tables"></a>Tabloları görüntülerken dikkate alınacaklar:

* Azure VPN ağ geçitleri terimlerinde bir değişiklik meydana gelmiştir. Yalnızca adlar değişmiştir. İşlevsellik değişmez.
  * Statik Yönlendirme = PolicyBased
  * Dinamik Yönlendirme = RouteBased
* Yüksek Performanslı VPN ağ geçidi ve RouteBased VPN ağ geçidi özellikleri, aksi belirtilmedikçe aynıdır. Örneğin, RouteBased VPN ağ geçitleri ile uyumlu doğrulanmış VPN cihazları, Yüksek Performanslı VPN ağ geçidi ile de uyumludur.

## <a name="devicetable"></a>Doğrulanmış VPN cihazları ve cihaz yapılandırma kılavuzları

> [!NOTE]
> Siteden Siteye bağlantı yapılandırırken, VPN cihazınız için genel kullanıma yönelik bir IPv4 IP adresi gereklidir.
>

Cihaz satıcılarıyla işbirliği yaparak bir grup standart VPN cihazını doğruladık. Aşağıdaki listede bulunan cihaz ailelerinde yer alan tüm cihazlar, VPN ağ geçitleriyle birlikte kullanılabilir. Yapılandırmak istediğiniz VPN Gateway çözümüne yönelik VPN türü kullanımını (PolicyBased veya RouteBased) anlamak için bkz. [VPN Gateway Ayarları Hakkında](vpn-gateway-about-vpn-gateway-settings.md#vpntype).

VPN cihazınızı yapılandırma konusunda yardım almak için, uygun cihaz ailesine karşılık gelen bağlantılara başvurun. Mümkün olan en iyi yapılandırma yönergeleri verilmiştir. VPN cihazı desteği için lütfen cihaz üreticinize başvurun.

|**Satıcı**          |**Cihaz ailesi**     |**En düşük işletim sistemi sürümü** |**PolicyBased yapılandırma yönergeleri** |**RouteBased yapılandırma yönergeleri** |
| ---                | ---                  | ---                   | ---            | ---           |
| A10 Networks, Inc. |Thunder CFW           |ACOS 4.1.1             |Uyumlu değil  |[Yapılandırma kılavuzu](https://www.a10networks.com/resources/deployment-guides/a10-thunder-cfw-ipsec-vpn-interoperability-azure-vpn-gateways)|
| Allied Telesis     |AR Serisi VPN Yönlendiricileri |2.9.2                  |Çok yakında     |Uyumlu değil  |
| Barracuda Networks, Inc. |Barracuda NextGen Firewall F-serisi |PolicyBased: 5.4.3<br>RouteBased: 6.2.0 |[Yapılandırma kılavuzu](https://techlib.barracuda.com/NGF/AzurePolicyBasedVPNGW) |[Yapılandırma kılavuzu](https://techlib.barracuda.com/NGF/AzureRouteBasedVPNGW) |
| Barracuda Networks, Inc. |Barracuda NextGen Firewall X-serisi |Barracuda Güvenlik Duvarı 6.5  |[Yapılandırma kılavuzu](https://techlib.barracuda.com/BFW/ConfigAzureVPNGateway) |Uyumlu değil |
| Brocade            |Vyatta 5400 vRouter   |Sanal Yönlendirici 6.6R3 GA|[Yapılandırma kılavuzu](http://www1.brocade.com/downloads/documents/html_product_manuals/vyatta/vyatta_5400_manual/wwhelp/wwhimpl/js/html/wwhelp.htm#href=VPN_Site-to-Site%20IPsec%20VPN/Preface.1.1.html) |Uyumlu değil |
| Denetim Noktası |Güvenlik Ağ Geçidi |R77.30 |[Yapılandırma kılavuzu](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |[Yapılandırma kılavuzu](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |
| Cisco              |ASA       |8.3<br>8.4+ (IKEv2*) |[Yapılandırma örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASA) |[Yapılandırma kılavuzu*](vpn-gateway-3rdparty-device-config-cisco-asa.md) |
| Cisco |ASR |PolicyBased: IOS 15.1<br>RouteBased: IOS 15.2 |[Yapılandırma örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR) |[Yapılandırma örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR) |
| Cisco |ISR |PolicyBased: IOS 15.0<br>RouteBased*: IOS 15.1 |[Yapılandırma örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR) |[Yapılandırma örnekleri\*\*](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR) |
| Cisco |Meraki |Yok |Uyumlu değil |Uyumlu değil |
| Citrix |NetScaler MPX, SDX, VPX |10.1 ve sonraki sürümleri |[Yapılandırma kılavuzu](https://docs.citrix.com/en-us/netscaler/11-1/system/cloudbridge-connector-introduction/cloudbridge-connector-azure.html) |Uyumlu değil |
| F5 |BIG-IP serisi |12.0 |[Yapılandırma kılavuzu](https://devcentral.f5.com/articles/connecting-to-windows-azure-with-the-big-ip) |[Yapılandırma kılavuzu](https://devcentral.f5.com/articles/big-ip-to-azure-dynamic-ipsec-tunneling) |
| Fortinet |FortiGate |FortiOS 5.6 |  |[Yapılandırma kılavuzu](http://cookbook.fortinet.com/ipsec-vpn-microsoft-azure-56/) |
| Internet Initiative Japan (IIJ) |SEIL Serisi |SEIL/X 4.60<br>SEIL/B1 4.60<br>SEIL/x86 3.20 |[Yapılandırma kılavuzu](http://www.iij.ad.jp/biz/seil/ConfigAzureSEILVPN.pdf) |Uyumlu değil |
| Juniper |SRX |PolicyBased: JunOS 10.2<br>Routebased: JunOS 11.4 |[Yapılandırma örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX) |[Yapılandırma örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX) |
| Juniper |J-Serisi |PolicyBased: JunOS 10.4r9<br>RouteBased: JunOS 11.4 |[Yapılandırma örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries) |[Yapılandırma örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries) |
| Juniper |ISG |ScreenOS 6.3 |[Yapılandırma örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG) |[Yapılandırma örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG) |
| Juniper |SSG |ScreenOS 6.2 |[Yapılandırma örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG) |[Yapılandırma örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG) |
| Microsoft |Yönlendirme ve Uzaktan Erişim Hizmeti |Windows Server 2012 |Uyumlu değil |[Yapılandırma örnekleri](http://go.microsoft.com/fwlink/p/?LinkId=717761) |
| Open Systems AG |Mission Control Security Ağ Geçidi |Yok |[Yapılandırma kılavuzu](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |Uyumlu değil |
| Palo Alto Networks |PAN-OS çalıştıran tüm cihazlar |PAN-OS<br>PolicyBased: 6.1.5 veya üzeri<br>RouteBased: 7.1.4 |[Yapılandırma kılavuzu](https://live.paloaltonetworks.com/t5/Configuration-Articles/How-to-Configure-VPN-Tunnel-Between-a-Palo-Alto-Networks/ta-p/59065) |[Yapılandırma kılavuzu](https://live.paloaltonetworks.com/t5/Integration-Articles/Configuring-IKEv2-VPN-for-Microsoft-Azure-Environment/ta-p/60340) |
| ShareTech | Yeni Nesil UTM (NU serisi) | 9.0.1.3 | Uyumlu değil | [Yapılandırma kılavuzu](http://www.sharetech.com.tw/images/file/Solution/NU_UTM/S2S_VPN_with_Azure_Route_Based_en.pdf) |
| SonicWall |TZ Series, NSA Series<br>SuperMassive Series<br>E-Class NSA Series |SonicOS 5.8.x<br>SonicOS 5.9.x<br>SonicOS 6.x |Uyumlu değil |[Yapılandırma kılavuzu](https://www.sonicwall.com/support/knowledge-base/170505320011694) |
| Sophos | XG Yeni Nesil Güvenlik Duvarı | XG v17 | | [Yapılandırma kılavuzu](https://community.sophos.com/kb/127546) |
| Ubiquiti | EdgeRouter | EdgeOS v1.10 |  | [Ikev2/IPSec üzerinden BGP](https://na01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fhelp.ubnt.com%2Fhc%2Fen-us%2Farticles%2F115012374708&data=02%7C01%7Cmaafiri%40microsoft.com%7C7580cdf59eb94528c0de08d4f9fd78bd%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636408314443168072&sdata=2EF5KFljZwtAGQDSm8%2FF2f6DqI2bkmA2qKG4u0rPgbQ%3D&reserved=0)<br><br>[VTI over IKEv2/IPsec](https://na01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fhelp.ubnt.com%2Fhc%2Fen-us%2Farticles%2F115012305347&data=02%7C01%7Cmaafiri%40microsoft.com%7C7580cdf59eb94528c0de08d4f9fd78bd%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636408314443168072&sdata=ycgiDJCOQYTPN7sAEBSigphzC6mBaADz%2FgdCOm7TsXA%3D&reserved=0)
| WatchGuard |Tümü |Fireware XTM<br> PolicyBased: v11.11.x<br>RouteBased: v11.12.x |[Yapılandırma kılavuzu](http://watchguardsupport.force.com/publicKB?type=KBArticle&SFDCID=kA2F00000000LI7KAM&lang=en_US) |[Yapılandırma kılavuzu](http://watchguardsupport.force.com/publicKB?type=KBArticle&SFDCID=kA22A000000XZogSAG&lang=en_US)|

> [!NOTE]
>
> (*) Cisco ASA 8.4 ve üzeri sürümleri IKEv2 desteği ekler, "UsePolicyBasedTrafficSelectors" seçeneğiyle özel IPsec/IKE ilkesini kullanarak Azure VPN ağ geçidine bağlanabilir. Bu [nasıl yapılır makalesine](vpn-gateway-connect-multiple-policybased-rm-ps.md) başvurun.
>
> (\*\*) ISR 7200 Serisi yönlendiriciler yalnızca PolicyBased VPN’leri destekler.

## <a name="configscripts"></a>Azure VPN cihazı yapılandırma komut dosyaları indirme

Belirli cihazlar, doğrudan Azure'dan yapılandırma komut indirebilirsiniz. Daha fazla bilgi ve yükleme yönergeleri için bkz: [karşıdan VPN cihazı yapılandırma komut](vpn-gateway-download-vpndevicescript.md).

### <a name="devices-with-available-configuration-scripts"></a>Kullanılabilen yapılandırma komut dosyaları ile cihazları

[!INCLUDE [scripts](../../includes/vpn-gateway-device-configuration-scripts.md)]

## <a name="additionaldevices"></a>Doğrulanmayan VPN cihazları

Cihazınızı Doğrulanan VPN cihazları tablosunda görmüyor olsanız bile cihazınız yine de Siteden Siteye bir bağlantı ile çalışabilir. Ek destek ve yapılandırma yönergeleri için cihaz üreticinize başvurun.

## <a name="editing"></a>Cihaz yapılandırma örneklerini düzenleme

Sağlanan VPN cihazı yapılandırma örneğini indirdikten sonra, ortamınıza ilişkin ayarları yansıtacak şekilde bazı değerleri değiştirmeniz gerekir.

### <a name="to-edit-a-sample"></a>Bir örneği düzenlemek için:

1. Not Defteri'ni kullanarak örneği açın.
2. Tüm <*text*> dizelerini arayın ve ortamınızla ilgili değerlerle değiştirin. < and > eklediğinizden emin olun. Bir ad belirtildiğinde, seçtiğiniz adın benzersiz olması gerekir. Bir komut çalışmazsa, lütfen cihazınızın üretici belgelerine başvurun.

| **Örnek metin** | **Şununla değiştirin:** |
| --- | --- |
| &lt;RP_OnPremisesNetwork&gt; |Bu nesne için seçtiğiniz ad. Örnek: myOnPremisesNetwork |
| &lt;RP_AzureNetwork&gt; |Bu nesne için seçtiğiniz ad. Örnek: myAzureNetwork |
| &lt;RP_AccessList&gt; |Bu nesne için seçtiğiniz ad. Örnek: myAzureAccessList |
| &lt;RP_IPSecTransformSet&gt; |Bu nesne için seçtiğiniz ad. Örnek: myIPSecTransformSet |
| &lt;RP_IPSecCryptoMap&gt; |Bu nesne için seçtiğiniz ad. Örnek: myIPSecCryptoMap |
| &lt;SP_AzureNetworkIpRange&gt; |Aralığı belirtin. Örnek: 192.168.0.0 |
| &lt;SP_AzureNetworkSubnetMask&gt; |Alt ağ maskesi belirtin. Örnek: 255.255.0.0 |
| &lt;SP_OnPremisesNetworkIpRange&gt; |Şirket içi aralığı belirtin. Örnek: 10.2.1.0 |
| &lt;SP_OnPremisesNetworkSubnetMask&gt; |Şirket içi alt ağ maskesini belirtin. Örnek: 255.255.255.0 |
| &lt;SP_AzureGatewayIpAddress&gt; |Bu bilgiler sanal ağınıza özeldir ve **Ağ geçidi IP adresi** olarak Yönetim Portalı’nda yer almaktadır. |
| &lt;SP_PresharedKey&gt; |Bu bilgiler sanal ağınıza özeldir ve Yönetme Anahtarı olarak Yönetim Portalı’nda yer almaktadır. |

## <a name="ipsec"></a>IPsec/IKE parametreleri

> [!IMPORTANT]
> 1. Aşağıdaki tablolar Azure VPN ağ geçitlerinin varsayılan yapılandırmada kullandığı algoritma ve parametre birleşimlerini içerir. Azure Kaynak Yönetimi dağıtım modeli kullanılarak oluşturulan rota tabanlı VPN ağ geçitleri için, her ayrı bağlantı üzerinde özel bir ilke belirleyebilirsiniz. Ayrıntılı yönergeler için lütfen [IPsec/IKE ilkesini yapılandırma](vpn-gateway-ipsecikepolicy-rm-powershell.md) bölümüne başvurun.
>
> 2. Ayrıca, TCP **MSS**’yi **1350**’de sıkıştırmanız gerekir. VPN cihazlarınız MSS sıkıştırmayı desteklemiyorsa, alternatif olarak tünel arabiriminde **MTU**’yu **1400** bayt olarak ayarlayabilirsiniz.
>

Aşağıdaki tablolarda:

* SA = Güvenlik İlişkisi
* IKE Aşama 1 "Ana Mod" olarak da adlandırılır
* IKE Aşama 2 "Hızlı Mod" olarak da adlandırılır

### <a name="ike-phase-1-main-mode-parameters"></a>IKE Aşama 1 (Ana Mod) parametreleri

| **Özellik**          |**PolicyBased**    | **RouteBased**    |
| ---                   | ---               | ---               |
| IKE Sürümü           |IKEv1              |IKEv2              |
| Diffie-Hellman Grubu  |Grup 2 (1024 bit) |Grup 2 (1024 bit) |
| Kimlik Doğrulama Yöntemi |Önceden Paylaşılan Anahtar     |Önceden Paylaşılan Anahtar     |
| Şifreleme ve Karma Algoritmaları |1. AES256, SHA256<br>2. AES256, SHA1<br>3. AES128, SHA1<br>4. 3DES, SHA1 |1. AES256, SHA1<br>2. AES256, SHA256<br>3. AES128, SHA1<br>4. AES128, SHA256<br>5. 3DES, SHA1<br>6. 3DES, SHA256 |
| SA Yaşam Süresi           |28.800 saniye     |28.800 saniye     |

### <a name="ike-phase-2-quick-mode-parameters"></a>IKE Aşama 2 (Hızlı Mod) parametreleri

| **Özellik**                  |**PolicyBased**| **RouteBased**                              |
| ---                           | ---           | ---                                         |
| IKE Sürümü                   |IKEv1          |IKEv2                                        |
| Şifreleme ve Karma Algoritmaları |1. AES256, SHA256<br>2. AES256, SHA1<br>3. AES128, SHA1<br>4. 3DES, SHA1 |[RouteBased QM SA Teklifleri](#RouteBasedOffers) |
| SA Yaşam Süresi (Zaman)            |3.600 saniye  |27.000 saniye                                |
| SA Yaşam Süresi (Bayt)           |102.400.000 KB | -                                           |
| Kusursuz İletme Gizliliği (PFS) |Hayır             |[RouteBased QM SA Teklifleri](#RouteBasedOffers) |
| Kullanılmayan Eş Algılama (DPD)     |Desteklenmiyor  |Desteklenen                                    |


### <a name ="RouteBasedOffers"></a>RouteBased VPN IPsec Güvenlik İlişkisi (IKE Hızlı Mod SA) Teklifleri

Aşağıdaki tabloda IPsec SA (IKE Hızlı Mod) Teklifleri listelenir. Teklifler, teklifin sunulduğu ya da kabul edildiği tercih sırasına göre listelenmiştir.

#### <a name="azure-gateway-as-initiator"></a>Başlatıcı olarak Azure Gateway

|-  |**Şifreleme**|**Kimlik doğrulaması**|**PFS Grubu**|
|---| ---          |---               |---          |
| 1 |GCM AES256    |GCM (AES256)      |None         |
| 2 |AES256        |SHA1              |None         |
| 3 |3DES          |SHA1              |None         |
| 4 |AES256        |SHA256            |None         |
| 5 |AES128        |SHA1              |None         |
| 6 |3DES          |SHA256            |Hiçbiri         |

#### <a name="azure-gateway-as-responder"></a>Yanıtlayıcı olarak Azure Gateway

|-  |**Şifreleme**|**Kimlik doğrulaması**|**PFS Grubu**|
|---| ---          | ---              |---          |
| 1 |GCM AES256    |GCM (AES256)      |Hiçbiri         |
| 2 |AES256        |SHA1              |None         |
| 3 |3DES          |SHA1              |None         |
| 4 |AES256        |SHA256            |None         |
| 5 |AES128        |SHA1              |Hiçbiri         |
| 6 |3DES          |SHA256            |None         |
| 7 |DES           |SHA1              |Hiçbiri         |
| 8 |AES256        |SHA1              |1            |
| 9 |AES256        |SHA1              |2            |
| 10|AES256        |SHA1              |14           |
| 11|AES128        |SHA1              |1            |
| 12|AES128        |SHA1              |2            |
| 13|AES128        |SHA1              |14           |
| 14|3DES          |SHA1              |1            |
| 15|3DES          |SHA1              |2            |
| 16|3DES          |SHA256            |2            |
| 17|AES256        |SHA256            |1            |
| 18|AES256        |SHA256            |2            |
| 19|AES256        |SHA256            |14           |
| 20|AES256        |SHA1              |24           |
| 21|AES256        |SHA256            |24           |
| 22|AES128        |SHA256            |None         |
| 23|AES128        |SHA256            |1            |
| 24|AES128        |SHA256            |2            |
| 25|AES128        |SHA256            |14           |
| 26|3DES          |SHA1              |14           |

* RouteBased ve Yüksek Performanslı VPN ağ geçitleri ile IPsec ESP NULL şifrelemesini belirtebilirsiniz. Null tabanlı şifreleme aktarımdaki verilere koruma sağlamaz ve yalnızca maksimum performans ve minimum gecikme gerekli olduğunda kullanılmalıdır. İstemciler, bunu Sanal Ağ-Sanal Ağ iletişim senaryolarında ya da çözümdeki başka bir yere şifreleme uygulandığında kullanmayı seçebilir.
* İnternet üzerinden şirket içi ve dışı bağlantı için, kritik iletişiminizin güvenliğini sağlamak üzere yukarıdaki tablolarda listelenen şifreleme ve karma algoritmalarla birlikte varsayılan Azure VPN ağ geçidi ayarlarını kullanın.

## <a name="known"></a>Bilinen cihaz uyumluluk sorunları

> [!IMPORTANT]
> Bunlar, üçüncü taraf VPN cihazları ile Azure VPN ağ geçitleri arasında bilinen uyumluluk sorunlarıdır. Azure ekibi, burada listelenen sorunların giderilmesi için satıcılarla etkin olarak çalışmaktadır. Sorunlar çözüldüğünde bu sayfada en güncel bilgilerle güncelleştirilecektir. Lütfen bu sayfayı düzenli aralıklarla kontrol edin.
>
>

### <a name="feb-16-2017"></a>16 Şubat 2017

Azure rota tabanlı VPN için **7.1.4’ten eski bir sürüme sahip Palo Alto Networks cihazları**: Palo Alto Networks tarafından sağlanan ve PAN-OS sürümü 7.1.4’ten eski olan VPN cihazları kullanıyor ve Azure rota tabanlı VPN ile bağlantı sorunları yaşıyorsanız aşağıdaki adımları gerçekleştirin:

1. Palo Alto Networks cihazınızın üretici yazılımı sürümünü denetleyin. PAN-OS sürümünüz 7.1.4’ten eskiyse 7.1.4 sürümüne yükseltin
2. Palo Alto Networks cihazında, Azure VPN Gateway’e bağlanırken kullanılan Phase 2 SA (veya Quick Mode SA) ömrünü 28.800 saniye (8 saat) olarak değiştirin.
3. Hala bir bağlantı sorunu yaşıyorsanız Azure portalından bir destek isteği oluşturun.
