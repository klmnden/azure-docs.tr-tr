---
title: "Oluşturun ve P2S RADIUS bağlantıları için VPN istemcisi yapılandırma dosyalarını yükleyin: PowerShell: Azure | Microsoft Docs"
description: "Bu makalede, RADIUS kimlik doğrulaması kullanan noktadan siteye bağlantıları için VPN istemci yapılandırma dosyası oluşturmanıza yardımcı olur."
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
ms.date: 01/24/2018
ms.author: cherylmc
ms.openlocfilehash: 838065287279f1c17e7f294bc919c4a0421e2a58
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="create-and-install-vpn-client-configuration-files-for-p2s-radius-authentication"></a>Oluşturun ve VPN istemcisi yapılandırma dosyalarını P2S RADIUS kimlik doğrulaması için yükleyin

VPN istemcisi yapılandırma dosyalarını bir zip dosyasında bulunur. Yapılandırma dosyaları noktası Site bir sanal ağa bağlanmak yerel bir Windows veya Mac Ikev2 VPN istemcisi için gerekli ayarları sağlar. RADIUS sunucusu birden çok kimlik doğrulama seçenekleri sağlar ve bu nedenle, her seçenek için VPN istemci yapılandırmasında değişir.

### <a name="workflow"></a>İş akışı

  1. [P2S bağlantısı için Azure VPN ağ geçidi ayarlamak](point-to-site-how-to-radius-ps.md).
  2. [RADIUS sunucunuz kimlik doğrulaması için ayarlama](point-to-site-how-to-radius-ps.md#radius). 
  3. (Bu makalede) - seçtiğiniz kimlik doğrulama seçeneği VPN istemci yapılandırmasını almak ve VPN istemcisi Windows Cihazınızı ayarlamak için kullanın. Bu, Azure sanal ağlar için bir P2S bağlantısı üzerinden sağlar.
  4. [P2S yapılandırmanızı tamamlamak ve bağlanın](point-to-site-how-to-radius-ps.md).

>[!IMPORTANT]
>Varsa noktadan siteye VPN yapılandırma değişiklikleri VPN protokol türü veya kimlik doğrulama türü gibi bir VPN istemci yapılandırma profili oluşturduktan sonra oluşturmak ve yeni bir VPN istemci yapılandırma kullanıcı aygıtlarınızda yüklemeniz gerekir.
>
>

## <a name="adeap"></a>Kullanıcı adı/parola kimlik doğrulaması hakkında

* **AD kimlik doğrulaması:** AD etki alanı kimlik doğrulaması yaygın senaryodur. Bu senaryoda, kullanıcılar Azure sanal ağlara bağlanmak için etki alanı kimlik bilgilerini kullanın. VPN istemcisi yapılandırma dosyalarını AD RADIUS kimlik doğrulaması için oluşturabilirsiniz.

* **AD olmadan kimlik doğrulaması:** kullanıcı adı/parola RADIUS kimlik doğrulaması senaryosu AD olmadan da yapılandırabilirsiniz.

Bağlanan tüm kullanıcılara RADIUS kimlik doğrulaması kullanıcı adı/parola kimlik bilgilerine sahip olduğunuzdan emin olun. EAP-MSCHAPv2 kullanıcı adı/parola kimlik doğrulama protokolü için bir yapılandırma yalnızca oluşturabilirsiniz. '-AuthenticationMethod' 'EapMSChapv2' belirtilir.

## <a name="usernamefiles"></a> 1. VPN istemcisi yapılandırma dosyalarını oluştur

Kullanıcı adı/parola kimlik doğrulaması ile kullanmak için VPN istemcisi yapılandırma dosyalarını oluşturur. Aşağıdaki komutu kullanarak VPN istemcisi yapılandırma dosyalarını oluşturabilirsiniz:

```powershell 
New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapMSChapv2"
```
 
Komutunu çalıştırarak bir bağlantıyı döndürür. 'VpnClientConfiguration.zip' karşıdan yüklemek için bir web tarayıcısı bağlantısını kopyalayıp yeniden açın. Aşağıdaki klasörlerin görüntülemek için dosyanın sıkıştırmasını açın: 
 
* **WindowsAmd64** ve **WindowsX86** -bu klasörler sırasıyla Windows 64-bit ve 32-bit yükleyicisi paketleri içerir. 
* **Genel** -bu klasör, kendi VPN istemci yapılandırması oluşturmak için kullanılan genel bilgiler içerir. Bu klasör için kullanıcı adı/parola kimlik doğrulaması yapılandırmaları gerekli değildir.
* **Mac** -sanal ağ geçidi oluştururken, Ikev2 yapılandırılmışsa, 'içeren Mac' adında bir klasör gördüğünüz bir **mobileconfig** dosya. Bu dosya, Mac istemcileri yapılandırmak için kullanılır.

İstemci yapılandırma dosyaları zaten oluşturduysanız 'Get-AzureRmVpnClientConfiguration' cmdlet'ini kullanarak alabilir. Ancak, kimlik doğrulama türü ve VPN protokol türü gibi P2S VPN yapılandırması için herhangi bir değişiklik yaparsanız yapılandırmasını otomatik olarak güncelleştirmez. Yeni bir yapılandırma indirme oluşturmak için 'New-AzureRmVpnClientConfiguration' cmdlet'ini çalıştırmanız gerekir.

Daha önce oluşturulan istemci yapılandırma dosyalarını almak için aşağıdaki komutu kullanın:

```powershell
Get-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW"
```

## <a name="setupusername"></a> 2. Windows ve Mac VPN istemcilerini yapılandırma
 
### <a name="adwincli"></a>Windows VPN istemcisi Kurulumu

Sürüm için İstemci mimarisi eşleştiği sürece, her Windows istemci bilgisayarda aynı VPN istemcisi yapılandırma paketini kullanabilirsiniz. Desteklenen istemci işletim sistemleri listesi için noktadan siteye bölümüne bakın [SSS](vpn-gateway-vpn-faq.md#P2S).

Sertifika kimlik doğrulaması için yerel Windows VPN istemcisi yapılandırmak için aşağıdaki adımları kullanın:

1. Windows bilgisayarın mimarisiyle karşılık gelen VPN istemcisi yapılandırma dosyalarını seçin. 64-bit işlemci mimarisi için 'VpnClientSetupAmd64' Yükleyici paketi seçin. 32-bit işlemci mimarisi için 'VpnClientSetupX86' Yükleyici paketi seçin. 
2. Paketi yüklemek için çift tıklatın. Bir SmartScreen açılır penceresi görürseniz **Daha fazla bilgi**’ye ve ardından **Yine de çalıştır**’a tıklayın.
3. İstemci bilgisayarda **Ağ Ayarları**’na gidin ve **VPN** öğesine tıklayın. VPN bağlantısı, bağlandığı sanal ağın adını gösterir. 

### <a name="admaccli"></a>Mac (OSX) VPN istemci kurulumu

1. Seçin **VpnClientSetup mobileconfig** dosya ve kullanıcılardan her biri için gönderin. Bunu yapmak için e-posta veya başka bir yöntemi kullanabilirsiniz.

2. Bulun **mobileconfig** Mac dosyada

  ![mobilconfig dosyasını bulun](./media/point-to-site-vpn-client-configuration-radius/admobileconfigfile.png)
3. Bunu Yükle'yi tıklatın profilindeki çift **devam**. Profil adı sanal ağınızın adı ile aynıdır.

  ![Yükleme](./media/point-to-site-vpn-client-configuration-radius/adinstall.png)
4. Tıklatın **devam** gönderenin profilinin güven ve yüklemeye devam etmek için.

  ![devam](./media/point-to-site-vpn-client-configuration-radius/adcontinue.png)
5. Profili yükleme sırasında kullanıcı adı ve VPN kimlik doğrulaması için kullanılan parola belirtmek için seçeneği de verilir. Bu bilgileri girmek için zorunlu değildir. Belirtilmişse, bilgileri kaydedilir ve bir bağlantı başlattığınızda otomatik olarak kullanılır. Tıklatın **yükleme** devam etmek için.

  ![ayarlar](./media/point-to-site-vpn-client-configuration-radius/adsettings.png)
6. Profil bilgisayarınıza yüklemek için gerekli gerekli ayrıcalıkların bir kullanıcı adı ve parola girin. **Tamam**’a tıklayın.

  ![Kullanıcı adı ve parola](./media/point-to-site-vpn-client-configuration-radius/adusername.png)
7. Bir kez yüklendikten sonra görünür profilidir **profilleri** iletişim kutusu. Bu iletişim kutusunu de daha sonra açılabilir **sistem tercihleri**.

  ![Sistem tercihleri](./media/point-to-site-vpn-client-configuration-radius/adsystempref.png)
8. VPN bağlantısı erişmek için açık **ağ** iletişim kutusundan **sistem tercihleri**.

  ![Ağ](./media/point-to-site-vpn-client-configuration-radius/adnetwork.png)
9. VPN bağlantısı, olarak gösterilir **Ikev2 VPN**. Ad güncelleştirerek değiştirilebilir **mobileconfig** dosya.

  ![bağlantı](./media/point-to-site-vpn-client-configuration-radius/adconnection.png)
10. Tıklatın **kimlik doğrulama ayarlarını**. Seçin **kullanıcıadı** açılan ve kimlik bilgilerinizi girin. Kimlik bilgileri, ardından girerseniz **kullanıcıadı** otomatik olarak açılan seçilen ve kullanıcı adı ve parola önceden doldurulmuş haldedir. Ayarları kaydetmek için **Tamam**’a tıklayın. Bu, geri ağ iletişim kutusuna götürür.

  ![kimlik doğrulaması](./media/point-to-site-vpn-client-configuration-radius/adauthentication.png)
11. Tıklatın **Uygula** değişiklikleri kaydedin. Bağlantı başlatmak için tıklatın **Bağlan**.

## <a name="certeap"></a>Sertifika kimlik doğrulaması hakkında
 
Yapılandırma dosyaları EAP-TLS protokolünü kullanan RADIUS sertifika kimlik doğrulaması için VPN istemcisi oluşturabilirsiniz. Genellikle, kuruluş tarafından verilen bir sertifika, VPN için bir kullanıcının kimliğini doğrulamak için kullanılır. Bağlanan tüm kullanıcılara kullanıcıların cihazda yüklü bir sertifikasının yüklü olduğundan ve sertifikanın, RADIUS sunucusu tarafından doğrulanabilecek emin olun.
 
* '-AuthenticationMethod' 'EapTls' değil.
* Sertifika kimlik doğrulaması sırasında istemci sertifikasını doğrulayarak RADIUS sunucusu doğrular. -RadiusRootCert RADIUS sunucusu doğrulamak için kullanılan kök sertifikayı içeren .cer dosyasıdır.  
* Bazen bir Windows aygıtı birden çok istemci sertifikalarını vardır. Kimlik doğrulaması sırasında tüm sertifikaları listeleyen bir açılan iletişim kutusunda Bu neden olabilir. Kullanıcı daha sonra kullanmak üzere sertifika seçmeniz gerekir. Doğru sertifikayı istemci sertifika zincir kök sertifikası belirterek filtrelenebilen. '-ClientRootCert' kök sertifikayı içeren .cer dosyasıdır. Bu isteğe bağlı bir parametredir. Bağlamak istediğiniz cihaz yalnızca bir istemci sertifikası varsa, ardından bu parametre belirtilmesi gerekmez.

## <a name="certfiles"></a>1. VPN istemcisi yapılandırma dosyalarını oluştur

Sertifika kimlik doğrulaması ile kullanmak için VPN istemcisi yapılandırma dosyalarını oluşturur. Aşağıdaki komutu kullanarak VPN istemcisi yapılandırma dosyalarını oluşturabilirsiniz:
 
```powershell
New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapTls" -RadiusRootCert <full path name of .cer file containing the RADIUS root> -ClientRootCert <full path name of .cer file containing the client root> | fl
```

Komutunu çalıştırarak bir bağlantıyı döndürür. 'VpnClientConfiguration.zip' karşıdan yüklemek için bir web tarayıcısı bağlantısını kopyalayıp yeniden açın. Aşağıdaki klasörlerin görüntülemek için dosyanın sıkıştırmasını açın:

* **WindowsAmd64** ve **WindowsX86** -bu klasörler sırasıyla Windows 64-bit ve 32-bit yükleyicisi paketleri içerir. 
* **GenericDevice** -bu klasör, kendi VPN istemci yapılandırması oluşturmak için kullanılan genel bilgiler içerir.

İstemci yapılandırma dosyaları zaten oluşturduysanız 'Get-AzureRmVpnClientConfiguration' cmdlet'ini kullanarak alabilir. Ancak, kimlik doğrulama türü ve VPN protokol türü gibi P2S VPN yapılandırması için herhangi bir değişiklik yaparsanız yapılandırmasını otomatik olarak güncelleştirmez. Yeni bir yapılandırma indirme oluşturmak için 'New-AzureRmVpnClientConfiguration' cmdlet'ini çalıştırmanız gerekir.

Daha önce oluşturulan istemci yapılandırma dosyalarını almak için aşağıdaki komutu kullanın:

```powershell
Get-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" | fl
```
 
## <a name="setupusername"></a> 2. Windows ve Mac VPN istemcilerini yapılandırma

### <a name="certwincli"></a>Windows VPN istemcisi Kurulumu

1. Bir yapılandırma paketi seçin ve istemci aygıtında yükleyin. 64-bit işlemci mimarisi için 'VpnClientSetupAmd64' Yükleyici paketi seçin. 32-bit işlemci mimarisi için 'VpnClientSetupX86' Yükleyici paketi seçin. Bir SmartScreen açılır penceresi görürseniz **Daha fazla bilgi**’ye ve ardından **Yine de çalıştır**’a tıklayın. Paketi ayrıca diğer istemci bilgisayarlara yüklemek üzere kaydedebilirsiniz.
2. İstemci bilgisayarda **Ağ Ayarları**’na gidin ve **VPN** öğesine tıklayın. VPN bağlantısı, bağlandığı sanal ağın adını gösterir.

### <a name="certmaccli"></a>Mac (OSX) VPN istemci kurulumu

Azure VNet bağlanan her Mac cihaz için ayrı bir profil oluşturulması gerekir. Bu cihazlar kullanıcı sertifikası profilinde belirtilen kimlik doğrulama gerektiren olmasıdır. **Genel** klasör bir profil oluşturmak için gerekli tüm bilgileri içerir.

  * **VpnSettings.xml** sunucu adresi ve tünel türü gibi önemli ayarları içerir.
  * **VpnServerRoot.cer** P2S bağlantısı kurulumda VPN ağ geçidi doğrulamak için gerekli kök sertifikası içeriyor.
  * **RadiusServerRoot.cer** kimlik doğrulaması sırasında RADIUS sunucusu doğrulamak için gerekli kök sertifikası içeriyor.

Yerel VPN istemcisi Mac sertifika kimlik doğrulamasını yapılandırmak için aşağıdaki adımları kullanın:

1. İçeri aktarma **VpnServerRoot** ve **RadiusServerRoot** kök sertifikaları Mac için Bu, Mac üzerinden dosya kopyalama ve çift yapılabilir.  
Tıklatın **Ekle** almak için.

  **Add VpnServerRoot**

  ![Sertifika Ekle](./media/point-to-site-vpn-client-configuration-radius/addcert.png)

  **RadiusServerRoot Ekle**

  ![Sertifika Ekle](./media/point-to-site-vpn-client-configuration-radius/radiusrootcert.png)
2. Açık **ağ** altında iletişim **ağ tercihlerini** tıklatıp **'+'** Azure VNet P2S bağlantısı için yeni bir VPN istemci bağlantı profili oluşturmak için.

  **Arabirimi** 'VPN' bir değerdir ve **VPN türü** değer 'IKEv2' dir. Profil için bir ad belirtin **hizmet adı** alan ve ardından **oluşturma** VPN istemci bağlantı profili oluşturmak için.

  ![Ağ](./media/point-to-site-vpn-client-configuration-radius/network.png)
3. İçinde **genel** klasörü, gelen **VpnSettings.xml** dosya, kopya **VpnServer** etiket değeri. Bu değeri yapıştırın **sunucu adresi** ve **Uzak Kimliği** profilinin alanları. Bırakın **yerel kimliği** alanı boş.

  ![Sunucu bilgisi](./media/point-to-site-vpn-client-configuration-radius/servertag.png)
4. Tıklatın **kimlik doğrulama ayarlarını** seçip **sertifika**. 

  ![kimlik doğrulama ayarları](./media/point-to-site-vpn-client-configuration-radius/certoption.png)
5. Tıklatın **seçin...** kimlik doğrulaması için kullanmak istediğiniz sertifikayı seçmek için.

  ![sertifika](./media/point-to-site-vpn-client-configuration-radius/certificate.png)
6. **Bir kimlik seçin** aralarından seçim yapabileceğiniz sertifikaların listesini görüntüler. Doğru sertifikayı seçin ve ardından **devam**.

  ![identity](./media/point-to-site-vpn-client-configuration-radius/identity.png)
7. İçinde **yerel kimliği** alanında, sertifika (5. adım) adını belirtin. Bu örnekte, "ikev2Client.com" dir. Ardından **Uygula** değişiklikleri kaydetmek için düğmesi.

  ![uygula](./media/point-to-site-vpn-client-configuration-radius/applyconnect.png)
8. Üzerinde **ağ** iletişim kutusunda, tıklatın **Uygula** tüm değişiklikleri kaydetmek için. Ardından **Bağlan** Azure VNet P2S bağlantısı başlatmak için.

## <a name="otherauth"></a>Diğer kimlik doğrulama türleri veya protokolleri ile çalışma

Farklı kimlik doğrulama türü (örneğin, OTP) ve kullanıcı adı/parola veya değil sertifikaları kullanın ya da farklı kimlik doğrulama protokolü (örneğin, PEAP-MSCHAPv2, EAP-MSCHAPv2 yerine) kullanmak için kendi VPN istemci yapılandırma profili oluşturmanız gerekir. Profili oluşturmak için sanal ağ ağ geçidi IP adresi, tünel türü ve bölünmüş tünel yolları gibi bilgiler gerekir. Aşağıdaki adımları kullanarak bu bilgileri elde edebilirsiniz:

1. EapMSChapv2 için VPN istemci yapılandırması oluşturmak için 'Get-AzureRmVpnClientConfiguration' kullanın. Yönergeler için bkz: [Bu bölümde](#ccradius) makalenin.

2. VpnClientConfiguration.zip dosyanın sıkıştırmasını açın ve GenenericDevice klasörü arayın. 64-bit ve 32-bit mimari Windows Installer'lar içeren klasörleri yoksay.
 
3. GenenericDevice klasörü VpnSettings adlı bir XML dosyası içeriyor. Bu dosya, gerekli tüm bilgileri içerir.

  * VpnServer - Azure VPN ağ geçidi FQDN'sini. Bu, istemcinin bağlandığı adresidir.
  * VpnType - bağlanmak için kullandığınız tünel türü.
  * Yollar - yalnızca Azure VNet trafiği bağlı böylece profilinizde yapılandırmak zorunda yollar P2S tüneli üzerinden gönderilir.
  * GenenericDevice klasörü 'VpnServerRoot' adlı bir .cer dosyasına da içerir. Bu dosya, P2S bağlantısı Kurulum sırasında Azure VPN ağ geçidi doğrulamak için gerekli kök sertifikası içerir. Azure VNet bağlanacak tüm cihazlarda sertifikası yükleyin. 
 
## <a name="next-steps"></a>Sonraki adımlar

Makaleye dönün [P2S yapılandırmanızı tamamlamak](point-to-site-how-to-radius-ps.md).
