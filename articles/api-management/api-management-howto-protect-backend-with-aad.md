---
title: Azure Active Directory ve API Management ile OAuth 2.0 kullanan bir API koruma | Microsoft Docs
description: Bir Web API arka ucu Azure Active Directory ve API Management ile korumak öğrenin.
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
ms.openlocfilehash: 2c05407d761a8848f9e032aa219960cd7ea6fa93
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="how-to-protect-an-api-using-oauth-20-with-azure-active-directory-and-api-management"></a>Azure Active Directory ve API Management ile OAuth 2.0 kullanan bir API korumak nasıl

Bu kılavuz, API Management (APIM) örneğinizin Azure Active Directory'ye (AAD) OAuth 2.0 protokolünü kullanarak bir API korumak için yapılandırma gösterir. 

## <a name="prerequisite"></a>Önkoşul
Bu makaledeki adımları tamamlayabilmeniz için şunlara sahip olmalısınız:
* APIM örneği
* Bir API APIM örneğini kullanarak yayımlanmakta
* Azure AD kiracısı

## <a name="overview"></a>Genel Bakış

Bu kılavuz bir API APIM OAuth 2.0 ile korumak nasıl gösterir. Bu makalede, yetkilendirme sunucusu (OAuth) olarak Azure AD ile kullanılır. Hızlı bir genel adımlar aşağıdadır:

1. Bir uygulamayı (uygulama arka uç) API temsil etmek için Azure AD'de kaydetme
2. Başka bir uygulama (istemci-app) API'si çağırmayı gerektiren bir istemci uygulaması temsil etmek için Azure AD'de kaydetme
3. Azure AD'de, arka uç uygulamasını çağırmak istemci-app izin vermek için izinleri
4. Geliştirici konsolunu OAuth 2.0 kullanıcı kimlik doğrulaması kullanmak için yapılandırma
5. Her gelen istek için OAuth belirteci doğrulamak için Doğrula jwt ilke Ekle

## <a name="register-an-application-in-azure-ad-to-represent-the-api"></a>API temsil etmek için Azure AD içinde bir uygulamayı kaydetme

Azure AD ile bir API korumak için ilk API temsil eden Azure AD'de uygulama kaydetmek için adımdır. 

Azure AD kiracınıza gidin ve ardından gidin **uygulama kayıtlar**.

**Yeni uygulama kaydı**’nı seçin. 

Uygulamanın adını sağlayın. Bu örnekte, `backend-app` kullanılır.  

Seçin **Web uygulaması / API** olarak **uygulama türü**. 

İçin **oturum açma URL'si**, kullanabileceğiniz `https://localhost` yer tutucu olarak.

Tıklayın **oluşturma**.

Uygulama oluşturulduktan sonra Not **uygulama kimliği** bir sonraki adımda kullanmak için. 

## <a name="register-another-application-in-azure-ad-to-represent-a-client-application"></a>Bir istemci uygulaması temsil etmek için Azure AD içinde başka bir uygulamayı kaydetme

API çağırmayı gerektiren her istemci uygulaması, bir uygulama Azure AD olarak kayıtlı olması gerekir. Bu kılavuzda, örnek istemci uygulaması gibi APIM Geliştirici Portalı Geliştirici konsolunda kullanacağız. 

Biz, geliştirici konsol temsil etmek için Azure AD içinde başka bir uygulama kaydetmeniz gerekir.

Tıklayın **yeni uygulama kaydı** yeniden. 

Uygulamanın adını sağlayın ve seçin **Web uygulaması / API** olarak **uygulama türü**. Bu örnekte, `client-app` kullanılır.  

İçin **oturum açma URL'si**, kullanabileceğiniz `https://localhost` bir yer tutucu ya da kullanım APIM örneğinizi oturum açma URL'si olarak. Bu örnekte, `https://contoso5.portal.azure-api.net/signin` kullanılır.

Tıklayın **oluşturma**.

