---
title: 'Azure AD Connect: Bağlantı sorunlarını giderme | Microsoft Docs'
description: Azure AD connect'teki bağlantı sorunlarını gidermek nasıl açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 3aa41bb5-6fcb-49da-9747-e7a3bd780e64
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: c0afc31bf08a5037d91885bc6a85c6aeaf858825
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60386695"
---
# <a name="troubleshoot-connectivity-issues-with-azure-ad-connect"></a>Azure AD connect'teki bağlantı sorunlarını giderme
Bu makalede, Azure AD Connect ve Azure AD arasındaki bağlantıyı nasıl çalıştığını ve bağlantı sorunlarını gidermek nasıl açıklanmaktadır. Bu proxy sunucusu olan bir ortamda görülebilmesi büyük olasılıkla sorunlardır.

## <a name="troubleshoot-connectivity-issues-in-the-installation-wizard"></a>Yükleme Sihirbazı'nda bağlantı sorunlarını giderme
Azure AD Connect kimlik doğrulaması için Modern kimlik doğrulaması (ADAL kitaplığını kullanarak) kullanıyor. Yükleme Sihirbazı'nı ve uygun eşitleme altyapısı machine.config, bu iki .NET uygulamaları olduğundan düzgün şekilde yapılandırılmasını gerektirir.

Bu makalede, Fabrikam ' proxy'si üzerinden Azure AD'ye nasıl bağlandığını gösterir. Proxy sunucusu fabrikamproxy adlı ve 8080 bağlantı noktası kullanıyordur.

