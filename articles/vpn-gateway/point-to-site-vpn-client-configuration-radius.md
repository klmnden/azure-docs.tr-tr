---
title: "Oluşturun ve P2S RADIUS bağlantıları için VPN istemcisi yapılandırma dosyalarını yükleyin: PowerShell: Azure | Microsoft Docs"
description: "Windows, Mac OS X ve Linux VPN istemci RADIUS kimlik doğrulaması kullanan bağlantılar için yapılandırma dosyaları oluşturun."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jpconnock
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/12/2018
ms.author: cherylmc
ms.openlocfilehash: 1d57537428f5ac1085b6cbae93be6f77c71b12e7
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2018
---
# <a name="create-and-install-vpn-client-configuration-files-for-p2s-radius-authentication"></a>Oluşturun ve VPN istemcisi yapılandırma dosyalarını P2S RADIUS kimlik doğrulaması için yükleyin

Noktası siteye (P2S) bir sanal ağa bağlanmak için gelen bağlanacağım istemci aygıt yapılandırmanız gerekir. Windows, Mac OS X ve Linux istemci cihazlar, P2S VPN bağlantıları oluşturabilirsiniz. 

RADIUS kimlik doğrulamasını kullanırken, birden çok kimlik doğrulama seçeneği vardır: kullanıcı adı/parola kimlik doğrulaması, sertifika kimlik doğrulaması ve diğer kimlik doğrulama türleri. VPN istemci yapılandırmasında her kimlik doğrulama türü için farklıdır. VPN istemcisini yapılandırmak için gereken ayarları içeren istemci yapılandırma dosyalarını kullanın. Bu makalede oluşturmak ve kullanmak istediğiniz RADIUS kimlik doğrulaması türü için VPN istemci yapılandırma yüklemenize yardımcı olur.

P2S RADIUS kimlik doğrulaması için yapılandırma iş akışı aşağıdaki gibidir:

