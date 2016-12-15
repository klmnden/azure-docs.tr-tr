---
title: "Azure Sanal Ağlarına yönelik Konumdan Konuma VPN Gateway bağlantıları için VPN Cihazları hakkında | Microsoft Belgeleri"
description: "Bu makalede, S2S VPN Gateway bağlantılarına yönelik VPN cihazları ve IPsec parametreleri ele alınmaktadır ve yapılandırma yönergeleri ile örneklere bağlantılar vardır."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager, azure-service-management
ms.assetid: ba449333-2716-4b7f-9889-ecc521e4d616
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/01/2016
ms.author: yushwang;cherylmc
translationtype: Human Translation
ms.sourcegitcommit: d85451d84f605f8472da66574cfec3ba212884ea
ms.openlocfilehash: f7ecd6b2e590122c4794cd0a81754da9398ba320


---
# <a name="about-vpn-devices-for-site-to-site-vpn-gateway-connections"></a>Siteden siteye VPN Gateway bağlantıları için VPN cihazları hakkında
Siteden Siteye (S2S) VPN bağlantısı yapılandırmak için bir VPN cihazı gereklidir. Siteden siteye bağlantılar karma çözüm oluşturmak amacıyla ya da şirket içi ağınız ile sanal ağınız arasında güvenli bir bağlantı istediğinizde kullanılabilir. Bu makalede uyumlu VPN cihazları ve yapılandırma parametreleri açıklanır.

> [!NOTE]
> Siteden Siteye bağlantı yapılandırırken, VPN cihazınız için genel kullanıma yönelik bir IPv4 IP adresi gereklidir.                                                                                                                                                                               
>
>

