---
title: "Azure noktadan siteye bağlantı sorunlarını giderme | Microsoft Docs"
description: "Noktadan siteye bağlantı sorunlarını giderme öğrenin."
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/14/2017
ms.author: genli
ms.openlocfilehash: 69d363b5ff0b94884cf6d13ae0260f3747e4e69a
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2017
---
# <a name="troubleshooting-azure-point-to-site-connection-problems"></a>Giderme: Azure noktadan siteye bağlantı sorunlarını

Bu makalede karşılaşabileceğiniz genel noktadan siteye bağlantı sorunları listeler. Ayrıca bu sorunlar için olası nedenler ve çözümler açıklanmaktadır.

## <a name="vpn-client-error-a-certificate-could-not-be-found"></a>VPN istemci hatası: bir sertifika bulunamadı

### <a name="symptom"></a>Belirti

VPN istemcisi kullanarak bir Azure sanal ağa bağlanmaya çalıştığında aşağıdaki hata iletisini alıyorsunuz:

**Bu Genişletilebilir kimlik doğrulama protokolü ile kullanılabilir bir sertifika bulunamadı. (Hata 798)**

### <a name="cause"></a>Nedeni

İstemci sertifikası eksik Bu sorun oluşur **Sertifikalar - Geçerli User\Personal\Certificates**.

### <a name="solution"></a>Çözüm

Bu sorunu çözmek için şu adımları izleyin:

1. Aşağıdaki sertifikalar ve doğru konumda olduğundan emin olun:

    | Sertifika | Konum |
    | ------------- | ------------- |
    | AzureClient.pfx  | Geçerli User\Personal\Certificates |
    | Azuregateway -*GUID*. cloudapp.net  | Geçerli User\Trusted kök sertifika yetkilileri|
    | AzureGateway -*GUID*. cloudapp.net, AzureRoot.cer    | Yerel bilgisayar/güvenilen kök sertifika yetkilileri|

2. Kullanıcıların gidin\<kullanıcı adı > \AppData\Roaming\Microsoft\Network\Connections\Cm\<GUID >, el ile kullanıcı ve bilgisayarın deposu (*.cer dosyası) sertifika yükleyin.

İstemci sertifikası yükleme hakkında daha fazla bilgi için bkz: [noktadan siteye bağlantıları için oluşturma ve verme sertifikaları](vpn-gateway-certificates-point-to-site.md).

> [!NOTE]
> İstemci sertifikasını içeri aktardığınızda seçmeyin **güçlü özel anahtar korumasını etkinleştir** seçeneği.

## <a name="vpn-client-error-the-message-received-was-unexpected-or-badly-formatted"></a>VPN istemci hatası: beklenmeyen veya hatalı biçimlendirilmiş bir ileti alındı

### <a name="symptom"></a>Belirti

VPN istemcisi kullanarak bir Azure sanal ağa bağlanmaya çalıştığında aşağıdaki hata iletisini alıyorsunuz:

**Alınan ileti beklenmeyen veya hatalı biçimlendirilmiş. (Hata 0x80090326)**

### <a name="cause"></a>Nedeni

Kök sertifika genel anahtarı Azure VPN ağ geçidine yüklenmemiştir Bu sorun oluşur. Anahtar bozulmuş ya da süresi dolmuş da oluşabilir.

### <a name="solution"></a>Çözüm

