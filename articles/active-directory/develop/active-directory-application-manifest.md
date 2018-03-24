---
title: Azure Active Directory Uygulama bildirimini anlama | Microsoft Docs
description: Azure AD kiracısı bir uygulamanın kimlik yapılandırmada temsil eder ve OAuth yetkilendirme, onayı deneyimi ve daha fazlasını kolaylaştırmak için kullanılan Azure Active Directory Uygulama bildirimini ayrıntılı kapsamını.
services: active-directory
documentationcenter: ''
author: sureshja
manager: mtillman
editor: ''
ms.assetid: 4804f3d4-0ff1-4280-b663-f8f10d54d184
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: sureshja
ms.custom: aaddev
ms.reviewer: elisol
ms.openlocfilehash: ee4e7eb6625cab274455ca787feeb7f267e11051
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-active-directory-application-manifest"></a>Azure Active Directory Uygulama bildirimini
Azure AD ile tümleştirmek uygulamaları Azure AD kiracısı ile kayıtlı olması gerekir. Bu uygulama, uygulama bildirimi (altında Azure AD dikey) kullanılarak yapılandırılabilir [Azure portal](https://portal.azure.com).

## <a name="manifest-reference"></a>Bildirim başvurusu

>[!div class="mx-tdBreakAll"]
>[!div class="mx-tdCol2BreakAll"]
|Anahtar  |Değer türü |Örnek Değer  |Açıklama  |
|---------|---------|---------|---------|
|AppID     |  Kimlik dizesi       |""|  Bir uygulama için Azure AD tarafından atanan uygulama için benzersiz tanımlayıcı.|
|appRoles     |    Dizi türü     |<code>[{<br>&emsp;"allowedMemberTypes": [<br>&emsp;&nbsp;&nbsp;&nbsp;"User"<br>&emsp;],<br>&emsp;"description":"Read-only access to device information",<br>&emsp;"displayName":"Read Only",<br>&emsp;"id":guid,<br>&emsp;"isEnabled":true,<br>&emsp;"value":"ReadOnly"<br>}]</code>|Bir uygulama bildirebilir rolleri koleksiyonu. Bu roller, kullanıcıları, grupları veya hizmet asıl adı atanabilir.|
|AvailableToOtherTenants|boole|`true`|Bu değer ayarlanırsa true, uygulama diğer kiracılar için kullanılabilir.  Uygulama false olarak ayarlanırsa yalnızca Kiracı için kullanılabilir olduğunda olarak kaydedilir.  Daha fazla bilgi için bkz: [çok kiracılı uygulama desenini kullanarak herhangi bir Azure Active Directory (AD) kullanıcı oturum nasıl](active-directory-devhowto-multi-tenant-overview.md). |
|displayName     |string         |`MyRegisteredApp`         |Uygulama görünen adı. |
|errorURL     |string         |`http://MyRegisteredAppError`         |Bir uygulamada oluşan hatalar için URL. |
|groupMembershipClaims     |    string     |    `1`     |   Bir kullanıcı veya uygulama bekler OAuth 2.0 erişim belirteci verilen "grupları" talep yapılandırır bir bit maskesi. Bit maskesi değerler: 0: None, 1: güvenlik gruplarını ve Azure AD roller, 2: ayrılmış ve 4: ayrılmış. 7 bit maskesi ayarı tüm güvenlik gruplarını, dağıtım grupları ve oturum açmış kullanıcının üyesi olduğu Azure AD directory rolleri alır.      |
|optionalClaims     |  string       |     `null`    |    [İsteğe bağlı talep](active-directory-optional-claims.md) özel bu uygulama için güvenlik belirteci hizmeti tarafından belirteç döndürdü.     |
|acceptMappedClaims    |      boole   | `true`        |    Bu değer verir, true olarak ayarlanırsa bir özel imzalama anahtarı belirtmeden eşlemesi kullanmak için bir uygulama talep.|
|Giriş sayfası     |  string       |`http://MyRegistererdApp`         |    Uygulama giriş sayfası URL'si.     |
|identifierUris     |  Dize dizisi       | `http://MyRegistererdApp`        |   Uygulama çok kiracılı ise, bir Web uygulaması özel doğrulanmış bir etki alanı veya kendi Azure AD kiracısı içinde benzersiz olarak tanımlayan kullanıcı tanımlı URI(s).      |
|keyCredentials     |   Dizi türü      | <code>[{<br>&nbsp;"customKeyIdentifier":null,<br>"endDate":"2018-09-13T00:00:00Z",<br>"keyId":"\<guid>",<br>"startDate":"2017-09-12T00:00:00Z",<br>"type":"AsymmetricX509Cert",<br>"usage":"Verify",<br>"value":null<br>}]</code>      |   Bu özellik atanan uygulama kimlik bilgileri, paylaşılan gizlilikler dize tabanlıdır ve X.509 sertifikalarını başvurular içerir.  Bu kimlik bilgileri erişim belirteçleri isterken kullanılır (zaman uygulama işlevi gören bir istemci olarak yerine, kaynak olarak).     |
|knownClientApplications     |     Dizi türü    |    [GUID]     |     Bir istemci uygulaması ve özel bir web API uygulamasını olmak üzere iki bölümden içeren bir çözüm varsa, değer paket onay için kullanılır. İstemci uygulamasının AppID bu değer girerseniz, kullanıcı yalnızca bir kez istemci uygulamaya kabul sahip olur. Azure AD istemciye onaylıyorsunuz örtük olarak web API onaylıyorsunuz anlamına gelir ve hem istemci hem de web API'si için hizmet asıl adı aynı anda otomatik olarak hazırlayacağınız olduğunu bilirsiniz.  Hem istemci hem de web API uygulamasını aynı Kiracı içinde kayıtlı olması gerekir.|
|logoutUrl     |   string      |     `http://MyRegisteredAppLogout`    |   Uygulamanın oturum kapatma URL'si.      |
|oauth2AllowImplicitFlow     |   boole      |  `false`       |       Bu web uygulaması OAuth2.0 örtük akış belirteçleri isteyip isteyemeyeceklerini belirtir. Varsayılan değer false. Bu bayrak, Javascript tek sayfa uygulamaları gibi tarayıcı tabanlı uygulamalar için kullanılır. |
|oauth2AllowUrlPathMatching     |   boole      |  `false`       |   OAuth 2.0 belirteç isteklerini bir parçası olarak, Azure AD yolu uygulamanın replyUrls karşı yeniden yönlendirme URI'si eşleştirilmesini izin verip vermeyeceğini, belirtir. Varsayılan değer false.      |
|oauth2Permissions     | Dizi türü         |      <code>[{<br>"adminConsentDescription":"Allow the application to access resources on behalf of the signed-in user.",<br>"adminConsentDisplayName":"Access resource1",<br>"id":"\<guid>",<br>"isEnabled":true,<br>"type":"User",<br>"userConsentDescription":"Allow the application to access resource1 on your behalf.",<br>"userConsentDisplayName":"Access resources",<br>"value":"user_impersonation"<br>}]  </code> |  İstemci uygulamaları için web API (kaynak) uygulama sunan OAuth 2.0 izin kapsamları koleksiyonu. Bu izin kapsamları istemci uygulamalar sırasında izin verilebilir. |
|oauth2RequiredPostResponse     | boole        |    `false`     |      OAuth 2.0 belirteç isteklerini bir parçası olarak, Azure AD GET istekleri aksine POST istekleri izin olup olmadığını belirtir. Yalnızca GET isteklerini verileceğini belirten varsayılan false değeridir.   
|objectId     | Kimlik dizesi        |     ""    |    Dizinde uygulama için benzersiz tanımlayıcı.  Bu kimliği herhangi bir protokolü işlem uygulamada tanımlamak için kullanılan tanımlayıcı değil.  Dizin sorguları nesnesinde başvurmak için kullanıcı değil.|
|passwordCredentials     | Dizi türü        |   <code>[{<br>"customKeyIdentifier":null,<br>"endDate":"2018-10-19T17:59:59.6521653Z",<br>"keyId":"\<guid>",<br>"startDate":"2016-10-19T17:59:59.6521653Z",<br>"value":null<br>}]  </code>    |    KeyCredentials özelliği açıklamasına bakın.     |
|PublicClient     |  boole       |      `false`   | Bir uygulamanın genel bir istemci (örneğin, bir mobil cihazda çalışan yüklü bir uygulama) olup olmadığını belirtir. Varsayılan false olur.        |
|supportsConvergence     |  boole       |   `false`      | Bu özelliği düzenlenmemelidir.  Varsayılan değerini kabul edin.        |
|replyUrls     |  Dize dizisi       |   `http://localhost`     |  Bu çok değerli özellik belirteçleri döndürülürken hedefleri olarak Azure AD kabul edeceği kayıtlı redirect_uri değerlerin listesini tutar. |
|requiredResourceAccess     |     Dizi türü    |    <code>[{<br>"resourceAppId":"00000002-0000-0000-c000-000000000000",<br>"resourceAccess":[{<br>&nbsp;&nbsp;&nbsp;&nbsp;"id":"311a71cc-e848-46a1-bdf8-97ff7156d8e6",<br>&nbsp;&nbsp;&nbsp;&nbsp;"type":"Scope"<br>&nbsp;&nbsp;}]<br>}] </code>    |   Bu uygulama için erişim ve OAuth izin kapsamları ve her kaynaklarla altında gereken uygulama rolleri kümesi gerektirir kaynaklarını belirtir. Bu ön yapılandırma gerekli kaynağa erişim onayı deneyimi sürücüleri.|
|resourceAppId     |    Kimlik dizesi     |  ""      |   Uygulama erişim gerektiren kaynak için benzersiz tanımlayıcı. Bu değer, hedef kaynak uygulamanın bildirilen AppID eşit olmalıdır.     |
|resourceAccess     |  Dizi türü       | Örnek değer requiredResourceAccess özelliği için bkz.        |   OAuth2.0 izin kapsamları ve uygulama (belirtilen kaynaklar kimliği ve türü değerleri içerir) belirtilen kaynak gerektiriyor uygulama rolleri listesi        |
|samlMetadataUrl    |string| `http://MyRegisteredAppSAMLMetadata` |Uygulama SAML meta verilerinin URL'si.| 

## <a name="next-steps"></a>Sonraki adımlar
* Bir uygulamanın uygulama ve hizmet sorumlusu nesneleri arasındaki ilişki hakkında daha fazla bilgi için bkz: [uygulama ve hizmet asıl nesneleri Azure AD'de][AAD-APP-OBJECTS].
* Bkz: [Azure AD Geliştirici sözlüğü] [ AAD-DEVELOPER-GLOSSARY] bazı Azure Active Directory (AD) Geliştirici kavramları tanımları için.

Daraltın ve içeriği şekil yardımcı geri bildirim sağlamak için aşağıdaki açıklamaları bölümü kullanın.

<!--article references -->
[AAD-APP-OBJECTS]: active-directory-application-objects.md
[AAD-DEVELOPER-GLOSSARY]: active-directory-dev-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]: active-directory-integrating-applications.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-PORTAL]: https://portal.azure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]: active-directory-dev-understanding-oauth2-implicit-grant.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/