Cihazınız [Doğrulanan VPN cihazları](#devicetable) tablosunda görünmüyorsa, bu makaledeki [Doğrulanmayan VPN cihazları](#additionaldevices) bölümüne bakın. Cihazınızın Azure ile çalışması hala mümkün olabilir. VPN cihaz desteği için lütfen cihaz üreticinize başvurun.

**Tabloları görüntülerken dikkate alınacaklar:**

* Statik ve dinamik yönlendirme terimlerinde bir değişiklik meydana gelmiştir. Büyük ihtimalle iki terimle de karşılaşacaksınız. İşlev değişikliği değil yalnızca ad değişikliği söz konusudur.
  * Statik Yönlendirme = PolicyBased
  * Dinamik Yönlendirme = RouteBased
* Yüksek Performanslı VPN ağ geçidi ve RouteBased VPN ağ geçidi özellikleri aksi belirtilmedikçe aynıdır. Örneğin, RouteBased VPN ağ geçitleri ile uyumlu doğrulanmış VPN cihazları, Azure Yüksek Performanslı VPN ağ geçidi ile de uyumludur.

## <a name="a-namedevicetableavalidated-vpn-devices"></a><a name="devicetable"></a>Doğrulanan VPN cihazları
Cihaz satıcılarıyla işbirliğiyle bir grup standart VPN cihazının doğruladık. Aşağıdaki listede bulunan cihaz ailelerinde yer alan tüm cihazlar, Azure VPN ağ geçitleriyle birlikte kullanılabilir. Yapılandırmak istediğiniz çözüm için oluşturmanız gereken ağ geçidinin türünü doğrulamak için bkz. [VPN Gateway Hakkında](vpn-gateway-about-vpngateways.md).

VPN cihazınızı yapılandırma konusunda yardım almak için, uygun cihaz ailesine karşılık gelen bağlantılara başvurun. VPN cihaz desteği için lütfen cihaz üreticinize başvurun.

| **Satıcı** | **Cihaz ailesi** | **En düşük işletim sistemi sürümü** | **PolicyBased** | **RouteBased** |
| --- | --- | --- | --- | --- |
| Allied Telesis |AR Serisi VPN Yönlendiricileri |2.9.2 |Çok yakında |Uyumlu değil |
| Barracuda Networks, Inc. |Barracuda NextGen Firewall F-serisi |PolicyBased: 5.4.3<br>RouteBased: 6.2.0 |[Yapılandırma yönergeleri](https://techlib.barracuda.com/NGF/AzurePolicyBasedVPNGW) |[Yapılandırma yönergeleri](https://techlib.barracuda.com/NGF/AzureRouteBasedVPNGW) |
| Barracuda Networks, Inc. |Barracuda NextGen Firewall X-serisi |Barracuda Güvenlik Duvarı 6.5  |[Barracuda Güvenlik Duvarı](https://techlib.barracuda.com/BFW/ConfigAzureVPNGateway) |Uyumlu değil |
| Brocade |Vyatta 5400 vRouter |Sanal Yönlendirici 6.6R3 GA |[Yapılandırma yönergeleri](http://www1.brocade.com/downloads/documents/html_product_manuals/vyatta/vyatta_5400_manual/wwhelp/wwhimpl/js/html/wwhelp.htm#href=VPN_Site-to-Site%20IPsec%20VPN/Preface.1.1.html) |Uyumlu değil |
| Denetim Noktası |Güvenlik Ağ Geçidi |R75.40<br>R75.40VS |[Yapılandırma yönergeleri](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |[Yapılandırma yönergeleri](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |
| Cisco |ASA |8.3 |[Cisco örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASA) |Uyumlu değil |
| Cisco |ASR |PolicyBased: IOS 15.1<br>RouteBased: IOS 15.2 |[Cisco örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR) |[Cisco örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR) |
| Cisco |ISR |PolicyBased: IOS 15.0<br>RouteBased*: IOS 15.1 |[Cisco örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR) |[Cisco örnekleri*](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR) |
| Citrix |NetScaler MPX, SDX, VPX |10.1 ve sonraki sürümleri |[Tümleştirme yönergeleri](https://docs.citrix.com/en-us/netscaler/11-1/system/cloudbridge-connector-introduction/cloudbridge-connector-azure.html) |Uyumlu değil |
| Dell SonicWALL |TZ Series, NSA Series<br>SuperMassive Series<br>E-Class NSA Series |SonicOS 5.8.x<br>[SonicOS 5.9.x](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=850)<br>[SonicOS 6.x](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=646) |[SonicOS 6.2 için yapılandırma kılavuzu](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646)<br>[SonicOS 5.9 için yapılandırma kılavuzu](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850) |[SonicOS 6.2 için yapılandırma kılavuzu](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646)<br>[SonicOS 5.9 için yapılandırma kılavuzu](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850) |
| F5 |BIG-IP serisi |12.0 |[Yapılandırma yönergeleri](https://devcentral.f5.com/articles/connecting-to-windows-azure-with-the-big-ip) |[Yapılandırma yönergeleri](https://devcentral.f5.com/articles/big-ip-to-azure-dynamic-ipsec-tunneling) |
| Fortinet |FortiGate |FortiOS 5.4.x |[Yapılandırma yönergeleri](http://cookbook.fortinet.com/ipsec-vpn-microsoft-azure-54) |[Yapılandırma yönergeleri](http://cookbook.fortinet.com/ipsec-vpn-microsoft-azure-54) |
| Internet Initiative Japan (IIJ) |SEIL Serisi |SEIL/X 4.60<br>SEIL/B1 4.60<br>SEIL/x86 3.20 |[Yapılandırma yönergeleri](http://www.iij.ad.jp/biz/seil/ConfigAzureSEILVPN.pdf) |Uyumlu değil |
| Juniper |SRX |PolicyBased: JunOS 10.2<br>Routebased: JunOS 11.4 |[Juniper örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX) |[Juniper örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX) |
| Juniper |J-Serisi |PolicyBased: JunOS 10.4r9<br>RouteBased: JunOS 11.4 |[Juniper örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries) |[Juniper örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries) |
| Juniper |ISG |ScreenOS 6.3 |[Juniper örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG) |[Juniper örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG) |
| Juniper |SSG |ScreenOS 6.2 |[Juniper örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG) |[Juniper örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG) |
| Microsoft |Yönlendirme ve Uzaktan Erişim Hizmeti |Windows Server 2012 |Uyumlu değil |[Microsoft örnekleri](http://go.microsoft.com/fwlink/p/?LinkId=717761) |
| Open Systems AG |Mission Control Security Ağ Geçidi |Yok |[Yükleme kılavuzu](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |[Yükleme kılavuzu](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |
| Openswan |Openswan |2.6.32 |(Çok yakında) |Uyumlu değil |
| Palo Alto Networks |PAN-OS çalıştıran tüm cihazlar |PAN-OS<br>PolicyBased: 6.1.5 veya üzeri<br>RouteBased: 7.0.5 veya üzeri |[Yapılandırma yönergeleri](https://live.paloaltonetworks.com/t5/Configuration-Articles/How-to-Configure-VPN-Tunnel-Between-a-Palo-Alto-Networks/ta-p/59065) |[Yapılandırma yönergeleri](https://live.paloaltonetworks.com/t5/Integration-Articles/Configuring-IKEv2-VPN-for-Microsoft-Azure-Environment/ta-p/60340) |
| Watchguard |Tümü |Fireware XTM<br> PolicyBased: v11.11.x<br>RouteBased: v11.12.x |[Yapılandırma yönergeleri](http://watchguardsupport.force.com/publicKB?type=KBArticle&SFDCID=kA2F00000000LI7KAM&lang=en_US) |[Yapılandırma yönergeleri](http://watchguardsupport.force.com/publicKB?type=KBArticle&SFDCID=kA22A000000XZogSAG&lang=en_US)|

(*) ISR 7200 Serisi yönlendiriciler yalnızca PolicyBased VPN'leri destekler.

## <a name="a-nameadditionaldevicesanon-validated-vpn-devices"></a><a name="additionaldevices"></a>Doğrulanmayan VPN cihazları
Cihazınızı, Doğrulanan VPN cihazları tablosunda görmüyor olsanız bile cihaz yine de Siteden Siteye bir bağlantı ile çalışabilir. VPN cihazınızın [VPN Gateway Hakkında](vpn-gateway-about-vpngateways.md) makalesinin Gateway Gereksinimleri bölümünde belirtilen minimum gereksinimleri karşıladığını doğrulayın. Minimum gereksinimleri karşılayan cihazların da VPN gateway’ler ile sorunsuz çalışması gerekir. Ek destek ve yapılandırma yönergeleri için cihaz üreticinize başvurun.

## <a name="editing-device-configuration-samples"></a>Cihaz yapılandırma örneklerini düzenleme
Sağlanan VPN cihazı yapılandırma örneğini indirdikten sonra, ortamınıza ilişkin ayarları yansıtacak şekilde bazı değerleri değiştirmeniz gerekir.

**Bir örneği düzenlemek için:**

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

## <a name="ipsec-parameters"></a>IPsec Parameters
> [!NOTE]
> Aşağıdaki tabloda listelenen değerler Azure VPN Gateway tarafından desteklense de şu anda Azure VPN Gateway'den belirli bir birleşim belirtemez ya da seçemezsiniz. Tüm kısıtlamaları şirket içi VPN cihazında belirtmeniz gerekir. Ayrıca, MSS’i 1350’de sıkıştırmanız gerekir.
>
>

### <a name="ike-phase-1-setup"></a>IKE Aşama 1 kurulumu
| **Özellik** | **PolicyBased** | **RouteBased ve Standart ya da Yüksek Performanslı VPN ağ geçidi** |
| --- | --- | --- |
| IKE Sürümü |IKEv1 |IKEv2 |
| Diffie-Hellman Grubu |Grup 2 (1024 bit) |Grup 2 (1024 bit) |
| Kimlik Doğrulama Yöntemi |Önceden Paylaşılan Anahtar |Önceden Paylaşılan Anahtar |
| Şifreleme Algoritmaları |AES256 AES128 3DES |AES256 3DES |
| Karma Algoritma |SHA1(SHA128) |SHA1(SHA128), SHA2(SHA256) |
| Aşama 1 Güvenlik İlişkisi (SA) Yaşam Süresi (Zaman) |28.800 saniye |10.800 saniye |

### <a name="ike-phase-2-setup"></a>IKE Aşama 2 kurulumu
| **Özellik** | **PolicyBased** | **RouteBased ve Standart ya da Yüksek Performanslı VPN ağ geçidi** |
| --- | --- | --- |
| IKE Sürümü |IKEv1 |IKEv2 |
| Karma Algoritma |SHA1(SHA128) |SHA1(SHA128) |
| Aşama 2 Güvenlik İlişkisi (SA) Yaşam süresi (Saat) |3.600 saniye |3.600 saniye |
| Aşama 2 Güvenlik İlişkisi (SA) Yaşam süresi (Verim) |102.400.000 KB |- |
| IPsec SA Şifreleme ve kimlik Doğrulama Teklifleri (öncelik sırasına göre) |1. ESP-AES256 2. ESP-AES128 3. ESP-3DES 4. Yok |Bkz. *RouteBased Ağ Geçidi IPsec Güvenlik İlişkisi (SA) Teklifleri* (aşağıda) |
| Kusursuz İletme Gizliliği (PFS) |Hayır |Hayır (*) |
| Kullanılmayan Eş Algılama |Desteklenmiyor |Destekleniyor |

(*) IKE Yanıtlayıcı olarak Azure Gateway, PFS DH Grubu 1, 2, 5, 14, 24’ü kabul edebilir.

### <a name="routebased-gateway-ipsec-security-association-sa-offers"></a>RouteBased Ağ Geçidi IPsec Güvenlik İlişkisi (SA) Teklifleri
Aşağıdaki tabloda IPsec SA Şifreleme ve Kimlik Doğrulama Teklifleri listelenmiştir. Teklifler, teklifin sunulduğu ya da kabul edildiği tercih sırasına göre listelenmiştir.

| **IPsec SA Şifreleme ve Kimlik Doğrulama Teklifleri** | **Başlatıcı olarak Azure Ağ Geçidi** | **Yanıtlayıcı olarak Azure Ağ Geçidi** |
| --- | --- | --- |
| 1 |ESP AES_256 SHA |ESP AES_128 SHA |
| 2 |ESP AES_128 SHA |ESP 3_DES MD5 |
| 3 |ESP 3_DES MD5 |ESP 3_DES SHA |
| 4 |ESP 3_DES SHA |AH SHA1 ile ESP AES_128 ve HMAC Null |
| 5 |AH SHA1 ile ESP AES_256 ve HMAC Null |AH SHA1 ile ESP 3_DES ve HMAC Null |
| 6 |AH SHA1 ile ESP AES_128 ve HMAC Null |AH MD5 ile ESP 3_DES ve HMAC Null, önerilen yaşam süresi yok |
| 7 |AH SHA1 ile ESP 3_DES ve HMAC Null |AH SHA1 ile ESP 3_DES SHA1, yaşam süresi yok |
| 8 |AH MD5 ile ESP 3_DES ve HMAC Null, önerilen yaşam süresi yok |AH MD5 ile ESP 3_DES MD5, yaşam süresi yok |
| 9 |AH SHA1 ile ESP 3_DES SHA1, yaşam süresi yok |ESP DES MD5 |
| 10 |AH MD5 ile ESP 3_DES MD5, yaşam süresi yok |ESP DES SHA1, yaşam süresi yok |
| 11 |ESP DES MD5 |AH SHA1 ile ESP DES HMAC Null, önerilen yaşam süresi yok |
| 12 |ESP DES SHA1, yaşam süresi yok |AH MD5 ile ESP DES HMAC Null, önerilen yaşam süresi yok |
| 13 |AH SHA1 ile ESP DES HMAC Null, önerilen yaşam süresi yok |AH SHA1 ile ESP DES SHA1, yaşam süresi yok |
| 14 |AH MD5 ile ESP DES HMAC Null, önerilen yaşam süresi yok |AH MD5 ile ESP DES MD5, yaşam süresi yok |
| 15 |AH SHA1 ile ESP DES SHA1, yaşam süresi yok |ESP SHA, yaşam süresi yok |
| 16 |AH MD5 ile ESP DES MD5, yaşam süresi yok |ESP MD5, yaşam süresi yok |
| 17 |- |AH SHA, yaşam süresi yok |
| 18 |- |AH MD5, yaşam süresi yok |

* RouteBased ve Yüksek Performanslı VPN ağ geçitleri ile IPsec ESP NULL şifrelemesini belirtebilirsiniz. Null tabanlı şifreleme aktarımdaki verilere koruma sağlamaz ve yalnızca maksimum performans ve minimum gecikme gerekli olduğunda kullanılmalıdır.  İstemciler, bunu Sanal Ağ-Sanal Ağ iletişim senaryolarında ya da çözümdeki başka bir yere şifreleme uygulandığında kullanmayı seçebilir.
* İnternet üzerinden şirket içi ve dışı bağlantı için, kritik iletişiminizin güvenliğini sağlamak üzere yukarıdaki tablolarda listelenen şifreleme ve karma algoritmalarla birlikte varsayılan Azure VPN ağ geçidi ayarlarını kullanın.



<!--HONumber=Dec16_HO1-->


