---
title: Self Servis parola sıfırlama için - Azure Active Directory'ye kaydetme | Microsoft Docs
description: Azure AD Self Servis parola için kimlik doğrulama verilerini kaydetme Sıfırla
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: sahenry
ms.assetid: ''
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 01/11/2018
ms.author: lizross
ms.collection: M365-identity-device-management
ms.openlocfilehash: f113c42ff45811f31eb77a92873fb187dd51ea6b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60530889"
---
# <a name="register-for-self-service-password-reset"></a>Self servis parola sıfırlama için kaydolma

> [!IMPORTANT]
> Oturum açılamıyor çünkü burada misiniz? Öyleyse bkz [iş veya Okul parolanızı sıfırlama](active-directory-passwords-update-your-own-password.md).

Bir son kullanıcı olarak parolanızı sıfırlama veya Azure Active Directory (Azure AD) Self Servis parola sıfırlama (SSPR) kullanırsanız kendiniz hesabınızın kilidini. Bu işlevi kullanabilmeniz için önce kimlik doğrulama yöntemlerini kaydetmeniz veya yöneticinizin doldurduğu önceden tanımlı kimlik doğrulama yöntemlerini onaylamanız gerekir.

## <a name="register-or-confirm-authentication-data-with-sspr"></a>SSPR ile kimlik doğrulama verilerini kaydetme veya onaylama

1. Cihazınızdaki web tarayıcısını açın ve gidin [parola sıfırlama kayıt sayfasına](https://aka.ms/ssprsetup).
2. Kullanıcı adınızı ve yöneticiniz sağlanan parolayı girin.
3. BT personelinizin şeyleri nasıl yapılandırdığına bağlı olarak bir veya daha fazla aşağıdaki seçenekleri yapılandırmak ve doğrulamak için kullanılabilir. Yöneticiniz bilgilerinizi kullanma izni varsa, bunlar bazı bilgiler sizin için doldurabilir.
    * **Ofis telefonu**: Bu seçenek yalnızca yöneticiniz ayarlayabilirsiniz.
    * **Kimlik doğrulama telefonu**: Erişimi olan başka bir telefon numarası için bu seçeneği ayarlayın. Bir metin veya çağrı alabilen bir cep telefonu buna bir örnektir.
    * **Kimlik doğrulama e-posta**: Bu seçenek, sıfırlamak istediğiniz parolayı kullanmadan erişebileceğiniz alternatif e-posta adresine ayarlayın.
    * **Güvenlik sorularını**: Yöneticinizin bu liste soruları yanıtlamanız için onayladığı. Aynı soru veya yanıt birden çok kez kullanılamaz.
4. Yöneticinizin gerektirdiği bilgileri doğrulayın ve sağlar. Birden fazla seçeneği varsa, birden çok yöntem kaydetmenizi öneririz. Yöntemlerin biri kullanılamadığında Bu size esneklik sağlar. Seyahat ve ofis telefonunuzu erişemiyor bir örnek verilmiştir.

    ![Kimlik doğrulama yöntemlerini kaydedin ve Son'u seçin][Register]

5. Seçin **son**. SSPR artık gelecekte ihtiyacınız olduğunda kullanabilirsiniz.

İçin veri girerseniz **kimlik doğrulama telefonu** veya **kimlik doğrulama e-posta**, genel dizinde görünmez değil. Bu verileri yalnızca siz ve yöneticileriniz görebilir. Güvenlik sorularınızın yanıtlarını yalnızca siz görebilirsiniz.

Yöneticiler, yine de uygun yöntemleri kayıtlı olduğundan emin olmak için zaman bir süre sonra kimlik doğrulama yöntemlerinizi onaylamanızı gerektirebilir.

## <a name="common-problems-and-their-solutions"></a>Sık karşılaşılan sorunlar ve çözümleri

 Bazı yaygın hata durumları ve çözümleri aşağıda verilmiştir:

| Bir hata durumu| Hatayı?| Çözüm |
| --- | --- | --- |
| Kullanıcı Kimliğimi girdikten sonra bir "Lütfen sistem yöneticinize başvurun" Sayfa alıyorum | Lütfen yöneticinize başvurun. <br> <br> Kullanıcı hesabınızın parolasının, Microsoft tarafından yönetilmiyor algıladık. Sonuç olarak, otomatik olarak parolanızı sıfırlama belirleyemiyoruz. <br> <br> Daha fazla yardım için BT personelinizin başvurun. | BT personelinizin şirket içi ortamınızda parolanızı yönetir ve, parolasını sıfırlamanıza izin vermiyor bu iletisini görüyorsunuz **hesabınıza erişemiyor** bağlantı. <br> <br> Parolanızı sıfırlamak için doğrudan Yardım için BT personelinizin başvurun. Bunlar bu özellik, verebilmeniz için parolanızı sıfırlamak istediğiniz bildiren olanak tanır.|
| Kullanıcı Kimliğimi girdikten sonra bir "hesabınızı parola sıfırlama için etkin değil" hatası alıyorum | Hesabınız parola sıfırlama için etkinleştirilmedi. <br> <br> Özür dileriz, ancak BT personeliniz bu hizmeti kullanmak için hesabınızı ayarlamadı. <br> <br> İsterseniz, biz, parolanızı sıfırlaması için kuruluşunuzdaki yönetici başvurabilirsiniz. | BT personelinizin parola sıfırlama kuruluşunuz için etkinleştirilmemiş olduğundan bu iletisini görüyorsunuz **hesabınıza erişemiyor** bağlantı veya bu özelliği kullanmak için lisanslı edilmemiş. <br> <br> Parolanızı sıfırlamak için seçin **yöneticiye başvurun** bağlantı. Şirketinizin e-posta gönderilecek BT personeli. E-posta biliyorsanız, bu özellik, verebilmeniz için parolanızı sıfırlamak istiyorsanız sağlar. |
| Kullanıcı Kimliğimi girdikten sonra bir "biz hesabınızı doğrulanamadı" hatası alıyorum | Hesabınızı doğrulayamıyor. <br> <br> İsterseniz, biz, parolanızı sıfırlaması için kuruluşunuzdaki yönetici başvurabilirsiniz. | Bu ileti, parola sıfırlama için etkinleştirildiğinden, ancak hizmeti kullanmaya kaydolmadıysanız görüyorsunuz. Parola sıfırlama için kaydolmasını gidin [parola sıfırlama kayıt sayfasına](https://aka.ms/ssprsetup) hesabınıza erişim buldum sonra. <br> <br> Parolanızı sıfırlamak için seçin **yöneticiye başvurun** , şirketinizin e-posta göndermek için bağlantı BT personeli. |

## <a name="next-steps"></a>Sonraki adımlar

* [Self Servis parola sıfırlamayı kullanarak parolanızı değiştirme](active-directory-passwords-update-your-own-password.md)
* [Parola sıfırlama kayıt sayfası](https://aka.ms/ssprsetup)
* [Parola sıfırlama portalı](https://passwordreset.microsoftonline.com/)
* [Microsoft hesabınızda oturum açtığınızda olamaz](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant)

[Register]: ./media/active-directory-passwords-reset-register/register-2-methods.png "Kayıtlı yöntemleri ve son düğmesini gösteren parola sıfırlama kayıt sayfası"