Uygulama oluşturulduktan sonra Not **uygulama kimliği** bir sonraki adımda kullanmak için. 

Şimdi sizi bir sonraki adımda kullanmak için bu uygulama için bir istemci parolası oluşturmanız gerekir.

Tıklayın **ayarları** yeniden ve Git **anahtarları**.

Altında **parolaları**, sağlayan bir **anahtar açıklama**, zaman anahtar süresi dolacak ve tıklayın seçim **kaydetmek**.

Anahtar değerini not edin. 

## <a name="grant-permissions-in-aad"></a>Aad'de izinleri

Şimdi biz (diğer bir deyişle, arka uç uygulama) API ve geliştirici konsol (diğer bir deyişle, istemci uygulama) göstermek için iki uygulama kaydettiniz, biz arka uç uygulamasını çağırmak istemci uygulaması izin vermek için izinleri vermeniz gerekir.  

Gidin **uygulama kayıtlar** yeniden. 

Tıklayın `client-app` ve Git **ayarları**.

Tıklayın **gerekli izinleri** ve ardından **Ekle**.

Tıklayın **bir API seçin** arayın ve `backend-app`.

Denetleme `Access backend-app` altında **izinleri atanmış**. 

Tıklayın **seçin** ve ardından **Bitti**. 

> [!NOTE]
> Varsa **Windows** **Azure Active Directory** olan diğer uygulamalara izinler altında listelenen değil, tıklatın **Ekle** ve listeden ekleyin.
> 
> 

## <a name="enable-oauth-20-user-authorization-in-the-developer-console"></a>OAuth 2.0 kullanıcı yetkilendirme Geliştirici konsolunda etkinleştir

Bu noktada, biz bizim uygulamaları Azure AD içinde oluşturdunuz ve arka uç uygulamasını çağırmak istemci uygulaması izin vermek için uygun izinlerin. 

Bu kılavuzda, uygulamayı istemci olarak Geliştirici konsol kullanacağız. Aşağıdaki adımları OAuth 2.0 kullanıcı yetkilendirme Geliştirici konsolunda etkinleştirmeyi açıklar 

APIM örneğine gidin.

Tıklayın **OAuth 2.0** ve ardından **eklemek**.

Sağlayan bir **görünen adı** ve **açıklama**.

URL, istemci kaydı için sayfa ** bir yer tutucu değerini girin `http://localhost`.  **İstemci kayıt sayfası URL'si** kullanıcılar oluşturmak ve kullanıcı hesaplarının yönetimini desteklemek için OAuth 2.0 sağlayıcıları kendi hesaplarını yapılandırmak için kullanabileceğiniz sayfa işaret eder. Bu örnekte, kullanıcıların değil oluşturun ve yer tutucu kullanılmak üzere kendi hesaplarını yapılandırın.

Denetleme **yetkilendirme kodu** olarak **yetki türleri**.

Ardından, belirtin **yetkilendirme uç noktası URL'si** ve **belirteç uç nokta URL'si**.

Bu değerleri penceresinden alınabilir **uç noktaları** Azure AD kiracınıza sayfasında. Uç noktaları erişmek için gidin **uygulama kayıtlar** yeniden sayfasında ve tıklayın **uç noktaları**.

Kopya **OAuth 2.0 yetkilendirme uç noktası** ve yapıştırın **yetkilendirme uç noktası URL'si** metin kutusu.

Kopya **OAuth 2.0 belirteç uç noktası** ve yapıştırın **belirteç uç nokta URL'si** metin kutusu.

Belirteç uç noktası yapıştırarak yanı sıra, adlandırılmış bir ek gövde parametresini ekleyin **kaynak** ve değerini kullanmak için **uygulama kimliği** arka uç uygulaması için.

Ardından, istemci kimlik bilgilerini belirtin. Bu, istemci uygulaması için kimlik bilgileridir.

İçin **istemci kimliği**, kullanın **uygulama kimliği** istemci uygulaması için.

