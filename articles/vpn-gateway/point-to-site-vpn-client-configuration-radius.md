---
title: 'Oluşturma ve RADIUS P2S bağlantıları için VPN istemcisi yapılandırma dosyalarını yükleyin: PowerShell: Azure | Microsoft Docs'
description: Windows, Mac OS X ve Linux VPN istemcisi RADIUS kimlik doğrulaması kullanan bağlantılar için yapılandırma dosyalarını oluşturun.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 02/27/2019
ms.author: cherylmc
ms.openlocfilehash: 34d8eb976a2a1e173f234be214799832dae7e9ca
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66115074"
---
# <a name="create-and-install-vpn-client-configuration-files-for-p2s-radius-authentication"></a>P2S RADIUS kimlik doğrulaması için VPN istemcisi yapılandırma dosyalarını yükleme ve oluşturma

Noktadan siteye (P2S) bir sanal ağa bağlanmak için bağlantı kurma şeklinizi istemci aygıtı'nı yapılandırmanız gerekir. Windows, Mac OS X ve Linux istemci cihazlardan P2S VPN bağlantıları oluşturabilirsiniz. 

RADIUS kimlik doğrulamasını kullanırken, birden çok kimlik doğrulama seçeneği vardır: kullanıcı adı/parola kimlik doğrulaması, sertifika kimlik doğrulaması ve diğer kimlik doğrulama türleri. VPN istemci yapılandırması, her kimlik doğrulama türü için farklıdır. VPN istemcisi yapılandırmak için gerekli ayarları içeren bir istemci yapılandırma dosyalarını kullanın. Bu makalede oluşturmak ve kullanmak istediğiniz RADIUS kimlik doğrulaması türü için VPN istemci yapılandırması yüklemenize yardımcı olur.

>[!IMPORTANT]
>[!INCLUDE [TLS](../../includes/vpn-gateway-tls-change.md)]
>

P2S RADIUS kimlik doğrulaması için yapılandırma iş akışı aşağıdaki gibidir:

