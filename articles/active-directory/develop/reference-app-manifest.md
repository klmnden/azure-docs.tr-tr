---
title: Azure Active Directory Uygulama bildirimini anlama | Microsoft Docs
description: Azure AD kiracısında bir uygulamanın kimlik yapılandırmasını temsil eder ve OAuth yetkilendirme, onayı deneyimi ve diğer kolaylaştırmak için kullanılan Azure Active Directory Uygulama bildirimini ayrıntılı kapsamını.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 4804f3d4-0ff1-4280-b663-f8f10d54d184
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/18/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: sureshja
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0e07e371afaa239ca423f4266557cd2f55aa3a55
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59495267"
---
# <a name="azure-active-directory-app-manifest"></a>Azure Active Directory Uygulama bildirimi

Uygulama bildirimi, Microsoft kimlik platformu uygulama nesnenin tüm öznitelikleri bir tanım içeriyor. Ayrıca uygulama nesnesi güncelleştirmeye yönelik bir mekanizma olarak kullanılır. Uygulama varlığı ve şeması hakkında daha fazla bilgi için bkz. [Graph API Uygulama varlığı belgeleri](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity).

Azure portalı veya program aracılığıyla Microsoft Graph'ı kullanarak uygulamanın öznitelikleri yapılandırabilirsiniz. Bununla birlikte, burada bir uygulamanın özniteliği yapılandırmak için uygulama bildiriminizi düzenleyin gerekecektir bazı senaryolar vardır. Bu senaryolar şunlardır:

* Azure AD çok kiracılı ve kişisel Microsoft hesapları olarak uygulama kaydettiyseniz, kullanıcı arabiriminde desteklenen Microsoft hesapları değiştiremezsiniz. Bunun yerine, desteklenen bir hesap türünü değiştirmek için uygulama bildirim düzenleyicisini kullanmanız gerekir.
* İzinler ve uygulamanızın desteklediği rolleri tanımlamak gerekiyorsa, uygulama bildirimini değiştirmeniz gerekir.

## <a name="configure-the-app-manifest"></a>Uygulama bildirimi yapılandırma

Uygulama bildirimini yapılandırmak için:

