---
title: Azure Active Directory kimlik doğrulaması ve Resource Manager | Microsoft Docs
description: Uygulama diğer Azure aboneliklerine ile tümleştirmek için Azure Active Directory ve Azure Resource Manager API'si ile kimlik doğrulaması için Geliştirici Kılavuzu.
author: dushyantgill
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 04/05/2019
ms.author: dugill
ms.openlocfilehash: 3a8f9f1975530c846008b3b3def4f4d4a22716fd
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67205441"
---
# <a name="use-resource-manager-authentication-api-to-access-subscriptions"></a>Aboneliklere erişmek için Kaynak Yöneticisi'ni kullanın kimlik doğrulama API'si

Bir müşterinin Azure kaynaklarını yöneten bir uygulama oluşturmak için gereken bir yazılım geliştiricisiyseniz, bu makalede, Azure Resource Manager API'leri ile kimlik doğrulaması ve diğer Aboneliklerdeki kaynaklara erişmek için işlemini göstermektedir.

Uygulamanızı çeşitli şekillerde Resource Manager API'leri erişebilirsiniz:

1. **Kullanıcı + uygulama erişimi**: oturum açmış kullanıcının kaynaklara erişen uygulamalar için. Bu yaklaşım, web uygulamaları ve yalnızca "Etkileşimli yönetim" Azure kaynakları ile ilgili komut satırı araçları gibi uygulamalar için çalışır.
2. **Salt uygulama erişim**: daemon Hizmetleri ve zamanlanan işler çalışan uygulamalar için. Uygulamanın kimliğini kaynaklarına doğrudan erişimi verilir. Bu yaklaşım, Azure uzun süreli gözetimsiz (katılımsız) erişmesi gereken uygulamaları için çalışır.

Bu makalede, bu iki yetkilendirme yöntemi kullanan bir uygulama oluşturmak için adım adım yönergeler sağlar. REST API ile her bir adımı nasıl gösterir veya C#. Eksiksiz bir ASP.NET MVC uygulaması kullanılabilir [ https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense ](https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense).

## <a name="what-the-web-app-does"></a>Web uygulaması yapar

Web uygulaması:

1. Bir Azure kullanıcısı oturum açtığında.
2. Resource Manager için web uygulaması erişimi vermek için kullanıcıya sorar.
3. Resource Manager'a erişmek için kullanıcı + uygulama erişim belirteci alır.
4. Abonelik rolünde uygulamanın hizmet sorumlusu atamak için (3. adımdaki) belirteci kullanır. Bu adım, aboneliğe uygulama uzun süreli erişim sağlar.
5. Salt uygulama erişim belirtecini alır.
6. Aboneliği aracılığıyla Resource Manager kaynaklarını yönetmek için (Başlangıç 5. adım) belirteci kullanır.

Web uygulamasının akışı aşağıda verilmiştir.

![Kaynak Yöneticisi kimlik doğrulama akışı](./media/resource-manager-api-authentication/Auth-Swim-Lane.png)

Bir kullanıcı olarak kullanmak istediğiniz abonelik için abonelik Kimliğini sağlayın:

![Abonelik kimliği sağlayın](./media/resource-manager-api-authentication/sample-ux-1.png)

Oturum açmak için kullanılacak hesabı seçin.

![hesabı seçin](./media/resource-manager-api-authentication/sample-ux-2.png)

Kimlik bilgilerinizi sağlayın.

![kimlik bilgilerini sağlayın](./media/resource-manager-api-authentication/sample-ux-3.png)

Azure Abonelikleriniz için uygulama erişimi verin:

![Erişim verme](./media/resource-manager-api-authentication/sample-ux-4.png)

Bağlı aboneliklerinizi yönetin:

![Abonelik'e bağlanma](./media/resource-manager-api-authentication/sample-ux-7.png)

## <a name="register-application"></a>Uygulamayı kaydetme
Kodlama başlamadan önce web uygulamanızı Azure Active Directory (AD ile) kaydedin. Uygulama kaydı, Azure AD'de uygulamanız için merkezi bir kimliği oluşturur. Bu, uygulamanız OAuth istemci kimliği ve yanıt URL'leri, uygulamanızın kimlik doğrulaması ve Azure Resource Manager API'lerine erişmek için kullandığı kimlik bilgileri gibi ilgili temel bilgileri tutar. Uygulama kaydı için kullanıcının Microsoft APIs erişirken uygulamanızın çeşitli temsilci izinleri de kaydeder.

Uygulamanızı kaydetmek için bkz: [hızlı başlangıç: Microsoft kimlik platformu bir uygulamayı kaydetme](../active-directory/develop/quickstart-register-app.md). Uygulamanıza bir ad verin ve seçin **herhangi bir kuruluş dizini hesaplarında** desteklenen hesap türleri için. Azure Active Directory ile ilişkili bir etki alanı yeniden yönlendirme URL'sini sağlayın.

AD uygulaması oturum açmak için uygulama kimliği ve gizli anahtarı gerekir. Uygulama kimliği, uygulama için bir genel bakış görüntülenir. Gizli dizi oluşturma ve API izinleri istemek için bkz: [hızlı başlangıç: Web API'leri erişmek için bir istemci uygulamasını yapılandırmak](../active-directory/develop/quickstart-configure-app-access-web-apis.md). Yeni bir istemci gizli anahtarı sağlayın. API izinleri seçin **Azure Hizmet Yönetimi**. Seçin **temsilci izinleri** ve **user_impersonation**.

### <a name="optional-configuration---certificate-credential"></a>İsteğe bağlı yapılandırma - sertifika kimlik bilgisi
Azure AD uygulamaları için de sertifika kimlik bilgilerini destekler: otomatik olarak imzalanan bir sertifika oluşturmak, özel anahtarı tutmak ve Azure AD uygulama kaydınızı için kullanılacak ortak anahtarı ekleyin. Kimlik doğrulaması için uygulamanızı Azure AD'ye özel anahtarınız ile imzalanmış küçük bir yükü gönderir ve Azure AD'ye kaydettiniz ortak anahtar kullanarak imzayı doğrular.

Bir sertifika ile bir AD uygulamasını oluşturma hakkında daha fazla bilgi için bkz: [kaynaklara erişmek için bir hizmet sorumlusu oluşturmak için Azure PowerShell kullanarak](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-certificate-from-certificate-authority) veya [kaynaklara erişmek için bir hizmet sorumlusu oluşturmak için Azure CLI'yı kullanın](resource-group-authenticate-service-principal-cli.md) .

## <a name="get-tenant-id-from-subscription-id"></a>Abonelik Kimliğinden Kiracı Kimliğinizi alma
Resource Manager'ı çağırmak için kullanılabilecek bir belirteç istemek için uygulamanızı barındıran Azure aboneliği Azure AD kiracısını Kiracı kimliği gerekir. Büyük olasılıkla kullandıkları abonelik kimliklerini kullanıcılarınızı tanıyın, ancak bunlar kendi Kiracı kimlikleri Azure Active Directory için fark etmeyebilirsiniz. Kullanıcının Kiracı Kimliğini almak için abonelik kimliği için kullanıcıya sor Bu aboneliği sağlama aboneliği hakkında bir isteği gönderirken kimliği:

    https://management.azure.com/subscriptions/{subscription-id}?api-version=2015-01-01

İstek, kullanıcı henüz oturum taşınmadığından, ancak Kiracı kimliği gelen yanıt almak için başarısız olur. Bu özel durum yanıt üst bilgisi değeri Kiracı Kimliğini almak **WWW-Authenticate**. Bu uygulamada gördüğünüz [GetDirectoryForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L20) yöntemi.

## <a name="get-user--app-access-token"></a>Kullanıcı + uygulama erişim belirteci alma
Uygulamanızı Azure AD ile bir OAuth 2.0 yetkilendirme kullanıcının kimlik bilgilerini kimlik doğrulaması ve yetkilendirme kodunu geri almak için isteği - kullanıcı yönlendirir. Uygulamanız, kaynak yöneticisi için bir erişim belirteci almak için yetkilendirme kodunu kullanır. [ConnectSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/Controllers/HomeController.cs#L42) yöntemi yetkilendirme isteği oluşturur.

Bu makalede, kullanıcının kimliğini doğrulamak için REST API istekleri gösterir. Kodunuzda doğrulamaya yardımcı kitaplıklarını da kullanabilirsiniz. Bu kitaplıklar hakkında daha fazla bilgi için bkz: [Azure Active Directory kimlik doğrulama kitaplıkları](../active-directory/active-directory-authentication-libraries.md). Bir uygulamada Kimlik Yönetimi ile ilgili yönergeler için bkz. [Azure Active Directory Geliştirici Kılavuzu](../active-directory/develop/v1-overview.md).

### <a name="auth-request-oauth-20"></a>Kimlik doğrulama isteği (OAuth 2.0)
Bir açık Bağlan/OAuth2.0 yetkilendirme kimliği için Azure AD Authorize son noktası yürütün:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize

Bu istek için sorgu dizesi parametreleri açıklanan [bir yetkilendirme kodu istek](../active-directory/develop/v1-protocols-oauth-code.md#request-an-authorization-code) makalesi.

Aşağıdaki örnek, OAuth2.0 yetkilendirme isteği gösterilmektedir:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=query&response_type=code&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&domain_hint=live.com

Azure AD kullanıcının kimliğini doğrular ve gerekirse, uygulama izni vermek için kullanıcıya sorar. Uygulamanızın yanıt URL'SİNİN yetkilendirme kodunu döndürür. Azure AD istenen response_mode ya da bağlı olarak geri verileri sorgu dizesi veya gönderme verisi olarak gönderir.

    code=AAABAAAAiL****FDMZBUwZ8eCAA&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="auth-request-open-id-connect"></a>Kimlik doğrulama isteği (Open ID Connect)
Bir açık bağlanmak yetkilendirme istek kimliği yalnızca kullanıcı için Azure Resource Manager'a erişmek istiyor, ancak Ayrıca uygulamanızı kendi Azure AD hesabını kullanarak oturum açmak kullanıcının izin verin. Open ID Connect ile uygulamanızı uygulama kullanıcının oturum açmak için kullanabileceğiniz Azure AD'den bir id_token de alır.

Bu istek için sorgu dizesi parametreleri açıklanan [oturum açma isteği Gönder](../active-directory/develop/v1-protocols-openid-connect-code.md#send-the-sign-in-request) makalesi.

Bir örnek istek Open ID Connect şöyledir:

     https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=form_post&response_type=code+id_token&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&scope=openid+profile&nonce=63567Dc4MDAw&domain_hint=live.com&state=M_12tMyKaM8

Azure AD kullanıcının kimliğini doğrular ve gerekirse, uygulama izni vermek için kullanıcıya sorar. Uygulamanızın yanıt URL'SİNİN yetkilendirme kodunu döndürür. Azure AD istenen response_mode ya da bağlı olarak geri verileri sorgu dizesi veya gönderme verisi olarak gönderir.

Open ID Connect yanıt örneği verilmiştir:

    code=AAABAAAAiL*****I4rDWd7zXsH6WUjlkIEQxIAA&id_token=eyJ0eXAiOiJKV1Q*****T3GrzzSFxg&state=M_12tMyKaM8&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="token-request-oauth20-code-grant-flow"></a>Belirteç isteği (OAuth2.0 kodu verme akışı)
Uygulamanızı Azure AD'den yetkilendirme kodu aldı, Azure Resource Manager için erişim belirteci almak için zaman var.  Bir OAuth2.0 kod verme belirteci isteği için Azure AD belirteç uç noktası gönderin:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

Bu istek için sorgu dizesi parametreleri açıklanan [yetkilendirme kodu](../active-directory/develop/v1-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token) makalesi.

Aşağıdaki örnek kodu verme belirteciyle parola kimlik bilgisi için bir istek gösterir:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Sertifika kimlik bilgileriyle çalışırken, bir JSON Web Token (JWT) ve uygulamanızın sertifika kimlik bilgisi özel anahtarı kullanarak oturum (RSA SHA256) oluşturun. Bu belirteç oluşturma gösterilir [istemci kimlik bilgileri akışı](../active-directory/develop/v1-oauth2-client-creds-grant-flow.md#second-case-access-token-request-with a-certificate).  Başvuru için bkz: [Active Directory kimlik doğrulama kitaplığı (.NET) kod](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/blob/dev/src/ADAL.PCL.Desktop/CryptographyHelper.cs) istemci onaylama JWT Belirteçleri imzalamak için.

Bkz: [Open ID Connect spec](https://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication) istemci kimlik doğrulaması hakkında ayrıntılı bilgi için.

Aşağıdaki örnek, bir istek için sertifika kimlik bilgilerini verme belirteciyle kod gösterir:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbG*****Y9cYo8nEjMyA

Örnek yanıt kodu verme belirteci:

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039858","not_before":"1432035958","resource":"https://management.core.windows.net/","access_token":"eyJ0eXAiOiJKV1Q****M7Cw6JWtfY2lGc5A","refresh_token":"AAABAAAAiL9Kn2Z****55j-sjnyYgAA","scope":"user_impersonation","id_token":"eyJ0eXAiOiJKV*****-drP1J3P-HnHi9Rr46kGZnukEBH4dsg"}

#### <a name="handle-code-grant-token-response"></a>Kod verme belirteç yanıtı işleme
Başarılı bir token yanıt içerir (kullanıcı + uygulama) erişim belirteci için Azure Resource Manager. Uygulamanız için kullanıcı Resource Manager'a erişmek için bu erişim belirtecini kullanır. Azure AD tarafından verilen erişim belirteçlerinin kullanım süresi bir saattir. Web uygulamanızın (kullanıcı + uygulama) yenilemek ihtiyaç duyduğu düşüktür erişim belirteci. Erişim belirtecini yenilemek gerekiyorsa, belirteç yanıt olarak, uygulamanın aldığı yenileme belirteci kullanın. Bir OAuth2.0 belirteci istemek için Azure AD belirteç uç noktası gönderin:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

Yenileme isteği ile kullanılacak parametreleri açıklanan [erişim belirtecini yenileme](../active-directory/develop/v1-protocols-oauth-code.md#refreshing-the-access-tokens).

Aşağıdaki örnek yenileme işlemi belirteci gösterilir:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=refresh_token&refresh_token=AAABAAAAiL9Kn2Z****55j-sjnyYgAA&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Azure Resource Manager için yeni erişim belirteçlerini almak için yenileme belirteçleri kullanılabilir olsa da, uygulamanız tarafından çevrimdışı erişim için uygun değildir. Yenileme belirteçleri ömrü sınırlıdır ve yenileme belirteçleri kullanıcıya bağlıdır. Kullanıcı kuruluştan ayrılırsa, yenileme belirtecini kullanarak uygulama erişimini kaybeder. Bu yaklaşım, takımlar tarafından Azure kaynaklarını yönetmek için kullanılan uygulamalar için uygun değildir.

## <a name="check-if-user-can-assign-access-to-subscription"></a>Kullanıcı aboneliği erişim atamanız durumunda denetleyin
Uygulamanız artık Azure Resource Manager'a erişmek için kullanıcı için bir belirteç sahiptir. Sonraki adım, uygulamanızı aboneliğine bağlamaktır. Kullanıcı mevcut olmadığında bile bağladıktan sonra uygulamanızı bu Aboneliklerdeki yönetebilirsiniz (uzun süreli çevrimdışı erişim).

Bağlanmak her abonelik için çağrı [Kaynak Yöneticisi listeleme izinleri](https://docs.microsoft.com/rest/api/authorization/permissions) kullanıcının abonelik için erişim yönetimi haklarına sahip olup olmadığını belirlemek için API.

[UserCanManagerAccessForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L44) yöntem ASP.NET MVC örnek uygulamanın bu çağrı uygular.

Aşağıdaki örnek, bir abonelikte bir kullanıcının izinleri istemek gösterilmektedir. 83cfe939-2402-4581-b761-4f59b0a041e4, abonelik Kimliğini gösterir.

    GET https://management.azure.com/subscriptions/83cfe939-2402-4581-b761-4f59b0a041e4/providers/microsoft.authorization/permissions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiLC***lwO1mM7Cw6JWtfY2lGc5A

Bir abonelikte kullanıcının izinleri almak için bir yanıt örneği verilmiştir:

    HTTP/1.1 200 OK

    {"value":[{"actions":["*"],"notActions":["Microsoft.Authorization/*/Write","Microsoft.Authorization/*/Delete"]},{"actions":["*/read"],"notActions":[]}]}

API izinleri birden fazla izin verir. Her izin izin verilen eylemleri oluşur (**eylemleri**) ve izin verilmeyen Eylemler (**notactions**). Bir eylem varsa herhangi bir izni, izin verilen eylemleri ve bu izni, izin verilmeyen Eylemler yok ise, kullanıcının bu eylemi gerçekleştirme izni. **Microsoft.Authorization/roleassignments/Write** erişim yönetim haklarını veren eylemdir. Uygulamanız bu eylemi dizesinde normal ifade bir eşleşme aramak için izinleri sonucu ayrıştırması gerekir **eylemleri** ve **notactions** her iznin.

## <a name="get-app-only-access-token"></a>Salt uygulama erişim belirteci alma
Artık, kullanıcının Azure aboneliğine erişim atamanız durumunda biliyorsunuz. Sonraki adımlar şunlardır:

1. Abonelik kimliği, uygulamanızın uygun RBAC rolü atayın.
2. Abonelik uygulamanın izinlerini sorgulama veya Resource Manager'ın salt uygulama belirteci kullanarak erişerek erişim atama doğrulayın.
3. Bağlantının abonelik kimliği kalıcı hale getirmeniz, uygulamaları "bağlı abonelikler" veri yapısında - kaydedin.

İlk adımda daha yakından göz atalım. Uygulamanın kimliğini uygun RBAC rolü atamak için belirlemeniz gerekir:

* Kullanıcının Azure Active Directory'de uygulamanızın kimliğini nesne kimliği
* Abonelik üzerinde uygulamanızın gerektirdiği RBAC rolü tanıtıcısı

Uygulamanızı bir Azure ad kullanıcı kimliği doğruladığında, Azure AD'deki uygulamanız için hizmet sorumlusu nesnesi oluşturur. Azure RBAC rolleri ilgili uygulamaları Azure kaynaklarına doğrudan erişimi vermek için hizmet sorumluları için atanacak sağlar. Bu tam olarak yapmak istediğiniz bir eylemdir. Azure AD hizmet sorumlusu, uygulamanızın oturum açmış kullanıcı tanımlayıcısını belirlemek için Azure AD Graph API'si sorgu güçlendirin.

Azure Resource Manager için bir erişim belirteci yalnızca sahip - Azure AD Graph API'sini çağırmak için yeni bir erişim belirteci gerekir. Azure AD'de her uygulamanın kendi hizmet sorumlusu nesnesi salt uygulama erişim belirteci yeterli olacak şekilde sorgu izni vardır.

<a id="app-azure-ad-graph" />

### <a name="get-app-only-access-token-for-azure-ad-graph-api"></a>Azure AD Graph API'si için salt uygulama erişim belirteci alma

Uygulamanız kimlik doğrulaması ve Azure AD Graph API için bir belirteç almak için Azure AD belirteç uç noktası için bir istemci kimlik bilgileri verme OAuth2.0 akış belirteci isteği sorun (**https:\//login.microsoftonline.com/{directory_domain_name}/OAuth2/Token** ).

[GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs) yöntemi ASP.net MVC örnek uygulamanın alır bir salt uygulama erişim belirteci Graph API'si için .NET için Active Directory Authentication Library kullanarak.

Bu istek için sorgu dizesi parametreleri açıklanan [bir erişim belirteci isteği](../active-directory/develop/v1-oauth2-client-creds-grant-flow.md#request-an-access-token) makalesi.

Bir örnek istek istemci kimlik bilgileri için belirteç verin:

    POST https://login.microsoftonline.com/62e173e9-301e-423e-bcd4-29121ec1aa24/oauth2/token HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 187</pre>
    <pre>grant_type=client_credentials&client_id=a0448380-c346-4f9f-b897-c18733de9394&resource=https%3A%2F%2Fgraph.windows.net%2F &client_secret=olna8C*****Og%3D

Bir istemci kimlik bilgileri için örnek yanıt vermesi belirteci:

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039862","not_before":"1432035962","resource":"https://graph.windows.net/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRv****G5gUTV-kKorR-pg"}

### <a name="get-objectid-of-application-service-principal-in-user-azure-ad"></a>Kullanıcı Azure AD uygulama hizmet sorumlusunun objectID alın
Şimdi, sorgu için salt uygulama erişim belirteci kullanmak [Azure AD Graph hizmet sorumluları](/previous-versions/azure/ad/graph/api/entity-and-complex-type-reference#serviceprincipal-entity) dizinde uygulama hizmet sorumlusu nesne kimliği belirlemek için API.

[GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs#) yöntem ASP.net MVC örnek uygulamanın bu çağrı uygular.

Aşağıdaki örnek, bir uygulamanın hizmet sorumlusu istek gösterilmiştir. a0448380-c346-4f9f-b897-c18733de9394 uygulamanın istemci kimliğidir.

    GET https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/servicePrincipals?api-version=1.5&$filter=appId%20eq%20'a0448380-c346-4f9f-b897-c18733de9394' HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJK*****-kKorR-pg

Aşağıdaki örnek bir uygulama hizmeti için bir isteğe yanıt asıl gösterir

    HTTP/1.1 200 OK

    {"odata.metadata":"https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/$metadata#directoryObjects/Microsoft.DirectoryServices.ServicePrincipal","value":[{"odata.type":"Microsoft.DirectoryServices.ServicePrincipal","objectType":"ServicePrincipal","objectId":"9b5018d4-6951-42ed-8a92-f11ec283ccec","deletionTimestamp":null,"accountEnabled":true,"appDisplayName":"CloudSense","appId":"a0448380-c346-4f9f-b897-c18733de9394","appOwnerTenantId":"62e173e9-301e-423e-bcd4-29121ec1aa24","appRoleAssignmentRequired":false,"appRoles":[],"displayName":"CloudSense","errorUrl":null,"homepage":"http://www.vipswapper.com/cloudsense","keyCredentials":[],"logoutUrl":null,"oauth2Permissions":[{"adminConsentDescription":"Allow the application to access CloudSense on behalf of the signed-in user.","adminConsentDisplayName":"Access CloudSense","id":"b7b7338e-683a-4796-b95e-60c10380de1c","isEnabled":true,"type":"User","userConsentDescription":"Allow the application to access CloudSense on your behalf.","userConsentDisplayName":"Access CloudSense","value":"user_impersonation"}],"passwordCredentials":[],"preferredTokenSigningKeyThumbprint":null,"publisherName":"vipswapper"quot;,"replyUrls":["http://www.vipswapper.com/cloudsense","http://www.vipswapper.com","http://vipswapper.com","http://vipswapper.azurewebsites.net","http://localhost:62080"],"samlMetadataUrl":null,"servicePrincipalNames":["http://www.vipswapper.com/cloudsense","a0448380-c346-4f9f-b897-c18733de9394"],"tags":["WindowsAzureActiveDirectoryIntegratedApp"]}]}

### <a name="get-azure-rbac-role-identifier"></a>Azure RBAC rolü tanımlayıcısı Al
Hizmet sorumlusu uygun RBAC rolü atamak için Azure RBAC rolü tanımlayıcısını belirlemeniz gerekir.

Uygulamanız için doğru RBAC rolü:

* Yalnızca uygulamanızı herhangi bir değişiklik yapmadan abonelik izler, yalnızca abonelik okuyucusu izinleri gerektirir. Ata **okuyucu** rol.
* Uygulamanızı Azure aboneliği, varlıklar oluşturma/değiştirme/silme, yönetiyorsa bir katkıda bulunan izinleri gerektirir.
  * Belirli bir kaynak türünü yönetmek için (sanal makine Katılımcısı, sanal ağ Katılımcısı, depolama hesabı Katılımcısı, vb.) kaynağa özgü katkıda bulunan rol atayın.
  * Herhangi bir kaynak türü yönetmek için Ata **katkıda bulunan** rol.

Uygulamanız için rol ataması seçin en az gereken ayrıcalık olan kullanıcılara görünür olur.

Çağrı [Resource Manager rol tanımı API](https://docs.microsoft.com/rest/api/authorization/roledefinitions) tüm Azure RBAC rollerini listelemek ve ada göre rol tanımı bulmak için sonuç boyunca yineleme yapmak için.

[GetRoleId](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L246) yöntem ASP.net MVC örnek uygulamanın bu çağrı uygular.

Aşağıdaki isteği örnek, Azure RBAC rolü tanımlayıcısını almak gösterilmektedir. 09cbd307-aa71-4aca-b346-5f253e6e3ebb, abonelik Kimliğini gösterir.

    GET https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV*****fY2lGc5

Yanıt aşağıdaki biçimdedir:

    HTTP/1.1 200 OK

    {"value":[{"properties":{"roleName":"API Management Service Contributor","type":"BuiltInRole","description":"Lets you manage API Management services, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.ApiManagement/Services/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/312a565d-c81f-4fd8-895a-4e21e48d571c","type":"Microsoft.Authorization/roleDefinitions","name":"312a565d-c81f-4fd8-895a-4e21e48d571c"},{"properties":{"roleName":"Application Insights Component Contributor","type":"BuiltInRole","description":"Lets you manage Application Insights components, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.Insights/components/*","Microsoft.Insights/webtests/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/ae349356-3a1b-4a5e-921d-050484c6347e","type":"Microsoft.Authorization/roleDefinitions","name":"ae349356-3a1b-4a5e-921d-050484c6347e"}]}

Sürekli olarak bu API'yi çağırması gerekmez. Rol tanımı iyi bilinen GUID saptadıktan sonra rol tanımı kimliği olarak oluşturabilirsiniz:

    /subscriptions/{subscription_id}/providers/Microsoft.Authorization/roleDefinitions/{well-known-role-guid}

Yaygın olarak kullanılan yerleşik roller tanımlayıcıların şunlardır:

| Rol | GUID |
| --- | --- |
| Okuyucu |acdd72a7-3385-48ef-bd42-f606fba81ae7 |
| Katılımcı |b24988ac-6180-42a0-ab88-20f7382dd24c |
| Sanal makine Katılımcısı |d73bb868-a0df-4d4d-bd69-98a00b01fccb |
| Sanal ağ Katılımcısı |b34d265f-36f7-4a0d-a4d4-e158ca92e90f |
| Depolama hesabı Katılımcısı |86e8f5dc-a6e9-4c67-9d15-de283e8eac25 |
| Web sitesi Katılımcısı |de139f84-1756-47ae-9be6-808fbbe84772 |
| Web planı Katılımcısı |2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b |
| SQL Server Katılımcısı |6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437 |
| SQL DB Katılımcısı |9b7fa17d-e63e-47b0-bb0a-15c516ac86ec |

### <a name="assign-rbac-role-to-application"></a>Uygulama için RBAC rolü atayın
Kullanarak hizmet sorumlunuzu uygun RBAC rolü atamak için ihtiyacınız olan her şeye sahip [Resource Manager rol ataması oluşturma](https://docs.microsoft.com/rest/api/authorization/roleassignments) API.

[GrantRoleToServicePrincipalOnSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L170) yöntem ASP.net MVC örnek uygulamanın bu çağrı uygular.

Uygulama için RBAC rolü atamak için bir örnek istek:

    PUT https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/microsoft.authorization/roleassignments/4f87261d-2816-465d-8311-70a27558df4c?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiL*****FlwO1mM7Cw6JWtfY2lGc5
    Content-Type: application/json
    Content-Length: 230

    {"properties": {"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2"}}

İstekte, aşağıdaki değerler kullanılır:

| Guid | Açıklama |
| --- | --- |
| 09cbd307-aa71-4aca-b346-5f253e6e3ebb |Abonelik kimliği |
| c3097b31-7309-4c59-b4e3-770f8406bad2 |uygulama hizmet sorumlusu nesne kimliği |
| b24988ac-6180-42a0-ab88-20f7382dd24c |katkıda bulunan rolü kimliği |
| 4f87261d-2816-465d-8311-70a27558df4c |Yeni rol ataması için oluşturulan yeni bir GUID |

Yanıt aşağıdaki biçimdedir:

    HTTP/1.1 201 Created

    {"properties":{"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2","scope":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb"},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleAssignments/4f87261d-2816-465d-8311-70a27558df4c","type":"Microsoft.Authorization/roleAssignments","name":"4f87261d-2816-465d-8311-70a27558df4c"}

### <a name="get-app-only-access-token-for-azure-resource-manager"></a>Azure Resource Manager için salt uygulama erişim belirteci alma
Bu uygulama doğrulamak için aboneliğe erişmesine, bir sınama bir salt uygulama belirteci kullanan abonelik görevi.

Salt uygulama erişim belirteci almak için bölümündeki yönergeleri izleyin [salt uygulama erişim belirteci alın Azure AD Graph API'si için](#app-azure-ad-graph), kaynak parametresi için farklı bir değerle:

    https://management.core.windows.net/

[ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) yöntemi ASP.NET MVC örnek uygulamanın alır bir salt uygulama erişim belirteci Azure Resource Manager için .net için Active Directory Authentication Library kullanarak.

#### <a name="get-applications-permissions-on-subscription"></a>Uygulama izinleri abonelikte alın
Uygulamanızı bir Azure aboneliği erişebilir, ayrıca çağırabilir denetlenecek [kaynak yöneticisi izinleri](https://docs.microsoft.com/rest/api/authorization/permissions) API. Bu yaklaşım, kullanıcının abonelik için erişim yönetimi haklarına sahip olup olmadığını belirleme için benzerdir. Ancak, bu kez, önceki adımda aldığınız salt uygulama erişim belirteci ile API izinleri çağırın.

[ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) yöntem ASP.NET MVC örnek uygulamanın bu çağrı uygular.

## <a name="manage-connected-subscriptions"></a>Bağlı aboneliklerini yönetme
Uygun RBAC rolü, uygulamanızın hizmet üzerinde bir abonelik sorumlusu atandığında, uygulamanızın izleme/Azure Resource Manager için salt uygulama erişim belirteçleri kullanarak yönetme tutun.

Abonelik sahibi portalı veya komut satırı araçlarını kullanarak, uygulamanızın rol ataması kaldırırsa, uygulamanız artık bu aboneliğe erişmek mümkün değildir. Bu durumda, bağlantı aboneliği ile gelen uygulama dışında yazıyordunuz kullanıcıyı bilgilendirir ve "Bağlantıyı Onar" seçeneğini vermediğiniz gerekir. "Onarım" Çevrimdışı silinen rol atamasını yeniden oluşturacak.

Kullanıcı abonelikleri uygulamanızı bağlamak etkinleştirdiğiniz gibi kullanıcı aboneliklerini çok bağlantısını kesmek izin vermeniz gerekir. Bir erişim yönetimi açısından bakıldığında, yöntem uygulamanın hizmet sorumlusu, abonelikte sahip rol atamasını kaldırma kesin. İsteğe bağlı olarak, abonelik için bir uygulamadaki herhangi bir durumu çok kaldırılabilir.
Yalnızca abonelik üzerinde erişim yönetimi izne sahip kullanıcılar, abonelik kesebilirsiniz.

[RevokeRoleFromServicePrincipalOnSubscription yöntemi](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L200) ASP.net MVC örnek uygulama bu çağrıyı uygular.

İşte bu kadar - kullanıcılar artık kolayca bağlanabilir ve uygulamanız ile Azure aboneliklerini yönetin.
