---
title: Azure AD SSPR ve mfa'yı (Önizleme) - Azure Active Directory için birleşik kayıt
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
ms.openlocfilehash: 536d26abf563f18ed7cec6668fcd1d4223f5a135
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58370168"
---
# <a name="combined-security-information-registration-preview"></a>Birleştirilmiş güvenlik bilgileri kayıt (Önizleme)

Birleşik kayıt önce kullanıcıların Azure multi-Factor Authentication (MFA) ve iki farklı deneyimler aracılığıyla Self Servis parola sıfırlama (SSPR) için kimlik doğrulama yöntemleri kayıtlı. Kişilerin, Azure MFA ve SSPR için benzer yöntemler kullanıldı, ancak her bir özellik için ayrı olarak kaydetmek sahip oldukları karıştırılır. Şimdi, birleşik kayıt ile kullanıcılar bir kez kaydedebilir ve Azure mfa'yı hem SSPR avantajlarından yararlanın.

![Bir kullanıcı için güvenlik bilgisi My profili gösteren kayıtlı](media/concept-registration-mfa-sspr-combined/combined-security-info-defualts-registered.png)

Yeni deneyimi etkinleştirmeden önce bu yönetici odaklı belgeleri ve işlevleri ve bu özellik etkisini anladığınızdan emin olmak için kullanıcı odaklı belgeleri gözden geçirin. Kullanıcı belgeleri, kullanıcıların yeni deneyimi hazırlamak ve başarılı bir sunum emin olmaya yardım etmek için eğitim temel alır.

