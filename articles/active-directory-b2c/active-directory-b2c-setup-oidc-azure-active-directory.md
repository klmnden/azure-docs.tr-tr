---
title: Yerleşik ilkeleri Azure Active Directory B2C kullanarak bir Azure AD sağlayıcısını ekleyin | Microsoft Docs
description: Bir Open ID Connect kimlik sağlayıcısı (Azure AD) eklemeyi öğrenin.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/27/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 70d3a19b715052fe658102929a1c29cf3db2d595
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443741"
---
# <a name="azure-active-directory-b2c-sign-in-using-azure-ad-accounts-through-a-built-in-policy"></a>Azure Active Directory B2C: Yerleşik bir ilke aracılığıyla Azure AD hesapları kullanarak oturum açın.

>[!NOTE]
> Bu özellik genel Önizleme aşamasındadır. Bu özellik, üretim ortamında kullanmayın.

Bu makalede, oturum açmak için kullanıcıların belirli bir Azure Active Directory (Azure AD) kuruluş yerleşik ilkeleri etkinleştirme işlemini göstermektedir.

## <a name="create-an-azure-ad-app"></a>Bir Azure AD uygulamanızı oluşturma

Belirli kullanıcılar için oturum açma etkinleştirmek için Azure AD kuruluş ihtiyacınız kuruluş içinde bir uygulamayı kaydetmek Azure AD kiracısı.

>[!NOTE]
> "Contoso.com" kuruluş için kullandığımız Azure AD kiracısı ve aşağıdaki yönergeler, Azure AD B2C kiracısı olarak "fabrikamb2c.onmicrosoft.com".

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Üst çubuğunda, hesabınızı seçin. Gelen **dizin** liste öğesini kuruluş burada kayıt (contoso.com) uygulamanızı Azure AD kiracısı.
1. Seçin **tüm hizmetleri** sol bölme ve "Uygulama kayıtları" için arama
1. **Yeni uygulama kaydı**’nı seçin.
1. Uygulamanız için bir ad girin (örneğin, `Azure AD B2C App`).
1. Uygulama türü için **Web uygulaması / API** öğesini seçin.
1. İçin **oturum açma URL'si**, aşağıdaki URL'yi girin. burada `yourtenant` Azure AD B2C kiracınızın adı tarafından değiştirilir (`fabrikamb2c.onmicrosoft.com`):

    >[!NOTE]
    >"Yourtenant" değeri, harflerden oluşmalıdır **oturum açma URL'si**.

    ```Console
    https://login.microsoftonline.com/te/yourtenant.onmicrosoft.com/oauth2/authresp
    ```

1. Uygulama kimliği, sonraki bölümde istemci kimliği olarak kullanacağınız Kaydet
1. Altında **ayarları** dikey penceresinde **anahtarları**.
1. Girin bir **anahtar açıklaması** altında **parolaları** bölümünde ve ayarlama **süresi** "Her zaman geçerli olsun" için. 
1. Tıklayın **Kaydet**not alın, elde edilen anahtar **değer**, sonraki bölümde gizli kullanacağı.

## <a name="configure-azure-ad-as-an-identity-provider-in-your-tenant"></a>Kiracınızdaki bir kimlik sağlayıcısı olarak Azure AD'yi yapılandırma

1. Üst çubuğunda, hesabınızı seçin. Gelen **dizin** listesinde, Azure AD B2C kiracısı (fabrikamb2c.onmicrosoft.com) seçin.
1. [Azure AD B2C ayarları menüsüne gidin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) Azure portalında.
1. Azure AD B2C ayarları menüsünde tıklayarak **kimlik sağlayıcıları**.
1. Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.
1. Kullanımı kolay bir sağlamak **adı** kimlik sağlayıcısı yapılandırması için. Örneğin, "Contoso Azure AD" girin.
1. Tıklayın **kimlik sağlayıcısı türü**seçin **Open ID Connect**, tıklatıp **Tamam**.
1. Tıklayın **bu kimlik sağlayıcısını ayarlama**
1. İçin **meta veri URL'si**, aşağıdaki URL'yi girin. burada `yourtenant` Azure AD kiracınıza adıyla değiştirildiği (örneğin `contoso.com`):

    ```Console
    https://login.microsoftonline.com/yourtenant/.well-known/openid-configuration
    ```
1. İçin **istemci kimliği** ve **gizli**, önceki bölümde uygulama kimliği ve anahtarı girin.
1. İçin varsayılan değer tutmak **kapsam**, ayarlanması `openid`.
1. İçin varsayılan değer tutmak **yanıt türü**, ayarlanması `code`.
1. İçin varsayılan değer tutmak **yanıt modu**, ayarlanması `form_post`.
1. İsteğe bağlı olarak, bir değer girin **etki alanı** (örneğin `ContosoAD`). Bu değeri kullanarak bu kimlik sağlayıcısını söz konusu olduğunda kullanılacak olan *domain_hint* istek. 
1. **Tamam**’a tıklayın.
1. Tıklayarak **bu kimlik sağlayıcısının taleplerini Eşle**.
1. İçin **kullanıcı kimliği**, girin `oid`.
1. İçin **görünen ad**, girin `name`.
1. İçin **verilen ad**, girin `given_name`.
1. İçin **Soyadı**, girin `family_name`.
1. İçin **e-posta**, girin `unique_name`
1. Tıklayın **Tamam**, ardından **Oluştur** yapılandırmanızı kaydetmek için.

## <a name="next-steps"></a>Sonraki adımlar

Yeni oluşturulan eklemek için yerleşik bir ilke Azure AD kimlik sağlayıcısı ve geri bildirim sağlamak [ AADB2CPreview@microsoft.com ](mailto:AADB2CPreview@microsoft.com).
