---
title: Öğretici - kimlik sağlayıcıları, uygulamalarınıza - Azure Active Directory B2C ekleme | Microsoft Docs
description: Kimlik sağlayıcıları, Azure portalını kullanarak uygulamalarınıza Azure Active Directory B2C'de eklemeyi öğrenin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 02/01/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 76e12dc6bf9bcb50dc58e7730f3a08dd6a9d4440
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66512205"
---
# <a name="tutorial-add-identity-providers-to-your-applications-in-azure-active-directory-b2c"></a>Öğretici: Kimlik sağlayıcıları Azure Active Directory B2C uygulamalarınızın ekleyin

Uygulamalarınızda farklı kimlik sağlayıcıları ile oturum açmasını sağlamak isteyebilirsiniz. Bir *kimlik sağlayıcısı* oluşturur, korur ve uygulamalar için kimlik doğrulama hizmetleri sağlarken kimlik bilgilerini yönetir. Azure Active Directory (Azure AD) B2C'ye tarafından desteklenen kimlik sağlayıcıları ekleyebilirsiniz, [kullanıcı akışları](active-directory-b2c-reference-policies.md) Azure portalını kullanarak.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Kimlik sağlayıcısı uygulamalar oluşturma
> * Kimlik sağlayıcıları kiracınıza ekleyin
> * Kimlik sağlayıcıları kullanıcı akışınıza ekleme

Uygulamalarınızda genellikle yalnızca bir kimlik sağlayıcısı kullanın, ancak daha fazla ekleme seçeneğine sahipsiniz. Bu öğreticide bir Azure AD kimlik sağlayıcısı ve Facebook kimlik sağlayıcısı, uygulamanıza nasıl ekleneceğini gösterir. Bu kimlik sağlayıcılarından hem de uygulamanıza eklemek isteğe bağlıdır. Diğer kimlik sağlayıcıları gibi ekleyebilirsiniz [Amazon](active-directory-b2c-setup-amzn-app.md), [Github](active-directory-b2c-setup-github-app.md), [Google](active-directory-b2c-setup-goog-app.md), [LinkedIn](active-directory-b2c-setup-li-app.md), [Microsoft](active-directory-b2c-setup-msa-app.md), veya [Twitter](active-directory-b2c-setup-twitter-app.md). 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

[Kullanıcı akışı Oluştur](tutorial-create-user-flows.md) etkinleştirme kullanıcıların kaydolmak ve uygulamanız için oturum açın. 

## <a name="create-applications"></a>Uygulama oluşturma

Kimlik sağlayıcısı uygulama tanımlayıcısı ve Azure AD B2C kiracınızı ile iletişimi etkinleştirmek için anahtar sağlar. Öğreticinin bu bölümünde, bir Azure AD uygulaması ve tanımlayıcıları ve anahtarları, kiracınıza kimlik sağlayıcıları eklemek için size bir Facebook uygulaması oluşturun. Kimlik sağlayıcıları yalnızca biri ekliyorsanız, yalnızca bu sağlayıcı için bir uygulama oluşturmanız gerekir.

### <a name="create-an-azure-active-directory-application"></a>Bir Azure Active Directory uygulaması oluşturma

