---
title: Azure Active Directory B2C'de yerleşik ilkeleri kullanarak bir Azure AD sağlayıcısı ekleme | Microsoft Docs
description: Open ID Connect kimlik sağlayıcısı (Azure AD) eklemeyi öğrenin.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 04/27/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 4e756a4bf3d9e42b47c0b96a6bf73e8e2b267b77
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34712394"
---
# <a name="azure-active-directory-b2c-sign-in-using-azure-ad-accounts-through-a-built-in-policy"></a>Azure Active Directory B2C: Yerleşik İlkesi aracılığıyla Azure AD hesapları kullanarak oturum açın.

>[!NOTE]
> Bu özellik genel önizlemede değil. Özellik üretim ortamında kullanmayın.

Bu makalede, oturum açma için belirli bir Azure Active Directory (Azure AD) kuruluş yerleşik ilkeleri kullanıcılardan etkinleştirme gösterilmektedir.

## <a name="create-an-azure-ad-app"></a>Bir Azure AD uygulaması oluştur

Oturum açma için belirli bir kullanıcılardan etkinleştirmek için Azure AD kuruluş ihtiyacınız kuruluş dahilinde bir uygulamayı kaydetmek Azure AD kiracısı.

>[!NOTE]
> "Contoso.com" kuruluş için kullandığımız Azure AD kiracısı ve Azure AD B2C kiracısı'nda aşağıdaki yönergeleri olarak "fabrikamb2c.onmicrosoft.com".

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Üst çubuğunda hesabınızı seçin. Gelen **Directory** listesinde, kuruluş seçin Azure AD kiracısı nerede kaydettiğiniz uygulamanız (contoso.com).
1. Seçin **tüm hizmetleri** sol bölmesinde ve "Uygulama kayıtlar" için arama
1. **Yeni uygulama kaydı**’nı seçin.
1. Uygulamanız için bir ad girin (örneğin, `Azure AD B2C App`).
1. Uygulama türü için **Web uygulaması / API** öğesini seçin.
1. İçin **oturum açma URL'si**, aşağıdaki URL'yi girin nerede `yourtenant` Azure AD B2C kiracınızın adıyla değiştirilen (`fabrikamb2c.onmicrosoft.com`):

    >[!NOTE]
    >"Yourtenant" değeri, tüm küçük harfli olması gerektiğini **oturum açma URL'si**.

    ```Console
    https://login.microsoftonline.com/te/yourtenant.onmicrosoft.com/oauth2/authresp
    ```

1. Sonraki bölümde istemci kimliği olarak kullanacağınız uygulama Kimliğini Kaydet
1. Altında **ayarları** dikey penceresinde, select **anahtarları**.
1. Girin bir **anahtar açıklama** altında **parolaları** bölümünde ve ayarlama **süresi** "Her zaman geçerli olsun" için. 
1. Tıklatın **kaydetmek**ve elde edilen anahtar Not **değeri**, sonraki bölümde istemci gizli anahtarı olarak kullanacağı.

## <a name="configure-azure-ad-as-an-identity-provider-in-your-tenant"></a>Azure AD kiracınızda kimlik sağlayıcısı yapılandırma

1. Üst çubuğunda hesabınızı seçin. Gelen **Directory** listesinde, Azure AD B2C kiracısı (fabrikamb2c.onmicrosoft.com) seçin.
1. [Azure AD B2C ayarlarını menüsüne gidin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) Azure portalında.
1. Azure AD B2C ayarlar menüsünü tıklatın **kimlik sağlayıcıları**.
1. Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.
1. Kolay bir sağlamak **adı** kimlik sağlayıcısı yapılandırması için. Örneğin, "Contoso Azure AD" girin.
1. Tıklatın **kimlik sağlayıcısı türü**seçin **Open ID Connect**, tıklatıp **Tamam**.
1. Tıklatın **bu kimlik sağlayıcısı'nı ayarlama**
1. İçin **meta veri URL'sini**, aşağıdaki URL'yi girin nerede `yourtenant` Azure AD kiracınıza adıyla değiştirilen (örneğin `contoso.com`):

    ```Console
    https://login.microsoftonline.com/yourtenant/.well-known/openid-configuration
    ```
1. İçin **istemci kimliği** ve **gizli**, önceki bölümden uygulama kimliği ve anahtarı girin.
1. İçin varsayılan değer tutmak **kapsam**, hangi ayarlanmalıdır `openid`.
1. İçin varsayılan değer tutmak **yanıt türü**, hangi ayarlanmalıdır `code`.
1. İçin varsayılan değer tutmak **yanıt modu**, hangi ayarlanmalıdır `form_post`.
1. İsteğe bağlı olarak, bir değer girin **etki alanı** (örneğin `ContosoAD`). Kullanarak bu kimlik sağlayıcısı için söz konusu olduğunda kullanılacak değer budur *domain_hint* isteği. 
1. **Tamam**’a tıklayın.
1. Tıklayın **bu kimlik sağlayıcısının talep eşleme**.
1. İçin **kullanıcı kimliği**, girin `oid`.
1. İçin **görünen adı**, girin `name`.
1. İçin **verilen ad**, girin `given_name`.
1. İçin **Soyadı**, girin `family_name`.
1. İçin **e-posta**, girin `unique_name`
1. Tıklatın **Tamam**ve ardından **oluşturma** yapılandırmanızı kaydetmek için.

## <a name="next-steps"></a>Sonraki adımlar

Yeni oluşturulan eklemek için yerleşik bir ilke Azure AD kimlik sağlayıcısı ve geri bildirim sağlayın [ AADB2CPreview@microsoft.com ](mailto:AADB2CPreview@microsoft.com).
