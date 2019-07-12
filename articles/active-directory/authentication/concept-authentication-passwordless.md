---
title: Azure Active Directory parolasız oturum açma (Önizleme)
description: FIDO2 güvenlik anahtarları veya Microsoft Authenticator uygulamasını (Önizleme) kullanarak Azure AD'ye parolasız oturum açma
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: librown
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2d80b359be0a6249327ba1ba1d51ffbc330bb073
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67712091"
---
# <a name="what-is-passwordless"></a>Parolasız nedir?

Çok faktörlü kimlik doğrulaması (MFA), kuruluşunuzun güvenliğini sağlamak için harika bir yoludur ancak kullanıcıların parolalarını anımsamak üzerine ek katman engellenen alın. Parola kaldırıldı ve sahip olduğunuz şey artı olduğunuz bir şey veya bildiğiniz bir şey yerine olduğundan parolasız kimlik doğrulama yöntemleri daha kullanışlıdır.

|   | Sahip olduğunuz şey | Olan veya bildiğiniz bir şey |
| --- | --- | --- |
| Parolasız | Telefon ya da güvenlik anahtarı | Biyometrik veya PIN |

Biz, kimlik doğrulaması için söz konusu olduğunda her kuruluşun farklı ihtiyaçları olduğunu algılar. Microsoft şu anda Windows Hello, Windows PC için premier parolasız deneyimimizi sunar. Parolasız ailesine yeni kimlik bilgileri ekliyoruz: Microsoft Authenticator uygulamasını ve FIDO2 güvenlik anahtarları.

## <a name="microsoft-authenticator-app"></a>Microsoft Authenticator uygulaması

Parolasız kimlik doğrulama yöntemi olacak telefon, çalışanın izin verir. Sizi zaten Microsoft Authenticator uygulamasını bir parola yanı sıra uygun çok faktörlü kimlik doğrulaması seçeneği olarak kullanıyor olabilir. Ancak, parolasız bir seçenek olarak kullanıma sunulmuştur.

