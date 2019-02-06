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
ms.component: B2C
ms.openlocfilehash: c20f455a0a325dadd3eeeb77dea7026de4834c56
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2019
ms.locfileid: "55757701"
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
3. İçin **kapsam**, girin `Hello.Read`ve açıklama yazın `Read access to hello`.
4. İçin **kapsam**, girin `Hello.Write`ve açıklama yazın `Write access to hello`.
5. **Kaydet**’e tıklayın.

Yayımlanan kapsamlar istemci vermek için kullanılan Web API'sine uygulama izni.

## <a name="grant-permissions"></a>İzinleri verme

Bir uygulamadan korumalı web API'sini çağırmak için API'ye uygulama izinleri vermeniz gerekir. Önkoşul öğreticisinde, Azure AD B2C'adlı bir web uygulamasında oluşturulan *webapp1*. Web API'sini çağırmak için bu uygulamayı kullanın.

1. Seçin **uygulamaları**ve ardından web uygulamanızı seçin.
2. Seçin **API erişimi**ve ardından **Ekle**.
3. İçinde **API seçin** açılır menüsünde, select *webapi1*.
4. İçinde **kapsamları seçin** açılır menüsünde, select **Hello.Read** ve **Hello.Write** daha önce tanımladığınız kapsamları.
5. **Tamam** düğmesine tıklayın.

Uygulamanız korumalı web API'sini çağırmak için kaydedilir. Bir kullanıcının uygulamayı kullanmak için Azure AD B2C ile kimliğini doğrular. Uygulama, korumalı web API'sine erişmek için Azure AD B2C bir yetkilendirme izni alır.