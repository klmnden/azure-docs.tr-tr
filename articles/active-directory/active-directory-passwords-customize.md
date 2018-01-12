---
title: "Özelleştirme - Azure Active Directory Self Servis parola sıfırlama"
description: "Özelleştirme seçenekleri için Azure AD Self Servis parola sıfırlama"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 6d8a2f2106e57bdf84bc3bead70d379691b79742
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="customize-the-azure-ad-functionality-for-self-service-password-reset"></a>Self Servis parola sıfırlama için Azure AD işlevselliği özelleştirme

Self Servis parola sıfırlama (SSPR) dağıtmak için Azure Active directory (Azure AD) isteyen BT uzmanları, kullanıcıların ihtiyaçlarını karşılamak üzere deneyimi özelleştirebilirsiniz.

## <a name="customize-the-contact-your-administrator-link"></a>"Sistem yöneticinize başvurun" bağlantı özelleştirme

SSPR etkinleştirilmemiş olsa bile kullanıcılar hala sıfırlama portalını parola bir "Yöneticinize başvurun" bağlantısına sahip. Bir kullanıcı bu bağlantıyı seçerse, ya da:
   * Yöneticilerinizi e-postalar ve kullanıcının parolasını değiştirme konusunda yardım almak için ister. 
   * Kullanıcılarınızın Yardım için belirttiğiniz bir URL'ye gönderir. 

Bu kişi için bir şeyler kullanıcılarınızın zaten destek soruları için kullandığı bir e-posta adresi veya Web sitesi gibi ayarlamanızı öneririz.

![İlgili kişi][Contact]

İletişim e-posta aşağıdaki sırayla aşağıdaki alıcılara gönderilir:

1. Varsa **parola Yöneticisi** rol atanır, Yöneticiler bu rolüne sahip bildirim.
2. Parola yöneticileri atanır varsa, ardından yöneticilerine **Kullanıcı Yöneticisi** rol bildirim.
3. Önceki roller hiçbiri atanırsa, sonra **genel Yöneticiler** bildirilir.

Her durumda en fazla 100 alıcıların bildirim.

Farklı yönetici rolleri ve bunları atama hakkında daha fazla bilgi için bkz: [Azure Active Directory'de yönetici rolleri atama](active-directory-assign-admin-roles-azure-portal.md).

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
    * Geçirerek *ws* parametresi parolayı Sıfırla "https://login.microsoftonline.com/?whr=contoso.com" gibi sayfa
    * Geçirerek *kullanıcıadı* parola parametresi sıfırla sayfası, olduğu gibi "https://login.microsoftonline.com/?username=admin@contoso.com"

### <a name="graphics-details"></a>Grafik ayrıntıları

Oturum açma sayfasının görsel özelliklerini değiştirmek için aşağıdaki ayarları kullanın. Git **Azure Active Directory** > **şirket markası** > **düzenleme şirket markası**:

* Oturum açma sayfası görüntüsü bir .png veya .jpg dosyası 1420 x 1200 piksel ve 500 KB'den büyük olmalıdır. En iyi sonuçlar için yaklaşık 200 KB tutmanızı öneririz.
* Oturum açma sayfası arka plan rengi üzerinde yüksek Gecikmeli bağlantılar kullanılır ve RGB onaltılık biçimde olmalıdır.
* Başlık resmi bir .png veya .jpg dosyası 60 x 280 piksel olması ve 10 KB'den büyük olmaması gerekir.
* (Normal açık ve koyu renkli tema) kare logo bir .png veya .jpg dosyası 240 x 240 (yeniden boyutlandırılabilir) piksel ve 10 KB'den büyük olmalıdır.

### <a name="sign-in-text-options"></a>Metin oturum açma seçenekleri

Kuruluşunuz için uygun olan oturum açma sayfası metni eklemek için aşağıdaki ayarları kullanın. Git **Azure Active Directory** > **şirket markası** > **düzenleme şirket markası**:

* **Kullanıcı adı İpucu**: örnek metnini değiştirir  *someone@example.com*  kullanıcılarınız için daha uygun bir şeyle. İç ve dış kullanıcılar desteklediğinde, varsayılan ipucu bırakmanız önerilir.
* **Oturum açma sayfası metni**: en fazla 256 karakter uzunluğunda olabilir. Bu metin herhangi bir yerde çevrimiçi ve Windows 10 Azure AD çalışma alanına katılma deneyimi kullanıcılarınız oturum görüntülenir. Bu metin koşullarını kullanın, yönergeler ve kullanıcılarınız için ipuçları için kullanın. 

   >[!IMPORTANT]
   >Herkes herhangi bir önemli bilgi burada sağlamıyorsa için oturum açma sayfanız, görebilirsiniz.
   >

### <a name="the-keep-me-signed-in-disabled-setting"></a>"Bana imzalı Koru devre dışı" ayarı

İle **Oturumumu devre dışı açık tutmak** seçeneği, kullanıcıların kalır oturum açmış durumda kapatın ve bunların tarayıcı penceresini yeniden açın. Bu seçenek oturum yaşam etkilemez. Git **Azure Active Directory** > **şirket markası** > **düzenleme şirket markası**.

SharePoint Online ve Office 2010 özelliklerinden bazıları bu onay kutusunu seçmek için kullanıcıların yeteneğini bir bağımlılık vardır. Bu seçenek Gizle kullanıcılar ek ve beklenmeyen oturum açma komut istemlerini edinebilirsiniz.

### <a name="directory-name"></a>Dizin adı

Dizin adı özniteliği altında değiştirebileceğiniz **Azure Active Directory** > **özellikleri**. Portal ve otomatik iletişimleri görülen bir kolay kuruluş adı gösterebilir. Bu seçenek otomatik e-postalar izleyin formlarda en görünür olur:

* Örneğin "Microsoft CONTOSO tanıtım adına" e-postadaki kolay adı
* Örneğin "CONTOSO demo hesabı e-posta doğrulama kodu" e-postadaki konu satırı

## <a name="next-steps"></a>Sonraki adımlar

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](active-directory-passwords-best-practices.md)
* [Parolanızı sıfırlama veya değiştirme](active-directory-passwords-update-your-own-password.md)
* [Self servis parola sıfırlama için kaydolma](active-directory-passwords-reset-register.md)
* [Lisansla ilgili bir sorunuz mu var?](active-directory-passwords-licensing.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](active-directory-passwords-data.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](active-directory-passwords-how-it-works.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](active-directory-passwords-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](active-directory-passwords-writeback.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](active-directory-passwords-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](active-directory-passwords-how-it-works.md)
* [Bir arıza olduğunu düşünüyorum. SSPR’de nasıl sorun giderebilirim?](active-directory-passwords-troubleshoot.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)

[Contact]: ./media/active-directory-passwords-customize/sspr-contact-admin.png "Parola e-posta örnek sıfırlama Yardım için yöneticinize başvurun"
