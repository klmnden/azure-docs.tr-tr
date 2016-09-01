<properties 
   pageTitle="Azure Virtual Network’lere yönelik Siteden Siteye VPN Gateway bağlantıları için VPN Cihazları hakkında | Microsoft Azure"
   description="S2S VPN Gateway bağlantıları için VPN cihazları ve IPsec parametreleri hakkında bilgi edinin. Siteden siteye bağlantılar karma yapılandırmalar için kullanılabilir. Bu makale VPN gateway cihazları için yapılandırma yönergeleri ve örneklere bağlantılar içermektedir."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
  tags="azure-resource-manager, azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/10/2016"
   ms.author="cherylmc" />

# Siteden siteye VPN Gateway bağlantıları için VPN cihazları hakkında

Siteden Siteye (S2S) VPN bağlantısı yapılandırmak için bir VPN cihazı gereklidir. Siteden siteye bağlantılar karma çözüm oluşturmak amacıyla ya da şirket içi ağınız ile sanal ağınız arasında güvenli bir bağlantı istediğinizde kullanılabilir. Bu makalede uyumlu VPN cihazları ve yapılandırma parametreleri açıklanır. Lütfen, Siteden Siteye bağlantı yapılandırırken, VPN cihazınız için genel kullanıma yönelik bir IPv4 IP adresi gerektiğini unutmayın.                                                                                                                                                                                

