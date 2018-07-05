---
title: Azure Active Directory B2C'de GitHub kimlik sağlayıcı yapılandırması | Microsoft Docs
description: Uygulamalarınızda Azure Active Directory B2C ile güvenliği sağlanan GitHub hesabı olan müşteriler için kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/06/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 3754a169b301bac97f3e12d10b754222e3cf325d
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443350"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-github-accounts"></a>Azure Active Directory B2C: Kaydolma ve oturum açma için GitHub hesaplarında tüketicileriyle sağlayın

> [!NOTE]
> Bu özellik önizlemede.
> 

Bu makalede GitHub hesabı olan kullanıcılar için oturum açma olanağı tanıma gösterilmektedir.

## <a name="create-a-github-oauth-application"></a>GitHub OAuth uygulaması oluşturma

GitHub kimlik sağlayıcısı olarak Azure AD B2C'yi kullanmak için bir GitHub OAuth uygulaması oluşturma ve doğru parametreleri sağlamanız gerekir.

1. Git [GitHub Geliştirici ayarları](https://github.com/settings/developers) Github'da oturum oturum açtıktan sonra.
1. Tıklayın **yeni bir OAuth uygulaması**
1. Girin bir **uygulama adı** ve **giriş sayfası URL'si**.
1. İçin **yetkilendirme geri çağırma URL'si**, girin `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`. Değiştirin **{tenant}** ile Azure AD B2C kiracınızın adı (örneğin, contosob2c.onmicrosoft.com).

    >[!NOTE]
    >"Tenant" değeri, harflerden oluşmalıdır **oturum açma URL'si**.

1. Tıklayın **kaydetme uygulama**.
1. Değerlerini kaydetmek **istemci kimliği** ve **gizli**. Sonraki bölümde hem de gerekir.

## <a name="configure-github-as-an-identity-provider-in-your-azure-ad-b2c-tenant"></a>Azure AD B2C kiracınızdaki bir kimlik sağlayıcısı olarak GitHub'ı yapılandırma

1. Bu adımları [B2C özellikleri dikey penceresine gitme](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) Azure portalında.
1. B2C özellikleri dikey penceresinde tıklayın **kimlik sağlayıcıları**.
1. Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.
1. Kullanımı kolay bir sağlamak **adı** kimlik sağlayıcısı yapılandırması için. Örneğin, "GitHub" girin.
1. Tıklayın **kimlik sağlayıcısı türü**seçin **GitHub**, tıklatıp **Tamam**.
1. Tıklayın **bu kimlik sağlayıcısını ayarlama** istemci kimliği ve daha önce kopyaladığınız GitHub OAuth uygulamasının istemci gizli anahtarını girin.
1. Tıklayın **Tamam** ve ardından **Oluştur** GitHub yapılandırmanızı kaydetmek için.

## <a name="next-steps"></a>Sonraki adımlar

Oluşturma veya düzenleme bir [yerleşik ilke](active-directory-b2c-reference-policies.md) ve GitHub kimlik sağlayıcısı olarak ekleyin.