---
title: Oturum açmak için bir Azure Active Directory kuruluş - Azure Active Directory B2C ayarlama | Microsoft Docs
description: Oturum açma için Azure Active Directory B2C, belirli bir Azure Active Directory kuruluşunu ayarlayın.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: bae5759beb6a817c411ee52d7eb27dbff4cfe01c
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65785242"
---
# <a name="set-up-sign-in-for-a-specific-azure-active-directory-organization-in-azure-active-directory-b2c"></a>Azure Active Directory B2C, belirli bir Azure Active Directory kuruluş için oturum açma ayarlama

>[!NOTE]
> Bu özellik genel önizleme aşamasındadır. Bu özellik, üretim ortamında kullanmayın.

Bir Azure Active Directory (Azure AD) olarak kullanılacak bir [kimlik sağlayıcısı](active-directory-b2c-reference-oauth-code.md) Azure AD B2C'de, temsil ettiği bir uygulama oluşturmanız gerekir. Belirli bir Azure AD kullanıcıları için oturum açma olanağı tanıma bu makale Azure AD B2C'de bir kullanıcı kullanarak kuruluş akış.

## <a name="create-an-azure-ad-app"></a>Bir Azure AD uygulamanızı oluşturma

Belirli kullanıcılar için oturum açma etkinleştirmek için Azure AD kuruluş ihtiyacınız kuruluş içinde bir uygulamayı kaydetme Azure AD kiracısı, Azure AD B2C kiracınızı ile aynı değil.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD kiracınıza içeren dizine kullandığınızdan emin olun. Seçin **dizin ve abonelik filtresi** üst menüdeki ve Azure AD kiracınıza içeren dizini seçin. Azure AD B2C kiracınızı olarak aynı kiracıda değil.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **uygulama kayıtları**.
4. Seçin **yeni kayıt**.
5. Uygulamanız için bir ad girin. Örneğin, `Azure AD B2C App`.
6. Seçimi kabul **hesapları yalnızca kuruluş bu dizinde** bu uygulama için.
7. İçin **yeniden yönlendirme URI'si**, değerini kabul **Web**, tüm küçük harfleri, aşağıdaki URL'yi girin burada `your-B2C-tenant-name` Azure AD B2C kiracınızın adı ile değiştirilir. Örneğin, `https://fabrikam.b2clogin.com/fabrikam.onmicrosoft.com/oauth2/authresp`:

    ```
    https://your--B2C-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```

    Tüm URL'leri artık kullanması gereken [b2clogin.com](b2clogin.md).

8. Tıklayın **kaydetme**. Kopyalama **uygulama (istemci) kimliği** daha sonra kullanılacak.
9. Seçin **sertifikaları ve parolaları** uygulama menüsünü ve ardından **yeni gizli**.
10. İstemci gizli anahtarı için bir ad girin. Örneğin, `Azure AD B2C App Secret`.
11. Süre sonu dönemi seçin. Bu uygulama için seçimi kabul **1 yıl**.
12. Seçin **Ekle** ve daha sonra kullanılmak üzere görüntülenen yeni gizli anahtar değerini kopyalayın.

## <a name="configure-azure-ad-as-an-identity-provider"></a>Kimlik sağlayıcısı olarak Azure AD'yi yapılandırma

1. Azure AD B2C kiracısı içeren dizine kullandığınızdan emin olun. Seçin **dizin ve abonelik filtresi** üst menüdeki ve Azure AD B2C kiracınızı içeren dizini seçin.
2. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
3. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
4. Girin bir **adı**. Örneğin, `Contoso Azure AD` girin.
5. Seçin **kimlik sağlayıcısı türü**seçin **Open ID Connect (Önizleme)** ve ardından **Tamam**.
6. Seçin **bu kimlik sağlayıcısını ayarlama**
7. İçin **meta veri URL'si**, değiştirerek aşağıdaki URL'yi girin `your-AD-tenant-domain` Azure AD kiracınızın etki alanı adına sahip. Örneğin `https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration`:

    ```
    https://login.microsoftonline.com/your-AD-tenant-domain/.well-known/openid-configuration
    ```

8. İçin **istemci kimliği**, daha önce kaydettiğiniz uygulama Kimliğini girin ve **gizli**, daha önce kaydettiğiniz istemci gizli anahtarını girin.
9. İsteğe bağlı olarak, bir değer girin **Domain_hint**. Örneğin, `ContosoAD`. Bu değeri kullanarak bu kimlik sağlayıcısını söz konusu olduğunda kullanılacak olan *domain_hint* istek. 
10. **Tamam**'ı tıklatın.
11. Seçin **bu kimlik sağlayıcısının taleplerini Eşle** ve aşağıdaki talep ayarlayın:
    
    - İçin **kullanıcı kimliği**, girin `oid`.
    - İçin **görünen ad**, girin `name`.
    - İçin **verilen ad**, girin `given_name`.
    - İçin **Soyadı**, girin `family_name`.
    - İçin **e-posta**, girin `unique_name`.

12. Tıklayın **Tamam**ve ardından **Oluştur** yapılandırmanızı kaydetmek için.
