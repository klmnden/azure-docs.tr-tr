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
ms.date: 07/20/2017
ms.author: celested
ms.custom: aaddev
ms.reviewer: elisol, sureshja
ms.openlocfilehash: a0d302e740732c5bf76ba75486b75f6f73091940
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39576466"
---
# <a name="azure-active-directory-application-manifest"></a>Azure Active Directory Uygulama bildirimini
Azure AD ile tümleştirilen uygulamalar Azure AD kiracısı ile kayıtlı olması gerekir. Bu uygulama, uygulama bildirimi (altında Azure AD dikey penceresi) kullanılarak yapılandırılabilir [Azure portalında](https://portal.azure.com).

## <a name="manifest-reference"></a>Bildirimi başvurusu

>[!div class="mx-tdBreakAll"]
>[!div class="mx-tdCol2BreakAll"]
|Anahtar  |Değer türü |Örnek Değer  |Açıklama  |
|---------|---------|---------|---------|
|Uygulama Kimliği     |  Kimlik dizesi       |""|  Uygulamanın Azure AD tarafından atanmış bir uygulama için benzersiz tanımlayıcı.|
|appRoles     |    Dizi türü     |<code>[{<br>&emsp;"allowedMemberTypes": [<br>&emsp;&nbsp;&nbsp;&nbsp;"User"<br>&emsp;],<br>&emsp;"description":"Read-only access to device information",<br>&emsp;"displayName":"Read Only",<br>&emsp;"id":guid,<br>&emsp;"isEnabled":true,<br>&emsp;"value":"ReadOnly"<br>}]</code>|Uygulamanın bildirebileceği rolleri koleksiyonu. Bu roller, kullanıcıları, grupları veya hizmet sorumlularına atanabilir.|
|availableToOtherTenants|boole|`true`|Bu değer ayarlanırsa true, uygulamanın diğer kiracılar için kullanılabilir. Uygulama false olarak ayarlanırsa yalnızca kiracıya kullanılabilir durumdaysa olarak kaydedilir. Daha fazla bilgi için bkz: [çok kiracılı uygulama desenini kullanarak istediğiniz bir Azure Active Directory (AD) kullanıcısı ile oturum açma](howto-convert-app-to-be-multi-tenant.md). |
|displayName     |dize         |`MyRegisteredApp`         |Uygulama görünen adı. |
|errorURL     |dize         |`http://MyRegisteredAppError`         |Bir uygulamada karşılaşılan hatalar için URL. |
|groupMembershipClaims     |    dize     |    `1`     |   Bir kullanıcı veya uygulamanın beklediği OAuth 2.0 erişim belirteci veren "grupları" talep yapılandırır bir bit maskesi. Bit maskesi değerleri: 0: hiçbiri, 1: güvenlik grupları ve Azure AD rolleri, 2: ayrılmış ve 4: ayrılmış. Bit maskesi 7 olarak ayarlanması tüm güvenlik grupları, dağıtım grupları ve oturum açmış kullanıcının üyesi olduğu bir Azure AD Dizin rolleri alırsınız. |
|optionalClaims     |  dize       |     `null`    |    [İsteğe bağlı bir talep](active-directory-optional-claims.md) özel bu uygulama için güvenlik belirteci hizmeti tarafından belirteç döndürdü. |
|acceptMappedClaims    |      boole   | `true`        |    Bu değer veren true olarak ayarlanırsa bir uygulamayı kullanmak için özel bir imzalama anahtarı belirtmeden eşleme talep.|
|Giriş sayfası     |  dize       |`http://MyRegistererdApp`         |    Uygulamanın giriş sayfası URL'si. |
|identifierUris     |  Dize dizisi       | `http://MyRegistererdApp`        |   Çok kiracılı uygulama ise, bir Web uygulaması doğrulanmış özel etki alanı veya kendi Azure AD kiracısı içinde benzersiz olarak tanımlayan kullanıcı tanımlı URI(s). |
|keyCredentials     |   Dizi türü      | <code>[{<br>&nbsp;"customKeyIdentifier":null,<br>"endDate":"2018-09-13T00:00:00Z",<br>"keyId":"\<guid>",<br>"startDate":"2017-09-12T00:00:00Z",<br>"type":"AsymmetricX509Cert",<br>"usage":"Verify",<br>"value":null<br>}]</code>      |   Bu özellik, uygulama tarafından atanan kimlik bilgileri, paylaşılan gizlilikler dize tabanlı ve X.509 sertifikaları başvuruları içerir. Bu kimlik bilgileri erişim belirteci isteğinde bulunurken kullanılır (uygulama istemci olarak hareket ne zaman yerine, kaynak olarak). |
|knownClientApplications     |     Dizi türü    |    [GUID]     |     Bir istemci uygulaması ve özel bir web API uygulaması iki bölümden içeren bir çözümü varsa değeri paketleme onayı için kullanılır. Bu değerde istemci uygulamasının uygulama kimliği girin, kullanıcı yalnızca bir kez istemci uygulama onayı gerekir. Azure AD, istemciye yönelik örtük olarak web API'sine yönelik anlamına gelir ve hem istemci hem de web API'si için hizmet sorumluları, aynı anda otomatik olarak sağlamış olduğunu bilirsiniz. Hem istemci hem de web API'si uygulama aynı kiracıda kayıtlı olması gerekir.|
|logoutUrl     |   dize      |     `http://MyRegisteredAppLogout`    |   Uygulamanın oturum kapatma URL'si. |
|oauth2AllowImplicitFlow     |   boole      |  `false`       |       Bu web uygulaması OAuth2.0 örtük akış belirteç isteyip isteyemeyeceğini belirtir. Varsayılan değer false'dur. Bu bayrak, Javascript tek sayfa uygulamaları gibi tarayıcı tabanlı uygulamalar için kullanılır. |
|oauth2AllowUrlPathMatching     |   boole      |  `false`       |   OAuth 2.0 belirteç isteklerini bir parçası olarak, Azure AD yeniden yönlendirme URI'si uygulamanın replyUrls karşı yolla bağdaşan izin verip vermeyeceğini, belirtir. Varsayılan değer false'dur. |
|oauth2Permissions     | Dizi türü         |      <code>[{<br>"adminConsentDescription":"Allow the application to access resources on behalf of the signed-in user.",<br>"adminConsentDisplayName":"Access resource1",<br>"id":"\<guid>",<br>"isEnabled":true,<br>"type":"User",<br>"userConsentDescription":"Allow the application to access resource1 on your behalf.",<br>"userConsentDisplayName":"Access resources",<br>"value":"user_impersonation"<br>}]  </code> |  Web API (kaynak) uygulamasının istemci uygulamalarına sunan, OAuth 2.0 izin kapsamları koleksiyonu. Bu izin kapsamları, onay işlemi sırasında istemci uygulamalara verilebilir. |
|oauth2RequiredPostResponse     | boole        |    `false`     |      OAuth 2.0 belirteç isteklerini bir parçası olarak, Azure AD GET istekleri aksine POST istekleri izin olup olmadığını belirtir. Yalnızca GET isteklerini verileceğini belirten varsayılan false değeridir. 
|objectId     | Kimlik dizesi        |     ""    |    Uygulama dizinindeki benzersiz tanımlayıcısı. Bu kimliği, herhangi bir protokol işlem uygulamayı tanımlamak için kullanılan tanımlayıcı değil. Kullanıcı, nesneyi dizin sorguları başvurmak için değil.|
|passwordCredentials     | Dizi türü        |   <code>[{<br>"customKeyIdentifier":null,<br>"endDate":"2018-10-19T17:59:59.6521653Z",<br>"keyId":"\<guid>",<br>"startDate":"2016-10-19T17:59:59.6521653Z",<br>"value":null<br>}]  </code>    |    KeyCredentials özelliğinin açıklamasına bakın. |
|publicClient     |  boole       |      `false`   | Bir uygulamanın genel bir istemci (örneğin, bir mobil cihazda çalışan yüklü bir uygulama) olup olmadığını belirtir. Varsayılan değer false’tur. |
|supportsConvergence     |  boole       |   `false`      | Bu özelliği düzenlenmemelidir. Varsayılan değeri kabul edin. |
|replyUrls     |  Dize dizisi       |   `http://localhost`     |  Bu çok değerli özellik belirteçleri döndürülürken hedef olarak Azure AD'ye kabul kayıtlı redirect_uri değerlerin listesini tutar. |
|requiredResourceAccess     |     Dizi türü    |    <code>[{<br>"resourceAppId":"00000002-0000-0000-c000-000000000000",<br>"resourceAccess":[{<br>&nbsp;&nbsp;&nbsp;&nbsp;"id":"311a71cc-e848-46a1-bdf8-97ff7156d8e6",<br>&nbsp;&nbsp;&nbsp;&nbsp;"type":"Scope"<br>&nbsp;&nbsp;}]<br>}] </code>    |   Bu uygulama için erişim ve OAuth izin kapsamları ve bu kaynakların her altında gereken uygulama rolleri kümesini gerektirdiği kaynakların belirtir. Bu gerekli kaynak erişim yapılandırma öncesi onayı deneyimi beraberinde getirir.|
|resourceAppId     |    Kimlik dizesi     |  ""      |   Uygulama erişim gerektiren bir kaynak için benzersiz tanımlayıcı. Bu değer, hedef kaynak uygulama üzerinde bildirilen AppID eşit olmalıdır. |
|resourceAccess     |  Dizi türü       | Örnek değer requiredResourceAccess özelliği için bkz. |   OAuth2.0 izin kapsamları ve uygulama, belirtilen kaynak (belirtilen kaynak kimliği ve tür değerleri içeren) gerektirir. uygulama rolleri listesi        |
|samlMetadataUrl    |dize| `http://MyRegisteredAppSAMLMetadata` |Uygulama SAML meta verilerinin URL'si.| 

## <a name="next-steps"></a>Sonraki adımlar
* Bir uygulamanın uygulama ve hizmet sorumlusu nesneleri arasındaki ilişki hakkında daha fazla bilgi için bkz. [uygulaması ve Azure AD'de hizmet sorumlusu nesneleri][AAD-APP-OBJECTS].
* Bkz: [Azure AD Geliştirici sözlüğü] [ AAD-DEVELOPER-GLOSSARY] Azure Active Directory (AD) Geliştirici kavramları bazılarının tanımları için.

Aşağıdaki Açıklamalar bölümü daraltmak ve içeriklerimizde şekil yardımcı olur. geri bildirim sağlamak için kullanın.

<!--article references -->
[AAD-APP-OBJECTS]:app-objects-and-service-principals.md
[AAD-DEVELOPER-GLOSSARY]: active-directory-dev-glossary.md
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

