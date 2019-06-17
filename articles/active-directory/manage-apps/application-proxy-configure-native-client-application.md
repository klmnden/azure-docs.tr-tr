---
title: Native client uygulamaları - Azure AD yayımlama | Microsoft Docs
description: Yerel istemci uygulamaların, şirket içi uygulamalara güvenli uzaktan erişim sağlamak için Azure AD uygulama ara sunucusu Bağlayıcısı ile iletişim kurmasını sağlamak nasıl etkinleştireceğinizi de açıklar.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/15/2019
ms.author: mimart
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: cb36d6a03da07681db468184a489a79f7f0deab7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65825498"
---
# <a name="how-to-enable-native-client-applications-to-interact-with-proxy-applications"></a>Proxy uygulamaları ile etkileşim kurmak yerel istemci uygulamaları etkinleştirme

Azure Active Directory (Azure AD) uygulama proxy'si web uygulamaları yayımlamak için kullanabileceğiniz, ancak Azure AD Authentication Library (ADAL) ile yapılandırılmış yerel istemci uygulamaları yayımlamak için de kullanılabilir. Bir tarayıcı erişilen Web uygulamaları sırada bir cihazda yüklü için yerel istemci uygulamaları, web uygulamalarından farklı.

Yerel istemci uygulamaları desteklemek için uygulama proxy'si üstbilgisinde gönderilen Azure AD tarafından verilen belirteçleri kabul eder. Uygulama proxy'si hizmeti kullanıcıların kimlik doğrulaması yapar. Bu çözüm, uygulama belirteçleri için kimlik doğrulaması kullanmaz.

![Son kullanıcılar, Azure Active Directory ve yayımlanan uygulama arasındaki ilişki](./media/application-proxy-configure-native-client-application/richclientflow.png)

Yerel uygulama yayımlamak için Azure AD kimlik doğrulaması kimlik doğrulaması üstlenir ve çok sayıda istemci ortamlarını destekler. kitaplığını kullanın. Uygulama proxy'si uygun içine [yerel uygulaması Web API'si senaryosu](../develop/native-app.md).

Bu makalede uygulama ara sunucusu ve Azure AD kimlik doğrulama kitaplığı ile yerel bir uygulamayı yayımlamak için dört adımlarında size kılavuzluk eder.

## <a name="step-1-publish-your-proxy-application"></a>1\. adım: Proxy uygulamanızı yayımlayın

Diğer uygulamalarda olduğu gibi ara sunucu uygulamasını yayımlayın ve uygulamanıza erişmek için kullanıcı atama. Daha fazla bilgi için [uygulama ara sunucusu ile uygulama yayımlama](application-proxy-add-on-premises-application.md).

## <a name="step-2-register-your-native-application"></a>2\. adım: Yerel uygulamanızı kaydetme

Şimdi uygulamanızı Azure AD'de gibi kaydetmeniz gerekir:

