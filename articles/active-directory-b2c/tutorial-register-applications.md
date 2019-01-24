---
title: Öğretici - Azure Active Directory B2C'de uygulamalarınızı kaydetme | Microsoft Docs
description: Azure Active Directory B2C Azure portalını kullanarak uygulamalarınızda kaydetme hakkında bilgi edinin.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 01/11/2019
ms.author: davidmu
ms.openlocfilehash: 99ad1bbaa732b1207ead9da8da36f345d4978241
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54856035"
---
# <a name="tutorial-register-your-applications-in-azure-active-directory-b2c"></a>Öğretici: Uygulamalarınızı Azure Active Directory B2C'de kaydetme

Önce [uygulamaları](active-directory-b2c-apps.md) Azure Active Directory (Azure AD) B2C ile etkileşim kurabilir, yönettiğiniz bir kiracıya kaydedilmesi gerekir. Bu öğreticide Azure portalını kullanarak uygulamaları kaydetme gösterilir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Web uygulaması kaydetme
> * Web API’si kaydetme
> * Mobil veya yerel bir uygulamayı kaydetme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Henüz kendi oluşturmadıysanız [Azure AD B2C Kiracısı](tutorial-create-tenant.md), şimdi oluşturun. Mevcut bir kiracınız kullanabilirsiniz.

## <a name="register-a-web-application"></a>Web uygulaması kaydetme

1. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.

    ![Abonelik dizinine geçin](./media/tutorial-register-applications/switch-directories.png)

2. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
3. Seçin **uygulamaları**ve ardından **Ekle**.

    ![Uygulama ekleme](./media/tutorial-register-applications/add-application.png)

4. Uygulama için bir ad girin. Örneğin, *webapp1*.
5. İçin **içeren web uygulaması / web API'sini** ve **örtük akışa izin ver**seçin **Evet**.
6. İçin **yanıt URL'si**, Azure AD B2C, uygulamanız tarafından istenen belirteçleri döndürdüğü bir uç noktasını girin. Örneğin, yerel olarak dinlemesine ayarlayabilirsiniz `https://localhost:44316` bağlantı noktası numarasını henüz bilmiyorsanız, bir yer tutucu değerini girin ve daha sonra değiştirin. Test amacıyla, ayarlayabilirsiniz `https://jwt.ms`, inceleme için bir belirteç içeriğini görüntüler. Bu öğreticide, ayarlayın `https://jwt.ms`. 

    Yanıt URL'si ile şemasıyla başlamalı `https`, ve tüm yanıt URL'si değerleri tek bir DNS etki alanı paylaşım gerekir. Örneğin, uygulamanın yanıt URL'sini varsa `https://login.contoso.com`, kendisine bu URL gibi ekleyebilirsiniz `https://login.contoso.com/new`. Veya bir DNS alt etki alanı için başvurabilirsiniz `login.contoso.com`, gibi `https://new.login.contoso.com`. İle bir uygulama istiyorsanız `login-east.contoso.com` ve `login-west.contoso.com` yanıt URL'leri gibi bu yanıt URL'lerini şu sırayla eklemeniz gerekir: `https://contoso.com`, `https://login-east.contoso.com`, `https://login-west.contoso.com`. İlk yanıt URL'sinin alt etki alanlarını oldukları için sonraki iki ekleyebilirsiniz `contoso.com`.

7. **Oluştur**’a tıklayın.

    ![Uygulama özelliklerini ayarlama](./media/tutorial-register-applications/application-properties.png)

### <a name="create-a-client-secret"></a>İstemci gizli dizi oluşturma

Size uygulama bir belirteç kodunu değiştirir, bir uygulama gizli dizisi oluşturmanız gerekir.

1. Seçin **anahtarları** ve ardından **anahtar üret**.

    ![Anahtarları oluştur](./media/tutorial-register-applications/generate-keys.png)