İlk emin olmak ihtiyacımız [ **machine.config** ](how-to-connect-install-prerequisites.md#connectivity) doğru yapılandırılmış.  
![machineconfig](./media/tshoot-connect-connectivity/machineconfig.png)

> [!NOTE]
> Bazı Microsoft olmayan blog'larda değişiklikler miiserver.exe.config için bunun yerine yapılmalıdır, bu belgelenmiştir. Ancak, bu dosya, ilk yükleme sırasında çalışırsa, sistemin ilk yükseltme üzerinde çalışmayı durduruyor. Bu nedenle bile her yükseltmede üzerine yazılır. Bu nedenle, bunun yerine machine.config güncelleştirmek için kullanılması önerilir.
>
>

Proxy sunucusu, ayrıca açılan gerekli URL olması gerekir. Resmi liste belgelenen [Office 365 URL'leri ve IP adresi aralıkları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

Aşağıdaki tabloda bu URL'leri, Azure AD'ye hiç bağlanabilmesi için mutlak tam düşük düzeyde grup üyeliğidir. Bu liste, parola geri yazma ya da Azure AD Connect Health gibi isteğe bağlı özellikleri içermez. İçin başlangıç yapılandırmasını gidermenize yardımcı olması için burada belgelenmektedir.

| URL'si | Bağlantı noktası | Açıklama |
| --- | --- | --- |
| mscrl.microsoft.com |HTTP/80 |CRL listelerini indirmek için kullanılır. |
| \*. verisign.com |HTTP/80 |CRL listelerini indirmek için kullanılır. |
| \*. entrust.net |HTTP/80 |CRL listeleri için mfa'yı indirmek için kullanılır. |
| \*.windows.net |HTTPS/443 |Azure AD'de oturum açmak için kullanılır. |
| secure.aadcdn.microsoftonline-p.com |HTTPS/443 |MFA için kullanılır. |
| \*.microsoftonline.com |HTTPS/443 |Azure AD dizininizi yapılandırın ve verileri içeri/dışarı aktarma için kullanılır. |

## <a name="errors-in-the-wizard"></a>Sihirbazdaki hataları
Yükleme Sihirbazı'nı iki farklı güvenlik kapsamları kullanıyor. Sayfasında **Azure ad Connect**, şu anda oturum açmış kullanıcı kullanıyor. Sayfasında **yapılandırma**, için değiştirme [eşitleme altyapısı için hizmetini çalıştıran hesap](reference-connect-accounts-permissions.md#adsync-service-account). Bir sorun varsa, büyük olasılıkla zaten göründüğü **Azure ad Connect** proxy yapılandırmasını genel olduğundan Sihirbazı sayfası.

Aşağıdaki sorunlar Yükleme Sihirbazı'nda karşılaştığınız en yaygın hatalardır.

### <a name="the-installation-wizard-has-not-been-correctly-configured"></a>Yükleme Sihirbazı'nı doğru şekilde yapılandırılmamış
Sihirbaz proxy ulaştığında bu hata görüntülenir.  
![nomachineconfig](./media/tshoot-connect-connectivity/nomachineconfig.png)

* Bu hatayı görürseniz, doğrulama [machine.config](how-to-connect-install-prerequisites.md#connectivity) doğru şekilde yapılandırıldı.
* Hiçbir yanlışlık yoksa adımları [proxy bağlanabilirliği doğrulamak](#verify-proxy-connectivity) sorunu Sihirbazı mevcut olup olmadığını görmek için.

### <a name="a-microsoft-account-is-used"></a>Bir Microsoft hesabı kullanılır
Kullanıyorsanız bir **Microsoft hesabı** yerine **okul veya kuruluş** hesap, genel bir hata görürsünüz.  
![Bir Microsoft Account kullanılır](./media/tshoot-connect-connectivity/unknownerror.png)

### <a name="the-mfa-endpoint-cannot-be-reached"></a>MFA uç noktaya erişilemiyor
Bu hata görünür uç nokta **https://secure.aadcdn.microsoftonline-p.com** ulaşılamıyor ve genel Yöneticisi MFA etkin.  
![nomachineconfig](./media/tshoot-connect-connectivity/nomicrosoftonlinep.png)

* Bu hatayı görürseniz, doğrulama uç noktası **secure.aadcdn.microsoftonline p.com** Ara sunucuya eklendi.

### <a name="the-password-cannot-be-verified"></a>Parola doğrulanamıyor
Yükleme Sihirbazı'nı Azure AD'ye bağlanırken başarılı olduğu halde bu hatayı parola doğrulanamıyor varsa:  
![Hatalı parola.](./media/tshoot-connect-connectivity/badpassword.png)

* Parola geçici bir parola ve değiştirilmesi gerekiyor mu? Bu, gerçekten doğru parolayı mi? Oturum açmayı deneyen https://login.microsoftonline.com (başka bir Azure AD Connect sunucusundan bilgisayarda) ve hesabı kullanılabilir olduğunu doğrulayın.

### <a name="verify-proxy-connectivity"></a>Ara sunucu bağlantısını kontrol edin
Azure AD Connect sunucusu Ara sunucu ve Internet ile gerçek bağlantı olup olmadığını doğrulamak için proxy veya web isteklerine izin, görmek için bazı PowerShell kullanın. Bir PowerShell komut isteminde çalıştırmak `Invoke-WebRequest -Uri https://adminwebservice.microsoftonline.com/ProvisioningService.svc`. (Teknik ilk çağrı https://login.microsoftonline.com ve bu URI de çalışır, ancak diğer daha hızlı yanıt vermesini URI'dir.)

PowerShell yapılandırma proxy kişiye Machine.config dosyasına kullanır. Winhttp/Netsh ayarları, bu cmdlet'ler etkilememesi gerekir.

Proxy doğru şekilde yapılandırıldıysa, başarı durumu almanız gerekir: ![proxy200](./media/tshoot-connect-connectivity/invokewebrequest200.png)

Alırsanız **uzak sunucuya bağlanılamıyor**, daha sonra PowerShell kullanarak ara sunucu olmadan doğrudan çağrı yapmak çalışıyor veya DNS düzgün yapılandırılmamış. Emin **machine.config** dosyası doğru şekilde yapılandırılmış.
![unabletoconnect](./media/tshoot-connect-connectivity/invokewebrequestunable.png)

Proxy düzgün şekilde yapılandırılmadıysa, hata alırsınız: ![proxy200](./media/tshoot-connect-connectivity/invokewebrequest403.png)
![proxy407](./media/tshoot-connect-connectivity/invokewebrequest407.png)

| Hata | Hata metni | Açıklama |
| --- | --- | --- |
| 403 |Yasak |Proxy için istenen URL'yi açılmadı. Emin olun ve proxy yapılandırmasını yeniden ziyaret [URL'leri](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) açılmış olan. |
| 407 |Proxy kimlik doğrulaması gerekli |Bir oturum açma proxy sunucusu gerekliyse ve sağlanmadı. Ara sunucunuz kimlik doğrulaması gerektiriyorsa, bu ayar Machine.config dosyasına yapılandırılmış olduğundan emin olun. Ayrıca etki alanı hesapları için Sihirbazı çalıştıran kullanıcı ve hizmet hesabı kullandığınızdan emin olun. |

### <a name="proxy-idle-timeout-setting"></a>Proxy boşta kalma zaman aşımı ayarını
Azure AD, Azure AD Connect, Azure AD'ye dışarı aktarma isteği gönderdiğinde, yanıt oluşturmadan önce bir isteği işlemek için 5 dakika sürebilir. Özellikle, Grup nesneleri ile büyük grup üyeliklerini aynı dışarı aktarma isteğinde bir dizi varsa bu durum oluşabilir. Proxy boşta kalma zaman aşımı, 5 dakikadan fazla olacak şekilde yapılandırılmış emin olun. Aksi takdirde, aralıklı bağlantı sorunu Azure AD ile Azure AD Connect sunucusunda gözlemlenen.

## <a name="the-communication-pattern-between-azure-ad-connect-and-azure-ad"></a>Azure AD Connect ile Azure AD arasında iletişim deseni
Yukarıdaki adımları izlediyseniz ve yine de bağlanamıyorsanız, ağ günlükleri bakarak bu noktada başlayabilir. Bu bölümde, normal ve başarılı bağlantı deseni tanım sağlar. Bu listeleme Ayrıca ortak kırmızı herrings ağ günlükleri okurken, göz ardı edilebilir.

* Çağrıları vardır https://dc.services.visualstudio.com. Bu URL açık proxy'sinde yüklemesinin başarılı olması için gerekli değildir ve bu çağrıları yoksayılabilir.
* Dns çözümlemesi DNS ad alanı nsatc.net ve microsoftonline.com altında diğer ad olarak gerçek ana bilgisayar listeler görürsünüz. Ancak, gerçek sunucu adları herhangi bir web hizmeti isteklerinin değildir ve proxy sunucusuna bu URL'leri eklemek gerekmez.
* Uç noktaları adminwebservice ve provisioningapi bulma uç noktaları ve kullanmak için gerçek bir uç nokta bulmak için kullanılır. Bu uç noktaları, bölgenize bağlı olarak farklılık gösterir.

### <a name="reference-proxy-logs"></a>Başvuru Ara sunucu günlükleri
İşte bir döküm gerçek proxy günlüğünden ve Yükleme Sihirbazı sayfasından nereden alındığı (aynı uç noktasına yinelenen girişler kaldırılmıştır). Bu bölümde, kendi ara sunucu ve ağ günlükleri için referans olarak kullanılabilir. Gerçek bitiş ortamınızda farklı olabilir (özellikle bu URL'leri *italik*).

**Azure AD'ye Bağlanma**

| Zaman | URL'si |
| --- | --- |
| 1/11/2016 8:31 |Connect://Login.microsoftonline.com:443 |
| 1/11/2016 8:31 |Connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:32 |Bağlan: / /*bba800 bağlantı*. microsoftonline.com:443 |
| 1/11/2016 8:32 |Connect://Login.microsoftonline.com:443 |
| 1/11/2016 8:33 |connect://provisioningapi.microsoftonline.com:443 |
| 1/11/2016 8:33 |Bağlan: / /*bwsc02 geçiş*. microsoftonline.com:443 |

**Yapılandırma**

| Zaman | URL'si |
| --- | --- |
| 1/11/2016 8:43 |Connect://Login.microsoftonline.com:443 |
| 1/11/2016 8:43 |Bağlan: / /*bba800 bağlantı*. microsoftonline.com:443 |
| 1/11/2016 8:43 |Connect://Login.microsoftonline.com:443 |
| 1/11/2016 8:44 |Connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:44 |Bağlan: / /*bba900 bağlantı*. microsoftonline.com:443 |
| 1/11/2016 8:44 |Connect://Login.microsoftonline.com:443 |
| 1/11/2016 8:44 |Connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:44 |Bağlan: / /*bba800 bağlantı*. microsoftonline.com:443 |
| 1/11/2016 8:44 |Connect://Login.microsoftonline.com:443 |
| 1/11/2016 8:46 |connect://provisioningapi.microsoftonline.com:443 |
| 1/11/2016 8:46 |Bağlan: / /*bwsc02 geçiş*. microsoftonline.com:443 |

**İlk eşitleme**

| Zaman | URL'si |
| --- | --- |
| 1/11/2016 8:48 |Connect://Login.Windows.NET:443 |
| 1/11/2016 8:49 |Connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:49 |Bağlan: / /*bba900 bağlantı*. microsoftonline.com:443 |
| 1/11/2016 8:49 |Bağlan: / /*bba800 bağlantı*. microsoftonline.com:443 |

## <a name="authentication-errors"></a>Kimlik Doğrulama hataları
Bu bölüm, ADAL'ı (Azure AD Connect tarafından kullanılan kimlik doğrulama kitaplığı) ve PowerShell döndürülen hataları kapsar. Açıklanan hata size sonraki adımları anlamanıza yardımcı olmalıdır.

### <a name="invalid-grant"></a>Geçersiz verin
Geçersiz kullanıcı adı veya parola. Daha fazla bilgi için [parola doğrulanamıyor](#the-password-cannot-be-verified).

### <a name="unknown-user-type"></a>Bilinmeyen kullanıcı türü
Azure AD dizininizi bulunamadı veya çözümlendi. Belki de doğrulanmamış bir etki alanında bir kullanıcı adıyla oturum açmayı denemek ister misiniz?

### <a name="user-realm-discovery-failed"></a>Kullanıcı bölge bulma işlemi başarısız oldu
Ağ ya da proxy yapılandırma sorunları. Ağa erişilemiyor. Bkz: [Yükleme Sihirbazı'nda bağlantı sorunlarını giderme](#troubleshoot-connectivity-issues-in-the-installation-wizard).

### <a name="user-password-expired"></a>Kullanıcı parolasının süresi doldu
Kimlik bilgilerinizin süresi dolmuş. Parolanızı değiştirin.

### <a name="authorization-failure"></a>Yetkilendirme hatası
Azure AD'de eylemi gerçekleştirmek için kullanıcı yetkisi verilemedi.

### <a name="authentication-canceled"></a>Kimlik doğrulaması iptal edildi
Çok faktörlü kimlik doğrulaması (MFA) sınama iptal edildi.

<div id="connect-msolservice-failed">
<!--
  Empty div just to act as an alias for the "Connect To MS Online Failed" header
  because we used the mentioned id in the code to jump to this section.
-->
</div>

### <a name="connect-to-ms-online-failed"></a>Çevrimiçi başarısız MS ile bağlanma
Kimlik doğrulaması başarılı ancak Azure AD PowerShell bir kimlik doğrulama sorunu vardır.

<div id="get-msoluserrole-failed">
<!--
  Empty div just to act as an alias for the "Azure AD Global Admin Role Needed" header
  because we used the mentioned id in the code to jump to this section.
-->
</div>

### <a name="azure-ad-global-admin-role-needed"></a>Azure AD genel Yönetici rolüne gerekli
Kullanıcı başarıyla doğrulandı. Ancak kullanıcı genel yönetici rolü atanmaz. Bu [genel yönetici rolünü nasıl atayabilirsiniz](../users-groups-roles/directory-assign-admin-roles.md) kullanıcı. 

<div id="privileged-identity-management">
<!--
  Empty div just to act as an alias for the "Privileged Identity Management Enabled" header
  because we used the mentioned id in the code to jump to this section.
-->
</div>

### <a name="privileged-identity-management-enabled"></a>Privileged Identity Management etkin
Kimlik doğrulaması başarılı. Privileged Identity management etkinleştirildi ve şu anda bir genel Yönetici değilseniz. Daha fazla bilgi için [Privileged Identity Management](../privileged-identity-management/pim-getting-started.md).

<div id="get-msolcompanyinformation-failed">
<!--
  Empty div just to act as an alias for the "Company Information Unavailable" header
  because we used the mentioned id in the code to jump to this section.
-->
</div>

### <a name="company-information-unavailable"></a>Şirket bilgisi yok
Kimlik doğrulaması başarılı. Azure AD'den şirket bilgileri alınamadı.

<div id="get-msoldomain-failed">
<!--
  Empty div just to act as an alias for the "Domain Information Unavailable" header
  because we used the mentioned id in the code to jump to this section.
-->
</div>

### <a name="domain-information-unavailable"></a>Etki alanı bilgisi yok
Kimlik doğrulaması başarılı. Azure AD etki alanı bilgileri alınamadı.

### <a name="unspecified-authentication-failure"></a>Belirtilmeyen bir kimlik doğrulama hatası
Yükleme Sihirbazı'nda beklenmeyen bir hata olarak gösterilir. Kullanmayı denerseniz oluşabilir bir **Microsoft Account** yerine **okul veya kuruluş hesabı**.

## <a name="troubleshooting-steps-for-previous-releases"></a>Önceki sürümlerine yönelik sorun giderme adımları.
Sürümlerle 1.1.105.0 (Şubat 2016'da yayımlanan), derleme numarasıyla oturum açma Yardımcısı başlayarak devre dışı bırakılan. Bu bölümde, yapılandırma ve artık gerekli olmalıdır, ancak başvuru olarak tutulur.

Çoklu oturum için çalışmaya yardımcı olarak, winhttp yapılandırılması gerekir. Bu yapılandırma ile yapılabilir [ **netsh**](how-to-connect-install-prerequisites.md#connectivity).  
![Netsh](./media/tshoot-connect-connectivity/netsh.png)

### <a name="the-sign-in-assistant-has-not-been-correctly-configured"></a>Oturum Açma Yardımcısı'nı doğru şekilde yapılandırılmamış
Oturum Açma Yardımcısı proxy erişilemiyor veya proxy isteği izin vermiyor bu hata görüntülenir.
![nonetsh](./media/tshoot-connect-connectivity/nonetsh.png)

* Bu hatayı görürseniz, içindeki proxy yapılandırması bakmak [netsh](how-to-connect-install-prerequisites.md#connectivity) ve doğru doğrulayın.
  ![netshshow](./media/tshoot-connect-connectivity/netshshow.png)
* Hiçbir yanlışlık yoksa adımları [proxy bağlanabilirliği doğrulamak](#verify-proxy-connectivity) sorunu Sihirbazı mevcut olup olmadığını görmek için.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