Azure ad kullanıcıları için oturum açma etkinleştirmek için uygulamanın Azure AD kiracısı içindeki'ı kaydetmeniz gerekir. Azure AD kiracısı, Azure AD B2C kiracısı ile aynı değil.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD kiracınıza tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve Azure AD kiracınıza içeren dizine seçme.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **uygulama kayıtları**.
4. **Yeni uygulama kaydı**’nı seçin.
5. Uygulamanız için bir ad girin. Örneğin, `Azure AD B2C App`.
6. İçin **uygulama türü**seçin `Web app / API`.
7. İçin **oturum açma URL'si**, tüm küçük harfleri, aşağıdaki URL'yi girin burada `your-B2C-tenant-name` Azure AD B2C kiracınızın adı ile değiştirilir.

    ```
    https://your-B2C-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```
    
    Örneğin, `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.
    
    Tüm URL'leri artık kullanması gereken [b2clogin.com](b2clogin.md).

8. **Oluştur**’a tıklayın. Kopyalama **uygulama kimliği** daha sonra kullanılacak.
9. Uygulamayı seçin ve ardından **ayarları**.
10. Seçin **anahtarları**anahtar için bir açıklama girin, bir süre seçin ve ardından **Kaydet**. Bu öğreticinin ilerleyen bölümlerinde kullanmak anahtar değerini kopyalayın.

### <a name="create-a-facebook-application"></a>Bir Facebook uygulaması oluşturun

Bir Facebook hesabıyla bir kimlik sağlayıcısı olarak Azure AD B2C'yi kullanmak için Facebook bir uygulama oluşturmak gerekir. Bir Facebook hesabı yoksa adresinden edinebilirsiniz [ https://www.facebook.com/ ](https://www.facebook.com/).

1. Oturum [geliştiriciler için Facebook](https://developers.facebook.com/) Facebook hesabı kimlik bilgilerinizle.
2. Zaten yapmadıysanız, Facebook geliştiricisi olarak kaydolma gerekir. Seçin kaydetmek için **kaydetme** sayfanın sağ üst köşesindeki Facebook'ın ilkeleri kabul edin ve kayıt adımları tamamlayın.
3. Seçin **uygulamalarım** ve ardından **yeni uygulama Ekle**. 
4. Girin bir **görünen ad** ve geçerli bir **ilgili kişi e-posta**.
5. Tıklayın **uygulama kimliği oluşturma**. Facebook platform ilkeleri kabul edin ve çevrimiçi güvenlik denetimini tamamlamak için gerekli.
6. Seçin **ayarları** > **temel**.
7. Seçin bir **kategori**, örneğin `Business and Pages`. Bu değer tarafından Facebook gerekli, ancak Azure AD B2C için kullanılmaz.
8. Sayfanın en altında seçin **Platform Ekle**ve ardından **Web sitesi**.
9. İçinde **Site URL'si**, girin `https://your-tenant-name.b2clogin.com/` değiştirerek `your-tenant-name` kiracınızın ada sahip. Bir URL girin **gizlilik ilkesi URL'si**, örneğin `http://www.contoso.com`. İlke URL'si, uygulamanız için gizlilik bilgileri sağlamak için bakımını bir sayfadır.
10. Seçin **değişiklikleri kaydetmek**.
11. Sayfanın üst kısmında değerini kopyalayın **uygulama kimliği**. 
12. Tıklayın **Göster** ve değerini kopyalayın **uygulama gizli anahtarı**. Facebook kimlik sağlayıcısı olarak kiracınızda yapılandırmak için bu ikisinin de kullanın. **Uygulama gizli anahtarı** bir önemli güvenlik kimlik bilgisidir.
13. Seçin **ürünleri**ve ardından **ayarlanan** altında **Facebook oturum açma**.
14. Seçin **ayarları** altında **Facebook oturum açma**.
15. İçinde **geçerli OAuth yeniden yönlendirme URI'leri**, girin `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp`. Değiştirin `your-tenant-name` kiracınızın ada sahip. Tıklayın **Değişiklikleri Kaydet** sayfanın alt kısmındaki.
16. Facebook uygulamanızı Azure AD B2C için kullanılabilir hale getirme için tıklatın **durumu** Seçici üst sayfanın sağ ve **üzerinde**. **Onayla**'ya tıklayın. Bu noktada, gelen durum değiştirmelisiniz **geliştirme** için **canlı**.

## <a name="add-the-identity-providers"></a>Kimlik Sağlayıcıları Ekle

Eklemek istediğiniz kimlik sağlayıcısı için uygulamayı oluşturduktan sonra kiracınız için kimlik sağlayıcısı ekleyin.

### <a name="add-the-azure-active-directory-identity-provider"></a>Azure Active Directory kimlik sağlayıcısı Ekle

1. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve Azure AD B2C kiracınızı içeren dizine seçme.
2. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
3. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
4. Girin bir **adı**. Örneğin, *Contoso Azure AD*.
5. Seçin **kimlik sağlayıcısı türü**seçin **Open ID Connect (Önizleme)** ve ardından **Tamam**.
6. Tıklayın **bu kimlik sağlayıcısını ayarlama**
7. İçin **meta veri URL'si**, değiştirerek aşağıdaki URL'yi girin `your-AD-tenant-domain` Azure AD kiracınızın etki alanı adına sahip.

    ```
    https://login.microsoftonline.com/your-AD-tenant-domain/.well-known/openid-configuration
    ```

    Örneğin, `https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration`.

8. İçin **istemci kimliği**, daha önce kaydettiğiniz uygulama Kimliğini girin ve **gizli**, daha önce kaydettiğiniz anahtarı değerini girin.
9. İsteğe bağlı olarak, bir değer girin **Domain_hint**. Örneğin, `ContosoAD`. 
10. **Tamam**'ı tıklatın.
11. Seçin **bu kimlik sağlayıcısının taleplerini Eşle** ve aşağıdaki talep ayarlayın:
    
    - İçin **kullanıcı kimliği**, girin `oid`.
    - İçin **görünen ad**, girin `name`.
    - İçin **verilen ad**, girin `given_name`.
    - İçin **Soyadı**, girin `family_name`.
    - İçin **e-posta**, girin `unique_name`.

12. Tıklayın **Tamam**ve ardından **Oluştur** yapılandırmanızı kaydetmek için.

### <a name="add-the-facebook-identity-provider"></a>Facebook kimlik sağlayıcısı Ekle

1. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
2. Girin bir **adı**. Örneğin, *Facebook*.
3. Seçin **kimlik sağlayıcısı türü**seçin **Facebook**, tıklatıp **Tamam**.
4. Seçin **bu kimlik sağlayıcısını ayarlama** olarak daha önce kaydedilen uygulama kimliği girin **istemci kimliği**. Olarak kayıtlı uygulama gizli anahtarı girin **gizli**.
5. Tıklayın **Tamam** ve ardından **Oluştur** Facebook yapılandırmanızı kaydetmek için.

## <a name="update-the-user-flow"></a>Kullanıcı akışı güncelleştir

Önkoşulların bir parçası tamamlanan öğreticide oluşturduğunuz kullanıcı akışı kaydolma ve oturum açma adlı *B2C_1_signupsignin1*. Bu bölümde, kimlik sağlayıcıları için eklediğiniz *B2C_1_signupsignin1* kullanıcı akışı.

1. Seçin **kullanıcı akışları (ilke)** ve ardından *B2C_1_signupsignin1* kullanıcı akışı.
2. Seçin **kimlik sağlayıcıları**seçin **Facebook** ve **Contoso Azure AD** eklediğiniz kimlik sağlayıcıları.
3. **Kaydet**’i seçin.

### <a name="test-the-user-flow"></a>Kullanıcı akışı test edin

1. Oluşturduğunuz kullanıcı akışı genel bakış sayfasında **kullanıcı akışı çalıştırma**.
2. İçin **uygulama**, adlı web uygulamasını seçin *webapp1* daha önce kaydettiğiniz. **Yanıt URL'si** göstermelidir `https://jwt.ms`.
3. Tıklayın **kullanıcı akışı çalıştırma**ve ardından daha önce eklediğiniz bir kimlik sağlayıcısı oturum.
4. Eklediğiniz diğer kimlik sağlayıcıları için 1 ile 3 arasındaki adımları yineleyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, öğrendiğiniz nasıl yapılır:

> [!div class="checklist"]
> * Kimlik sağlayıcısı uygulamalar oluşturma
> * Kimlik sağlayıcıları kiracınıza ekleyin
> * Kimlik sağlayıcıları kullanıcı akışınıza ekleme

> [!div class="nextstepaction"]
> [Uygulamalarınızı Azure Active Directory B2C, kullanıcı arabirimini özelleştirme](tutorial-customize-ui.md)