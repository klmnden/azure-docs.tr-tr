---
title: Azure Active Directory B2C'de bir uygulamayı kaydetme | Microsoft Docs
description: Uygulamanızı Azure Active Directory B2C ile kaydetme hakkında bilgi edinin.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/05/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: fcebada4ef10c3e0bb298e9308d66ecb37247832
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52999268"
---
# <a name="register-an-application-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de bir uygulamayı kaydetme

Oluşturulacak bir [uygulama](active-directory-b2c-apps.md) tüketicinin kaydolmasını ve oturum açma kabul eden, öncelikle uygulamayı Azure AD B2C kiracısı ile kaydetmeniz gerekir. Bu makale birkaç dakika içinde bir Azure Active Directory (Azure AD) B2C kiracısında bir uygulamayı kaydetme yardımcı olur. İşiniz bittiğinde, uygulamanız Azure AD B2C Kiracısında kullanım için kaydedilir.

## <a name="prerequisites"></a>Önkoşullar

Adımları izleyerek kendi kiracınızı edinin [bir Azure Active Directory B2C kiracısı oluşturmayı](tutorial-create-tenant.md).

Uygulama türüne göre sonraki adımları seçin:

- [Web uygulaması kaydetme](#register-a-web-application)
- [Web API’si kaydetme](#register-a-web-api)
- [Mobil veya yerel bir uygulamayı kaydetme](#register-a-mobile-or-native-application)

## <a name="register-a-web-application"></a>Web uygulaması kaydetme

1. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
2. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
3. Seçin **uygulamaları**ve ardından **Ekle**.
4. Uygulama için bir ad girin. Örneğin *testapp1*.
5. İçin **içeren web uygulaması / web API'sini** ve **örtük akışa izin ver**seçin **Evet**.
6. İçin **yanıt URL'si**, Azure AD B2C, uygulamanız tarafından istenen belirteçleri döndürdüğü uç noktasını girin. Örneğin, yerel olarak dinlemesine ayarlayabilirsiniz `https://localhost:44316`. Bağlantı noktası numarasını henüz bilmiyorsanız, bir yer tutucu değerini girin ve daha sonra değiştirin.
7. **Oluştur**’a tıklayın.

### <a name="create-a-client-secret"></a>İstemci gizli dizi oluşturma

Uygulamanız bir web API'si Azure AD B2C ile güvenliği sağlanan çağırıyorsa, bir uygulama gizli dizisi oluşturmanız gerekir.

1. Seçin **anahtarları** ve ardından **anahtar üret**. 
2. Seçin **Kaydet** anahtarı görüntülemek için. **Uygulama anahtarı** değerini not edin. Bu değeri, uygulamanızın kodunda uygulama gizli dizisi olarak kullanırsınız.
3. Seçin **API erişimi**, tıklayın **Ekle**, web API'si ve kapsamlarınızı (izinler) seçin.

## <a name="register-a-web-api"></a>Web API’si kaydetme

1. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
2. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
3. Seçin **uygulamaları**ve ardından **Ekle**.
4. Uygulama için bir ad girin. Örneğin *testapp2*.
5. İçin **içeren web uygulaması / web API'sini** ve **örtük akışa izin ver**seçin **Evet**.
6. İçin **yanıt URL'si**, Azure AD B2C, uygulamanız tarafından istenen belirteçleri döndürdüğü uç noktasını girin. Örneğin, yerel olarak dinlemesine ayarlayabilirsiniz `https://localhost:44316`. Bağlantı noktası numarasını henüz bilmiyorsanız, bir yer tutucu değerini girin ve daha sonra değiştirin.
7. İçin **uygulama kimliği URI'si**, web API'niz için kullanılan tanımlayıcı girin. Tam etki alanı ile birlikte URI tanımlayıcısı sizin için oluşturulur. Örneğin, `https://contosotenant.onmicrosoft.com/api`.
8. **Oluştur**’a tıklayın.
9. Seçin **yayımlanan kapsamlar** gereken diğer kapsamları eklemek için. Varsayılan olarak, `user_impersonation` kapsamı tanımlanır. `user_impersonation` Kapsamı, diğer uygulamaların oturum açmış kullanıcı adına bu api'de imkanı sunar. İsterseniz, `user_impersonation` kapsam kaldırılabilir.

## <a name="register-a-mobile-or-native-application"></a>Mobil veya yerel bir uygulamayı kaydetme

1. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
2. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
3. Seçin **uygulamaları**ve ardından **Ekle**.
4. Uygulama için bir ad girin. Örneğin *testapp3*.
5. İçin **içeren web uygulaması / web API'sini**seçin **Hayır**.
6. İçin **yerel istemciyi dahil et**seçin **Evet**.
7. İçin **yeniden yönlendirme URI'si**, girin bir [özel bir düzen ile yeniden yönlendirme URI'si](active-directory-b2c-apps.md). İyi bir yeniden yönlendirme URI'si seçin ve alt çizgi gibi özel karakterler içermiyor emin olun.
8. **Oluştur**’a tıklayın.

### <a name="create-a-client-secret"></a>İstemci gizli dizi oluşturma

Uygulamanız bir web API'si Azure AD B2C ile güvenliği sağlanan çağırıyorsa, bir uygulama gizli dizisi oluşturmanız gerekir.

1. Seçin **anahtarları** ve ardından **anahtar üret**. 
2. Seçin **Kaydet** anahtarı görüntülemek için. **Uygulama anahtarı** değerini not edin. Bu değeri, uygulamanızın kodunda uygulama gizli dizisi olarak kullanırsınız.
3. Seçin **API erişimi**, tıklayın **Ekle**, web API'si ve kapsamlarınızı (izinler) seçin.

## <a name="next-steps"></a>Sonraki adımlar

Erişim belirteçleri uygulamalar tarafından nasıl kullanıldığı hakkında daha fazla izin verme API'leri öğrenin [isteyen erişim belirteçleri](active-directory-b2c-access-tokens.md)
