---
title: Uygulama kaydı - Azure Active Directory B2C
description: Uygulamanızı Azure Active Directory B2C'ye kaydetme
services: active-directory-b2c
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: get-started-article
ms.date: 6/13/2017
ms.author: davidmu
ms.openlocfilehash: 8ba511464f8ce0bef2a14706a272f6c09dfe5d07
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="azure-active-directory-b2c-register-your-application"></a>Azure Active Directory B2C: Uygulamanızı kaydetme

Bu Hızlı Başlangıç, bir Microsoft Azure Active Directory (Azure AD) B2C Kiracısında bir uygulamayı birkaç dakika içinde kaydetmenize yardımcı olur. İşiniz bittiğinde, uygulamanız Azure AD B2C Kiracısında kullanım için kaydedilir.

## <a name="prerequisites"></a>Ön koşullar

Tüketicinin kaydolmasını ve oturum açmasını kabul eden bir uygulama oluşturmak için öncelikle uygulamayı Azure Active Directory B2C kiracısına kaydetmeniz gerekir. [Azure AD B2C kiracısı oluşturma](active-directory-b2c-get-started.md) makalesinde ana hatlarıyla belirtilen adımları izleyerek kendi kiracınızı edinin.

Azure portalında oluşturulan uygulamaların aynı konumdan yönetilmesi gerekir. Azure AD B2C uygulamalarını PowerShell veya başka bir portal kullanarak düzenlerseniz bu uygulamalar desteklenmez duruma gelir ve Azure AD B2C ile çalışmaz. [Hatalı uygulamalar](#faulted-apps) bölümünden ayrıntılara bakabilirsiniz. 

Bu makalede örneklerimizle çalışmaya başlamanıza yardımcı olacak örnekler kullanır. Bu örnekler hakkında sonraki makalelerinde daha fazla bilgi edinebilirsiniz.

## <a name="navigate-to-b2c-settings"></a>B2C ayarlarına gidin

[Azure portalında](https://portal.azure.com/) B2C kiracısının Genel Yöneticisi olarak oturum açın. 

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](../../includes/active-directory-b2c-portal-navigate-b2c-service.md)]

## <a name="choose-next-steps-based-on-your-application-type"></a>Uygulama türüne göre sonraki adımları seçin

