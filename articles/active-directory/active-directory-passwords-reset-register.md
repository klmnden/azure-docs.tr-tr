---
title: "Azure AD: SSPR kaydı | Microsoft Docs"
description: "Kimlik doğrulama verileri için Azure AD Self Servis parola kaydı Sıfırla"
services: active-directory
keywords: 
documentationcenter: 
author: barlanmsft
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: barlan
ms.custom: end-user
ms.openlocfilehash: b884f8bc3a20052fa0cb40772deef591b69d7b10
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="register-for-self-service-password-reset"></a>Self servis parola sıfırlama için kaydolma

> [!IMPORTANT]
> **Oturum açmada sorun yaşadığınız için mi buradasınız?** Sorun yaşıyorsanız bkz. [kendi parolanızı değiştirme ve sıfırlama](active-directory-passwords-update-your-own-password.md).

Bir son kullanıcı parolanızı sıfırlama veya Self Servis parola sıfırlama (SSPR) kullanan bir kişi için seslendir gerek kalmadan hesabınızın kilidini açın. Bu işlevi kullanabilmeniz için önce kimlik doğrulama yöntemlerini kaydetmeniz veya yöneticinizin doldurduğu önceden tanımlı kimlik doğrulama yöntemlerini onaylamanız gerekir.

## <a name="register-or-confirm-authentication-data-with-sspr"></a>SSPR ile kimlik doğrulama verilerini kaydetme veya onaylama

1. Cihazınızda bir web tarayıcısı açıp [parola sıfırlama kayıt sayfasına](http://aka.ms/ssprsetup) gidin
2. Yöneticiniz tarafından sağlanan kullanıcı adınızı ve parolanızı girin
3. BT personeliniz şeyleri nasıl yapılandırdığınıza bağlı olarak bir veya daha fazla aşağıdaki seçenekleri yapılandırın ve doğrulamak için kullanılabilir. Bilgileri kullanma izni varsa yöneticinize bu bazıları sizin için doldurmak.
    * Ofis telefonu yalnızca yöneticiniz tarafından ayarlanan yapabiliyor
    * Kimlik doğrulama telefon erişebilirsiniz bir metin veya arama alabileceği bir cep telefonu ister başka bir telefon numarası olarak ayarlanmalıdır.
    * Kimlik doğrulama e-posta sıfırlamanız gerekirse parola olmadan erişebileceği bir alternatif e-posta adresi ayarlamanız gerekir.
    * Güvenlik soruları soruları yanıtlamak için yöneticinize onayladıktan listesini sağlar. Değil, aynı soruyu kullanın veya birden çok kez yanıt.
4. Yöneticiniz tarafından gerekli kılınan bilgileri sağlayın ve doğrulayın. Birden fazla seçeneği kullanılabilir durumdaysa, başka bir yöntem kullanılamadığında esneklik sağlamak için birden çok yöntem kaydetmenizi öneririz (örnek: seyahat ediyor ve ofis telefonunuza erişilemiyor)

    ![Kimlik doğrulama yöntemlerini kaydedin ve Son’a tıklayın][Register]

5. 4. adımı tamamlayıp **Son**’u seçtiğinizde, artık gelecekte ihtiyacınız olması durumunda self servis parola sıfırlama özelliğini kullanabilirsiniz.

Kimlik Doğrulama Telefonu veya Kimlik Doğrulama E-postası alanına veri girerseniz, bu veriler genel dizinde görünmez. Bu verileri yalnızca siz ve yöneticileriniz görebilir. Güvenlik Sorularınızın yanıtlarını yalnızca siz görebilirsiniz.

Yöneticiler, uygun yöntemleri kayıtlı tuttuğunuzdan emin olmak için belirli bir dönem sonunda kimlik doğrulama yöntemlerinizi onaylamanızı gerektirebilir.

## <a name="common-problems-and-their-solutions"></a>Ortak sorunlar ve çözümleri

 Bazı ortak hata durumları ve bunların çözümleri şunlardır:

| Hata durumu| Hangi hata görürüm?| Çözüm |
| --- | --- | --- |
| Kullanıcı Kimliğimi girdikten sonra "Lütfen sistem yöneticinize başvurun" sayfası alıyorum | Lütfen sistem yöneticinize başvurun <br> <br> Kullanıcı hesabı parolanızı Microsoft tarafından yönetilmediğini algıladık. Sonuç olarak, otomatik olarak parolanızı sıfırlamak alamıyoruz. <br> <br> Daha fazla yardım için BT personeliniz başvurmanız gerekir. | BT personeliniz parolanızı, şirket içi ortamınızda yönetir ve gelen parolanızı sıfırlamak izin vermez, bu iletiyi görüyorsunuz hesap bağlantısı erişemiyor. <br> <br> Parolanızı sıfırlamak için doğrudan Yardım için BT personeliniz başvurun ve bunlar bu özellik sizin için etkinleştirebileceğiniz şekilde parolanızı sıfırlamak istediğiniz bildirmek.|
| Kullanıcı Kimliğimi girdikten sonra bir "hesabınız parola sıfırlama için etkin değil" hatası alıyorum | Hesabınız parola sıfırlama için etkinleştirilmedi <br> <br> Üzgünüz, ancak, BT personeliniz hesabınız bu hizmeti kullanacak şekilde ayarlama değil. <br> <br> İsterseniz, biz parolanızı sıfırlaması için kuruluşunuzdaki yönetici başvurabilirsiniz. | BT personeliniz parola sıfırlama kuruluşunuzdan için etkin değil çünkü bu iletiyi görüyorsunuz hesap bağlantısı erişemez veya bu özelliği kullanmak için lisanslı kurmadı. <br> <br> Parolanızı sıfırlamak için şirket için bir e-posta göndermek için bir yönetici bağlantısı kişiyi tıklatın BT personeli, adı ve bunlar bu özellik sizin için etkinleştirebileceğiniz şekilde parolanızı sıfırlamak istediğiniz bildirmek. |
| Kullanıcı Kimliğimi girdikten sonra bir "biz hesabınızı doğrulanamadı" hatası alıyorum | Hesabınız doğrulanamıyor <br> <br> İsterseniz, biz parolanızı sıfırlaması için kuruluşunuzdaki yönetici başvurabilirsiniz. | Parola sıfırlama için etkinleştirilir, ancak hizmeti kullanmak için kaydolmadınız için bu iletiyi görüyorsunuz. Parola sıfırlama için kaydetme için hesabınıza erişim artık sonra için http://aka.ms/ssprsetup gidin. <br> <br> Parolanızı sıfırlamak için şirketinizin e-posta göndermek için bir yönetici bağlantısı kişiyi tıklatın BT personeli. |

## <a name="next-steps"></a>Sonraki Adımlar

* [Self Servis parola sıfırlamayı kullanarak parolanızı değiştirme](active-directory-passwords-update-your-own-password.md)
* [Parola sıfırlama kayıt sayfası](http://aka.ms/ssprsetup)
* [Parola sıfırlama portalı](https://passwordreset.microsoftonline.com/)
* [Microsoft hesabınızla oturum açamıyorsunuz](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant)

[Register]: ./media/active-directory-passwords-reset-register/register-2-methods.png "Kayıtlı yöntemleri ve son düğmesini gösteren parola sıfırlama kayıt sayfası"
