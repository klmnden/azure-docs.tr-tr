---
title: "Azure Active Directory kimlik doğrulaması ve Resource Manager | Microsoft Docs"
description: "Bir uygulama başka Azure abonelikleri ile tümleştirmek için Azure Kaynak Yöneticisi API'si ve Azure Active Directory ile kimlik doğrulaması için bir Geliştirici Kılavuzu."
services: azure-resource-manager,active-directory
documentationcenter: na
author: dushyantgill
manager: timlt
editor: tysonn
ms.assetid: 17b2b40d-bf42-4c7d-9a88-9938409c5088
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/27/2016
ms.author: dugill;tomfitz
ms.openlocfilehash: 7830dc4774652f4d108e98660dce3bcea7b32d05
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="use-resource-manager-authentication-api-to-access-subscriptions"></a>Erişim abonelikler için kaynak yöneticisi kimlik doğrulaması API'sini kullanın
## <a name="introduction"></a>Giriş
Müşterinin Azure kaynaklarını yöneten bir uygulama oluşturmak için gereken bir yazılım geliştirici varsa, bu konuda, Azure Resource Manager API'leri ile kimlik doğrulaması ve diğer abonelikler kaynaklarına erişim kazanmak nasıl gösterir.

Uygulamanızı çeşitli şekillerde Resource Manager API'leri erişebilirsiniz:

1. **Kullanıcı + uygulama erişimi**: oturum açmış bir kullanıcı adına kaynaklara uygulamalar için. Bu yaklaşım, web uygulamaları ve yalnızca "Etkileşimli Yönetimi" Azure kaynakları ile ilgili komut satırı araçları gibi uygulamalar için çalışır.
2. **Yalnızca uygulama erişim**: arka plan programı Hizmetleri ve zamanlanmış işler çalışan uygulamalar için. Uygulamanın kimlik kaynaklarına doğrudan erişimi verilir. Bu yaklaşım, Azure uzun vadeli gözetimsiz (katılımsız) erişimi olması gereken uygulamalar için çalışır.

Bu konu, her iki yetkilendirme yöntemi kullanan bir uygulama oluşturmak için adım adım yönergeler sağlar. REST API veya C# ile her adımı gerçekleştirmek nasıl gösterir. Tam bir ASP.NET MVC uygulaması şu adresten edinilebilir [https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense](https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense).

## <a name="what-the-web-app-does"></a>Web uygulaması yaptığı
Web uygulaması:

1. Bir Azure kullanıcı oturum açtığında.
2. Kaynak Yöneticisi için web uygulaması erişimi vermek için kullanıcıya sorar.
3. Resource Manager erişmek için kullanıcı + uygulama erişim belirtecini alır.
4. Resource Manager çağırın ve uygulamanın hizmet sorumlusu Abonelikteki aboneliğine uygulama uzun vadeli erişim sağlayan bir rol atamak için belirtecinden (3. adım) kullanır.
5. Yalnızca uygulama erişim belirtecini alır.
6. Kaynak Yöneticisi'ni abonelik alanındaki kaynakları yönetmek için belirtecinden (5. adım) kullanır.

Web uygulamasının uçtan uca akışı aşağıda verilmiştir.

![Kaynak Yöneticisi kimlik doğrulama akışı](./media/resource-manager-api-authentication/Auth-Swim-Lane.png)

Bir kullanıcı olarak kullanmak istediğiniz aboneliği için abonelik kimliğini sağlar:

![Abonelik kimliği sağlayın](./media/resource-manager-api-authentication/sample-ux-1.png)

Oturum açma için kullanılacak hesabı seçin.

![hesabı seçin](./media/resource-manager-api-authentication/sample-ux-2.png)

Kimlik bilgilerinizi sağlayın.

![kimlik bilgilerini sağlayın](./media/resource-manager-api-authentication/sample-ux-3.png)

Azure aboneliklerinize uygulama erişimi verin:

![Erişim verme](./media/resource-manager-api-authentication/sample-ux-4.png)

Bağlı aboneliklerinizi yönetin:

![Abonelik Bağlan](./media/resource-manager-api-authentication/sample-ux-7.png)