* [Web uygulaması kaydetme](#register-a-web-app)
* [Web API’si kaydetme](#register-a-web-api)
* [Mobil veya yerel bir uygulamayı kaydetme](#register-a-mobile-or-native-app)
 
### <a name="register-a-web-app"></a>Web uygulaması kaydetme

[!INCLUDE [active-directory-b2c-register-web-app](../../includes/active-directory-b2c-register-web-app.md)]

### <a name="create-a-web-app-client-secret"></a>Web uygulaması gizli anahtarı oluşturma

Web uygulamanız Azure AD B2C tarafından güvence altına alınmış bir web API'sini çağırıyorsa, aşağıdaki adımları gerçekleştirin:
   1. **Anahtarlar** dikey penceresine gidip **Anahtar Oluştur** düğmesine tıklayarak bir uygulama gizli dizisi oluşturun. **Uygulama anahtarı** değerini not edin. Bu değeri, uygulamanızın kodunda uygulama gizli dizisi olarak kullanırsınız.
   2. **API Erişimi**’ne ve **Ekle**’ye tıklayıp web API’si ve kapsamlarınızı (izinler) seçin.

> [!NOTE]
> **Uygulama Gizli Anahtarı** önemli bir güvenlik kimlik bilgisidir ve güvenliği uygun şekilde sağlanmalıdır.
> 

[Sonraki **adımlara geçin**](#next-steps)

### <a name="register-a-web-api"></a>Web API’si kaydetme

[!INCLUDE [active-directory-b2c-register-web-api](../../includes/active-directory-b2c-register-web-api.md)]

Gereken diğer kapsamları eklemek için **Yayımlanan kapsamlar**’a tıklayın. Varsayılan olarak, "user_impersonation" kapsamı tanımlanır. user_impersonation kapsamı, diğer uygulamaların oturum açmış kullanıcı adına bu api’de oturum açmasına olanak tanır. İsterseniz, user_impersonation kapsamı kaldırılabilir.

[Sonraki **adımlara geçin**](#next-steps)

### <a name="register-a-mobile-or-native-app"></a>Mobil veya yerel bir uygulamayı kaydetme

[!INCLUDE [active-directory-b2c-register-mobile-native-app](../../includes/active-directory-b2c-register-mobile-native-app.md)]

[Sonraki **adımlara geçin**](#next-steps)

## <a name="limitations"></a>Sınırlamalar

### <a name="choosing-a-web-app-or-api-reply-url"></a>Bir web uygulaması veya api yanıt URL'si seçme

Şu anda Azure AD B2C’ye kayıtlı uygulamalar sınırlı sayıda yanıt URL'si değeri ile kısıtlıdır. Web uygulamaları ve hizmetlerine yönelik yanıt URL’si, `https` şemasıyla başlamalı ve tüm yanıt URL’si değerleri tek bir DNS etki alanını paylaşmalıdır. Örneğin, şu yanıt URL'lerinden birine sahip bir web uygulamasını kaydedemezsiniz:

`https://login-east.contoso.com`

`https://login-west.contoso.com`

Kayıt sistemi mevcut yanıt URL'sinin tam DNS adını, eklemekte olduğunuz yanıt URL'sinin DNS adı ile karşılaştırır. Aşağıdaki koşullardan biri geçerli olduğunda DNS adı ekleme isteği başarısız olur:

* Yeni yanıt URL'sinin tam DNS adı, mevcut yanıt URL'sinin DNS adı ile eşleşmiyorsa.
* Yeni yanıt URL'sinin tam DNS adı, mevcut yanıt URL'sinin alt etki alanı değilse.

Örneğin, uygulamanın yanıt URL'si şu ise:

`https://login.contoso.com`

Aşağıdaki gibi ekleme yapabilirsiniz:

`https://login.contoso.com/new`

Bu durumda, DNS adı tam olarak eşleşir. Ya da şunu yapabilirsiniz:

`https://new.login.contoso.com`

Bu durumda, login.contoso.com DNS alt etki alanına başvurursunuz. Yanıt URL’leri login-east.contoso.com ve login-west.contoso.com olan bir uygulamanızın olmasını istiyorsanız, bu yanıt URL’lerini şu sırayla eklemeniz gerekir:

`https://contoso.com`

`https://login-east.contoso.com`

`https://login-west.contoso.com`

Sonraki iki yanıt URL’si, ilk yanıt URL'si olan contoso.com’un alt etki alanları olduğu için bunları ekleyebilirsiniz.

### <a name="choosing-a-native-app-redirect-uri"></a>Yerel uygulama yeniden yönlendirme URI'si seçme

Mobil/yerel uygulamalar için bir yeniden yönlendirme URI’si seçerken dikkat edilmesi gereken iki önemli nokta şunlardır:

* **Benzersiz**: Yeniden yönlendirme URI’si şeması her uygulama için benzersiz olmalıdır. Örnekte (com.onmicrosoft.contoso.appname://redirect/path), şema olarak com.onmicrosoft.contoso.appname kullanılır. Bu örneği izlemeniz önerilir. İki uygulama aynı şemayı paylaşıyorsa, kullanıcı bir “bir uygulama seçin” iletişim kutusu görür. Kullanıcı yanlış seçim yaparsa, oturum açma başarısız olur.
* **Tam**: Yeniden yönlendirme URI’sinin bir şeması ve yolu olmalıdır. Yol, etki alanından sonra en az bir eğik çizgi içermelidir (örneğin, //contoso/ çalışırken //contoso başarısız olur).

Yeniden yönlendirme URI'sinde alt çizgi gibi özel karakterler olmadığından emin olun.

### <a name="faulted-apps"></a>Hatalı uygulamalar

B2C uygulamaları şu durumlarda DÜZENLENMEMELİDİR:

* [Uygulama Kayıt Portalı](https://apps.dev.microsoft.com/) gibi diğer uygulama yanıt portallarında.
* Graph API'si veya PowerShell kullanılarak

Azure AD B2C uygulamasını açıklanan şekilde düzenleyip Azure portalının Azure AD B2C özellikleri menüsünde yeniden düzenlemeye çalışırsanız, uygulama hatalı bir hale gelir ve Azure AD B2C ile artık kullanılamaz. Uygulamayı silip yeniden oluşturmanız gerekir.

Uygulamayı silmek için [Uygulama Kayıt Portalı](https://apps.dev.microsoft.com/)’na gidin ve uygulamayı silin. Uygulamanın görünür olması için uygulamanın sahibi olmanız (ve yalnızca kiracının yöneticisi olmamanız) gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2C'ye kayıtlı bir uygulamaya sahip olduğunuza göre başlamak için [hızlı başlangıç öğreticilerinden](active-directory-b2c-overview.md) birini tamamlayabilirsiniz.

> [!div class="nextstepaction"]
> [Kaydolma, oturum açma ve parola sıfırlama seçenekleriyle bir ASP.NET web uygulaması oluşturma](active-directory-b2c-devquickstarts-web-dotnet-susi.md)
