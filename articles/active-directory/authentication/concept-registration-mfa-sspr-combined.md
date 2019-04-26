---
title: Kayıt için Azure AD SSPR ve multi-Factor Authentication (Önizleme) - Azure Active Directory birleştirilmiş
description: Kayıt (Önizleme) Azure AD multi-Factor Authentication ve Self Servis parola sıfırlama
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7cf8d5cb13b39d58920555ff9d99a4949e1bfc20
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60415788"
---
# <a name="combined-security-information-registration-preview"></a>Birleştirilmiş güvenlik bilgileri kayıt (Önizleme)

Birleşik kayıt önce kullanıcılar kimlik doğrulama yöntemleri Azure multi-Factor Authentication ve Self Servis parola sıfırlama (SSPR) için ayrı olarak kayıtlı. Kişiler, çok faktörlü kimlik doğrulaması ve SSPR için benzer yöntemler kullanıldı, ancak her iki özellik için kaydolmak sahip oldukları karıştırılır. Şimdi, birleşik kayıt ile kullanıcılar bir kez kaydedebilir ve multi-Factor Authentication hem SSPR avantajlarından yararlanın.

![Bir kullanıcı için güvenlik bilgisi My profili gösteren kayıtlı](media/concept-registration-mfa-sspr-combined/combined-security-info-defualts-registered.png)

Yeni deneyimi etkinleştirmeden önce bu yönetici odaklı belgeleri ve işlevleri ve bu özellik etkisini anladığınızdan emin olmak için kullanıcı odaklı belgeleri gözden geçirin. Kullanıcı belgeleri, kullanıcıların yeni deneyimi hazırlamak ve başarılı bir sunum emin olmaya yardım etmek için eğitim temel alır.

Azure AD güvenlik bilgileri kayıt Ulusal bulutlara Azure ABD devlet kurumları, Azure Almanya'yı veya Azure Çin 21Vianet gibi şu anda kullanılabilir değil birleştirilmiş.

|     |
| --- |
| Çok faktörlü kimlik doğrulaması ve Azure Active Directory (Azure AD) Self Servis parola sıfırlama için birleşik güvenlik bilgileri kayıt bir Azure AD genel Önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

