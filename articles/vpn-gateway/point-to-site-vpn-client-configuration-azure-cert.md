---
title: "Oluşturun ve Azure sertifika kimlik doğrulaması için P2S VPN istemci yapılandırma dosyalarını yükleyin: PowerShell: Azure | Microsoft Docs"
description: "Bu makalede oluşturma ve sertifika kimlik doğrulaması kullanan noktadan siteye bağlantıları için VPN istemcisi yapılandırma dosyalarını yüklemenize yardımcı olur."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/27/2017
ms.author: cherylmc
ms.openlocfilehash: 7fe8d5e473e2c8281b1d6c8d7d5423294c428678
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="create-and-install-vpn-client-configuration-files-for-native-azure-certificate-authentication-p2s-configurations"></a>Oluşturun ve yerel Azure sertifika kimlik doğrulaması P2S yapılandırmaları için VPN istemcisi yapılandırma dosyalarını yükleyin

VPN istemcisi yapılandırma dosyalarını bir zip dosyasında bulunur. Yapılandırma dosyalarını yerel Azure sertifika kimlik doğrulaması kullanmak noktadan siteye bağlantıları üzerinden bir sanal ağa bağlanmak yerel bir Windows veya Mac Ikev2 VPN istemcisi için gerekli ayarları sağlar.

>[!NOTE]
>P2S için IKEv2 şu anda Önizleme sürümündedir.
>

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
  * **Genel**, kendi VPN istemci yapılandırması oluşturmak için kullanılan genel bilgiler içerir. Bu klasör yok sayın. Ağ geçidinde Ikev2 veya SSTP + Ikev2 yapılandırdıysanız genel klasör sağlanır. Yalnızca SSTP yapılandırılmışsa, genel klasör yok.

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

## <a name="installwin"></a>Windows VPN istemcisi yapılandırma paketini yükle

Sürüm için İstemci mimarisi eşleştiği sürece, her Windows istemci bilgisayarda aynı VPN istemcisi yapılandırma paketini kullanabilirsiniz. Desteklenen istemci işletim sistemleri listesi için noktadan siteye bölümüne bakın [VPN Gateway SSS](vpn-gateway-vpn-faq.md#P2S).

Sertifika kimlik doğrulaması için yerel Windows VPN istemcisi yapılandırmak için aşağıdaki adımları kullanın:

1. Windows bilgisayarın mimarisiyle karşılık gelen VPN istemcisi yapılandırma dosyalarını seçin. 64-bit işlemci mimarisi için 'VpnClientSetupAmd64' Yükleyici paketi seçin. 32-bit işlemci mimarisi için 'VpnClientSetupX86' Yükleyici paketi seçin. 
2. Paketi yüklemek için çift tıklatın. Bir SmartScreen açılır penceresi görürseniz **Daha fazla bilgi**’ye ve ardından **Yine de çalıştır**’a tıklayın.
3. İstemci bilgisayarda **Ağ Ayarları**’na gidin ve **VPN** öğesine tıklayın. VPN bağlantısı, bağlandığı sanal ağın adını gösterir. 

## <a name="installmac"></a>VPN istemci yapılandırması üzerinde Mac'ler (OSX)

Azure mobileconfig dosyası yerel Azure sertifika kimlik doğrulaması sağlamaz. Azure'a bağlanır her Mac üzerinde yerel Ikev2 VPN istemcisi el ile yapılandırmanız gerekir. **Genel** klasör yapılandırmak gereken tüm bilgileri içerir. Genel klasör karşıdan yüklemenizi görmüyorsanız, Ikev2 tünel türü olarak seçilmeyen olasıdır. Ikev2 seçildikten sonra genel klasör yeniden almak için zip dosyası oluşturun. Genel klasör aşağıdaki dosyaları içerir:

* **VpnSettings.xml**, sunucu adresi ve tünel türü gibi önemli ayarları içerir. 
* **VpnServerRoot.cer**, P2S bağlantısı Kurulum sırasında Azure VPN ağ geçidi doğrulamak için gerekli kök sertifikası içerir.

Yerel VPN istemcisi Mac sertifika kimlik doğrulamasını yapılandırmak için aşağıdaki adımları kullanın. Azure'a bağlanır her Mac üzerinde aşağıdaki adımları tamamlamanız gerekir:

1. İçeri aktarma **VpnServerRoot** Mac için kök sertifikası Bu dosya, Mac üzerinden kopyalayarak ve çift yapılabilir.  
Tıklatın **Ekle** almak için.

  ![Sertifika Ekle](./media/point-to-site-vpn-client-configuration-azure-cert/addcert.png)
  
    >[!NOTE]
    >Sertifikayı çift görüntülenmeyebilir **Ekle** iletişim ancak sertifika doğru depoya yüklenir. Sertifikaları kategorisi altında oturum açma Anahtarlık sertifika için kontrol edebilirsiniz.
  
2. Açık **ağ** altında iletişim **ağ tercihlerini** tıklatıp **'+'** Azure VNet P2S bağlantısı için yeni bir VPN istemci bağlantı profili oluşturmak için.

  **Arabirimi** 'VPN' bir değerdir ve **VPN türü** değer 'IKEv2' dir. Profil için bir ad belirtin **hizmet adı** alan ve ardından **oluşturma** VPN istemci bağlantı profili oluşturmak için.

  ![Ağ](./media/point-to-site-vpn-client-configuration-azure-cert/network.png)
3. İçinde **genel** klasörü, gelen **VpnSettings.xml** dosya, kopya **VpnServer** etiket değeri. Bu değeri yapıştırın **sunucu adresi** ve **Uzak Kimliği** profilinin alanları.

  ![Sunucu bilgisi](./media/point-to-site-vpn-client-configuration-azure-cert/server.png)
4. Tıklatın **kimlik doğrulama ayarlarını** seçip **sertifika**. 

  ![kimlik doğrulama ayarları](./media/point-to-site-vpn-client-configuration-azure-cert/authsettings.png)
5. Tıklatın **seçin...** kimlik doğrulaması için kullanmak istediğiniz istemci sertifikası seçmek için. Bir istemci sertifikası makinede yüklü olması (bkz. Adım #2 **P2S iş akışı** yukarıdaki bölümde).

  ![sertifika](./media/point-to-site-vpn-client-configuration-azure-cert/certificate.png)
6. **Bir kimlik seçin** aralarından seçim yapabileceğiniz sertifikaların listesini görüntüler. Doğru sertifikayı seçin ve ardından **devam**.

  ![identity](./media/point-to-site-vpn-client-configuration-azure-cert/identity.png)
7. İçinde **yerel kimliği** alanında, sertifika (6. adım) adını belirtin. Bu örnekte, "ikev2Client.com" dir. Ardından **Uygula** değişiklikleri kaydetmek için düğmesi.

  ![Uygula](./media/point-to-site-vpn-client-configuration-azure-cert/applyconnect.png)
8. Üzerinde **ağ** iletişim kutusunda, tıklatın **Uygula** tüm değişiklikleri kaydetmek için. Ardından **Bağlan** Azure VNet P2S bağlantısı başlatmak için.

## <a name="next-steps"></a>Sonraki Adımlar

Makaleye dönün [P2S yapılandırmanızı tamamlamak](vpn-gateway-howto-point-to-site-rm-ps.md).