2. Seçin **Kaydet** anahtarı görüntülemek için. **Uygulama anahtarı** değerini not edin. Bu değeri, uygulamanızın kodunda uygulama gizli dizisi olarak kullanırsınız.

    ![Anahtarı kaydetme](./media/tutorial-register-applications/save-key.png)
    
3. Seçin **API erişimi**, tıklayın **Ekle**, web API'si ve kapsamlarınızı (izinler) seçin.

    ![API erişimi yapılandırma](./media/tutorial-register-applications/api-access.png)

## <a name="register-a-web-api"></a>Web API’si kaydetme

1. Seçin **uygulamaları**ve ardından **Ekle**.
3. Uygulama için bir ad girin. Örneğin, *webapi1*.
4. İçin **içeren web uygulaması / web API'sini** ve **örtük akışa izin ver**seçin **Evet**.
5. İçin **yanıt URL'si**, Azure AD B2C, uygulamanız tarafından istenen belirteçleri döndürdüğü bir uç noktasını girin. Örneğin, yerel olarak dinlemesine ayarlayabilirsiniz `https://localhost:44316`. Bağlantı noktası numarasını henüz bilmiyorsanız, bir yer tutucu değerini girin ve daha sonra değiştirin.
6. İçin **uygulama kimliği URI'si**, web API'niz için kullanılan tanımlayıcı girin. Tam etki alanı ile birlikte URI tanımlayıcısı sizin için oluşturulur. Örneğin, `https://contosotenant.onmicrosoft.com/api`.
7. **Oluştur**’a tıklayın.
8. Seçin *webapi1* oluşturduğunuz ve ardından uygulama **yayımlanan kapsamlar** gereken diğer kapsamları eklemek için. Varsayılan olarak, `user_impersonation` kapsamı tanımlanır. `user_impersonation` Kapsamı, diğer uygulamaların oturum açmış kullanıcı adına bu api'de imkanı sunar. İsterseniz, `user_impersonation` kapsam kaldırılabilir.

    ![Yayımlanan kapsamlar ayarlayın](./media/tutorial-register-applications/published-scopes.png)


## <a name="register-a-mobile-or-native-application"></a>Mobil veya yerel bir uygulamayı kaydetme

1. Seçin **uygulamaları**ve ardından **Ekle**.
2. Uygulama için bir ad girin. Örneğin, *nativeapp1*.
3. İçin **içeren web uygulaması / web API'sini**seçin **Hayır**.
4. İçin **yerel istemciyi dahil et**seçin **Evet**.
5. İçin **yeniden yönlendirme URI'si**, özel bir düzen ile geçerli bir yeniden yönlendirme URI'si girin. Yeniden yönlendirme URI'si seçerken iki önemli noktalar vardır:

    - **Benzersiz** -yeniden yönlendirme URI'si şeması her uygulama için benzersiz olmalıdır. Örnekte `com.onmicrosoft.contoso.appname://redirect/path`, `com.onmicrosoft.contoso.appname` düzenidir. Bu düzen gelmelidir. İki uygulama aynı şemayı paylaşıyorsa, kullanıcı bir uygulama seçmek için bir seçenek verilir. Kullanıcı yanlış seçim yaparsa, oturum açma başarısız olur.
    - **Tam** -yeniden yönlendirme URI'SİNİN bir şeması ve yolu olmalıdır. Yol, etki alanından sonra en az bir eğik çizgi içermelidir. Örneğin, `//contoso/` çalışır ve `//contoso` başarısız olur. Yeniden yönlendirme URI'si, alt çizgi gibi özel karakterleri içermeyen emin olun.

6. **Oluştur**’a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, öğrendiğiniz nasıl yapılır:

> [!div class="checklist"]
> * Web uygulaması kaydetme
> * Web API’si kaydetme
> * Mobil veya yerel bir uygulamayı kaydetme

> [!div class="nextstepaction"]
> [Azure Active Directory B2C'de kullanıcı akışları oluşturma](tutorial-create-user-flows.md)