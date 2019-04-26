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
ms.openlocfilehash: 1f78a3135fca290d50370652b33fe0a4d16a6f83
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60358812"
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Azure multi-Factor authentication'da raporları

Azure multi-Factor Authentication, Azure portalı üzerinden erişilebilir, kuruluşunuz tarafından kullanılan çeşitli raporlar sağlar. Aşağıdaki tablo, kullanılabilen raporları listeler:

| Rapor | Location | Açıklama |
|:--- |:--- |:--- |
| Engellenen Kullanıcı Geçmişi | Azure AD > MFA sunucusu > kullanıcıları engelle/engelini kaldır | Engelleme veya kullanıcıların engelini kaldırma isteklerinin geçmişini gösterir. |
| Kullanım ve dolandırıcılık uyarıları | Azure AD > oturum açma işlemleri | Bilgi genel kullanımı, kullanıcı özeti ve kullanıcı ayrıntıları sağlar. yanı sıra belirtilen tarih aralığı içinde gönderilen sahtekarlık uyarısı geçmişini. |
| Şirket içi bileşenleri için kullanım | Azure AD > MFA sunucusu > Etkinlik Raporu | Bilgi genel kullanım için MFA NPS uzantısı ile ADFS, sağlar ve MFA sunucusu. |
| Atlanan Kullanıcı Geçmişi | Azure AD > MFA sunucusu > bir kerelik atlama | Bir kullanıcı için multi-Factor Authentication'ı atlama isteklerinin geçmişini sağlar. |
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

## <a name="powershell-reporting"></a>PowerShell raporlama

Aşağıdaki PowerShell kullanarak MFA için kaydolduğunu kullanıcıları belirleyin.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName```

Aşağıdaki PowerShell kullanarak MFA için kayıtlı değil kullanıcıları belirleyin.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName```

## <a name="next-steps"></a>Sonraki adımlar

* [Kullanıcılar için](../user-help/multi-factor-authentication-end-user.md)
* [Dağıtılacağı yeri](concept-mfa-whichversion.md)