|     |
| --- |
| Azure multi-Factor Authentication ve Azure AD Self Servis parola sıfırlama için birleşik güvenlik bilgileri kayıt Azure Active Directory genel Önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz: [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

> [!IMPORTANT]
> Bir kullanıcı hem özgün Önizleme hem de Gelişmiş birleşik kayıt deneyimi için etkinse, yeni deneyimi görürler. Her iki deneyimleri etkin kullanıcılar yalnızca Profilim deneyim elde edeceksiniz. Yeni Profilim, birleşik kayıt görünümü ve deneyimini ile hizalanan ve kullanıcılar için sorunsuz bir deneyim sağlar. Kullanıcılar, giderek Profilim görebilirsiniz [ https://myprofile.microsoft.com ](https://myprofile.microsoft.com).

Profilim'i sayfaları geçerli dil ayarları sayfasına erişme makineye göre yerelleştirilir. Microsoft, tarayıcı önbelleğine sonraki denemeler erişmek için kullanılan son dil işleme devam edecek şekilde kullanılan en son dil depolar. Önbelleği temizleme, yeniden oluşturulacak sayfaların neden olur. Ekleme belirli dil zorlamak istiyorsanız bir `?lng=de-DE` URL'nin sonuna burada `de-DE` ayarlanır uygun dili için kod sayfaları o dilde işlemek için zorlar.

![SSPR veya diğer ek güvenlik doğrulama yöntemlerini Kurulumu](media/howto-registration-mfa-sspr-combined/combined-security-info-my-profile.png)

## <a name="methods-available-in-converged-registration"></a>Yakınsanmış kaydında kullanılabilir yöntemleri

Şu anda birleşik kayıt aşağıdaki yöntemleri ve eylemleri için bu yöntemleri destekler:

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
> Uygulama parolaları yalnızca kullanıcılar için mfa'yı zorunlu kullanılabilir. Uygulama parolaları bir koşullu erişim ilkesi MFA için etkinleştirilmiş kullanıcılar tarafından kullanılamaz.

Kullanıcılar, kendi varsayılan yöntemi olarak MFA için aşağıdaki seçenekleri ayarlayabilirsiniz:

- Microsoft Authenticator – bildirimi
- Authenticator uygulaması veya donanım belirteci – kodu
- Telefon araması
- Kısa mesaj

Daha fazla kimlik doğrulama yöntemleri gibi Azure AD'ye eklemeye devam ediyoruz, bu yöntemleri birleşik kaydında kullanılabilir olacaktır.

## <a name="combined-registration-modes"></a>Birleşik kayıt modları

Birleşik kayıt iki "mod" vardır: kesme ve yönetin.

Kesme modu, bir kullanıcıya gösterilen kaydolun veya oturum açma sırasında güvenlik bilgileri Yenile Sihirbazı benzeri deneyim sunar.

Yönetme modu kullanıcının profilinin parçası olan ve güvenlik bilgilerini yönetmenizi sağlar.

Kullanıcı MFA için kullanılabilecek bir yöntem daha önce kaydedildiyse her iki mod için bunlar güvenlik bilgilerini erişebilmeniz için önce MFA gerçekleştirmeniz gerekir.

### <a name="interrupt-mode"></a>Kesme modu

Her ikisi de kiracınız için etkinleştirilip etkinleştirilmediğini birleşik kayıt mfa'yı hem SSPR ilkeleri dikkate alır. Bu ilkeleri denetim, oturum açma sırasında kaydetmek için bir kullanıcı olup olmadığını kesintiye ve hangi yöntemlerin kaydetmek kullanılabilir.

Kaydolun veya güvenlik bilgilerini yenilemek için bir kullanıcı burada istenebilir birkaç senaryo listeleyin:

* Kimlik koruması zorunlu MFA kaydı: Kullanıcılar, oturum açma sırasında kaydedilecek istenecektir. (Kullanıcının SSPR'ye etkinse) mfa'yı ve SSPR yöntemleri kaydedin.
* MFA kaydı kullanıcı başına MFA zorlanır: Kullanıcılar, oturum açma sırasında kaydedilecek istenecektir. (Kullanıcının SSPR'ye etkinse) mfa'yı ve SSPR yöntemleri kaydedin.
* Koşullu erişim veya diğer ilkeleri zorunlu MFA kaydı: Kullanıcılar, MFA gerektiren bir kaynağa erişilirken kaydetmek için istenir. (Kullanıcının SSPR'ye etkinse) kullanıcı MFA yöntemleri ve SSPR yöntemleri kaydeder.
* SSPR kaydı zorlanır: Kullanıcılar, oturum açma sırasında kaydetmek için istenir. Bunlar yalnızca SSPR yöntemlerini kaydedin
* SSPR yenileme zorlanır: Kullanıcılar yöneticiniz tarafından belirlenen aralıklarla güvenlik bilgilerini gözden geçirmek için gereklidir Kullanıcılar kendi bilgi gösterilir ve "İyi görünüyor" seçin veya gerekirse değişiklik.

Kayıt uygulandığında, kullanıcılara az güvenliğini sağlamak için yöntemleri en fazla MFA hem SSPR ilkeleri ile uyumlu olması gereken en düşük sayısı gösterilir.

Örnek:

* Bir kullanıcının SSPR için etkinleştirilir. SSPR ilke sıfırlamak için iki yöntem gerekli ve mobil uygulama kodu, e-posta ve telefon etkinleştirdi.
   * Bu kullanıcı, iki yöntem kaydetmek için gereklidir.
      * Bunlar authenticator uygulamasını ve telefon varsayılan olarak gösterilir.
      * Kullanıcı e-posta yerine kimlik doğrulayıcısı uygulaması ya da telefon kaydetmek seçebilirsiniz.

Aşağıdaki akış çizelgesi, hangi yöntemlerin kesintiye, bir kullanıcı oturum açma sırasında kaydetmek için gösterilen açıklanmaktadır:

![Birleştirilmiş güvenlik bilgisi akış çizelgesi](media/concept-registration-mfa-sspr-combined/combined-security-info-flow-chart.png)

MFA hem de etkin SSPR varsa, MFA kaydı zorunlu kılmanız önerilir.

SSPR İlkesi güvenlik bilgileri düzenli aralıklarla gözden geçirilecek kullanıcılar gerektiriyorsa, kullanıcılar oturum açma sırasında kesintiye ve bunların tüm kayıtlı yöntemleri gösterilmektedir. Bilgileri güncel olduğundan ya da "Bilgi düzenleme" seçtikleri "İyi görünüyor" seçebilirsiniz değişiklik yapma.

### <a name="manage-mode"></a>Modunu yönetin

Kullanıcıların erişebileceği giderek modu yönetme [ https://aka.ms/mysecurityinfo ](https://aka.ms/mysecurityinfo) veya "Güvenlik bilgisi" Profilim içindeki seçerek. Buradan, kullanıcılar yöntemleri ekleyin, silin veya var olan yöntemleri değiştirmek için ve kendi varsayılan yöntemini değiştirme.

## <a name="key-usage-scenarios"></a>Anahtar kullanım senaryoları

### <a name="set-up-security-info-during-sign-in"></a>Oturum açma sırasında güvenlik bilgileri ' ayarlayın

Bir yönetici, kaydı zorunlu.

Bir kullanıcı tüm gerekli güvenlik bilgisi ayarlamamış ve Azure portalına götürür. Kullanıcı adı ve parola girdikten sonra kullanıcı güvenlik bilgileri ' ayarlaması istenir. Kullanıcı, ardından gerekli güvenlik bilgilerini ayarlamak için Sihirbazı'nda gösterilen adımları izler. Kullanıcı ayarlarınız izin veriyorsa varsayılan olarak gösterilen dışında yöntemleri ayarlamak seçebilirsiniz. Sihirbazın sonunda, kullanıcı için mfa'yı bunlar ayarlama yöntemleri ve kendi varsayılan yöntemini inceler. Kurulum işlemini tamamlamak için kullanıcı bilgileri onaylar ve Azure portalına devam eder.

### <a name="set-up-security-info-from-my-profile"></a>Profilim içindeki güvenlik bilgileri ' ayarlayın

Bir yönetici, kaydı zorunlu değildir.

Henüz tüm gerekli güvenlik bilgisi ayarlamamış bir kullanıcıyı gider [ https://myprofile.microsoft.com ](https://myprofile.microsoft.com). Ardından kullanıcının seçtiği **güvenlik bilgisi** sol gezinti bölmesinden. Kullanıcı bir yöntem eklemeyi seçtiğinde buradan kullanabilecekleri yöntemlerden herhangi birini seçer ve bu yöntemi ayarlamak için adımları izler. İşiniz bittiğinde kullanıcı bunlar yalnızca güvenlik bilgileri sayfasında ayarlanmış yöntemi görür.

### <a name="delete-security-info-from-my-profile"></a>Profilim içindeki güvenlik bilgilerini sil

En az bir yöntemi ayarlamak daha önce bir kullanıcı gider [ https://aka.ms/mysecurityinfo ](https://aka.ms/mysecurityinfo). Kullanıcının, önceden kaydedilmiş yöntemlerden birini silmek seçer. İşiniz bittiğinde kullanıcı güvenlik bilgileri sayfasında bu yöntem artık görür.

### <a name="change-default-method-from-my-profile"></a>Profilim içindeki varsayılan yöntemini değiştirme

MFA için kullanılabilecek en az bir yöntem önceden ayarlanmış bir kullanıcıyı gider [ https://aka.ms/mysecurityinfo ](https://aka.ms/mysecurityinfo). Kullanıcı, geçerli varsayılan yöntemleri için farklı varsayılan bir yöntemi değiştirir. İşiniz bittiğinde kullanıcı güvenlik bilgileri sayfasında, yeni varsayılan yöntemi görür.

## <a name="next-steps"></a>Sonraki adımlar

[Kiracınızda bulunan toplam kayıt etkinleştirme](howto-registration-mfa-sspr-combined.md)

[MFA ve SSPR için kullanılabilen yöntemler](concept-authentication-methods.md)

[Self Servis parola sıfırlamayı yapılandırın](howto-sspr-deployment.md)

[Azure çok faktörlü kimlik doğrulamasını yapılandırma](howto-mfa-getstarted.md)