Bunu herhangi bir iOS veya Android telefon güçlü, parolasız kimlik bilgisi içinde herhangi bir platform veya tarayıcı telefon numaraları için bir bildirim alma telefonundan bir ekranda görüntülenen bir sayı eşleşen ve sonra da kendi biyometrik ('ı kullanarak oturum açmasına izin vererek kapatır dokunma veya yüz tanıma) ya da onaylamak için PIN.

## <a name="fido2-security-keys"></a>FIDO2 güvenlik anahtarları

FIDO2 güvenlik anahtarları tüm form faktörüne gelebilir unphishable standartlara dayalı parolasız kimlik doğrulama yöntemidir. Hızlı kimlik Online (FIDO), parolasız kimlik doğrulaması için bir açık standarttır. Kaynakları bir kullanıcı adı veya bir dış güvenlik anahtarı veya bir cihazda yerleşik bir platform anahtarı kullanarak parola olmadan oturum açmak kullanıcılar ve kuruluşlar standart kullanmasına izin verir.

Genel Önizleme için çalışanların dış güvenlik anahtarları (1809 veya üzeri sürümü çalıştıran) Azure Active Directory katılmış Windows 10 makinelerinde oturum açmak için kullanabilir ve çoklu oturum bulut kaynaklarını alın. Bunlar ayrıca için desteklenen tarayıcılar oturum açabilir.

Microsoft, FIDO Alliance tarafından sertifikalı FIDO2 birçok anahtarları olsa da, isteğe bağlı uzantılar en üst düzey güvenlik ve en iyi deneyimi sağlamak için satıcı tarafından uygulanmak üzere FIDO2 CTAP belirtimi gerektirir.

Güvenlik anahtarı **gerekir** aşağıdaki özellikler ve Uzantılar'dan FIDO2 CTAP protokolü, Microsoft ile uyumlu şekilde uygulayın:

| # | Özellik / uzantı güven | Bu neden gerekli? |
| --- | --- | --- |
| 1\. | Yerleşik anahtarı | Bu özellik, güvenlik anahtarı güvenlik anahtarı kimlik bilgilerinizi depolandığı taşınabilmesini sağlar. |
| 2 | İstemci PIN | Bu özellik, ikinci faktörle kimlik bilgilerinizi korumanıza olanak tanır ve bir kullanıcı arabirimi olmayan güvenlik anahtarları için geçerlidir. |
| 3 | HMAC-secret | Bu uzantı, uçak modu veya çevrimdışı olduğunda, cihazınız için oturum sağlar. |
| 4 | RP başına birden çok hesap | Bu özellik, aynı güvenlik anahtarı Microsoft Account ve Azure Active Directory gibi çoklu hizmetlerinde kullanabileceğiniz sağlar. |

Aşağıdaki sağlayıcıları paswordless deneyimiyle uyumlu olduğu bilinen farklı form faktörleri FIDO2 güvenlik anahtarları sunar. Microsoft, müşterilerin satıcı yanı sıra FIDO Alliance başvurarak Bu anahtarları güvenlik özelliklerini değerlendirmek için teşvik eder.

| Sağlayıcı | İlgili kişi |
| --- | --- |
| Yubico | [https://www.yubico.com/support/contact/](https://www.yubico.com/support/contact/) |
| Feitian | [https://www.ftsafe.com/about/Contact_Us](https://www.ftsafe.com/about/Contact_Us) |
| GİZLENMİŞ | [https://www.hidglobal.com/contact-us](https://www.hidglobal.com/contact-us) |
| Ensurity | [https://ensurity.com/contact-us.html](https://ensurity.com/contact-us.html) |
| eWBM | [https://www.ewbm.com/page/sub1_5](https://www.ewbm.com/page/sub1_5) |

Bir satıcı ve Cihazınızı listede almak istiyorsanız, başvurun [ Fido2Request@Microsoft.com ](mailto:Fido2Request@Microsoft.com).

FIDO2 güvenlik anahtarları, çok güvenlik hassas veya bunları senaryoları veya işleyemeyeceği ya da telefon numaraları ikinci bir faktör olarak kullanmak mümkün olmayan çalışanların sahip kuruluşlar için harika bir seçenektir.

## <a name="what-scenarios-work-with-the-preview"></a>Hangi senaryoları önizlemesi ile çalışıyor?

1. Yöneticiler için kendi Kiracı parolasız kimlik doğrulama yöntemleri etkinleştirebilirsiniz
1. Yöneticiler, tüm kullanıcıları hedeflemek veya her yöntem için kendi Kiracı içindeki kullanıcılar/gruplar seçin
1. Son kullanıcılar kaydedebilir ve bu parolasız kimlik doğrulama yöntemleri, hesap portalında yönetme
1. Son kullanıcılar bu parolasız kimlik doğrulama yöntemleri ile oturum açabilirsiniz
   1. Microsoft Authenticator uygulaması: İş Azure AD kimlik doğrulaması kullanıldığı her senaryoda tüm tarayıcıları dahil olmak üzere Windows 10 (OOBE) kurulumundan ve ile sırasında tümleşik tüm işletim sistemlerinde mobil uygulamalar.
   1. Güvenlik anahtarları: Windows 10 1809 veya üzeri sürümler için kilit ekranı ve Microsoft Edge gibi desteklenen tarayıcılar ve web üzerinde çalışır.

## <a name="next-steps"></a>Sonraki adımlar

[Kuruluşunuzdaki parolasız seçeneklerini etkinleştirin](howto-authentication-passwordless-enable.md)

### <a name="external-links"></a>Dış bağlantılar

[FIDO Alliance](https://fidoalliance.org/)

[FIDO2 CTAP belirtimi](https://fidoalliance.org/specs/fido-v2.0-id-20180227/fido-client-to-authenticator-protocol-v2.0-id-20180227.html)