Cihazınız [Doğrulanan VPN cihazları](#devicetable) tablosunda görünmüyorsa, bu makaledeki [Doğrulanmayan VPN cihazları](#additionaldevices) bölümüne bakın. Cihazınızın Azure ile çalışması hala mümkün olabilir. VPN cihaz desteği için lütfen cihaz üreticinize başvurun.

**Tabloları görüntülerken dikkate alınacaklar:**

- Statik ve dinamik yönlendirme terimlerinde bir değişiklik meydana gelmiştir. Büyük ihtimalle iki terimle de karşılaşacaksınız. İşlev değişikliği değil yalnızca ad değişikliği söz konusudur.
    - Statik Yönlendirme = İlke tabanlı
    - Dinamik Yönlendirme = Rota tabanlı 
- Yüksek Performanslı VPN gateway ve rota tabanlı VPN gateway özellikleri aksi belirtilmedikçe, aynıdır. Örneğin, yol tabanlı VPN ağ geçitleriyle uyumlu doğrulanmış VPN cihazları, Azure Yüksek Performanslı VPN ağ geçidiyle de uyumludur. 


## <a name="devicetable"></a>Doğrulanan VPN cihazları 

Cihaz satıcılarıyla işbirliğiyle bir grup standart VPN cihazının doğruladık. Aşağıdaki listede bulunan cihaz ailelerinde yer alan tüm cihazlar, Azure VPN ağ geçitleriyle birlikte kullanılabilir. Yapılandırmak istediğiniz çözüm için oluşturmanız gereken ağ geçidinin türünü doğrulamak için bkz. [VPN Gateway Hakkında](vpn-gateway-about-vpngateways.md). 

VPN cihazınızı yapılandırmaya yardım için lütfen uygun cihaz ailesine karşılık gelen bağlantılara bakın. 



| **Satıcı**                      | **Cihaz ailesi**                                        | **En düşük işletim sistemi sürümü**                             | **İlke tabanlı**                                                                                                                                                                                                             | **Rota tabanlı**                                                                                                                                                                    |
|---------------------------------|----------------------------------------------------------|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Allied Telesis                  | AR Serisi VPN Yönlendiricileri                                    | 2.9.2                                              | Çok yakında                                                                                                                                                                                                                                          | Uyumlu değil                                                                                                                                                                                               |
| Barracuda Networks, Inc.        | Barracuda NextGen Firewall F-serisi             | İlke tabanlı: 5.4.3 Rota tabanlı: 6.2.0  | [Yapılandırma yönergeleri](https://techlib.barracuda.com/NGF/AzurePolicyBasedVPNGW) | [Yapılandırma yönergeleri](https://techlib.barracuda.com/NGF/AzureRouteBasedVPNGW)                                                                                                                                                                                              |
| Barracuda Networks, Inc.        |  Barracuda NextGen Firewall X-serisi                 | Barracuda Güvenlik Duvarı 6.5  | [Barracuda Güvenlik Duvarı](https://techlib.barracuda.com/BFW/ConfigAzureVPNGateway) | Uyumlu değil                                                                                                                                                                                               |
| Brocade                         | Vyatta 5400 vRouter                                      | Sanal Yönlendirici 6.6R3 GA                            | [Yapılandırma yönergeleri](http://www1.brocade.com/downloads/documents/html_product_manuals/vyatta/vyatta_5400_manual/wwhelp/wwhimpl/js/html/wwhelp.htm#href=VPN_Site-to-Site%20IPsec%20VPN/Preface.1.1.html)                                       | Uyumlu değil                                                                                                                                                                                               |
| Denetim Noktası                     | Güvenlik Ağ Geçidi                                         | R75.40, R75.40VS                                     | [Yapılandırma yönergeleri](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275)                                         | [Yapılandırma yönergeleri](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |
| Cisco                           | ASA                                                      | 8.3                                                | [Cisco örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASA)                                                                                                                                                                        | Uyumlu değil                                                                                                                                                                                               |
| Cisco                           | ASR                                                      | IOS 15.1 (ilke tabanlı), IOS 15.2 (yol tabanlı)                | [Cisco örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR)                                                                                                                                                                        | [Cisco örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR)                                                                                                                                 |
| Cisco                           | ISR                                                      | IOS 15.0 (ilke tabanlı), IOS 15.1 (yol tabanlı*)               | [Cisco örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR)                                                                                                                                                                        | [Cisco örnekleri*](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR)                                                                                                                                |
| Citrix                          | NetScaler MPX, SDX, VPX      |10.1 ve sonraki sürümleri                                           | [Tümleştirme yönergeleri](https://docs.citrix.com/en-us/netscaler/11-1/system/cloudbridge-connector-introduction/cloudbridge-connector-azure.html)                                                                                                                                                                            | Uyumlu değil                                                                                                                                                                                               |
| Dell SonicWALL                  | TZ Serisi, NSA Serisi, SuperMassive Serisi, E-Class NSA Serisi | SonicOS 5.8.x, [SonicOS 5.9.x](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=850), [SonicOS 6.x](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=646 )          | [Yönergeler - SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Yönergeler - SonicOS 5.9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850)                                                                                                                                   | [Yönergeler - SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Yönergeler - SonicOS 5.9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850)                                                                                                                                                                                      |
| F5                              | BIG-IP serisi                                            | Yok                                                | [Yapılandırma yönergeleri](https://devcentral.f5.com/articles/connecting-to-windows-azure-with-the-big-ip)                                                                                                                                                                          | Uyumlu değil                                                                                                                                                                                               |
| Fortinet                        | FortiGate                                                | FortiOS 5.2.7                                      | [Yapılandırma yönergeleri](http://docs.fortinet.com/d/fortigate-configuring-ipsec-vpn-between-a-fortigate-and-microsoft-azure)                                                                                                                                                                          | [Yapılandırma yönergeleri](http://docs.fortinet.com/d/fortigate-configuring-ipsec-vpn-between-a-fortigate-and-microsoft-azure)                                                                                                                                  |
| Internet Initiative Japan (IIJ) | SEIL Serisi                                              | SEIL/X 4.60, SEIL/B1 4.60, SEIL/x86 3.20            | [Yapılandırma yönergeleri](http://www.iij.ad.jp/biz/seil/ConfigAzureSEILVPN.pdf)                                                                                                                                                                   | Uyumlu değil                                                                                                                                                                                               |
| Juniper                         | SRX                                                      | JunOS 10.2 (ilke tabanlı), JunOS 11.4 (yol tabanlı)            | [Juniper örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX)                                                                                                                                                                      | [Juniper örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX)                                                                                                                              |
| Juniper                         | J-Serisi                                                 | JunOS 10.4r9 (ilke tabanlı), JunOS 11.4 (yol tabanlı)          | [Juniper örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries)                                                                                                                                                                 | [Juniper örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries)                                                                                                                         |
| Juniper                         | ISG                                                      | ScreenOS 6.3 (ilke tabanlı ve rota tabanlı)                  | [Juniper örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG)                                                                                                                                                                      | [Juniper örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG)                                                                                                                              |
| Juniper                         | SSG                                                      | ScreenOS 6.2 (ilke tabanlı ve rota tabanlı)                  | [Juniper örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG)                                                                                                                                                                      | [Juniper örnekleri](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG)                                                                                                                              |
| Microsoft                       | Yönlendirme ve Uzaktan Erişim Hizmeti                        | Windows Server 2012                                | Uyumlu değil                                                                                                                                                                                                                                       | [Microsoft örnekleri](http://go.microsoft.com/fwlink/p/?LinkId=717761)                                                                                           |
| Open Systems AG | Mission Control Security Ağ Geçidi | Yok | [Yükleme Kılavuzu](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) | [Yükleme Kılavuzu](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |
| Openswan                        | Openswan                                                 | 2.6.32                                             | (Çok yakında)                                                                                                                                                                                                                                        | Uyumlu değil                                                                                                                                                                                               |
| Palo Alto Networks              | PAN-OS çalıştıran tüm cihazlar     | PAN-OS 6.1.5 veya üst sürümü (ilke tabanlı), PAN-OS 7.0.5 veya üst sürümü (rota tabanlı)       | [Yapılandırma Yönergeleri]( https://live.paloaltonetworks.com/t5/Configuration-Articles/How-to-Configure-VPN-Tunnel-Between-a-Palo-Alto-Networks/ta-p/59065)                                                                                                                                                                                          | [Yapılandırma Yönergeleri](https://live.paloaltonetworks.com/t5/Integration-Articles/Configuring-IKEv2-VPN-for-Microsoft-Azure-Environment/ta-p/60340)                                                                                                                                                                                    |
| Watchguard                      | Tümü                                                      | Fireware XTM v11.x                                 | [Yapılandırma yönergeleri](http://customers.watchguard.com/articles/Article/Configure-a-VPN-connection-to-a-Windows-Azure-virtual-network/)                                                                                                                                                                          | Uyumlu değil                                                                                                                                                                                               |

(*) ISR 7200 Serisi yönlendiriciler yalnızca ilke tabanlı VPN'leri destekler.

## <a name="additionaldevices"></a>Doğrulanmayan VPN cihazları

Cihazınızı Doğrulanan VPN cihazları tablosunda (yukarıda) görmüyorsanız, yine de Siteden Siteye bağlantı için çalışabilir. VPN cihazınızın [VPN Gateway’ler hakkında](vpn-gateway-about-vpngateways.md#gateway-requirements) makalesinin Gateway Gereksinimleri bölümünde belirtilen minimum gereksinimleri karşıladığını doğrulayın. Minimum gereksinimleri karşılayan cihazların da VPN gateway’ler ile sorunsuz çalışması gerekir. Lütfen ek destek ve yapılandırma yönergeleri için cihaz üreticinize başvurun.


## Cihaz yapılandırma örneklerini düzenleme

Sağlanan VPN cihazı yapılandırma örneğini indirdikten sonra, ortamınıza ilişkin ayarları yansıtacak şekilde bazı değerleri değiştirmeniz gerekir. 

**Bir örneği düzenlemek için:**

1. Not Defteri'ni kullanarak örneği açın. 
1. Tüm <*text*> dizelerini arayın ve ortamınızla ilgili değerlerle değiştirin. < and > eklediğinizden emin olun. Bir ad belirtildiğinde, seçtiğiniz adın benzersiz olması gerekir. Bir komut çalışmazsa, lütfen cihaz üreticisi belgelerinize başvurun.

| **Örnek metin**                | **Şununla değiştirin**                                                                                                        |
|----------------------------------|----------------------------------------------------------------------------------------------------------------------|
| &lt;RP_OnPremisesNetwork&gt;           | Bu nesne için seçtiğiniz ad. Örnek: myOnPremisesNetwork                                                       |
| &lt;RP_AzureNetwork&gt;                | Bu nesne için seçtiğiniz ad. Örnek: myAzureNetwork                                                            |
| &lt;RP_AccessList&gt;                  | Bu nesne için seçtiğiniz ad. Örnek: myAzureAccessList                                                         |
| &lt;RP_IPSecTransformSet&gt;           | Bu nesne için seçtiğiniz ad. Örnek: myIPSecTransformSet                                                       |
| &lt;RP_IPSecCryptoMap&gt;              | Bu nesne için seçtiğiniz ad. Örnek: myIPSecCryptoMap                                                          |
| &lt;SP_AzureNetworkIpRange&gt;         | Aralığı belirtin. Örnek: 192.168.0.0                                                                                  |
| &lt;SP_AzureNetworkSubnetMask&gt;      | Alt ağ maskesi belirtin. Örnek: 255.255.0.0                                                                            |
| &lt;SP_OnPremisesNetworkIpRange&gt;    | Şirket içi aralığı belirtin. Örnek: 10.2.1.0                                                                         |
| &lt;SP_OnPremisesNetworkSubnetMask&gt; | Şirket içi alt ağ maskesini belirtin. Örnek: 255.255.255.0                                                              |
| &lt;SP_AzureGatewayIpAddress&gt;       | Bu bilgiler sanal ağınıza özeldir ve **Ağ geçidi IP adresi** olarak Yönetim Portalı’nda yer almaktadır. |
| &lt;SP_PresharedKey&gt;                | Bu bilgiler sanal ağınıza özeldir ve Yönetme Anahtarı olarak Yönetim Portalı’nda yer almaktadır.          |



## IPsec Parameters

>[AZURE.NOTE] Aşağıdaki tabloda listelenen değerler Azure VPN Gateway tarafından desteklense de şu anda Azure VPN Gateway'den belirli bir birleşim belirtemez ya da seçemezsiniz. Tüm kısıtlamaları şirket içi VPN cihazında belirtmeniz gerekir. Ayrıca, MSS’i 1350’de sıkıştırmanız gerekir.

### IKE Aşama 1 kurulumu

| **Özellik**                                       | **İlke tabanlı** | **Rota tabanlı ve Standart ya da Yüksek Performanslı VPN Gateway** |
|----------------------------------------------------|--------------------------------|------------------------------------------------------------------|
| IKE Sürümü                                        | IKEv1                          | IKEv2                                                            |
| Diffie-Hellman Grubu                               | Grup 2 (1024 bit)             | Grup 2 (1024 bit)                                               |
| Kimlik Doğrulama Yöntemi                              | Önceden Paylaşılan Anahtar                 | Önceden Paylaşılan Anahtar                                                   |
| Şifreleme Algoritmaları                              | AES256 AES128 3DES             | AES256 3DES                                                      |
| Karma Algoritma                                  | SHA1(SHA128)                   | SHA1(SHA128), SHA2(SHA256)                                                     |
| Aşama 1 Güvenlik İlişkisi (SA) Yaşam Süresi (Zaman) | 28.800 saniye                 | 10.800 saniye                                                   |


### IKE Aşama 2 kurulumu

| **Özellik**                                                             | **İlke tabanlı**                 | **Rota tabanlı ve Standart ya da Yüksek Performanslı VPN Gateway**   |
|--------------------------------------------------------------------------|------------------------------------------------|--------------------------------------------------------------------|
| IKE Sürümü                                                              | IKEv1                                          | IKEv2                                                              |
| Karma Algoritma                                                        | SHA1(SHA128)                                   | SHA1(SHA128)                                                       |
| Aşama 2 Güvenlik İlişkisi (SA) Yaşam süresi (Saat)                        | 3.600 saniye                                  | 3.600 saniye                                                                  |
| Aşama 2 Güvenlik İlişkisi (SA) Yaşam süresi (Verim)                  | 102.400.000 KB                                 | -                                                                  |
| IPsec SA Şifreleme ve kimlik Doğrulama Teklifleri (öncelik sırasına göre) | 1. ESP-AES256 2. ESP-AES128 3. ESP-3DES 4. Yok | Bkz: *Rota tabanlı Ağ Geçidi IPsec Güvenlik İlişkisi (SA) Teklifleri* (aşağıda) |
| Kusursuz İletme Gizliliği (PFS)                                            | Hayır                                             | Evet (DH grubu1, 2, 5, 14, 24)                                                    |
| Kullanılmayan Eş Algılama                                                      | Desteklenmiyor                                  | Destekleniyor                                                          |

### Rota tabanlı Ağ Geçidi IPsec Güvenlik İlişkisi (SA) Teklifleri

Aşağıdaki tabloda IPsec SA Şifreleme ve Kimlik Doğrulama Teklifleri listelenmiştir. Teklifler, teklifin sunulduğu ya da kabul edildiği tercih sırasına göre listelenmiştir.

| **IPsec SA Şifreleme ve Kimlik Doğrulama Teklifleri** | **Başlatıcı olarak Azure Gateway**                               | **Yanıtlayıcı olarak Azure Gateway**                                   |
|---------------------------------------------------|--------------------------------------------------------------|--------------------------------------------------------------|
| 1                                                 | ESP AES_256 SHA                                              | ESP AES_128 SHA                                              |
| 2                                                 | ESP AES_128 SHA                                              | ESP 3_DES MD5                                                |
| 3                                                 | ESP 3_DES MD5                                                | ESP 3_DES SHA                                                |
| 4                                                 | ESP 3_DES SHA                                                | AH SHA1 ile ESP AES_128 ve HMAC Null                      |
| 5                                                 | AH SHA1 ile ESP AES_256 ve HMAC Null                      | AH SHA1 ile ESP 3_DES ve HMAC Null                        |
| 6                                                 | AH SHA1 ile ESP AES_128 ve HMAC Null                      | AH MD5 ile ESP 3_DES ve HMAC Null, önerilen yaşam süresi yok |
| 7                                                 | AH SHA1 ile ESP 3_DES ve HMAC Null                        | AH SHA1 ile ESP 3_DES SHA1, yaşam süresi yok                    |
| 8                                                 | AH MD5 ile ESP 3_DES ve HMAC Null, önerilen yaşam süresi yok | AH MD5 ile ESP 3_DES MD5, yaşam süresi yok                     |
| 9                                                 | AH SHA1 ile ESP 3_DES SHA1, yaşam süresi yok                    | ESP DES MD5                                                  |
| 10                                                | AH MD5 ile ESP 3_DES MD5, yaşam süresi yok                     | ESP DES SHA1, yaşam süresi yok                                   |
| 11                                                | ESP DES MD5                                                  | AH SHA1 ile ESP DES HMAC Null, önerilen yaşam süresi yok        |
| 12                                                | ESP DES SHA1, yaşam süresi yok                                   | AH MD5 ile ESP DES HMAC Null, önerilen yaşam süresi yok        |
| 13                                                | AH SHA1 ile ESP DES HMAC Null, önerilen yaşam süresi yok        | AH SHA1 ile ESP DES SHA1, yaşam süresi yok                      |
| 14                                                | AH MD5 ile ESP DES HMAC Null, önerilen yaşam süresi yok        | AH MD5 ile ESP DES MD5, yaşam süresi yok                       |
| 15                                                | AH SHA1 ile ESP DES SHA1, yaşam süresi yok                      | ESP SHA, yaşam süresi yok                                        |
| 16                                                | AH MD5 ile ESP DES MD5, yaşam süresi yok                       | ESP MD5, yaşam süresi yok                                        |
| 17                                                | -                                                            | AH SHA, yaşam süresi yok                                         |
| 18                                                | -                                                            | AH MD5, yaşam süresi yok                                         |


- Rota tabanlı ve Yüksek Performanslı VPN Gateway’ler ile IPsec ESP NULL şifrelemesini belirtebilirsiniz. Null tabanlı şifreleme aktarımdaki verilere koruma sağlamaz ve yalnızca maksimum performans ve minimum gecikme gerekli olduğunda kullanılmalıdır.  İstemciler, bunu VNet - VNet iletişim senaryolarında ya da çözümdeki başka bir yere şifreleme uygulandığında kullanmayı seçebilir.

- Internet üzerinden şirket içi ve dışı bağlantı için, kritik iletişiminizin güvenliğini sağlamak üzere yukarıdaki tablolarda listelenen şifreleme ve karma algoritmalarla birlikte varsayılan Azure VPN Gateway ayarlarını kullanın.








<!--HONumber=Aug16_HO4-->


