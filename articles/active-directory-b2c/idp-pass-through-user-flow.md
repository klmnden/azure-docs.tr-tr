---
title: Uygulamanızı Azure Active Directory B2C için bir kullanıcı akışı aracılığıyla erişim belirteci geçirin | Microsoft Docs
description: Bir erişim belirteci OAuth2.0 kimlik sağlayıcıları için Azure Active Directory B2C kullanıcı akışında bir talebi olarak nasıl geçirebilirsiniz öğrenin.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/28/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: b26605bd0b436d948fb1f62cbf32a17ea4f386d0
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53602265"
---
# <a name="pass-an-access-token-through-a-user-flow-to-your-application-in-azure-active-directory-b2c"></a>Uygulamanızı Azure Active Directory B2C için bir kullanıcı akışı aracılığıyla erişim belirteci geçirin

> [!NOTE]
> Bu özellik şu anda genel Önizleme aşamasındadır.

> [!Important]
> Bu genel önizleme özelliği geçici olarak kullanılamıyor.

A [kullanıcı akışı](active-directory-b2c-reference-policies.md) Azure Active Directory (Azure AD) B2C'de, uygulamanızın kullanıcıları ıntune'a kaydolma veya bir kimlik sağlayıcısı ile oturum açmak için bir fırsat sağlar. Yolculuğunuzun başladığında, Azure AD B2C alır bir [erişim belirteci](active-directory-b2c-reference-tokens.md) kimlik sağlayıcısından gelen. Azure AD B2C kullanıcı hakkında bilgi almak için bu belirteci kullanır. Bir talep belirteci aracılığıyla Azure AD B2C'de kayıt uygulamaları geçirmek için kullanıcı akışınızı etkinleştirin.

Azure AD B2C şu anda yalnızca destekler erişim belirtecini geçirme [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) içeren kimlik sağlayıcıları [Facebook](active-directory-b2c-setup-fb-app.md) ve [Google](active-directory-b2c-setup-goog-app.md). Diğer tüm kimlik sağlayıcıları için talep boş olarak döndürülür.

## <a name="prerequisites"></a>Önkoşullar

- Uygulamanızı kullanarak bir [v2 kullanıcı akışı](user-flow-versions.md).
- Kullanıcı akışı, OAuth 2.0 kimlik sağlayıcısı ile yapılandırılır.

## <a name="enable-the-claim"></a>Talep etkinleştir

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. Seçin **kullanıcı akışları**ve ardından, kullanıcı akışı seçin. Örneğin, **B2C_1_SignupSignIn**.
5. **Uygulama talepleri**’ni seçin.
6. Etkinleştirme **kimlik sağlayıcısı erişim belirteci**.

    ![Uygulama talep](./media/idp-pass-through-user-flow/idp-pass-through-user-flow-app-claim.png)

7. Tıklayın **Kaydet** kullanıcı akışı kaydedin.

## <a name="test-the-user-flow"></a>Kullanıcı akışı test edin

Uygulamalarınızı Azure AD B2C'de test etme, döndürülen Azure AD B2C belirteci sahip olmak yararlı olabilir `https://jwt.ms` Taleplerde gözden geçirmek için.

1. Kullanıcı akışı genel bakış sayfasında **kullanıcı akışı çalıştırma**.
2. İçin **uygulama**, daha önce kaydettiğiniz uygulamanızı seçin. Aşağıdaki örnekte, belirteci görmek için **yanıt URL'si** göstermelidir `https://jwt.ms`.
3. Tıklayın **kullanıcı akışı çalıştırma**ve ardından hesabı kimlik bilgilerinizle oturum açın. Erişim belirteci kimlik sağlayıcısının görmelisiniz **idp_access_token** talep.

    Aşağıdaki örneğe benzer bir şey görmeniz gerekir:

    ![Kodu çözülen belirteci](./media/idp-pass-through-user-flow/idp-pass-through-user-flow-token.png)

## <a name="next-steps"></a>Sonraki adımlar

Belirteçleri hakkında daha fazla bilgi [Azure Active Directory belirteci başvurusu](active-directory-b2c-reference-tokens.md).




