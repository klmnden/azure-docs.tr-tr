---
title: Bir web uygulaması - Azure Active Directory B2C ekleme | Microsoft Docs
description: Bir Active Directory B2C kiracınıza web uygulamasına eklemeyi öğrenin.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.author: davidmu
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: conceptual
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: cae9d51bbe1d67734e9c2163140ec3b969122bc2
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2019
ms.locfileid: "56671497"
---
# <a name="add-a-web-api-application-to-your-azure-active-directory-b2c-tenant"></a>Bir web API uygulaması Azure Active Directory B2C kiracınıza ekleyin

Web API'si kaynaklarına kabul edebilir ve korunan kaynak isteklerini yanıtlamak önce kiracınızda bir erişim belirteci mevcut istemci uygulamalar tarafından kayıtlı olması gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
4. Seçin **uygulamaları**ve ardından **Ekle**.
5. Uygulama için bir ad girin. Örneğin, *webapi1*.
6. İçin **içeren web uygulaması / web API'sini** ve **örtük akışa izin ver**seçin **Evet**.
7. İçin **yanıt URL'si**, Azure AD B2C, uygulamanız tarafından istenen belirteçleri döndürdüğü bir uç noktasını girin. Bu öğreticide örnek yerel olarak çalışır ve adresindeki dinler `https://localhost:44332`.
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