1. [P2S bağlantısı için Azure VPN ağ geçidi ayarlamak](point-to-site-how-to-radius-ps.md).
2. [RADIUS sunucunuz kimlik doğrulaması için ayarlama](point-to-site-how-to-radius-ps.md#radius). 
3. **Seçtiğiniz kimlik doğrulama seçeneği VPN istemci yapılandırmasını almak ve VPN istemci ayarlamak için kullanın** (Bu makalede).
4. [P2S yapılandırmanızı tamamlamak ve bağlanın](point-to-site-how-to-radius-ps.md).

>[!IMPORTANT]
>Varsa noktadan siteye VPN yapılandırma değişiklikleri VPN protokol türü veya kimlik doğrulama türü gibi bir VPN istemci yapılandırma profili oluşturduktan sonra oluşturmak ve yeni bir VPN istemci yapılandırması, kullanıcıların cihazlarına yüklemeniz gerekir.
>
>

Bu makalede bölümleri kullanmak için ilk olarak kullanmak istediğiniz kimlik doğrulaması türünü karar verin: kullanıcı adı/parola, sertifika veya diğer kimlik doğrulama türleri. Her bölüm, Windows, Mac OS X ve Linux (şu anda sınırlı adımları) için adım vardır.

## <a name="adeap"></a>Kullanıcı adı/parola kimlik doğrulaması

Active Directory kullanabilir veya Active Directory kullanmamanız için kullanıcı adı/parola kimlik doğrulaması yapılandırabilirsiniz. Her iki senaryo ile bağlanan tüm kullanıcılara RADIUS kimlik doğrulaması kullanıcı adı/parola kimlik bilgileri olduğundan emin olun.

Kullanıcı adı/parola kimlik doğrulamasını yapılandırdığınızda, yalnızca EAP-MSCHAPv2 kullanıcı adı/parola kimlik doğrulama protokolü için bir yapılandırma oluşturabilirsiniz. Komutlarda `-AuthenticationMethod` olan `EapMSChapv2`.

### <a name="usernamefiles"></a> 1. VPN istemcisi yapılandırma dosyalarını oluştur

Kullanıcı adı/parola kimlik doğrulaması ile kullanmak için VPN istemcisi yapılandırma dosyalarını oluşturur. Aşağıdaki komutu kullanarak VPN istemcisi yapılandırma dosyalarını oluşturabilirsiniz:

```powershell 
New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapMSChapv2"
```
 
Komutunu çalıştırarak bir bağlantıyı döndürür. Karşıdan yüklemek için bir web tarayıcısı bağlantısını kopyalayıp **VpnClientConfiguration.zip**. Aşağıdaki klasörlerin görüntülemek için dosyanın sıkıştırmasını açın: 
 
* **WindowsAmd64** ve **WindowsX86**: Bu klasörler sırasıyla Windows 64-bit ve 32-bit yükleyicisi paketleri içerir. 
* **Genel**: Bu klasör, kendi VPN istemci yapılandırması oluşturmak için kullandığınız genel bilgiler içerir. Bu klasör için kullanıcı adı/parola kimlik doğrulaması yapılandırmaları gerekmez.
* **Mac**: sanal ağ geçidi oluştururken, Ikev2 yapılandırdıysanız, adında bir klasör bkz **Mac** içeren bir **mobileconfig** dosya. Bu dosya, Mac istemcileri yapılandırmak için kullanın.

İstemci yapılandırma dosyaları zaten oluşturduysanız, bunları kullanarak alabilirsiniz `Get-AzureRmVpnClientConfiguration` cmdlet'i. Ancak VPN protokol türü veya kimlik doğrulama türü gibi P2S VPN yapılandırması için herhangi bir değişiklik yaparsanız yapılandırmasını otomatik olarak güncelleştirilmez. Çalıştırmalısınız `New-AzureRmVpnClientConfiguration` yeni bir yapılandırma indirme oluşturmak için cmdlet'i.

Daha önce oluşturulan istemci yapılandırma dosyalarını almak için aşağıdaki komutu kullanın:

```powershell
Get-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW"
```

### <a name="setupusername"></a> 2. VPN istemcileri yapılandırın

Aşağıdaki VPN istemcileri yapılandırabilirsiniz:

* [Windows](#adwincli)
* [Mac (OS X)](#admaccli)
* [StrongSwan kullanarak Linux](#adlinuxcli)
 
#### <a name="adwincli"></a>Windows VPN istemcisi Kurulumu

Sürüm için İstemci mimarisi eşleştiği sürece, her Windows istemci bilgisayarda aynı VPN istemcisi yapılandırma paketini kullanabilirsiniz. Desteklenen istemci işletim sistemleri listesi için bkz: [SSS](vpn-gateway-vpn-faq.md#P2S).

Sertifika kimlik doğrulaması için yerel Windows VPN istemcisi yapılandırmak için aşağıdaki adımları kullanın:

1. Windows bilgisayarın mimarisiyle karşılık gelen VPN istemcisi yapılandırma dosyalarını seçin. 64-bit işlemci mimarisi için seçin **VpnClientSetupAmd64** Yükleyici paketi. 32-bit işlemci mimarisi için seçin **VpnClientSetupX86** Yükleyici paketi. 
2. Paketi yüklemek için çift tıklayın. SmartScreen açılır pencere görürseniz seçin **daha fazla bilgi** > **yine de çalıştırmaya**.
3. İstemci bilgisayarda Gözat **ağ ayarlarını** seçip **VPN**. VPN bağlantısı, bağlandığı sanal ağın adını gösterir. 

#### <a name="admaccli"></a>Mac (OS X) VPN istemci kurulumu

1. Seçin **VpnClientSetup mobileconfig** dosya ve kullanıcılardan her biri için gönderin. E-posta veya başka bir yöntemi kullanabilirsiniz.

2. Bulun **mobileconfig** Mac dosyada

   ![Mobilconfig dosyasının konumu](./media/point-to-site-vpn-client-configuration-radius/admobileconfigfile.png)
3. Yükleyin ve seçmek için profil çift **devam**. Profil adı, sanal ağın adı ile aynıdır.

   ![Yükleme iletisi](./media/point-to-site-vpn-client-configuration-radius/adinstall.png)
4. Seçin **devam** gönderenin profilinin güven ve yüklemeye devam etmek için.

   ![Onay iletisi](./media/point-to-site-vpn-client-configuration-radius/adcontinue.png)
5. Profili yükleme sırasında kullanıcı adı ve VPN kimlik doğrulama parolasını belirtmek için seçeneğiniz vardır. Bu bilgileri girmek için zorunlu değildir. Bunu yaparsanız, bilgileri kaydedilir ve bir bağlantı başlattığınızda otomatik olarak kullanılır. Seçin **yükleme** devam etmek için.

   ![VPN için kullanıcı adı ve parola kutuları](./media/point-to-site-vpn-client-configuration-radius/adsettings.png)
6. Profil bilgisayarınıza yüklemek için gerekli ayrıcalıklara sahip bir kullanıcı adı ve parola girin. **Tamam**’ı seçin.

   ![Profil yüklemesi için kullanıcı adı ve parola kutuları](./media/point-to-site-vpn-client-configuration-radius/adusername.png)
7. Profili yükledikten sonra görünür **profilleri** iletişim kutusu. Bu iletişim kutusundan daha sonra da açabilirsiniz **sistem tercihleri**.

   !["Profilleri" iletişim kutusu](./media/point-to-site-vpn-client-configuration-radius/adsystempref.png)
8. VPN bağlantısı erişmek için açık **ağ** iletişim kutusundan **sistem tercihleri**.

   ![Sistem tercihleri simgeleri](./media/point-to-site-vpn-client-configuration-radius/adnetwork.png)
9. VPN bağlantısı olarak görünür **Ikev2 VPN**. Güncelleştirerek adını değiştirebilirsiniz **mobileconfig** dosya.

   ![VPN bağlantısı için Ayrıntılar](./media/point-to-site-vpn-client-configuration-radius/adconnection.png)
10. Seçin **kimlik doğrulama ayarlarını**. Seçin **kullanıcıadı** listesinde ve kimlik bilgilerinizi girin. Kimlik bilgilerini daha önce ardından girerseniz **kullanıcıadı** otomatik olarak seçilen liste ve kullanıcı adı ve parola önceden doldurulmaz. Seçin **Tamam** ayarları kaydetmek için.

    ![Kimlik doğrulama ayarları](./media/point-to-site-vpn-client-configuration-radius/adauthentication.png)
11. Geri **ağ** iletişim kutusunda **Uygula** değişiklikleri kaydedin. Bağlantı başlatmasını seçin **Bağlan**.

#### <a name="adlinuxcli"></a>Linux VPN istemci kurulumu strongSwan aracılığıyla

Aşağıdaki yönergeler, Ubuntu 17.0.4 üzerinde strongSwan 5.5.1 üzerinden oluşturuldu. Gerçek ekranlar, Linux ve strongSwan sürümüne bağlı olarak farklı olabilir.

1. Açık **Terminal** yüklemek için **strongSwan** ve örnekte komutunu çalıştırarak, Ağ Yöneticisi. İlgili bir hata alırsanız, `libcharon-extra-plugins`, bunların yerine `strongswan-plugin-eap-mschapv2`.

   ```Terminal
   sudo apt-get install strongswan libcharon-extra-plugins moreutils iptables-persistent network-manager-strongswan
   ```
2. Seçin **Ağ Yöneticisi** simgesi (yukarı-oku/aşağı ok) ve select **düzenleme bağlantıları**.

   ![Seçim Ağ Yöneticisi'nde "Bağlantıları Düzenle"](./media/point-to-site-vpn-client-configuration-radius/EditConnection.png)
3. Seçin **Ekle** düğmesi yeni bir bağlantı oluşturun.

   ![Bir bağlantı için "Ekle" düğmesi](./media/point-to-site-vpn-client-configuration-radius/AddConnection.png)
4. Seçin **IPSec/Ikev2 (strongswan)** aşağı açılan menüsünden ve ardından **oluşturma**. Bu adımda, bağlantınızı yeniden adlandırabilirsiniz.

   ![Bağlantı türü seçme](./media/point-to-site-vpn-client-configuration-radius/AddIKEv2.png)
5. Açık **VpnSettings.xml** dosya **genel** indirilen istemci yapılandırma dosyalarının klasör. Adlı Etiket Bul `VpnServer` ve itibaren adını kopyalama `azuregateway` ve ile biten `.cloudapp.net`.

   ![VpnSettings.xml dosyasının içeriği](./media/point-to-site-vpn-client-configuration-radius/VpnSettings.png)
6. Bu ad alanının içine yapıştırma **adresi** yeni VPN bağlantınızı alanını **ağ geçidi** bölümü. Ardından, sonunda klasör simgesini seçin **sertifika** alan, Gözat **genel** klasörü ve select **VpnServerRoot** dosya.
7. İçinde **istemci** select bağlantı bölümünü **EAP** için **kimlik doğrulaması**, kullanıcı adı ve parolayı girin. Bu bilgileri kaydetmek için sağdaki kilit simgesini seçin gerekebilir. Ardından **Kaydet**’i seçin.

   ![Bağlantı ayarlarını düzenleme](./media/point-to-site-vpn-client-configuration-radius/editconnectionsettings.png)
8. Seçin **Ağ Yöneticisi** simgesi (yukarı-oku/aşağı ok) ve üzerine getirin **VPN bağlantıları**. Oluşturduğunuz VPN bağlantısını görür. Bağlantı başlatmak için onu seçin.

   ![Ağ Yöneticisi'nde "VPN Radius" bağlantı](./media/point-to-site-vpn-client-configuration-radius/ConnectRADIUS.png)

## <a name="certeap"></a>Sertifika kimlik doğrulaması
 
Yapılandırma dosyaları EAP-TLS protokolünü kullanan RADIUS sertifika kimlik doğrulaması için VPN istemcisi oluşturabilirsiniz. Genellikle, kuruluş tarafından verilen bir sertifika, VPN için bir kullanıcının kimliğini doğrulamak için kullanılır. Tüm bağlanan kullanıcıların cihazlarına bir sertifikasının yüklü olduğundan ve RADIUS sunucunuzun sertifikayı doğrulayabilirsiniz emin olun.

Komutlarda `-AuthenticationMethod` olan `EapTls`. Sertifika kimlik doğrulaması sırasında istemci sertifikasını doğrulayarak RADIUS sunucusu doğrular. `-RadiusRootCert` RADIUS sunucusu doğrulamak için kullanılan kök sertifikasını içeren .cer dosyasıdır.

Her VPN istemci aygıt yüklü istemci sertifikasını gerektirir. Bazen bir Windows aygıtı birden çok istemci sertifikalarını vardır. Kimlik doğrulaması sırasında tüm sertifikaları listeleyen bir açılır iletişim kutusunda Bu neden olabilir. Kullanıcı daha sonra kullanmak üzere sertifika seçmeniz gerekir. Doğru sertifikayı istemci sertifika zincir kök sertifikası belirterek filtrelenebilen. 

`-ClientRootCert` kök sertifikasını içeren .cer dosyasıdır. Bu isteğe bağlı bir parametredir. Bağlanmak istediğiniz cihaz yalnızca bir istemci sertifikası varsa, bu parametre belirtmeniz gerekmez.

### <a name="certfiles"></a>1. VPN istemcisi yapılandırma dosyalarını oluştur

Sertifika kimlik doğrulaması ile kullanmak için VPN istemcisi yapılandırma dosyalarını oluşturur. Aşağıdaki komutu kullanarak VPN istemcisi yapılandırma dosyalarını oluşturabilirsiniz:
 
```powershell
New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapTls" -RadiusRootCert <full path name of .cer file containing the RADIUS root> -ClientRootCert <full path name of .cer file containing the client root> | fl
```

Komutunu çalıştırarak bir bağlantıyı döndürür. VpnClientConfiguration.zip karşıdan yüklemek için bir web tarayıcısı bağlantısını kopyalayıp yeniden açın. Aşağıdaki klasörlerin görüntülemek için dosyanın sıkıştırmasını açın:

* **WindowsAmd64** ve **WindowsX86**: Bu klasörler sırasıyla Windows 64-bit ve 32-bit yükleyicisi paketleri içerir. 
* **GenericDevice**: Bu klasör, kendi VPN istemci yapılandırması oluşturmak için kullanılan genel bilgiler içerir.

İstemci yapılandırma dosyaları zaten oluşturduysanız, bunları kullanarak alabilirsiniz `Get-AzureRmVpnClientConfiguration` cmdlet'i. Ancak VPN protokol türü veya kimlik doğrulama türü gibi P2S VPN yapılandırması için herhangi bir değişiklik yaparsanız yapılandırmasını otomatik olarak güncelleştirilmez. Çalıştırmalısınız `New-AzureRmVpnClientConfiguration` yeni bir yapılandırma indirme oluşturmak için cmdlet'i.

Daha önce oluşturulan istemci yapılandırma dosyalarını almak için aşağıdaki komutu kullanın:

```powershell
Get-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" | fl
```
 
### <a name="setupusername"></a> 2. VPN istemcileri yapılandırın

Aşağıdaki VPN istemcileri yapılandırabilirsiniz:

* [Windows](#certwincli)
* [Mac (OS X)](#certmaccli)
* Linux (desteklenen hiçbir makale henüz adımları)

#### <a name="certwincli"></a>Windows VPN istemcisi Kurulumu

1. Bir yapılandırma paketi seçin ve istemci aygıtında yükleyin. 64-bit işlemci mimarisi için seçin **VpnClientSetupAmd64** Yükleyici paketi. 32-bit işlemci mimarisi için seçin **VpnClientSetupX86** Yükleyici paketi. SmartScreen açılır pencere görürseniz seçin **daha fazla bilgi** > **yine de çalıştırmaya**. Paketi ayrıca diğer istemci bilgisayarlara yüklemek üzere kaydedebilirsiniz.
2. Her bir istemci kimlik doğrulaması için bir istemci sertifikası gerektirir. İstemci sertifikası yükleyin. İstemci sertifikaları hakkında daha fazla bilgi için bkz: [noktası site için istemci sertifikaları](vpn-gateway-certificates-point-to-site.md). Oluşturulmuş bir sertifika yüklemek için bkz: [Windows istemcilerinde bir sertifika yüklemek](point-to-site-how-to-vpn-client-install-azure-cert.md).
3. İstemci bilgisayarda Gözat **ağ ayarlarını** seçip **VPN**. VPN bağlantısı, bağlandığı sanal ağın adını gösterir.

#### <a name="certmaccli"></a>Mac (OS X) VPN istemci kurulumu

Azure sanal ağa bağlanan her Mac cihaz için ayrı bir profil oluşturmanız gerekir. Bu cihazlar kullanıcı sertifikası profilinde belirtilen kimlik doğrulama gerektiren olmasıdır. **Genel** klasör bir profil oluşturmak için gerekli olan tüm bilgileri içerir:

* **VpnSettings.xml** sunucu adresi ve tünel türü gibi önemli ayarları içerir.
* **VpnServerRoot.cer** P2S bağlantısı kurulumda VPN ağ geçidi doğrulamak için gerekli kök sertifikası içeriyor.
* **RadiusServerRoot.cer** kimlik doğrulaması sırasında RADIUS sunucusu doğrulamak için gerekli kök sertifikası içeriyor.

Yerel VPN istemcisi Mac sertifika kimlik doğrulaması için yapılandırmak için aşağıdaki adımları kullanın:

1. İçeri aktarma **VpnServerRoot** ve **RadiusServerRoot** kök sertifikaları Mac için Mac için her bir dosya kopyalama, çift tıklayın ve ardından **Ekle**.

   ![VpnServerRoot sertifika ekleniyor](./media/point-to-site-vpn-client-configuration-radius/addcert.png)

   ![RadiusServerRoot sertifika ekleniyor](./media/point-to-site-vpn-client-configuration-radius/radiusrootcert.png)
2. Her bir istemci kimlik doğrulaması için bir istemci sertifikası gerektirir. İstemci sertifikası istemci cihaza yükleyin.
3. Açık **ağ** iletişim kutusunda altında **ağ tercihlerini**. Seçin  **+**  P2S bağlantısı Azure sanal ağı için yeni bir VPN istemci bağlantı profili oluşturmak için.

   **Arabirimi** değer **VPN**ve **VPN türü** değer **Ikev2**. Profil için bir ad belirtin **hizmet adı** kutusuna ve ardından **oluşturma** VPN istemci bağlantı profili oluşturmak için.

   ![Arabirim ve hizmet adı bilgileri](./media/point-to-site-vpn-client-configuration-radius/network.png)
4. İçinde **genel** klasörü, gelen **VpnSettings.xml** dosya, kopya **VpnServer** etiket değeri. Bu değeri yapıştırın **sunucu adresi** ve **Uzak Kimliği** profilinin kutuları. Bırakın **yerel kimliği** kutusu boş.

   ![Sunucu bilgileri](./media/point-to-site-vpn-client-configuration-radius/servertag.png)
5. Seçin **kimlik doğrulama ayarlarını**seçip **sertifika**. 

   ![Kimlik doğrulama ayarları](./media/point-to-site-vpn-client-configuration-radius/certoption.png)
6. Tıklatın **seçin** kimlik doğrulaması için kullanmak istediğiniz sertifikayı seçin.

   ![Kimlik doğrulaması için bir sertifika seçme](./media/point-to-site-vpn-client-configuration-radius/certificate.png)
7. **Bir kimlik seçin** aralarından seçim yapabileceğiniz sertifikaların listesini görüntüler. Doğru sertifikayı seçin ve ardından **devam**.

   !["Bir kimlik seçin" listesi](./media/point-to-site-vpn-client-configuration-radius/identity.png)
8. İçinde **yerel kimliği** kutusunda, sertifika (6. adım) adını belirtin. Bu örnekte bunun **ikev2Client.com**. Ardından, seçin **Uygula** değişiklikleri kaydetmek için düğmesi.

   !["Yerel kimliği" kutusu](./media/point-to-site-vpn-client-configuration-radius/applyconnect.png)
9. İçinde **ağ** iletişim kutusunda **Uygula** tüm değişiklikleri kaydetmek için. Ardından, seçin **Bağlan** Azure sanal ağı P2S bağlantısı başlatmak için.

## <a name="otherauth"></a>Diğer kimlik doğrulama türleri veya protokolleri ile çalışma

Farklı kimlik doğrulama türünü (örneğin, OTP) kullanın ya da farklı kimlik doğrulama protokolü (örneğin, PEAP-MSCHAPv2 EAP-MSCHAPv2 yerine) kullanmak için kendi VPN istemci yapılandırma profili oluşturmanız gerekir. Profili oluşturmak için sanal ağ ağ geçidi IP adresi, tünel türü ve bölünmüş tünel yolları gibi bilgiler gerekir. Aşağıdaki adımları kullanarak bu bilgileri elde edebilirsiniz:

1. Kullanım `Get-AzureRmVpnClientConfiguration` EapMSChapv2 için VPN istemci yapılandırması oluşturmak için cmdlet'i. Yönergeler için bkz: [Bu bölümde](#ccradius) makalenin.

2. VpnClientConfiguration.zip dosyanın sıkıştırmasını açın ve Ara **GenenericDevice** klasör. 64-bit ve 32-bit mimari Windows Installer'lar içeren klasörleri yoksay.
 
3. **GenenericDevice** adlı bir XML dosyasını içeren klasör **VpnSettings**. Bu dosya, gerekli tüm bilgileri içerir:

   * **VpnServer**: Azure VPN ağ geçidi FQDN'sini. Bu, istemcinin bağlandığı adresidir.
   * **VpnType**: tünel bağlanmak için kullandığınız türü.
   * **Yollar**: Azure sanal ağı için bağlı yalnızca trafiği P2S tünelinden gönderilir böylece profilinizde yapılandırmak zorunda yollar.
   
   **GenenericDevice** klasör adında bir .cer dosyasına da içerir **VpnServerRoot**. Bu dosya, P2S bağlantısı Kurulum sırasında Azure VPN ağ geçidi doğrulamak için gerekli kök sertifikasını içerir. Azure sanal ağına bağlayacaktır tüm cihazlarda sertifikası yükleyin.

## <a name="next-steps"></a>Sonraki adımlar

Makaleye dönün [P2S yapılandırmanızı tamamlamak](point-to-site-how-to-radius-ps.md).

P2S için sorun giderme bilgileri için bkz: [sorun giderme Azure noktadan siteye bağlantıları](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).