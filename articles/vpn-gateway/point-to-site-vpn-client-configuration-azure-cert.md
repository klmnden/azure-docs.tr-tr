---
title: 'Oluşturma ve Azure sertifika doğrulaması P2S VPN istemci yapılandırma dosyalarını yükleyin: Azure'
description: Oluşturun ve P2S sertifika kimlik doğrulaması için Windows, Linux, (strongSwan) Linux ve Mac OS X VPN istemcisi yapılandırma dosyalarını yükleyin.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 03/20/2019
ms.author: cherylmc
ms.openlocfilehash: b590dabbe4b2c6526f2c602aeed64667348eefa9
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59525176"
---
# <a name="create-and-install-vpn-client-configuration-files-for-native-azure-certificate-authentication-p2s-configurations"></a>Oluşturma ve yerel Azure sertifika doğrulaması P2S yapılandırmaları için VPN istemcisi yapılandırma dosyalarını yükleme

VPN istemcisi yapılandırma dosyalarını bir zip dosyası içinde yer alır. Yapılandırma dosyaları, yerel Azure sertifika kimlik doğrulaması kullanan noktadan siteye bağlantılar üzerinden bir sanal ağa bağlanmak yerel bir Windows, Mac Ikev2 VPN ya da Linux istemcileri için gerekli olan ayarları sağlayın.

İstemci yapılandırma dosyalarını, sanal ağ için VPN yapılandırması özgüdür. Herhangi bir değişiklik varsa noktadan siteye VPN yapılandırması için VPN Protokolü türü veya kimlik doğrulama türü gibi bir VPN istemcisi yapılandırma dosyalarını oluşturduktan sonra kullanıcı cihazları için yeni VPN istemcisi yapılandırma dosyalarını oluşturma emin olun. 

* Noktadan Siteye bağlantılar hakkında daha fazla bilgi edinmek için bkz. [Noktadan Siteye VPN hakkında](point-to-site-about.md).
* OpenVPN yönergeler için bkz: [yapılandırma OpenVPN P2S için](vpn-gateway-howto-openvpn.md) ve [yapılandırma OpenVPN istemciler](vpn-gateway-howto-openvpn-clients.md).

>[!IMPORTANT]
>[!INCLUDE [TLS](../../includes/vpn-gateway-tls-change.md)]
>

## <a name="generate"></a>VPN istemcisi yapılandırma dosyalarını oluştur

Başlamadan önce tüm bağlanan kullanıcıların kullanıcı cihazında yüklü geçerli bir sertifika olduğundan emin olun. Bir istemci sertifikası yükleme hakkında daha fazla bilgi için bkz. [bir istemci sertifikasını yükleme](point-to-site-how-to-vpn-client-install-azure-cert.md).

PowerShell'i kullanarak istemci yapılandırma dosyaları oluşturabilir veya Azure portalını kullanarak. Her iki yöntem aynı zip dosyasını döndürür. Aşağıdaki klasörlerin görüntülemek için dosyanın sıkıştırmasını açın:

  * **WindowsAmd64** ve **WindowsX86**, sırasıyla Windows 32-bit ve 64-bit yükleyici paketleri içerir. **WindowsAmd64** Yükleyici paketi, tüm desteklenen 64 bit Windows istemcileri, yalnızca Amd için.
  * **Genel**, kendi VPN istemci yapılandırması oluşturmak için kullanılan genel bilgiler içerir. Genel klasörü, Ikev2 veya SSTP + Ikev2 ağ geçidi yapılandırılmışsa sağlanır. Yalnızca SSTP yapılandırılmışsa, genel klasörü mevcut değil.

### <a name="zipportal"></a>Azure portalını kullanarak dosyaları oluştur

1. Azure portalında, bağlanmak istediğiniz sanal ağı için sanal ağ geçidine gidin.
2. Sanal ağ geçidi sayfasında tıklayın **noktadan siteye yapılandırma**.
3. Noktadan siteye yapılandırma sayfanın üst kısmında tıklayın **VPN istemcisini indir**. İşlem birkaç dakika için istemci yapılandırma paketini oluşturmak için alır.
4. Tarayıcınız, bir istemci yapılandırma zip dosyası kullanılabilir olduğunu gösterir. Bu, aynı adı taşıyan ağ geçidi olarak adlandırılır. Klasörleri görüntülemek için dosyanın sıkıştırmasını açın.

### <a name="zipps"></a>PowerShell kullanarak dosyaları oluştur

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. Ne zaman oluşturma VPN istemci yapılandırması dosyaları, değeri '-AuthenticationMethod' olan 'EapTls'. Aşağıdaki komutu kullanarak VPN istemcisi yapılandırma dosyalarını oluştur:

   ```azurepowershell-interactive
   $profile=New-AzVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapTls"

   $profile.VPNProfileSASUrl
   ```
