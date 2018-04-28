---
title: Azure AD B2C için twitter yapılandırma | Microsoft Docs
description: Uygulamalarınızda Azure Active Directory B2C tarafından güvenliği sağlanan Twitter hesaplarıyla müşterilere kaydolma ve oturum açma sağlar.
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 4/17/2018
ms.author: davidmu
ms.openlocfilehash: 40e4c5549414765dabc6f37c5ffb5aea519ae673
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="provide-sign-up-and-sign-in-to-consumers-with-twitter-accounts-using-azure-ad-b2c"></a>Tüketicilere Twitter hesapları ile Azure AD B2C kullanarak, kaydolma ve oturum açma sağlar.

## <a name="create-a-twitter-application"></a>Twitter uygulaması oluşturma
Twitter Azure Active Directory (Azure AD) B2C bir kimlik sağlayıcısı olarak kullanmak için bir Twitter uygulaması oluşturmak ve doğru parametrelerle sağlamanız gerekir. Bunu yapmak için bir Twitter hesabı gerekir. Bir sahip değilseniz, yerinde edinebilirsiniz [ https://twitter.com/signup ](https://twitter.com/signup).

1. Git [Twitter uygulamalar](https://apps.twitter.com/) ve kimlik bilgilerinizle oturum açın.
2. Tıklatın **yeni uygulama oluştur**. 
3. Formda için bir değer girin **adı**, **açıklama**, ve **Web sitesi**.
4. İçin **geri çağırma URL'si**, girin `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`. Değiştirdiğinizden emin olun **{tenant}** , kiracının adlı (örneğin, contosob2c.onmicrosoft.com).
5. Kabul etmek için kutuyu **Geliştirici anlaşma** tıklatıp **Twitter uygulamanızı oluşturma**.
6. Uygulama oluşturulduktan sonra tıklayın **anahtarları ve erişim belirteçleri**.
7. Değerini kopyalayın **tüketici anahtarı** ve **tüketici gizli**. Her ikisi de Twitter kimlik sağlayıcısı kiracınızda yapılandırmak için gerekir.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Twitter kimlik sağlayıcısı kiracınızda yapılandırın
1. Oturum [Azure portal](https://portal.azure.com/) Azure AD B2C kiracısının genel Yöneticisi olarak. 
2. Azure AD B2C kiracınızın geçiş yapmak için portalın sağ üst köşesinde Azure AD B2C dizini seçin.
3. Tıklatın **tüm hizmetleri**ve ardından **Azure AD B2C** altında Hizmetleri listesinden **güvenlik + kimlik**.
4. Tıklatın **kimlik sağlayıcıları**.
4. Kolay bir sağlamak **adı** kimlik sağlayıcısı yapılandırması için. Örneğin, "Twitter" girin.
5. Tıklatın **kimlik sağlayıcısı türü**seçin **Twitter (Önizleme)**, tıklatıp **Tamam**.
6. Tıklatın **bu kimlik sağlayıcısı'nı ayarlama** ve Twitter girin **tüketici anahtarı** için **istemci kimliği** ve Twitter **tüketici gizli** için **gizli**.
7. Tıklatın **Tamam**ve ardından **oluşturma** Twitter yapılandırmanızı kaydetmek için.

## <a name="next-steps"></a>Sonraki adımlar

Oluşturma veya düzenleme bir [yerleşik ilke](active-directory-b2c-reference-policies.md) ve Twitter kimlik sağlayıcısı olarak ekleyin.