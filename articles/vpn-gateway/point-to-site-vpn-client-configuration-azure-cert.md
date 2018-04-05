---
title: 'Oluşturun ve Azure sertifika kimlik doğrulaması için P2S VPN istemci yapılandırma dosyalarını yükleyin: PowerShell: Azure | Microsoft Docs'
description: Oluşturun ve P2S sertifika kimlik doğrulaması için Windows, Linux (strongSwan) ve Mac OS X VPN istemcisi yapılandırma dosyalarını yükleyin.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/02/2018
ms.author: cherylmc
ms.openlocfilehash: 9b9528aba0be8fd46087d97bc294552db608f1c1
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="create-and-install-vpn-client-configuration-files-for-native-azure-certificate-authentication-point-to-site-configurations"></a>Oluşturma ve VPN yükleme istemcisi yapılandırma dosyalarını yerel Azure için sertifika kimlik doğrulaması noktadan siteye yapılandırmaları

VPN istemcisi yapılandırma dosyalarını bir zip dosyasında bulunur. Yapılandırma dosyalarını yerel Azure sertifika kimlik doğrulaması kullanmak noktadan siteye bağlantıları üzerinden bir sanal ağa bağlanmak yerel bir Windows, Mac Ikev2 VPN ya da Linux istemcisi için gerekli ayarları sağlar.

### <a name="workflow"></a>P2S iş akışı

  1. P2S bağlantısı için Azure VPN ağ geçidi ayarlayın.
  2. Kök sertifikayı ve istemci sertifikaları oluşturur. Kök sertifika genel anahtarı Azure'a yükleyin ve istemci cihazlarda istemci sertifikalarını yükleyin. Bağlanan kullanıcıların kimlik doğrulaması için sertifikalar kullanılır.
  3. Seçtiğiniz kimlik doğrulama seçeneği VPN istemci yapılandırmasını almak ve Windows ve Mac cihazları VPN istemcisi ayarlamak için kullanın. Bu, Azure sanal ağlar için noktadan siteye bağlantı üzerinden bağlanmanıza olanak sağlar.

>[!NOTE]
>İstemci yapılandırma dosyalarını, VNet için VPN yapılandırması özgüdür. Tüm değişiklikleri varsa noktadan siteye VPN yapılandırması için VPN istemcisi yapılandırma dosyalarını, kimlik doğrulama türü ve VPN protokol türü gibi oluşturduktan sonra kullanıcı aygıtları için yeni VPN istemcisi yapılandırma dosyalarını oluşturmak emin olun.
>
>

## <a name="generate"></a>VPN istemcisi yapılandırma dosyalarını oluştur

Başlamadan önce tüm bağlanan kullanıcılar kullanıcı cihazda yüklü geçerli bir sertifika olduğundan emin olun. Bir istemci sertifikası yükleme hakkında daha fazla bilgi için bkz: [bir istemci sertifikası yüklemek](point-to-site-how-to-vpn-client-install-azure-cert.md).

PowerShell kullanarak istemci yapılandırma dosyaları oluşturabilir veya Azure portalını kullanarak. Her iki yöntem aynı zip dosyası döndürür. Aşağıdaki klasörlerin görüntülemek için dosyanın sıkıştırmasını açın:

  * **WindowsAmd64** ve **WindowsX86**, sırasıyla Windows 32-bit ve 64-bit yükleyicisi paketleri içerir. **WindowsAmd64** tüm 64-bit Windows istemcileri, yalnızca Amd desteklenen Yükleyici paketi değil.
  * **Genel**, kendi VPN istemci yapılandırması oluşturmak için kullanılan genel bilgiler içerir. Ağ geçidinde Ikev2 veya SSTP + Ikev2 yapılandırdıysanız genel klasör sağlanır. Yalnızca SSTP yapılandırılmışsa, genel klasör yok.

### <a name="zipportal"></a>Azure Portalı'nı kullanarak dosyaları oluştur

