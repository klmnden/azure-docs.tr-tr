---
title: Öğretici - kaydolma ve oturum açma Azure Active Directory B2C kullanarak etkinleştirmek için uygulama kaydı | Microsoft Docs
description: Bir Azure AD B2C kiracısı oluşturma ve uygulama ile kaydetmek için Azure Portalı'nı kullanın.
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: patricka
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 03/08/2018
ms.author: davidmu
ms.openlocfilehash: 81ab3288d7a365c2665b3b38ca220a3e7cb648c7
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="tutorial-register-an-application-to-enable-sign-up-and-sign-in-using-azure-active-directory-b2c"></a>Öğretici: Kaydolma ve oturum açma Azure Active Directory B2C kullanarak etkinleştirmek için bir uygulamayı kaydetme

Bu öğretici, bir Microsoft Azure Active Directory (Azure AD) B2C kiracısı oluşturma ve onunla yalnızca birkaç dakika içinde bir uygulamayı kaydetme yardımcı olur.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Azure AD B2C kiracısı oluşturma
> * Kiracı aboneliğinize bağlantı
> * Uygulamanızı kaydetme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](https://portal.azure.com/)’da oturum açın.

## <a name="create-an-azure-ad-b2c-tenant"></a>Azure AD B2C kiracısı oluşturma

B2C özellikleri, var olan kiracılar etkinleştirilemez. Azure AD B2C kiracısı oluşturmanız gerekir.

[!INCLUDE [active-directory-b2c-create-tenant](../../includes/active-directory-b2c-create-tenant.md)]

Tebrikler, Azure Active Directory B2C kiracısı oluşturdunuz. Kiracı genel Yöneticisi olduğunuz. Gerektiğinde başka Genel Yöneticiler ekleyebilirsiniz. Yeni Kiracı için geçiş yapmak için tıklatın *yeni Kiracı bağlantıyı Yönet*.

![Yeni Kiracı bağlantıyı Yönet](./media/active-directory-b2c-get-started/manage-new-b2c-tenant-link.png)

> [!IMPORTANT]
> Bir üretim uygulaması için bir B2C Kiracı kullanmayı planlıyorsanız, makaleyi okuyun [Önizleme B2C kiracılar ve üretim ölçekli](active-directory-b2c-reference-tenant-type.md). Mevcut bir B2C Kiracı silin ve aynı etki alanı adıyla yeniden oluştururken bilinen sorunlar vardır. Farklı bir etki alanı adı ile bir B2C Kiracı oluşturmanız gerekir.
>
>

## <a name="link-your-tenant-to-your-subscription"></a>Kiracı aboneliğinize bağlantı

Azure aboneliğiniz için kullanım ücretleri ödeme tüm B2C işlevselliğini etkinleştirmek için Azure AD B2C kiracınızın bağlamanız gerekir. Daha fazla bilgi edinmek için okuma [bu makalede](active-directory-b2c-how-to-enable-billing.md). Azure aboneliğinize Azure AD B2C kiracınızın bağlantı yok, bazı işlevler engellenir ve bir uyarı iletisi ("abonelik bu B2C Kiracı veya abonelik gereksinimlerinize, ilgilenilmesi gerekiyor. B2C ayarlarında bağlı") bakın. Üretime uygulamalarınızı sevk önce bu adımı uygulamanız önemlidir.


[!INCLUDE [active-directory-b2c-find-service-settings](../../includes/active-directory-b2c-find-service-settings.md)]

Girerek dikey erişebilirsiniz `Azure AD B2C` içinde **arama kaynakları** portalı üstündeki. Sonuçlar listesinde **Azure AD B2C** B2C ayarlar dikey erişmek için.

## <a name="register-your-application"></a>Uygulamanızı kaydetme

Tüketicinin kaydolmasını ve oturum açmasını kabul eden bir uygulama oluşturmak için öncelikle uygulamayı Azure Active Directory B2C kiracısına kaydetmeniz gerekir. [Azure AD B2C kiracısı oluşturma](active-directory-b2c-get-started.md) makalesinde ana hatlarıyla belirtilen adımları izleyerek kendi kiracınızı edinin.

Azure portalında oluşturulan uygulamaların aynı konumdan yönetilmesi gerekir. Azure AD B2C uygulamalarını PowerShell veya başka bir portal kullanarak düzenlerseniz bu uygulamalar desteklenmez duruma gelir ve Azure AD B2C ile çalışmaz. [Hatalı uygulamalar](#faulted-apps) bölümünden ayrıntılara bakabilirsiniz. 

Bu makalede örneklerimizle çalışmaya başlamanıza yardımcı olacak örnekler kullanır. Bu örnekler hakkında sonraki makalelerinde daha fazla bilgi edinebilirsiniz.

### <a name="navigate-to-b2c-settings"></a>B2C ayarlarına gidin

[Azure portalında](https://portal.azure.com/) B2C kiracısının Genel Yöneticisi olarak oturum açın. 

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](../../includes/active-directory-b2c-portal-navigate-b2c-service.md)]

### <a name="choose-next-steps-based-on-your-application-type"></a>Uygulama türüne göre sonraki adımları seçin

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

Bu makalede, öğrenilen nasıl yapılır:

> [!div class="checklist"]
> * Azure AD B2C kiracısı oluşturma
> * Kiracı aboneliğinize bağlantı
> * Uygulamanızı kaydetme

> [!div class="nextstepaction"]
> [Bir web uygulaması ile hesaplarının kimliğini doğrulamak amacıyla etkinleştir](active-directory-b2c-tutorials-web-app.md)