1. Oturum açma [Azure portalında](https://portal.azure.com).
1. Seçin **Azure Active Directory** hizmet ve ardından **uygulama kayıtları** veya **uygulama kayıtları (Önizleme)**.
1. Yapılandırmak istediğiniz uygulamayı seçin.
1. Uygulamanın **Genel Bakış** sayfasında, **Bildirim** bölümünü seçin. Portal bildirimi düzenlemenize olanak sağlayan web tabanlı bir bildirim düzenleyicisinde açılır. İsteğe bağlı olarak seçebileceğiniz **indirin** yerel bildirimi düzenleyin ve ardından **karşıya** uygulamanızı uygulamak için.

## <a name="manifest-reference"></a>Bildirimi başvurusu

> [!NOTE]
> Göremiyorsanız **örnek değer** sonra sütun **açıklama**tarayıcı pencerenizin ekranı kaplaması ve kaydırma/geçirme görene kadar **örnek değer** sütun.

| Anahtar  | Değer türü | Açıklama  | Örnek değer |
|---------|---------|---------|---------|
| `accessTokenAcceptedVersion` | Boş değer atanabilir Int32 | Kaynak tarafından beklenen erişim belirteci sürümünü belirtir. Bu sürüm değiştirir ve biçimi JWT uç nokta veya istemci erişim belirteci istemek için kullanılan bağımsız olarak üretilen.<br/><br/>Kullanılan uç nokta veya v1.0, v2.0 istemci tarafından seçilir ve yalnızca id_tokens sürümünü etkiler. Açıkça yapılandırmanız gereken kaynakları `accesstokenAcceptedVersion` desteklenen erişim belirteci biçimini belirtmek için.<br/><br/>Olası değerler için `accesstokenAcceptedVersion` 1, 2 ya da null. Değer null ise, bu v1.0 uç noktaya karşılık gelen varsayılan olarak 1. <br/><br/>Varsa `signInAudience` olduğu `AzureADandPersonalMicrosoftAccount`, değer olmalıdır `2` | `2` |
| `allowPublicClient` | boole | Geri dönüş uygulaması türünü belirtir. Azure AD, varsayılan olarak replyUrlsWithType uygulama türünden çıkarır. Burada Azure AD'ye belirleyemiyor istemci uygulama türüne belirli senaryolar vardır (örneğin [ROPC](https://tools.ietf.org/html/rfc6749#section-4.3) burada HTTP isteği bir URL yeniden yönlendirme ' olmuyor akış). Bu gibi durumlarda, Azure AD uygulama türü, bu özelliğin değerine göre görürler. Bu değer, geri dönüş uygulama türünü true olarak ayarlanırsa, bir mobil cihazda çalışan yüklü bir uygulama gibi ortak istemci olarak ayarlanır. Web uygulaması gibi gizli bir istemci geri dönüş uygulaması türüdür yani varsayılan değer false'tur. | `false` |
| `appId` | Kimlik dizesi | Azure AD tarafından atanmış bir uygulama uygulama için benzersiz tanımlayıcısını belirtir. | `"601790de-b632-4f57-9523-ee7cb6ceba95"` |
| `appRoles` | Dizi türü | Bir uygulamanın bildirebileceği rolleri koleksiyonunu belirtir. Bu roller, kullanıcıları, grupları veya hizmet sorumlularına atanabilir. Daha fazla örnekler ve bilgi için bkz. [uygulamanızda uygulama rolleri eklemek ve bunları belirteci alma](howto-add-app-roles-in-azure-ad-apps.md) | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"allowedMemberTypes": [<br>&emsp;&nbsp;&nbsp;&nbsp;"User"<br>&nbsp;&nbsp;&nbsp;],<br>&nbsp;&nbsp;&nbsp;"description":"Read-only access to device information",<br>&nbsp;&nbsp;&nbsp;"displayName":"Read Only",<br>&nbsp;&nbsp;&nbsp;"id":guid,<br>&nbsp;&nbsp;&nbsp;"isEnabled":true,<br>&nbsp;&nbsp;&nbsp;"value":"ReadOnly"<br>&nbsp;&nbsp;}<br>]</code>  |
| `groupMembershipClaims` | string | Yapılandırır `groups` bir kullanıcı veya uygulamanın beklediği OAuth 2.0 erişim belirteci veren talep. Bu öznitelik ayarlamak için aşağıdaki geçerli dize değerlerinden birini kullanın:<br/><br/>- `"None"`<br/>- `"SecurityGroup"` (güvenlik grupları ve Azure AD rolleri için)<br/>- `"All"` (Bu tüm güvenlik grupları, dağıtım grupları ve oturum açmış kullanıcının üyesi olduğu bir Azure AD Dizin rolleri alırsınız. | `"SecurityGroup"` |
| `optionalClaims` | string | İsteğe bağlı talepleri özel bu uygulama için güvenlik belirteci hizmeti tarafından belirteç döndürdü.<br>Şu anda, hem kişisel hesapları hem de Azure AD (kayıtlı uygulama kayıt portalı üzerinden) destekleyen uygulamalar, isteğe bağlı bir talep kullanamazsınız. Ancak, yalnızca Azure AD v2.0 uç noktası kullanmak için kayıtlı uygulamalar bildiriminde talep isteğe bağlı bir talep elde edebilirsiniz. Daha fazla bilgi için bkz. [isteğe bağlı bir talep](active-directory-optional-claims.md). | `null` |
| `id` | Kimlik dizesi | Uygulama dizinindeki benzersiz tanımlayıcısı. Bu kimliği, herhangi bir protokol işlem uygulamayı tanımlamak için kullanılan tanımlayıcı değil. Dizin sorguları nesneye başvurmak için kullanılır. | `"f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd"` |
| `identifierUris` | Dize dizisi | Çok kiracılı bir uygulama ise, bir Web uygulaması doğrulanmış özel etki alanı veya kendi Azure AD kiracısı içinde benzersiz olarak tanımlayan kullanıcı tanımlı URI(s). | <code>[<br>&nbsp;&nbsp;"https://MyRegisteredApp"<br>]</code> |
| `informationalUrls` | string | Uygulamanın koşullarını hizmet ve gizlilik bildirimi bağlantıları belirtir. Koşulları hizmet ve gizlilik bildirimi kullanıcı onayı deneyimi aracılığıyla kullanıcılara çıkarılır. Daha fazla bilgi için bkz. [nasıl yapılır: Kayıtlı Azure AD uygulamaları için hizmet ve gizlilik bildirimini koşulları ekleme](howto-add-terms-of-service-privacy-statement.md). | <code>{<br>&nbsp;&nbsp;&nbsp;"marketing":"https://MyRegisteredApp/marketing",<br>&nbsp;&nbsp;&nbsp;"privacy":"https://MyRegisteredApp/privacystatement",<br>&nbsp;&nbsp;&nbsp;"support":"https://MyRegisteredApp/support",<br>&nbsp;&nbsp;&nbsp;"termsOfService":"https://MyRegisteredApp/termsofservice"<br>}</code> |
| `keyCredentials` | Dizi türü | Uygulama tarafından atanan kimlik bilgileri, paylaşılan gizlilikler dize tabanlı ve X.509 sertifikaları için başvurular içerir. Bu kimlik bilgileri erişim belirteci isteğinde bulunurken kullanılır (uygulama istemci olarak hareket ne zaman yerine, kaynak olarak). | <code>[<br>&nbsp;{<br>&nbsp;&nbsp;&nbsp;"customKeyIdentifier":null,<br>&nbsp;&nbsp;&nbsp;"endDate":"2018-09-13T00:00:00Z",<br>&nbsp;&nbsp;&nbsp;"keyId":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"startDate":"2017-09-12T00:00:00Z",<br>&nbsp;&nbsp;&nbsp;"type":"AsymmetricX509Cert",<br>&nbsp;&nbsp;&nbsp;"usage":"Verify",<br>&nbsp;&nbsp;&nbsp;"value":null<br>&nbsp;&nbsp;}<br>]</code> |
| `knownClientApplications` | Dizi türü | İki bölümü içeren bir çözümünüz varsa paket onayı için kullanılan: bir istemci uygulaması hem de özel web API uygulaması. İçinde bu değer istemci uygulamasının uygulama kimliği girin, kullanıcının yalnızca bir kez istemci uygulamasına kabul sahip olur. Azure AD, istemciye yönelik örtük olarak web API'sine yönelik anlamına gelir ve hem istemci hem de web API'si için hizmet sorumluları, aynı anda otomatik olarak sağlamış olduğunu bilirsiniz. Hem istemci hem de web API'si uygulaması aynı kiracıda kayıtlı olması gerekir. | `[GUID]` |
| `logoUrl` | string | Portalda karşıya yüklenen logoyu CDN URL'sine işaret eden değerini okur. | `https://MyRegisteredAppLogo` |
| `logoutUrl` | string | Uygulama oturumunu URL. | `https://MyRegisteredAppLogout` |
| `name` | string | Uygulama görünen adı. | `MyRegisteredApp` |
| `oauth2AllowImplicitFlow` | boole | Bu web uygulaması OAuth2.0 örtük akış erişim belirteçleri isteme olanaklarının olup olmadığını belirtir. Varsayılan değer false'dur. Bu bayrak, Javascript tek sayfa uygulamaları gibi tarayıcı tabanlı uygulamalar için kullanılır. Daha fazla bilgi için girin `OAuth 2.0 implicit grant flow` içindekiler tablosunda ve örtük akış hakkındaki konulara bakın. | `false` |
| `oauth2AllowIdTokenImplicitFlow` | boole | Bu web uygulaması OAuth2.0 örtük akış kimliği belirteç isteyip isteyemeyeceğini belirtir. Varsayılan değer false'dur. Bu bayrak, Javascript tek sayfa uygulamaları gibi tarayıcı tabanlı uygulamalar için kullanılır. | `false` |
| `oauth2Permissions` | Dizi türü | İstemci uygulamalarında web API (kaynak) uygulamanın sunduğu OAuth 2.0 izin kapsamları koleksiyonu belirtir. Bu izin kapsamları, onay işlemi sırasında istemci uygulamalara verilebilir. | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"adminConsentDescription":"Allow the app to access resources on behalf of the signed-in user.",<br>&nbsp;&nbsp;&nbsp;"adminConsentDisplayName":"Access resource1",<br>&nbsp;&nbsp;&nbsp;"id":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"isEnabled":true,<br>&nbsp;&nbsp;&nbsp;"type":"User",<br>&nbsp;&nbsp;&nbsp;"userConsentDescription":"Allow the app to access resource1 on your behalf.",<br>&nbsp;&nbsp;&nbsp;"userConsentDisplayName":"Access resources",<br>&nbsp;&nbsp;&nbsp;"value":"user_impersonation"<br>&nbsp;&nbsp;}<br>] </code>|
| `oauth2RequiredPostResponse` | boole | OAuth 2.0 belirteç isteklerini bir parçası olarak, Azure AD GET istekleri aksine POST istekleri izin olup olmadığını belirtir. Yalnızca GET isteklerini verileceğini belirten varsayılan false değeridir. | `false` |
| `parentalControlSettings` | string | `countriesBlockedForMinors` uygulama için yetişkin engellenir ülkeleri belirtir.<br>`legalAgeGroupRule` Uygulamayı kullanıcılara uygulanan yasal yaş grubu kuralı belirtir. Ayarlanabilir `Allow`, `RequireConsentForPrivacyServices`, `RequireConsentForMinors`, `RequireConsentForKids`, veya `BlockMinors`.  | <code>{<br>&nbsp;&nbsp;&nbsp;"countriesBlockedForMinors":[],<br>&nbsp;&nbsp;&nbsp;"legalAgeGroupRule":"Allow"<br>} </code> |
| `passwordCredentials` | Dizi türü | Açıklamasına bakın `keyCredentials` özelliği. | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"customKeyIdentifier":null,<br>&nbsp;&nbsp;&nbsp;"endDate":"2018-10-19T17:59:59.6521653Z",<br>&nbsp;&nbsp;&nbsp;"keyId":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"startDate":"2016-10-19T17:59:59.6521653Z",<br>&nbsp;&nbsp;&nbsp;"value":null<br>&nbsp;&nbsp;&nbsp;}<br>] </code> |
| `preAuthorizedApplications` | Dizi türü | Uygulamalar ve tanımlanabilen istenen izinleri listeler. Bir yöneticinin uygulamaya izin vermiş gerektirir. preAuthorizedApplications istenen izinleri kabul etmesi gerekmez. Kullanıcı onayı preAuthorizedApplications içinde listelenen izinlere gerek yoktur. Ancak preAuthorizedApplications listelenmeyen ek istenen izinleri, kullanıcı onayı gerektirir. | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;"appId": "abcdefg2-000a-1111-a0e5-812ed8dd72e8",<br>&nbsp;&nbsp;&nbsp;&nbsp;"permissionIds": [<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"8748f7db-21fe-4c83-8ab5-53033933c8f1"<br>&nbsp;&nbsp;&nbsp;&nbsp;]<br>&nbsp;&nbsp;}<br>]</code> |
| `replyUrlsWithType` | Dize dizisi | Bu çok değerli özellik belirteçleri döndürülürken hedefleri olarak Azure AD'ye kabul kayıtlı redirect_uri değerlerin listesini tutar. Her URI değeri, bir ilişkili uygulama türü değeri içermelidir. Desteklenen türde değerler şunlardır: `Web`, `InstalledClient`. | <code>"replyUrlsWithType":&nbsp;[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"url":&nbsp;"https://localhost:4400/services/office365/redirectTarget.html",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type":&nbsp;"InstalledClient"&nbsp;&nbsp;&nbsp;<br>&nbsp;&nbsp;}<br>]</code> |
| `requiredResourceAccess` | Dizi türü | Dinamik onayı ile `requiredResourceAccess` yönetici onayı deneyimi ve statik onay kullanan kullanıcılar için kullanıcı onayı deneyimi sürücüler. Ancak, bu genel durum kullanıcı onayı deneyimi sürücü değil.<br>`resourceAppId` Uygulama erişim gerektiren kaynağın benzersiz tanımlayıcısıdır. Bu değer, hedef kaynak uygulama bildirilen AppID eşit olmalıdır.<br>`resourceAccess` Belirtilen kaynak uygulama gerektiren uygulama rolleri ve izin kapsamları OAuth2.0 listeleyen bir dizidir. İçeren `id` ve `type` değerleri belirtilen kaynakları. | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;"resourceAppId":"00000002-0000-0000-c000-000000000000",<br>&nbsp;&nbsp;&nbsp;&nbsp;"resourceAccess":[<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"id":"311a71cc-e848-46a1-bdf8-97ff7156d8e6",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type":"Scope"<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;&nbsp;&nbsp;]<br>&nbsp;&nbsp;}<br>] </code> |
| `samlMetadataUrl` | string | Uygulama SAML meta verilerinin URL'si. | `https://MyRegisteredAppSAMLMetadata` |
| `signInUrl` | string | Uygulama giriş sayfası URL'sini belirtir. | `https://MyRegisteredApp` |
| `signInAudience` | string | Hangi Microsoft hesapları için geçerli uygulamanın desteklenen belirtir. Desteklenen değerler şunlardır:<ul><li>**AzureADMyOrg** -kullanıcılar Microsoft ile iş veya Okul hesabı kuruluşumun Azure AD kiracısında (yani, tek kiracılı)</li><li>**AzureADMultipleOrgs** -kullanıcılar Microsoft ile iş veya Okul hesabı herhangi bir kuruluşun Azure AD kiracısında (yani, çok kiracılı)</li> <li>**AzureADandPersonalMicrosoftAccount** -kişisel bir Microsoft hesabı ya da herhangi bir kuruluşun Azure AD kiracısında bir iş veya Okul hesabı olan kullanıcılar</li></ul> | `AzureADandPersonalMicrosoftAccount` |
| `tags` | Dize dizisi | Kategorilere ayırmak ve uygulamayı tanımlamak için kullanılan özel dizeleri. | <code>[<br>&nbsp;&nbsp;"ProductionApp"<br>]</code> |


## <a name="manifest-limits"></a>Sınırları bildirimi
Bir uygulama bildirimi için adlandırılan örneğin approles koleksiyonları keycredentials, knownClientApplications, identifierUris, rediretUris, requiredResourceAccess, oauth2Permissions birden çok özniteliğe sahip vs. Herhangi bir uygulama için tam bir uygulama bildirimi içinde birlikte tüm koleksiyonları giriş toplam sayısı 1200 tavan. Yalnızca sol 1100 kalan ile olursunuz uygulama bildiriminde belirtilen 100 yeniden yönlendirme URI'si zaten varsa, diğer tüm Koleksiyonlar boyunca kullanmak için hangi bildirimi oluşturan olun birleştirilmiş giriş.

> [!NOTE]
> Durumda uygulama bildiriminde 1200'den fazla girişleri eklemeyi deneyin. Bir hata iletisi alabilirsiniz **"uygulama xxxxxx güncelleştirilemedi. Hata ayrıntıları: Bildirimin boyut sınırını aştı. Lütfen değerlerin sayısını azaltın ve isteğinizi yeniden deneyin.** "


## <a name="next-steps"></a>Sonraki adımlar

* Bir uygulamanın uygulama ve hizmet sorumlusu nesneleri arasındaki ilişki hakkında daha fazla bilgi için bkz. [uygulaması ve Azure AD'de hizmet sorumlusu nesneleri](app-objects-and-service-principals.md).
* Bkz: [Azure AD Geliştirici sözlüğü](developer-glossary.md) Azure Active Directory (AD) Geliştirici kavramları bazılarının tanımları için.

Aşağıdaki Açıklamalar bölümü daraltmak ve içeriklerimizde şekil yardımcı olur. geri bildirim sağlamak için kullanın.

<!--article references -->
[AAD-APP-OBJECTS]:app-objects-and-service-principals.md
[AAD-DEVELOPER-GLOSSARY]:developer-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]:quickstart-v1-integrate-apps-with-azure-ad.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-PORTAL]: https://portal.azure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]:v1-oauth2-implicit-grant-flow.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