1. Azure Portal'da, bağlanmak istediğiniz sanal ağ için sanal ağ geçidi gidin.
2. Sanal ağ geçidi sayfasında, tıklatın **noktadan siteye Yapılandırması**.
3. Noktadan siteye yapılandırması sayfanın en üstünde tıklatın **karşıdan VPN istemcisi**. İstemci için bir kaç dakika oluşturmak için bir yapılandırma paketi alır.
4. Tarayıcınız, bir istemci yapılandırma zip dosyası kullanılabilir olduğunu gösterir. Aynı adı, ağ geçidi olarak adlandırılır. Klasörleri görüntülemek için dosyanın sıkıştırmasını açın.

### <a name="zipps"></a>PowerShell kullanarak dosyaları oluştur

1. Ne zaman oluşturma VPN istemci yapılandırma dosyaları, değeri '-AuthenticationMethod' 'EapTls' değil. Aşağıdaki komutu kullanarak VPN istemcisi yapılandırma dosyalarını oluşturur:

  ```powershell
  $profile=New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapTls"

  $profile.VPNProfileSASUrl
  ```
2. Zip dosyasını karşıdan yüklemek için tarayıcınızı URL'yi kopyalayın, sonra klasörleri görüntülemek için dosyanın sıkıştırmasını açın.

## <a name="installwin"></a>Install - Windows

