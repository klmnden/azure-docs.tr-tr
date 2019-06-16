---
title: Kullanıcı akışı aracılığıyla erişim belirteci, uygulamanıza - Azure Active Directory B2C geçirin | Microsoft Docs
description: Bir erişim belirteci OAuth2.0 kimlik sağlayıcıları için Azure Active Directory B2C kullanıcı akışında bir talebi olarak nasıl geçirebilirsiniz öğrenin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: db4b799aa31a4132609b0dd158b65070fb2474b7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66510959"
---
# <a name="pass-an-access-token-through-a-user-flow-to-your-application-in-azure-active-directory-b2c"></a>Uygulamanızı Azure Active Directory B2C için bir kullanıcı akışı aracılığıyla erişim belirteci geçirin

> [!NOTE]
> Bu özellik şu anda genel Önizleme aşamasındadır.

A [kullanıcı akışı](active-directory-b2c-reference-policies.md) Azure Active Directory (Azure AD) B2C'de, uygulamanızın kullanıcıları ıntune'a kaydolma veya bir kimlik sağlayıcısı ile oturum açmak için bir fırsat sağlar. Yolculuğunuzun başladığında, Azure AD B2C alır bir [erişim belirteci](active-directory-b2c-reference-tokens.md) kimlik sağlayıcısından gelen. Azure AD B2C kullanıcı hakkında bilgi almak için bu belirteci kullanır. Bir talep belirteci aracılığıyla Azure AD B2C'de kayıt uygulamaları geçirmek için kullanıcı akışınızı etkinleştirin.

Azure AD B2C şu anda yalnızca destekler erişim belirtecini geçirme [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) içeren kimlik sağlayıcıları [Facebook](active-directory-b2c-setup-fb-app.md) ve [Google](active-directory-b2c-setup-goog-app.md). Diğer tüm kimlik sağlayıcıları için talep boş olarak döndürülür.

## <a name="prerequisites"></a>Önkoşullar

- Uygulamanızı kullanarak bir [v2 kullanıcı akışı](user-flow-versions.md).
- Kullanıcı akışı, OAuth 2.0 kimlik sağlayıcısı ile yapılandırılır.

## <a name="enable-the-claim"></a>Talep etkinleştir

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
2. Azure AD B2C kiracınızı içeren dizine kullandığınızdan emin olun. Seçin **dizin ve abonelik filtresi** üst menüdeki ve kiracınız içeren dizini seçin.
3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. Seçin **kullanıcı akışları (ilke)** ve ardından, kullanıcı akışı seçin. Örneğin, **B2C_1_signupsignin1**.
5. **Uygulama talepleri**’ni seçin.
6. Etkinleştirme **kimlik sağlayıcısı erişim belirteci** talep.

    ![Kimlik sağlayıcısı erişim belirteci talep etkinleştir](./media/idp-pass-through-user-flow/idp-pass-through-user-flow-app-claim.png)

7. Tıklayın **Kaydet** kullanıcı akışı kaydedin.

## <a name="test-the-user-flow"></a>Kullanıcı akışı test edin

Uygulamalarınızı Azure AD B2C'de test etme, döndürülen Azure AD B2C belirteci sahip olmak yararlı olabilir `https://jwt.ms` Taleplerde gözden geçirmek için.

1. Kullanıcı akışı genel bakış sayfasında **kullanıcı akışı çalıştırma**.
2. İçin **uygulama**, daha önce kaydettiğiniz uygulamanızı seçin. Aşağıdaki örnekte, belirteci görmek için **yanıt URL'si** göstermelidir `https://jwt.ms`.
3. Tıklayın **kullanıcı akışı çalıştırma**ve ardından hesabı kimlik bilgilerinizle oturum açın. Erişim belirteci kimlik sağlayıcısının görmelisiniz **idp_access_token** talep.

    Aşağıdaki örneğe benzer bir şey görmeniz gerekir:

    ![Kodu çözülen belirteci](./media/idp-pass-through-user-flow/idp-pass-through-user-flow-token.png)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi [Azure AD B2C belirteçleri bakış](active-directory-b2c-reference-tokens.md).




