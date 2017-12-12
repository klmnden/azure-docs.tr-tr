---
title: "Azure AD Connect: bağlantı sorunlarını giderme | Microsoft Docs"
description: "Azure AD Connect ile bağlantı sorunlarının nasıl giderileceği açıklanmaktadır."
services: active-directory
documentationcenter: 
author: andkjell
manager: mtillman
editor: 
ms.assetid: 3aa41bb5-6fcb-49da-9747-e7a3bd780e64
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 09e1858c748c50a084cd66ac8bc8406180d97ace
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="troubleshoot-connectivity-issues-with-azure-ad-connect"></a>Azure AD Connect ile ilgili bağlantı sorunlarını giderme
Bu makalede, Azure AD Connect ve Azure AD arasında bağlantı nasıl çalıştığını ve bağlantı sorunlarının nasıl giderileceği açıklanmaktadır. Bu sorunları bir proxy sunucusu olan bir ortamda görüntülenmesine olasılığı daha yüksektir.

## <a name="troubleshoot-connectivity-issues-in-the-installation-wizard"></a>Yükleme Sihirbazı'nda bağlantı sorunlarını giderme
Azure AD Connect (ADAL kitaplığını kullanarak) Modern kimlik doğrulaması için kimlik doğrulaması kullanıyor. Yükleme Sihirbazı'nı ve uygun eşitleme altyapısı machine.config bu iki .NET uygulamaları olduğundan doğru şekilde yapılandırılmış olması gerekir.

Bu makalede, Fabrikam proxy'si aracılığıyla Azure ad nasıl bağlanacağını gösterir. Proxy sunucusu fabrikamproxy olarak adlandırılır ve 8080 bağlantı noktası kullanma.

İlk emin olmak ihtiyacımız [ **machine.config** ](active-directory-aadconnect-prerequisites.md#connectivity) doğru yapılandırılmış.  
![machineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/machineconfig.png)

> [!NOTE]
> Değişiklikler miiserver.exe.config için bunun yerine yapılmalıdır, bazı Microsoft dışı bloglar, belgelenmiştir. Ancak, bu dosyanın ilk yükleme sırasında çalışırsa, sistem ilk yükseltme çalışmayı durduruyor da her yükseltmede üzerine yazılır. Bu nedenle, bunun yerine machine.config güncelleştirmek için önerilir.
>
>

Proxy sunucusu ayrıca açılan gerekli URL'leri sahip olmalıdır. Resmi listesi belgelenen [Office 365 URL'leri ve IP adresi aralıkları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

Bu URL'leri, aşağıdaki tabloda Azure AD ile hiç bağlanabilmesi için mutlak tam gereksinimdir. Bu liste, parola geri yazma veya Azure AD Connect Health gibi tüm isteğe bağlı özellikleri içermez. Burada, ilk yapılandırmasını gidermenize yardımcı olması için belgelenmiştir.

| URL | Bağlantı noktası | Açıklama |
| --- | --- | --- |
| mscrl.microsoft.com |HTTP/80 |CRL listelerini indirmek için kullanılır. |
| \*. verisign.com |HTTP/80 |CRL listelerini indirmek için kullanılır. |
| \*. entrust.com |HTTP/80 |MFA için CRL listelerini indirmek için kullanılır. |
| \*.windows.net |HTTPS/443 |Azure AD ile oturum açmak için kullanılır. |
| Secure.aadcdn.microsoftonline p.com |HTTPS/443 |MFA için kullanılır. |
| \*.microsoftonline.com |HTTPS/443 |Azure AD dizininizi yapılandırmak ve verileri içeri/dışarı aktarma için kullanılır. |

## <a name="errors-in-the-wizard"></a>Sihirbazdaki hataları
Yükleme Sihirbazı'nı iki farklı güvenlik kapsamları kullanıyor. Sayfasında **Azure ad Connect**, şu anda oturum açmış olan kullanıcının kullanıyor. Sayfasında **yapılandırma**, için değiştirme [eşitleme altyapısı için hizmetini çalıştıran hesabı](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-account). Bir sorun varsa, büyük olasılıkla zaten göründüğü **Azure ad Connect** proxy yapılandırmasını genel olduğundan sihirbaz sayfasında.

Yükleme Sihirbazı'nda karşılaştığınız en yaygın hataları sorunlardır.

### <a name="the-installation-wizard-has-not-been-correctly-configured"></a>Yükleme Sihirbazı'nı doğru şekilde yapılandırılmadı
Sihirbaz proxy ulaştığında bu hata görünür.  
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomachineconfig.png)

