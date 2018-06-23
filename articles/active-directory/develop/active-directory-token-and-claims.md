---
title: Farklı belirtecin hakkında bilgi edinin ve talep türleri Azure AD tarafından desteklenen | Microsoft Docs
description: Anlama ve Azure Active Directory (AAD tarafından) yayınlanan SAML 2.0 ve JSON Web belirteçleri (JWT) belirteçleri Taleplerde değerlendirme kılavuzu
documentationcenter: na
author: CelesteDG
services: active-directory
manager: mtillman
editor: ''
ms.assetid: 166aa18e-1746-4c5e-b382-68338af921e2
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/22/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: a12ac87eba14db4ff13868446cf8d14b10d1f5fb
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36317835"
---
# <a name="azure-ad-token-reference"></a>Azure AD belirteç başvurusu
Azure Active Directory (Azure AD), her kimlik doğrulama akışı işlenmesini güvenlik belirteçlerinde çeşitli türlerde yayar. Bu belgede biçimi, güvenlik özellikleri ve her tür bir belirteç içeriği açıklanmaktadır. 

## <a name="types-of-tokens"></a>Belirteç türleri
Azure AD destekler [OAuth 2.0 yetkilendirme protokolünü](active-directory-protocols-oauth-code.md), hangi yapar access_tokens ve refresh_tokens kullanın. Kimlik doğrulama ve oturum açma aracılığıyla da destekler [Openıd Connect](active-directory-protocols-openid-connect-code.md), belirteç, id_token üçüncü türü sunar. Bu belirteçler her "taşıyıcı belirteç" olarak gösterilir.

