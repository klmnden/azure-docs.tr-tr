---
title: Öğretici - uygulamalarınıza - Azure Active Directory B2C kimlik Sağlayıcıları Ekle
description: Kimlik sağlayıcıları, Azure portalını kullanarak uygulamalarınıza Azure Active Directory B2C'de eklemeyi öğrenin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 07/08/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 33f595dd36ac9448cc1276647f9943326b0b74c1
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67655230"
---
# <a name="tutorial-add-identity-providers-to-your-applications-in-azure-active-directory-b2c"></a>Öğretici: Kimlik sağlayıcıları Azure Active Directory B2C uygulamalarınızın ekleyin

Uygulamalarınızda farklı kimlik sağlayıcıları ile oturum açmasını sağlamak isteyebilirsiniz. Bir *kimlik sağlayıcısı* oluşturur, korur ve uygulamalar için kimlik doğrulama hizmetleri sağlarken kimlik bilgilerini yönetir. Azure Active Directory (Azure AD) B2C'ye tarafından desteklenen kimlik sağlayıcıları ekleyebilirsiniz, [kullanıcı akışları](active-directory-b2c-reference-policies.md) Azure portalını kullanarak.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Kimlik sağlayıcısı uygulamalar oluşturma
> * Kimlik sağlayıcıları kiracınıza ekleyin
> * Kimlik sağlayıcıları kullanıcı akışınıza ekleme

