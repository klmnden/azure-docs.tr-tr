---
title: "Azure AD: Self servis parola sıfırlama kaydı | Microsoft Docs"
description: "Self servis parola sıfırlama için kimlik doğrulama verilerini kaydetme"
services: active-directory
keywords: "Active directory parola yönetimi, parola yönetimi, Azure AD self servis parola sıfırlama, SSPR"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/16/2017
ms.author: joflore
ms.custom: end-user
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 0f10ad4904b10a9554e949e77f0192edcb8f624f
ms.lasthandoff: 04/12/2017


---
# <a name="register-for-self-service-password-reset"></a>Self servis parola sıfırlama için kaydolma

Yöneticiniz tarafından bu özellik etkinleştirdiyse, bir son kullanıcı olarak, kimseyle konuşmak zorunda kalmadan self servis parola sıfırlama (SSPR) ile parolanızı sıfırlayabilir veya hesabınızın kilidini açabilirsiniz. Bu işlevi kullanabilmeniz için önce kimlik doğrulama yöntemlerini kaydetmeniz veya yöneticinizin doldurduğu önceden tanımlı kimlik doğrulama yöntemlerini onaylamanız gerekir.

## <a name="register-or-confirm-authentication-data-with-sspr"></a>SSPR ile kimlik doğrulama verilerini kaydetme veya onaylama

1. Cihazınızda bir web tarayıcısı açıp [parola sıfırlama kayıt sayfasına](http://aka.ms/ssprsetup) gidin
2. Yöneticiniz tarafından sağlanan kullanıcı adınızı ve parolanızı girin
3. Yöneticinizin onayladığı seçeneklere bağlı olarak, parolanızı sıfırlamanız gerekirse kullanmak üzere yapılandırabileceğiniz veya doğrulayabileceğiniz aşağıdaki öğelerden birini veya daha fazlasını görürsünüz
    * Ofis telefonu: Bu seçenek yalnızca yöneticiniz tarafından ayarlanabilir
    * Kimlik Doğrulama Telefonu: Bu seçenek, erişebildiğiniz başka bir telefon numarası (SMS veya çağrı alabilen bir cep telefonu gibi) olarak ayarlanmalıdır. Yöneticiniz cep telefonu numaranızı kullanma iznine zaten sahipse, bu alanı sizin için doldurabilir.
    * Kimlik Doğrulama E-postası: Bu seçenek, sıfırlamanız gereken parolayı kullanmadan erişebileceğiniz alternatif bir e-posta adresi olarak ayarlanmalıdır
    * Güvenlik Soruları: Bu seçenek, yöneticinizin yanıtlamanız için onayladığı soruların listesini sunar. Birden çok soru için aynı yanıtı kullanamazsınız.
4. Yöneticiniz tarafından gerekli kılınan bilgileri sağlayın ve doğrulayın. Birden çok seçenek varsa, başka bir yöntemin kullanılamaz olduğu durumlarda (örneğin, seyahat ettiğiniz için ofis telefonunuza erişememe) esneklik sağlamak için birden çok yöntem kaydetmenizi öneririz

    ![Kimlik doğrulama yöntemlerini kaydedin ve Son’a tıklayın][Register]

5. 4. adımı tamamlayıp **Son**’u seçtiğinizde, artık gelecekte ihtiyacınız olması durumunda self servis parola sıfırlama özelliğini kullanabilirsiniz.

Kimlik Doğrulama Telefonu veya Kimlik Doğrulama E-postası alanına veri girerseniz, bu veriler genel dizinde görünmez. Bu verileri yalnızca siz ve yöneticileriniz görebilir. Güvenlik Sorularınızın yanıtlarını yalnızca siz görebilirsiniz.

Yöneticiler, uygun yöntemleri kayıtlı tuttuğunuzdan emin olmak için belirli bir dönem sonunda kimlik doğrulama yöntemlerinizi onaylamanızı gerektirebilir.

## <a name="next-steps"></a>Sonraki Adımlar

* [Self Servis parola sıfırlamayı kullanarak parolanızı değiştirme](active-directory-passwords-update-your-own-password.md)
* [Parola sıfırlama kayıt sayfası](http://aka.ms/ssprsetup)
* [Parola sıfırlama portalı](https://passwordreset.microsoftonline.com/)

[Register]: ./media/active-directory-passwords-reset-register/register-2-methods.png "Kayıtlı yöntemleri ve son düğmesini gösteren parola sıfırlama kayıt sayfası"