* Bu hatayı görürseniz, doğrulayın [machine.config](active-directory-aadconnect-prerequisites.md#connectivity) doğru şekilde yapılandırıldı.
* Doğru görünüyorsa, adımları [proxy bağlanabilirliği doğrulamak](#verify-proxy-connectivity) sorunu Sihirbazı dışında mevcut olup olmadığını görmek için.

### <a name="a-microsoft-account-is-used"></a>Bir Microsoft hesabı kullanılır
Kullanırsanız, bir **Microsoft hesabı** yerine **okul veya kuruluş** hesap, genel bir hata görürsünüz.  
![Bir Microsoft Account kullanılır](./media/active-directory-aadconnect-troubleshoot-connectivity/unknownerror.png)

### <a name="the-mfa-endpoint-cannot-be-reached"></a>MFA uç noktaya erişilemiyor
Bu hata görünür uç nokta **https://secure.aadcdn.microsoftonline-p.com** ulaşılamıyor ve MFA etkinse, genel yönetici sahiptir.  
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomicrosoftonlinep.png)

* Bu hatayı görürseniz, doğrulayın uç nokta **secure.aadcdn.microsoftonline p.com** proxy'sine eklendi.

### <a name="the-password-cannot-be-verified"></a>Parola doğrulanamıyor.
Yükleme Sihirbazı'nı Azure AD'ye bağlanma başarılı olur, ancak bu hatayı görmek parola doğrulanamıyor ise:  
![badpassword](./media/active-directory-aadconnect-troubleshoot-connectivity/badpassword.png)

* Geçici bir parola paroladır ve değiştirilmesi gerekiyor? Gerçekte doğru parolayı mi? Deneyin https://login.microsoftonline.com (bilgisayarda başka bir Azure AD Connect sunucusu) oturum açın ve hesabı kullanılabilir olduğunu doğrulayın.

### <a name="verify-proxy-connectivity"></a>Proxy bağlanabildiğini doğrulayın
Azure AD Connect sunucusu proxy'si ve Internet ile gerçek bağlantı olup olmadığını doğrulamak için bazı PowerShell veya proxy web isteklerine izin olmadığını görmek için kullanın. Bir PowerShell komut isteminde çalıştırmak `Invoke-WebRequest -Uri https://adminwebservice.microsoftonline.com/ProvisioningService.svc`. (Teknik olarak ilk çağrı için https://login.microsoftonline.com bağlıdır ve bu URI de çalışır, ancak diğer daha hızlı yanıt URI'dir.)

PowerShell proxy kişiye machine.config içinde yapılandırmasını kullanır. Winhttp/netsh ayarlarında bu cmdlet'leri etkisi değil.

Proxy doğru şekilde yapılandırıldıysa, başarı durumunu almanız gerekir: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest200.png)

Alırsanız **uzak sunucuya bağlanılamıyor**, proxy kullanmadan doğrudan arama yapmak PowerShell sonra çalışıyor veya DNS düzgün yapılandırılmamış. Emin olun **machine.config** dosyası doğru şekilde yapılandırılmış.
![unabletoconnect](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequestunable.png)

Proxy doğru yapılandırılmamışsa, bir hata alıyorsunuz: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest403.png)
![proxy407](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest407.png)

