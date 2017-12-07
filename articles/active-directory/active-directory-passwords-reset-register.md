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
ms.openlocfilehash: 6fff5b31e70b0782aaf87e6bb4252f0c939d4c72
ms.sourcegitcommit: 7f1ce8be5367d492f4c8bb889ad50a99d85d9a89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
---
# <a name="register-for-self-service-password-reset"></a>Self servis parola sıfırlama için kaydolma

> [!IMPORTANT]
> Oturum açamazsınız çünkü burada misiniz? Aksi takdirde bkz [iş veya Okul parolanızı sıfırlama](active-directory-passwords-update-your-own-password.md).

Bir son kullanıcı parolanızı sıfırlama veya kendiniz tarafından Azure Active Directory (Azure AD) Self Servis parola sıfırlama (SSPR) kullanıyorsanız, hesabınızın kilidini. Bu işlevi kullanabilmek için kimlik doğrulama yöntemleri kaydetmek veya yöneticinize doldurulmuş önceden tanımlanmış kimlik doğrulama yöntemlerini onaylayın gerekir.

## <a name="register-or-confirm-authentication-data-with-sspr"></a>SSPR ile kimlik doğrulama verilerini kaydetme veya onaylama

1. Cihazınızda web tarayıcısını açın ve gidin [parola sıfırlama kayıt sayfası](http://aka.ms/ssprsetup).
2. Kullanıcı adınızı ve yöneticiniz sağlanan parolayı girin.
3. BT personeliniz şeyleri nasıl yapılandırdığına bağlı olarak bir veya daha fazla aşağıdaki seçenekleri yapılandırın ve doğrulamak için kullanılabilir. Yöneticiniz, kişisel bilgilerinizi kullanma izni varsa, bunlar için bu bilgileri bazıları doldurabilirsiniz.
    * **Ofis telefonu**: yalnızca yöneticiniz bu seçenek belirleyebilirsiniz.
    * **Kimlik doğrulama telefon**: erişiminiz başka bir telefon numarası için bu seçeneği belirleyin. Bir metin ya da bir çağrı alabileceği bir cep telefonu örneğidir.
    * **Kimlik doğrulama e-posta**: sıfırlamak istediğinizden parola kullanmadan erişebileceğiniz bir alternatif e-posta adresi için bu seçeneği belirleyin.
    * **Güvenlik soruları**: yöneticiniz bu soruları yanıtlamak size listesi onayladı. Aynı soruyu kullanın veya birden çok kez yanıtlayın.
4. Yöneticiniz gerektirir bilgilerini doğrulayın ve sağlar. Birden fazla seçeneği kullanılabilir durumdaysa, birden çok yöntem kaydetmenizi öneririz. Yöntemlerden birini kullanılamadığında Bu size esneklik sağlar. Seyahat ve ofis telefonunuza erişemiyor örneğidir.

    ![Kimlik doğrulama yöntemleri kaydetmek ve Son'u seçin][Register]

5. Seçin **son**. Gelecekte için gerektiğinde SSPR artık kullanabilirsiniz.

Verileri girerseniz **kimlik doğrulama telefon** veya **kimlik doğrulama e-posta**, genel dizinde görünür değil. Bu verileri yalnızca siz ve yöneticileriniz görebilir. Yalnızca güvenlik sorularınıza görebilirsiniz.

Yöneticilerinizi, kimlik doğrulama yöntemleri bir süre kayıtlı uygun yöntemleri çözümlenmedi emin olmak için sonra onaylamak gerektirebilir.

## <a name="common-problems-and-their-solutions"></a>Ortak sorunlar ve çözümleri

 Bazı ortak hata durumları ve bunların çözümleri şunlardır:

| Hata durumu| Hangi hata görürüm?| Çözüm |
| --- | --- | --- |
| Kullanıcı Kimliğimi girdikten sonra "Lütfen sistem yöneticinize başvurun" sayfası alıyorum | Lütfen sistem yöneticinize başvurun. <br> <br> Kullanıcı hesabı parolanızı Microsoft tarafından yönetilmediğini algıladık. Sonuç olarak, otomatik olarak parolanızı sıfırlamak alamıyoruz. <br> <br> Daha fazla yardım için BT personeliniz başvurun. | Çünkü, BT personeliniz parolanızı, şirket içi ortamınızda yönetir ve gelen parolanızı sıfırlamak izin vermiyor bu iletiyi görüyorsunuz **hesabınıza erişemiyor** bağlantı. <br> <br> Parolanızı sıfırlamak için doğrudan Yardım için BT personeliniz başvurun. Bunlar bu özellik sizin için etkinleştirebileceğiniz şekilde parolanızı sıfırlamak istediğiniz bildirmek.|
| Kullanıcı Kimliğimi girdikten sonra bir "hesabınız parola sıfırlama için etkin değil" hatası alıyorum | Hesabınız parola sıfırlama için etkinleştirilmedi. <br> <br> Üzgünüz, ancak, BT personeliniz hesabınız bu hizmeti kullanacak şekilde ayarlama değil. <br> <br> İsterseniz, biz parolanızı sıfırlaması için kuruluşunuzdaki yönetici başvurabilirsiniz. | BT personeliniz parola sıfırlama kuruluşunuzdan için etkin değil çünkü bu iletiyi görüyorsunuz **hesabınıza erişemiyor** bağlantı veya bu özelliği kullanmak için lisanslı kurmadı. <br> <br> Parolanızı sıfırlamak üzere seçin **bir yöneticiye başvurun** bağlantı. Bir e-posta, şirketinizin gönderilecek BT personeli. E-posta bunları biliyorsanız, bu özellik sizin için etkinleştirebileceğiniz şekilde parolanızı sıfırlamak istiyorsanız olanak sağlar. |
| Kullanıcı Kimliğimi girdikten sonra bir "biz hesabınızı doğrulanamadı" hatası alıyorum | Hesabınız doğrulanamıyor. <br> <br> İsterseniz, biz parolanızı sıfırlaması için kuruluşunuzdaki yönetici başvurabilirsiniz. | Parola sıfırlama için etkin ancak hizmeti kullanmak için kayıtlı olmayabilirsiniz çünkü bu iletiyi görüyorsunuz. Parola sıfırlama için kaydolmak için Git [parola sıfırlama kayıt sayfası](http://aka.ms/ssprsetup) hesabınıza erişim artık sonra. <br> <br> Parolanızı sıfırlamak üzere seçin **bir yöneticiye başvurun** , şirketinizin e-posta göndermek için bağlantı BT personeli. |

## <a name="next-steps"></a>Sonraki adımlar

* [Self Servis parola sıfırlamayı kullanarak parolanızı değiştirme](active-directory-passwords-update-your-own-password.md)
* [Parola sıfırlama kayıt sayfası](http://aka.ms/ssprsetup)
* [Parola sıfırlama portalı](https://passwordreset.microsoftonline.com/)
* [Ne zaman da Microsoft hesabınızda oturum açamazsınız](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant)

[Register]: ./media/active-directory-passwords-reset-register/register-2-methods.png "Kayıtlı yöntemleri ve son düğmesini gösteren parola sıfırlama kayıt sayfası"

