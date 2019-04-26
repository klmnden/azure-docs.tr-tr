---
title: Azure MFA NPS uzantısı - Azure Active Directory ile ilgili hata kodlarında sorun giderme
description: Azure multi-Factor Authentication için NPS uzantısı ile ilgili sorunları giderme konusunda yardım alın
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: f80ecf02a7e517300c41e84986659a66cfa11c90
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60414945"
---
# <a name="resolve-error-messages-from-the-nps-extension-for-azure-multi-factor-authentication"></a>Azure multi-Factor Authentication için NPS uzantısından alınan hata iletilerini çözme

Azure multi-Factor Authentication için NPS uzantısı ile hatalarla karşılaşırsanız, daha hızlı bir çözüm ulaşmak için bu makaleyi kullanın. NPS uzantı günlükleri altında Olay Görüntüleyicisi'nde bulunur **özel görünümler** > **sunucu rolleri** > **Ağ İlkesi ve Erişim Hizmetleri** üzerinde NPS uzantısı, yüklü olduğu sunucu.

## <a name="troubleshooting-steps-for-common-errors"></a>Sık karşılaşılan hatalar için sorun giderme adımları

| Hata kodu | Sorun giderme adımları |
| ---------- | --------------------- |
| **CONTACT_SUPPORT** | [Destek ekibiyle iletişime geçin](#contact-microsoft-support)ve günlüklerinin toplanması için adımlar listesi bahsedebilirsiniz. Kiracı kimliği ve kullanıcı asıl adı (UPN) dahil olmak üzere önce hata ne hakkında mümkün olduğunca fazla bilgi sağlar. |
| **CLIENT_CERT_INSTALL_ERROR** | İstemci sertifikası yüklü veya kiracınız ile ilişkili nasıl bir sorun olabilir. Bölümündeki yönergeleri [MFA NPS uzantısı sorun giderme](howto-mfa-nps-extension.md#troubleshooting) istemci sertifikası sorunları araştırmak için. |
| **ESTS_TOKEN_ERROR** | Bölümündeki yönergeleri [MFA NPS uzantısı sorun giderme](howto-mfa-nps-extension.md#troubleshooting) araştırmaya sorun istemci sertifikası ve ADAL belirteç. |
| **HTTPS_COMMUNICATION_ERROR** | Azure mfa'yı yanıtları almak NPS sunucusunu değiştiremiyor. Güvenlik duvarlarınızdan ve ondan trafik için açık çift yönlü olduğunu doğrulayın https://adnotifications.windowsazure.com |
| **HTTP_CONNECT_ERROR** | NPS uzantısını çalıştıran sunucuda, size ulaşabildiğimizden doğrulayın https://adnotifications.windowsazure.com ve https://login.microsoftonline.com/. Bu siteler yükleme, o sunucudaki bağlantı sorunlarını giderme. |
| **Azure mfa NPS uzantısı:** <br> Azure mfa NPS uzantısı yalnızca RADIUS isteklerini AccessAccept durumundaki ikincil kimlik doğrulaması gerçekleştirir. İstek yoksayılıyor AccessReject, yanıt durumu ile kullanıcının kullanıcı adı için alınan istek. | Bu hata, genellikle bir kimlik doğrulama hatası AD'de yansıtır veya Azure AD'den yanıtlar almasına NPS sunucusunu alamıyor. Güvenlik duvarlarınızdan ve ondan trafik için açık çift yönlü olduğunu doğrulayın https://adnotifications.windowsazure.com ve https://login.microsoftonline.com 80 ve 443 bağlantı noktalarını kullanarak. Ağ erişim izinleri arama Giriş sekmesinde "NPS Ağ İlkesi aracılığıyla erişimi denetlemek için" ayarının olduğunu denetlemek önemlidir. |
| **REGISTRY_CONFIG_ERROR** | Kayıt defteri kaynaklanıyor olabilir uygulamanın bir anahtar eksik [PowerShell Betiği](howto-mfa-nps-extension.md#install-the-nps-extension) yüklendikten sonra Çalıştır değildi. Hata iletisi, eksik bir anahtar içermelidir. HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureMfa anahtarında olduğundan emin olun. |
| **REQUEST_FORMAT_ERROR** <br> RADIUS isteğini zorunlu RADIUS userName\Identifier özniteliği eksik. NPS RADIUS isteklerini aldığını doğrulayın. | Bu hata genellikle bir yükleme sorunu yansıtır. RADIUS isteklerini alabileceğini NPS sunucuları NPS uzantısı'nın yüklü olması gerekir. RDG ve RRAS gibi hizmetler için bağımlılık olarak yüklenen NPS sunucuları, RADIUS isteklerini alacak yok. NPS uzantısı, kimlik doğrulama isteğini ayrıntıları okunamıyor olduğundan bu tür yüklemeleri ve hataları üzerinden yüklendiğinde çalışmaz. |
| **REQUEST_MISSING_CODE** | Parola şifreleme protokolünü NPS ve NAS sunucuları arasında kullanmakta olduğunuz ikincil kimlik doğrulama yöntemini desteklediğinden emin olun. **PAP** bulutta Azure MFA'ın tüm kimlik doğrulama yöntemleri destekler: telefon araması, tek yönlü SMS mesajı, mobil uygulama bildirimi ve mobil uygulama doğrulama kodu. **CHAPV2** ve **EAP** telefon araması ve mobil uygulama bildirimi destekler. |
| **USERNAME_CANONICALIZATION_ERROR** | Kullanıcının şirket içi Active Directory Örneğinizde mevcut olduğunu ve NPS hizmetini dizine erişim izni olduğunu doğrulayın. Ormanlar arası güven kullanıyorsanız [desteğe](#contact-microsoft-support) daha fazla yardım için. |

### <a name="alternate-login-id-errors"></a>Alternatif bir oturum açma kimliği hataları

| Hata kodu | Hata iletisi | Sorun giderme adımları |
| ---------- | ------------- | --------------------- |
| **ALTERNATE_LOGIN_ID_ERROR** | Hata: userObjectSid arama başarısız oldu | Kullanıcının şirket içi Active Directory Örneğinizde var olduğunu doğrulayın. Ormanlar arası güven kullanıyorsanız [desteğe](#contact-microsoft-support) daha fazla yardım için. |
| **ALTERNATE_LOGIN_ID_ERROR** | Hata: Alternatif Loginıd arama başarısız oldu | LDAP_ALTERNATE_LOGINID_ATTRIBUTE değerine ayarlandığını doğrulayın bir [geçerli active directory özniteliğini](https://msdn.microsoft.com/library/ms675090(v=vs.85).aspx). <br><br> LDAP_FORCE_GLOBAL_CATALOG True olarak ayarlandığında veya LDAP_LOOKUP_FORESTS boş olmayan bir değer ile yapılandırılmışsa, bir genel katalog yapılandırdıktan ve AlternateLoginId öznitelik kendisine eklenir doğrulayın. <br><br> LDAP_LOOKUP_FORESTS boş olmayan bir değer ile yapılandırılmışsa, değerin doğru olduğunu doğrulayın. Birden fazla orman adı varsa, adları noktalı virgül, boşluk ile ayrılması gerekir. <br><br> Bu adımları, sorunu yoksa, [desteğe](#contact-microsoft-support) daha fazla yardım için. |
| **ALTERNATE_LOGIN_ID_ERROR** | Hata: Alternatif Loginıd değeri boştur | AlternateLoginId öznitelik kullanıcı için yapılandırılmış olduğunu doğrulayın. |

## <a name="errors-your-users-may-encounter"></a>Kullanıcılarınızın hatalarla karşılaşabilirsiniz

| Hata kodu | Hata iletisi | Sorun giderme adımları |
| ---------- | ------------- | --------------------- |
| **AccessDenied** | Çağırana Kiracı Kullanıcı için kimlik doğrulaması yapmak için erişim izni yok | Kiracı etki alanı ve etki alanı kullanıcı asıl adı (UPN) aynı olup olmadığını denetleyin. Örneğin, emin user@contoso.com Contoso Kiracı kimlik doğrulaması çalışıyor. UPN, geçerli bir kullanıcı Azure kiracısı için temsil eder. |
| **AuthenticationMethodNotConfigured** | Belirtilen kimlik doğrulama yöntemi kullanıcı için yapılandırılmamış | Kullanıcı ekleme veya yönergelere göre kendi doğrulama yöntemlerini doğrulama sahip [iki adımlı doğrulama ayarlarınızı yönetme](../user-help/multi-factor-authentication-end-user-manage-settings.md). |
| **AuthenticationMethodNotSupported** | Belirtilen kimlik doğrulama yöntemi desteklenmiyor. | Bu hataya dahil etmek, günlük toplama ve [desteğe](#contact-microsoft-support). Destek ekibiyle iletişime geçtiğinizde, kullanıcı adı ve hatayı tetikleyen ikincil doğrulama yöntemini belirtin. |
| **BecAccessDenied** | MSODS'un Bec çağrısı erişim reddedildi döndürdü, büyük olasılıkla kullanıcı kiracıda tanımlı değil | Kullanıcının şirket içinde Active Directory var, ancak Azure AD tarafından AD Connect ile eşitlenmedi. Veya kullanıcının kiracısı için eksik. Azure AD'ye kullanıcı ekleme ve bunların yönergelere göre kendi doğrulama yöntemlerini ekleyin [iki adımlı doğrulama ayarlarınızı yönetme](../user-help/multi-factor-authentication-end-user-manage-settings.md). |
| **InvalidFormat** veya **StrongAuthenticationServiceInvalidParameter** | Telefon numarası tanınmayan bir biçimde ' dir. | Kullanıcı doğrulama telefon numaralarına düzeltmek sahip. |
| **InvalidSession** | Belirtilen oturum geçersiz veya süresi dolmuş olabilir | Oturum üç tamamlanması dakikadan uzun sürdü. Kullanıcı doğrulama kodunu girerek veya uygulama bildirimi, kimlik doğrulama isteğini başlatmak, üç dakika içinde yanıt olduğunu doğrulayın. Bu sorunu çözmezse, istemci, NAS sunucu, NPS sunucusunu ve Azure mfa'yı uç nokta arasında hiçbir ağ gecikme süresi olup olmadığını denetleyin.  |
| **Nodefaultauthenticationmethodısconfigured** | Kullanıcı için yapılandırılmış hiçbir varsayılan kimlik doğrulama yöntemi | Kullanıcı ekleme veya yönergelere göre kendi doğrulama yöntemlerini doğrulama sahip [iki adımlı doğrulama ayarlarınızı yönetme](../user-help/multi-factor-authentication-end-user-manage-settings.md). Kullanıcının varsayılan kimlik doğrulama yöntemini seçmiş ve bu yöntem, hesap için yapılandırılmış olduğunu doğrulayın. |
| **OathCodePinIncorrect** | Yanlış kod ve PIN girildi. | Bu hata, NPS uzantısı'nda beklenmiyor. Bu, kullanıcınızın karşılaşırsa, [desteğe](#contact-microsoft-support) sorun giderme Yardımı. |
| **ProofDataNotFound** | Kavram veri belirtilen kimlik doğrulama yöntemi için yapılandırılmadı. | Farklı bir doğrulama yöntemi deneyin veya yeni bir doğrulama yöntemlerini yönergeleri göre ekleyin kullanıcının [iki adımlı doğrulama ayarlarınızı yönetme](../user-help/multi-factor-authentication-end-user-manage-settings.md). Kullanıcı, kendi doğrulama yöntemini doğru şekilde ayarlandığını onaylandıktan sonra bu hatayı görmeye devam ederse [desteğe](#contact-microsoft-support). |
| **SMSAuthFailedWrongCodePinEntered** | Yanlış kod ve PIN girildi. (OneWaySMS) | Bu hata, NPS uzantısı'nda beklenmiyor. Bu, kullanıcınızın karşılaşırsa, [desteğe](#contact-microsoft-support) sorun giderme Yardımı. |
| **TenantIsBlocked** | Kiracı engelleniyor | [Destek ekibiyle iletişime geçin](#contact-microsoft-support) Azure portalında Azure AD özellikler sayfasından Directory Kimliğine sahip. |
| **UserNotFound** | Belirtilen kullanıcı bulunamadı | Kiracı, artık Azure AD'de etkin olarak görülebilir. Aboneliğinizin etkin olduğunu ve gerekli olan uygulamalar'birinci taraf. Ayrıca, sertifika konusu kiracıda beklendiği gibi sertifikası hala geçerli ve hizmet sorumlusu altında kayıtlı olduğundan emin olun. |

## <a name="messages-your-users-may-encounter-that-arent-errors"></a>Kullanıcılarınızın, karşılaşabileceğiniz iletileri olmayan hataları

Bazı durumlarda, kendi kimlik doğrulama isteği başarısız olduğundan, kullanıcılarınızın çok faktörlü kimlik doğrulamasını iletileri alabilirsiniz. Bu ürünün yapılandırma hataları değildir, ancak neden bir kimlik doğrulama isteği reddedildi kasıtlı uyarıları açıklayan.

| Hata kodu | Hata iletisi | Önerilen adımlar | 
| ---------- | ------------- | ----------------- |
| **OathCodeIncorrect** | Yanlış kod entered\OATH kodu yanlış | Kullanıcı yanlış kod girildi. Yeni bir kod istiyor ya da yeniden oturum açmayı yeniden deneyin sağlayın. | 
| **SMSAuthFailedMaxAllowedCodeRetryReached** | İzin verilen maksimum kod yeniden deneme üst sınırına | Kullanıcı doğrulama sınaması, çok fazla kez başarısız oldu. Ayarlarınıza bağlı olarak, bunlar bir yönetici tarafından artık engeli kaldırılmış gerekebilir.  |
| **SMSAuthFailedWrongCodeEntered** | Girilen/metin iletisi OTP yanlış yanlış kod | Kullanıcı yanlış kod girildi. Yeni bir kod istiyor ya da yeniden oturum açmayı yeniden deneyin sağlayın. |

## <a name="errors-that-require-support"></a>Destek gerektiren hataları

Şu hatalardan biriyle karşılaşırsanız olmasını öneririz, [desteğe](#contact-microsoft-support) tanılama konusunda yardım almak için. Standart ayarlanmış bu hataları gidermek adımları yok. Destek ekibiyle iletişime geçtiğinizde, bir hataya götüren adımlar hakkında mümkün olduğunca fazla bilgi ve Kiracı bilgilerinizi eklediğinizden emin olun.

| Hata kodu | Hata iletisi |
| ---------- | ------------- |
| **InvalidParameter** | İsteği null olmamalıdır |
| **InvalidParameter** | Nesne kimliği null veya boş ReplicationScope olmamalıdır:{0} |
| **InvalidParameter** | Şirket adı uzunluğunu \{0} \ olan izin verilen uzunluk üst sınırı aşıyor {1} |
| **InvalidParameter** | UserPrincipalName null veya boş olmamalıdır |
| **InvalidParameter** | Sağlanan Tenantıd doğru biçimde değil. |
| **InvalidParameter** | Oturum kimliği null veya boş olmamalıdır |
| **InvalidParameter** | İstek herhangi ProofData veya Msods'un çözümlenemedi. ProofData bilinmiyor |
| **Internalerror** |  |
| **OathCodePinIncorrect** |  |
| **VersionNotSupported** |  |
| **MFAPinNotSetup** |  |

## <a name="next-steps"></a>Sonraki adımlar

### <a name="troubleshoot-user-accounts"></a>Kullanıcı hesaplarıyla ilgili sorunları giderme

Kullanıcılarınız varsa [iki aşamalı doğrulama konusunda sorun mu yaşıyorsunuz](../user-help/multi-factor-authentication-end-user-troubleshoot.md), bunları kendi kendine tanılama sorunlarını yardımcı olur.

### <a name="contact-microsoft-support"></a>Microsoft desteğine başvurun

Ek yardıma ihtiyacınız olursa aracılığıyla destek uzmanına başvurun [Azure multi-Factor Authentication sunucusu desteği](https://support.microsoft.com/oas/default.aspx?prid=14947). Mümkün olduğunca sorununuzla ilgili kadar bilgi dahil ederseniz bizimle iletişime geçtiğiniz yararlı olur. Belirtebilirsiniz bilgiler içeren hata, özel hata kodu nerede gördüğünüzü sayfasında belirli bir oturum kimliği, gördüğünüz hata ve hata ayıklama günlüklerini kullanıcının kimliği.

Hata ayıklama Desteği Tanılama günlüklerini toplamak için NPS sunucusunda aşağıdaki adımları kullanın:

1. Kayıt Defteri Düzenleyicisi'ni açın ve Gözat HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureMfa kümesine **VERBOSE_LOG** için **TRUE**
2. Bir yönetici komut istemi açın ve şu komutları çalıştırın:

   ```
   Mkdir c:\NPS
   Cd NPS
   netsh trace start Scenario=NetConnection capture=yes tracefile=c:\NPS\nettrace.etl
   logman create trace "NPSExtension" -ow -o c:\NPS\NPSExtension.etl -p {7237ED00-E119-430B-AB0F-C63360C8EE81} 0xffffffffffffffff 0xff -nb 16 16 -bs 1024 -mode Circular -f bincirc -max 4096 -ets
   logman update trace "NPSExtension" -p {EC2E6D3A-C958-4C76-8EA4-0262520886FF} 0xffffffffffffffff 0xff -ets
   ```

3. Sorunu yeniden oluşturun

4. Şu komutlarla izlemeyi durdurun:

   ```
   logman stop "NPSExtension" -ets
   netsh trace stop
   wevtutil epl AuthNOptCh C:\NPS\%computername%_AuthNOptCh.evtx
   wevtutil epl AuthZOptCh C:\NPS\%computername%_AuthZOptCh.evtx
   wevtutil epl AuthZAdminCh C:\NPS\%computername%_AuthZAdminCh.evtx
   Start .
   ```

5. Kayıt Defteri Düzenleyicisi'ni açın ve Gözat HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureMfa kümesine **VERBOSE_LOG** için **FALSE**
6. Zip C:\NPS klasörünün içeriğini ve sıkıştırılmış dosya için destek talebi ekleyin.
