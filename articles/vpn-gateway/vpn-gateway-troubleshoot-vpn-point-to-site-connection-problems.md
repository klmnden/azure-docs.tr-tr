---
title: Azure noktadan siteye bağlantı sorunlarını giderme | Microsoft Docs
description: Noktadan siteye bağlantı sorunları gidermeyi öğrenin.
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: cshepard
editor: ''
tags: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/31/2019
ms.author: genli
ms.openlocfilehash: cab40284f36f21f9de72ee4dc1faf78153621d26
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66475963"
---
# <a name="troubleshooting-azure-point-to-site-connection-problems"></a>Sorun giderme: Azure noktadan siteye bağlantı sorunları

Bu makalede, karşılaşabileceğiniz genel noktadan siteye bağlantı sorunları listeler. Ayrıca bu sorunlar için olası nedenler ve çözümler açıklanmaktadır.

## <a name="vpn-client-error-a-certificate-could-not-be-found"></a>VPN istemci hatası: Bir sertifika bulunamadı

### <a name="symptom"></a>Belirti

VPN istemcisini kullanarak bir Azure sanal ağına bağlanmaya çalıştığınızda, aşağıdaki hata iletisini alıyorsunuz:

**Bu Genişletilebilir kimlik doğrulama protokolü ile kullanılabilen bir sertifika bulunamadı. (Hata 798)**

### <a name="cause"></a>Nedeni

İstemci sertifikası eksik olması durumunda bu sorun oluşur **Sertifikalar - Geçerli kullanıcı\kişisel\sertifikalar**.

### <a name="solution"></a>Çözüm

Bu sorunu çözmek için aşağıdaki adımları izleyin:

1. Sertifika Yöneticisi'ni açın: Tıklayın **Başlat**, türü **bilgisayar sertifikalarını yönetme**ve ardından **bilgisayar sertifikalarını yönetme** arama sonucunda.

2. Aşağıdaki sertifikalar doğru konumda olduğundan emin olun:

    | Sertifika | Location |
    | ------------- | ------------- |
    | AzureClient.pfx  | Geçerli kullanıcı\kişisel\sertifikalar |
    | Azuregateway -*GUID*. cloudapp.net  | Geçerli User\Trusted kök sertifika yetkilileri|
    | AzureGateway -*GUID*. cloudapp.net, AzureRoot.cer    | Yerel bilgisayar/güvenilen kök sertifika yetkilileri|

3. C:\Users Git\<kullanıcıadı > \AppData\Roaming\Microsoft\Network\Connections\Cm\<GUID >, el ile sertifika (*.cer dosyası) kullanıcı ve bilgisayar deposuna yükleyin.

İstemci sertifikasını yükleme hakkında daha fazla bilgi için bkz. [noktadan siteye bağlantılar için sertifikaları oluşturma ve dışarı aktarma](vpn-gateway-certificates-point-to-site.md).

> [!NOTE]
> İstemci sertifikasını içeri aktardığınızda seçmeyin **güçlü özel anahtar korumasını etkinleştir** seçeneği.

## <a name="the-network-connection-between-your-computer-and-the-vpn-server-could-not-be-established-because-the-remote-server-is-not-responding"></a>Uzak sunucu yanıt vermediği için bilgisayarınızı VPN sunucusu arasında ağ bağlantısı kurulamadı

### <a name="symptom"></a>Belirti

Deneyin ve Windows üzerinde IKEv2'yi kullanarak bir Azure sanal ağ geçidine bağlanmak, şu hata iletisiyle karşılaşırsınız:

**Uzak sunucu yanıt vermediği için bilgisayarınızı VPN sunucusu arasında ağ bağlantısı kurulamadı**

### <a name="cause"></a>Nedeni
 
 IKE parçalanma için destek Windows sürümüne sahip değilse sorun ortaya çıkar.
 
### <a name="solution"></a>Çözüm

IKEv2, Windows 10 ve Server 2016’da desteklenir. Ancak IKEv2 kullanmak için güncelleştirmeleri yüklemeli ve yerel bir kayıt defteri anahtar değeri ayarlamalısınız. Windows 10’dan önceki işletim sistemleri desteklenmez ve yalnızca SSTP kullanabilir.

IKEv2 için Windows 10 ve Server 2016’yı hazırlamak için:

1. Güncelleştirmeyi yükleyin.

   | İşletim sistemi sürümü | Tarih | Sayı/Bağlantı |
   |---|---|---|---|
   | Windows Server 2016<br>Windows 10 Sürüm 1607 | 17 Ocak 2018 | [KB4057142](https://support.microsoft.com/help/4057142/windows-10-update-kb4057142) |
   | Windows 10 Sürüm 1703 | 17 Ocak 2018 | [KB4057144](https://support.microsoft.com/help/4057144/windows-10-update-kb4057144) |
   | Windows 10 sürüm 1709 | 22 Mart 2018 | [KB4089848](https://www.catalog.update.microsoft.com/search.aspx?q=kb4089848) |
   |  |  |  |  |

2. Kayıt defteri anahtar değerini ayarlayın. Kayıt defterinde “HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\RasMan\ IKEv2\DisableCertReqPayload” REG_DWORD anahtarını oluşturun veya 1 olarak ayarlayın.

## <a name="vpn-client-error-the-message-received-was-unexpected-or-badly-formatted"></a>VPN istemci hatası: Beklenmeyen veya kötü biçimlendirilmiş bir ileti alındı.

### <a name="symptom"></a>Belirti

VPN istemcisini kullanarak bir Azure sanal ağına bağlanmaya çalıştığınızda, aşağıdaki hata iletisini alıyorsunuz:

**Beklenmeyen veya kötü biçimlendirilmiş bir ileti alındı. (Hata 0x80090326)**

### <a name="cause"></a>Nedeni

Aşağıdaki koşullardan biri doğru olması durumunda bu sorun oluşur:

- Hatalı kullanım kullanıcı tanımlı yollar (UDR) ağ geçidi alt ağı üzerinde varsayılan yol ile ayarlanır.
- Azure VPN ağ geçidine kök sertifikanın ortak anahtarı karşıya yüklenmemiş. 
- Anahtarı bozuk veya süresi doldu.

### <a name="solution"></a>Çözüm

Bu sorunu çözmek için aşağıdaki adımları izleyin:

1. Ağ geçidi alt ağı üzerinde UDR kaldırın. UDR tüm trafiğin düzgün bir şekilde iletir emin olun.
2. Kök sertifika iptal edildi olup olmadığını görmek için Azure portalında durumunu denetleyin. İptal değil, kök sertifika ve reupload silmeyi deneyin. Daha fazla bilgi için [sertifikaları oluşturma](vpn-gateway-howto-point-to-site-classic-azure-portal.md#generatecerts).

## <a name="vpn-client-error-a-certificate-chain-processed-but-terminated"></a>VPN istemci hatası: Bir sertifika zinciri işlenen ancak sonlandırıldı 

### <a name="symptom"></a>Belirti 

VPN istemcisini kullanarak bir Azure sanal ağına bağlanmaya çalıştığınızda, aşağıdaki hata iletisini alıyorsunuz:

**Bir sertifika zinciri işlenen ancak güven sağlayıcısı tarafından güvenilmeyen bir kök sertifika sona erdi.**

### <a name="solution"></a>Çözüm

1. Aşağıdaki sertifikalar doğru konumda olduğundan emin olun:

    | Sertifika | Location |
    | ------------- | ------------- |
    | AzureClient.pfx  | Geçerli kullanıcı\kişisel\sertifikalar |
    | Azuregateway -*GUID*. cloudapp.net  | Geçerli User\Trusted kök sertifika yetkilileri|
    | AzureGateway -*GUID*. cloudapp.net, AzureRoot.cer    | Yerel bilgisayar/güvenilen kök sertifika yetkilileri|

2. Sertifikaların konumda zaten olması durumunda, sertifikalar silin ve yeniden deneyin. **Azuregateway -*GUID*. cloudapp.net** Azure portalından indirdiğiniz VPN istemcisi yapılandırma paketini sertifika konusu. Dosya archivers paketinden dosyaları ayıklayın için kullanabilirsiniz.

## <a name="file-download-error-target-uri-is-not-specified"></a>Dosya indirme hatası: Hedef URI belirtilmedi

### <a name="symptom"></a>Belirti

Aşağıdaki hata iletisini alıyorsunuz:

**Dosya indirme hatası. Hedef URI belirtilmedi.**

### <a name="cause"></a>Nedeni 

Bu sorun, bir hatalı ağ geçidi türü nedeniyle oluşur. 

### <a name="solution"></a>Çözüm

VPN ağ geçidi türü olmalıdır **VPN**, ve VPN türünde olmalıdır **RouteBased**.

## <a name="vpn-client-error-azure-vpn-custom-script-failed"></a>VPN istemci hatası: Azure VPN özel betiği başarısız oldu 

### <a name="symptom"></a>Belirti

VPN istemcisini kullanarak bir Azure sanal ağına bağlanmaya çalıştığınızda, aşağıdaki hata iletisini alıyorsunuz:

**Özel betik (sizin bir yönlendirme tablosu güncelleştirmek için) başarısız oldu. (Hata 8007026f)**

### <a name="cause"></a>Nedeni

Kısayol kullanarak site noktası VPN bağlantısı açar çalışıyorsanız bu sorun oluşabilir.

### <a name="solution"></a>Çözüm 

Kısayoldan açmak yerine doğrudan VPN paketini açın.

## <a name="cannot-install-the-vpn-client"></a>VPN istemci yüklenemiyor

### <a name="cause"></a>Nedeni 

Ek bir sertifika, sanal ağınızın VPN ağ geçidi güvenmesi için gereklidir. Sertifika, Azure portalından oluşturulan VPN istemcisi yapılandırma paketini bulunmaktadır.

### <a name="solution"></a>Çözüm

VPN istemcisi yapılandırma paketini ayıklayın ve .cer dosyasını bulun. Sertifikayı yüklemek için aşağıdaki adımları izleyin:

1. MMC.exe açın.
2. Ekleme **sertifikaları** ek.
3. Seçin **bilgisayar** yerel bilgisayar hesabı.
4. Sağ **güvenilen kök sertifika yetkilileri** düğümü. Tıklayın **tüm görev** > **alma**, VPN istemcisi yapılandırma paketini ayıkladığınız .cer dosyasına göz atın.
5. Bilgisayarı yeniden başlatın. 
6. VPN istemcisi yüklemeyi deneyin.

## <a name="azure-portal-error-failed-to-save-the-vpn-gateway-and-the-data-is-invalid"></a>Azure portal hata: VPN ağ geçidi kaydedilemedi ve verileri geçersiz

### <a name="symptom"></a>Belirti

Azure portalında VPN ağ geçidi değişiklikleri kaydetmeyi denediğinizde, şu hata iletisini alıyorsunuz:

**Sanal ağ geçidi kaydedilemedi &lt; *ağ geçidi adı*&gt;. Sertifika verileri &lt; *kimliği sertifika* &gt; geçersiz.**

### <a name="cause"></a>Nedeni 

Karşıya yüklediğiniz kök sertifikanın ortak anahtarı bir alanı gibi geçersiz bir karakter içeriyorsa, bu sorun oluşabilir.

### <a name="solution"></a>Çözüm

Sertifikadaki verilerin satır sonları (Satır başları) gibi geçersiz karakterler içermediğinden emin olun. Tüm değer bir uzun satır olması gerekir. Sertifikanın bir örneği aşağıda gösterilmiştir:

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

## <a name="azure-portal-error-failed-to-save-the-vpn-gateway-and-the-resource-name-is-invalid"></a>Azure portal hata: VPN ağ geçidini kaydetme başarısız oldu ve kaynak adı geçersiz

### <a name="symptom"></a>Belirti

Azure portalında VPN ağ geçidi değişiklikleri kaydetmeyi denediğinizde, şu hata iletisini alıyorsunuz: 

**Sanal ağ geçidi kaydedilemedi &lt; *ağ geçidi adı*&gt;. Kaynak adı &lt; *deneyin karşıya yüklemek için sertifika adı* &gt; geçersiz**.

### <a name="cause"></a>Nedeni

Sertifika adı boşluk gibi bir geçersiz karakter içerdiğinden bu sorun oluşur. 

## <a name="azure-portal-error-vpn-package-file-download-error-503"></a>Azure portal hata: VPN paketi dosya indirme hatası 503

### <a name="symptom"></a>Belirti

VPN istemcisi yapılandırma paketini yüklemeye çalışırken şu hata iletisini alıyorsunuz:

**Dosya indirilemedi. Hata ayrıntıları: 503 hatası. Sunucu meşgul.**
 
### <a name="solution"></a>Çözüm

Bu hata geçici bir ağ sorunu neden olabilir. VPN paketini indir birkaç dakika sonra tekrar deneyin.

## <a name="azure-vpn-gateway-upgrade-all-point-to-site-clients-are-unable-to-connect"></a>Azure VPN ağ geçidi yükseltme: Tüm noktadan siteye istemcileri bağlanamıyor.

### <a name="cause"></a>Nedeni

Sertifika yüzde 50'den fazla ise, yaşam sertifika alındı.

### <a name="solution"></a>Çözüm

Bu sorunu gidermek için yeniden indirin ve tüm istemcilerde Site paket noktasına yeniden dağıtın.

## <a name="too-many-vpn-clients-connected-at-once"></a>Çok fazla VPN istemcileri tek seferde bağlı

İzin verilen bağlantı sayısı üst sınırına. Azure portalında bağlanan istemcilerin toplam sayısı görebilirsiniz.

## <a name="point-to-site-vpn-incorrectly-adds-a-route-for-100008-to-the-route-table"></a>Noktadan siteye VPN 10.0.0.0/8 için bir rota için rota tablosu yanlış ekler.

### <a name="symptom"></a>Belirti

VPN istemcisi, noktadan siteye istemci VPN bağlantısıyla çevirdiğinizde, Azure sanal ağı doğru bir yol eklemeniz gerekir. IP yardımcı hizmeti, VPN istemcilerinin bir alt ağ için bir rota eklemeniz gerekir. 

10.0.0.0/8, 10.0.12.0/24 gibi daha küçük bir alt ağı için VPN istemci aralığı aittir. 10.0.12.0/24 için bir yol yerine, daha yüksek önceliğe sahip 10.0.0.0/8 için bir yol eklenir. 

Bu yanlış bir yol tanımlanan belirli bir yol olmayan 10.0.0.0/8 aralıkta 10.50.0.0/24 gibi başka bir alt ağa ait olabilir diğer şirket içi ağlar ile bağlantısını keser. 

### <a name="cause"></a>Nedeni

Bu davranış, Windows istemcileri için tasarım gereğidir. İstemci PPP IPCP protokol kullandığında, sunucudan (Bu örnekte VPN gateway) tüneli arabiriminin IP adresini alır. Ancak, protokolü bir sınırlama nedeniyle, alt ağ maskesi istemci yok. Almak için diğer bir yolu yoktur çünkü istemci tüneli arabiriminin IP adresinin sınıfına göre alt ağ maskesi tahmin etmeye çalışır. 

Bu nedenle, bir yol bağlı olarak aşağıdaki statik eşleme eklenir: 

Adresin bir sınıfa ait /8--> uygulayın

Adres B--> sınıfa ait /16 uygulayın

Adres C--> sınıfa ait /24 uygulayın

### <a name="solution"></a>Çözüm

Diğer ağlar için yönlendirme tablosunda en uzun ön ek eşleştirmesi veya noktadan siteye daha düşük ölçüye (Bu nedenle daha yüksek öncelikli) eklenerek yollar vardır. 

## <a name="vpn-client-cannot-access-network-file-shares"></a>VPN istemcisi ağ dosya paylaşımlarına erişemez.

### <a name="symptom"></a>Belirti

VPN istemcisi, Azure sanal ağına bağlandı. Ancak, istemci ağ paylaşımlarına erişemez.

### <a name="cause"></a>Nedeni

SMB protokolü, dosya paylaşımı erişimi için kullanılır. Bağlantısı başlatıldığında, VPN istemcisi oturum bilgilerini ekler ve hata oluşur. Bağlantı kurulduktan sonra istemci kimlik bilgilerini önbelleğe Kerberos kimlik doğrulaması için kullanılacak zorlanır. Bu işlem, merkezi bir belirteç almak için anahtar dağıtım (bir etki alanı denetleyicisi) sorguları başlatır. İstemci Internet'ten bağlandığından, etki alanı denetleyicisine mümkün olmayabilir. Bu nedenle, istemci üzerinde Kerberos'tan NTLM olarak çalışamaz. 

Olduğunda bir kimlik bilgisi için istemci istenir yalnızca bir kez geçerli bir sertifikası olan (SAN ile UPN =) katılan etki alanı tarafından verilmiş. İstemci ayrıca etki alanı ağına fiziksel olarak bağlanmalıdır. Bu durumda, istemci sertifikasını kullanmayı dener ve etki alanı denetleyicisine ulaşır. Daha sonra anahtar dağıtım merkezi bir "KDC_ERR_C_PRINCIPAL_UNKNOWN" hatası döndürür. İstemci, NTLM olarak yük devretmek için zorlanır. 

### <a name="solution"></a>Çözüm

Sorunu çözmek için aşağıdaki kayıt defteri alt etki alanı kimlik bilgilerini önbelleğe alma devre dışı bırakın: 

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\DisableDomainCreds - Set the value to 1 


## <a name="cannot-find-the-point-to-site-vpn-connection-in-windows-after-reinstalling-the-vpn-client"></a>Noktadan siteye VPN bağlantısı Windows VPN istemcisini yeniden yükledikten sonra bulunamıyor

### <a name="symptom"></a>Belirti

Noktadan siteye VPN bağlantısı kaldırıp sonra da VPN istemcisini yeniden yükleyin. Bu durumda, VPN bağlantısı başarıyla yapılandırılmadı. VPN bağlantısı görmüyorsanız **ağ bağlantıları** Windows ayarları.

### <a name="solution"></a>Çözüm

Bu sorunu gidermek için eski VPN istemcisi yapılandırma dosyalarını silin. **C:\Users\UserName\AppData\Roaming\Microsoft\Network\Connections\<sanal ağ kimliği >** , ve ardından VPN istemci yükleyiciyi yeniden çalıştırın .

## <a name="point-to-site-vpn-client-cannot-resolve-the-fqdn-of-the-resources-in-the-local-domain"></a>Noktadan siteye VPN istemcisi yerel etki alanındaki kaynaklara FQDN'si çözümlenemiyor

### <a name="symptom"></a>Belirti

İstemci, noktadan siteye VPN bağlantısı kullanarak Azure'a bağlandığında, bu, yerel etki alanındaki kaynaklara FQDN'si çözümlenemiyor.

### <a name="cause"></a>Nedeni

Noktadan siteye VPN istemcisi, Azure sanal ağında yapılandırılmış olan Azure DNS sunucularını kullanır. Azure DNS sunucularını tüm DNS sorgularını Azure DNS sunucularına gönderilir, istemcide yapılandırılan yerel DNS sunucularını daha önceliklidir. Azure DNS sunucularını yerel kaynakları için kayıtları yoksa, sorgu başarısız olur.

### <a name="solution"></a>Çözüm

Azure DNS sunucuları, Azure sanal ağda kullanılan emin olun sorunu çözmek için yerel kaynakları için DNS kayıtlarını çözebilirsiniz. Bunu yapmak için DNS ileticileri veya koşullu ileticileri kullanabilirsiniz. Daha fazla bilgi için [kendi DNS sunucunuzu kullanarak ad çözümlemesi](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server)

## <a name="the-point-to-site-vpn-connection-is-established-but-you-still-cannot-connect-to-azure-resources"></a>Noktadan siteye VPN bağlantısı kuruldu, ancak yine de Azure kaynakları için bağlantı kurulamıyor 

### <a name="cause"></a>Nedeni

VPN istemcisi Azure VPN ağ geçidinden yolları almazsa, bu sorun oluşabilir.

### <a name="solution"></a>Çözüm

Bu sorunu çözmek için [Azure VPN Gateway'i sıfırlama](vpn-gateway-resetgw-classic.md). Sanal Ağ eşlemesi başarıyla yapılandırıldıktan sonra yeni yollar kullanılan emin olmak için noktadan siteye VPN istemcileri yeniden yüklenmesi gerekir.

## <a name="error-the-revocation-function-was-unable-to-check-revocation-because-the-revocation-server-was-offlineerror-0x80092013"></a>Hata: "İptal işlevi iptal iptal etme sunucusu çevrimdışı olduğundan denetlenemiyor. (Hata 0x80092013)"

### <a name="causes"></a>Nedenler
İstemci erişemiyorsanız bu hata iletisi görüntüleniyor http://crl3.digicert.com/ssca-sha2-g1.crl ve http://crl4.digicert.com/ssca-sha2-g1.crl.  İptal denetimini, bu iki site erişim gerektirir.  Bu sorun genellikle yapılandırılan proxy sunucusu olan istemcide gerçekleşir. Bazı ortamlarda, isteklerin bir proxy sunucu üzerinden etmeyecekseniz, kenar güvenlik duvarında reddedilir.

### <a name="solution"></a>Çözüm

Ara sunucu ayarlarını denetleyin, istemci erişebildiğinden emin olun http://crl3.digicert.com/ssca-sha2-g1.crl ve http://crl4.digicert.com/ssca-sha2-g1.crl.

## <a name="vpn-client-error-the-connection-was-prevented-because-of-a-policy-configured-on-your-rasvpn-server-error-812"></a>VPN istemci hatası: Bağlantı RAS/VPN sunucunuzda yapılandırılan ilke nedeniyle engellendi. (Hata 812)

### <a name="cause"></a>Nedeni

Azure ağ geçidine RADIUS sunucusuna erişilemiyor veya VPN istemci kimlik doğrulaması için kullanılan bir RADIUS sunucusu ayarları yanlış varsa bu hata oluşur.

### <a name="solution"></a>Çözüm

RADIUS sunucusu doğru şekilde yapılandırıldığından emin olun. Daha fazla bilgi için [tümleştirme RADIUS kimlik doğrulaması ile Azure multi-Factor Authentication sunucusu](../active-directory/authentication/howto-mfaserver-dir-radius.md).

## <a name="error-405-when-you-download-root-certificate-from-vpn-gateway"></a>"Error 405" ne zaman indirdiğiniz sertifika VPN ağ geçidinden

### <a name="cause"></a>Nedeni

Kök sertifika yüklenmemiş. İstemcinin kök sertifika yüklü **güvenilen Sertifikalar** depolayın.

## <a name="vpn-client-error-the-remote-connection-was-not-made-because-the-attempted-vpn-tunnels-failed-error-800"></a>VPN istemci hatası: VPN tünellerinin girişimi başarısız olduğundan uzak bağlantı yapılmadı. (Hata 800) 

### <a name="cause"></a>Nedeni

NIC sürücüsünün güncel değil.

### <a name="solution"></a>Çözüm

NIC sürücüyü güncelleştirin:

1. Tıklayın **Başlat**, türü **cihaz Yöneticisi**ve sonuçlar listesinden seçin. Bir yönetici parolası veya onay istenirse parolayı yazın ya da onay sağlayın.
2. İçinde **ağ bağdaştırıcıları** kategoriler, güncelleştirmek istediğiniz NIC bulun.  
3. Cihaz adına çift tıklayın, seçin **güncelleştirme sürücü**seçin **otomatik olarak güncelleştirilen sürücü yazılım Ara**.
4. Yeni bir sürücü Windows bulamazsa, bir üreticinin Web sitesini bakmayın deneyin ve bunların yönergeleri izleyin.
5. Bilgisayarı yeniden başlatın ve bağlantıyı yeniden deneyin.

## <a name="error-file-download-error-target-uri-is-not-specified"></a>Hata: 'Dosya indirme hatası hedef URI belirtilmedi'

### <a name="cause"></a>Nedeni

Bunun nedeni hatalı bir ağ geçidi tarafından yapılandırılır.

### <a name="solution"></a>Çözüm

Azure VPN ağ geçidi türü VPN olmalıdır ve VPN türünde olmalıdır **RouteBased**.

## <a name="vpn-package-installer-doesnt-complete"></a>VPN paketi yükleyicisini tamamlanmıyor

### <a name="cause"></a>Nedeni

Önceki VPN istemci yüklemeleri tarafından bu soruna neden olabilir. 

### <a name="solution"></a>Çözüm

Eski VPN istemcisi yapılandırma dosyalarını Sil **C:\Users\UserName\AppData\Roaming\Microsoft\Network\Connections\<sanal ağ kimliği >** ve VPN istemci yükleyiciyi yeniden çalıştırın. 

## <a name="the-vpn-client-hibernates-or-sleep-after-some-time"></a>VPN istemcisi, hazırda bekleme veya bir süre sonra uyku

### <a name="solution"></a>Çözüm

Uyku denetleyin ve ayarları VPN istemcisini çalıştıran bilgisayarda hazırda bekleme.