| Hata | Hata metni | Yorum |
| --- | --- | --- |
| 403 |Yasak |İstenen URL için proxy açılmadı. Proxy yapılandırmasını tekrar ziyaret edip emin olun [URL'leri](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) açılmış. |
| 407 |Proxy kimlik doğrulaması gerekli |Proxy sunucu bir oturum açma gerekli ve sağlanmadı. Proxy sunucusu kimlik doğrulaması gerektiriyorsa, bu ayar machine.config içinde yapılandırılmış emin olun. Ayrıca Sihirbazı çalıştıran kullanıcı ve hizmet hesabı için etki alanı hesapları kullandığınızdan emin olun. |

### <a name="proxy-idle-timeout-setting"></a>Proxy boşta kalma zaman aşımı ayarı
Azure AD Connect için Azure AD dışarı aktarma isteği gönderdiğinde, Azure AD yanıt oluşturmadan önce isteğini işlemek için 5 dakika sürebilir. Özellikle büyük grup üyelikleri aynı verme isteğine dahil olan grup nesne sayısı varsa bu durum oluşabilir. 5 dakikadan fazla olacak şekilde yapılandırılmış bir Proxy boşta kalma zaman aşımı olun. Aksi takdirde, aralıklı bağlantı sorunu Azure AD ile Azure AD Connect sunucuda gözlenen.

## <a name="the-communication-pattern-between-azure-ad-connect-and-azure-ad"></a>Azure AD Connect ve Azure AD arasındaki iletişim düzeni
Bu önceki adımları izlediğinizde ve yine bağlanamıyorsanız, ağ günlüklerine arayan bu noktada başlayabilir. Bu bölümde, normal ve başarılı bağlantısı düzeni belgeleme. Ayrıca ağ günlükleri okunurken, göz ardı edilebilir ortak kırmızı herrings listeleme.

* Https://dc.services.visualstudio.com çağrıları vardır. Bu URL Aç başarılı olması için yükleme için proxy sağlamak için gerekli değildir ve bu çağrıları göz ardı edilebilir.
* Dns çözümlemesi DNS ad alanı nsatc.net ve diğer ad microsoftonline.com altında olduğu gerçek barındıran listeler bakın. Ancak, gerçek sunucu adları tüm web hizmeti isteklerinin değildir ve bu URL'leri proxy sunucusuna eklemek zorunda değilsiniz.
* Uç noktaları adminwebservice ve provisioningapi bulma uç noktaları ve kullanmak için gerçek uç noktası bulmak için kullanılır. Bu uç noktalar bölgenizdeki bağlı olarak farklılık gösterir.

### <a name="reference-proxy-logs"></a>Başvuru proxy günlükleri
İşte gerçek proxy günlük bir dökümü ve Yükleme Sihirbazı'nı sayfasından, burada alındığı (aynı uç yinelenen girişler kaldırılmıştır). Bu bölümde, kendi proxy ve ağ günlükleri için bir başvuru olarak kullanılabilir. Gerçek bitiş noktaları ortamınızda farklı olabilir (özellikle bu URL'leri *italik*).

**Azure AD'ye bağlanma**