1. [P2S bağlantı Azure VPN ağ geçidi ayarlama](point-to-site-how-to-radius-ps.md).
2. [RADIUS sunucunuzun kimlik doğrulaması için ayarlama](point-to-site-how-to-radius-ps.md#radius). 
3. **VPN istemci ayarlamak için kullanın ve elde ettiğiniz kimlik doğrulama seçeneği için VPN istemci yapılandırması** (Bu makale).
4. [P2S yapılandırmayı tamamlamak ve connect](point-to-site-how-to-radius-ps.md).

>[!IMPORTANT]
>Herhangi bir değişiklik varsa noktadan siteye VPN yapılandırması için VPN Protokolü türü veya kimlik doğrulama türü gibi bir VPN istemci yapılandırma profili oluşturduktan sonra oluşturun ve yeni bir VPN istemci yapılandırması, kullanıcıların cihazlarında yüklemeniz gerekir.
>
>

Bu makaledeki bölümleri kullanmak için ilk kimlik doğrulaması türünü kullanmak istediğinize karar verin: kullanıcı adı/parola, sertifika veya diğer kimlik doğrulama türleri. Her bölüm, Windows, Mac OS X ve Linux (sınırlı adımları şu anda kullanılabilir) için adım vardır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="adeap"></a>Kullanıcı adı/parola kimlik doğrulaması

Kullanıcı adı/parola kimlik doğrulaması, Active Directory kullanabilir veya Active Directory kullanmak için yapılandırabilirsiniz. İle iki senaryoda da bağlanan tüm kullanıcılara RADIUS kimlik doğrulaması kullanıcı adı/parola kimlik bilgilerine sahip olduğunuzdan emin olun.

Kullanıcı adı/parola kimlik doğrulamasını yapılandırdığınızda, yalnızca EAP-MSCHAPv2 kullanıcı adı/parola kimlik doğrulama protokolü için bir yapılandırma oluşturabilirsiniz. Komutlarda `-AuthenticationMethod` olduğu `EapMSChapv2`.

### <a name="usernamefiles"></a> 1. VPN istemcisi yapılandırma dosyalarını oluştur

VPN istemcisi yapılandırma dosyalarını, Azure portalını kullanarak ya da Azure PowerShell kullanarak oluşturabilirsiniz.

#### <a name="azure-portal"></a>Azure portal

1. Sanal ağ geçidine gidin.
2. Tıklayın **noktadan siteye yapılandırma**.
3. Tıklayın **VPN istemcisini indir**.
4. İstemci seçin ve istenen tüm bilgileri doldurun.
5. Tıklayın **indirme** .zip dosyasını oluşturmak için.
6. .Zip dosyası, genellikle, indirmeler klasörünüze indirir.

#### <a name="azure-powershell"></a>Azure PowerShell

Kullanıcı adı/parola kimlik doğrulaması ile kullanılacak VPN istemcisi yapılandırma dosyalarını oluşturur. VPN istemcisi yapılandırma dosyalarını, aşağıdaki komutu kullanarak oluşturabilirsiniz:

```azurepowershell-interactive
New-AzVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapMSChapv2"
```
 
Komut çalıştıran bir bağlantıyı döndürür. İndirmek için bir web tarayıcısı bağlantısını kopyalayıp **VpnClientConfiguration.zip**. Aşağıdaki klasörlerin görüntülemek için dosyanın sıkıştırmasını açın: 
 
* **WindowsAmd64** ve **WindowsX86**: Bu klasörler sırasıyla Windows 64-bit ve 32-bit yükleyici paketleri içerir. 
* **Genel**: Bu klasör, kendi VPN istemci yapılandırması oluşturmak için kullandığınız genel bilgiler içerir. Bu klasör için kullanıcı adı/parola kimlik doğrulaması yapılandırmalarıyla gerekmez.
* **Mac**: Sanal ağ geçidi oluşturduğunuzda, Ikev2 yapılandırdıysanız, adlı bir klasör gördüğünüz **Mac** içeren bir **mobileconfig** dosya. Bu dosya, Mac istemcileri yapılandırmak için kullanın.

İstemci yapılandırması dosyaları zaten oluşturduysanız, bunları kullanarak alabileceğiniz `Get-AzVpnClientConfiguration` cmdlet'i. Ancak örneğin VPN protokol türü veya kimlik doğrulama türü, P2S VPN yapılandırmanıza, herhangi bir değişiklik yaparsanız yapılandırmasını otomatik olarak güncelleştirilmez. Çalıştırmalısınız `New-AzVpnClientConfiguration` yeni bir yapılandırma yükleme oluşturmak için cmdlet'i.

Daha önce oluşturulan istemci yapılandırma dosyalarını almak için aşağıdaki komutu kullanın:

```azurepowershell-interactive
Get-AzVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW"
```

### <a name="setupusername"></a> 2. VPN istemcisi yapılandırma

Aşağıdaki VPN istemcileri yapılandırabilirsiniz:

* [Windows](#adwincli)
* [Mac (OS X)](#admaccli)
* [StrongSwan kullanarak Linux](#adlinuxcli)
 
#### <a name="adwincli"></a>Windows VPN istemcisi Kurulumu

İçin istemci sürümünün eşleşmesi sürece her Windows istemci bilgisayarda aynı VPN istemcisi yapılandırma paketini kullanabilirsiniz. Desteklenen istemci işletim sistemlerinin listesi için bkz: [SSS](vpn-gateway-vpn-faq.md#P2S).

Sertifika kimlik doğrulaması için yerel Windows VPN istemcisini yapılandırmak için aşağıdaki adımları kullanın:

1. Windows bilgisayarın mimarisine karşılık gelen VPN istemcisi yapılandırma dosyalarını seçin. Bir 64-bit işlemci mimarisi için seçin **VpnClientSetupAmd64** Yükleyici paketi. Bir 32-bit işlemci mimarisi için seçin **VpnClientSetupX86** Yükleyici paketi. 
2. Paketi yüklemek için çift tıklayın. Bir SmartScreen açılır pencere görürseniz seçin **daha fazla bilgi** > **yine de Çalıştır**.
3. İstemci bilgisayarda göz atın **ağ ayarlarını** seçip **VPN**. VPN bağlantısı, bağlandığı sanal ağın adını gösterir. 

#### <a name="admaccli"></a>Mac (OS X) VPN istemcisi Kurulumu

1. Seçin **VpnClientSetup mobileconfig** dosya ve kullanıcılardan her biri için gönderin. E-posta veya başka bir yöntemi kullanabilirsiniz.

2. Bulun **mobileconfig** Mac'te dosyası

   ![Mobileconfig dosyasının konumu](./media/point-to-site-vpn-client-configuration-radius/admobileconfigfile.png)

3. İsteğe bağlı adım - özel bir DNS belirtmek istiyorsanız aşağıdaki satırları ekleyin **mobileconfig** dosyası:

   ```xml
    <key>DNS</key>
    <dict>
      <key>ServerAddresses</key>
        <array>
            <string>10.0.0.132</string>
        <array>
      <key>SupplementalMatchDomains</key>
        <array>
            <string>TestDomain.com</string>
        </array>
    </dict> 
   ```
4. Profil yükleyin ve seçmek için çift **devam**. Profil adı, sanal ağ adı ile aynıdır.

   ![Yükleme iletisi](./media/point-to-site-vpn-client-configuration-radius/adinstall.png)
5. Seçin **devam** güven profili gönderen ve yüklemeye devam etmek için.

   ![Onay iletisi](./media/point-to-site-vpn-client-configuration-radius/adcontinue.png)
6. Profili yükleme sırasında kullanıcı adı ve VPN kimlik doğrulaması için parola belirtmek için seçeneğiniz vardır. Bu bilgileri girmeniz için zorunlu değildir. Bunu yaparsanız, bilgileri kaydedilir ve bir bağlantı başlatmak, otomatik olarak kullanılır. Seçin **yükleme** devam etmek için.

   ![VPN için kullanıcı adı ve parola kutuları](./media/point-to-site-vpn-client-configuration-radius/adsettings.png)
7. Profil bilgisayarınıza yüklemek için gerekli ayrıcalıklara sahip bir kullanıcı adı ve parola girin. **Tamam**’ı seçin.

   ![Profili yükleme için kullanıcı adı ve parola kutuları](./media/point-to-site-vpn-client-configuration-radius/adusername.png)
8. Profili yükledikten sonra görünür **profilleri** iletişim kutusu. Ayrıca, daha sonra bu iletişim kutusunu açabilirsiniz **sistem tercihleri**.

   !["Profiller" iletişim kutusu](./media/point-to-site-vpn-client-configuration-radius/adsystempref.png)
9. VPN bağlantısı erişmek için açın **ağ** iletişim kutusundan **sistem tercihleri**.

   ![Sistem Tercihleri'nde simgeleri](./media/point-to-site-vpn-client-configuration-radius/adnetwork.png)
10. VPN bağlantı olarak görünür **Ikev2 VPN**. Güncelleştirerek adını değiştirebilirsiniz **mobileconfig** dosya.

    ![VPN bağlantısı için Ayrıntılar](./media/point-to-site-vpn-client-configuration-radius/adconnection.png)
11. Seçin **kimlik doğrulama ayarları**. Seçin **kullanıcıadı** listesinde ve kimlik bilgilerinizi girin. Kimlik bilgileri daha önce ardından girerseniz **kullanıcıadı** otomatik olarak seçilen liste ve kullanıcı adı ve parola önceden doldurulmuş. Seçin **Tamam** ayarları kaydetmek için.

    ![Kimlik doğrulama ayarları](./media/point-to-site-vpn-client-configuration-radius/adauthentication.png)
12. Geri **ağ** iletişim kutusunda **Uygula** değişiklikleri kaydedin. Bağlantıyı başlatmak için seçin **Connect**.

#### <a name="adlinuxcli"></a>Linux VPN aracılığıyla strongSwan istemcisi Kurulumu

Aşağıdaki yönergeler, ubuntu'da 17.0.4 strongSwan 5.5.1 aracılığıyla oluşturuldu. Gerçek ekranlar, Linux ve strongSwan sürümünüze bağlı olarak farklı olabilir.

1. Açık **Terminal** yüklemek için **strongSwan** ve örnekte komutu çalıştırarak, Ağ Yöneticisi. İlgili bir hata alırsanız `libcharon-extra-plugins`, değiştirin `strongswan-plugin-eap-mschapv2`.

   ```Terminal
   sudo apt-get install strongswan libcharon-extra-plugins moreutils iptables-persistent network-manager-strongswan
   ```
2. Seçin **Ağ Yöneticisi** simgesi (yukarı-oku/aşağı ok) ve seçim **düzenleme bağlantıları**.

   !["Bağlantıları Düzenle" seçimi Ağ Yöneticisi'nde](./media/point-to-site-vpn-client-configuration-radius/EditConnection.png)
3. Seçin **Ekle** yeni bir bağlantı oluşturmak için.

   ![Bağlantı için "Ekle" düğmesi](./media/point-to-site-vpn-client-configuration-radius/AddConnection.png)
4. Seçin **IPSec/Ikev2 (strongswan)** öğelerine tıklayın ve ardından gelen **Oluştur**. Bu adımda, bağlantınızı yeniden adlandırabilirsiniz.

   ![Bağlantı türü seçme](./media/point-to-site-vpn-client-configuration-radius/AddIKEv2.png)
5. Açık **VpnSettings.xml** dosya **genel** indirilen istemci yapılandırma dosya klasörü. Adlı Etiket Bul `VpnServer` ve itibaren bu adı kopyalayın `azuregateway` ve ile biten `.cloudapp.net`.

   ![VpnSettings.xml dosyanın içeriği](./media/point-to-site-vpn-client-configuration-radius/VpnSettings.png)
6. Bu ad alanının içine yapıştırın **adresi** yeni VPN bağlantınızın alan **ağ geçidi** bölümü. Ardından, sonunda, klasör simgesini seçerek **sertifika** göz atın, alan **genel** klasörünü açın ve seçin **VpnServerRoot** dosya.
7. İçinde **istemci** bölümünü seçin bağlantıyı **EAP** için **kimlik doğrulaması**, kullanıcı adı ve parolayı girin. Bu bilgileri kaydetmek için sağ taraftaki kilit simgesini gerekebilir. Ardından **Kaydet**’i seçin.

   ![Bağlantı ayarlarını düzenleme](./media/point-to-site-vpn-client-configuration-radius/editconnectionsettings.png)
8. Seçin **Ağ Yöneticisi** simgesi (yukarı-oku/aşağı ok) ve üzerine geldiğinizde **VPN bağlantıları**. Oluşturduğunuz VPN bağlantısını görürsünüz. Bağlantıyı başlatmak için bunu seçin.

   ![Ağ Yöneticisi'nde "VPN Radius" bağlantısı](./media/point-to-site-vpn-client-configuration-radius/ConnectRADIUS.png)

## <a name="certeap"></a>Sertifika kimlik doğrulaması
 
VPN istemcisi yapılandırma dosyalarını EAP-TLS protokolünü kullanan RADIUS sertifika kimlik doğrulaması için oluşturabilirsiniz. Genellikle, VPN için bir kullanıcının kimliğini doğrulamak için bir kuruluş tarafından verilen bir sertifika kullanılır. Tüm bağlanan kullanıcıların cihazlarında yüklü bir sertifika olduğundan ve RADIUS sunucunuzun sertifika doğrulayabilirsiniz emin olun.

>[!NOTE]
>[!INCLUDE [TLS](../../includes/vpn-gateway-tls-change.md)]
>

Komutlarda `-AuthenticationMethod` olduğu `EapTls`. İstemci, sertifika kimlik doğrulaması sırasında RADIUS sunucusu sertifikasını doğrulayarak doğrular. `-RadiusRootCert` RADIUS sunucusu doğrulamak için kullanılan kök sertifikasını içeren .cer dosyasıdır.

Her VPN istemci cihaz, yüklü istemci sertifikasını gerektirir. Bazen bir Windows cihazı birden çok istemci sertifika yok. Kimlik doğrulaması sırasında tüm sertifikaları listeleyen bir açılır iletişim kutusunda Bu neden olabilir. Kullanıcı daha sonra kullanmak üzere bir sertifika seçmeniz gerekir. Doğru sertifikayı istemci sertifikasının bağlanması kök sertifikayı belirterek filtrelenebilen. 

`-ClientRootCert` kök sertifikasını içeren .cer dosyasıdır. Bu isteğe bağlı bir parametredir. Bağlanmak istediğiniz cihazın yalnızca bir istemci sertifikası varsa, bu parametre belirtmeniz gerekmez.

### <a name="certfiles"></a>1. VPN istemcisi yapılandırma dosyalarını oluştur

Sertifika kimlik doğrulaması ile kullanılacak VPN istemcisi yapılandırma dosyalarını oluşturur. VPN istemcisi yapılandırma dosyalarını, aşağıdaki komutu kullanarak oluşturabilirsiniz:
 
```azurepowershell-interactive
New-AzVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapTls" -RadiusRootCert <full path name of .cer file containing the RADIUS root> -ClientRootCert <full path name of .cer file containing the client root> | fl
```

Komut çalıştıran bir bağlantıyı döndürür. Bağlantıyı kopyalayıp VpnClientConfiguration.zip indirmek için bir web tarayıcısına yapıştırın. Aşağıdaki klasörlerin görüntülemek için dosyanın sıkıştırmasını açın:

* **WindowsAmd64** ve **WindowsX86**: Bu klasörler sırasıyla Windows 64-bit ve 32-bit yükleyici paketleri içerir. 
* **GenericDevice**: Bu klasör, kendi VPN istemci yapılandırması oluşturmak için kullanılan genel bilgiler içerir.

İstemci yapılandırması dosyaları zaten oluşturduysanız, bunları kullanarak alabileceğiniz `Get-AzVpnClientConfiguration` cmdlet'i. Ancak örneğin VPN protokol türü veya kimlik doğrulama türü, P2S VPN yapılandırmanıza, herhangi bir değişiklik yaparsanız yapılandırmasını otomatik olarak güncelleştirilmez. Çalıştırmalısınız `New-AzVpnClientConfiguration` yeni bir yapılandırma yükleme oluşturmak için cmdlet'i.

Daha önce oluşturulan istemci yapılandırma dosyalarını almak için aşağıdaki komutu kullanın:

```azurepowershell-interactive
Get-AzVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" | fl
```
 
### <a name="setupusername"></a> 2. VPN istemcisi yapılandırma

Aşağıdaki VPN istemcileri yapılandırabilirsiniz:

* [Windows](#certwincli)
* [Mac (OS X)](#certmaccli)
* Linux (desteklenen hiçbir makale henüz adımlar)

#### <a name="certwincli"></a>Windows VPN istemcisi Kurulumu

1. Bir yapılandırma paketini seçin ve istemci cihaza yükleyin. Bir 64-bit işlemci mimarisi için seçin **VpnClientSetupAmd64** Yükleyici paketi. Bir 32-bit işlemci mimarisi için seçin **VpnClientSetupX86** Yükleyici paketi. Bir SmartScreen açılır pencere görürseniz seçin **daha fazla bilgi** > **yine de Çalıştır**. Paketi ayrıca diğer istemci bilgisayarlara yüklemek üzere kaydedebilirsiniz.
2. Her bir istemci kimlik doğrulaması için bir istemci sertifikası gerektirir. İstemci sertifikası yükleyin. İstemci sertifikaları hakkında daha fazla bilgi için bkz. [noktadan siteye için istemci sertifikalarını](vpn-gateway-certificates-point-to-site.md). Oluşturulmuş bir sertifika yüklemek için bkz [Windows istemcilerinde sertifika yükleme](point-to-site-how-to-vpn-client-install-azure-cert.md).
3. İstemci bilgisayarda göz atın **ağ ayarlarını** seçip **VPN**. VPN bağlantısı, bağlandığı sanal ağın adını gösterir.

#### <a name="certmaccli"></a>Mac (OS X) VPN istemcisi Kurulumu

Azure sanal ağa bağlanan her Mac cihaz için ayrı profil oluşturmanız gerekir. Bu cihazlar kullanıcı sertifikası kimlik doğrulamasını profilinde belirtilmesi gerekli olmasıdır. **Genel** klasörü bir profil oluşturmak için gereken tüm bilgiler bulunur:

* **VpnSettings.xml** sunucu adresi ve tünel türü gibi önemli ayarları içerir.
* **VpnServerRoot.cer** P2S bağlantı kurulumda VPN ağ geçidini doğrulamak için gerekli kök sertifika içerir.
* **RadiusServerRoot.cer** kimlik doğrulaması sırasında RADIUS sunucusu doğrulamak için gerekli kök sertifika içerir.

Sertifika kimlik doğrulaması için bir Mac bilgisayarda yerel VPN istemcisini yapılandırmak için aşağıdaki adımları kullanın:

1. İçeri aktarma **VpnServerRoot** ve **RadiusServerRoot** kök Mac'iniz için sertifikalar Her dosya, Mac bilgisayara kopyalayın, çift tıklayın ve ardından **Ekle**.

   ![VpnServerRoot sertifikası ekleniyor](./media/point-to-site-vpn-client-configuration-radius/addcert.png)

   ![RadiusServerRoot sertifikası ekleniyor](./media/point-to-site-vpn-client-configuration-radius/radiusrootcert.png)
2. Her bir istemci kimlik doğrulaması için bir istemci sertifikası gerektirir. İstemci cihaza istemci sertifikası yükleyin.
3. Açık **ağ** iletişim kutusunun altında **ağ tercihlerini**. Seçin **+** Azure sanal ağı için P2S bağlantısı için yeni bir VPN istemci bağlantı profili oluşturmak için.

   **Arabirimi** değer **VPN**ve **VPN türü** değer **Ikev2**. Profili için bir ad belirtin **hizmet adı** kutusuna ve ardından **Oluştur** VPN istemci bağlantı profili oluşturmak için.

   ![Arabirim ve hizmet adı bilgileri](./media/point-to-site-vpn-client-configuration-radius/network.png)
4. İçinde **genel** klasörü, gelen **VpnSettings.xml** dosyası, kopya **VpnServer** etiket değeri. Bu değeri yapıştırın **sunucu adresi** ve **Uzak Kimliği** profilinin kutuları. Bırakın **yerel kimliği** kutusunu boş.

   ![Sunucu bilgileri](./media/point-to-site-vpn-client-configuration-radius/servertag.png)
5. Seçin **kimlik doğrulama ayarları**seçip **sertifika**. 

   ![Kimlik doğrulama ayarları](./media/point-to-site-vpn-client-configuration-radius/certoption.png)
6. Tıklayın **seçin** için kimlik doğrulaması için kullanmak istediğiniz sertifikayı seçin.

   ![Kimlik doğrulaması için bir sertifika seçme](./media/point-to-site-vpn-client-configuration-radius/certificate.png)
7. **Bir kimlik seçin** sertifikaları için öncelikle repairshop listesini görüntüler. Sunucunun doğru sertifikayı seçin ve ardından **devam**.

   !["Bir kimlik seçin" listesi](./media/point-to-site-vpn-client-configuration-radius/identity.png)
8. İçinde **yerel kimliği** kutusunda, (6. adım)'deki Sertifika adını belirtin. Bu örnekte, bunun **ikev2Client.com**. Ardından, **Uygula** değişiklikleri kaydetmek için düğme.

   !["Yerel ID" kutusu](./media/point-to-site-vpn-client-configuration-radius/applyconnect.png)
9. İçinde **ağ** iletişim kutusunda **Uygula** tüm değişiklikleri kaydedin. Ardından, **Connect** Azure sanal ağı için P2S bağlantısı başlatılamadı.

## <a name="otherauth"></a>Diğer kimlik doğrulama türleri ve protokolleri ile çalışma

Farklı kimlik doğrulama türü (örneğin, OTP) kullanın veya farklı kimlik doğrulama protokolü (örneğin, PEAP-MSCHAPv2 EAP-MSCHAPv2 yerine) kullanmak için kendi VPN istemci yapılandırma profili oluşturmanız gerekir. Profili oluşturmak için sanal ağ geçidinin IP adresi, tünel türü ve bölünmüş tünel yolları gibi bilgiler gerekir. Aşağıdaki adımları kullanarak bu bilgi edinebilirsiniz:

1. Kullanım `Get-AzVpnClientConfiguration` EapMSChapv2 VPN istemci yapılandırması oluşturmak için cmdlet'i.

2. VpnClientConfiguration.zip dosyanın sıkıştırmasını açın ve Ara **GenericDevice** klasör. 64-bit ve 32 bit mimarileri için Windows Yükleyici içeren klasörlere göz ardı edin.
 
3. **GenericDevice** klasörde adlı bir XML dosyası **VpnSettings**. Bu dosya, gerekli tüm bilgileri içerir:

   * **VpnServer**: Azure VPN ağ geçidi FQDN'si. Bu, istemcinin bağlandığı adresidir.
   * **VpnType**: Bağlanmak için kullandığınız tünel türü.
   * **Yollar**: Azure sanal ağı için bağlı olan tek trafik P2S tüneli üzerinden gönderilmesi profilinizde yapılandırmak zorunda yollar.
   
   **GenericDevice** klasör adı verilen bir .cer dosyasına da içerir **VpnServerRoot**. Bu dosyayı, P2S bağlantısı Kurulum sırasında Azure VPN ağ geçidini doğrulamak için gerekli kök sertifika içerir. Azure sanal ağına bağlanan tüm cihazlarda, sertifikayı yükleyin.

## <a name="next-steps"></a>Sonraki adımlar

Makaleye dönün [P2S yapılandırmanızı tamamlamak](point-to-site-how-to-radius-ps.md).

P2S için sorun giderme bilgileri için bkz. [sorun giderme Azure noktadan siteye bağlantılar](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).
