---
title: Azure Active Directory Sertifika tabanlı kimlik doğrulamasını Android
description: Desteklenen senaryoları ve yapılandırma sertifika tabanlı kimlik doğrulama çözümlerinde gereksinimleri ile Android cihazları öğrenin
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 01/15/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: annaba
ms.openlocfilehash: ef58fd6c4701f367c6b8664c9cf9ee76e15fbd70
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="azure-active-directory-certificate-based-authentication-on-android"></a>Azure Active Directory Sertifika tabanlı kimlik doğrulamasını Android

Android cihazlar Azure Active Directory'ye bağlanılırken cihazında bir istemci sertifikası kullanarak kimlik doğrulaması için sertifika tabanlı kimlik doğrulaması (CBA) kullanabilirsiniz:

* Microsoft Outlook ve Microsoft Word gibi Office mobil uygulamaları
* Exchange ActiveSync (EAS) istemcileri

Bu özelliği yapılandıran bir kullanıcı adı ve parola birleşimini belirli mail ve Microsoft Office uygulamaları mobil cihazınızdan girin gereğini ortadan kaldırır.

Bu konu, gereksinimler ve desteklenen senaryoları ile Office 365 Kurumsal, iş, eğitim, ABD devlet kurumları, Çin kiracılar kullanıcılarının iOS(Android) cihazında CBA yapılandırmak için sağlar ve Almanya planları.

Bu özellik, Office 365 US Government savunma ve Federal planlarda Önizleme'de kullanılabilir.

## <a name="microsoft-mobile-applications-support"></a>Microsoft mobil uygulamaları desteği

| Uygulamalar | Destek |
| --- | --- |
| Azure Information Protection uygulama |![İşaretli][1] |
| Intune Şirket portalı |![İşaretli][1] |
| Microsoft Teams |![İşaretli][1] |
| OneNote |![İşaretli][1] |
| OneDrive |![İşaretli][1] |
| Outlook |![İşaretli][1] |
| Power BI |![İşaretli][1] |
| Skype Kurumsal |![İşaretli][1] |
| Word / Excel / PowerPoint |![İşaretli][1] |
| Yammer |![İşaretli][1] |

### <a name="implementation-requirements"></a>Uygulama gereksinimleri

Aygıt işletim sistemi sürümü Android 5.0 (Lolipop) olmalıdır ve üstü.

Bir federasyon sunucusunun yapılandırılması gerekir.

Azure bir istemci sertifikası iptal etmek için Active Directory, ADFS belirteç aşağıdaki talep sahip olmanız gerekir:

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>` (İstemci sertifikanın seri numarası)
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>` (İstemci sertifikası veren dize)

ADFS belirteç (veya başka bir SAML belirteci) kullanılabilir olmaları durumunda azure Active Directory yenileme belirtecini bu talep ekler. Yenileme belirteci doğrulanması gerektiğinde, bu bilgileri iptalini denetlemek için kullanılır.

En iyi uygulama, aşağıdaki bilgilerle kuruluşunuzun ADFS hata sayfaları güncelleştirmeniz gerekir:

* Android Microsoft Authenticator yükleme gereksinimi.
* Kullanıcı sertifika alma hakkında yönergeler.

Daha fazla bilgi için bkz: [AD FS oturum açma sayfalarını özelleştirme](https://technet.microsoft.com/library/dn280950.aspx).

Bazı Office uygulamaları (modern kimlik doğrulaması etkin ile) Gönder '*oturum açma istemini =*', istekte Azure ad. Varsayılan olarak, Azure AD çevirir '*oturum açma istemini =*'ın ADFS isteğine'*wauth usernamepassworduri =*' (U/P kimlik doğrulama yapmak için ADFS ister) ve '*wfresh = 0*' (için ADFS sorar SSO durumu yoksay ve yeni bir kimlik doğrulaması yapın). Sertifika tabanlı kimlik doğrulaması bu uygulamalar için etkinleştirmek istiyorsanız, varsayılan Azure AD davranışını değiştirmek gerekir. Ayarlama '*PromptLoginBehavior*'ın Federasyon etki alanı ayarlarınızı'*devre dışı*'.
Kullanabileceğiniz [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) bu görevi gerçekleştirmek için cmdlet:

`Set-MSOLDomainFederationSettings -domainname <domain> -PromptLoginBehavior Disabled`

## <a name="exchange-activesync-clients-support"></a>Exchange ActiveSync istemcileri desteği

Belirli Exchange ActiveSync uygulamaları Android 5.0 (Lolipop) veya sonraki sürümlerde desteklenmektedir. E-posta uygulamanız bu özelliği destekleyen belirlemek için uygulamanın geliştiricisine başvurun.

## <a name="next-steps"></a>Sonraki adımlar

Sertifika tabanlı kimlik doğrulaması, ortamınızda yapılandırmak istiyorsanız, bkz: [Android sertifika tabanlı kimlik doğrulamasını kullanmaya başlama](active-directory-certificate-based-authentication-get-started.md) yönergeler için.

<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-android/ic195031.png