Uygulamalarınızda genellikle yalnızca bir kimlik sağlayıcısı kullanın, ancak daha fazla ekleme seçeneğine sahipsiniz. Bu öğreticide bir Azure AD kimlik sağlayıcısı ve Facebook kimlik sağlayıcısı, uygulamanıza nasıl ekleneceğini gösterir. Bu kimlik sağlayıcılarından hem de uygulamanıza eklemek isteğe bağlıdır. Diğer kimlik sağlayıcıları gibi ekleyebilirsiniz [Amazon](active-directory-b2c-setup-amzn-app.md), [GitHub](active-directory-b2c-setup-github-app.md), [Google](active-directory-b2c-setup-goog-app.md), [LinkedIn](active-directory-b2c-setup-li-app.md), [Microsoft](active-directory-b2c-setup-msa-app.md), veya [Twitter](active-directory-b2c-setup-twitter-app.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

[Kullanıcı akışı Oluştur](tutorial-create-user-flows.md) etkinleştirme kullanıcıların kaydolmak ve uygulamanız için oturum açın.

## <a name="create-applications"></a>Uygulama oluşturma

Kimlik sağlayıcısı uygulama tanımlayıcısı ve Azure AD B2C kiracınızı ile iletişimi etkinleştirmek için anahtar sağlar. Öğreticinin bu bölümünde, bir Azure AD uygulaması ve tanımlayıcıları ve anahtarları, kiracınıza kimlik sağlayıcıları eklemek için size bir Facebook uygulaması oluşturun. Kimlik sağlayıcıları yalnızca biri ekliyorsanız, yalnızca bu sağlayıcı için bir uygulama oluşturmanız gerekir.

### <a name="create-an-azure-active-directory-application"></a>Bir Azure Active Directory uygulaması oluşturma

Azure ad kullanıcıları için oturum açma etkinleştirmek için uygulamanın Azure AD kiracısı içindeki'ı kaydetmeniz gerekir. Azure AD kiracısı, Azure AD B2C kiracısı ile aynı değil.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Azure AD kiracınıza tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve Azure AD kiracınıza içeren dizine seçme.
1. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **uygulama kayıtları**.
1. Seçin **yeni kayıt**.
1. Uygulamanız için bir ad girin. Örneğin: `Azure AD B2C App`.
1. Seçimi kabul **hesapları yalnızca kuruluş bu dizinde** bu uygulama için.
1. İçin **yeniden yönlendirme URI'si**, değerini kabul **Web** ve tüm değiştirerek kodunda küçük harfler, aşağıdaki URL'yi girin `your-B2C-tenant-name` Azure AD B2C kiracınızın adı.

    ```
    https://your-B2C-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```

    Örneğin: `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.

    Tüm URL'leri artık kullanması gereken [b2clogin.com](b2clogin.md).

1. Seçin **kaydetme**, ardından kayıt **uygulama (istemci) kimliği** daha sonraki bir adımda kullanacağınız.
1. Altında **Yönet** uygulama menüde **sertifikaları ve parolaları**, ardından **yeni gizli**.
1. Girin bir **açıklama** için istemci gizli anahtarı. Örneğin: `Azure AD B2C App Secret`.
1. Süre sonu dönemi seçin. Bu uygulama için seçimi kabul **1 yıl**.
1. Seçin **Ekle**, ardından bir sonraki adımda kullanacağınız yeni istemci gizli anahtarı değerini kaydedin.

### <a name="create-a-facebook-application"></a>Bir Facebook uygulaması oluşturun

Bir Facebook hesabıyla bir kimlik sağlayıcısı olarak Azure AD B2C'yi kullanmak için Facebook bir uygulama oluşturmak gerekir. Bir Facebook hesabı yoksa adresinden edinebilirsiniz [ https://www.facebook.com/ ](https://www.facebook.com/).

1. Oturum [geliştiriciler için Facebook](https://developers.facebook.com/) Facebook hesabı kimlik bilgilerinizle.
1. Zaten yapmadıysanız, Facebook geliştiricisi olarak kaydolma gerekir. Bunu yapmak için **Başlarken** sayfanın sağ üst köşesindeki Facebook'ın ilkeleri kabul edin ve kayıt adımları tamamlayın.
1. Seçin **uygulamalarım** ardından **uygulaması oluşturma**.
1. Girin bir **görünen ad** ve geçerli bir **ilgili kişi e-posta**.
1. Tıklayın **uygulama kimliği oluşturma**. Bu, Facebook platform ilkeleri kabul edin ve çevrimiçi güvenlik denetimini Tamamla gerektirebilir.
1. Seçin **ayarları** > **temel**.
1. Seçin bir **kategori**, örneğin `Business and Pages`. Bu değer tarafından Facebook gereklidir, ancak Azure AD B2C tarafından kullanılmaz.
1. Sayfanın en altında seçin **Platform Ekle**ve ardından **Web sitesi**.
1. İçinde **Site URL'si**, girin `https://your-tenant-name.b2clogin.com/` değiştirerek `your-tenant-name` kiracınızın ada sahip.
1. Bir URL girin **gizlilik ilkesi URL'si**, örneğin `http://www.contoso.com/`. Gizlilik İlkesi URL'si, uygulamanız için gizlilik bilgileri sağlamak için bakımını bir sayfadır.
1. Seçin **değişiklikleri kaydetmek**.
1. Sayfanın üst kısmında değerini kayıt **uygulama kimliği**.
1. Yanındaki **uygulama gizli anahtarı**seçin **Göster** ve değerini kaydedin. Facebook kimlik sağlayıcısı olarak kiracınızda yapılandırmak için uygulama kimliği ve uygulama gizli anahtarı'nı kullanın. **Uygulama gizli anahtarı** güvenli bir şekilde saklamalısınız bir önemli güvenlik kimlik bilgisidir.
1. Artı işaretini seçin **ürünleri**, altında **Facebook oturum açma**seçin **ayarlanan**.
1. Altında **Facebook oturum açma** sol taraftaki menüde **ayarları**.
1. İçinde **geçerli OAuth yeniden yönlendirme URI'leri**, girin `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp`. Değiştirin `your-tenant-name` kiracınızın ada sahip. Seçin **Değişiklikleri Kaydet** sayfanın alt kısmındaki.
1. Facebook uygulamanızı Azure AD B2C için kullanılabilir hale getirme için tıklatın **durumu** Seçici üst sayfanın sağ gidip **üzerinde** uygulama genel yapın ve ardından **Onayla** . Bu noktada, gelen durum değiştirmelisiniz **geliştirme** için **canlı**.

## <a name="add-the-identity-providers"></a>Kimlik Sağlayıcıları Ekle

Eklemek istediğiniz kimlik sağlayıcısı için uygulamayı oluşturduktan sonra kiracınız için kimlik sağlayıcısı ekleyin.

### <a name="add-the-azure-active-directory-identity-provider"></a>Azure Active Directory kimlik sağlayıcısı Ekle

1. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve Azure AD B2C kiracınızı içeren dizine seçme.
1. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
1. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
1. Girin bir **adı**. Örneğin, *Contoso Azure AD*.
1. Seçin **kimlik sağlayıcısı türü**seçin **Open ID Connect (Önizleme)** ve ardından **Tamam**.
1. Tıklayın **bu kimlik sağlayıcısını ayarlama**
1. İçin **meta veri URL'si**, aşağıdaki URL'yi girin değiştirerek `your-AD-tenant-domain` Azure AD kiracınızın etki alanı adına sahip.

    ```
    https://login.microsoftonline.com/your-AD-tenant-domain/.well-known/openid-configuration
    ```

    Örneğin: `https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration`.

1. İçin **istemci kimliği**, girin *uygulama (istemci) kimliği* daha önce kaydettiğiniz.
1. İçin **gizli**, girin *gizli* daha önce kaydettiğiniz bir değer.
1. İsteğe bağlı olarak, bir değer girin **Domain_hint**. Örneğin: `ContosoAD`. [Etki alanı İpucu](../active-directory/manage-apps/configure-authentication-for-federated-users-portal.md) uygulamadan kimlik doğrulama isteği bulunan yönergeleri. Kullanıcının kendi Federasyon Idp'nin oturum açma sayfasına hızlandırmak için kullanılabilir. Veya çok kiracılı bir uygulama tarafından kullanıcı doğrudan markalı hızlandırmak için kullanılabilmesi için kendi Kiracı için Azure AD oturum açma sayfası.
1. **Tamam**’ı seçin.
1. Seçin **bu kimlik sağlayıcısının taleplerini Eşle** ve aşağıdaki talep ayarlayın:

    - İçin **kullanıcı kimliği**, girin `oid`.
    - İçin **görünen ad**, girin `name`.
    - İçin **verilen ad**, girin `given_name`.
    - İçin **Soyadı**, girin `family_name`.
    - İçin **e-posta**, girin `unique_name`.

1. Seçin **Tamam**, ardından **Oluştur** yapılandırmanızı kaydetmek için.

### <a name="add-the-facebook-identity-provider"></a>Facebook kimlik sağlayıcısı Ekle

1. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
1. Girin bir **adı**. Örneğin, *Facebook*.
1. Seçin **kimlik sağlayıcısı türü**seçin **Facebook**, ardından **Tamam**.
1. Seçin **bu kimlik sağlayıcısını ayarlama** girin *uygulama kimliği* olarak daha önce kaydettiğiniz **istemci kimliği**.
1. Girin *uygulama gizli anahtarı* olarak kaydettiğiniz **gizli**.
1. Seçin **Tamam** seçip **Oluştur** Facebook yapılandırmanızı kaydetmek için.

## <a name="update-the-user-flow"></a>Kullanıcı akışı güncelleştir

Önkoşulların bir parçası tamamlanan öğreticide oluşturduğunuz kullanıcı akışı kaydolma ve oturum açma adlı *B2C_1_signupsignin1*. Bu bölümde, kimlik sağlayıcıları için eklediğiniz *B2C_1_signupsignin1* kullanıcı akışı.

1. Seçin **kullanıcı akışları (ilke)** ve ardından *B2C_1_signupsignin1* kullanıcı akışı.
2. Seçin **kimlik sağlayıcıları**seçin **Facebook** ve **Contoso Azure AD** eklediğiniz kimlik sağlayıcıları.
3. **Kaydet**’i seçin.

## <a name="test-the-user-flow"></a>Kullanıcı akışı test edin

1. Oluşturduğunuz kullanıcı akışı genel bakış sayfasında **kullanıcı akışı çalıştırma**.
1. İçin **uygulama**, adlı web uygulamasını seçin *webapp1* daha önce kaydettiğiniz. **Yanıt URL'si** göstermelidir `https://jwt.ms`.
1. Seçin **kullanıcı akışı çalıştırma**ve ardından daha önce eklediğiniz bir kimlik sağlayıcısı oturum.
1. Eklediğiniz diğer kimlik sağlayıcıları için 1 ile 3 arasındaki adımları yineleyin.

Oturum açma işlemi başarılı olursa için yönlendirilirsiniz `https://jwt.ms` görüntüleyen çözülmüş belirteç benzer:

```json
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "<key-ID>"
}.{
  "exp": 1562346892,
  "nbf": 1562343292,
  "ver": "1.0",
  "iss": "https://your-b2c-tenant.b2clogin.com/10000000-0000-0000-0000-000000000000/v2.0/",
  "sub": "20000000-0000-0000-0000-000000000000",
  "aud": "30000000-0000-0000-0000-000000000000",
  "nonce": "defaultNonce",
  "iat": 1562343292,
  "auth_time": 1562343292,
  "name": "User Name",
  "idp": "facebook.com",
  "postalCode": "12345",
  "tfp": "B2C_1_signupsignin1"
}.[Signature]
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, öğrendiğiniz nasıl yapılır:

> [!div class="checklist"]
> * Kimlik sağlayıcısı uygulamalar oluşturma
> * Kimlik sağlayıcıları kiracınıza ekleyin
> * Kimlik sağlayıcıları kullanıcı akışınıza ekleme

Ardından, kendi uygulamalarınızda kimlik deneyiminin bir parçası olarak kullanıcılara gösterilen sayfalarının kullanıcı arabirimini özelleştirmeyi öğrenin:

> [!div class="nextstepaction"]
> [Uygulamalarınızı Azure Active Directory B2C, kullanıcı arabirimini özelleştirme](tutorial-customize-ui.md)