Bu sorunu gidermek için kök sertifikası iptal edilmiş olup olmadığını görmek için Azure portalında durumunu kontrol edin. Bunu iptal edilmediğini, kök sertifikasını ve reupload silmeyi deneyin. Daha fazla bilgi için bkz: [oluşturma sertifikaları](vpn-gateway-howto-point-to-site-classic-azure-portal.md#generatecerts).

## <a name="vpn-client-error-a-certificate-chain-processed-but-terminated"></a>VPN istemci hatası: bir sertifika zinciri işlendi, ancak sona erdi 

### <a name="symptom"></a>Belirti 

VPN istemcisi kullanarak bir Azure sanal ağa bağlanmaya çalıştığında aşağıdaki hata iletisini alıyorsunuz:

**Bir sertifika zinciri işlendi, ancak güven sağlayıcısı tarafından güvenilmeyen bir kök sertifika sona erdi.**

### <a name="solution"></a>Çözüm

1. Aşağıdaki sertifikalar ve doğru konumda olduğundan emin olun:

    | Sertifika | Konum |
    | ------------- | ------------- |
    | AzureClient.pfx  | Geçerli User\Personal\Certificates |
    | Azuregateway -*GUID*. cloudapp.net  | Geçerli User\Trusted kök sertifika yetkilileri|
    | AzureGateway -*GUID*. cloudapp.net, AzureRoot.cer    | Yerel bilgisayar/güvenilen kök sertifika yetkilileri|

2. Sertifikaları konumu zaten varsa, sertifikaları silin ve yeniden deneyin.  **Azuregateway -*GUID*. Azure portalından indirdiğiniz VPN istemcisi yapılandırma paketini cloudapp.net** sertifika konusu. Dosya archivers paketinden dosyaları ayıklayın için kullanabilirsiniz.

## <a name="file-download-error-target-uri-is-not-specified"></a>Dosya indirme hatası: hedef URI belirtilmedi

### <a name="symptom"></a>Belirti

Aşağıdaki hata iletisini alıyorsunuz:

**Dosya indirme hatası. Hedef URI belirtilmedi.**

### <a name="cause"></a>Nedeni 

Bu sorun bir hatalı ağ geçidi türü nedeniyle oluşur. 

### <a name="solution"></a>Çözüm

VPN ağ geçidi türü olmalıdır **VPN**, ve VPN türü olmalıdır **RouteBased**.

## <a name="vpn-client-error-azure-vpn-custom-script-failed"></a>VPN istemci hatası: Azure VPN özel komut başarısız oldu 

### <a name="symptom"></a>Belirti

VPN istemcisi kullanarak bir Azure sanal ağa bağlanmaya çalıştığında aşağıdaki hata iletisini alıyorsunuz:

**(Yönlendirme tablonuzu güncelleştirmek için) özel bir komut dosyası başarısız oldu. (Hata 8007026f)**

### <a name="cause"></a>Nedeni

Kısayol kullanarak site noktası VPN bağlantısı açmaya çalıştığınız, bu sorun ortaya çıkabilir.

### <a name="solution"></a>Çözüm 

Kısayoldan açmadan yerine doğrudan VPN paketini açın.

## <a name="cannot-install-the-vpn-client"></a>VPN istemci yüklenemiyor

### <a name="cause"></a>Nedeni 

Ek bir sertifika, VPN ağ geçidi sanal ağınız için güvenmesi için gereklidir. Sertifika Azure portalından oluşturulan VPN istemcisi yapılandırma paketini dahil edilir.

### <a name="solution"></a>Çözüm

VPN istemcisi yapılandırma paketini ayıklamak ve .cer dosyasını bulun. Sertifikayı yüklemek için aşağıdaki adımları izleyin:

1. MMC.exe açın.
2. Ekleme **sertifikaları** ek bileşenini.
3. Seçin **bilgisayar** yerel bilgisayar hesabı.
4. Sağ **güvenilen kök sertifika yetkilileri** düğümü. Tıklatın **tüm görev** > **alma**ve VPN istemci yapılandırma paketi ayıkladığınız .cer dosyasının konumuna göz atın.
5. Bilgisayarı yeniden başlatın. 
6. VPN istemcisi yüklemeyi deneyin.

## <a name="azure-portal-error-failed-to-save-the-vpn-gateway-and-the-data-is-invalid"></a>Azure portal hata: VPN ağ geçidi kaydedemedi ve veri geçersiz

### <a name="symptom"></a>Belirti

Azure portalında VPN ağ geçidi değişiklikleri kaydetmeyi denediğinizde aşağıdaki hata iletisini alıyorsunuz:

**Sanal ağ geçidi kaydedilemedi &lt;* ağ geçidi adı*&gt;. Sertifikasının verileri &lt; *kimliği sertifika* &gt; olan invalid.* *

### <a name="cause"></a>Nedeni 

Karşıya yüklediğiniz kök sertifikanın ortak anahtarı bir alanı gibi geçersiz bir karakter içeriyorsa, bu sorun ortaya çıkabilir.

### <a name="solution"></a>Çözüm

Sertifika verileri satır sonları (satır başı) gibi geçersiz karakterler içermediğinden emin olun. Değerin tamamını uzun bir satır olması gerekir. Aşağıdaki metni, sertifikanın bir örnektir:

    -----BEGIN CERTIFICATE-----
    MIIC5zCCAc+gAwIBAgIQFSwsLuUrCIdHwI3hzJbdBjANBgkqhkiG9w0BAQsFADAW
    MRQwEgYDVQQDDAtQMlNSb290Q2VydDAeFw0xNzA2MTUwMjU4NDZaFw0xODA2MTUw
    MzE4NDZaMBYxFDASBgNVBAMMC1AyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEF
    AAOCAQ8AMIIBCgKCAQEAz8QUCWCxxxTrxF5yc5uUpL/bzwC5zZ804ltB1NpPa/PI
    sa5uwLw/YFb8XG/JCWxUJpUzS/kHUKFluqkY80U+fAmRmTEMq5wcaMhp3wRfeq+1
    G9OPBNTyqpnHe+i54QAnj1DjsHXXNL4AL1N8/TSzYTm7dkiq+EAIyRRMrZlYwije
    407ChxIp0stB84MtMShhyoSm2hgl+3zfwuaGXoJQwWiXh715kMHVTSj9zFechYd7
    5OLltoRRDyyxsf0qweTFKIgFj13Hn/bq/UJG3AcyQNvlCv1HwQnXO+hckVBB29wE
    sF8QSYk2MMGimPDYYt4ZM5tmYLxxxvGmrGhc+HWXzMeQIDAQABozEwLzAOBgNVHQ8B
    Af8EBAMCAgQwHQYDVR0OBBYEFBE9zZWhQftVLBQNATC/LHLvMb0OMA0GCSqGSIb3
    DQEBCwUAA4IBAQB7k0ySFUQu72sfj3BdNxrXSyOT4L2rADLhxxxiK0U6gHUF6eWz
    /0h6y4mNkg3NgLT3j/WclqzHXZruhWAXSF+VbAGkwcKA99xGWOcUJ+vKVYL/kDja
    gaZrxHlhTYVVmwn4F7DWhteFqhzZ89/W9Mv6p180AimF96qDU8Ez8t860HQaFkU6
    2Nw9ZMsGkvLePZZi78yVBDCWMogBMhrRVXG/xQkBajgvL5syLwFBo2kWGdC+wyWY
    U/Z+EK9UuHnn3Hkq/vXEzRVsYuaxchta0X2UNRzRq+o706l+iyLTpe6fnvW6ilOi
    e8Jcej7mzunzyjz4chN0/WVF94MtxbUkLkqP
    -----END CERTIFICATE-----

## <a name="azure-portal-error-failed-to-save-the-vpn-gateway-and-the-resource-name-is-invalid"></a>Azure portal hata: VPN ağ geçidi kaydedilemedi ve kaynak adı geçersiz

### <a name="symptom"></a>Belirti

Azure portalında VPN ağ geçidi değişiklikleri kaydetmeyi denediğinizde aşağıdaki hata iletisini alıyorsunuz: 

**Sanal ağ geçidi kaydedilemedi &lt;* ağ geçidi adı*&gt;. Kaynak adı &lt; *deneyin karşıya yüklemek için sertifika adı* &gt; geçersiz ** değil.

### <a name="cause"></a>Nedeni

Sertifika adı bir boşluk gibi geçersiz bir karakter içerdiğinden bu sorun oluşur. 

## <a name="azure-portal-error-vpn-package-file-download-error-503"></a>Azure portal hata: VPN paket dosyasını indirme hatası 503

### <a name="symptom"></a>Belirti

VPN istemcisi yapılandırma paketini yüklemeye çalışırken aşağıdaki hata iletisini alıyorsunuz:

**Dosyası karşıdan yüklenemedi. Hata ayrıntıları: 503 hatası. Sunucu meşgul.**
 
### <a name="solution"></a>Çözüm

Bu hata geçici bir ağ sorunu neden olabilir. VPN paketini indir birkaç dakika sonra yeniden deneyin.

## <a name="azure-vpn-gateway-upgrade-all-p2s-clients-are-unable-to-connect"></a>Azure VPN ağ geçidi yükseltme: tüm P2S istemcileridir bağlanamıyor

### <a name="cause"></a>Nedeni

Sertifika yüzde 50'den fazla ise yaşam sertifika alındı.

### <a name="solution"></a>Çözüm

Bu sorunu gidermek için oluşturun ve yeni sertifikalar VPN istemcileri için yeniden dağıtın. 

## <a name="too-many-vpn-clients-connected-at-once"></a>Çok fazla VPN istemcileri aynı anda bağlı

Her VPN ağ geçidi için en fazla izin verilen bağlantı sayısı 128'dir. Azure portalında bağlanan istemcilerin toplam sayısı görebilirsiniz.

## <a name="point-to-site-vpn-incorrectly-adds-a-route-for-100008-to-the-route-table"></a>Noktadan siteye VPN, 10.0.0.0/8 için bir yol yanlış yol tablosuna ekler.

### <a name="symptom"></a>Belirti

Noktadan siteye istemci VPN bağlantısında çevirdiğinizde, VPN istemcisi Azure sanal ağı doğru bir yol eklemeniz gerekir. IP yardımcı hizmeti, VPN istemcileri alt ağ için bir yol eklemeniz gerekir. 

VPN istemci aralığı 10.0.12.0/24 gibi 10.0.0.0/8, daha küçük bir alt ağa ait. 10.0.12.0/24 için bir yol yerine, daha yüksek önceliğe sahip 10.0.0.0/8 için bir rota eklenir. 

Bu yanlış yol tanımlanan belirli bir yolu olmayan bu 10.0.0.0/8 aralıkta 10.50.0.0/24 gibi başka bir alt ağa ait olabilir diğer şirket içi ağlar ile bağlantısını keser. 

### <a name="cause"></a>Nedeni

Bu davranış, Windows istemcileri için tasarım gereğidir. İstemci PPP IPCP protokol kullandığında, tünel arabirimi için IP adresi (Bu durumda VPN gateway) sunucusundan alır. Ancak, protokolünde bir sınırlama nedeniyle, istemci alt ağ maskesi yok. Edinilir başka hiçbir yolu olduğundan, istemci tünel arabirimi IP adresi sınıfına göre alt ağ maskesi tahmin etmeye çalışır. 

Bu nedenle, bir yol aşağıdaki statik eşleme göre eklenir: 

Adres sınıf A aitse--> /8 Uygula

Adres B--> sınıfına aitse /16 Uygula

Adres C--> sınıfına aitse /24 Uygula

## <a name="vpn-client-cannot-access-network-file-shares"></a>VPN istemcisi ağ dosya paylaşımlarına erişemez

### <a name="symptom"></a>Belirti

Azure sanal ağı için VPN istemcisi bağlandı. Ancak, istemci ağ paylaşımlarına erişemez.

### <a name="cause"></a>Nedeni

SMB protokolü dosya paylaşımı erişimi için kullanılır. Bağlantısı başlatıldığında VPN istemcisi oturum bilgilerini ekler ve hata oluşur. Bağlantı kurulduktan sonra istemci kimlik bilgilerini önbelleğe için Kerberos kimlik doğrulamasını kullanmak için zorlanır. Bu işlem merkezi bir belirteç almak üzere anahtar dağıtım (bir etki alanı denetleyicisi) sorguları başlatır. İstemci Internet'ten bağlandığından, etki alanı denetleyicisi ulaşabilmesi olmayabilir. Bu nedenle, istemci üzerinde Kerberos'tan NTLM olarak çalışamaz. 

Olduğunda bir kimlik bilgisi için istemci istenir yalnızca bir kez geçerli bir sertifika sahiptir (SAN ile UPN =) katılan etki alanı tarafından verilmiş. İstemci ayrıca fiziksel etki alanı ağına bağlı olması gerekir. Bu durumda, istemci sertifikasını kullanmayı dener ve etki alanı denetleyicisine ulaştığında. Daha sonra anahtar dağıtım merkezi bir "KDC_ERR_C_PRINCIPAL_UNKNOWN" hatası döndürür. İstemci için NTLM üzerinden vermesine zorlanır. 

### <a name="solution"></a>Çözüm

Sorunu çözmek için aşağıdaki kayıt defteri alt etki alanı kimlik bilgilerini önbelleğe alma devre dışı bırakın: 

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\DisableDomainCreds - Set the value to 1 


## <a name="cannot-find-the-point-to-site-vpn-connection-in-windows-after-reinstalling-the-vpn-client"></a>Noktadan siteye VPN bağlantısı VPN istemcisi yeniden yükledikten sonra Windows bulunamıyor

### <a name="symptom"></a>Belirti

Noktadan siteye VPN bağlantısını kaldırın ve VPN istemcisi yeniden yükleyin. Bu durumda, VPN bağlantısı başarıyla yapılandırılmadı. VPN bağlantısı görüyor musunuz **ağ bağlantıları** Windows ayarları.

### <a name="solution"></a>Çözüm

Sorunu gidermek için eski VPN istemci yapılandırma dosyalarını silin **C:\Users\TheUserName\AppData\Roaming\Microsoft\Network\Connections**, ve ardından VPN istemcisi yükleyicisini çalıştırın.

## <a name="point-to-site-vpn-client-cannot-resolve-the-fqdn-of-the-resources-in-the-local-domain"></a>Noktadan siteye VPN istemcisi yerel etki alanındaki kaynaklara FQDN'si çözümlenemiyor.

### <a name="symptom"></a>Belirti

İstemci, noktadan siteye VPN bağlantısı kullanarak Azure'a bağlandığında, yerel etki alanındaki kaynaklara FQND çözümlenemiyor.

### <a name="cause"></a>Nedeni

Noktadan siteye VPN istemcisi Azure sanal ağında yapılandırılmış Azure DNS sunucularını kullanır. Azure DNS sunucularını tüm DNS sorgularını Azure DNS sunucularına gönderilir, böylece istemcisi yapılandırılmış olan yerel DNS sunucularını daha önceliklidir. Azure DNS sunucularını yerel kaynakları için kayıtlar yoksa, sorgu başarısız olur.

### <a name="solution"></a>Çözüm

Sorunu gidermek için Azure DNS sunucuları, Azure sanal ağda kullanılan emin olmak için yerel kaynakları için DNS kayıtlarını çözebilirsiniz. Bunu yapmak için DNS ileticileri veya koşullu ileticileri kullanabilirsiniz. Daha fazla bilgi için bkz: [kendi DNS sunucu kullanılarak ad çözümleme](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server)

## <a name="the-point-to-site-vpn-connection-is-established-but-you-still-cannot-connect-to-azure-resources"></a>Noktadan siteye VPN bağlantısı kuruldu, ancak Azure kaynaklarına hala bağlanamıyor 

### <a name="cause"></a>Nedeni

VPN istemcisi Azure VPN ağ geçidi'nden yolları almazsa Bu sorun ortaya çıkabilir.

### <a name="solution"></a>Çözüm

Bu sorunu gidermek için [Azure VPN gateway sıfırlama](vpn-gateway-resetgw-classic.md).

## <a name="error-the-revocation-function-was-unable-to-check-revocation-because-the-revocation-server-was-offlineerror-0x80092013"></a>Hata: "İptal işlevi iptal sunucu çevrimdışı olduğundan iptal denetleyemedi. (Hata 0x80092013)"

### <a name="causes"></a>Neden olur.
İstemci http://crl3.digicert.com/ssca-sha2-g1.crl ve http://crl4.digicert.com/ssca-sha2-g1.cr erişemiyorsanız, bu hata iletisi oluşur.  Bu iki site erişimi iptal denetimi gerektirir.  Bu sorun genellikle yapılandırılan proxy sunucusu olan istemcide olur. Bazı ortamlarda isteklerin proxy sunucu üzerinden değil kullanacaksanız, sınır güvenlik duvarında reddedilir.

### <a name="solution"></a>Çözüm

Proxy sunucu ayarlarını denetleyin, istemci http://crl3.digicert.com/ssca-sha2-g1.crl ve http://crl4.digicert.com/ssca-sha2-g1.cr erişebildiğinden emin olun.

## <a name="vpn-client-error-the-connection-was-prevented-because-of-a-policy-configured-on-your-rasvpn-server-error-812"></a>VPN istemci hatası: RAS/VPN sunucunuzda yapılandırılan bir ilkesi nedeniyle bağlantı engellendi. (Hata 812)

### <a name="cause"></a>Nedeni

VPN istemci kimlik doğrulaması için kullanılan RADIUS sunucusu ayarları yanlış varsa, bu hata oluşur. 

### <a name="solution"></a>Çözüm

RADIUS sunucusu doğru şekilde yapılandırıldığından emin olun. Daha fazla bilgi için bkz: [Azure multi-Factor Authentication sunucusu ile tümleştirmek RADIUS kimlik doğrulaması](../multi-factor-authentication/multi-factor-authentication-get-started-server-radius.md).

## <a name="error-405-when-you-download-root-certificate-from-vpn-gateway"></a>"Hatası 405" ne zaman karşıdan kök sertifikası VPN ağ geçidi'nden

### <a name="cause"></a>Nedeni

Kök sertifika yüklenmişse değil. İstemcinin içinde kök sertifika yüklü **güvenilen Sertifikalar** depolar.