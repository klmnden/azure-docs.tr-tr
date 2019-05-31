---
title: Azure Active Directory ile API Management OAuth 2.0 kullanarak bir API'yi koruma | Microsoft Docs
description: Azure Active Directory ve API Management ile bir web API arka ucunu koruma hakkında bilgi edinin.
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
ms.date: 05/21/2019
ms.author: apimpm
ms.openlocfilehash: 73dd46d1ca0a20748d7a3a7838c499f0c659253d
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66241672"
---
# <a name="protect-an-api-by-using-oauth-20-with-azure-active-directory-and-api-management"></a>Azure Active Directory ile API Management OAuth 2.0 kullanarak bir API'yi koruma

Bu kılavuz, Azure Active Directory (Azure AD) ile OAuth 2.0 protokolünü kullanarak bir API'yi korumak için Azure API Management örneğinizin yapılandırma göstermektedir. 

## <a name="prerequisites"></a>Önkoşullar
Bu makaledeki adımları izlemek için şunlara sahip olmalısınız:
* API Management örneği
* API Management örneği kullanan bir API yayımlanıyor
* Azure AD kiracısı

## <a name="overview"></a>Genel Bakış

Adımlara hızlı genel bakış aşağıda verilmiştir:

1. Bir uygulama (uygulama arka ucu) API temsil etmek için Azure AD'ye kaydetme.
2. Başka bir uygulama (istemci uygulaması) API'si çağırmayı gerektiren bir istemci uygulaması temsil etmek için Azure AD'ye kaydetme.
3. Azure AD'de, arka uç uygulamasını çağırmak istemci uygulaması izin vermek için izinleri verin.
4. OAuth 2.0 kullanıcı kimlik doğrulaması kullanmak için geliştirici konsolu yapılandırın.
5. Ekleme **doğrulama jwt** her gelen istek için bir OAuth belirteci doğrulamak için ilke.

## <a name="register-an-application-in-azure-ad-to-represent-the-api"></a>API temsil etmek için Azure AD'de bir uygulamayı kaydetme

Azure AD ile bir API'yi korumak için ilk adım bir uygulama, API'yi temsil eden Azure AD'ye kaydetme sağlamaktır. 