2. Zip dosyasını indirmek için tarayıcınızı URL'yi kopyalayın, sonra klasörleri görüntülemek için dosyanın sıkıştırmasını açın.

## <a name="installwin"></a>Windows

İçin istemci sürümünün eşleşmesi sürece her Windows istemci bilgisayarda aynı VPN istemcisi yapılandırma paketini kullanabilirsiniz. Desteklenen istemci işletim sistemlerinin listesi için noktadan siteye bölümüne bakın. [VPN Gateway SSS](vpn-gateway-vpn-faq.md#P2S).

>[!NOTE]
>Bağlanmak istediğiniz Windows istemci bilgisayarda yönetici haklarına sahip olmalıdır.
>
>

Sertifika kimlik doğrulaması için yerel Windows VPN istemcisini yapılandırmak için aşağıdaki adımları kullanın:

1. Windows bilgisayarın mimarisine karşılık gelen VPN istemcisi yapılandırma dosyalarını seçin. 64 bit işlemci mimarisi için 'VpnClientSetupAmd64' yükleyici paketini seçin. 32 bit işlemci mimarisi için 'VpnClientSetupX86' yükleyici paketini seçin. 
2. Yüklemek için pakete çift tıklayın. Bir SmartScreen açılır penceresi görürseniz **Daha fazla bilgi**’ye ve ardından **Yine de çalıştır**’a tıklayın.
3. İstemci bilgisayarda **Ağ Ayarları**’na gidin ve **VPN** öğesine tıklayın. VPN bağlantısı, bağlandığı sanal ağın adını gösterir. 
4. Bağlanmayı denemeden önce, istemci bilgisayara bir istemci sertifikası yüklediğinizi doğrulayın. Yerel Azure sertifika kimlik doğrulaması türü kullanılırken kimlik doğrulaması için bir istemci sertifikası gereklidir. Sertifikaları oluşturma hakkında daha fazla bilgi için bkz. [sertifikaları oluşturmak](vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert). Bir istemci sertifikası yükleme hakkında daha fazla bilgi için bkz: [bir istemci sertifikasını yükleme](point-to-site-how-to-vpn-client-install-azure-cert.md).

## <a name="installmac"></a>Mac (OS X)

 Yerel IKEv2 VPN istemcisini Azure’a bağlanacak her Mac cihazda el ile yapılandırmanız gerekir. Azure, yerel Azure sertifika kimlik doğrulaması için mobileconfig dosyası sağlamaz. **Genel** yapılandırması için gereken bilgilerin tümünü içerir. İndirdiğiniz pakette Genel klasörünü görmüyorsanız, tünel türü olarak IKEv2 seçilmemiş olabilir. Ikev2 VPN ağ geçidi temel SKU desteklemediğini unutmayın. IKEv2 seçildikten sonra zip dosyasını tekrar oluşturarak Genel klasörünü alın.<br>Genel klasörü aşağıdaki dosyaları içerir:

* **VpnSettings.xml**, sunucu adresi ve tünel türü gibi önemli ayarları içerir. 
* **VpnServerRoot.cer**, P2S bağlantısı Kurulum sırasında Azure VPN Gateway doğrulamak için gerekli kök sertifika içerir.

Yerel VPN istemcisini Mac üzerinde sertifika kimlik doğrulaması için yapılandırmak için aşağıdaki adımları kullanın. Azure'a bağlanan her Mac üzerinde aşağıdaki adımları tamamlamanız gerekir:

1. İçeri aktarma **VpnServerRoot** Mac'iniz için kök sertifika Bu dosya üzerinde Mac'inize kopyalayarak ve çift yapılabilir. Tıklayın **Ekle** içeri aktarmak için.

   ![Sertifika Ekle](./media/point-to-site-vpn-client-configuration-azure-cert/addcert.png)
  
    >[!NOTE]
    >Sertifikayı çift görüntülenmeyebilir **Ekle** iletişim, ancak sertifika doğru depoya yüklenir. Sertifikaları kategorisi altında oturum açma Anahtarlıkta sertifikayı denetleyebilirsiniz.
    >
  
2. P2S ayarlarını yapılandırdığınız zaman Azure'a yüklediğiniz kök sertifika tarafından verilmiş bir istemci sertifikası yüklü olduğunu doğrulayın. Bu, önceki adımda yüklediğiniz VPNServerRoot farklıdır. İstemci sertifikası kimlik doğrulaması için kullanılır ve gereklidir. Sertifikaları oluşturma hakkında daha fazla bilgi için bkz. [sertifikaları oluşturmak](vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert). Bir istemci sertifikası yükleme hakkında daha fazla bilgi için bkz: [bir istemci sertifikasını yükleme](point-to-site-how-to-vpn-client-install-azure-cert.md).
3. Açık **ağ** iletişim altında **ağ tercihlerini** tıklatıp **'+'** P2S bağlantısı Azure sanal ağ için yeni bir VPN istemci bağlantı profili oluşturmak için.

   **Arabirimi** 'VPN' değeridir ve **VPN türü** 'IKEv2' değeridir. Profili için bir ad belirtin **hizmet adı** alan ve ardından tıklayın **Oluştur** VPN istemci bağlantı profili oluşturmak için.

   ![Ağ](./media/point-to-site-vpn-client-configuration-azure-cert/network.png)
4. İçinde **genel** klasörü, gelen **VpnSettings.xml** dosyası, kopya **VpnServer** etiket değeri. Bu değeri yapıştırın **sunucu adresi** ve **Uzak Kimliği** profilinin alanları.

   ![Sunucu bilgisi](./media/point-to-site-vpn-client-configuration-azure-cert/server.png)
5. Tıklayın **kimlik doğrulama ayarları** seçip **sertifika**. 

   ![kimlik doğrulama ayarları](./media/point-to-site-vpn-client-configuration-azure-cert/authsettings.png)
6. Tıklayın **seçin...** kimlik doğrulaması için kullanmak istediğiniz istemci sertifikasını seçmek için. Bu adım 2'de yüklü olan sertifikadır.

   ![sertifika](./media/point-to-site-vpn-client-configuration-azure-cert/certificate.png)
7. **Bir kimlik seçin** sertifikaları için öncelikle repairshop listesini görüntüler. Sunucunun doğru sertifikayı seçin ve sonra tıklatın **devam**.

   ![identity](./media/point-to-site-vpn-client-configuration-azure-cert/identity.png)
8. İçinde **yerel kimliği** alanında, (6. adım)'deki Sertifika adını belirtin. Bu örnekte, "ikev2Client.com" dir. ' A tıklayarak **Uygula** değişiklikleri kaydetmek için düğme.

   ![uygula](./media/point-to-site-vpn-client-configuration-azure-cert/applyconnect.png)
9. Üzerinde **ağ** iletişim kutusunda, tıklayın **Uygula** tüm değişiklikleri kaydedin. ' A tıklayarak **Connect** Azure vnet'e P2S bağlantısı başlatılamadı.

## <a name="linuxgui"></a>Linux (strongSwan GUI)

### <a name="extract-the-key-and-certificate"></a>Anahtarı ve sertifikayı ayıklayın

StrongSwan için istemci sertifikası (.pfx dosyası) anahtarı ve sertifikayı ayıklayın ve bunları tek tek .pem dosyasına kaydetmek gerekir.
Aşağıdaki adımları izleyin:

1. OpenSSL yükleyip [OpenSSL](https://www.openssl.org/source/).
2. Bir komut satırı penceresi açın ve yüklediğiniz OpenSSL, örneğin dizinine ' c:\OpenSLL-Win64\bin\'.
3. Özel anahtarınızı ayıklamanız ve istemci sertifikanızın 'privatekey.pem' adlı yeni bir dosyaya kaydetmek için aşağıdaki komutu çalıştırın:

   ```
   C:\ OpenSLL-Win64\bin> openssl pkcs12 -in clientcert.pfx -nocerts -out privatekey.pem -nodes
   ```
4. Artık ortak sertifikayı ayıklayın ve yeni bir dosyaya kaydetmek için aşağıdaki komutu çalıştırın:

   ```
   C:\ OpenSLL-Win64\bin> openssl pkcs12 -in clientcert.pfx -nokeys -out publiccert.pem -nodes
   ```

### <a name="install"></a>Yükleme ve yapılandırma

Aşağıdaki yönergeler, ubuntu'da 17.0.4 strongSwan 5.5.1 aracılığıyla oluşturuldu. Ubuntu 16.0.10 strongSwan GUI desteklemez. Ubuntu 16.0.10 kullanmak istiyorsanız, kullanması gerekir [komut satırı](#linuxinstallcli). Aşağıdaki örnekler, Linux ve strongSwan sürümünüze bağlı olarak gördüğü ekranlar eşleşmeyebilir.

1. Açık **Terminal** yüklemek için **strongSwan** ve örnekte komutu çalıştırarak, Ağ Yöneticisi. İlgili bir hata alırsanız *libcharon ek eklentiler*, 'strongswan-plugin-eap-mschapv2 ile' değiştirin.

   ```
   sudo apt-get install strongswan libcharon-extra-plugins moreutils iptables-persistent network-manager-strongswan
   ```
2. Seçin **Ağ Yöneticisi** simgesi (yukarı-oku/aşağı oka), ardından **düzenleme bağlantıları**.

   ![Bağlantıları Düzenle](./media/point-to-site-vpn-client-configuration-azure-cert/editconnections.png)
3. Tıklayın **Ekle** yeni bir bağlantı oluşturmak için.

   ![Bağlantı Ekle](./media/point-to-site-vpn-client-configuration-azure-cert/addconnection.png)
4. Seçin **IPSec/Ikev2 (strongswan)** açılan menüsünü ve ardından **Oluştur**. Bu adımda, bağlantınızı yeniden adlandırabilirsiniz.

   ![bir bağlantı türü seçin](./media/point-to-site-vpn-client-configuration-azure-cert/choosetype.png)
5. Açık **VpnSettings.xml** dosya **genel** indirilen istemci yapılandırma dosyalarında bulunan klasör. Adlı Etiket Bul **VpnServer** ve adını 'azuregateway' ile başlayan ve ile biten kopyalama '. cloudapp.net'.

   ![adı Kopyala](./media/point-to-site-vpn-client-configuration-azure-cert/vpnserver.png)
6. Bu ad alanının içine yapıştırın **adresi** yeni VPN bağlantınızın alan **ağ geçidi** bölümü. Ardından, sonunda, klasör simgesini seçerek **sertifika** göz atın, alan **genel** klasörünü açın ve seçin **VpnServerRoot** dosya.
7. İçinde **istemci** bağlantı bölümünü için **kimlik doğrulaması**seçin **sertifika/özel anahtar**. İçin **sertifika** ve **özel anahtarı**, sertifika ve daha önce oluşturulan özel anahtarı seçin. İçinde **seçenekleri**seçin **iç IP adresi isteme**. ' A tıklayarak **Ekle**.

   ![İstek bir iç IP adresi](./media/point-to-site-vpn-client-configuration-azure-cert/inneripreq.png)
8. Tıklayın **Ağ Yöneticisi** simgesi (yukarı-oku/aşağı ok) ve üzerine geldiğinizde **VPN bağlantıları**. Oluşturduğunuz VPN bağlantısını görürsünüz. Bağlantıyı başlatmak için tıklayın.

## <a name="linuxinstallcli"></a>Linux (strongSwan CLI)

### <a name="install-strongswan"></a>StrongSwan yükleyin

Aşağıdaki CLI komutları kullanın veya strongSwan adımlarda [GUI](#install) strongSwan yüklemek için.

1. `apt-get install strongswan-ikev2 strongswan-plugin-eap-tls`
2. `apt-get install libstrongswan-standard-plugins`

### <a name="install-and-configure"></a>Yükleme ve yapılandırma

1. Bir VPNClient paketi, Azure Portalı'ndan indirin.
2. Dosyayı ayıklayın.
3. Gelen **genel** klasör kopyalayın veya VpnServerRoot.cer /etc/ipsec.d/cacerts için taşıyın.
4. Kopyalayın veya /etc/ipsec.d/private/ için cp client.p12 taşıyın. Azure VPN ağ geçidi için istemci sertifikası dosyasıdır.
5. Açık VpnSettings.xml dosya ve kopyalama `<VpnServer>` değeri. Bu değer sonraki adımda kullanır.
6. Aşağıdaki örnekte gösterilen değerleri ayarlayın ve ardından örnek /etc/ipsec.conf yapılandırmasına ekleyin.
  
   ```
   conn azure
   keyexchange=ikev2
   type=tunnel
   leftfirewall=yes
   left=%any
   leftauth=eap-tls
   leftid=%client # use the DNS alternative name prefixed with the %
   right= Enter the VPN Server value here# Azure VPN gateway address
   rightid=% # Enter the VPN Server value here# Azure VPN gateway FQDN with %
   rightsubnet=0.0.0.0/0
   leftsourceip=%config
   auto=add
   ```
6. Ekleyin */etc/ipsec.secrets*.

   ```
   : P12 client.p12 'password' # key filename inside /etc/ipsec.d/private directory
   ```

7. Aşağıdaki komutları çalıştırın:

   ```
   # ipsec restart
   # ipsec up azure
   ```

## <a name="next-steps"></a>Sonraki adımlar

Makaleye dönün [P2S yapılandırmanızı tamamlamak](vpn-gateway-howto-point-to-site-rm-ps.md).

P2S bağlantılarının sorunlarını gidermek için aşağıdaki makalelere bakın:

  * [Azure noktadan siteye bağlantı sorunlarını giderme](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md)
  * [Mac OS X VPN istemcisinden VPN bağlantı sorunlarını giderme](vpn-gateway-troubleshoot-point-to-site-osx-ikev2.md)