> [!IMPORTANT]
> Özgün Önizleme ve gelişmiş bir birleşik kayıt deneyimi için etkinleştirilmiş kullanıcılar yeni davranış görürsünüz. Her iki deneyimleri etkin kullanıcılar, yalnızca yeni Profilim deneyim elde edeceksiniz. Yeni Profilim, birleşik kayıt görünümü ve deneyimini ile hizalanan ve kullanıcılar için sorunsuz bir deneyim sağlar. Kullanıcılar, giderek Profilim görebilirsiniz [ https://myprofile.microsoft.com ](https://myprofile.microsoft.com).

Profili Sayfalarım sayfaya erişimi olan bilgisayarın dil ayarlarına göre yerelleştirilir. Microsoft, sonraki denemeler sayfalara erişmek için kullanılan son dil işleme devam edecek şekilde tarayıcı önbelleğini içinde kullanılan en son dil depolar. Önbelleği temizlemek, sayfaları yeniden işlenir. Ekleyebileceğiniz belirli bir dil zorlamak istiyorsanız, `?lng=<language>` URL'sinin sonuna burada `<language>` işlemek için istediğiniz dili kodudur.

![SSPR veya diğer güvenlik doğrulama yöntemlerini ayarlama](media/howto-registration-mfa-sspr-combined/combined-security-info-my-profile.png)

## <a name="methods-available-in-combined-registration"></a>İçinde birleşik kayıt yöntemleri

Aşağıdaki kimlik doğrulama yöntemlerini kayıt destekler ve eylemleri birleştirilmiş:

|   | Kaydolma | Değiştir | Sil |
| --- | --- | --- | --- |
| Microsoft Authenticator | Evet (en fazla 5) | Hayır | Evet |
| Diğer authenticator uygulaması | Evet (en fazla 5) | Hayır | Evet |
| Donanım belirteci | Hayır | Hayır | Evet |
| Telefon | Evet | Evet | Evet |
| Alternatif telefon | Evet | Evet | Evet |
| Ofis telefonu | Hayır | Hayır | Hayır |
| Email | Evet | Evet | Evet |
| Güvenlik soruları | Evet | Hayır | Evet |
| Uygulama parolaları | Evet | Hayır | Evet |

> [!NOTE]
> Uygulama parolaları, yalnızca çok faktörlü kimlik doğrulamasını zorunlu kullanıcılar tarafından kullanılabilir. Uygulama parolaları bir koşullu erişim ilkesi çok faktörlü kimlik doğrulaması için etkinleştirilmiş kullanıcılar tarafından kullanılamaz.

Kullanıcılar aşağıdaki seçeneklerden birini varsayılan çok faktörlü kimlik doğrulaması yöntemi ayarlayabilirsiniz:

- Microsoft Authenticator – bildirim.
- Authenticator uygulaması veya donanım belirteci – kod.
- Telefon araması.
- metin iletisi.

Azure AD'ye daha fazla kimlik doğrulama yöntemleri eklemeye devam ediyoruz, bu yöntemleri birleşik kaydında kullanılabilir olacaktır.

## <a name="combined-registration-modes"></a>Birleşik kayıt modları

Birleşik kayıt iki mod vardır: kesme ve yönetin.

- **Kesme modu** kaydolun veya oturum açma işlemi sırasında güvenlik bilgileri Yenile kullanıcılara sunulan bir sihirbaz benzeri deneyim sunar.

- **Modunun** kullanıcı profilinin bir parçasıdır ve kullanıcıların güvenlik bilgilerini yönetmesine olanak tanır.

Her iki mod için çok faktörlü kimlik doğrulaması için kullanılabilecek bir yöntem daha önce kaydettiğiniz kullanıcılar güvenlik bilgilerini erişebilmeniz için önce çok faktörlü kimlik doğrulaması gerçekleştirmeniz gerekir.

### <a name="interrupt-mode"></a>Kesme modu

Her ikisi de kiracınız için etkinleştirilip etkinleştirilmediğini birleşik kayıt multi-Factor Authentication hem SSPR ilkeleri dikkate alır. Bu ilkeler, bir kullanıcı oturum açma sırasında kayıt olup kesilir ve hangi yöntemlerin kayıt için kullanılabilir denetler.

Kaydolun veya güvenlik bilgilerini yenilemek için kullanıcıların hangi istenebilir çeşitli senaryolar şunlardır:

* Çok faktörlü kimlik doğrulaması kaydı: kimlik koruması zorlanır Kullanıcılar, oturum açma sırasında kaydetmek için istenir. (Kullanıcının SSPR'ye etkinse) çok faktörlü kimlik doğrulaması ve SSPR yöntemleri kaydedin.
* Çok faktörlü kimlik doğrulaması kaydı üzerinden kullanıcı başına çok faktörlü kimlik doğrulaması zorunlu: Kullanıcılar, oturum açma sırasında kaydetmek için istenir. (Kullanıcının SSPR'ye etkinse) çok faktörlü kimlik doğrulaması ve SSPR yöntemleri kaydedin.
* Çok faktörlü kimlik doğrulaması kaydı koşullu erişim veya diğer ilkeleri zorlanır: Kullanıcılar, çok faktörlü kimlik doğrulaması gerektiren bir kaynak kullandıklarında kaydetmek için istenir. (Kullanıcının SSPR'ye etkinse) çok faktörlü kimlik doğrulaması ve SSPR yöntemleri kaydedin.
* SSPR kaydı zorlanır: Kullanıcılar, oturum açma sırasında kaydetmek için istenir. Bunlar yalnızca SSPR yöntemlerini kaydedin.
* SSPR yenileme zorlanır: Kullanıcılar yöneticiniz tarafından belirlenen aralıklarla güvenlik bilgilerini gözden geçirmek için gereklidir Kullanıcılar kendi bilgi gösterilir ve geçerli bilgilerinizi onaylayın veya gerekirse değişiklik.

Kayıt uygulandığında, kullanıcılara en düşük olandan en az güvenli hale getirmek için multi-Factor Authentication hem SSPR ilkelerle uyumlu olmasını gerekli yöntemleri sayısı gösterilir.

Örneğin:

* Bir kullanıcının SSPR için etkinleştirilir. SSPR ilke sıfırlamak için iki yöntem gerekli ve mobil uygulama kodu, e-posta ve telefon etkinleştirdi.
   * Bu kullanıcı, iki yöntem kaydetmek için gereklidir.
      * Kullanıcı, kimlik doğrulayıcı uygulamasını ve telefon varsayılan olarak gösterilir.
      * Kullanıcı e-posta yerine kimlik doğrulayıcısı uygulaması ya da telefon kaydetmek seçebilirsiniz.

Bu akış, hangi yöntemlerin kesintiye, bir kullanıcı oturum açma sırasında kaydetmek için gösterilen açıklanmaktadır:

![Birleştirilmiş güvenlik bilgisi akış çizelgesi](media/concept-registration-mfa-sspr-combined/combined-security-info-flow-chart.png)

Multi-Factor Authentication hem de etkin SSPR varsa, multi-Factor Authentication kayıt zorunlu kılmanız önerilir.

SSPR İlkesi güvenlik bilgileri düzenli aralıklarla gözden geçirilecek kullanıcılar gerektiriyorsa, kullanıcılar oturum açma sırasında kesintiye ve bunların tüm kayıtlı yöntemleri gösterilmektedir. Güncel olduğundan veya bunlar gerekirse değişiklik yapabilir, geçerli bilgilerinizi doğrulayabilirsiniz.

### <a name="manage-mode"></a>Modunu yönetin

Kullanıcıların erişebileceği giderek modu yönetme [ https://aka.ms/mysecurityinfo ](https://aka.ms/mysecurityinfo) veya seçerek **güvenlik bilgisi** Profilim öğesinden. Buradan kullanıcılar yöntemleri ekleyin, silin veya var olan yöntemleri değiştirmek için varsayılan yöntemi ve daha fazlasını değiştirin.

## <a name="key-usage-scenarios"></a>Anahtar kullanım senaryoları

### <a name="set-up-security-info-during-sign-in"></a>Oturum açma sırasında güvenlik bilgileri ' ayarlayın

Bir yönetici, kaydı zorunlu.

Bir kullanıcı tüm gerekli güvenlik bilgisi ayarlamamış ve Azure portalına gider. Kullanıcı adını ve parolasını girdikten sonra kullanıcı güvenlik bilgileri ' ayarlaması istenir. Kullanıcı, ardından gerekli güvenlik bilgilerini ayarlamak için Sihirbazı'nda gösterilen adımları izler. Ayarlarınız izin veriyorsa, varsayılan olarak gösterilen dışındaki yöntemleri ayarlamak kullanıcı seçebilir. Sihirbazı tamamladıktan sonra kullanıcılar bunlar ayarlama yöntemleri ve varsayılan yöntemi için multi-Factor Authentication gözden geçirin. Kurulum işlemini tamamlamak için kullanıcı bilgileri onaylar ve Azure portalına devam eder.

### <a name="set-up-security-info-from-my-profile"></a>Profilim içindeki güvenlik bilgileri ' ayarlayın

Bir yönetici, kaydı zorunlu değildir.

Tüm gerekli güvenlik bilgisi henüz Ayarla edilmemiş bir kullanıcıyı gider [ https://myprofile.microsoft.com ](https://myprofile.microsoft.com). Kullanıcının seçtiği **güvenlik bilgisi** sol bölmesinde. Kullanıcı bir yöntem eklemeyi seçtiğinde buradan kullanılabilir yöntemlerden herhangi birini seçer ve bu yöntemi ayarlamak için adımları izler. İşiniz bittiğinde kullanıcı güvenlik bilgileri sayfasında yalnızca ayarlandığı yöntemi görür.

### <a name="delete-security-info-from-my-profile"></a>Profilim içindeki güvenlik bilgilerini sil

En az bir yöntemi ayarlamak daha önce bir kullanıcı gider [ https://aka.ms/mysecurityinfo ](https://aka.ms/mysecurityinfo). Kullanıcının, önceden kaydedilmiş yöntemlerden birini silmek seçer. İşiniz bittiğinde kullanıcı güvenlik bilgileri sayfasında bu yöntem artık görür.

### <a name="change-the-default-method-from-my-profile"></a>Profilim içindeki varsayılan yöntemini değiştirme

Çok faktörlü kimlik doğrulaması için kullanılabilecek en az bir yöntem önceden ayarlanmış bir kullanıcıyı gider [ https://aka.ms/mysecurityinfo ](https://aka.ms/mysecurityinfo). Kullanıcının geçerli varsayılan yöntemi için farklı varsayılan bir yöntemi değiştirir. İşiniz bittiğinde kullanıcı güvenlik bilgileri sayfasında yeni varsayılan yöntemi görür.

## <a name="next-steps"></a>Sonraki adımlar

[Kiracınızda bulunan toplam kayıt etkinleştirme](howto-registration-mfa-sspr-combined.md)

[Çok faktörlü kimlik doğrulaması ve SSPR için kullanılabilen yöntemler](concept-authentication-methods.md)

[Self Servis parola sıfırlamayı yapılandırın](howto-sspr-deployment.md)

[Azure çok faktörlü kimlik doğrulamasını yapılandırma](howto-mfa-getstarted.md)