1. Oturum [Azure Active Directory portalında](https://aad.portal.azure.com/). **Pano** için **Azure Active Directory Yönetim Merkezi** görünür.
2. Kenar çubuğunda seçin **Azure Active Directory**. **Azure Active Directory** genel bakış sayfası açılır.
3. Azure AD genel bakış Kenar çubuğunda seçin **uygulama kayıtları**. Tüm uygulama kayıtlarını listesi görüntülenir.
4. Seçin **yeni kayıt**. **Bir uygulamayı kaydetme** sayfası görüntülenir.

   ![Yeni bir uygulama kaydı oluşturma](./media/application-proxy-configure-native-client-application/create.png)
5. İçinde **adı** başlık, uygulamanız için bir kullanıcıya yönelik görünen ad belirtin.
6. Altında **desteklenen hesap türleri** başlığı, bu yönergeleri kullanarak bir erişim düzeyi seçin:
   - Yalnızca kuruluşunuzun iç hesapları hedeflemek için seçin **hesapları yalnızca kuruluş bu dizinde**.
   - Yalnızca iş veya eğitim müşterileri hedeflemek için seçin **herhangi bir kuruluş dizini hesaplarında**.
   - Microsoft kimlik geniş kümesini hedeflemek için seçin **herhangi bir kuruluş dizinini ve kişisel Microsoft hesapları hesaplarında**.
7. İçinde **yeniden yönlendirme URI'si** başlığı seçin **genel istemci (Mobil ve Masaüstü)** , uygulamanızın yeniden yönlendirme URI'sini yazın.
8. Seçin ve okuma **Microsoft Platformu ilkeleri**ve ardından **kaydetme**. Yeni uygulama kaydı için bir genel bakış sayfası oluşturulur ve görüntülenir.

Yeni bir uygulama kaydı oluşturma hakkında daha ayrıntılı bilgi için bkz: [uygulamaları Azure Active Directory ile tümleştirme](../develop/quickstart-v1-integrate-apps-with-azure-ad.md).

## <a name="step-3-grant-access-to-your-proxy-application"></a>3\. adım: Proxy uygulamanıza erişim izni ver

Yerel uygulamanız kaydettiğinize göre, bu erişim başka uygulamalar için dizininizde bu durumda proxy uygulamaya erişmek için verebilirsiniz. Yerel bir uygulama için Ara sunucu uygulamasını sağlamak etkinleştirmek için:

1. Yeni uygulama kaydı sayfası Kenar çubuğunda seçin **API izinleri**. **API izinleri** sayfası yeni uygulama kaydı görünür.
2. Seçin **bir izin eklemek**. **İstek API izinleri** sayfası görüntülenir.
3. Altında **bir API seçin** ayarını seçin **Kuruluşum kullandığı API'leri**. Dizininizdeki API'leri kullanıma uygulamaları içeren bir liste görüntülenir.
4. Yayımlanmış, Ara sunucu uygulamasını bulmak için arama kutusuna veya kaydırma türü [1. adım: Proxy uygulamanızı yayımlayın](#step-1-publish-your-proxy-application)ve ardından Ara sunucu uygulamasını seçin.
5. İçinde **ne tür izinler uygulamanızı gerektiriyor mu?** başlığı izin türü seçin. Yerel uygulamanız oturum açmış kullanıcı olarak proxy uygulama API erişmesi gerekiyorsa seçin **temsilci izinleri**. Yerel uygulamanız, bir arka plan hizmet veya yordam olmadan oturum açmış kullanıcı olarak çalışıyorsa, seçin **uygulama izinleri**.
6. İçinde **izinleri seçin** başlık, istenen izin seçip **izinleri eklemek**. **API izinleri** sayfası yerel uygulamanız artık proxy eklediğiniz uygulama ve izni API gösterir.

## <a name="step-4-edit-the-active-directory-authentication-library"></a>4\. Adım: Active Directory kimlik doğrulama Kitaplığı'nı Düzenle

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

Örnek kodda gerekli bilgileri gibi Azure AD portalında bulunabilir:

| Gerekli bilgileri | Azure AD portalında bulma |
| --- | --- |
| \<Kiracı kimliği > | **Azure Active Directory** > **özellikleri** > **dizin kimliği** |
| \<Ara sunucu uygulamasının harici URL'si > | **Kurumsal uygulamalar** > *proxy uygulamanızı* > **uygulama proxy'si** > **dış Url** |
| \<Yerel uygulamasının uygulama kimliği > | **Kurumsal uygulamalar** > *yerel uygulamanız* > **özellikleri** > **uygulama kimliği** |
| \<Yeniden yönlendirme URI'si yerel uygulamanın > | **Azure Active Directory** > **uygulama kayıtları** > *yerel uygulamanız* > **yeniden yönlendirme URI'leri** |
| \<Proxy Uygulama API URL'si > | **Azure Active Directory** > **uygulama kayıtları** > *yerel uygulamanız* > **API izinleri**  >  **API / izinleri ad** |

Bu parametreleri ile ADAL düzenledikten sonra kullanıcılarınızın bir kuruluş ağının dışında olduklarında bile yerel istemci uygulamaları için kimlik doğrulaması yapabilir.

## <a name="next-steps"></a>Sonraki adımlar

Yerel uygulama akışı hakkında daha fazla bilgi için bkz. [yerel uygulamaları Azure Active Directory'de](../develop/native-app.md).

Ayarlama hakkında bilgi edinin [Azure Active Directory'de uygulamalar için çoklu oturum açma](what-is-single-sign-on.md#choosing-a-single-sign-on-method).
