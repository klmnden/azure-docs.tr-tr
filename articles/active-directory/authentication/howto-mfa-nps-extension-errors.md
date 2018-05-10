---
title: Azure MFA NPS uzantısı Hata kodlarında sorun giderme | Microsoft Docs
description: Azure çok faktörlü kimlik doğrulaması için NPS uzantılı sorunlarını çözme hakkında yardım alın
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 07/14/2017
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.openlocfilehash: aa140bceb5f7ad5e638f747fa8d88803c27f02a3
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="resolve-error-messages-from-the-nps-extension-for-azure-multi-factor-authentication"></a>Azure çok faktörlü kimlik doğrulaması için hata iletileri NPS uzantı çözümleyin

Azure çok faktörlü kimlik doğrulaması için NPS uzantısı hatalarla karşılaşırsanız, daha hızlı bir çözüm ulaşmak için bu makaleyi kullanın. 

## <a name="troubleshooting-steps-for-common-errors"></a>Sık karşılaşılan hataları için sorun giderme adımları

| Hata kodu | Sorun giderme adımları |
| ---------- | --------------------- |
| **CONTACT_SUPPORT** | [Desteğe başvurun](#contact-microsoft-support)ve günlüklerinin toplanması için adımlar listesi Bahsediyor. Kiracı kimliği ve kullanıcı asıl adı (UPN) dahil olmak üzere hata önce ne hakkında mümkün olduğu kadar bilgi sağlayın. |
| **CLIENT_CERT_INSTALL_ERROR** | İstemci sertifikası yüklü veya Kiracı ile ilişkilendirilen nasıl bir sorun olabilir. ' Ndaki yönergeleri izleyin [MFA NPS uzantı sorunlarını giderme](howto-mfa-nps-extension.md#troubleshooting) istemci sertifika sorunları araştırmak için. |
| **ESTS_TOKEN_ERROR** | ' Ndaki yönergeleri izleyin [MFA NPS uzantı sorunlarını giderme](howto-mfa-nps-extension.md#troubleshooting) araştırmak için istemci sertifikası ve ADAL sorunları belirteci. |
| **HTTPS_COMMUNICATION_ERROR** | Azure MFA yanıtları almak NPS sunucusunu alamıyor. Güvenlik duvarlarında ve bu sunucudan giden trafiği için açık çift yönlü olduğunu doğrulayın https://adnotifications.windowsazure.com |
| **HTTP_CONNECT_ERROR** | NPS uzantısı çalıştıran sunucuda, size ulaşabildiğimizden doğrula https://adnotifications.windowsazure.com ve https://login.microsoftonline.com/. Bu siteleri yükleme, bu sunucu üzerinde bağlantı sorunlarını giderme. |
| **REGISTRY_CONFIG_ERROR** | Kayıt defteri olabilir uygulama için bir anahtarı eksik [PowerShell Betiği](howto-mfa-nps-extension.md#install-the-nps-extension) yüklendikten sonra Çalıştır değildi. Hata iletisi eksik anahtar içermelidir. Anahtarın HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureMfa altında olduğundan emin olun. |
| **REQUEST_FORMAT_ERROR** <br> RADIUS isteği zorunlu RADIUS userName\Identifier özniteliği eksik. NPS RADIUS isteklerini aldığını doğrulayın | Bu hata genellikle bir yükleme sorunu yansıtır. NPS uzantısı RADIUS isteklerini alacak NPS sunucularda yüklü olması gerekir. RDG ve RRAS gibi hizmetler için bağımlılık yüklü NPS sunucularını RADIUS isteklerini alacak yok. NPS uzantısı, bu tür yüklemeleri ve hataları üzerinden kimlik doğrulama isteğini ayrıntıları okuyamıyor beri yüklendiğinde çalışmaz. |
| **REQUEST_MISSING_CODE** | NPS ve NAS sunucuları arasında parola şifreleme Protokolü'nü kullanmakta olduğunuz ikincil kimlik doğrulama yöntemini desteklediğinden emin olun. **PAP** bulutta Azure mfa tüm kimlik doğrulama yöntemlerini destekler: telefon araması, tek yönlü SMS mesajı, mobil uygulama bildirimi ve mobil uygulama doğrulama kodu. **CHAPV2** ve **EAP** telefon araması ve mobil uygulama bildirimi destekler. |
| **USERNAME_CANONICALIZATION_ERROR** | Kullanıcı, şirket içi Active Directory örneğinde var olduğunu ve NPS hizmetini dizine erişim izni olduğunu doğrulayın. Ormanlar arası güven kullanıyorsanız [desteğine başvurun](#contact-microsoft-support) daha fazla yardım için. |


   

### <a name="alternate-login-id-errors"></a>Alternatif oturum açma kimliği hataları

| Hata kodu | Hata iletisi | Sorun giderme adımları |
| ---------- | ------------- | --------------------- |
| **ALTERNATE_LOGIN_ID_ERROR** | Hata: userObjectSid arama başarısız oldu | Kullanıcı, şirket içi Active Directory örneğinde var olduğunu doğrulayın. Ormanlar arası güven kullanıyorsanız [desteğine başvurun](#contact-microsoft-support) daha fazla yardım için. |
| **ALTERNATE_LOGIN_ID_ERROR** | Hata: Alternatif LoginId arama başarısız oldu | LDAP_ALTERNATE_LOGINID_ATTRIBUTE değerine ayarlandığını doğrulayın bir [geçerli bir active directory öznitelik](https://msdn.microsoft.com/library/ms675090(v=vs.85).aspx). <br><br> LDAP_FORCE_GLOBAL_CATALOG True olarak ayarlandığında veya LDAP_LOOKUP_FORESTS bir boş değer ile yapılandırılmışsa, bir genel katalog yapılandırdıysanız ve AlternateLoginId öznitelik kendisine eklenir doğrulayın. <br><br> LDAP_LOOKUP_FORESTS bir boş değer ile yapılandırılmışsa, değerin doğru olduğundan emin olun. Birden çok orman adı varsa, adları noktalı virgülle, boşluk ile ayrılmış olması gerekir. <br><br> Bu adımlar sorunu gidermeyi yok ediyorsanız [desteğine başvurun](#contact-microsoft-support) daha fazla yardım için. |
| **ALTERNATE_LOGIN_ID_ERROR** | Hata: Alternatif LoginId değeri boş. | AlternateLoginId öznitelik kullanıcı için yapılandırıldığından emin olun. |


## <a name="errors-your-users-may-encounter"></a>Kullanıcılarınızın hatalarla karşılaşabilirsiniz

| Hata kodu | Hata iletisi | Sorun giderme adımları |
| ---------- | ------------- | --------------------- |
| **Erişim engellendi** | Arayan Kiracı Kullanıcı için kimlik doğrulaması yapmak için erişim izinleri yok | Kiracı etki alanı ve etki alanı kullanıcı asıl adı (UPN) aynı olup olmadığını denetleyin. Örneğin, olduğundan emin olun user@contoso.com Contoso Kiracı kimlik doğrulaması çalışıyor. UPN Azure Kiracı için geçerli bir kullanıcı temsil eder. |
| **AuthenticationMethodNotConfigured** | Belirtilen kimlik doğrulama yöntemi kullanıcı için yapılandırılmadı | Ekleme veya kendi doğrulama yöntemlerini yönergelerine göre doğrulamak için kullanıcının sahip [iki aşamalı doğrulama için ayarlarınızı yönetme](./../../multi-factor-authentication/end-user/multi-factor-authentication-end-user-manage-settings.md). |
| **AuthenticationMethodNotSupported** | Belirtilen kimlik doğrulama yöntemi desteklenmiyor. | Bu hataya dahil etmek, günlükleri toplamak ve [desteğine başvurun](#contact-microsoft-support). Desteğe başvurduğunuzda, kullanıcı adı ve hatayı tetikleyen ikincil doğrulama yöntemi sağlar. |
| **BecAccessDenied** | MSODS Bec çağrısı erişim reddedildi döndürdü, büyük olasılıkla kullanıcıadı Kiracı içinde tanımlı değil | Kullanıcı Active Directory şirket içi var, ancak Azure AD tarafından AD Connect eşitlenmedi. Veya kullanıcı için Kiracı yok. Azure AD ile kullanıcı ekleyin ve bunları kendi doğrulama yöntemlerini yönergelerine göre eklemek [iki aşamalı doğrulama için ayarlarınızı yönetme](./../../multi-factor-authentication/end-user/multi-factor-authentication-end-user-manage-settings.md). |
| **InvalidFormat** veya **StrongAuthenticationServiceInvalidParameter** | Telefon numarası tanınmayan biçimindedir | Doğrulama telefon numaralarına düzeltmek kullanıcı sahip. |
| **InvalidSession** | Belirtilen oturum geçersiz veya süresi sona ermiş olabilir | Oturum üç tamamlamak için dakikadan uzun sürdü. Kullanıcı doğrulama kodunu girerek, veya kimlik doğrulama isteğini başlatarak, üç dakika içinde uygulama bildirimine yanıt verme doğrulayın. Bu sorunu çözmezse, istemci, NAS sunucusu, NPS sunucusu ve Azure MFA uç noktası arasında hiçbir ağ gecikmeleri olup olmadığını denetleyin.  |
| **NoDefaultAuthenticationMethodIsConfigured** | Varsayılan kimlik doğrulama yöntemi kullanıcı için yapılandırılan | Ekleme veya kendi doğrulama yöntemlerini yönergelerine göre doğrulamak için kullanıcının sahip [iki aşamalı doğrulama için ayarlarınızı yönetme](./../../multi-factor-authentication/end-user/multi-factor-authentication-end-user-manage-settings.md). Kullanıcı bir varsayılan kimlik doğrulama yöntemini seçmiş ve bu yöntem, hesap için yapılandırıldığını doğrulayın. |
| **OathCodePinIncorrect** | Yanlış kod ve PIN girildi. | Bu hata, NPS uzantısı'nda beklenmiyor. Bu, kullanıcı karşılaşırsa [desteğine başvurun](#contact-microsoft-support) sorun giderme Yardımı. |
| **ProofDataNotFound** | Sağlama verileri belirtilen kimlik doğrulama yöntemi için yapılandırılmadı. | Farklı bir doğrulama yöntemi deneyin veya yeni bir doğrulama yöntemi yönergelerine göre eklemek kullanıcının [iki aşamalı doğrulama için ayarlarınızı yönetme](./../../multi-factor-authentication/end-user/multi-factor-authentication-end-user-manage-settings.md). Kullanıcı kendi doğrulama yöntemini doğru şekilde kurulduğundan emin onaylandıktan sonra bu hatayı görmeye devam ederse [desteğine başvurun](#contact-microsoft-support). |
| **SMSAuthFailedWrongCodePinEntered** | Yanlış kod ve PIN girildi. (OneWaySMS) | Bu hata, NPS uzantısı'nda beklenmiyor. Bu, kullanıcı karşılaşırsa [desteğine başvurun](#contact-microsoft-support) sorun giderme Yardımı. |
| **TenantIsBlocked** | Kiracı engellendi | [Desteğe başvurun](#contact-microsoft-support) Azure portalında Azure AD özellikler sayfasından dizin kimliği. |
| **UserNotFound** | Belirtilen kullanıcı bulunamadı. | Kiracı artık Azure AD'de etkin olarak görünür olur. Aboneliğinizin etkin olduğunu ve gerekli olan denetleme uygulamaları'birinci taraf. Ayrıca sertifika konusu kiracısında beklendiği gibi değil ve sertifika hala geçerli ve hizmet sorumlusu altında kayıtlı olduğundan emin olun. |

## <a name="messages-your-users-may-encounter-that-arent-errors"></a>Hata iletileri, kullanıcılarınızın, karşılaşabileceğiniz değil

Bazı durumlarda, kendi kimlik doğrulama isteği başarısız olduğundan, kullanıcılarınızın çok faktörlü kimlik doğrulamasını iletileri alabilirsiniz. Bu yapılandırma ürün hataları değildir, ancak neden bir kimlik doğrulama isteği reddedildi kasıtlı uyarıları açıklayan.

| Hata kodu | Hata iletisi | Önerilen adımlar | 
| ---------- | ------------- | ----------------- |
| **OathCodeIncorrect** | Yanlış kod entered\OATH kodu yanlış | Bir hata kullanıcı yanlış kod girmiştir. | Kullanıcı yanlış kod girildi. Yeni bir kod isteme veya tekrar oturum açmayı tekrar denemelerini sağlayın. | 
| **SMSAuthFailedMaxAllowedCodeRetryReached** | İzin verilen en fazla kod yeniden deneme üst sınırına | Kullanıcı, çok fazla kez doğrulama sınaması başarısız oldu. Ayarlarınıza bağlı olarak, bunlar bir yönetici tarafından şimdi engeli kaldırılmış gerekebilir.  |
| **SMSAuthFailedWrongCodeEntered** | Yanlış kod girildi/metin iletisi OTP yanlış | Kullanıcı yanlış kod girildi. Yeni bir kod isteme veya tekrar oturum açmayı tekrar denemelerini sağlayın. |

## <a name="errors-that-require-support"></a>Desteği gerektiren hataları

Bu hatalardan biri karşılaşırsanız öneririz, [desteğine başvurun](#contact-microsoft-support) tanılama Yardım. Bu hataları gidermek adımları standard ayarlanmış yok. Desteğe başvurduğunuzda mümkün olduğu kadar bilgi bir hataya neden adımlar hakkında ve Kiracı bilgilerinizi eklediğinizden emin olun.

| Hata kodu | Hata iletisi |
| ---------- | ------------- |
| **InvalidParameter** | İstek null olmamalıdır |
| **InvalidParameter** | Null veya boş ReplicationScope için objectID olmamalıdır:{0} |
| **InvalidParameter** | Şirket adı uzunluğu \{0} \ uzunluğu izin verilenden daha uzun {1} |
| **InvalidParameter** | UserPrincipalName null veya boş olmamalıdır |
| **InvalidParameter** | Sağlanan Tenantıd doğru biçimde değil. |
| **InvalidParameter** | SessionID null veya boş olmamalıdır |
| **InvalidParameter** | İstek herhangi ProofData veya Msods çözümlenemedi. ProofData bilinmiyor |
| **InternalError** |  |
| **OathCodePinIncorrect** |  |
| **VersionNotSupported** |  |
| **MFAPinNotSetup** |  |

## <a name="next-steps"></a>Sonraki adımlar

### <a name="troubleshoot-user-accounts"></a>Kullanıcı hesaplarıyla ilgili sorunları giderme

Kullanıcılarınız varsa [iki aşamalı doğrulamayı sorununuz](./../../multi-factor-authentication/end-user/multi-factor-authentication-end-user-troubleshoot.md), bunları otomatik olarak tanılamak sorunları yardımcı olur. 

### <a name="contact-microsoft-support"></a>Microsoft Destek'e başvurun

Ek Yardım gerekirse, aracılığıyla destek uzmanına başvurun [Azure çok faktörlü kimlik doğrulama sunucusu desteği](https://support.microsoft.com/oas/default.aspx?prid=14947). Sorununuzu mümkün olduğunca hakkında kadar bilgi dahil ederseniz bize kurulurken yardımcı olur. Sağladığınız bilgiler içerir, özel hata kodu hatanın nerede gördüğünüzü sayfa belirli bir oturum kimliği, gördüğünüz hata ve hata ayıklama günlüklerini kullanıcının kimliği.

Destek Tanılama için hata ayıklama günlüklerini toplamak için NPS sunucusunda aşağıdaki adımları kullanın:

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

4. Bu komutlar İzlemeyi Durdur:

   ```
   logman stop "NPSExtension" -ets
   netsh trace stop
   wevtutil epl AuthNOptCh C:\NPS\%computername%_AuthNOptCh.evtx
   wevtutil epl AuthZOptCh C:\NPS\%computername%_AuthZOptCh.evtx
   wevtutil epl AuthZAdminCh C:\NPS\%computername%_AuthZAdminCh.evtx
   Start .
   ```

5. Kayıt Defteri Düzenleyicisi'ni açın ve Gözat HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureMfa kümesine **VERBOSE_LOG** için **FALSE**
6. C:\NPS klasörünün içeriğini zip ve sıkıştırılmış dosya için destek talebi ekleyin.