İçin **gizli**, daha önce oluşturduğunuz anahtarı için istemci uygulaması kullanın. 

İstemci gizli anahtarı aşağıdaki hemen **redirect_url** için yetkilendirme kodu verme türü.

Bu URL not edin.

Tıklayın **oluşturma**.

Geri gidin **ayarları** , istemci uygulamanızın sayfası.

Tıklayın **yanıt URL'leri** Yapıştır **redirect_url** ilk satırda. Bu örnekte, biz yerini `https://localhost` ilk satırda URL'si.  

Bir OAuth 2.0 yetkilendirme sunucusu yapılandırmış olduğunuz şimdi Geliştirici konsol Azure AD'den erişim belirteçleri elde etmiş olması gerekir. 

Sonraki adım bizim API için OAuth 2.0 kullanıcı yetkilendirme etkinleştirmek için bizim API çağrıları yapmadan önce kullanıcı adına bir erişim belirteci almak Bunu geliştirici konsol bilen böylece gerekiyor.

APIM örneğine gidin, Git **API'leri**.

Korumak istediğiniz API'ı tıklatın. Bu örnekte, kullanacağız `Echo API`.

Git **ayarları**.

Altında **güvenlik**, seçin **OAuth 2.0** ve biz yapılandırılmış önceki OAuth 2.0 sunucuyu seçin. 

**Kaydet**'e tıklayın.

## <a name="successfully-call-the-api-from-the-developer-portal"></a>Geliştirici Portalı'ndan başarıyla API çağrısı

OAuth 2.0 kullanıcı yetkilendirme etkin göre `Echo API`, geliştirici konsolunda bir API'yi çağırmadan önce kullanıcı adına bir erişim belirteci alacaktır.

Herhangi bir işlem altında gidin `Echo API` tıklatın ve Geliştirici Portalı'ndaki **deneyin**, hangi getirecek bize Geliştirici konsola.

Yeni bir öğe Not **yetkilendirme** yeni eklenen yetkilendirme sunucusu karşılık gelen bölüm.

Seçin **yetkilendirme kodu** onayı için Azure AD kiracısı oturum açmak için aşağı açılan liste ve istenir. Hesapla oturum açtıysanız, istenebilir değil.

Başarılı oturum açma sonra bir `Authorization` üstbilgi eklenecek bir erişim belirteci ile isteği için Azure AD. 

Bir örnek simge, aşağıda Base64 kodlu olmadığı görülüyor.

```
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCIsImtpZCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCJ9.eyJhdWQiOiIxYzg2ZWVmNC1jMjZkLTRiNGUtODEzNy0wYjBiZTEyM2NhMGMiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC80NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgvIiwiaWF0IjoxNTIxMTUyNjMzLCJuYmYiOjE1MjExNTI2MzMsImV4cCI6MTUyMTE1NjUzMywiYWNyIjoiMSIsImFpbyI6IkFWUUFxLzhHQUFBQUptVzkzTFd6dVArcGF4ZzJPeGE1cGp2V1NXV1ZSVnd1ZXZ5QU5yMlNkc0tkQmFWNnNjcHZsbUpmT1dDOThscUJJMDhXdlB6cDdlenpJdzJLai9MdWdXWWdydHhkM1lmaDlYSGpXeFVaWk9JPSIsImFtciI6WyJyc2EiXSwiYXBwaWQiOiJhYTY5ODM1OC0yMWEzLTRhYTQtYjI3OC1mMzI2NTMzMDUzZTkiLCJhcHBpZGFjciI6IjEiLCJlbWFpbCI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsImZhbWlseV9uYW1lIjoiSmlhbmciLCJnaXZlbl9uYW1lIjoiTWlhbyIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJpcGFkZHIiOiIxMzEuMTA3LjE3NC4xNDAiLCJuYW1lIjoiTWlhbyBKaWFuZyIsIm9pZCI6IjhiMTU4ZDEwLWVmZGItNDUxMS1iOTQzLTczOWZkYjMxNzAyZSIsInNjcCI6InVzZXJfaW1wZXJzb25hdGlvbiIsInN1YiI6IkFGaWtvWFk1TEV1LTNkbk1pa3Z3MUJzQUx4SGIybV9IaVJjaHVfSEM1aGciLCJ0aWQiOiI0NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgiLCJ1bmlxdWVfbmFtZSI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsInV0aSI6ImFQaTJxOVZ6ODBXdHNsYjRBMzBCQUEiLCJ2ZXIiOiIxLjAifQ.agGfaegYRnGj6DM_-N_eYulnQdXHhrsus45QDuApirETDR2P2aMRxRioOCR2YVwn8pmpQ1LoAhddcYMWisrw_qhaQr0AYsDPWRtJ6x0hDk5teUgbix3gazb7F-TVcC1gXpc9y7j77Ujxcq9z0r5lF65Y9bpNSefn9Te6GZYG7BgKEixqC4W6LqjtcjuOuW-ouy6LSSox71Fj4Ni3zkGfxX1T_jiOvQTd6BBltSrShDm0bTMefoyX8oqfMEA2ziKjwvBFrOjO0uK4rJLgLYH4qvkR0bdF9etdstqKMo5gecarWHNzWi_tghQu9aE3Z3EZdYNI_ZGM-Bbe3pkCfvEOyA
```

