---
title: Yerel istemci uygulamaları - Azure AD yayımlama | Microsoft Docs
description: Yerel istemci uygulamaların, şirket içi uygulamalara güvenli uzaktan erişim sağlamak için Azure AD uygulama ara sunucusu Bağlayıcısı ile iletişim sağlamak nasıl ele alınmaktadır.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 5c231ce09add63c6e46dee0c76bbe64c438ff820
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="how-to-enable-native-client-apps-to-interact-with-proxy-applications"></a>Proxy uygulamaları ile etkileşim kurmak yerel istemci uygulamaları etkinleştirme

Web uygulamalarının yanı sıra, Azure Active Directory Uygulama proxy'si, Azure AD Authentication Library (ADAL) ile yapılandırılan yerel istemci uygulamalar yayımlamak için de kullanılabilir. Web uygulamaları bir tarayıcı erişilen sırada bir aygıtta yüklü olmadığından yerel istemci uygulamaları, web uygulamaları farklılık gösterir. 

Uygulama proxy'si yerel istemci uygulamalar tarafından verilen belirteçler üstbilgisinde gönderilen kabul Azure AD destekler. Uygulama proxy'si hizmeti kullanıcılar adına kimlik doğrulaması gerçekleştirir. Bu çözüm, kimlik doğrulaması için uygulama belirteçleri kullanmaz. 

![Son kullanıcılar, Azure Active Directory ve yayımlanmış uygulamalar arasındaki ilişki](./media/application-proxy-configure-native-client-application/richclientflow.png)

Yerel uygulamalar yayımlamak için Azure AD kimlik doğrulama mvc'deki kimlik doğrulama ve birçok istemci ortamını destekler, kitaplığını kullanın. Uygulama proxy'si uygun içine [Web API'si senaryosu için yerel uygulama](../develop/active-directory-authentication-scenarios.md#native-application-to-web-api). 

Bu makalede, uygulama proxy'si ve Azure AD kimlik doğrulama kitaplığı ile yerel bir uygulamayı yayımlamak için dört adım adım anlatılmaktadır. 

## <a name="step-1-publish-your-application"></a>1. adım: uygulamanızı yayımlama
Başka bir uygulama gibi proxy uygulamanızı yayımlamak ve uygulamanızı erişmek için kullanıcıları atayın. Daha fazla bilgi için bkz: [uygulama proxy'si ile uygulama yayımlama](application-proxy-publish-azure-portal.md).

## <a name="step-2-configure-your-application"></a>2. adım: uygulamanızı yapılandırma
Yerel uygulamanız aşağıdaki gibi yapılandırın:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Gidin **Azure Active Directory** > **uygulama kayıtlar**.
3. **Yeni uygulama kaydı**’nı seçin.
4. Select, uygulamanız için bir ad belirtin **yerel** uygulama türü olarak ve uygulamanız için yeniden yönlendirme URI'sini belirtin. 

   ![Yeni bir uygulama kaydı oluşturma](./media/application-proxy-configure-native-client-application/create.png)
5. **Oluştur**’u seçin.

Yeni bir uygulama kaydı oluşturma hakkında daha ayrıntılı bilgi için bkz: [uygulamaları Azure Active Directory ile tümleştirme](./../develop/active-directory-integrating-applications.md).


## <a name="step-3-grant-access-to-other-applications"></a>3. adım: Diğer uygulamalara erişim izni ver
Diğer uygulamalara dizininizde açığa çıkarılması yerel uygulama etkinleştirin:

1. Hala **uygulama kayıtlar**, az önce oluşturduğunuz yeni yerel uygulaması'nı seçin.
2. Seçin **gerekli izinleri**.
3. **Add (Ekle)** seçeneğini belirleyin.
4. İlk adım, açık **bir API seçin**.
5. İlk bölümde yayımlanan uygulama proxy'si uygulama bulmak için arama çubuğunu kullanın. Bu uygulama seçin ve ardından **seçin**. 

   ![Proxy uygulamayı arayın](./media/application-proxy-configure-native-client-application/select_api.png)
6. İkinci adım açmak **seçin izinleri**.
7. Yerel uygulama erişim proxy uygulamanıza ve ardından için onay kutusunu kullanın **seçin**.

   ![Proxy uygulamaya erişim izni ver](./media/application-proxy-configure-native-client-application/select_perms.png)
8. Seçin **Bitti**.


## <a name="step-4-edit-the-active-directory-authentication-library"></a>4. adım: Active Directory kimlik doğrulama kitaplığı Düzenle
Kimlik doğrulaması bağlamı, Active Directory kimlik doğrulama kitaplığı (aşağıdaki metni dahil etmek için ADAL) yerel uygulama kodunda düzenleyin:

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

* **Kiracı kimliği** Azure Portalı'nda bulunabilir. Gidin **Azure Active Directory** > **özellikleri** ve dizin kimliği kopyalayın 
* **Dış URL** Proxy uygulamada girdiğiniz ön uç URL'si. Bu değeri bulmak için gidin **uygulama proxy'si** proxy uygulama bölümü.
* **Uygulama Kimliği** yerel uygulama bulunabilir **özellikleri** yerel uygulama sayfası.
* **Yerel uygulamasının URI'sini yeniden yönlendir** bulunabilir **yeniden yönlendirme URI'ler** yerel uygulama sayfası.

ADAL bu parametrelerle düzenlendikten sonra kullanıcılarınızın bile şirket ağının dışından olduğunuzda yerel istemci uygulamaları için kimlik doğrulaması için olmalıdır. 

## <a name="next-steps"></a>Sonraki adımlar

Yerel uygulama akışı hakkında daha fazla bilgi için bkz: [Web API'ye yerel uygulama](../develop/active-directory-authentication-scenarios.md#native-application-to-web-api)

Şunları ayarlama hakkında bilgi edinin [çoklu oturum açma için uygulama proxy'si](application-proxy-single-sign-on.md)
