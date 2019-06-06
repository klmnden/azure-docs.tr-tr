---
title: Öğretici - bir uygulama kaydı - Azure Active Directory B2C | Microsoft Docs
description: Azure portalını kullanarak bir web uygulamasında Azure Active Directory B2C kaydetme hakkında bilgi edinin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 02/05/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 380fc1633f94f2365162c1a4e4087c9113e5f663
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66511938"
---
# <a name="tutorial-register-an-application-in-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C'de bir uygulamayı kaydetme

Önce [uygulamaları](active-directory-b2c-apps.md) Azure Active Directory (Azure AD) B2C ile etkileşim kurabilir, yönettiğiniz bir kiracıya kaydedilmesi gerekir. Bu öğreticide, Azure portalını kullanarak bir web uygulaması kaydetme işlemini göstermektedir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Web uygulaması kaydetme
> * İstemci gizli dizi oluşturma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Henüz kendi oluşturmadıysanız [Azure AD B2C Kiracısı](tutorial-create-tenant.md), şimdi oluşturun. Mevcut bir Azure AD B2C kiracınızı kullanabilirsiniz.

## <a name="register-a-web-application"></a>Web uygulaması kaydetme

1. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
2. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
3. Seçin **uygulamaları**ve ardından **Ekle**.
4. Uygulama için bir ad girin. Örneğin, *webapp1*.
5. İçin **içeren web uygulaması / web API'sini** ve **örtük akışa izin ver**seçin **Evet**.
6. İçin **yanıt URL'si**, Azure AD B2C, uygulamanız tarafından istenen belirteçleri döndürdüğü bir uç noktasını girin. Örneğin, yerel olarak dinlemesine ayarlayabilirsiniz `https://localhost:44316` bağlantı noktası numarasını henüz bilmiyorsanız, bir yer tutucu değerini girin ve daha sonra değiştirin. Test amacıyla, ayarlayabilirsiniz `https://jwt.ms`, inceleme için bir belirteç içeriğini görüntüler. Bu öğreticide, ayarlayın `https://jwt.ms`. 

    Yanıt URL'si ile şemasıyla başlamalı `https`, ve tüm yanıt URL'si değerleri tek bir DNS etki alanı paylaşım gerekir. Örneğin, uygulamanın yanıt URL'sini varsa `https://login.contoso.com`, kendisine bu URL gibi ekleyebilirsiniz `https://login.contoso.com/new`. Veya bir DNS alt etki alanı için başvurabilirsiniz `login.contoso.com`, gibi `https://new.login.contoso.com`. İle bir uygulama istiyorsanız `login-east.contoso.com` ve `login-west.contoso.com` yanıt URL'leri gibi bu yanıt URL'lerini şu sırayla eklemeniz gerekir: `https://contoso.com`, `https://login-east.contoso.com`, `https://login-west.contoso.com`. İlk yanıt URL'sinin alt etki alanlarını oldukları için sonraki iki ekleyebilirsiniz `contoso.com`.

7. **Oluştur**’a tıklayın.

## <a name="create-a-client-secret"></a>İstemci gizli dizi oluşturma

Uygulamanızı bir belirteç kodunu değiştirir, bir uygulama gizli anahtarı oluşturmak gerekir.

1. Seçin **anahtarları** ve ardından **anahtar üret**.
2. Seçin **Kaydet** anahtarı görüntülemek için. **Uygulama anahtarı** değerini not edin. Bu değeri, uygulamanızın kodunda uygulama gizli dizisi olarak kullanırsınız.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, öğrendiğiniz nasıl yapılır:

> [!div class="checklist"]
> * Web uygulaması kaydetme
> * İstemci gizli dizi oluşturma

> [!div class="nextstepaction"]
> [Azure Active Directory B2C'de kullanıcı akışları oluşturma](tutorial-create-user-flows.md)
