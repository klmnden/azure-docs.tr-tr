---
title: Özelleştirme - Azure Active Directory Self Servis parola sıfırlama
description: Özelleştirme seçenekleri için Azure AD Self Servis parola sıfırlama
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: ffd12d03dffb5deafc8605cc7352bd71d588d235
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
ms.locfileid: "33866745"
---
# <a name="customize-the-azure-ad-functionality-for-self-service-password-reset"></a>Self Servis parola sıfırlama için Azure AD işlevselliği özelleştirme

Self Servis parola sıfırlama (SSPR) dağıtmak için Azure Active directory (Azure AD) isteyen BT uzmanları, kullanıcıların ihtiyaçlarını karşılamak üzere deneyimi özelleştirebilirsiniz.

## <a name="customize-the-contact-your-administrator-link"></a>"Sistem yöneticinize başvurun" bağlantı özelleştirme

SSPR etkinleştirilmemiş olsa bile kullanıcılar hala sıfırlama portalını parola bir "Yöneticinize başvurun" bağlantısına sahip. Bir kullanıcı bu bağlantıyı seçerse, ya da:
   * Yöneticilerinizi e-postalar ve kullanıcının parolasını değiştirme konusunda yardım almak için ister. 
   * Kullanıcılarınızın Yardım için belirttiğiniz bir URL'ye gönderir. 

Bu kişi için bir şeyler kullanıcılarınızın zaten destek soruları için kullandığı bir e-posta adresi veya Web sitesi gibi ayarlamanızı öneririz.

![lgili kişi][Contact]

İletişim e-posta aşağıdaki sırayla aşağıdaki alıcılara gönderilir:

1. Varsa **parola Yöneticisi** rol atanır, Yöneticiler bu rolüne sahip bildirim.
2. Parola yöneticileri atanır varsa, ardından yöneticilerine **Kullanıcı Yöneticisi** rol bildirim.
3. Önceki roller hiçbiri atanırsa, sonra **genel Yöneticiler** bildirilir.

Her durumda en fazla 100 alıcıların bildirim.

Farklı yönetici rolleri ve bunları atama hakkında daha fazla bilgi için bkz: [Azure Active Directory'de yönetici rolleri atama](../active-directory-assign-admin-roles-azure-portal.md).

### <a name="disable-contact-your-administrator-emails"></a>"Sistem yöneticinize başvurun" e-postaları devre dışı bırak

Kuruluşunuz, yöneticiler parola ile ilgili istekleri sıfırlama bildirmek istiyorsanız değil, aşağıdaki yapılandırma etkinleştirebilirsiniz:

* Self Servis parola sıfırlama tüm son kullanıcılarınız için etkinleştirin. Bu seçenek altındadır **parola sıfırlama** > **özellikleri**.
  
  Kullanıcıların kendi parolalarını sıfırlamasına izin olmasını istemezseniz, boş bir grubu erişim kapsamını belirleyebilirsiniz. *Bu seçenek öneririz yok.*
* Bir web URL'si veya mailto sağlamak için Yardım Masası bağlantısı özelleştirme: kullanıcıların Yardım almak için kullanabileceği adresi. Bu seçenek altındadır **parola sıfırlama** > **özelleştirme** > **özel Yardım Masası e-posta veya URL**.

## <a name="customize-the-ad-fs-sign-in-page-for-sspr"></a>SSPR için AD FS oturum açma sayfası özelleştirme

Active Directory Federasyon Hizmetleri (AD FS) Yöneticiler ekleyebilirsiniz bir bağlantı kullanıcıların oturum açma sayfasına bulunan yönergeleri kullanarak [Ekle oturum açma sayfası açıklaması](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/add-sign-in-page-description) makalesi.

AD FS oturum açma sayfasına bir bağlantı eklemek için AD FS sunucusunda aşağıdaki komutu kullanın. Kullanıcılar, SSPR iş akışı girmek için bu sayfayı kullanabilirsiniz.

``` Set-ADFSGlobalWebContent -SigninPageDescriptionText "<p><A href=’https://passwordreset.microsoftonline.com’>Can’t access your account?</A></p>" ```

## <a name="customize-the-sign-in-page-and-access-panel-look-and-feel"></a>Oturum açma sayfası ve erişim panelinde görünüm özelleştirme

Oturum açma sayfasını özelleştirebilirsiniz. Şirket markası uygun görüntüyü birlikte görüntülenen bir logo ekleyebilirsiniz.

Aşağıdaki durumlarda, seçtiğiniz grafik gösterilmektedir:

* Bir kullanıcı kullanıcı adlarını girdikten sonra
* Kullanıcı özelleştirilmiş URL erişirse:
    * Geçirerek *ws* parola parametresi sıfırla sayfası, olduğu gibi "https://login.microsoftonline.com/?whr=contoso.com"
    * Geçirerek *kullanıcıadı* parola parametresi sıfırla sayfası, olduğu gibi "https://login.microsoftonline.com/?username=admin@contoso.com"

Şirket markası makalesinde yapılandırma ayrıntılarını bulun [şirket Azure AD'de, oturum açma sayfasına markası ekleme](../customize-branding.md).

### <a name="directory-name"></a>Dizin adı

Dizin adı özniteliği altında değiştirebileceğiniz **Azure Active Directory** > **özellikleri**. Portal ve otomatik iletişimleri görülen bir kolay kuruluş adı gösterebilir. Bu seçenek otomatik e-postalar izleyin formlarda en görünür olur:

* Örneğin "Microsoft CONTOSO tanıtım adına" e-postadaki kolay adı
* Örneğin "CONTOSO demo hesabı e-posta doğrulama kodu" e-postadaki konu satırı

## <a name="next-steps"></a>Sonraki adımlar

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](howto-sspr-deployment.md)
* [Parolanızı sıfırlama veya değiştirme](../active-directory-passwords-update-your-own-password.md)
* [Self servis parola sıfırlama için kaydolma](../active-directory-passwords-reset-register.md)
* [Lisansla ilgili bir sorunuz mu var?](concept-sspr-licensing.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](howto-sspr-authenticationdata.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](concept-sspr-howitworks.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](concept-sspr-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](howto-sspr-writeback.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](howto-sspr-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](concept-sspr-howitworks.md)
* [Bir arıza olduğunu düşünüyorum. SSPR’de nasıl sorun giderebilirim?](active-directory-passwords-troubleshoot.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)

[Contact]: ./media/concept-sspr-customization/sspr-contact-admin.png "Parola e-posta örnek sıfırlama Yardım için yöneticinize başvurun"
