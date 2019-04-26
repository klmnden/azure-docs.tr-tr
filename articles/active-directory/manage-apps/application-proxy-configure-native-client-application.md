---
title: Native client uygulamaları - Azure AD yayımlama | Microsoft Docs
description: Yerel istemci uygulamaların, şirket içi uygulamalara güvenli uzaktan erişim sağlamak için Azure AD uygulama ara sunucusu Bağlayıcısı ile iletişim kurmasını sağlamak nasıl etkinleştireceğinizi de açıklar.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/08/2018
ms.author: celested
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 21e101dee878a48cce1005d51ad5e59166b0cfa1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60293328"
---
# <a name="how-to-enable-native-client-apps-to-interact-with-proxy-applications"></a>Yerel istemci uygulama proxy uygulamaları ile etkileşim kurmak etkinleştirme

Web uygulamalarının yanı sıra Azure Active Directory Uygulama proxy'si de, Azure AD Authentication Library (ADAL) ile yapılandırılan yerel istemci uygulamaları yayımlamak için kullanılabilir. Web apps, bir tarayıcı erişilen ancak bunlar bir cihazda yüklü olmadığından native client uygulamaları web uygulamalarından farklı. 

Uygulama proxy'si, verilen belirteçler üstbilgisinde gönderilen kabul Azure AD tarafından native client uygulamaları destekler. Uygulama proxy'si hizmeti kullanıcılar adına kimlik doğrulaması gerçekleştirir. Bu çözüm, uygulama belirteçleri için kimlik doğrulaması kullanmaz. 

![Son kullanıcılar, Azure Active Directory ve yayımlanan uygulama arasındaki ilişki](./media/application-proxy-configure-native-client-application/richclientflow.png)

Yerel uygulamalar yayımlamak için Azure AD kimlik doğrulaması kimlik doğrulaması üstlenir ve çok sayıda istemci ortamlarını destekler, kitaplığını kullanın. Uygulama proxy'si uygun içine [yerel uygulaması Web API'si senaryosu](../develop/native-app.md). 

Bu makalede uygulama ara sunucusu ve Azure AD kimlik doğrulama kitaplığı ile yerel bir uygulamayı yayımlamak için dört adımlarında size kılavuzluk eder. 

## <a name="step-1-publish-your-application"></a>1. Adım: Uygulamanızı yayımlama
Diğer uygulamalarda olduğu gibi ara sunucu uygulamasını yayımlayın ve uygulamanıza erişmek için kullanıcı atama. Daha fazla bilgi için [uygulama ara sunucusu ile uygulama yayımlama](application-proxy-add-on-premises-application.md).

## <a name="step-2-configure-your-application"></a>2. Adım: Uygulamanızı yapılandırma
Yerel uygulamanız aşağıdaki gibi yapılandırın:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Gidin **Azure Active Directory** > **uygulama kayıtları**.
3. **Yeni uygulama kaydı**’nı seçin.
4. Uygulamanız, select için bir ad belirtin **yerel** uygulama türü ve uygulamanızın yeniden yönlendirme URI'sini belirtin. 

   ![Yeni bir uygulama kaydı oluşturma](./media/application-proxy-configure-native-client-application/create.png)
5. **Oluştur**’u seçin.

Yeni bir uygulama kaydı oluşturma hakkında daha ayrıntılı bilgi için bkz: [uygulamaları Azure Active Directory ile tümleştirme](../develop/quickstart-v1-integrate-apps-with-azure-ad.md).


## <a name="step-3-grant-access-to-other-applications"></a>3. Adım: Diğer uygulamalara yönelik erişim izni ver
Yerel uygulamanın diğer uygulamalara dizininizdeki açığa etkinleştir:

1. Hala **uygulama kayıtları**, az önce oluşturduğunuz yeni yerel uygulamayı seçin.
2. Seçin **API izinleri**.
3. Seçin **bir izin eklemek**.
4. İlk adım, açık **bir API seçin**.
5. Birinci bölümde yayımlanan uygulama proxy'si uygulamasını bulmak için arama çubuğunu kullanın. Bu uygulamayı seçin, ardından tıklatın **seçin**. 

   ![Proxy uygulama arayın](./media/application-proxy-configure-native-client-application/select_api.png)
6. İkinci adım açın **izinleri seçin**.
7. Yerel uygulama erişim proxy uygulamanıza'a tıklayın, onay kutusunu kullanın **seçin**.

   ![Proxy uygulamasına erişim izni ver](./media/application-proxy-configure-native-client-application/select_perms.png)
8. **Done** (Bitti) öğesini seçin.


## <a name="step-4-edit-the-active-directory-authentication-library"></a>4. Adım: Active Directory kimlik doğrulama Kitaplığı'nı Düzenle
Kimlik doğrulaması bağlamı, Active Directory Authentication Library (aşağıdaki metni eklemek için ADAL) yerel uygulama kodunda düzenleyin:

```
// Acquire Access Token from AAD for Proxy Application
AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<Tenant ID>");
AuthenticationResult result = await authContext.AcquireTokenAsync("< External Url of Proxy App >",
        "<App ID of the Native app>",
        new Uri("<Redirect Uri of the Native App>"),
        PromptBehavior.Never);

//Use the Access Token to access the Proxy Application
HttpClient httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");
```

Örnek kodda değişkenleri şu şekilde değiştirilmelidir:

* **Kiracı kimliği** Azure portalında bulunabilir. Gidin **Azure Active Directory** > **özellikleri** ve dizin kimliği kopyalayın 
* **Dış URL** Proxy Uygulama girdiğiniz ön uç URL'dir. Bu değeri bulmak için gidin **uygulama proxy'si** proxy uygulama bölümü.
* **Uygulama Kimliği** yerel uygulamanın bulunabilir **özellikleri** yerel uygulama sayfası.
* **Yeniden yönlendirme URI'si yerel uygulamanın** bulunabilir **yeniden yönlendirme URI'leri** yerel uygulama sayfası.

ADAL bu parametrelerle düzenlendikten sonra kullanıcılar kuruluş ağının dışında olduklarında bile yerel istemci uygulamaları için kimlik doğrulaması için olmalıdır. 

## <a name="next-steps"></a>Sonraki adımlar

Yerel uygulama akışı hakkında daha fazla bilgi için bkz: [web API'si için yerel uygulama](../develop/native-app.md)

Ayarlama hakkında bilgi edinin [çoklu oturum açma için uygulama proxy'si](what-is-single-sign-on.md#single- sign-on-methods)