## <a name="register-application"></a>Uygulamayı Kaydet
Kodlama başlamadan önce web uygulamanızı Azure Active Directory (AD ile) kaydedin. Uygulama Kayıt Merkezi Kimliği uygulamanız için Azure AD içinde oluşturur. Uygulamanız OAuth istemci kimliği, yanıt URL'leri ve uygulamanızın kimlik doğrulaması ve Azure Resource Manager API'leri erişmek için kullandığı kimlik bilgileri gibi hakkındaki temel bilgileri tutar. Uygulama kaydı ayrıca Microsoft APIs kullanıcı adına erişirken uygulamanız gereken çeşitli izinlere temsilci kaydeder.

Uygulamanızı diğer abonelik eriştiği için bir çok kiracılı uygulama olarak yapılandırmanız gerekir. Doğrulama geçirmek için Azure Active Directory ile ilişkilendirilmiş bir etki alanı sağlar. Azure Active Directory ile ilişkili etki alanları görmek için oturum [Klasik portal](https://manage.windowsazure.com). Azure Active Directory'yi seçin ve ardından **etki alanları**.

Aşağıdaki örnek, Azure PowerShell kullanarak uygulamayı kaydedin gösterilmektedir. Bu komutun çalışması Azure PowerShell'in en son sürümünü (Ağustos 2016) olması gerekir.

    $app = New-AzureRmADApplication -DisplayName "{app name}" -HomePage "https://{your domain}/{app name}" -IdentifierUris "https://{your domain}/{app name}" -Password "{your password}" -AvailableToOtherTenants $true

AD uygulaması olarak oturum açmak için uygulama kimliği ve parola gerekir. Önceki komutu döndürülen uygulama kimliği görmek için kullanın:

    $app.ApplicationId

Aşağıdaki örnek, Azure CLI kullanarak uygulamanızı kaydetmeniz gösterilmektedir.

    azure ad app create --name {app name} --home-page https://{your domain}/{app name} --identifier-uris https://{your domain}/{app name} --password {your password} --available true

Sonuç olarak uygulamadan doğrulanırken ihtiyacınız AppID içerir.

### <a name="optional-configuration---certificate-credential"></a>İsteğe bağlı yapılandırma - sertifika kimlik bilgisi
Azure AD uygulamaları için de sertifika kimlik bilgileri destekler: otomatik olarak imzalanan sertifika oluştur, özel anahtarı tutmak ve Azure AD uygulama kaydınızı ortak anahtarı ekleyin. Kimlik doğrulaması, uygulamanızın küçük bir yükü özel anahtarınızı kullanarak imzalanmış Azure AD ile gönderir ve Azure AD kaydettiğiniz ortak anahtar kullanarak imzayı doğrular.

Bir sertifika ile AD uygulaması oluşturma hakkında daha fazla bilgi için bkz: [kaynaklara erişmek için bir hizmet sorumlusu oluşturmak için kullanım Azure PowerShell](resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority) veya [kaynaklara erişmek için bir hizmet sorumlusu oluşturmak için kullanım Azure CLI](resource-group-authenticate-service-principal-cli.md#create-service-principal-with-certificate) .

## <a name="get-tenant-id-from-subscription-id"></a>Abonelik kimliği Kiracı kimliğinizi alma
Resource Manager çağırmak için kullanılabilecek bir belirteç istemek için uygulamanızı Azure aboneliği barındıran Azure AD kiracısı Kiracı kimliği bilmek ister. Büyük olasılıkla, kullanıcılarınızın abonelik kimlikleri biliyorum, ancak bunlar kendi Kiracı kimlikleri için Azure Active Directory anlamayabilirsiniz. Kullanıcının Kiracı Kimliği almak için abonelik kimliği için kullanıcı isteyin. Bu abonelik kimliği, abonelik ilgili bir istek gönderirken sağlar:

    https://management.azure.com/subscriptions/{subscription-id}?api-version=2015-01-01

Kullanıcı henüz oturum açtıktan değil, ancak Kiracı kimliği gelen yanıt almak için isteği başarısız olur. Bu durum, yanıt üstbilgi değeri Kiracı Kimliği almak **WWW-Authenticate**. Bu uygulamasında gördüğünüz [GetDirectoryForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L20) yöntemi.

## <a name="get-user--app-access-token"></a>Kullanıcı + uygulama erişim belirteci alın
Uygulamanızı Azure AD ile bir OAuth 2.0 yetkilendirme kullanıcının kimlik bilgilerini doğrulamak ve bir kimlik doğrulama kodu geri alma isteği - kullanıcı yönlendirir. Uygulamanız, kaynak yöneticisi için bir erişim belirteci almak için yetki kodunu kullanır. [ConnectSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/Controllers/HomeController.cs#L42) yöntemi, yetkilendirme isteği oluşturur.

Bu konu, kullanıcının kimliğini doğrulamak için REST API istekleri gösterir. Kodunuzda kimlik doğrulaması yapmak için yardımcı kitaplıkları da kullanabilirsiniz. Bu kitaplıklar hakkında daha fazla bilgi için bkz: [Azure Active Directory kimlik doğrulama kitaplıkları](../active-directory/active-directory-authentication-libraries.md). Kimlik Yönetimi uygulamada tümleştirme ile ilgili yönergeler için bkz: [Azure Active Directory Geliştirici Kılavuzu](../active-directory/active-directory-developers-guide.md).

### <a name="auth-request-oauth-20"></a>Kimlik doğrulama isteği (OAuth 2.0)
Bir açık Bağlan/OAuth2.0 yetkilendirmek istek kimliği için Azure AD Authorize son noktası yürütün:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize

Bu istek için sorgu dizesi parametreleri açıklanan [bir kimlik doğrulama kodu isteme](../active-directory/develop/active-directory-protocols-oauth-code.md#request-an-authorization-code) konu.

Aşağıdaki örnek, OAuth2.0 yetkilendirme isteği gösterilmektedir:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=query&response_type=code&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&domain_hint=live.com

Azure AD kullanıcının kimliğini doğrular ve gerekirse, uygulama izni vermek için kullanıcıya sorar. Bu, uygulamanızın yanıt URL'si için yetkilendirme kodu döndürür. Azure AD istenen response_mode ya da bağlı olarak geri verileri sorgu dizesi veya gönderme verisi olarak gönderir.

    code=AAABAAAAiL****FDMZBUwZ8eCAA&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="auth-request-open-id-connect"></a>Kimlik doğrulama isteği (Open ID Connect)
Bir açık bağlanma yetkisi istek kimliği yalnızca Azure kaynak yöneticisi kullanıcı adına erişmek istediğiniz, ancak Ayrıca, uygulamanız kendi Azure AD hesabı kullanarak oturum açmak kullanıcının izin verin. Open ID Connect ile uygulamanızı uygulamanızı kullanıcıyla oturum açmak için kullanabileceğiniz Azure AD'den bir id_token de alır.

Bu istek için sorgu dizesi parametreleri açıklanan [oturum açma isteği Gönder](../active-directory/develop/active-directory-protocols-openid-connect-code.md#send-the-sign-in-request) konu.

Bir örnek Open ID Connect isteğidir:

     https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=form_post&response_type=code+id_token&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&scope=openid+profile&nonce=63567Dc4MDAw&domain_hint=live.com&state=M_12tMyKaM8

Azure AD kullanıcının kimliğini doğrular ve gerekirse, uygulama izni vermek için kullanıcıya sorar. Bu, uygulamanızın yanıt URL'si için yetkilendirme kodu döndürür. Azure AD istenen response_mode ya da bağlı olarak geri verileri sorgu dizesi veya gönderme verisi olarak gönderir.

Open ID Connect yanıt örneğidir:

    code=AAABAAAAiL*****I4rDWd7zXsH6WUjlkIEQxIAA&id_token=eyJ0eXAiOiJKV1Q*****T3GrzzSFxg&state=M_12tMyKaM8&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="token-request-oauth20-code-grant-flow"></a>Belirteç isteği (OAuth2.0 kod Grant akış)
Uygulamanızı Azure AD'den yetkilendirme kodu aldı, Azure kaynak yöneticisi için erişim belirteci almak için zaman yapılır.  Bir OAuth2.0 kod Grant belirteç isteği Azure AD belirteç uç noktası gönderin:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

Bu istek için sorgu dizesi parametreleri açıklanan [yetkilendirme kodu kullanın](../active-directory/develop/active-directory-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token) konu.

Aşağıdaki örnek kod grant belirteci parola kimlik bilgisi için bir istek gösterir:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Sertifika kimlik bilgileri ile çalışırken, bir JSON Web Token (JWT) ve uygulamanızın sertifika kimlik bilgisi özel anahtarı kullanılarak oturum (RSA SHA256) oluşturun. Belirteç için talep türleri gösterilmektedir [JWT belirteci taleplerini](../active-directory/develop/active-directory-protocols-oauth-code.md#jwt-token-claims). Başvuru için bkz: [Active Directory kimlik doğrulama kitaplığı (.NET) kod](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/blob/dev/src/ADAL.PCL.Desktop/CryptographyHelper.cs) istemci onaylama JWT Belirteçleri imzalamak için.

Bkz: [Open ID Connect spec](http://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication) istemci kimlik doğrulaması hakkında ayrıntılı bilgi için.

Aşağıdaki örnek kod grant belirteciyle sertifika kimlik bilgisi için bir istek gösterir:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbG*****Y9cYo8nEjMyA

Bir örnek yanıt kodu için belirteç verin:

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039858","not_before":"1432035958","resource":"https://management.core.windows.net/","access_token":"eyJ0eXAiOiJKV1Q****M7Cw6JWtfY2lGc5A","refresh_token":"AAABAAAAiL9Kn2Z****55j-sjnyYgAA","scope":"user_impersonation","id_token":"eyJ0eXAiOiJKV*****-drP1J3P-HnHi9Rr46kGZnukEBH4dsg"}

#### <a name="handle-code-grant-token-response"></a>Kod grant belirteç yanıtı işlemek
Başarılı bir token yanıt içerir (kullanıcı + uygulama) erişim belirteci için Azure Resource Manager. Uygulamanız bu erişim belirteci kullanıcı adına Kaynak Yöneticisi'ne erişmek için kullanır. Azure AD tarafından verilen erişim belirteçleri süresi bir saattir. Web uygulamanız (kullanıcı + uygulama) yenilemek için ihtiyaç duyduğu düşüktür erişim belirteci. Erişim belirteci yenilemeye gerektiriyorsa, uygulamanızın içinde belirteç yanıtı alır yenileme belirteci kullanın. Bir OAuth2.0 belirteci isteği Azure AD belirteç uç noktası gönderin:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

İle yenileme isteği kullanılacak parametreleri açıklanan [erişim belirtecini yenilemeyi](../active-directory/develop/active-directory-protocols-oauth-code.md#refreshing-the-access-tokens).

Aşağıdaki örnek yenilemeyi kullanmak belirteci gösterilmektedir:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=refresh_token&refresh_token=AAABAAAAiL9Kn2Z****55j-sjnyYgAA&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Yenileme belirteçleri, Azure Resource Manager için yeni erişim belirteçleri almak için kullanılır ancak bunlar uygulamanızı çevrimdışı erişmek için uygun değildir. Yenileme belirteçleri ömrü sınırlıdır ve yenileme belirteçleri kullanıcıya bağlıdır. Kullanıcı kuruluş ayrılsa yenileme belirtecini kullanarak uygulama erişim kaybeder. Bu yaklaşım ekipleri tarafından Azure kaynaklarını yönetmek için kullanılan uygulamalar için uygun değil.

## <a name="check-if-user-can-assign-access-to-subscription"></a>Kullanıcı aboneliğe erişimi atayın olmadığını denetleyin
Uygulamanız artık Azure Resource Manager kullanıcı adına erişmek için bir belirteç sahiptir. Sonraki adım, abonelik uygulamanızı bağlamaktır. Kullanıcı mevcut değilse bile bağladıktan sonra uygulamanızı bu abonelikleri yönetebilirsiniz (uzun süreli çevrimdışı erişim).

Bağlanmak her abonelik için çağrı [Resource Manager liste izinlerini](https://docs.microsoft.com/rest/api/authorization/permissions) kullanıcı abonelik için erişim yönetim haklarına sahip olup olmadığını belirlemek için API.

[UserCanManagerAccessForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L44) yöntemi ASP.NET MVC örnek uygulaması, bu çağrıyı uygular.

Aşağıdaki örnek, bir abonelik bir kullanıcının izinleri istemek gösterilmiştir. 83cfe939-2402-4581-b761-4f59b0a041e4 abonelik kimliğini gösterir.

    GET https://management.azure.com/subscriptions/83cfe939-2402-4581-b761-4f59b0a041e4/providers/microsoft.authorization/permissions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiLC***lwO1mM7Cw6JWtfY2lGc5A

Abonelik üzerinde kullanıcı izinlerini almak için yanıt örneğidir:

    HTTP/1.1 200 OK

    {"value":[{"actions":["*"],"notActions":["Microsoft.Authorization/*/Write","Microsoft.Authorization/*/Delete"]},{"actions":["*/read"],"notActions":[]}]}

API izinleri birden çok izin verir. Her izin izin verilen eylemleri (Eylemler) oluşur ve eylemleri (notactions) izin verilmiyor. Bir eylem tüm izinleri izin verilen eylemleri listesinde mevcut ve bu izni notactions listesinde yok olduğunda, kullanıcının bu eylemi gerçekleştirmek için izin verilir. **Microsoft.Authorization/roleassignments/Write** , erişim yönetim hakları veren eylemdir. Uygulamanız bu eylem dizesi eylemleri ve her izin notactions bir regex eşleşme aramak için izinleri sonuç ayrıştırma gerekir.

## <a name="get-app-only-access-token"></a>Yalnızca uygulama erişim belirteci alma
Artık, kullanıcının Azure aboneliğine erişimi atayın, biliyorsunuz. Sonraki adımlar şunlardır:

1. Uygulamanızın kimlik aboneliğe ilişkin uygun RBAC rolü atayın.
2. Erişim atama abonelik uygulamanın izni sorgulanırken veya Kaynak Yöneticisi'ni yalnızca uygulama belirteci kullanarak erişerek doğrulayın.
3. Bağlantının abonelik kimliği sürdürmek, uygulamaları "bağlı abonelikleri" veri yapısında - kaydedin.

İlk adımda daha yakın olarak bakalım. Uygulamanın kimliğini uygun RBAC rolü atamak için belirlemeniz gerekir:

* Uygulamanızın kimlik kullanıcının Azure Active Directory'de nesne kimliği
* Abonelikte uygulamanızın gerektirdiği RBAC rolü tanıtıcısı

Uygulamanız bir Azure AD'den bir kullanıcı kimliği doğruladığında, Azure AD'de, uygulamanız için bir hizmet sorumlusu nesnesi oluşturur. Azure hizmet asıl adı için karşılık gelen uygulamaları Azure kaynaklarına doğrudan erişim vermek üzere atanacak RBAC rolleri sağlar. Bu tam olarak yapmak istediğimiz eylemdir. Azure AD hizmet sorumlusu oturum açmış kullanıcı, uygulamanızın tanıtıcısı belirlemek için Azure AD Graph API sorgu kullanıcının.

Azure kaynak yöneticisi için yalnızca bir erişim belirteci sahip - Azure AD grafik API'si yi çağırmak için yeni bir erişim belirteci gerekir. Azure AD her uygulamada yalnızca uygulama erişim belirteci yeterli olacak şekilde, kendi hizmet sorumlusu nesnesi sorgulama izni vardır.

<a id="app-azure-ad-graph" />

### <a name="get-app-only-access-token-for-azure-ad-graph-api"></a>Yalnızca uygulama erişimi için Azure AD Graph API belirteci alma
Uygulamanıza kimlik doğrulaması ve Azure AD grafik API'si için bir belirteç almak için Azure AD belirteç uç noktası için bir istemci kimlik bilgileri verin OAuth2.0 akış belirteç isteği gönderin (**https://login.microsoftonline.com/ {directory_domain_name} / OAuth2/Token**).

[GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs) yöntemi ASP.net MVC örnek uygulamasının alır yalnızca uygulama erişim belirteci grafik API'si için Active Directory kimlik doğrulama kitaplığı .NET için kullanma.

Bu istek için sorgu dizesi parametreleri açıklanan [bir erişim belirteci isteği](../active-directory/develop/active-directory-protocols-oauth-service-to-service.md#request-an-access-token) konu.

İstemci kimlik bilgileri için bir örnek isteği belirteci verin:

    POST https://login.microsoftonline.com/62e173e9-301e-423e-bcd4-29121ec1aa24/oauth2/token HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 187</pre>
    <pre>grant_type=client_credentials&client_id=a0448380-c346-4f9f-b897-c18733de9394&resource=https%3A%2F%2Fgraph.windows.net%2F &client_secret=olna8C*****Og%3D

İstemci kimlik bilgisi için bir örnek yanıt belirteç verin:

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039862","not_before":"1432035962","resource":"https://graph.windows.net/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRv****G5gUTV-kKorR-pg"}

### <a name="get-objectid-of-application-service-principal-in-user-azure-ad"></a>Kullanıcı Azure AD uygulama hizmet sorumlusu objectID alın
Şimdi, sorgu için yalnızca uygulama erişim belirtecini kullanır [Azure AD grafik hizmet asıl adı](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) dizininde uygulamanın hizmet sorumlusu nesnesi kimliğini belirlemek için API.

[GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs#) yöntemi ASP.net MVC örnek uygulamasının bu çağrıyı uygular.

Aşağıdaki örnek, bir uygulamanın hizmet asıl istek gösterilmektedir. a0448380-c346-4f9f-b897-c18733de9394 uygulama istemci kimliğini gösterir.

    GET https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/servicePrincipals?api-version=1.5&$filter=appId%20eq%20'a0448380-c346-4f9f-b897-c18733de9394' HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJK*****-kKorR-pg

Aşağıdaki örnek, bir uygulamanın hizmet isteğine yanıt asıl gösterir

    HTTP/1.1 200 OK

    {"odata.metadata":"https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/$metadata#directoryObjects/Microsoft.DirectoryServices.ServicePrincipal","value":[{"odata.type":"Microsoft.DirectoryServices.ServicePrincipal","objectType":"ServicePrincipal","objectId":"9b5018d4-6951-42ed-8a92-f11ec283ccec","deletionTimestamp":null,"accountEnabled":true,"appDisplayName":"CloudSense","appId":"a0448380-c346-4f9f-b897-c18733de9394","appOwnerTenantId":"62e173e9-301e-423e-bcd4-29121ec1aa24","appRoleAssignmentRequired":false,"appRoles":[],"displayName":"CloudSense","errorUrl":null,"homepage":"http://www.vipswapper.com/cloudsense","keyCredentials":[],"logoutUrl":null,"oauth2Permissions":[{"adminConsentDescription":"Allow the application to access CloudSense on behalf of the signed-in user.","adminConsentDisplayName":"Access CloudSense","id":"b7b7338e-683a-4796-b95e-60c10380de1c","isEnabled":true,"type":"User","userConsentDescription":"Allow the application to access CloudSense on your behalf.","userConsentDisplayName":"Access CloudSense","value":"user_impersonation"}],"passwordCredentials":[],"preferredTokenSigningKeyThumbprint":null,"publisherName":"vipswapper"quot;,"replyUrls":["http://www.vipswapper.com/cloudsense","http://www.vipswapper.com","http://vipswapper.com","http://vipswapper.azurewebsites.net","http://localhost:62080"],"samlMetadataUrl":null,"servicePrincipalNames":["http://www.vipswapper.com/cloudsense","a0448380-c346-4f9f-b897-c18733de9394"],"tags":["WindowsAzureActiveDirectoryIntegratedApp"]}]}

### <a name="get-azure-rbac-role-identifier"></a>Azure RBAC rolü tanımlayıcısını alın
Hizmet sorumlusu uygun RBAC rolü atamak için Azure RBAC rolü tanıtıcısı belirlemeniz gerekir.

Uygulamanız için uygun RBAC rolü:

* Yalnızca uygulamanızı herhangi bir değişiklik yapmadan abonelik izler, yalnızca abonelik okuyucusu izinleri gerektirir. Ata **okuyucu** rol.
* Uygulamanızı Azure varlıkları oluşturma/değiştirme/silme, abonelik yönetiyorsa katkıda bulunan izinleri birini gerektirir.
  * Belirli bir kaynak türünü yönetmek için kaynak özgü katkıda bulunan rollerinin (sanal makine Katılımcısı, sanal ağ Katılımcısı, depolama hesabı katkıda bulunan, vb.) atayın.
  * Herhangi bir kaynak türü yönetmek için Ata **katkıda bulunan** rol.

Uygulamanız için rol ataması, böylece select en az gereken ayrıcalık kullanıcılara görünür olur.

Çağrı [Kaynağı Yöneticisi rol tanımı API](https://docs.microsoft.com/rest/api/authorization/roledefinitions) tüm Azure RBAC rolleri ve arama listesi sonra istenen rol tanımı ada göre bulmak için sonuç üzerinden yineleme.

[GetRoleId](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L246) yöntemi ASP.net MVC örnek uygulaması, bu çağrıyı uygular.

Aşağıdaki isteği örnek Azure RBAC rolü tanımlayıcı alma gösterir. 09cbd307-aa71-4aca-b346-5f253e6e3ebb abonelik kimliğini gösterir.

    GET https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV*****fY2lGc5

Yanıt şu biçimdedir:

    HTTP/1.1 200 OK

    {"value":[{"properties":{"roleName":"API Management Service Contributor","type":"BuiltInRole","description":"Lets you manage API Management services, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.ApiManagement/Services/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/312a565d-c81f-4fd8-895a-4e21e48d571c","type":"Microsoft.Authorization/roleDefinitions","name":"312a565d-c81f-4fd8-895a-4e21e48d571c"},{"properties":{"roleName":"Application Insights Component Contributor","type":"BuiltInRole","description":"Lets you manage Application Insights components, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.Insights/components/*","Microsoft.Insights/webtests/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/ae349356-3a1b-4a5e-921d-050484c6347e","type":"Microsoft.Authorization/roleDefinitions","name":"ae349356-3a1b-4a5e-921d-050484c6347e"}]}

Sürekli olarak bu API çağrısı gerekmez. Rol tanımı iyi bilinen GUID saptadıktan sonra rol tanımı kimliği olarak oluşturabileceğiniz:

    /subscriptions/{subscription_id}/providers/Microsoft.Authorization/roleDefinitions/{well-known-role-guid}

Yaygın olarak kullanılan yerleşik roller iyi bilinen GUID'lerini şunlardır:

| Rol | GUID |
| --- | --- |
| Okuyucu |acdd72a7-3385-48EF-bd42-f606fba81ae7 |
| Katılımcı |b24988ac-6180-42a0-ab88-20f7382dd24c |
| Sanal makine Katılımcısı |d73bb868-a0df-4d4d-bd69-98a00b01fccb |
| Sanal ağ Katılımcısı |b34d265f-36f7-4a0d-a4d4-e158ca92e90f |
| Depolama hesabı katkıda bulunan |86e8f5dc-a6e9-4c67-9d15-de283e8eac25 |
| Web sitesi katkıda bulunan |de139f84-1756-47ae-9be6-808fbbe84772 |
| Web planı katkıda bulunan |2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b |
| SQL Server katkıda bulunan |6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437 |
| SQL DB Katılımcısı |9b7fa17d-e63e-47b0-bb0a-15c516ac86ec |

### <a name="assign-rbac-role-to-application"></a>Uygulama için RBAC rolü atama
Kullanarak hizmet sorumlusuna uygun RBAC rolü atamak için ihtiyaç duyduğunuz her şeyi sahip [Kaynağı Yöneticisi rol ataması oluşturma](https://docs.microsoft.com/rest/api/authorization/roleassignments) API.

[GrantRoleToServicePrincipalOnSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L170) yöntemi ASP.net MVC örnek uygulaması, bu çağrıyı uygular.

Uygulama için RBAC rolü atamak için bir örnek isteği:

    PUT https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/microsoft.authorization/roleassignments/4f87261d-2816-465d-8311-70a27558df4c?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiL*****FlwO1mM7Cw6JWtfY2lGc5
    Content-Type: application/json
    Content-Length: 230

    {"properties": {"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2"}}

İstekte aşağıdaki değerler kullanılır:

| GUID | Açıklama |
| --- | --- |
| 09cbd307-aa71-4aca-b346-5f253e6e3ebb |Abonelik kimliği |
| c3097b31-7309-4c59-b4e3-770f8406bad2 |uygulamanın hizmet asıl nesne kimliği |
| acdd72a7-3385-48EF-bd42-f606fba81ae7 |Okuyucu rolü kimliği |
| 4f87261d-2816-465D-8311-70a27558df4c |Yeni rol ataması için oluşturulan yeni bir GUID |

Yanıt şu biçimdedir:

    HTTP/1.1 201 Created

    {"properties":{"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2","scope":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb"},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleAssignments/4f87261d-2816-465d-8311-70a27558df4c","type":"Microsoft.Authorization/roleAssignments","name":"4f87261d-2816-465d-8311-70a27558df4c"}

### <a name="get-app-only-access-token-for-azure-resource-manager"></a>Azure kaynak yöneticisi için yalnızca uygulama erişim belirteci alma
Bu uygulama doğrulamak için Abonelik üzerinde erişim, yalnızca uygulama belirteci kullanarak abonelikte test görevi gerçekleştirmek istediğiniz sahiptir.

Yalnızca uygulama erişim belirteci almak için bölümündeki yönergeleri izleyin [almak yalnızca uygulama erişim belirteci için Azure AD Graph API](#app-azure-ad-graph), kaynak parametresi için farklı bir değerle:

    https://management.core.windows.net/

[ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) yöntemi ASP.NET MVC örnek uygulamasının alır yalnızca uygulama erişim belirteci Azure Resource Manager için Active Directory kimlik doğrulama kitaplığı .net için kullanma.

#### <a name="get-applications-permissions-on-subscription"></a>Uygulama izinleri abonelikte Al
Uygulamanız bir Azure aboneliğine yönelik istenen erişime sahip olduğunu denetlemek için aynı zamanda çağırabilir [kaynak yöneticisi izinleri](https://docs.microsoft.com/rest/api/authorization/permissions) API. Bu yaklaşım, kullanıcının abonelik için erişim yönetim haklarına sahip olup olmadığını belirleme için benzer. Ancak, bu süre, önceki adımda aldığınız yalnızca uygulama erişim belirtecini izinlerle API çağrısı.

[ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) yöntemi ASP.NET MVC örnek uygulaması, bu çağrıyı uygular.

## <a name="manage-connected-subscriptions"></a>Bağlı aboneliklerini yönetme
Uygulamanızın hizmet üzerinde bir abonelik sorumlusu için uygun RBAC rolü atandığında, uygulamanızı izleme/Azure kaynak yöneticisi için yalnızca uygulama erişim belirteçleri kullanarak yönetme tutun.

Abonelik sahibi Klasik Portalı'nı veya komut satırı araçlarını kullanarak, uygulamanızın rol ataması kaldırırsa, uygulamanız bu abonelik erişebilir. Bu durumda, abonelik ile bağlantı uygulaması dışında koptu kullanıcıya bildir ve "Bağlantıyı Onar" seçeneğini verin. "Onar" yalnızca çevrimdışı silindi rol ataması yeniden oluşturur.

Abonelikler, uygulamaya bağlanmak kullanıcı yalnızca etkin olarak abonelikleri çok bağlantısını kesmek kullanıcı izin vermesi gerekir. Bir erişim yönetimi açısından bakıldığında, uygulamanın hizmet sorumlusu abonelikte sahip rol atamasının kaldırılması anlamına gelir bağlantısını kesin. İsteğe bağlı olarak, abonelik için uygulamada herhangi bir durum çok kaldırılmış olabilir.
Yalnızca abonelik erişim yönetim izni olan kullanıcılar abonelik bağlantısını kesmek kullanabilirsiniz.

[RevokeRoleFromServicePrincipalOnSubscription yöntemi](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L200) ASP.net MVC örnek uygulama bu çağrıyı uygular.

İşte bu kadar - kullanıcılar artık kolayca bağlanabilir ve uygulamanız ile bunların Azure Aboneliklerini yönetmek.
