---
title: Bir web uygulaması - Azure Active Directory B2C ekleme | Microsoft Docs
description: Bir Active Directory B2C kiracınıza web uygulamasına eklemeyi öğrenin.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.author: davidmu
ms.date: 04/16/2019
ms.custom: mvc
ms.topic: conceptual
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: ede3fd0dd1d0351e691a9f160260c029d01c8f8a
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64704037"
---
# <a name="add-a-web-api-application-to-your-azure-active-directory-b2c-tenant"></a>Bir web API uygulaması Azure Active Directory B2C kiracınıza ekleyin

 Böylece kabul edebilir ve bir erişim belirteci mevcut istemci uygulamalar tarafından isteklerine yanıt kiracınıza Web API'si kaynaklarına kaydetme. Bu makalede, Azure Active Directory (Azure AD) B2C'de uygulamayı kaydetme işlemini göstermektedir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD B2C kiracınızı içeren dizine kullandığınızdan emin olun. Seçin **dizin ve abonelik filtresi** üst menüdeki ve kiracınız içeren dizini seçin.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
4. Seçin **uygulamaları**ve ardından **Ekle**.
5. Uygulama için bir ad girin. Örneğin, *webapi1*.
6. İçin **içeren web uygulaması / web API'sini** ve **örtük akışa izin ver**seçin **Evet**.
7. İçin **yanıt URL'si**, Azure AD B2C, uygulamanız tarafından istenen belirteçleri döndürdüğü bir uç noktasını girin. Üretim uygulamanızın yanıt URL'si için bir değer gibi ayarlayabilirsiniz `https://localhost:44332`. Test amacıyla, yanıt URL'sini ayarlamak `https://jwt.ms`.
8. İçin **uygulama kimliği URI'si**, web API'niz için kullanılan tanımlayıcı girin. Tam etki alanı ile birlikte URI tanımlayıcısı sizin için oluşturulur. Örneğin, `https://contosotenant.onmicrosoft.com/api`.
9. **Oluştur**’a tıklayın.
10. Özellikler sayfasında, web uygulamasını yapılandırırken kullanacağınız uygulama Kimliğini kaydedin.

## <a name="configure-scopes"></a>Kapsamlarını yapılandırma

Kapsamlar korumalı kaynaklara erişimi yönetmenin bir yolunu sağlar. Kapsamlar web API’si tarafından kapsam tabanlı erişim denetimi uygulamak için kullanılır. Örneğin web API'sinin kullanıcıları hem okuma hem de yazma veya yalnızca okuma erişimine sahip olabilir. Bu öğreticide web API’si için okuma ve yazma izinlerini tanımlamak için kapsamları kullanacaksınız.

1. Seçin **uygulamaları**ve ardından *webapi1*.
2. Seçin **yayımlanan kapsamlar**.
3. İçin **kapsam**, girin `Read`ve açıklama yazın `Read access to the application`.
4. İçin **kapsam**, girin `Write`ve açıklama yazın `Write access to the application`.
5. **Kaydet**’e tıklayın.

Yayımlanan kapsamlar istemci vermek için kullanılan Web API'sine uygulama izni.

## <a name="grant-permissions"></a>İzinleri verme

Bir uygulamadan korumalı web API'sini çağırmak için API'ye uygulama izinleri vermeniz gerekir. Örneğin, [Öğreticisi: Azure Active Directory B2C'de bir uygulamayı kaydetme](tutorial-register-applications.md), bir web uygulamasını Azure AD adlı B2C'de oluşturulan *webapp1*. Web API'sini çağırmak için bu uygulamayı kullanabilirsiniz.

1. Seçin **uygulamaları**ve ardından web uygulamanızı seçin.
2. Seçin **API erişimi**ve ardından **Ekle**.
3. İçinde **API seçin** açılır menüsünde, select *webapi1*.
4. İçinde **kapsamları seçin** açılır menüsünde, select **okuma** ve **yazma** daha önce tanımladığınız kapsamları.
5. **Tamam** düğmesine tıklayın.

Uygulamanız korumalı web API'sini çağırmak için kaydedilir. Bir kullanıcının uygulamayı kullanmak için Azure AD B2C ile kimliğini doğrular. Uygulama, korumalı web API'sine erişmek için Azure AD B2C bir yetkilendirme izni alır.