Bir taşıyıcı belirteci korunan bir kaynağa "bearer" erişim veren bir basit güvenlik belirtecidir. Bu anlamda belirteç sunabilir herhangi bir tarafa "bearer" dir. Azure AD ile kimlik doğrulaması bir taşıyıcı belirteci almak için gerekli olan ancak istenmeyen bir taraf tarafından kişiler tarafından ele önlemek için belirteç, güvenli hale getirmek için adımları alınması gerekir. Taşıyıcı belirteçlerini yetkisiz tarafların onları kullanmasına engel olmak için yerleşik bir mekanizma olmadığı için Aktarım Katmanı Güvenliği (HTTPS) gibi güvenli bir kanal taşınan gerekir. Bir taşıyıcı belirteci açık bir şekilde iletilirse, bir adam-Orta saldırı belirtecini almak ve korunan bir kaynağa yetkisiz erişim kazanmak için kullanılabilir. Depolama veya taşıyıcı belirteçlerini daha sonra kullanmak için önbelleğe alma aynı güvenlik ilkeleri uygulayın. Her zaman uygulamanızı aktarır ve güvenli bir şekilde taşıyıcı belirteçleri depolar emin olun. Taşıyıcı belirteçlerini hakkında daha fazla güvenlik konuları için bkz: [RFC 6750 bölüm 5](http://tools.ietf.org/html/rfc6750).

Azure AD tarafından yayınlanan belirteçleri birçoğu, JSON Web belirteçlerini veya Jwt'ler olarak uygulanır. JWT, iki taraf arasında bilgi aktarma compact, URL için güvenli bir yoludur. Jwt'ler içinde yer alan bilgileri bilinir "talep" ya da bilgi onaylar belirtecin konu ve taşıyıcı hakkında. Jwt'ler Taleplerde kodlanmış ve aktarım için seri JSON nesneleridir. Azure AD tarafından verilen Jwt'ler imzalanmış, ancak şifreli olduğundan, hata ayıklama amacıyla JWT içeriğini kolayca inceleyebilirsiniz. Bunu aşağıdaki gibi yapmak kullanılabilen çeşitli araçlar vardır [jwt.ms](https://jwt.ms/). Jwt'ler hakkında daha fazla bilgi için başvurabilirsiniz [JWT belirtimi](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

## <a name="idtokens"></a>Id_tokens
Id_tokens olan bir form kimlik doğrulaması kullanarak gerçekleştirirken uygulamanızı alan oturum açma güvenlik belirtecinin [Openıd Connect](active-directory-protocols-openid-connect-code.md). Olarak temsil [Jwt'ler](#types-of-tokens)ve kullanıcının uygulamanıza imzalamak için kullanabileceğiniz talepleri içerir. Talepler bir id_token kullanabileceğiniz uygun gördüğünüz - hesap bilgilerini görüntüleme veya erişim denetimi kararları bir uygulama için sık kullanılan olarak.

Id_tokens imzalanmış, ancak şu anda şifrelenmez. Uygulamanızı bir id_token aldığında, gerekir [imzayı doğrulamak](#validating-tokens) belirtecin Orijinallik kanıtlamak ve geçerliliğini kanıtlamak için birkaç talep belirteci doğrulamak için. Bir uygulama tarafından doğrulanmış talep senaryonun gereksinimlerine bağlı olarak farklılık gösterir, ancak bazı vardır [ortak talep doğrulamaları](#validating-tokens) , uygulamanızı her senaryoda gerçekleştirmeniz gerekir.

Bilgi için aşağıdaki bölümde id_tokens talep yanı sıra üzerinde bir örnek id_token bakın. İd_tokens talepleri belirli bir sırada döndürülmez unutmayın. Ayrıca, yeni talep id_tokens herhangi bir noktada içine zamanında tanıtılabilir - yeni talep sunulduğu şekilde uygulamanızı parçalara değil. Aşağıdaki listede, uygulamanızı güvenilir bir şekilde bu makalenin yazıldığı sırada çevirebilir talepleri içerir. Gerekirse, daha fazla ayrıntı bulunabilir, [Openıd Connect belirtimi](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-idtoken"></a>Örnek id_token
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.
```

> [!TIP]
> Uygulama için örnek id_token Taleplerde içine yapıştırarak inceleniyor deneyin [jwt.ms](https://jwt.ms/).
> 
> 

#### <a name="claims-in-idtokens"></a>İd_tokens talepleri
> [!div class="mx-codeBreakAll"]
| JWT talep | Ad | Açıklama |
| --- | --- | --- |
| `aud` |Hedef kitle |Belirtecin hedeflenen alıcı. Belirteç alan uygulama İzleyici değerin doğru olduğundan ve farklı bir izleyici için amaçlanan herhangi bir belirtece Reddet doğrulamanız gerekir. <br><br> **Örnek SAML değer**: <br> `<AudienceRestriction>`<br>`<Audience>`<br>`https://contoso.com`<br>`</Audience>`<br>`</AudienceRestriction>` <br><br> **Örnek JWT değer**: <br> `"aud":"https://contoso.com"` |
| `appidacr` |Uygulama kimlik doğrulama bağlamı sınıf başvurusu |İstemcinin kimlik doğrulamasının nasıl yapıldığı gösterir. Ortak bir istemci için değeri 0'dır. İstemci Kimliğini ve istemci gizli anahtarı kullandıysanız, değer 1'dir. Bir istemci sertifikası kimlik doğrulaması için kullanıldıysa, değer 2'dir. <br><br> **Örnek JWT değer**: <br> `"appidacr": "0"` |
| `acr` |Kimlik doğrulama bağlamı sınıf başvurusu |Nasıl konu, uygulama kimlik doğrulama bağlamı sınıf başvurusu talep istemcisinde aksine doğrulanmış olduğunu gösterir. Son kullanıcı kimlik doğrulaması ISO/IEC 29115 gereksinimlerini karşılamayan "0" değerini gösterir. <br><br> **Örnek JWT değer**: <br> `"acr": "0"` |
| Anlık kimlik doğrulaması |Kimlik doğrulama gerçekleştiği saat ve tarihi kaydeder. <br><br> **Örnek SAML değer**: <br> `<AuthnStatement AuthnInstant="2011-12-29T05:35:22.000Z">` | |
| `amr` |Kimlik Doğrulama Yöntemi |Belirteç konu nasıl doğrulandı tanımlar. <br><br> **Örnek SAML değer**: <br> `<AuthnContextClassRef>`<br>`http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod/password`<br>`</AuthnContextClassRef>` <br><br> **Örnek JWT değer**: `“amr”: ["pwd"]` |
| `given_name` |Ad |İlk sağlar ya da "Azure AD kullanıcı nesnesindeki belirlenen kullanıcı adı verilen". <br><br> **Örnek SAML değer**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname”>`<br>`<AttributeValue>Frank<AttributeValue>` <br><br> **Örnek JWT değer**: <br> `"given_name": "Frank"` |
| `groups` |Gruplar |İlgilinin grup üyelikleri temsil eden nesne kimlikleri sağlar. Bu değerler benzersiz (nesne kimliği bakın) ve güvenli bir şekilde bir kaynağa erişim yetkisi zorlama gibi erişimi yönetmek için kullanılabilir. Grupları talep kümesinde bulunan grupları uygulama bildiriminin "groupMembershipClaims" özelliği aracılığıyla bir uygulama başına temelinde yapılandırılır. Bir null değeri tüm grupları dışlayacak, Active Directory güvenlik grubu üyeliği ve bir değer "Tümü" bırakılacak güvenlik grupları ve Office 365 dağıtım listeleri içerir yalnızca "Güvenlik grubuna" değerini içerir. <br><br> **Notlar**: <br> Bkz: `hasgroups` kullanımıyla ilgili ayrıntılar için aşağıda talep `groups` ile örtük grant talep. <br> Bir fazla kullanım talep sonra kullanıcının bulunduğu grupların sayısı sınırı (150 SAML, JWT için 200 için) üzerinden gider değilse, diğer akışlar için kullanıcı gruplarının listesini içeren grafik uç noktada işaret eden talep kaynakları eklenir. (içinde. <br><br> **Örnek SAML değer**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">`<br>`<AttributeValue>07dd8a60-bf6d-4e17-8844-230b77145381</AttributeValue>` <br><br> **Örnek JWT değer**: <br> `“groups”: ["0e129f5b-6b0a-4944-982d-f776045632af", … ]` |
|`hasgroups` | JWT örtük akış grupları aşma göstergesi| Varsa, her zaman `true`, kullanıcı belirten olan en az bir grubunda. Yerine kullanılan `groups` tam grupları talep varsa örtük grant akışları Jwt'ler için talep URI parça URL uzunluğu sınırları (şu anda 6 veya daha fazla grup) ötesine genişletmek. Kullanıcı grupları belirlemek için istemci grafiği kullanması gerektiğini gösterir (`https://graph.windows.net/{tenantID}/users/{userID}/getMemberObjects`). |
| `groups:src1` <br> `http://schemas.microsoft.com/claims/groups.link` | Grupları aşma göstergesi | Sınırlı uzunluğu olmayan belirteci isteği için (bkz `hasgroups` yukarıda) ancak hala çok büyük belirteç için kullanıcı için tam grupları listesi bağlantı dahil edilir. SAML yerine yeni bir talep olarak için Dağıtılmış bir talep olarak Jwt'ler için `groups` talep. <br><br> **Örnek SAML değer**: <br> `<Attribute Name=” http://schemas.microsoft.com/claims/groups.link”>`<br>`<AttributeValue>https://graph.windows.net/{tenantID}/users/{userID}/getMemberObjects<AttributeValue>` <br><br> **Örnek JWT değer**: <br> `"groups":"src1` <br> `_claim_sources`: `"src1" : { "endpoint" : "https://graph.windows.net/{tenantID}/users/{userID}/getMemberObjects" }`|
| `idp` |Kimlik Sağlayıcısı |Belirtecin konusu kimlik doğrulaması kimlik sağlayıcısı kaydeder. Dağıtımcı'den farklı bir kiracı kullanıcı hesabıdır sürece bu verenin talep değeriyle aynı bir değerdir. <br><br> **Örnek SAML değer**: <br> `<Attribute Name=” http://schemas.microsoft.com/identity/claims/identityprovider”>`<br>`<AttributeValue>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/<AttributeValue>` <br><br> **Örnek JWT değer**: <br> `"idp":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `iat` |IssuedAt |Belirteç düzenlendiği zaman depolar. Genellikle, belirteç yenilik ölçmek için kullanılır. <br><br> **Örnek SAML değer**: <br> `<Assertion ID="_d5ec7a9b-8d8f-4b44-8c94-9812612142be" IssueInstant="2014-01-06T20:20:23.085Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">` <br><br> **Örnek JWT değer**: <br> `"iat": 1390234181` |
| `iss` |Sertifikayı Veren |Oluşturur ve belirteci döndüren güvenlik belirteci hizmeti (STS) tanımlar. Azure AD döndürür belirteçlerinde veren sts.windows.net ' dir. Azure AD dizini Kiracı kimliği verenin talep değeri GUID'dir. Kiracı kimliği dizinin değişmez ve güvenilir bir tanımlayıcıdır. <br><br> **Örnek SAML değer**: <br> `<Issuer>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/</Issuer>` <br><br> **Örnek JWT değer**: <br>  `"iss":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `family_name` |Soyadı |Son adını, soyadını veya kullanıcının aile adını Azure AD kullanıcı nesnesinde tanımlanan sağlar. <br><br> **Örnek SAML değer**: <br> `<Attribute Name=” http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname”>`<br>`<AttributeValue>Miller<AttributeValue>` <br><br> **Örnek JWT değer**: <br> `"family_name": "Miller"` |
| `unique_name` |Ad |Belirteç konu tanımlayan bir insan okunabilir değer sağlar. Bu değer, bir kiracı içinde benzersiz olması garanti edilmez ve yalnızca görüntüleme amacıyla kullanılmak üzere tasarlanmıştır. <br><br> **Örnek SAML değer**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name”>`<br>`<AttributeValue>frankm@contoso.com<AttributeValue>` <br><br> **Örnek JWT değer**: <br> `"unique_name": "frankm@contoso.com"` |
| `oid` |Nesne kimliği |Bir nesnenin benzersiz bir tanımlayıcı Azure AD'de içerir. Bu değer sabittir ve yeniden atandığında yeniden ya da silinemez. Nesne kimliği, Azure ad sorgularda nesneyi tanımlamak için kullanın. <br><br> **Örnek SAML değer**: <br> `<Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">`<br>`<AttributeValue>528b2ac2-aa9c-45e1-88d4-959b53bc7dd0<AttributeValue>` <br><br> **Örnek JWT değer**: <br> `"oid":"528b2ac2-aa9c-45e1-88d4-959b53bc7dd0"` |
| `roles` |Roller |Konu doğrudan ve dolaylı olarak Grup üyeliğiyle verildi ve rol tabanlı erişim denetimi uygulamak için kullanılan tüm uygulama rolleri temsil eder. Uygulama rolleri aracılığıyla bir uygulama başına temelinde tanımlanan `appRoles` uygulama bildiriminin özelliği. `value` Rolleri talep görüntülenen değer her uygulama rolü özelliğidir. <br><br> **Örnek SAML değer**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/role">`<br>`<AttributeValue>Admin</AttributeValue>` <br><br> **Örnek JWT değer**: <br> `“roles”: ["Admin", … ]` |
| `scp` |Kapsam |İstemci uygulaması için kimliğe bürünme izinler gösterir. Varsayılan izni `user_impersonation`. Güvenli kaynağın sahibi Azure AD içinde ek değerler kaydedebilirsiniz. <br><br> **Örnek JWT değer**: <br> `"scp": "user_impersonation"` |
| `sub` |Konu |Hakkında bilgi, bir uygulamanın kullanıcısı gibi belirteci onaylar asıl tanımlar. Bu değer sabittir ve atanamaz veya yeniden, bu nedenle bu yetkilendirme denetimleri güvenli bir şekilde gerçekleştirmek için kullanılabilir. Konu her zaman Azure AD sorunları belirteçlerinde mevcut olduğundan, bu değerin bir genel amaçlı yetkilendirme sisteminde kullanılması önerilir. <br> `SubjectConfirmation` bir talep değil. Bu konu belirtecin nasıl doğrulanır açıklar. `Bearer` Konu belirteç kendi mülkü tarafından onaylandıktan gösterir. <br><br> **Örnek SAML değer**: <br> `<Subject>`<br>`<NameID>S40rgb3XjhFTv6EQTETkEzcgVmToHKRkZUIsJlmLdVc</NameID>`<br>`<SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />`<br>`</Subject>` <br><br> **Örnek JWT değer**: <br> `"sub":"92d0312b-26b9-4887-a338-7b00fb3c5eab"` |
| `tid` |Kiracı Kimliği |Belirtecin dizin Kiracı tanımlayan bir değişmez, yeniden kullanılabilir olmayan tanımlayıcısı. Çok kiracılı uygulama Kiracı özgü dizin kaynaklara erişim için bu değeri kullanın. Örneğin, Kiracı grafik API'sine çağrıda tanımlamak için bu değeri kullanın. <br><br> **Örnek SAML değer**: <br> `<Attribute Name=”http://schemas.microsoft.com/identity/claims/tenantid”>`<br>`<AttributeValue>cbb1a5ac-f33b-45fa-9bf5-f37db0fed422<AttributeValue>` <br><br> **Örnek JWT değer**: <br> `"tid":"cbb1a5ac-f33b-45fa-9bf5-f37db0fed422"` |
| `nbf`, `exp` |Belirteç ömrü |Bir belirtecin geçerli olduğu zaman aralığını tanımlar. Belirteci doğrular hizmeti geçerli tarih belirteci reddetme belirteç ömrü içinde başka olduğunu doğrulamanız gerekir. Hizmet için beş dakika belirteç ömrü aralık ötesinde farklılıklarını saatin ("saat eğriltme") için hesap için Azure AD arasında izin verebilir ve hizmeti. <br><br> **Örnek SAML değer**: <br> `<Conditions`<br>`NotBefore="2013-03-18T21:32:51.261Z"`<br>`NotOnOrAfter="2013-03-18T22:32:51.261Z"`<br>`>` <br><br> **Örnek JWT değer**: <br> `"nbf":1363289634, "exp":1363293234` |
| `upn` |Kullanıcı Asıl Adı |Kullanıcı asıl kullanıcı adını depolar.<br><br> **Örnek JWT değer**: <br> `"upn": frankm@contoso.com` |
| `ver` |Sürüm |Belirteç sürüm numarasını depolar. <br><br> **Örnek JWT değer**: <br> `"ver": "1.0"` |

## <a name="access-tokens"></a>Erişim belirteçleri
Başarılı kimlik doğrulaması sırasında Azure AD korunan kaynaklara erişim için kullanılan bir erişim belirteci döndürür. Erişim belirteci JSON Web Token (JWT) bir base 64 kodlu ve içeriğinin bir kod çözücü çalıştırarak denetlenecek ' dir.

Varsa, uygulamanızın yalnızca *kullanır* erişmek için erişim belirteçleri için API ' ları ve gereken kabul erişim belirteçleri olarak tamamen opak - uygulamanızı HTTP isteklerini kaynaklarında geçirebilirsiniz yalnızca dizeleri oldukları.

Bir erişim belirteci istediğinde, Azure AD Ayrıca, uygulamanızın tüketimi için erişim belirtecini hakkında bazı meta verileri döndürür. Bu bilgiler erişim belirteci kapsamlar için geçerlidir ve sona erme saati içerir. Bu, erişim belirteci akıllı erişim belirteçleri açık ayrıştırmak zorunda kalmadan önbelleğe alma işlemi için uygulamanızı sağlar.

Ardından, uygulamanızın HTTP isteklerine erişim belirteçleri bekliyor Azure AD ile korunan bir API ise, doğrulama ve İnceleme aldığınız belirteçlerin gerçekleştirmeniz gerekir. Uygulamanızı kaynaklara erişmek için kullanmadan önce erişim belirteci doğrulaması gerçekleştirmeniz gerekir. Doğrulama hakkında daha fazla bilgi için lütfen bkz [doğrulama belirteçleri](#validating-tokens). .NET ile nasıl yapılacağı hakkında daha fazla bilgi için bkz: [Azure AD'den taşıyıcı belirteçlerini kullanarak bir Web API korumak](active-directory-devquickstarts-webapi-dotnet.md).

## <a name="refresh-tokens"></a>Yenileme belirteçlerini

Yenileme belirteçleri olan bir OAuth 2.0 akışı yeni erişim belirteçleri almak için uygulamanızı kullanabilir güvenlik belirteçleri. Uzun vadeli kaynaklara erişimi bir kullanıcı adına kullanıcı etkileşimi gerektirmeden elde etmek uygulamanızı sağlar.

Yenileme belirteçleri çok kaynak. Yani bir kaynak için bir belirteç isteğini sırasında alınan bir yenileme belirteci erişebileceği erişim belirteçleri tamamen farklı bir kaynak için kullanılan. Bunu yapmak için ayarlayın `resource` hedeflenen kaynak isteğinde parametresi.

Yenileme belirteçleri, uygulamanızın tamamen opak. Uzun süreli, ancak bir yenileme belirteci süre boyunca sürer beklenir uygulamanızı yazılmamalıysa. Yenileme belirteçleri geçersiz olabilir, çeşitli nedenlerle - zamanında herhangi bir anda bkz [belirteci iptal](#token-revocation) bu nedenlerle. Tek bir yenileme belirteci geçerli olup olmadığını bilmek, uygulamanız için Azure AD belirteç uç noktası için bir belirteç isteğini yaparak kullanma girişiminde yoludur.

Yeni bir erişim belirteci için bir yenileme belirteci almak, belirteç yanıt olarak yeni bir yenileme belirteci alır. İstekte kullanılan değiştirme, yeni verilen yenileme belirteci kaydetmeniz gerekir. Bu, yenileme belirteçleri mümkün olduğunca uzun bir süre geçerli kalacağını garanti.

## <a name="validating-tokens"></a>Belirteçleri doğrulanıyor

Bir id_token veya bir access_token doğrulamak için uygulamanızı belirtecinin imzası ve talep doğrulamalıdır. Erişim belirteçleri doğrulamak için uygulamanız da veren, İzleyici ve imzalama belirteçleri doğrulamalıdır. Bunlar, Openıd bulma belgesindeki değerler karşı doğrulanması gerekir. Örneğin, belge Kiracı bağımsız sürümü bulunur [ https://login.microsoftonline.com/common/.well-known/openid-configuration ](https://login.microsoftonline.com/common/.well-known/openid-configuration). Azure AD Ara erişim belirteçleri doğrulamak için yerleşik özellikleri vardır ve aracılığıyla göz atabilirsiniz bizim [örnekleri](https://docs.microsoft.com/azure/active-directory/active-directory-code-samples) tercih ettiğiniz dilde bulmak için. Açıkça bir JWT belirteci doğrulama hakkında daha fazla bilgi için lütfen bkz [el ile JWT doğrulama örnek](https://github.com/Azure-Samples/active-directory-dotnet-webapi-manual-jwt-validation). 

Kitaplıkları ve kolayca belirteci doğrulama - nasıl ele alınacağını gösteren kod örnekleri sunuyoruz altındaki bilgi yalnızca temel alınan işlem anlamak isteyen olanlar için sağlanır. Ayrıca çeşitli üçüncü taraf açık kaynak kitaplıkları JWT doğrulama için kullanılabilir olduğundan - neredeyse her platform ve ölçeklendiriyor dili için en az bir seçenek yoktur. Azure AD kimlik doğrulama kitaplıkları ve kod örnekleri hakkında daha fazla bilgi için lütfen bkz [Azure AD kimlik doğrulama kitaplıkları](active-directory-authentication-libraries.md).

#### <a name="validating-the-signature"></a>İmza doğrulama

JWT'nin tarafından ayrılmış üç segmentleri içerir `.` karakter. İlk kesim olarak bilinen **üstbilgi**, saniye olarak **gövde**ve üçüncü olarak **imza**. İmza kesimi, böylece uygulamanız tarafından güvenilen belirteç özgünlüğünü doğrulamak için kullanılabilir.

Azure AD tarafından yayınlanan belirteçleri RSA 256 gibi endüstri standart asimetrik şifreleme algoritmaları kullanılarak imzalanmıştır. JWT üstbilgisi belirteç imzalamak için kullanılan anahtar ve şifreleme yöntemi hakkında bilgi içerir:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "x5t": "iBjL1Rcqzhiy4fpxIxdZqohM2Yk"
  "kid": "iBjL1Rcqzhiy4fpxIxdZqohM2Yk"
}
```

`alg` Talep belirteci, while imzalamak için kullanılan algoritmayı gösterir `x5t` talep belirteç imzalamak için kullanılan belirli ortak anahtarı gösterir.

Herhangi bir anda zaman, belirli bir genel-özel anahtar çiftleri kümesini herhangi birini kullanarak bir id_token Azure AD oturum. Bu anahtar değişiklikleri otomatik olarak işlemek için uygulamanızı yazılması gereken şekilde azure AD anahtarları düzenli aralıklarla, olası kümesini döndürür. 24 saatte bir Azure AD tarafından kullanılan ortak anahtarlar için güncelleştirmeleri denetlemek için makul bir sıklığıdır.

Konumunda bulunan Openıd Connect meta veri belgesi kullanarak imzayı doğrulamak gereken imzalama anahtar verileri elde edebilirsiniz:

```
https://login.microsoftonline.com/common/.well-known/openid-configuration
```

> [!TIP]
> Bu URL'yi bir tarayıcıda deneyin!

Bu meta veri belgesi, bilgi, Openıd Connect kimlik doğrulama gerçekleştirmek için gereken çeşitli uç noktaları konumu gibi çeşitli yararlı parçalarını içeren bir JSON nesnesidir. 

Ayrıca içeren bir `jwks_uri`, Belirteçleri imzalamak için kullanılan ortak anahtarlar konumunu sağlar. JSON belgesini konumundaki `jwks_uri` tüm zaman içinde belirli o anda kullanımda ortak anahtar bilgileri içerir. Uygulamanızı kullanabilirsiniz `kid` hangi ortak anahtar bu belgedeki belirli bir belirteç imzalamak için kullanılan seçmek için JWT üstbilgisinde talep. Ardından, doğru ortak anahtar ve belirtilen algoritmasını kullanarak imza doğrulaması de gerçekleştirebilirsiniz.

> [!NOTE]
> Her ikisi de v1.0 son noktasını döndürür `x5t` ve `kid` talep. `x5t` Talep v2.0 belirteçlerinden eksik. V2.0 uç noktası ile yanıt `kid` talep. İleriye dönük kullanmanızı öneririz `kid` belirtecinizi doğrulamak talep.

İmza doğrulaması gerçekleştirme, bu belgenin kapsamı dışında - birçok açık kaynak kitaplıkları, gerekirse, bunu yardımcı olduğunuz için kullanılabilir.

#### <a name="validating-the-claims"></a>Talep doğrulama

Uygulamanızı bir belirteç (kullanıcı oturum açma sırasında bir id_token ya da bir erişim belirteci HTTP isteği bir taşıyıcı belirteci olarak) aldığında, talepleri karşı birkaç denetimleri belirteç de gerçekleştirmeniz gerekir. Bunlar dahil ancak bunlarla sınırlı değildir:

* **İzleyici** - belirteç uygulamanıza verilecek tasarlanmıştır doğrulamak için talep.
* **Önce değil** ve **süre sonu** - belirtecin süresi geçmemiş doğrulamak için talep.
* **Veren** - belirteç gerçekten, uygulamanızın Azure AD tarafından verilmiş olduğunu doğrulamak için talep.
* **Nonce** - belirteç yeniden yürütme saldırının etkisini azaltmak için.
* ve daha fazlası...

Talep doğrulama uygulamanız için kimlik belirteçlerini gerçekleştirmesi gerektiğini tam listesi için bkz [Openıd Connect belirtimi](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation). Bu talep beklenen değerler ayrıntılarını yer alan önceki [id_token](#id-tokens) bölümü.

## <a name="token-revocation"></a>Belirteci iptal

Yenileme belirteçleri geçersiz kılınan veya çeşitli nedenlerle için herhangi bir zamanda iptal edildi. Bu iki ana kategoriye ayrılır: zaman aşımları ve iptalleri. 

**Belirteci zaman aşımları**

* MaxInactiveTime: yenileme belirtecini MaxInactiveTime tarafından dikte sürede kullanılmamış, yenileme belirteci artık geçerli olacaktır. 
* MaxSessionAge: MaxAgeSessionMultiFactor veya MaxAgeSessionSingleFactor (kadar iptal edilen) varsayılan dışında bir şey için ayarlanmışsa, belirlenen MaxAgeSession * içinde süresi sona erdiğinde daha sonra yeniden kimlik doğrulaması gerekir. 
* Örnekler:
  * Kiracı 5 gün MaxInactiveTime sahip kullanıcı için bir hafta tatile oluştu ve bu nedenle AAD kullanıcıdan yeni bir belirteç isteğini 7 gün içinde görmediği. Kullanıcı yeni bir belirteç istediğinde bunlar kendi yenileme belirteci iptal edilmiş ve bunlar kimlik bilgilerini yeniden girmeniz gerekir bulacaksınız. 
  * Duyarlı bir uygulamaya MaxAgeSessionSingleFactor 1 gün sahiptir. Pazartesi ve Salı (25 saat geçtikten sonra) bir kullanıcı oturum açarsa, yeniden kimlik doğrulama yapılması gerekir. 

**İptal etme**

|   | Parola tabanlı tanımlama bilgisi | Parola tabanlı belirteci | Olmayan parola tabanlı tanımlama bilgisi | Olmayan parola tabanlı simgesi | Gizli istemci belirteci| 
|---|-----------------------|----------------------|---------------------------|--------------------------|--------------------------|
|Parola süre sonu| Canlı kalır|Canlı kalır|Canlı kalır|Canlı kalır|Canlı kalır|
|Parola kullanıcı tarafından değiştirildi| İptal edildi | İptal edildi | Canlı kalır|Canlı kalır|Canlı kalır|
|SSPR kullanıcı mu|İptal edildi | İptal edildi | Canlı kalır|Canlı kalır|Canlı kalır|
|Yönetici parolasını sıfırlar|İptal edildi | İptal edildi | Canlı kalır|Canlı kalır|Canlı kalır|
|Kullanıcı iptal, yenileme belirteçleri [PowerShell aracılığıyla](https://docs.microsoft.com/powershell/module/azuread/revoke-azureadsignedinuserallrefreshtoken) | İptal edildi | İptal edildi |İptal edildi | İptal edildi |İptal edildi | İptal edildi |
|Yönetici, Kiracı için tüm yenileme belirteçleri iptal [PowerShell aracılığıyla](https://docs.microsoft.com/powershell/module/azuread/revoke-azureaduserallrefreshtoken) | İptal edildi | İptal edildi |İptal edildi | İptal edildi |İptal edildi | İptal edildi |
|[Çoklu oturum çıkışı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code#single-sign-out) Web'de | İptal edildi | Canlı kalır |İptal edildi | Canlı kalır |Canlı kalır |Canlı kalır |

> [!NOTE]
> "Olmayan parola tabanlı" bir oturum açma kullanıcı almak için bir parola girildiği kaydetmedi biridir.  Örneğin, yüz Windows Hello, bir FIDO anahtar veya bir PIN ile kullanma. 
>
> Windows birincil yenileme belirteci ile bilinen bir sorun var.  PRT parola alınır ve ardından kullanıcı oturumu Merhaba, bu PRT oluşturulma değiştirmez ve kullanıcı parolalarını değiştirirse iptal edilir. 

## <a name="sample-tokens"></a>Örnek belirteçleri

Bu bölümde Azure AD döndürür SAML ve JWT belirteçleri örneklerini görüntüler. Bu örnekleri bağlam Taleplerde görmenize olanak tanır.

### <a name="saml-token"></a>SAML belirteci

Bu, tipik bir SAML belirtecine örneğidir.

    <?xml version="1.0" encoding="UTF-8"?>
    <t:RequestSecurityTokenResponse xmlns:t="http://schemas.xmlsoap.org/ws/2005/02/trust">
      <t:Lifetime>
        <wsu:Created xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T05:15:47.060Z</wsu:Created>
        <wsu:Expires xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T06:15:47.060Z</wsu:Expires>
      </t:Lifetime>
      <wsp:AppliesTo xmlns:wsp="http://schemas.xmlsoap.org/ws/2004/09/policy">
        <EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
          <Address>https://contoso.onmicrosoft.com/MyWebApp</Address>
        </EndpointReference>
      </wsp:AppliesTo>
      <t:RequestedSecurityToken>
        <Assertion xmlns="urn:oasis:names:tc:SAML:2.0:assertion" ID="_3ef08993-846b-41de-99df-b7f3ff77671b" IssueInstant="2014-12-24T05:20:47.060Z" Version="2.0">
          <Issuer>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</Issuer>
          <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
            <ds:SignedInfo>
              <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
              <ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
              <ds:Reference URI="#_3ef08993-846b-41de-99df-b7f3ff77671b">
                <ds:Transforms>
                  <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                  <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
                </ds:Transforms>
                <ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <ds:DigestValue>cV1J580U1pD24hEyGuAxrbtgROVyghCqI32UkER/nDY=</ds:DigestValue>
              </ds:Reference>
            </ds:SignedInfo>
            <ds:SignatureValue>j+zPf6mti8Rq4Kyw2NU2nnu0pbJU1z5bR/zDaKaO7FCTdmjUzAvIVfF8pspVR6CbzcYM3HOAmLhuWmBkAAk6qQUBmKsw+XlmF/pB/ivJFdgZSLrtlBs1P/WBV3t04x6fRW4FcIDzh8KhctzJZfS5wGCfYw95er7WJxJi0nU41d7j5HRDidBoXgP755jQu2ZER7wOYZr6ff+ha+/Aj3UMw+8ZtC+WCJC3yyENHDAnp2RfgdElJal68enn668fk8pBDjKDGzbNBO6qBgFPaBT65YvE/tkEmrUxdWkmUKv3y7JWzUYNMD9oUlut93UTyTAIGOs5fvP9ZfK2vNeMVJW7Xg==</ds:SignatureValue>
            <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
              <X509Data>
                <X509Certificate>MIIDPjCCAabcAwIBAgIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTQwMTAxMDcwMDAwWhcNMTYwMTAxMDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAkSCWg6q9iYxvJE2NIhSyOiKvqoWCO2GFipgH0sTSAs5FalHQosk9ZNTztX0ywS/AHsBeQPqYygfYVJL6/EgzVuwRk5txr9e3n1uml94fLyq/AXbwo9yAduf4dCHTP8CWR1dnDR+Qnz/4PYlWVEuuHHONOw/blbfdMjhY+C/BYM2E3pRxbohBb3x//CfueV7ddz2LYiH3wjz0QS/7kjPiNCsXcNyKQEOTkbHFi3mu0u13SQwNddhcynd/GTgWN8A+6SN1r4hzpjFKFLbZnBt77ACSiYx+IHK4Mp+NaVEi5wQtSsjQtI++XsokxRDqYLwus1I1SihgbV/STTg5enufuwIDAQABo2IwYDBeBgNVHQEEVzBVgBDLebM6bK3BjWGqIBrBNFeNoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAA4IBAQCJ4JApryF77EKC4zF5bUaBLQHQ1PNtA1uMDbdNVGKCmSp8M65b8h0NwlIjGGGy/unK8P6jWFdm5IlZ0YPTOgzcRZguXDPj7ajyvlVEQ2K2ICvTYiRQqrOhEhZMSSZsTKXFVwNfW6ADDkN3bvVOVbtpty+nBY5UqnI7xbcoHLZ4wYD251uj5+lo13YLnsVrmQ16NCBYq2nQFNPuNJw6t3XUbwBHXpF46aLT1/eGf/7Xx6iy8yPJX4DyrpFTutDz882RWofGEO5t4Cw+zZg70dJ/hH/ODYRMorfXEW+8uKmXMKmX2wyxMKvfiPbTy5LmAU8Jvjs2tLg4rOBcXWLAIarZ</X509Certificate>
              </X509Data>
            </KeyInfo>
          </ds:Signature>
          <Subject>
            <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">m_H3naDei2LNxUmEcWd0BZlNi_jVET1pMLR6iQSuYmo</NameID>
            <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
          </Subject>
          <Conditions NotBefore="2014-12-24T05:15:47.060Z" NotOnOrAfter="2014-12-24T06:15:47.060Z">
            <AudienceRestriction>
              <Audience>https://contoso.onmicrosoft.com/MyWebApp</Audience>
            </AudienceRestriction>
          </Conditions>
          <AttributeStatement>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
              <AttributeValue>a1addde8-e4f9-4571-ad93-3059e3750d23</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/tenantid">
              <AttributeValue>b9411234-09af-49c2-b0c3-653adc1f376e</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
              <AttributeValue>sample.admin@contoso.onmicrosoft.com</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname">
              <AttributeValue>Admin</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname">
              <AttributeValue>Sample</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">
              <AttributeValue>5581e43f-6096-41d4-8ffa-04e560bab39d</AttributeValue>
              <AttributeValue>07dd8a89-bf6d-4e81-8844-230b77145381</AttributeValue>
              <AttributeValue>0e129f4g-6b0a-4944-982d-f776000632af</AttributeValue>
              <AttributeValue>3ee07328-52ef-4739-a89b-109708c22fb5</AttributeValue>
              <AttributeValue>329k14b3-1851-4b94-947f-9a4dacb595f4</AttributeValue>
              <AttributeValue>6e32c650-9b0a-4491-b429-6c60d2ca9a42</AttributeValue>
              <AttributeValue>f3a169a7-9a58-4e8f-9d47-b70029v07424</AttributeValue>
              <AttributeValue>8e2c86b2-b1ad-476d-9574-544d155aa6ff</AttributeValue>
              <AttributeValue>1bf80264-ff24-4866-b22c-6212e5b9a847</AttributeValue>
              <AttributeValue>4075f9c3-072d-4c32-b542-03e6bc678f3e</AttributeValue>
              <AttributeValue>76f80527-f2cd-46f4-8c52-8jvd8bc749b1</AttributeValue>
              <AttributeValue>0ba31460-44d0-42b5-b90c-47b3fcc48e35</AttributeValue>
              <AttributeValue>edd41703-8652-4948-94a7-2d917bba7667</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/identityprovider">
              <AttributeValue>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</AttributeValue>
            </Attribute>
          </AttributeStatement>
          <AuthnStatement AuthnInstant="2014-12-23T18:51:11.000Z">
            <AuthnContext>
              <AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
            </AuthnContext>
          </AuthnStatement>
        </Assertion>
      </t:RequestedSecurityToken>
      <t:RequestedAttachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedAttachedReference>
      <t:RequestedUnattachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedUnattachedReference>
      <t:TokenType>http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0</t:TokenType>
      <t:RequestType>http://schemas.xmlsoap.org/ws/2005/02/trust/Issue</t:RequestType>
      <t:KeyType>http://schemas.xmlsoap.org/ws/2005/05/identity/NoProofKey</t:KeyType>
    </t:RequestSecurityTokenResponse>

### <a name="jwt-token---user-impersonation"></a>JWT belirteci - kullanıcı kimliğine bürünme
Bir yetkilendirme kodu verme akışında kullanılan bir tipik JSON web token (JWT) örneğidir.
Talepler ek olarak, bir sürüm numarası belirteci içeren **ver** ve **appidacr**, istemcinin kimlik doğrulamasının nasıl yapıldığı gösterir kimlik doğrulama bağlamı sınıf başvurusu. Ortak bir istemci için değeri 0'dır. Bir istemci kimliği veya istemci gizli kullanıldıysa, değer 1'dir.

    {
     typ: "JWT",
     alg: "RS256",
     x5t: "kriMPdmBvx68skT8-mPAB3BseeA"
    }.
    {
     aud: "https://contoso.onmicrosoft.com/scratchservice",
     iss: "https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/",
     iat: 1416968588,
     nbf: 1416968588,
     exp: 1416972488,
     ver: "1.0",
     tid: "b9411234-09af-49c2-b0c3-653adc1f376e",
     amr: [
      "pwd"
     ],
     roles: [
      "Admin"
     ],
     oid: "6526e123-0ff9-4fec-ae64-a8d5a77cf287",
     upn: "sample.user@contoso.onmicrosoft.com",
     unique_name: "sample.user@contoso.onmicrosoft.com",
     sub: "yf8C5e_VRkR1egGxJSDt5_olDFay6L5ilBA81hZhQEI",
     family_name: "User",
     given_name: "Sample",
     groups: [
      "0e129f6b-6b0a-4944-982d-f776000632af",
      "323b13b3-1851-4b94-947f-9a4dacb595f4",
      "6e32c250-9b0a-4491-b429-6c60d2ca9a42",
      "f3a161a7-9a58-4e8f-9d47-b70022a07424",
      "8d4c81b2-b1ad-476d-9574-544d155aa6ff",
      "1bf80164-ff24-4866-b19c-6212e5b9a847",
      "76f80127-f2cd-46f4-8c52-8edd8bc749b1",
      "0ba27160-44d0-42b5-b90c-47b3fcc48e35"
     ],
     appid: "b075ddef-0efa-123b-997b-de1337c29185",
     appidacr: "1",
     scp: "user_impersonation",
     acr: "1"
    }.

## <a name="related-content"></a>İlgili içerik
* Azure AD grafik bkz [İlkesi işlemleri](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) ve [İlkesi varlık](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity), belirteç ömrü ilkesi Azure AD grafik API'si aracılığıyla yönetme hakkında daha fazla bilgi edinmek için.
* Daha fazla bilgi ve örnekler dahil olmak üzere, PowerShell cmdlet'leri aracılığıyla ilkelerini yönetme örnekleri için bkz: [yapılandırılabilir belirteci yaşam süreleri Azure AD'de](../active-directory-configurable-token-lifetimes.md). 
* Ekleme [özel ve isteğe bağlı talep](active-directory-optional-claims.md) üzere uygulamanız için belirteç. 
