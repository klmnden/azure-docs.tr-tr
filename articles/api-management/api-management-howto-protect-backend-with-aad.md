---
title: Azure Active Directory ve API Management ile OAuth 2.0 kullanarak bir API koruma | Microsoft Docs
description: Web API'si arka uç Azure Active Directory ve API Management ile korumak öğrenin.
services: api-management
documentationcenter: ''
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2018
ms.author: apimpm
ms.openlocfilehash: f5662a4082487137dfd642cc3264a90f8ab19054
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="protect-an-api-by-using-oauth-20-with-azure-active-directory-and-api-management"></a>Azure Active Directory ve API Management ile OAuth 2.0 kullanarak bir API koruyun

Bu kılavuz size Azure Active Directory (Azure AD) ile OAuth 2.0 protokolünü kullanarak bir API korumak için Azure API Management Örneğinize yapılandırma gösterir. 

## <a name="prerequisites"></a>Önkoşullar
Bu makaledeki adımları tamamlayabilmeniz için şunlara sahip olmalısınız:
* API Management örneği
* API Management örneği kullanan yayımlanmakta API
* Azure AD kiracısı

## <a name="overview"></a>Genel Bakış

Hızlı bir genel bakış adımları şöyledir:

1. Bir uygulamayı (uygulama arka uç) API temsil etmek için Azure AD'de kaydedin.
2. Başka bir uygulama (istemci-app) API'si çağırmayı gerektiren bir istemci uygulaması temsil etmek için Azure AD'de kaydedin.
3. Azure AD'de arka uç uygulamasını çağırmak istemci uygulaması izin vermek için izinleri verin.
4. Geliştirici Konsolu OAuth 2.0 kullanıcı kimlik doğrulaması kullanacak şekilde yapılandırın.
5. Ekleme **doğrulamak jwt** her gelen istek için OAuth belirteci doğrulamak için ilke.

## <a name="register-an-application-in-azure-ad-to-represent-the-api"></a>API temsil etmek için Azure AD içinde bir uygulamayı kaydetme

Azure AD ile bir API korumak için ilk API temsil eden Azure AD'de uygulama kaydetmek için adımdır. 

1. Azure AD kiracınıza göz atın ve sonra gidin **uygulama kayıtlar**.

2. **Yeni uygulama kaydı**’nı seçin. 

3. Uygulamanın adını sağlayın. (Bu örnekte, ad, `backend-app`.)  

4. Seçin **Web uygulaması / API** olarak **uygulama türü**. 

5. İçin **oturum açma URL'si**, kullanabileceğiniz `https://localhost` yer tutucu olarak.

6. **Oluştur**’u seçin.

Uygulama oluşturulduğunda, Not **uygulama kimliği**, bir sonraki adımda kullanmak için. 

## <a name="register-another-application-in-azure-ad-to-represent-a-client-application"></a>Bir istemci uygulaması temsil etmek için Azure AD içinde başka bir uygulamayı kaydetme

API çağrıları her istemci uygulaması, bir uygulama Azure AD olarak kayıtlı olması gerekir. Bu örnekte, örnek istemci API Management Geliştirici Portalı Geliştirici konsolunda uygulamasıdır. Aşağıda, geliştirici konsol temsil etmek için Azure AD içinde başka bir uygulamanın nasıl verilmiştir.

1. **Yeni uygulama kaydı**’nı seçin. 

2. Uygulamanın adını sağlayın. (Bu örnekte, ad, `client-app`.)

3. Seçin **Web uygulaması / API** olarak **uygulama türü**.  