1. Gidin [Azure Portalı - Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfası. 

2. Seçin **yeni kayıt**. 

1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin: 
    - **Ad** alanına uygulama kullanıcılarına gösterilecek anlamlı bir uygulama adı girin, örneğin `backend-app`. 
    - İçinde **desteklenen hesap türleri** bölümünden **herhangi bir kuruluş dizini hesaplarında**. 

1. Bırakın **yeniden yönlendirme URI'si** bölüm şimdilik boş.

1. Uygulamayı kaydetmek için **Kaydet**'i seçin. 

1. Uygulamasında **genel bakış** sayfasında, bulmak **uygulama (istemci) kimliği** değeri ve daha sonra kullanmak üzere kaydedin.

Uygulama oluşturulduğunda, Not **uygulama kimliği**, bir sonraki adımda kullanmak üzere. 

## <a name="register-another-application-in-azure-ad-to-represent-a-client-application"></a>Bir istemci uygulaması temsil etmek için Azure AD'de başka bir uygulamayı kaydetme

API'yi çağıran her istemci uygulaması, Azure AD'de bir uygulama olarak kaydedilmesi gerekiyor. Bu örnekte, örnek istemci uygulaması, API Management Geliştirici portalındaki Geliştirici konsoludur. Başka bir uygulama Geliştirici Konsolu temsil etmek için Azure AD'ye kaydetme açıklanmıştır.

1. Gidin [Azure Portalı - Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfası. 

1. Seçin **yeni kayıt**.

1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin: 
    - **Ad** alanına uygulama kullanıcılarına gösterilecek anlamlı bir uygulama adı girin, örneğin `client-app`. 
    - İçinde **desteklenen hesap türleri** bölümünden **herhangi bir kuruluş dizini hesaplarında**. 

1. İçinde **yeniden yönlendirme URI'si** bölümünden `Web` URL'sini girin `https://contoso5.portal.azure-api.net/signin`

1. Uygulamayı kaydetmek için **Kaydet**'i seçin. 

1. Uygulamasında **genel bakış** sayfasında, bulmak **uygulama (istemci) kimliği** değeri ve daha sonra kullanmak üzere kaydedin.

Şimdi bir sonraki adımda kullanmak üzere bu uygulama için bir istemci gizli anahtarı oluşturun.

1. İstemci uygulamanız için sayfa listesinden seçin **sertifikaları ve parolaları**seçip **yeni gizli**.

2. Altında **istemci gizli dizi eklemek**, sağlayan bir **açıklama**. Zaman anahtarı sona seçin ve seçim **Ekle**.

Anahtar değerini not edin. 

## <a name="grant-permissions-in-azure-ad"></a>Azure AD'de izin ver

API ve geliştirici Konsolu temsil etmek için iki uygulama kaydolduğundan, arka uç uygulamasını çağırmak istemci uygulaması izin vermek için izinleri vermeniz gerekir.  

1. Gidin **uygulama kayıtları**. 

2. Seçin `client-app`, uygulama sayfaları listesinde gidin **API izinleri**.

3. Seçin **bir izin eklemek**.

4. Altında **bir API seçin**bulup seçin `backend-app`.

5. Altında **Temsilcili izinler**, uygun izinleri seçin `backend-app`.

6. Seçin **izinleri ekleme** 

> [!NOTE]
> Varsa **Azure Active Directory** select diğer uygulamalara izinler altında listelenmez **Ekle** listeden eklemek için.

## <a name="enable-oauth-20-user-authorization-in-the-developer-console"></a>OAuth 2.0 kullanıcı kimlik Geliştirici konsolunda etkinleştir

Bu noktada, uygulamalarınızı Azure AD'de oluşturduğunuz ve arka uç uygulamasını çağırmak istemci uygulaması izin vermek için uygun izinleri vermiş olması. 

Bu örnekte istemci uygulaması Geliştirici konsoludur. Aşağıdaki adımlar, OAuth 2.0 kullanıcı kimlik Geliştirici konsolunda etkinleştirmek açıklanmaktadır. 

1. Azure portalında, API Management Örneğinize göz atın.

2. Seçin **OAuth 2.0** > **ekleme**.

3. Sağlayan bir **görünen ad** ve **açıklama**.

4. İçin **istemci kayıt sayfası URL'si**, bir yer tutucu değerini girmeniz `http://localhost`. **İstemci kayıt sayfası URL'si** kullanıcılar oluşturmak ve bunu desteklemek için OAuth 2.0 sağlayıcıları kendi hesaplarını yapılandırmak için kullanabileceğiniz sayfayı işaret eder. Bu örnekte, kullanıcılar değil oluşturabilir ve bunun yerine bir yer tutucu kullanmak için kendi hesaplarını yapılandırın.

5. İçin **yetkilendirme verme türleri**seçin **yetkilendirme kodu**.

6. Belirtin **yetkilendirme uç noktası URL'si** ve **belirteç uç noktası URL'si**. Bu değerleri almak **uç noktaları** Azure AD kiracınızda sayfası. Gözat **uygulama kayıtları** yeniden sayfasında ve seçin **uç noktaları**.

    >[!NOTE]
    > Kullanım **v1** burada uç noktaları

7. Kopyalama **OAuth 2.0 yetkilendirme uç noktası**, yapıştırın **yetkilendirme uç noktası URL'si** metin kutusu.

8. Kopyalama **OAuth 2.0 belirteç uç noktası**, yapıştırın **belirteç uç noktası URL'si** metin kutusu. Belirteç uç noktası yapıştırarak ek olarak, adlı bir gövde parametresi Ekle **kaynak**. Bu parametrenin değerini kullanmak **uygulama kimliği** arka uç uygulaması için.

9. Ardından, istemci kimlik bilgilerini belirtin. Bu, istemci uygulaması için kimlik bilgileridir.

10. İçin **istemci kimliği**, kullanın **uygulama kimliği** istemci uygulaması.

11. İçin **gizli**, istemci uygulama daha önce oluşturduğunuz anahtarı kullanın. 

12. İstemci gizli anahtarı aşağıdaki hemen **redirect_url** yetkilendirme kodu verme türü. Bu URL'yi not edin.

13. **Oluştur**’u seçin.

14. Geri Git **ayarları** , istemci uygulamasının sayfası.

15. Seçin **yanıt URL'leri**, Yapıştır **redirect_url** ilk satırında. Bu örnekte, değiştirdiğiniz `https://localhost` ilk satırında URL'siyle.  

Bir OAuth 2.0 yetkilendirme sunucusu yapılandırdığınıza göre Geliştirici Konsolu Azure AD'den erişim belirteci elde edebilirsiniz. 

Sonraki adım, API'niz için OAuth 2.0 kullanıcı kimlik etkinleştirmektir. Bu, apı'nize çağrı yapmadan önce kullanıcı adına bir erişim belirteci almak gerektiğini öğrenmek Geliştirici Konsolu sağlar.

1. API Management Örneğinize göz atın ve Git **API'leri**.

2. Korumak istediğiniz API'yi seçin. Bu örnekte, kullandığınız `Echo API`.

3. Git **ayarları**.

4. Altında **güvenlik**, seçin **OAuth 2.0**, daha önce yapılandırdığınız OAuth 2.0 sunucusu seçin. 

5. **Kaydet**’i seçin.

## <a name="successfully-call-the-api-from-the-developer-portal"></a>Başarıyla Geliştirici portalından API çağırma

> [!NOTE]
> Bu bölümde uygulanmaz **tüketim** katmanı, Geliştirici Portalı desteklemez.

OAuth 2.0 kullanıcı kimlik doğrulaması etkin göre `Echo API`, bir API'yi çağırmadan önce Geliştirici konsolu, kullanıcı adına bir erişim belirteci alır.

1. Herhangi bir işlem altında Gözat `Echo API` Geliştirici Portalı ve seçme içinde **deneyin**. Bu Geliştirici konsoluna getirir.

2. İçinde yeni bir öğe Not **yetkilendirme** yeni eklenen yetkilendirme sunucusu karşılık gelen bölüm.

3. Seçin **yetkilendirme kodu** yetkilendirme Azure AD kiracısı için oturum açmanız istenir ve açılır listede. Zaten bir hesapla oturum açtıysanız, size istenebilir değil.

4. Başarılı oturum açma sonra bir `Authorization` başlığı, Azure AD'den bir erişim belirteci ile isteği eklenir. Bir örnek simge (şifreli Base64) aşağıdadır:

   ```
   Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCIsImtpZCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCJ9.eyJhdWQiOiIxYzg2ZWVmNC1jMjZkLTRiNGUtODEzNy0wYjBiZTEyM2NhMGMiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC80NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgvIiwiaWF0IjoxNTIxMTUyNjMzLCJuYmYiOjE1MjExNTI2MzMsImV4cCI6MTUyMTE1NjUzMywiYWNyIjoiMSIsImFpbyI6IkFWUUFxLzhHQUFBQUptVzkzTFd6dVArcGF4ZzJPeGE1cGp2V1NXV1ZSVnd1ZXZ5QU5yMlNkc0tkQmFWNnNjcHZsbUpmT1dDOThscUJJMDhXdlB6cDdlenpJdzJLai9MdWdXWWdydHhkM1lmaDlYSGpXeFVaWk9JPSIsImFtciI6WyJyc2EiXSwiYXBwaWQiOiJhYTY5ODM1OC0yMWEzLTRhYTQtYjI3OC1mMzI2NTMzMDUzZTkiLCJhcHBpZGFjciI6IjEiLCJlbWFpbCI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsImZhbWlseV9uYW1lIjoiSmlhbmciLCJnaXZlbl9uYW1lIjoiTWlhbyIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJpcGFkZHIiOiIxMzEuMTA3LjE3NC4xNDAiLCJuYW1lIjoiTWlhbyBKaWFuZyIsIm9pZCI6IjhiMTU4ZDEwLWVmZGItNDUxMS1iOTQzLTczOWZkYjMxNzAyZSIsInNjcCI6InVzZXJfaW1wZXJzb25hdGlvbiIsInN1YiI6IkFGaWtvWFk1TEV1LTNkbk1pa3Z3MUJzQUx4SGIybV9IaVJjaHVfSEM1aGciLCJ0aWQiOiI0NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgiLCJ1bmlxdWVfbmFtZSI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsInV0aSI6ImFQaTJxOVZ6ODBXdHNsYjRBMzBCQUEiLCJ2ZXIiOiIxLjAifQ.agGfaegYRnGj6DM_-N_eYulnQdXHhrsus45QDuApirETDR2P2aMRxRioOCR2YVwn8pmpQ1LoAhddcYMWisrw_qhaQr0AYsDPWRtJ6x0hDk5teUgbix3gazb7F-TVcC1gXpc9y7j77Ujxcq9z0r5lF65Y9bpNSefn9Te6GZYG7BgKEixqC4W6LqjtcjuOuW-ouy6LSSox71Fj4Ni3zkGfxX1T_jiOvQTd6BBltSrShDm0bTMefoyX8oqfMEA2ziKjwvBFrOjO0uK4rJLgLYH4qvkR0bdF9etdstqKMo5gecarWHNzWi_tghQu9aE3Z3EZdYNI_ZGM-Bbe3pkCfvEOyA
   ```

5. Seçin **Gönder**, ve API başarıyla çağırabilirsiniz.


## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>İstekleri önceden yetkilendirmek üzere JWT doğrulama ilkesini yapılandırma

Bir kullanıcı arama Geliştirici konsolundan yapmaya çalıştığında bu noktada, kullanıcı oturum açmanız istenir. Geliştirici konsolu, kullanıcı adına bir erişim belirteci alır.

Ancak ne birisi bir belirteç olmadan veya geçersiz bir belirteç ile API'nizi çağıran? Örneğin, silseniz bile API yine de çağırabilirsiniz `Authorization` başlığı. API Management erişim belirteci bu noktada doğrulamaz, nedenidir. Yalnızca geçirir `Authorization` arka uç API'sine başlığı.

Kullanabileceğiniz [doğrulamak için JWT](api-management-access-restriction-policies.md#ValidateJWT) gelen her isteğin erişim belirteci doğrulayarak API Management istekleri önceden yetkilendirmek için ilke. Bir isteğin geçerli bir belirteç yoksa, API Management bunu engeller. Örneğin, aşağıdaki ilkeye ekleyebilirsiniz `<inbound>` İlkesi bölümünden `Echo API`. Bir erişim belirteci hedef kitlesi talebi denetler ve belirteç geçerli değil. hata iletisi döndürür. İlkeleri yapılandırma hakkında daha fazla bilgi için bkz: [ayarlayın veya ilkeleri düzenleme](set-edit-policies.md).

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

## <a name="build-an-application-to-call-the-api"></a>API'yi çağırmak için uygulama oluşturma

Bu kılavuzda, API Management Geliştirici Konsolu örnek istemci uygulaması çağırmak için kullanılan `Echo API` OAuth 2.0 tarafından korunan. Bir uygulama oluşturmak ve uygulama OAuth 2.0 hakkında daha fazla bilgi için bkz. [Azure Active Directory kod örnekleri](../active-directory/develop/sample-v1-code.md).

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinin [Azure Active Directory ve OAuth2.0](../active-directory/develop/authentication-scenarios.md).
* Daha fazla bilgi denetleyin [videoları](https://azure.microsoft.com/documentation/videos/index/?services=api-management) API Management hakkında.
* Arka uç hizmetinizin güvenliğini sağlamak diğer yolları için bkz. [karşılıklı sertifika kimlik doğrulaması](api-management-howto-mutual-certificates.md).

* [Bir API Management hizmet örneği oluşturma](get-started-create-service-instance.md).

* [İlk API'nizi yönetme](import-and-publish.md).
