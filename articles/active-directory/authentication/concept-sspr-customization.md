---
title: Özelleştirme Azure AD Self Servis parola sıfırlama - Azure Active Directory
description: Özelleştirme seçenekleri için Azure AD Self Servis parola sıfırlama
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: d38d93a1c9716cc3a71d904b7b1a46fb8b1c2ee0
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58369233"
---
# <a name="customize-the-azure-ad-functionality-for-self-service-password-reset"></a>Azure AD Self Servis parola sıfırlama işlevselliği özelleştirme

Self Servis parola sıfırlama (SSPR) dağıtmak için Azure Active directory (Azure AD) isteyen BT uzmanları, kullanıcıların ihtiyaçlarını karşılamak amacıyla deneyimi özelleştirebilirsiniz.

## <a name="customize-the-contact-your-administrator-link"></a>"Yöneticinize başvurun" bağlantısını özelleştirin

SSPR etkin değilse, kullanıcılar sıfırlama portalı parola bir "Yöneticinize başvurun" bağlantısını çözümlenmedi. Bir kullanıcı bu bağlantıyı seçerse, ya da:

* Yöneticiler e-posta gönderir ve kullanıcının parolasını değiştirme konusunda yardım almak için ister.
* Kullanıcılarınızı, Yardım için belirttiğiniz URL gönderir.

Bu kişi için kullanıcılarınızın destek soruları için zaten kullandığı bir e-posta adresi veya Web sitesine ayarlamanızı öneririz.

![Yöneticisine gönderilen e-posta sıfırlamak için örnek istek][Contact]

Aşağıdaki sırayla şu alıcılara iletişim e-posta gönderilir:

1. Varsa **parola Yöneticisi** Yöneticiler bu role sahip bildirim, rol atanır.
2. Parola yönetici yok atanmış ise, ardından yöneticilerine **Kullanıcı Yöneticisi** rolü bildirilir.
3. Önceki rollerin hiçbiri atanırsa, ardından **genel Yöneticiler** bildirilir.

Her durumda en fazla 100 alıcılara bildirim.

Farklı yönetici rolleri ve bunların atama hakkında daha fazla bilgi için bkz: [Azure Active Directory'de yönetici rolleri atama](../users-groups-roles/directory-assign-admin-roles.md).

### <a name="disable-contact-your-administrator-emails"></a>E-postaları "yöneticinize başvurun" devre dışı bırak

Kuruluşunuz, istekleri yöneticilere parola sıfırlama bildirmek istemezse, aşağıdaki yapılandırma etkinleştirebilirsiniz:

* Tüm son kullanıcılar için Self Servis parola sıfırlamayı etkinleştirin. Bu seçenek altında **parola sıfırlama** > **özellikleri**. Kullanıcıların kendi parolalarını sıfırlamasına olmasını istemezseniz, boş bir gruba erişim kapsamını belirleyebilirsiniz. *Bu seçenek önerilmemektedir.*
* Web URL veya mailto sağlamak için Yardım Masası bağlantısını Özelleştir: kullanıcılar, ilgili Yardım almak için kullanabileceği adresi. Bu seçenek altında **parola sıfırlama** > **özelleştirme** > **özel Yardım Masası e-posta veya URL**.

## <a name="customize-the-ad-fs-sign-in-page-for-sspr"></a>SSPR için AD FS oturum açma sayfasını özelleştirme

Active Directory Federasyon Hizmetleri (AD FS) yöneticileri ekleyebileceğiniz bir bağlantı kendi oturum açma sayfasında bulunan yönergeleri kullanarak [Ekle oturum açma sayfası açıklaması](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/add-sign-in-page-description) makalesi.

AD FS oturum açma sayfasına bir bağlantı eklemek için AD FS sunucusunda aşağıdaki komutu kullanın. Kullanıcıların SSPR iş akışı girmek için bu sayfayı kullanabilirsiniz.

``` powershell
Set-ADFSGlobalWebContent -SigninPageDescriptionText "<p><A href='https://passwordreset.microsoftonline.com' target='_blank'>Can’t access your account?</A></p>"
```

## <a name="customize-the-sign-in-page-and-access-panel-look-and-feel"></a>Oturum açma sayfasında ve erişim paneli görünümünü özelleştirme

Oturum açma sayfasını özelleştirebilirsiniz. Şirket markanızı uygun görüntüyü birlikte görüntülenen bir logo ekleyebilirsiniz.

Seçtiğiniz grafikler aşağıdaki durumlarda gösterilir:

* Bir kullanıcı, kullanıcı adlarını girdikten sonra
* Kullanıcı özelleştirilmiş URL'ye erişirse:
   * Geçirerek `whr` parametre parola sıfırlama sayfasına, gibi `https://login.microsoftonline.com/?whr=contoso.com`
   * Geçirerek `username` parametre parola sıfırlama sayfasına, gibi `https://login.microsoftonline.com/?username=admin@contoso.com`

Şirket markası makalesinde yapılandırma konusunda bilgi [şirket Azure AD'de oturum açma sayfanız için markası ekleme](../fundamentals/customize-branding.md).

### <a name="directory-name"></a>Dizin adı

Dizin adı özniteliği altında değiştirebilirsiniz **Azure Active Directory** > **özellikleri**. Portalda ve otomatik iletişim görülür bir kolay kuruluş adı gösterebilirsiniz. Bu seçenek en otomatik e-postalarda gösterilen formları içinde görülebilir:

* E-posta, örneğin "Microsoft CONTOSO tanıtım adına" kolay adı
* Örneğin "CONTOSO tanıtım hesap e-posta doğrulama kodu" e-postadaki konu satırı

## <a name="next-steps"></a>Sonraki adımlar

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](howto-sspr-deployment.md)
* [Parolanızı sıfırlama veya değiştirme](../user-help/active-directory-passwords-update-your-own-password.md)
* [Self servis parola sıfırlama için kaydolma](../user-help/active-directory-passwords-reset-register.md)
* [Lisansla ilgili bir sorunuz mu var?](concept-sspr-licensing.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](howto-sspr-authenticationdata.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](concept-sspr-howitworks.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](concept-sspr-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](howto-sspr-writeback.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](howto-sspr-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](concept-sspr-howitworks.md)
* [Bir arıza olduğunu düşünüyorum. SSPR’de nasıl sorun giderebilirim?](active-directory-passwords-troubleshoot.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)

[Contact]: ./media/concept-sspr-customization/sspr-contact-admin.png "E-posta örnek parola sıfırlama Yardım için yöneticinize başvurun"
