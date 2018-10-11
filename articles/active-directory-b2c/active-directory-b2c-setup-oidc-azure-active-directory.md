---
title: Oturum açma Azure Active Directory hesaplarını Azure Active Directory B2C'de yerleşik bir ilke ayarlama | Microsoft Docs
description: Oturum açma Azure Active Directory hesaplarını Azure Active Directory B2C'de yerleşik bir ilke ayarlama.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 5f51fbff11412324ad167d49202f7215cefb5ac2
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49076927"
---
# <a name="set-up-sign-in-azure-active-directory-accounts-a-built-in-policy-in-azure-active-directory-b2c"></a>Oturum açma Azure Active Directory hesaplarını Azure Active Directory B2C'de yerleşik bir ilke ayarlama

>[!NOTE]
> Bu özellik genel Önizleme aşamasındadır. Bu özellik, üretim ortamında kullanmayın.

Bu makalede Azure Active Directory (Azure AD) B2C'de yerleşik bir ilke kullanarak belirli bir Azure Active Directory (Azure AD) kuruluşun kullanıcıları için oturum açma olanağı tanıma gösterilmektedir.

## <a name="create-an-azure-ad-app"></a>Bir Azure AD uygulamanızı oluşturma

Belirli kullanıcılar için oturum açma etkinleştirmek için Azure AD kuruluş ihtiyacınız kuruluş içinde bir uygulamayı kaydetmek Azure AD kiracısı.

>[!NOTE]
>`Contoso.com` Kuruluş için kullanılan Azure AD kiracısı ve `fabrikamb2c.onmicrosoft.com` aşağıdaki yönergeler, Azure AD B2C kiracısı olarak kullanılır.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üst menüde dizin ve abonelik filtresi tıklattıktan sonra Azure AD kiracınıza içeren dizine seçerek Azure AD kiracınız (contoso.com) içeren dizine kullandığınızdan emin olun.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **uygulama kayıtları**.
4. **Yeni uygulama kaydı**’nı seçin.
5. Uygulamanız için bir ad girin. Örneğin, `Azure AD B2C App`.
6. İçin **uygulama türü**seçin `Web app / API`.
7. İçin **oturum açma URL'si**, tüm küçük harfleri, aşağıdaki URL'yi girin burada `your-tenant` (fabrikamb2c.onmicrosoft.com) Azure AD B2C kiracınızın adı ile değiştirilir:

    ```
    https://your-tenant.b2clogin.com/your-tenant.onmicrosoft.com/oauth2/authresp
    ```

    Tüm URL'leri artık kullanması gereken [b2clogin.com](b2clogin.md).

8. **Oluştur**’a tıklayın. Kopyalama **uygulama kimliği** daha sonra kullanılacak.
9. Uygulamayı seçin ve ardından **ayarları**.
10. Seçin **anahtarları**anahtar için bir açıklama girin, bir süre seçin ve ardından **Kaydet**. Daha sonra kullanılmak üzere görüntülenen anahtarının değerini kopyalayın.

## <a name="configure-azure-ad-as-an-identity-provider-in-your-tenant"></a>Kiracınızdaki bir kimlik sağlayıcısı olarak Azure AD'yi yapılandırma

1. Azure AD B2C kiracısı (fabrikamb2c.onmicrosoft.com) tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve Azure AD B2C'yi içeren dizine seçme Kiracı.
2. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
3. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
4. Girin bir **adı**. Örneğin, "Contoso Azure AD" girin.
5. Seçin **kimlik sağlayıcısı türü**seçin **Open ID Connect (Önizleme)** ve ardından **Tamam**.
6. Tıklayın **bu kimlik sağlayıcısını ayarlama**
7. İçin **meta veri URL'si**, değiştirerek aşağıdaki URL'yi girin `your-tenant` ile Azure AD Kiracı adı:

    ```
    https://login.microsoftonline.com/your-tenant/.well-known/openid-configuration
    ```
8. İçin **istemci kimliği**, daha önce kaydettiğiniz uygulama Kimliğini girin ve **gizli**, daha önce kaydettiğiniz anahtarı değerini girin.
9. İsteğe bağlı olarak, bir değer girin **Domain_hint** (örneğin `ContosoAD`). Bu değeri kullanarak bu kimlik sağlayıcısını söz konusu olduğunda kullanılacak olan *domain_hint* istek. 
10. **Tamam** düğmesine tıklayın.
11. Seçin **bu kimlik sağlayıcısının taleplerini Eşle** ve aşağıdaki talep ayarlayın:
    
    - İçin **kullanıcı kimliği**, girin `oid`.
    - İçin **görünen ad**, girin `name`.
    - İçin **verilen ad**, girin `given_name`.
    - İçin **Soyadı**, girin `family_name`.
    - İçin **e-posta**, girin `unique_name`.

12. Tıklayın **Tamam**ve ardından **Oluştur** yapılandırmanızı kaydetmek için.
