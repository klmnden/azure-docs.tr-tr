---
title: Azure mfa'yı - Azure Active Directory erişim ve kullanım raporları
description: Bu, Azure multi-Factor Authentication özelliğinin - raporların nasıl kullanılacağı açıklanır.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 988e8982b6f06fb1210330c5cafdb696892794fe
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66235522"
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Azure multi-Factor authentication'da raporları

Azure multi-Factor Authentication, Azure portalı üzerinden erişilebilir, kuruluşunuz tarafından kullanılan çeşitli raporlar sağlar. Aşağıdaki tablo, kullanılabilen raporları listeler:

| Rapor | Location | Açıklama |
|:--- |:--- |:--- |
| Engellenen kullanıcı geçmişi | Azure AD > MFA sunucusu > kullanıcıları engelle/engelini kaldır | Engelleme veya kullanıcıların engelini kaldırma isteklerinin geçmişini gösterir. |
| Kullanım ve dolandırıcılık uyarıları | Azure AD > oturum açma işlemleri | Bilgi genel kullanımı, kullanıcı özeti ve kullanıcı ayrıntıları sağlar. yanı sıra belirtilen tarih aralığı içinde gönderilen sahtekarlık uyarısı geçmişini. |
| Şirket içi bileşenleri için kullanım | Azure AD > MFA sunucusu > Etkinlik Raporu | Bilgi genel kullanım için MFA NPS uzantısı ile ADFS, sağlar ve MFA sunucusu. |
| Atlanan kullanıcı geçmişi | Azure AD > MFA sunucusu > bir kerelik atlama | Bir kullanıcı için multi-Factor Authentication'ı atlama isteklerinin geçmişini sağlar. |
| Sunucu durumu | Azure AD > MFA sunucusu > sunucu durumu | Multi-Factor Authentication sunucularının durumunu hesabınızla ilişkili görüntüler. |

## <a name="view-mfa-reports"></a>MFA raporları görüntüleme

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol tarafta, seçin **Azure Active Directory** > **MFA sunucusu**.
3. Görüntülemek istediğiniz raporu seçin.

   ![Azure portalında MFA sunucusu sunucu durum raporu](./media/howto-mfa-reporting/report.png)

## <a name="azure-ad-sign-ins-report"></a>Azure AD oturum açma işlemleri raporu

