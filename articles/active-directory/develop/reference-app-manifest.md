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
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/27/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: sureshja, justhu
ms.openlocfilehash: f336771da334ffd964ecd032654b8549b7a5e847
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43127029"
---
# <a name="azure-active-directory-app-manifest"></a>Azure Active Directory Uygulama bildirimi

Azure Active Directory (Azure AD) ile tümleştiren uygulamalar Azure AD kiracısı ile kayıtlı olması gerekir. Uygulamada yapılandırabileceğiniz [Azure portalında](https://portal.azure.com) seçerek **Azure Active Directory**, yapılandırmak istediğiniz uygulama seçme ve ardından seçerek **bildirim**.

## <a name="manifest-reference"></a>Bildirimi başvurusu

> [!NOTE]
> Açıklamaları göremiyorsanız, tarayıcı pencerenizin ekranı kaplaması veya açıklamaları görmek için kaydırma/geçirme.

>[!div class="mx-tdBreakAll"]
>[!div class="mx-tdCol2BreakAll"]

| Anahtar  | Değer türü | Örnek değer | Açıklama  |
|---------|---------|---------|---------|
| `appID` | Kimlik dizesi | `"601790de-b632-4f57-9523-ee7cb6ceba95"` | Azure AD tarafından atanmış bir uygulama uygulama için benzersiz tanımlayıcısını belirtir. |
| `appRoles` | Dizi türü | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"allowedMemberTypes": [<br>&emsp;&nbsp;&nbsp;&nbsp;"User"<br>&nbsp;&nbsp;&nbsp;],<br>&nbsp;&nbsp;&nbsp;"description":"Read-only access to device information",<br>&nbsp;&nbsp;&nbsp;"displayName":"Read Only",<br>&nbsp;&nbsp;&nbsp;"id":guid,<br>&nbsp;&nbsp;&nbsp;"isEnabled":true,<br>&nbsp;&nbsp;&nbsp;"value":"ReadOnly"<br>&nbsp;&nbsp;}<br>]</code> | Bir uygulamanın bildirebileceği rolleri koleksiyonunu belirtir. Bu roller, kullanıcıları, grupları veya hizmet sorumlularına atanabilir. |
| `availableToOtherTenants`| boole | `true` | Bu değer ayarlanırsa true, uygulamanın diğer kiracılar için kullanılabilir. Uygulama false olarak ayarlanırsa yalnızca kiracıya kullanılabilir durumdaysa olarak kaydedilir. Daha fazla bilgi için bkz. [nasıl yapılır: çok kiracılı uygulama desenini kullanarak, herhangi bir Azure AD kullanıcısıyla oturum](howto-convert-app-to-be-multi-tenant.md). |
| `displayName` | dize | `MyRegisteredApp` | Uygulama görünen adı. |
| `errorURL` | dize | `http://MyRegisteredAppError` | Bir uygulamada karşılaşılan hatalar için URL. |
| `groupMembershipClaims` | dize | `1` | Yapılandıran bir bit maskesi `groups` bir kullanıcı veya uygulamanın beklediği OAuth 2.0 erişim belirteci veren talep. Bit maskesi değerleri şunlardır:<br>0: yok<br>1: güvenlik grupları ve Azure AD rolleri<br>2: ayrılmış<br>4: ayrılmış<br>Bit maskesi 7 olarak ayarlanması tüm güvenlik grupları, dağıtım grupları ve oturum açmış kullanıcının üyesi olduğu bir Azure AD Dizin rolleri alırsınız. |
| `optionalClaims` | dize | `null` | İsteğe bağlı talepleri özel bu uygulama için güvenlik belirteci hizmeti tarafından belirteç döndürdü. Daha fazla bilgi için bkz. [isteğe bağlı bir talep](active-directory-optional-claims.md). |
| `acceptMappedClaims` | boole | `true` | Bu değer ayarlanırsa `true`, uygulamanın talepleri özel bir imzalama anahtarı belirtmeden eşleme kullanmasına izin verir. |
| `homepage` | dize | `http://MyRegisteredApp` | Uygulama giriş sayfası URL'sini belirtir. |
| `informationalUrls` | dize | <code>{<br>&nbsp;&nbsp;&nbsp;"privacy":"http://MyRegisteredApp/privacystatement",<br>&nbsp;&nbsp;&nbsp;"termsOfService":"http://MyRegisteredApp/termsofservice"<br>}</code> | Uygulamanın koşullarını hizmet ve gizlilik bildirimi bağlantıları belirtir. Koşulları hizmet ve gizlilik bildirimi kullanıcı onayı deneyimi aracılığıyla kullanıcılara çıkarılır. Daha fazla bilgi için bkz. [nasıl yapılır: kayıtlı Azure AD uygulamaları için hizmet ve gizlilik bildirimini koşulları ekleme](howto-add-terms-of-service-privacy-statement.md). |
| `identifierUris` | Dize dizisi | `http://MyRegistererdApp` | Çok kiracılı bir uygulama ise, bir Web uygulaması doğrulanmış özel etki alanı veya kendi Azure AD kiracısı içinde benzersiz olarak tanımlayan kullanıcı tanımlı URI(s). |
| `keyCredentials` | Dizi türü | <code>[<br>&nbsp;{<br>&nbsp;&nbsp;&nbsp;"customKeyIdentifier":null,<br>&nbsp;&nbsp;&nbsp;"endDate":"2018-09-13T00:00:00Z",<br>&nbsp;&nbsp;&nbsp;"keyId":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"startDate":"2017-09-12T00:00:00Z",<br>&nbsp;&nbsp;&nbsp;"type":"AsymmetricX509Cert",<br>&nbsp;&nbsp;&nbsp;"usage":"Verify",<br>&nbsp;&nbsp;&nbsp;"value":null<br>&nbsp;&nbsp;}<br>]</code> | Uygulama tarafından atanan kimlik bilgileri, paylaşılan gizlilikler dize tabanlı ve X.509 sertifikaları için başvurular içerir. Bu kimlik bilgileri erişim belirteci isteğinde bulunurken kullanılır (uygulama istemci olarak hareket ne zaman yerine, kaynak olarak). |
| `knownClientApplications` | Dizi türü | `[GUID]` | İki bölümü içeren bir çözümünüz varsa paket onayı için kullanılan: bir istemci uygulaması hem de özel web API uygulaması. İçinde bu değer istemci uygulamasının uygulama kimliği girin, kullanıcının yalnızca bir kez istemci uygulamasına kabul sahip olur. Azure AD, istemciye yönelik örtük olarak web API'sine yönelik anlamına gelir ve hem istemci hem de web API'si için hizmet sorumluları, aynı anda otomatik olarak sağlamış olduğunu bilirsiniz. Hem istemci hem de web API'si uygulaması aynı kiracıda kayıtlı olması gerekir. |
| `logoutUrl` | dize | `http://MyRegisteredAppLogout` | Uygulama oturumunu URL. |
| `oauth2AllowImplicitFlow` | boole | `false` | Bu web uygulaması OAuth2.0 örtük akış belirteç isteyip isteyemeyeceğini belirtir. Varsayılan değer false'dur. Bu bayrak, Javascript tek sayfa uygulamaları gibi tarayıcı tabanlı uygulamalar için kullanılır. |
| `oauth2AllowUrlPathMatching` | boole | `false` | OAuth 2.0 belirteç isteklerini bir parçası olarak, Azure AD yeniden yönlendirme URI'si uygulamanın replyUrls karşı yolla bağdaşan izin verip vermeyeceğini, belirtir. Varsayılan değer false'dur. |
| `oauth2Permissions` | Dizi türü | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"adminConsentDescription":"Allow the app to access resources on behalf of the signed-in user.",<br>&nbsp;&nbsp;&nbsp;"adminConsentDisplayName":"Access resource1",<br>&nbsp;&nbsp;&nbsp;"id":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"isEnabled":true,<br>&nbsp;&nbsp;&nbsp;"type":"User",<br>&nbsp;&nbsp;&nbsp;"userConsentDescription":"Allow the app to access resource1 on your behalf.",<br>&nbsp;&nbsp;&nbsp;"userConsentDisplayName":"Access resources",<br>&nbsp;&nbsp;&nbsp;"value":"user_impersonation"<br>&nbsp;&nbsp;}<br>]</code> | İstemci uygulamalarında web API (kaynak) uygulamanın sunduğu OAuth 2.0 izin kapsamları koleksiyonu belirtir. Bu izin kapsamları, onay işlemi sırasında istemci uygulamalara verilebilir. |
| `oauth2RequiredPostResponse` | boole | `false` | OAuth 2.0 belirteç isteklerini bir parçası olarak, Azure AD GET istekleri aksine POST istekleri izin olup olmadığını belirtir. Yalnızca GET isteklerini verileceğini belirten varsayılan false değeridir. |
| `objectId` | Kimlik dizesi | `"f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd"` | Uygulama dizinindeki benzersiz tanımlayıcısı. Bu kimliği, herhangi bir protokol işlem uygulamayı tanımlamak için kullanılan tanımlayıcı değil. Dizin sorguları nesneye başvurmak için kullanılır. |
| `parentalControlSettings` | dize | <code>{<br>&nbsp;&nbsp;&nbsp;"countriesBlockedForMinors":[],<br>&nbsp;&nbsp;&nbsp;"legalAgeGroupRule":"Allow"<br>} </code> | `countriesBlockedForMinors` uygulama için yetişkin engellenir ülkeleri belirtir.<br>`legalAgeGroupRule` Uygulamayı kullanıcılara uygulanan yasal yaş grubu kuralı belirtir. Ayarlanabilir `Allow`, `RequireConsentForPrivacyServices`, `RequireConsentForMinors`, `RequireConsentForKids`, veya `BlockMinors`.  |
| `passwordCredentials` | Dizi türü | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"customKeyIdentifier":null,<br>&nbsp;&nbsp;&nbsp;"endDate":"2018-10-19T17:59:59.6521653Z",<br>&nbsp;&nbsp;&nbsp;"keyId":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"startDate":"2016-10-19T17:59:59.6521653Z",<br>&nbsp;&nbsp;&nbsp;"value":null<br>&nbsp;&nbsp;&nbsp;}<br>] </code> | Açıklamasına bakın `keyCredentials` özelliği. |
| `publicClient` | boole | `false` | Bir uygulamayı mobil cihazda çalışan yüklü bir uygulama gibi genel bir istemci olup olmadığını belirtir. Varsayılan değer false'tur. |
| `replyUrls` | Dize dizisi | `"http://localhost"` | Bu çok değerli özellik belirteçleri döndürülürken hedefleri olarak Azure AD'ye kabul kayıtlı redirect_uri değerlerin listesini tutar. |
| `requiredResourceAccess` | Dizi türü | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"resourceAppId":"00000002-0000-0000-c000-000000000000",<br>&nbsp;&nbsp;&nbsp;"resourceAccess":[<br>&nbsp;&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"id":"311a71cc-e848-46a1-bdf8-97ff7156d8e6",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type":"Scope"<br>&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;]<br>&nbsp;}<br>] </code> | Dinamik onayı ile `requiredResourceAccess` yönetici onayı deneyimi ve statik onay kullanan kullanıcılar için kullanıcı onayı deneyimi sürücüler. Ancak, bu genel durum kullanıcı onayı deneyimi sürücü değil.<br>`resourceAppId` Uygulama erişim gerektiren kaynağın benzersiz tanımlayıcısıdır. Bu değer, hedef kaynak uygulama bildirilen AppID eşit olmalıdır.<br>`resourceAccess` Belirtilen kaynak uygulama gerektiren uygulama rolleri ve izin kapsamları OAuth2.0 listeleyen bir dizidir. İçeren `id` ve `type` değerleri belirtilen kaynakları. |
| `samlMetadataUrl` | dize | `http://MyRegisteredAppSAMLMetadata` | Uygulama SAML meta verilerinin URL'si. |

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