Tıklatın **Gönder** ve API başarıyla çağırabilmesi.


## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>İstekleri önceden yetkilendirmek için JWT doğrulama ilkesini yapılandırma

Bu noktada, geliştirici konsolundan arama yapmak bir kullanıcı çalıştığında, kullanıcı oturum açmak için istenir ve geliştirici konsolunda bir erişim belirteci kullanıcı adına alacaktır. Her şeyin beklendiği gibi çalışmaktadır. Ancak, ne birisi bizim API bir belirteç olmadan veya geçersiz bir belirteç çağrılarının? Örneğin, silmeyi deneyebilirsiniz `Authorization` üstbilgi ve yine de API çağrısı tüketimi bulur. APIM erişim belirteci bu noktada doğrulamadığı için nedenidir. Yalnızca geçirir `Auhtorization` arka uç API'si için üstbilgi.

Biz kullanabilirsiniz [doğrulamak JWT](api-management-access-restriction-policies.md#ValidateJWT) APIM isteklerinde her gelen istek erişim belirteçleri doğrulayarak önceden yetkilendirmek için ilke. Bir isteğin geçerli bir belirteci yoksa, API Management tarafından engellenir ve boyunca arka ucuna aktarılmaz. Örneğin, ekleyebiliriz ilkesine aşağıda `<inbound>` ilke bölümünü `Echo API`. Bir erişim belirteci İzleyici talepte denetler ve belirtecin geçerli değilse, bir hata iletisi döndürür. İlkeleri yapılandırma hakkında daha fazla bilgi için bkz: [ayarlayın veya ilkeleri düzenleme](set-edit-policies.md).

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

Bu kılavuzda, biz APIM Geliştirici konsolunda örnek istemci uygulaması olarak çağırmak için kullanılan `Echo API` OAuth 2.0 tarafından korumalı. Bir uygulama oluşturmak ve OAuth 2.0 akışını uygulama hakkında daha fazla bilgi için lütfen bkz [Azure Active Directory kod örnekleri](../active-directory/develop/active-directory-code-samples.md).

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinmek [Azure Active Directory ve OAuth2.0](../active-directory/develop/active-directory-authentication-scenarios.md)
* Daha fazla bilgi denetleyin [videolar](https://azure.microsoft.com/documentation/videos/index/?services=api-management) API Management hakkında.
* Arka uç hizmetinizin güvenliğini sağlamak diğer yolları için bkz: [karşılıklı sertifika kimlik doğrulaması](api-management-howto-mutual-certificates.md).

[Create an API Management service instance]: get-started-create-service-instance.md
[Manage your first API]: import-and-publish.md