4. İçin **oturum açma URL'si**, kullanabileceğiniz `https://localhost` bir yer tutucu veya kullanım oturum açma URL'si, API Management örneğinin olarak. (Bu örnekte URL'nin, `https://contoso5.portal.azure-api.net/signin`.)

5. **Oluştur**’u seçin.

Uygulama oluşturulduğunda, Not **uygulama kimliği**, bir sonraki adımda kullanmak için. 

Şimdi, bir sonraki adımda kullanmak için bu uygulama için bir istemci parolası oluşturun.

1. Seçin **ayarları** yeniden ve Git **anahtarları**.

2. Altında **parolaları**, sağlayan bir **anahtar açıklama**. Ne zaman anahtar sona seçin ve seçim **kaydetmek**.

Anahtar değerini not edin. 

## <a name="grant-permissions-in-azure-ad"></a>Azure AD'de izinleri

API ve geliştirici konsol göstermek üzere iki uygulama kaydettiğiniz, arka uç uygulamasını çağırmak istemci uygulaması izin vermek için izinleri vermeniz gerekir.  

1. Gözat **uygulama kayıtlar**. 

2. Seçin `client-app`ve Git **ayarları**.

3. Seçin **gerekli izinleri** > **Ekle**.

4. Seçin **bir API seçin**, arayın ve `backend-app`.

5. Altında **izinlere temsilci**seçin `Access backend-app`. 

6. Seçin **seçin**ve ardından **Bitti**. 

> [!NOTE]
> Varsa **Azure Active Directory** select diğer uygulamalara izinler altında listelenmeyen **Ekle** listeden eklemek için.
> 
> 

## <a name="enable-oauth-20-user-authorization-in-the-developer-console"></a>OAuth 2.0 kullanıcı yetkilendirme Geliştirici konsolunda etkinleştir

Bu noktada, uygulamalarınızın Azure AD'de oluşturdunuz ve arka uç uygulamasını çağırmak istemci uygulaması izin vermek için uygun izinlerin. 

Bu örnekte, istemci uygulaması Geliştirici konsoludur. Aşağıdaki adımlar nasıl OAuth 2.0 kullanıcı yetkilendirme Geliştirici konsolunda etkinleştirileceğini açıklar. 

1. API Management örneğine göz atın.

2. Seçin **OAuth 2.0** > **eklemek**.

3. Sağlayan bir **görünen adı** ve **açıklama**.

4. İçin **istemci kayıt sayfası URL'si**, bir yer tutucu değerini girin `http://localhost`. **İstemci kayıt sayfası URL'si** kullanıcılar oluşturmak ve bunu desteklemek için OAuth 2.0 sağlayıcıları kendi hesaplarını yapılandırmak için kullanabileceğiniz sayfa işaret eder. Bu örnekte, kullanıcılar değil oluşturabilir ve bunun yerine bir yer tutucu kullanmanız için kendi hesaplarını yapılandırın.

5. İçin **yetki türleri**seçin **yetkilendirme kodu**.

6. Belirtin **yetkilendirme uç noktası URL'si** ve **belirteç uç nokta URL'si**. Bu değerleri almak **uç noktaları** Azure AD kiracınıza sayfasında. Gözat **uygulama kayıtlar** yeniden sayfasında ve seçin **uç noktaları**.

7. Kopya **OAuth 2.0 yetkilendirme uç noktası**ve yapıştırın **yetkilendirme uç noktası URL'si** metin kutusu.

8. Kopya **OAuth 2.0 belirteç uç noktası**ve yapıştırın **belirteç uç nokta URL'si** metin kutusu. Belirteç uç noktası yapıştırarak yanı sıra, adlı bir gövde parametre Ekle **kaynak**. Bu parametre değeri için kullanmak **uygulama kimliği** arka uç uygulaması için.

9. Ardından, istemci kimlik bilgilerini belirtin. Bu, istemci uygulaması için kimlik bilgileridir.

10. İçin **istemci kimliği**, kullanın **uygulama kimliği** istemci uygulaması için.

11. İçin **gizli**, daha önce oluşturduğunuz anahtarı için istemci uygulaması kullanın. 

12. İstemci gizli anahtarı aşağıdaki hemen **redirect_url** için yetkilendirme kodu verme türü. Bu URL not edin.

13. **Oluştur**’u seçin.

14. Geri dönerek **ayarları** , istemci uygulamanızın sayfası.

15. Seçin **yanıt URL'leri**, Yapıştır **redirect_url** ilk satırda. Bu örnekte, değiştirdiğiniz `https://localhost` ilk satırda URL'si.  

Yapılandırılmış bir OAuth 2.0 yetkilendirme sunucusu, geliştirici konsol Azure AD'den erişim belirteçleri elde edebilirsiniz. 

Sonraki adım, OAuth 2.0 kullanıcı yetkilendirme için API'nize sağlamaktır. Bu, API çağrıları yapmadan önce kullanıcı adına bir erişim belirteci almak gerektiğini öğrenmek Geliştirici konsol sağlar.

1. API Management örneğine göz atın ve Git **API'leri**.

2. Korumak istediğiniz API seçin. Bu örnekte, kullandığınız `Echo API`.

3. Git **ayarları**.

4. Altında **güvenlik**, seçin **OAuth 2.0**ve daha önce yapılandırdığınız OAuth 2.0 sunucuyu seçin. 

5. **Kaydet**’i seçin.

## <a name="successfully-call-the-api-from-the-developer-portal"></a>Geliştirici Portalı'ndan başarıyla API çağrısı

OAuth 2.0 kullanıcı yetkilendirme etkin göre `Echo API`, geliştirici konsolunda bir API'yi çağırmadan önce kullanıcı adına bir erişim belirteci alır.

1. Herhangi bir işlem altında Gözat `Echo API` Geliştirici Portalı ve select içinde **deneyin**. Bu Geliştirici konsoluna getirir.

2. Yeni bir öğe Not **yetkilendirme** yeni eklenen yetkilendirme sunucusu karşılık gelen bölüm.

3. Seçin **yetkilendirme kodu** onayı için Azure AD kiracısı oturum açmanız istenir aşağı açılan liste ve. Hesapla oturum açtıysanız, istenebilir değil.

4. Başarılı oturum açma sonra bir `Authorization` üstbilgisi, Azure AD'den bir erişim belirteci ile isteği eklenir. Bir örnek simge (Base64 ile kodlanmış) aşağıdadır:

   ```
   Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCIsImtpZCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCJ9.eyJhdWQiOiIxYzg2ZWVmNC1jMjZkLTRiNGUtODEzNy0wYjBiZTEyM2NhMGMiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC80NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgvIiwiaWF0IjoxNTIxMTUyNjMzLCJuYmYiOjE1MjExNTI2MzMsImV4cCI6MTUyMTE1NjUzMywiYWNyIjoiMSIsImFpbyI6IkFWUUFxLzhHQUFBQUptVzkzTFd6dVArcGF4ZzJPeGE1cGp2V1NXV1ZSVnd1ZXZ5QU5yMlNkc0tkQmFWNnNjcHZsbUpmT1dDOThscUJJMDhXdlB6cDdlenpJdzJLai9MdWdXWWdydHhkM1lmaDlYSGpXeFVaWk9JPSIsImFtciI6WyJyc2EiXSwiYXBwaWQiOiJhYTY5ODM1OC0yMWEzLTRhYTQtYjI3OC1mMzI2NTMzMDUzZTkiLCJhcHBpZGFjciI6IjEiLCJlbWFpbCI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsImZhbWlseV9uYW1lIjoiSmlhbmciLCJnaXZlbl9uYW1lIjoiTWlhbyIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJpcGFkZHIiOiIxMzEuMTA3LjE3NC4xNDAiLCJuYW1lIjoiTWlhbyBKaWFuZyIsIm9pZCI6IjhiMTU4ZDEwLWVmZGItNDUxMS1iOTQzLTczOWZkYjMxNzAyZSIsInNjcCI6InVzZXJfaW1wZXJzb25hdGlvbiIsInN1YiI6IkFGaWtvWFk1TEV1LTNkbk1pa3Z3MUJzQUx4SGIybV9IaVJjaHVfSEM1aGciLCJ0aWQiOiI0NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgiLCJ1bmlxdWVfbmFtZSI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsInV0aSI6ImFQaTJxOVZ6ODBXdHNsYjRBMzBCQUEiLCJ2ZXIiOiIxLjAifQ.agGfaegYRnGj6DM_-N_eYulnQdXHhrsus45QDuApirETDR2P2aMRxRioOCR2YVwn8pmpQ1LoAhddcYMWisrw_qhaQr0AYsDPWRtJ6x0hDk5teUgbix3gazb7F-TVcC1gXpc9y7j77Ujxcq9z0r5lF65Y9bpNSefn9Te6GZYG7BgKEixqC4W6LqjtcjuOuW-ouy6LSSox71Fj4Ni3zkGfxX1T_jiOvQTd6BBltSrShDm0bTMefoyX8oqfMEA2ziKjwvBFrOjO0uK4rJLgLYH4qvkR0bdF9etdstqKMo5gecarWHNzWi_tghQu9aE3Z3EZdYNI_ZGM-Bbe3pkCfvEOyA
   ```

5. Seçin **Gönder**, API başarıyla çağırabilirsiniz.


## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>İstekleri önceden yetkilendirmek için JWT doğrulama ilkesini yapılandırma

Geliştirici konsolundan arama yapmak bir kullanıcı çalıştığında, bu noktada, kullanıcı oturum istenir. Geliştirici konsol kullanıcı adına bir erişim belirteci alır.

Ancak ne birisi bir belirteç olmadan veya geçersiz bir belirteç, API çağrılarının? Örneğin, silerseniz bile API hala çağırabilirsiniz `Authorization` üstbilgi. API yönetim erişim belirteci bu noktada doğrulamaz, nedenidir. Yalnızca geçirir `Authorization` üstbilgi arka uç API'si.

Kullanabileceğiniz [doğrulamak JWT](api-management-access-restriction-policies.md#ValidateJWT) API Management isteklerini her gelen istek erişim belirteçleri doğrulayarak önceden yetkilendirmek için ilke. Bir isteğin geçerli bir belirteci yoksa, API Management bunu engeller. Örneğin, aşağıdaki ilkeye ekleyebilirsiniz `<inbound>` ilke bölümünü `Echo API`. Bir erişim belirteci İzleyici talepte denetler ve belirtecin geçerli değilse, bir hata iletisi döndürür. İlkeleri yapılandırma hakkında daha fazla bilgi için bkz: [ayarlayın veya ilkeleri düzenleme](set-edit-policies.md).

```xml
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
    <openid-config url="https://login.microsoftonline.com/{aad-tenant}/.well-known/openid-configuration" />
    <required-claims>
        <claim name="aud">
            <value>{Application ID of backend-app}</value>
        </claim>
    </required-claims>
</validate-jwt>
```

## <a name="build-an-application-to-call-the-api"></a>API'yi çağırmak için bir uygulama oluşturma

Bu kılavuzda, API Management Geliştirici konsolunda örnek istemci uygulaması olarak çağırmak için kullanılan `Echo API` OAuth 2.0 tarafından korumalı. Bir uygulama oluşturmak ve OAuth 2.0 uygulamak hakkında daha fazla bilgi için bkz: [Azure Active Directory kod örnekleri](../active-directory/develop/active-directory-code-samples.md).

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinmek [Azure Active Directory ve OAuth2.0](../active-directory/develop/active-directory-authentication-scenarios.md).
* Daha fazla bilgi denetleyin [videolar](https://azure.microsoft.com/documentation/videos/index/?services=api-management) API Management hakkında.
* Arka uç hizmetinizin güvenliğini sağlamak diğer yolları için bkz: [karşılıklı sertifika kimlik doğrulaması](api-management-howto-mutual-certificates.md).

* [Bir API Management hizmet örneği oluşturma](get-started-create-service-instance.md).

* [İlk API'nizi yönetme](import-and-publish.md).