| Zaman | URL |
| --- | --- |
| 1/11/2016 8:31 |Connect://Login.microsoftonline.com:443 |
| 1/11/2016 8:31 |Connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:32 |Bağlan: / /*bba800 bağlantı*. microsoftonline.com:443 |
| 1/11/2016 8:32 |Connect://Login.microsoftonline.com:443 |
| 1/11/2016 8:33 |Connect://provisioningapi.microsoftonline.com:443 |
| 1/11/2016 8:33 |Bağlan: / /*bwsc02 geçiş*. microsoftonline.com:443 |

**Yapılandırma**

| Zaman | URL |
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
| 1/11/2016 8:46 |Connect://provisioningapi.microsoftonline.com:443 |
| 1/11/2016 8:46 |Bağlan: / /*bwsc02 geçiş*. microsoftonline.com:443 |

**İlk eşitleme**

| Zaman | URL |
| --- | --- |
| 1/11/2016 8:48 |Connect://Login.Windows.NET:443 |
| 1/11/2016 8:49 |Connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:49 |Bağlan: / /*bba900 bağlantı*. microsoftonline.com:443 |
| 1/11/2016 8:49 |Bağlan: / /*bba800 bağlantı*. microsoftonline.com:443 |

## <a name="authentication-errors"></a>Kimlik doğrulama hataları
Bu bölüm, ADAL (Azure AD Connect tarafından kullanılan kimlik doğrulama kitaplığı) ve PowerShell döndürülen hatalar kapsar. Açıklanan hata, sonraki adımlarda anlamanıza yardımcı olması.

### <a name="invalid-grant"></a>Geçersiz verin
Geçersiz kullanıcı adı veya parola. Daha fazla bilgi için bkz: [parola doğrulanamıyor](#the-password-cannot-be-verified).

### <a name="unknown-user-type"></a>Bilinmeyen kullanıcı türü
Azure AD dizininizi bulunamadı veya çözümlendi. Belki de doğrulanmamış bir etki alanında bir kullanıcı adıyla oturum açmayı deneyin?

### <a name="user-realm-discovery-failed"></a>Kullanıcı bölgesi bulma başarısız oldu
Ağ veya proxy yapılandırma sorunları. Ağ erişilemiyor. Bkz: [Yükleme Sihirbazı'nda bağlantı sorunlarını giderme](#troubleshoot-connectivity-issues-in-the-installation-wizard).

### <a name="user-password-expired"></a>Kullanıcı parolasının süresi
Kimlik bilgilerinizin süresi doldu. Parolanızı değiştirin.

### <a name="authorizationfailure"></a>AuthorizationFailure
Bilinmeyen sorun.

### <a name="authentication-cancelled"></a>Kimlik doğrulama iptal edildi
Çok faktörlü kimlik doğrulaması (MFA) sınama iptal edildi.

### <a name="connecttomsonline"></a>ConnectToMSOnline
Kimlik doğrulaması başarılı ancak bir kimlik doğrulama sorunu Azure AD PowerShell sahiptir.

### <a name="azurerolemissing"></a>AzureRoleMissing
Kimlik doğrulaması başarılı oldu. Genel yönetici değildir.

### <a name="privilegedidentitymanagement"></a>PrivilegedIdentityManagement
Kimlik doğrulaması başarılı oldu. Privileged Identity management etkinleştirildi ve şu anda bir genel yönetici değildir. Daha fazla bilgi için bkz: [Privileged Identity Management](../active-directory-privileged-identity-management-getting-started.md).

### <a name="companyinfounavailable"></a>CompanyInfoUnavailable
Kimlik doğrulaması başarılı oldu. Şirket bilgileri, Azure AD'den alınamadı.

### <a name="retrievedomains"></a>RetrieveDomains
Kimlik doğrulaması başarılı oldu. Azure AD etki alanı bilgileri alınamadı.

### <a name="unexpected-exception"></a>Beklenmeyen özel durum
Yükleme Sihirbazı'nda beklenmeyen bir hata olarak gösterilir. Kullanmaya çalıştığınızda oluşabilir bir **Microsoft Account** yerine **okul veya kuruluş hesabı**.

## <a name="troubleshooting-steps-for-previous-releases"></a>Önceki sürümler için sorun giderme adımları.
Sürümleriyle yapı numarası 1.1.105.0 (Şubat 2016 yayımlandı), oturum açma Yardımcısı'nı başlatmadan devre dışı bırakılan. Bu bölümde ve yapılandırmasını artık gerekli olması gerekir, ancak başvuru olarak tutulur.

Çoklu oturum için çalışmasına yardımcı olarak, winhttp yapılandırılması gerekir. Bu yapılandırma ile yapılabilir [ **netsh**](active-directory-aadconnect-prerequisites.md#connectivity).  
![Netsh](./media/active-directory-aadconnect-troubleshoot-connectivity/netsh.png)

### <a name="the-sign-in-assistant-has-not-been-correctly-configured"></a>Oturum Açma Yardımcısı'nı doğru şekilde yapılandırılmadı
Bu hata oturum açma Yardımcısı proxy ulaşamıyor ya da proxy izin vermiyor. istek olduğunda görüntülenir.
![nonetsh](./media/active-directory-aadconnect-troubleshoot-connectivity/nonetsh.png)

* Bu hatayı görürseniz, içindeki proxy yapılandırması bakmak [netsh](active-directory-aadconnect-prerequisites.md#connectivity) ve doğru doğrulayın.
  ![netshshow](./media/active-directory-aadconnect-troubleshoot-connectivity/netshshow.png)
* Doğru görünüyorsa, adımları [proxy bağlanabilirliği doğrulamak](#verify-proxy-connectivity) sorunu Sihirbazı dışında mevcut olup olmadığını görmek için.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.