İle **oturum açma işlemleri etkinlik raporuna** içinde [Azure portalında](https://portal.azure.com), ortamınızın nasıl çalıştığını belirlemek için gereken bilgileri alabilirsiniz.

Oturum açma işlemleri raporu, çok faktörlü kimlik doğrulaması (MFA) kullanımı hakkında bilgi içeren kullanımı hakkında bilgi yönetilen uygulamalar ve kullanıcı oturum açma etkinlikleri sağlayabilirsiniz. MFA verileri MFA'nın kuruluşunuzdaki kullanımı hakkında öngörüler sunar. Aşağıdaki gibi sorulara yanıt bulmanızı sağlar:

- Oturum açma sırasında MFA kullanıldı mı?
- Kullanıcı MFA'yı nasıl tamamladı?
- Kullanıcının MFA'yı tamamlayamadığı durumlar oldu mu?
- Kaç kullanıcıdan MFA istendi?
- Kaç kullanıcı MFA isteğini tamamlayamadı?
- Son kullanıcıların karşılaştığı ortak MFA sorunları nelerdir?

Bu veriler üzerinden kullanılabilir [Azure portalında](https://portal.azure.com) ve [raporlama API'sini](../reports-monitoring/concept-reporting-api.md).

![Azure portalında Azure AD oturum açma işlemleri raporu](./media/howto-mfa-reporting/sign-in-report.png)

### <a name="sign-ins-report-structure"></a>Oturum açma işlemleri raporu yapısı

MFA hakkındaki oturum açma etkinliği raporları aşağıdaki bilgilere erişmenizi sağlar:

**MFA gerekli:** Mfa'yı veya oturum açma için gerekli olup olmadığı. MFA nedeniyle kullanıcı başına MFA, koşullu erişim veya diğer nedenlerle gerekli olabilir. Olası değerler **Evet** veya **Hayır**.

**MFA sonucu:** MFA memnun veya reddedilen hakkında daha fazla bilgi:

- MFA tamamlandıysa bu sütunda MFA'nın tamamlanma şekli hakkında daha fazla bilgi yer alır.
   - Azure Multi-Factor Authentication
      - bulutta tamamlandı
      - kiracıda yapılandırılmış ilkeler nedeniyle süresi doldu
      - kayıt istendi
      - belirteç talebiyle gerçekleştirildi
      - harici sağlayıcı tarafından sağlanan taleple gerçekleştirildi
      - güçlü kimlik doğrulamasıyla gerçekleştirildi
      - gerçekleştirilen akış Windows aracısı oturum açma akışı tarafı olduğundan atlandı
      - uygulama parolası nedeniyle atlandı
      - konum nedeniyle atlandı
      - kayıtlı cihaz nedeniyle atlandı
      - hatırlanan cihaz nedeniyle atlandı
      - başarıyla tamamlandı
   - Çok faktörlü kimlik doğrulaması için harici sağlayıcıya yönlendirildi

- MFA reddedildiyse bu sütunda reddedilme nedeni yer alır.
   - Azure Multi-Factor Authentication tarafından reddedildi;
      - kimlik doğrulaması devam ediyor
      - yinelenen kimlik doğrulaması girişimi
      - çok fazla kez yanlış kod girildi
      - geçersiz kimlik doğrulaması
      - geçersiz mobil uygulama doğrulama kodu
      - yanlış yapılandırma
      - telefon araması sesli mesaja düştü
      - telefon numarası biçimi geçersiz
      - hizmet hatası
      - kullanıcının telefonuna ulaşılamadı
      - cihaza mobil uygulama bildirimi gönderilemedi
      - mobil uygulama bildirimi gönderilemedi
      - kullanıcı kimlik doğrulamasını reddetti
      - kullanıcı mobil uygulama bildirimine yanıt vermedi
      - kullanıcının kayıtlı doğrulama yöntemi yok
      - kullanıcı hatalı kod girdi
      - kullanıcı hatalı PIN girdi
      - kullanıcı kimlik doğrulaması başarılı olmadan telefon aramasını sonlandırdı
      - kullanıcı engellendi
      - kullanıcı doğrulama kodunu hiç girmedi
      - kullanıcı bulunamadı
      - doğrulama kodu zaten bir kez kullanıldı

**MFA kimlik doğrulama yöntemi:** Mfa'yı tamamlamak için kullanılan kullanıcı kimlik doğrulama yöntemi. Olası değerler şunlardır:

- Kısa mesaj
- Mobil uygulama bildirimi
- Telefon araması (Kimlik doğrulama telefonu)
- Mobil uygulama doğrulama kodu
- Telefon araması (Ofis telefonu)
- Telefon araması (Alternatif kimlik doğrulama telefonu)

**MFA kimlik doğrulama Ayrıntısı:** Örneğin telefon numarası sürümünü iptal etti: + X XXXXXXXX64.

**Koşullu erişim** etkilenen oturum açma denemesi de dahil olmak üzere bir koşullu erişim ilkeleri hakkında daha fazla bilgi:

- İlke adı
- İzin verme denetimleri
- Oturum denetimleri
- Sonuç

## <a name="powershell-reporting-on-users-registered-for-mfa"></a>MFA için kaydolduğunu kullanıcıların raporlama PowerShell

İlk olarak, sahip olduğunuzdan emin olun. [MSOnline V1 PowerShell modülünü](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-1.0) yüklü.

Aşağıdaki PowerShell kullanarak MFA için kaydolduğunu kullanıcıları belirleyin.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName```

Aşağıdaki PowerShell kullanarak MFA için kayıtlı değil kullanıcıları belirleyin.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName```

## <a name="possible-results-in-activity-reports"></a>Etkinlik raporlarını olası sonuçları

Aşağıdaki tabloda, çok faktörlü kimlik doğrulaması Etkinlik Raporu indirilen sürümünü kullanarak çok faktörlü kimlik doğrulaması sorunlarını gidermek için kullanılabilir. Doğrudan Azure Portalı'nda görünmez.

| Arama sonucu | Açıklama | Geniş açıklaması |
| --- | --- | --- |
| SUCCESS_WITH_PIN | PIN girildi | Kullanıcı bir PIN girildi.  Ardından kimlik doğrulaması başarılı olursa doğru PIN girilmelidir.  Authentication reddedilirse, yanlış bir PIN girildiğinde ardından veya kullanıcı standart moda ayarlanır. |
| SUCCESS_NO_PIN | Girilen # | Kullanıcı PIN moduna ayarlanır ve kimlik doğrulaması reddedildi, bu kullanıcı PIN'ini girmeli değildir ve yalnızca, # girilen anlamına gelir.  Kullanıcı standart moda ayarlanır ve bunun anlamı doğrulama başarılı olursa kullanıcı yalnızca girilen yapmak için doğru olanı standart modunda olan #. |
| SUCCESS_WITH_PIN_BUT_TIMEOUT | # Girişten sonra tuşuna basılmadı | # Değil girildiğinden kullanıcının tüm DTMF rakamlarıyla göndermedi.  # Girişinin tamamlama belirten belirtilmedikçe diğer rakamlar da girilen gönderilmez. |
|SUCCESS_NO_PIN_BUT_TIMEOUT | Telefon girişi yok - zaman aşımına uğradı | Arama yanıtlandı, ancak yanıt yoktu.  Bu çağrı tarafından sesli mesaj çekilen bir gösterir. |
| SUCCESS_PIN_EXPIRED | PIN süresi doldu ve değiştirilmedi | Kullanıcının PIN'inin süresi doldu ve değiştirmek için istenmiş ancak PIN değişikliği başarılı bir şekilde tamamlanmadı. |
| SUCCESS_USED_CACHE | Kullanılan önbellek | Önceki başarılı bir kimlik aynı kullanıcı adı için yapılandırılmış önbellek zaman dilimi içinde gerçekleştikten kimlik doğrulaması bir multi-Factor Authentication araması başarılı. |
| SUCCESS_BYPASSED_AUTH | Atlanan kimlik doğrulaması | Kimlik doğrulaması için kullanıcı tarafından başlatılan bir kerelik geçiş kullanarak başarılı oldu.  Atlama atlanacağı kullanıcı Geçmişi raporu daha fazla ayrıntı için bkz. |
| SUCCESS_USED_IP_BASED_CACHE | IP tabanlı kullanılan önbellek | Aynı kullanıcı adı, kimlik doğrulama türü, uygulama adı için bir önceki başarılı kimlik doğrulaması bu yana kimlik doğrulaması bir multi-Factor Authentication araması başarılı oldu ve yapılandırılmış önbellek zaman dilimi içinde IP oluştu. |
| SUCCESS_USED_APP_BASED_CACHE | Uygulama tabanlı kullanılan önbellek | Kimlik doğrulaması bir multi-Factor Authentication araması itibaren aynı kullanıcı adı, kimlik doğrulama türü ve yapılandırılmış önbellek zaman çerçevesi içinde uygulama adı için bir önceki başarılı kimlik doğrulaması başarılı. |
| SUCCESS_INVALID_INPUT | Geçersiz telefon girişi | Telefonunuzdan gönderilen yanıt geçerli değil.  Bir faks makineden bunun veya modem veya kullanıcı girmiş * PIN'ini bir parçası olarak. |
| SUCCESS_USER_BLOCKED | Kullanıcı engellendi | Kullanıcının telefon numarasını engellenir.  Engellenen bir sayı, bir kimlik doğrulama araması sırasında kullanıcı tarafından veya Azure portalını kullanarak bir yönetici tarafından başlatılabilir. <br> NOT:  Ayrıca olan bir byproduct bir sahtekarlık Uyarısı, engellenen bir sayıdır. |
| SUCCESS_SMS_AUTHENTICATED | SMS mesajı kimliği doğrulandı | İçin iki yönlü sınama iletisi, kullanıcı bir kerelik geçiş kodunu (OTP) ile doğru şekilde yanıtlanan ya da OTP + PIN. |
| SUCCESS_SMS_SENT | SMS mesajı gönderildi | Metin iletisi için bir kerelik geçiş kodu (OTP) içeren kısa mesaj başarıyla gönderildi.  Kullanıcı, OTP veya OTP + PIN kimlik doğrulamasını tamamlamak için uygulamada girer. |
| SUCCESS_PHONE_APP_AUTHENTICATED | Mobil uygulama kimliği doğrulandı | Kullanıcı başarıyla mobil uygulama kimlik doğrulaması. |
| SUCCESS_OATH_CODE_PENDING | OATH kodu beklemede | Kullanıcı OATH kodlarını için istendi ancak yanıt alamadık. |
| SUCCESS_OATH_CODE_VERIFIED | OATH kodu doğrulandı | Kullanıcının geçerli bir OATH kodu istendiğinde girdi. |
| SUCCESS_FALLBACK_OATH_CODE_VERIFIED | Geri dönüş OATH kodu doğrulandı | Kullanıcının birincil çok faktörlü kimlik doğrulaması yöntemi kullanılarak kimlik doğrulaması reddedildi ve sonra da geri dönüş için geçerli bir OATH kodu sağlanan. |
| SUCCESS_FALLBACK_SECURITY_QUESTIONS_ANSWERED | Geri dönüş güvenlik soruları yanıtlandı | Kullanıcı birincil çok faktörlü kimlik doğrulaması yöntemi kullanılarak kimlik doğrulaması reddedildi ve ardından doğru geri dönüş için güvenlik sorularını yanıt verdi. |
| FAILED_PHONE_BUSY | Kimlik doğrulaması zaten devam ediyor | Çok faktörlü kimlik doğrulaması, bu kullanıcı için bir kimlik doğrulaması zaten işleniyor.  Bu, genellikle aynı oturum açma sırasında birden çok kimlik doğrulama istekleri gönderen RADIUS istemcileri tarafından kaynaklanır. |
| CONFIG_ISSUE | Telefona ulaşılamıyor | Arama denendi ancak ya da yüklenemedi yerleştirilme veya verilmemesi.  Bu, meşgul sinyali içerir (bağlı) hızlı meşgul sinyali üç-tonları (sayı) artık hizmetinde zaman aşımına uğradı telefonlarının, vb. |
| FAILED_INVALID_PHONENUMBER | Geçersiz telefon numarası biçimi | Telefon numarası geçersiz biçime sahip.  Telefon numaraları, sayısal bir değer olmalıdır ve 10 rakam ülke kodu + 1 için (Amerika Birleşik Devletleri ve Kanada) olmalıdır. |
| FAILED_USER_HUNGUP_ON_US | Kullanıcı telefonu kapattı | Kullanıcı telefonu yanıt verdi ancak düğmeleri tuşuna basmadan ardından Kapat. |
| FAILED_INVALID_EXTENSION | Geçersiz Dahili hat | Uzantı geçersiz karakterler içeriyor.  Sadece rakamlar, virgüller, *, ve # kullanılabilir.  Bir @ öneki de kullanılabilir. |
| FAILED_FRAUD_CODE_ENTERED | Sahtekarlık kodu girildi | Kullanıcı kimlik doğrulamasının reddedilmesiyle ve engellenen telefon numarası kaynaklanan çağrısı sırasında sahtekarlık bildirme etmediniz.| 
| FAILED_SERVER_ERROR | Arama yapılamıyor | Aramak multi-Factor Authentication hizmetini bulamadı. |
| FAILED_SMS_NOT_SENT | SMS mesajı gönderilemedi | SMS mesajı gönderilemedi.  Kimlik doğrulaması reddedildi. |
| FAILED_SMS_OTP_INCORRECT | SMS mesajı OTP yanlış | Kullanıcı yanlış bir kerelik geçiş kodu (OTP) alınana programından metin iletisi girdi.  Kimlik doğrulaması reddedildi. |
| FAILED_SMS_OTP_PIN_INCORRECT | SMS mesajı OTP + PIN yanlış | Kullanıcı yanlış bir kerelik geçiş kodu (OTP) ve/veya yanlış bir kullanıcı PIN girildi.  Kimlik doğrulaması reddedildi. |
| FAILED_SMS_MAX_OTP_RETRY_REACHED | Maksimum SMS mesajı OTP deneme aşıldı | Kullanıcı bir kerelik geçiş kodu (OTP) deneme sayısını aştı. |
| FAILED_PHONE_APP_DENIED | Mobil uygulama reddedildi | Kullanıcı, reddetme düğmesine basarak mobil uygulamasında kimlik doğrulaması reddedildi. |
| FAILED_PHONE_APP_INVALID_PIN | Mobil uygulama geçersiz PIN | Kullanıcı, mobil uygulamada kimlik doğrulaması yapılırken geçersiz bir PIN girildi. |
| FAILED_PHONE_APP_PIN_NOT_CHANGED | Mobil uygulama PIN değişmedi | Kullanıcı gerekli bir PIN değişikliği mobil uygulamaya başarıyla tamamlanmadı. |
| FAILED_FRAUD_REPORTED | Sahtekarlık bildirildi | Kullanıcı mobil uygulamasında sahtekarlık bildirdi. |
| FAILED_PHONE_APP_NO_RESPONSE | Mobil uygulama yanıt yok | Kullanıcı mobil uygulama kimlik doğrulama isteğine yanıt vermedi. |
| FAILED_PHONE_APP_ALL_DEVICES_BLOCKED | Mobil uygulama tüm cihazlar engellendi | Bu kullanıcı için mobil uygulama cihazları için bildirimler artık yanıt ve engellendi. |
| FAILED_PHONE_APP_NOTIFICATION_FAILED | Mobil uygulama bildirimi başarısız oldu | Kullanıcının cihazın üzerindeki mobil uygulamadan bir bildirim göndermeye çalışırken bir hata oluştu. |
| FAILED_PHONE_APP_INVALID_RESULT | Mobil uygulama geçersiz sonuç | Mobil uygulama geçersiz bir sonuç döndürdü. |
| FAILED_OATH_CODE_INCORRECT | Yanlış OATH kodu | Kullanıcı yanlış bir OATH kodu girdi.  Kimlik doğrulaması reddedildi. |
| FAILED_OATH_CODE_PIN_INCORRECT | OATH kodu + PIN yanlış | Kullanıcı doğru OATH kodunu ve/veya yanlış bir kullanıcı PIN girildi.  Kimlik doğrulaması reddedildi. |
| FAILED_OATH_CODE_DUPLICATE | Yinelenen OATH kodu | Kullanıcı daha önce kullanılmış bir OATH kodu girdi.  Kimlik doğrulaması reddedildi. |
| FAILED_OATH_CODE_OLD | OATH kodu eski | Kullanıcı daha önce kullanılmış bir OATH kodu önündeki bir OATH kodu girdi.  Kimlik doğrulaması reddedildi. |
| FAILED_OATH_TOKEN_TIMEOUT | OATH kodu sonuç zaman aşımı | Kullanıcı OATH kodunu girmesi için çok uzun sürdü ve çok faktörlü kimlik doğrulama denemesi zaten zaman aşımına. |
| FAILED_SECURITY_QUESTIONS_TIMEOUT | Güvenlik soruları sonuç zaman aşımı | Kullanıcının güvenlik sorularını Yanıtla girmek için çok uzun sürdü ve çok faktörlü kimlik doğrulama denemesi zaten zaman aşımına. |
| FAILED_AUTH_RESULT_TIMEOUT | Kimlik doğrulama sonuç zaman aşımı | Kullanıcı çok faktörlü kimlik doğrulama girişimi tamamlanması çok uzun sürdü. |
| FAILED_AUTHENTICATION_THROTTLED | Kısıtlanan kimlik doğrulaması | Çok faktörlü kimlik doğrulama girişimi, hizmet tarafından kısıtladı. |

## <a name="next-steps"></a>Sonraki adımlar

* [Kullanıcılar için](../user-help/multi-factor-authentication-end-user.md)
* [Dağıtılacağı yeri](concept-mfa-whichversion.md)
