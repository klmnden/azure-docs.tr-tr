---
title: 'Azure Active Directory B2C: Twitter yapılandırma | Microsoft Docs'
description: Uygulamalarınızda Azure Active Directory B2C tarafından güvenliği sağlanan Twitter hesaplarıyla tüketiciye kaydolma ve oturum açma sağlar.
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 4/06/2017
ms.author: davidmu
ms.openlocfilehash: ee2d82f8c90b88a898428973a1febaa21034a14f
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-twitter-accounts"></a>Azure Active Directory B2C: Kaydolma ve oturum açma Twitter hesaplarıyla tüketicileri sağlayın

## <a name="create-a-twitter-application"></a>Bir Twitter uygulaması oluşturma
Twitter Azure Active Directory (Azure AD) B2C bir kimlik sağlayıcısı olarak kullanmak için bir Twitter uygulaması oluşturmak ve doğru parametrelerle sağlamanız gerekir. Bunu yapmak için bir Twitter Geliştirici hesabınızın olması gerekir. Bir sahip değilseniz, yerinde edinebilirsiniz [ https://dev.twitter.com/ ](https://dev.twitter.com/).

1. Git [Geliştirici Web sitesi Twitter](https://dev.twitter.com/) ve kimlik bilgilerinizle oturum açın.
2. Tıklatın **uygulamalarım** altında **araçları ve Destek** ve ardından **yeni uygulama oluşturma**. 
3. Formda için bir değer girin **adı**, **açıklama**, ve **Web sitesi**.
4. İçin **geri çağırma URL'si**, girin `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`. Değiştirdiğinizden emin olun **{tenant}** , kiracının adlı (örneğin, contosob2c.onmicrosoft.com).
5. Kabul etmek için kutuyu **Geliştirici anlaşma** tıklatıp **Twitter uygulamanızı oluşturma**.
6. Uygulama oluşturulduktan sonra tıklayın **anahtarları ve erişim belirteçleri**.
7. Değerini kopyalayın **tüketici anahtarı** ve **tüketici gizli**. Her ikisi de Twitter kimlik sağlayıcısı kiracınızda yapılandırmak için gerekir.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Twitter kimlik sağlayıcısı kiracınızda yapılandırın
1. Aşağıdaki adımları izleyin [B2C özellikleri dikey penceresine gidin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) Azure portalındaki.
2. B2C özellikleri dikey penceresinde **kimlik sağlayıcıları**.
3. Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.
4. Kolay bir sağlamak **adı** kimlik sağlayıcısı yapılandırması için. Örneğin, "Twitter" girin.
5. Tıklatın **kimlik sağlayıcısı türü**seçin **Twitter**, tıklatıp **Tamam**.
6. Tıklatın **bu kimlik sağlayıcısı'nı ayarlama** ve Twitter girin **tüketici anahtarı** için **istemci kimliği** ve Twitter **tüketici gizli** için **gizli**.
7. Tıklatın **Tamam**ve ardından **oluşturma** Twitter yapılandırmanızı kaydetmek için.

## <a name="next-steps"></a>Sonraki adımlar

Oluşturma veya düzenleme bir [yerleşik ilke](active-directory-b2c-reference-policies.md) ve Twitter kimlik sağlayıcısı olarak ekleyin.