Sürüm için İstemci mimarisi eşleştiği sürece, her Windows istemci bilgisayarda aynı VPN istemcisi yapılandırma paketini kullanabilirsiniz. Desteklenen istemci işletim sistemleri listesi için noktadan siteye bölümüne bakın [VPN Gateway SSS](vpn-gateway-vpn-faq.md#P2S).

>[!NOTE]
>Bağlanmak istediğiniz Windows istemci bilgisayarda yönetici haklarına sahip olmalıdır.
>
>

Sertifika kimlik doğrulaması için yerel Windows VPN istemcisi yapılandırmak için aşağıdaki adımları kullanın:

1. Windows bilgisayarın mimarisiyle karşılık gelen VPN istemcisi yapılandırma dosyalarını seçin. 64-bit işlemci mimarisi için 'VpnClientSetupAmd64' Yükleyici paketi seçin. 32-bit işlemci mimarisi için 'VpnClientSetupX86' Yükleyici paketi seçin. 
2. Paketi yüklemek için çift tıklatın. Bir SmartScreen açılır penceresi görürseniz **Daha fazla bilgi**’ye ve ardından **Yine de çalıştır**’a tıklayın.
3. İstemci bilgisayarda **Ağ Ayarları**’na gidin ve **VPN** öğesine tıklayın. VPN bağlantısı, bağlandığı sanal ağın adını gösterir. 
4. Bağlanma, doğrulamak, denemeden önce bir istemci sertifikası istemci bilgisayara yüklediniz. Yerel Azure sertifika kimlik doğrulama türü kullanılırken bir istemci sertifikası kimlik doğrulaması için gereklidir. Sertifika oluşturma hakkında daha fazla bilgi için bkz: [sertifikalar oluşturmak](vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert). Bir istemci sertifikası yükleme hakkında daha fazla bilgi için bkz: [bir istemci sertifikası yüklemek](point-to-site-how-to-vpn-client-install-azure-cert.md).

## <a name="installmac"></a>Install - Mac (OS X)

Azure mobileconfig dosyası yerel Azure sertifika kimlik doğrulaması sağlamaz. Azure'a bağlanır her Mac üzerinde yerel Ikev2 VPN istemcisi el ile yapılandırmanız gerekir. **Genel** klasör yapılandırmak gereken tüm bilgileri içerir. Genel klasör karşıdan yüklemenizi görmüyorsanız, Ikev2 tünel türü olarak seçilmeyen olasıdır. Ikev2 seçildikten sonra genel klasör yeniden almak için zip dosyası oluşturun. Genel klasör aşağıdaki dosyaları içerir:

* **VpnSettings.xml**, sunucu adresi ve tünel türü gibi önemli ayarları içerir. 
* **VpnServerRoot.cer**, P2S bağlantısı Kurulum sırasında Azure VPN ağ geçidi doğrulamak için gerekli kök sertifikası içerir.

Yerel VPN istemcisi Mac sertifika kimlik doğrulamasını yapılandırmak için aşağıdaki adımları kullanın. Azure'a bağlanır her Mac üzerinde aşağıdaki adımları tamamlamanız gerekir:

1. İçeri aktarma **VpnServerRoot** Mac için kök sertifikası Bu dosya, Mac üzerinden kopyalayarak ve çift yapılabilir.  
Tıklatın **Ekle** almak için.

  ![Sertifika Ekle](./media/point-to-site-vpn-client-configuration-azure-cert/addcert.png)
  
    >[!NOTE]
    >Sertifikayı çift görüntülenmeyebilir **Ekle** iletişim ancak sertifika doğru depoya yüklenir. Sertifikaları kategorisi altında oturum açma Anahtarlık sertifika için kontrol edebilirsiniz.
  
2. P2S ayarlarını yapılandırdığınız zaman Azure'a karşıya kök sertifikası tarafından verilmiş bir istemci sertifikası yüklü olduğunu doğrulayın. Bu önceki adımda yüklediğiniz VPNServerRoot farklıdır. İstemci sertifikası kimlik doğrulaması için kullanılır ve gereklidir. Sertifika oluşturma hakkında daha fazla bilgi için bkz: [sertifikalar oluşturmak](vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert). Bir istemci sertifikası yükleme hakkında daha fazla bilgi için bkz: [bir istemci sertifikası yüklemek](point-to-site-how-to-vpn-client-install-azure-cert.md).
3. Açık **ağ** altında iletişim **ağ tercihlerini** tıklatıp **'+'** Azure VNet P2S bağlantısı için yeni bir VPN istemci bağlantı profili oluşturmak için.

  **Arabirimi** 'VPN' bir değerdir ve **VPN türü** değer 'IKEv2' dir. Profil için bir ad belirtin **hizmet adı** alan ve ardından **oluşturma** VPN istemci bağlantı profili oluşturmak için.

  ![Ağ](./media/point-to-site-vpn-client-configuration-azure-cert/network.png)
4. İçinde **genel** klasörü, gelen **VpnSettings.xml** dosya, kopya **VpnServer** etiket değeri. Bu değeri yapıştırın **sunucu adresi** ve **Uzak Kimliği** profilinin alanları.

  ![Sunucu bilgisi](./media/point-to-site-vpn-client-configuration-azure-cert/server.png)
5. Tıklatın **kimlik doğrulama ayarlarını** seçip **sertifika**. 

  ![kimlik doğrulama ayarları](./media/point-to-site-vpn-client-configuration-azure-cert/authsettings.png)
6. Tıklatın **seçin...** kimlik doğrulaması için kullanmak istediğiniz istemci sertifikası seçmek için. Bu adım 2'de yüklü olan sertifikadır.

  ![sertifika](./media/point-to-site-vpn-client-configuration-azure-cert/certificate.png)
7. **Bir kimlik seçin** aralarından seçim yapabileceğiniz sertifikaların listesini görüntüler. Doğru sertifikayı seçin ve ardından **devam**.

  ![identity](./media/point-to-site-vpn-client-configuration-azure-cert/identity.png)
8. İçinde **yerel kimliği** alanında, sertifika (6. adım) adını belirtin. Bu örnekte, "ikev2Client.com" dir. Ardından **Uygula** değişiklikleri kaydetmek için düğmesi.

  ![Uygula](./media/point-to-site-vpn-client-configuration-azure-cert/applyconnect.png)
9. Üzerinde **ağ** iletişim kutusunda, tıklatın **Uygula** tüm değişiklikleri kaydetmek için. Ardından **Bağlan** Azure VNet P2S bağlantısı başlatmak için.

## <a name="installlinux"></a>Install - Linux (strongSwan)

### <a name="extract-the-key-and-certificate"></a>Anahtar ve sertifika Ayıkla

StrongSwan için anahtar ve sertifika istemci sertifikası (.pfx dosyası) ayıklayın ve bunları tek tek .pem dosyaları kaydetmek gerekir.
Aşağıdaki adımları izleyin:

1. Gelen OpenSSL yükleyip [OpenSSL](https://www.openssl.org/source/).
2. Bir komut satırı penceresi açın ve yüklediğiniz OpenSSL, örneğin, dizin değiştirmek ' c:\OpenSLL-Win64\bin\'.
3. Özel anahtarı ayıklayın ve istemci sertifikanızı 'privatekey.pem' adlı yeni bir dosyaya kaydetmek için aşağıdaki komutu çalıştırın:

  ```
  C:\ OpenSLL-Win64\bin> openssl pkcs12 -in clientcert.pfx -nocerts -out privatekey.pem -nodes
  ```
4.  Artık genel cert ayıklayın ve yeni bir dosyaya kaydetmek için aşağıdaki komutu çalıştırın:

  ```
  C:\ OpenSLL-Win64\bin> openssl pkcs12 -in clientcert.pfx -nokeys -out publiccert.pem -nodes
  ```

### <a name="install"></a>Yükleme

Aşağıdaki yönergeler, Ubuntu 17.0.4 üzerinde strongSwan 5.5.1 üzerinden oluşturuldu. Ubuntu 16.0.10 strongSwan GUI desteklemez. Ubuntu 16.0.10 kullanmak istiyorsanız, komut satırını kullanmanız gerekir. Aşağıdaki örnekler, Linux ve strongSwan sürümüne bağlı olarak gördüğü ekranlar eşleşmeyebilir.

1. Açık **Terminal** yüklemek için **strongSwan** ve örnekte komutunu çalıştırarak, Ağ Yöneticisi. İlgili bir hata alırsanız, *libcharon ek eklentileri*, 'strongswan-plugin-eap-mschapv2 ile' değiştirin.

  ```
  sudo apt-get install strongswan libcharon-extra-plugins moreutils iptables-persistent network-manager-strongswan
  ```
2. Seçin **Ağ Yöneticisi** simgesi (yukarı-oku/aşağı ok), ardından **düzenleme bağlantıları**.

  ![Bağlantıları Düzenle](./media/point-to-site-vpn-client-configuration-azure-cert/editconnections.png)
3. Tıklatın **Ekle** düğmesi yeni bir bağlantı oluşturun.

  ![Bağlantı Ekle](./media/point-to-site-vpn-client-configuration-azure-cert/addconnection.png)
4. Seçin **IPSec/Ikev2 (strongswan)** açılan menüsünden ve ardından **oluşturma**. Bu adımda, bağlantınızı yeniden adlandırabilirsiniz.

  ![bir bağlantı türü seçin](./media/point-to-site-vpn-client-configuration-azure-cert/choosetype.png)
5. Açık **VpnSettings.xml** dosya **genel** indirilen istemci yapılandırma dosyalarında bulunan klasör. Adlı Etiket Bul **VpnServer** ve 'azuregateway' ile başlayan ve ile biten adını kopyalama '. cloudapp.net'.

  ![kopya adı](./media/point-to-site-vpn-client-configuration-azure-cert/vpnserver.png)
6. Bu ad alanının içine yapıştırma **adresi** yeni VPN bağlantınızı alanını **ağ geçidi** bölümü. Ardından, sonunda klasör simgesini seçin **sertifika** alan, Gözat **genel** klasörü ve select **VpnServerRoot** dosya.
7. İçinde **istemci** bağlantı bölümü için **kimlik doğrulaması**seçin **sertifika/özel anahtar**. İçin **sertifika** ve **özel anahtarı**, sertifika ve daha önce oluşturulan özel anahtarı seçin. İçinde **seçenekleri**seçin **iç IP adresi isteği**. Ardından **Ekle**.

  ![İstek bir iç IP adresi](./media/point-to-site-vpn-client-configuration-azure-cert/inneripreq.png)
8. Tıklatın **Ağ Yöneticisi** simgesi (yukarı-oku/aşağı ok) ve üzerine getirin **VPN bağlantıları**. Oluşturduğunuz VPN bağlantısını görür. Bağlantı başlatmak için tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

Makaleye dönün [P2S yapılandırmanızı tamamlamak](vpn-gateway-howto-point-to-site-rm-ps.md).

P2S sorun giderme bilgileri için [sorun giderme Azure noktadan siteye bağlantıları](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md) ve [sorun giderme VPN bağlantıları Mac OS X VPN istemcilerinden gelen](vpn-gateway-troubleshoot-point-to-site-osx-ikev2.md).