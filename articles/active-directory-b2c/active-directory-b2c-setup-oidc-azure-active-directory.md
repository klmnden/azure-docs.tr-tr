---
title: Oturum açmak için bir Azure Active Directory kuruluş - Azure Active Directory B2C ayarlama | Microsoft Docs
description: Oturum açma için Azure Active Directory B2C, belirli bir Azure Active Directory kuruluşunu ayarlayın.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 76585f91358ad4744dd5ae1f426afda0650d9a8f
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64704006"
---
# <a name="set-up-sign-in-for-a-specific-azure-active-directory-organization-in-azure-active-directory-b2c"></a>Azure Active Directory B2C, belirli bir Azure Active Directory kuruluş için oturum açma ayarlama

>[!NOTE]
> Bu özellik genel önizleme aşamasındadır. Bu özellik, üretim ortamında kullanmayın.

Bir Azure Active Directory (Azure AD) olarak kullanılacak bir [kimlik sağlayıcısı](active-directory-b2c-reference-oauth-code.md) Azure AD B2C'de, temsil ettiği bir uygulama oluşturmanız gerekir. Belirli bir Azure AD kullanıcıları için oturum açma olanağı tanıma bu makale Azure AD B2C'de bir kullanıcı kullanarak kuruluş akış.

## <a name="create-an-azure-ad-app"></a>Bir Azure AD uygulamanızı oluşturma

Belirli kullanıcılar için oturum açma etkinleştirmek için Azure AD kuruluş ihtiyacınız kuruluş içinde bir uygulamayı kaydetme Azure AD kiracısı, Azure AD B2C kiracınızı ile aynı değil.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üst menüde dizin ve abonelik filtresi tıklattıktan sonra Azure AD kiracınıza içeren dizine seçerek Azure AD kiracınıza içeren dizine kullandığınızdan emin olun.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **uygulama kayıtları**.
4. **Yeni uygulama kaydı**’nı seçin.
5. Uygulamanız için bir ad girin. Örneğin, `Azure AD B2C App`.
6. İçin **uygulama türü**seçin `Web app / API`.
7. İçin **oturum açma URL'si**, tüm küçük harfleri, aşağıdaki URL'yi girin burada `your-B2C-tenant-name` Azure AD B2C kiracınızın adı ile değiştirilir. Örneğin, `https://fabrikam.b2clogin.com/fabrikam.onmicrosoft.com/oauth2/authresp`:

    ```
    https://your-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```

    Tüm URL'leri artık kullanması gereken [b2clogin.com](b2clogin.md).

8. **Oluştur**’a tıklayın. Kopyalama **uygulama kimliği** daha sonra kullanılacak.
9. Uygulamayı seçin ve ardından **ayarları**.
10. Seçin **anahtarları**anahtar için bir açıklama girin, bir süre seçin ve ardından **Kaydet**. Daha sonra kullanılmak üzere görüntülenen anahtarının değerini kopyalayın.

## <a name="configure-azure-ad-as-an-identity-provider"></a>Kimlik sağlayıcısı olarak Azure AD'yi yapılandırma

1. Azure AD B2C kiracısı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve Azure AD B2C kiracınızı içeren dizine seçme.
2. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
3. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
4. Girin bir **adı**. Örneğin, "Contoso Azure AD" girin.
5. Seçin **kimlik sağlayıcısı türü**seçin **Open ID Connect (Önizleme)** ve ardından **Tamam**.
6. Tıklayın **bu kimlik sağlayıcısını ayarlama**
7. İçin **meta veri URL'si**, değiştirerek aşağıdaki URL'yi girin `your-AD-tenant-domain` Azure AD kiracınızın etki alanı adına sahip. Örneğin `https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration`:

    ```
    https://login.microsoftonline.com/your-AD-tenant-domain/.well-known/openid-configuration
    ```

8. İçin **istemci kimliği**, daha önce kaydettiğiniz uygulama Kimliğini girin ve **gizli**, daha önce kaydettiğiniz anahtarı değerini girin.
9. İsteğe bağlı olarak, bir değer girin **Domain_hint**. Örneğin, `ContosoAD`. Bu değeri kullanarak bu kimlik sağlayıcısını söz konusu olduğunda kullanılacak olan *domain_hint* istek. 
10. **Tamam** düğmesine tıklayın.
11. Seçin **bu kimlik sağlayıcısının taleplerini Eşle** ve aşağıdaki talep ayarlayın:
    
    - İçin **kullanıcı kimliği**, girin `oid`.
    - İçin **görünen ad**, girin `name`.
    - İçin **verilen ad**, girin `given_name`.
    - İçin **Soyadı**, girin `family_name`.
    - İçin **e-posta**, girin `unique_name`.

12. Tıklayın **Tamam**ve ardından **Oluştur** yapılandırmanızı kaydetmek için.
