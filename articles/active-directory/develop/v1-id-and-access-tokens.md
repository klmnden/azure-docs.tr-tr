---
title: Farklı bir belirteç hakkında bilgi edinin ve talep türleri Azure AD tarafından desteklenen | Microsoft Docs
description: Anlama ve Azure Active Directory (AAD) verilen SAML 2.0 ve JSON Web belirteçleri (JWT) belirteçlere talep değerlendirmek için bir kılavuz
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
ms.openlocfilehash: 5655490ab724a5570bfa7e74aa513f9b0d900b7e
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39582022"
---
# <a name="azure-ad-token-reference"></a>Azure AD belirteç başvurusu
Azure Active Directory (Azure AD) güvenlik belirteçleri her kimlik doğrulama akışı işlenmesinde çeşitli yayar. Bu belge, biçimi, güvenlik özellikleri ve her belirteç türünün içeriğini açıklar. 

## <a name="types-of-tokens"></a>Belirteç türleri
Azure AD destekler [OAuth 2.0 Yetkilendirme Protokolü](v1-protocols-oauth-code.md), hem access_tokens ve refresh_tokens kullanan. Kimlik doğrulaması ve oturum açma aracılığıyla da destekler [Openıd Connect](v1-protocols-openid-connect-code.md), üçüncü bir belirtecin, id_token türü sunar. Bu belirteçlerden her biri "taşıyıcı belirteç" olarak temsil edilir.

Taşıyıcı belirteç korumalı bir kaynağın "bearer" erişim veren bir basit güvenlik belirtecidir. Bu anlamda belirteç sunabilir herhangi bir tarafa "bearer" olur. Azure AD ile kimlik doğrulaması, bir taşıyıcı belirteç almak için gerekli olan ancak istenmeyen bir taraf tarafından durdurma önlemek için belirteci güvenli hale getirmek için adım atılmalıdır. Taşıyıcı belirteçleri yetkisiz taraflar, onları kullanmasına engel olmak için yerleşik bir mekanizma yoktur çünkü bunlar, Aktarım Katmanı Güvenliği (HTTPS) gibi güvenli bir kanal taşınan gerekir. Açık bir şekilde bir taşıyıcı belirteç iletilirse, bir adam-Orta saldırı belirteci almak ve korumalı kaynağa yetkisiz erişim sağlamak için kullanılabilir. Depolama veya daha sonra kullanmak için taşıyıcı belirteçlerini önbelleğe alma aynı güvenlik ilkeleri uygulayın. Uygulamanızı iletir ve güvenli bir şekilde taşıyıcı belirteçleri depolar her zaman emin olmalısınız. Taşıyıcı belirteçleri hakkında daha fazla güvenlik konuları için bkz. [RFC 6750 bölüm 5](http://tools.ietf.org/html/rfc6750).

Azure AD tarafından verilen belirteçleri birçoğu, JSON Web belirteçlerini veya Jwt'ler olarak uygulanır. JWT'nin, iki taraf arasında bilgi aktarma compact, URL güvenli bir yoludur. Jwt'ler içinde yer alan bilgileri bilinir "talepler" ya da onaylar bilgi taşıyıcı ve belirtecin konu. Jwt'ler Taleplerde kodlanmış ve aktarım için seri hale getirilmiş JSON nesneleridir. Azure AD tarafından verilen Jwt'ler imzalanmış, ancak şifreli olduğundan, kolayca hata ayıklama amacıyla bir JWT içeriğini inceleyebilirsiniz. Bunu gibi yapmak kullanılabilen çeşitli araçlar vardır [jwt.ms](https://jwt.ms/). Jwt'ler hakkında daha fazla bilgi için başvurabilirsiniz [JWT belirtimi](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

## <a name="idtokens"></a>Id_tokens
Id_tokens uygulamanıza kimlik doğrulaması kullanarak gerçekleştirirken alan oturum güvenlik belirteci biçimi olan [Openıd Connect](v1-protocols-openid-connect-code.md). Olarak temsil edilen [Jwt'ler](#types-of-tokens)ve kullanıcı uygulamanızı imzalamak için kullandığınız bir talep içermelidir. Talepler bir id_token içinde kullanabileceğiniz gördüğünüz - hesap bilgilerini görüntülemek veya erişim denetimi kararları bir uygulamada sık kullanılan olarak.

Id_tokens imzalanmış, ancak şu anda şifrelenmez. Uygulamanızı bir id_token aldığında, bu gerekir [imzayı doğrulamak](#validating-tokens) belirtecinin kimliğinin doğruluğunu kanıtlamak ve belirtecin geçerliliğini kanıtlamak için birkaç talepleri doğrulamak için. Bir uygulama tarafından doğrulanmış talep senaryo gereksinimlerine bağlı olarak farklılık gösterir, ancak bazı kısıtlamalar var. [ortak talep doğrulamaları](#validating-tokens) , uygulamanızı her senaryoda gerçekleştirmeniz gerekir.

İd_tokens talep, aynı zamanda bir örnek id_token bilgi için aşağıdaki bölüme bakın. İd_tokens Taleplerde herhangi belirli bir sırada döndürülmediğini unutmayın. Ayrıca, yeni bir talep herhangi bir noktada id_tokens içine sürede tanıtılabilir - yeni talep sunulduğu şekilde uygulamanızı kesmemesi. Aşağıdaki liste, uygulamanız bu makalenin yazıldığı sırada güvenilir bir şekilde yorumlayabilir talepleri içerir. Gerekirse, daha fazla ayrıntı bulunabilir, [Openıd Connect belirtimi](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-idtoken"></a>Örnek id_token
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.
```

> [!TIP]
> Uygulama için buraya yapıştırarak örnek id_token Taleplerde inceleyerek deneyin [jwt.ms](https://jwt.ms/).
> 
> 

#### <a name="claims-in-idtokens"></a>İd_tokens talepleri
> [!div class="mx-codeBreakAll"]
| JWT talep | Ad | Açıklama |
| --- | --- | --- |
| `aud` |Hedef kitle |Amaçladığınız alıcının belirtecin. Belirteç alan uygulamasını İzleyici değerin doğru olduğundan ve farklı bir kitle için hedeflenen tarafından istenen belirteçleri Reddet doğrulamanız gerekir. <br><br> **Örnek SAML değer**: <br> `<AudienceRestriction>`<br>`<Audience>`<br>`https://contoso.com`<br>`</Audience>`<br>`</AudienceRestriction>` <br><br> **Örnek JWT değer**: <br> `"aud":"https://contoso.com"` |
| `appidacr` |Uygulama kimlik doğrulaması bağlamı sınıf başvurusu |İstemcinin kimlik doğrulamasının nasıl yapıldığı gösterir. Genel bir istemci için değeri 0'dır. İstemci Kimliğini ve istemci gizli anahtarı kullandıysanız, değer 1'dir. Bir istemci sertifikası kimlik doğrulaması için kullanılırsa, değeri 2'dir. <br><br> **Örnek JWT değer**: <br> `"appidacr": "0"` |
| `acr` |Kimlik doğrulaması bağlamı sınıf başvurusu |Nasıl konu, istemci uygulaması kimlik doğrulaması bağlamı sınıf başvurusu talep aksine doğrulandı gösterir. "0" değeri, son kullanıcı kimlik doğrulaması, ISO/IEC 29115 gereksinimlerini karşılamayan gösterir. <br><br> **Örnek JWT değer**: <br> `"acr": "0"` |
| Anında kimlik doğrulaması |Kimlik doğrulaması oluştuğu saat ve tarihi kaydeder. <br><br> **Örnek SAML değer**: <br> `<AuthnStatement AuthnInstant="2011-12-29T05:35:22.000Z">` | |
| `amr` |Kimlik Doğrulama Yöntemi |Belirtecin konu kimliğinin nasıl tanımlar. <br><br> **Örnek SAML değer**: <br> `<AuthnContextClassRef>`<br>`http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod/password`<br>`</AuthnContextClassRef>` <br><br> **Örnek JWT değer**: `“amr”: ["pwd"]` |
| `given_name` |Ad |İlk sağlar veya "Azure AD kullanıcı nesnesindeki belirlenen kullanıcı adı verilen". <br><br> **Örnek SAML değer**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname”>`<br>`<AttributeValue>Frank<AttributeValue>` <br><br> **Örnek JWT değer**: <br> `"given_name": "Frank"` |
| `groups` |Gruplar |Öznenin grup üyeliklerini temsil eden nesne kimlikleri sağlar. Bu değerler benzersiz (nesne kimliği bakın) ve güvenli bir şekilde bir kaynağa erişim yetkisi zorlama gibi erişimi yönetmek için kullanılabilir. Grupları talepte bulunan gruplarını, uygulama başına temelinde, uygulama bildiriminin "groupMembershipClaims" özelliği aracılığıyla yapılandırılır. Bir null değeri tüm grupları hariç, yalnızca güvenlik grupları ve Office 365 dağıtım listeleri Active Directory güvenlik grubu üyelikleri ve bir değeri "Tüm" dahil "IDAP" değerini içerecektir. <br><br> **Notları**: <br> Bkz: `hasgroups` kullanımıyla ilgili ayrıntılar için aşağıda talep `groups` örtük vermeyi ile talep. <br> Bir kapasite aşımı talep sonra kullanıcının grup sayısı sınırı (SAML, JWT için 200 150) üzerinden aşması durumunda, diğer akışlar için kullanıcı gruplarının listesini içeren Graph uç noktası işaret talep kaynağı eklenecektir. (içinde. <br><br> **Örnek SAML değer**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">`<br>`<AttributeValue>07dd8a60-bf6d-4e17-8844-230b77145381</AttributeValue>` <br><br> **Örnek JWT değer**: <br> `“groups”: ["0e129f5b-6b0a-4944-982d-f776045632af", … ]` |
|`hasgroups` | JWT örtük akış grupları fazla kullanım göstergesi| Varsa, her zaman `true`, olan en az bir gruba kullanıcı belirten. Öğesinin yerine kullanılmalıdırlar `groups` Jwt'ler için bir talep tam grupları talep varsa örtük vermeyi akışlardaki URI parçası (şu anda, 6 veya daha fazla grup) URL uzunluk sınırları ötesinde genişletin. Kullanıcının grupları belirlemek için istemci Graph kullanmanız gerektiğini belirtir (`https://graph.windows.net/{tenantID}/users/{userID}/getMemberObjects`). |
| `groups:src1` <br> `http://schemas.microsoft.com/claims/groups.link` | Fazla kullanım göstergesi grupları | Uzunluğu sınırlı olmayan belirteç istekleri için (bkz `hasgroups` yukarıda), ancak yine de çok büyük belirteç için bir bağlantı kullanıcı için tam grupları listesine eklenir. SAML yerine yeni bir talep olarak için Dağıtılmış bir talep olarak jwt'lerinin `groups` talep. <br><br> **Örnek SAML değer**: <br> `<Attribute Name=” http://schemas.microsoft.com/claims/groups.link”>`<br>`<AttributeValue>https://graph.windows.net/{tenantID}/users/{userID}/getMemberObjects<AttributeValue>` <br><br> **Örnek JWT değer**: <br> `"groups":"src1` <br> `_claim_sources`: `"src1" : { "endpoint" : "https://graph.windows.net/{tenantID}/users/{userID}/getMemberObjects" }`|
| `idp` |Kimlik Sağlayıcısı |Belirtecin konu kimliği doğrulanmış kimlik sağlayıcısı olarak kaydeder. Farklı bir kiracıya veren kullanıcı hesabının bulunduğu sürece bu değeri veren talep değeri için aynıdır. <br><br> **Örnek SAML değer**: <br> `<Attribute Name=” http://schemas.microsoft.com/identity/claims/identityprovider”>`<br>`<AttributeValue>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/<AttributeValue>` <br><br> **Örnek JWT değer**: <br> `"idp":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `iat` |IssuedAt |Belirteç verildiği zaman depolar. Genellikle, belirteç güncellik ölçmek için kullanılır. <br><br> **Örnek SAML değer**: <br> `<Assertion ID="_d5ec7a9b-8d8f-4b44-8c94-9812612142be" IssueInstant="2014-01-06T20:20:23.085Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">` <br><br> **Örnek JWT değer**: <br> `"iat": 1390234181` |
| `iss` |Sertifikayı Veren |Oluşturur ve belirteci döndürür güvenlik belirteci hizmeti (STS) tanımlar. Azure AD döndüren belirteçlerinde veren sts.windows.net ' dir. Azure AD dizini Kiracı kimliği verenin talep değeri GUID'dir. Kiracı kimliği dizinin değişmez ve güvenilir bir tanımlayıcıdır. <br><br> **Örnek SAML değer**: <br> `<Issuer>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/</Issuer>` <br><br> **Örnek JWT değer**: <br>  `"iss":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `family_name` |Soyadı |Son adını, soyadını veya kullanıcının aile adı Azure AD kullanıcı nesnesinde tanımlanan sağlar. <br><br> **Örnek SAML değer**: <br> `<Attribute Name=” http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname”>`<br>`<AttributeValue>Miller<AttributeValue>` <br><br> **Örnek JWT değer**: <br> `"family_name": "Miller"` |
| `unique_name` |Ad |Belirtecin konu tanımlayan bir insan tarafından okunabilir değer sağlar. Bu değer, bir kiracıda benzersiz olması garanti edilmez ve yalnızca görüntüleme amaçları için kullanılmak üzere tasarlanmıştır. <br><br> **Örnek SAML değer**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name”>`<br>`<AttributeValue>frankm@contoso.com<AttributeValue>` <br><br> **Örnek JWT değer**: <br> `"unique_name": "frankm@contoso.com"` |
| `oid` |Nesne kimliği |Azure AD'de bir nesnenin benzersiz bir tanımlayıcı içerir. Bu değer sabittir ve yeniden atandı yeniden veya değiştirilemez. Azure AD'ye sorgulardaki bir nesne tanımlamak için nesne Kimliğini kullanın. <br><br> **Örnek SAML değer**: <br> `<Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">`<br>`<AttributeValue>528b2ac2-aa9c-45e1-88d4-959b53bc7dd0<AttributeValue>` <br><br> **Örnek JWT değer**: <br> `"oid":"528b2ac2-aa9c-45e1-88d4-959b53bc7dd0"` |
| `roles` |Roller |Konu grubu üyeliği üzerinden doğrudan ve dolaylı olarak verilmiş ve rol tabanlı erişim denetimi uygulamak için kullanılabilir tüm uygulama rolleri temsil eder. Uygulama rolleri aracılığıyla uygulama başına temelinde, tanımlanan `appRoles` uygulama bildiriminin özelliğidir. `value` Rol talebi görüntülenen değeri her uygulama rolü özelliğidir. <br><br> **Örnek SAML değer**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/role">`<br>`<AttributeValue>Admin</AttributeValue>` <br><br> **Örnek JWT değer**: <br> `“roles”: ["Admin", … ]` |
| `scp` |Kapsam |İstemci uygulaması için kimliğe bürünme izinleri belirtir. Varsayılan izin `user_impersonation`. Güvenli kaynağın sahibi ek değerler Azure AD'ye kaydedebilir. <br><br> **Örnek JWT değer**: <br> `"scp": "user_impersonation"` |
| `sub` |Konu |Sorumlu olduğu hakkında bir uygulamanın kullanıcı gibi bilgileri belirteci onaylar tanımlar. Bu değer sabittir ve sonraki atanamaz veya yeniden, bu nedenle bu yetkilendirme denetimleri güvenli bir şekilde gerçekleştirmek için kullanılabilir. Konu her zaman Azure AD sorunlarını belirteçlerinde bulunduğundan, bu değer bir genel amaçlı yetkilendirme sistemde kullanılması önerilir. <br> `SubjectConfirmation` bir talep değil. Bu konu belirtecin nasıl doğrulanır açıklar. `Bearer` Konu belirtecin kendi mülkü tarafından doğrulandığını gösterir. <br><br> **Örnek SAML değer**: <br> `<Subject>`<br>`<NameID>S40rgb3XjhFTv6EQTETkEzcgVmToHKRkZUIsJlmLdVc</NameID>`<br>`<SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />`<br>`</Subject>` <br><br> **Örnek JWT değer**: <br> `"sub":"92d0312b-26b9-4887-a338-7b00fb3c5eab"` |
| `tid` |Kiracı Kimliği |Belirteci veren dizin Kiracı tanımlayan bir sabit, yeniden kullanılabilir olmayan tanımlayıcısı. Bu değer, çok kiracılı bir uygulamadaki kiracıya özgü directory kaynaklarına erişmek için kullanabilirsiniz. Örneğin, bu değer, Graph API'sine çağrıda kiracıda tanımlamak için kullanabilirsiniz. <br><br> **Örnek SAML değer**: <br> `<Attribute Name=”http://schemas.microsoft.com/identity/claims/tenantid”>`<br>`<AttributeValue>cbb1a5ac-f33b-45fa-9bf5-f37db0fed422<AttributeValue>` <br><br> **Örnek JWT değer**: <br> `"tid":"cbb1a5ac-f33b-45fa-9bf5-f37db0fed422"` |
| `nbf`, `exp` |Belirteç ömrü |Bir belirtecin geçerli olduğu zaman aralığını tanımlar. Belirteci doğrular hizmet geçerli bir tarih belirteci reddetme belirteç ömrü içinde başka olduğunu doğrulamanız gerekir. Hizmet belirteci ömrü aralığı dışındaki beş dakikaya kadar herhangi bir farklılığın saatindeki ("saat eğriltme") hesabı için Azure AD arasında sağlayabilir ve hizmeti. <br><br> **Örnek SAML değer**: <br> `<Conditions`<br>`NotBefore="2013-03-18T21:32:51.261Z"`<br>`NotOnOrAfter="2013-03-18T22:32:51.261Z"`<br>`>` <br><br> **Örnek JWT değer**: <br> `"nbf":1363289634, "exp":1363293234` |
| `upn` |Kullanıcı Asıl Adı |Kullanıcı asıl kullanıcı adını depolar.<br><br> **Örnek JWT değer**: <br> `"upn": frankm@contoso.com` |
| `ver` |Sürüm |Belirteç sürüm numarasını depolar. <br><br> **Örnek JWT değer**: <br> `"ver": "1.0"` |

## <a name="access-tokens"></a>Erişim belirteçleri
Kimlik başarıyla doğrulandıktan sonra Azure AD, korunan kaynaklara erişim için kullanılan bir erişim belirteci döndürür. Erişim belirteci, bir taban 64 kodlamalı JSON Web Token (JWT) ve içeriğinin bir kod çözücü çalıştırarak denetlenecek.

Uygulamanızı yalnızca *kullanan* erişim elde etmek için erişim belirteçleri API'lere, kullanabilirsiniz ve kullanmalısınız kabul erişim belirteçleri olarak tamamen opak - bunlar, uygulamanız HTTP isteklerini kaynaklara geçirebilirsiniz yalnızca dizelerdir.

Azure AD, ayrıca bir erişim belirteci isteğinde bulunduğunuzda uygulamanızın tüketim için erişim belirteci hakkında bazı meta verileri döndürür. Bu bilgiler, kapsamlar için geçerlidir ve erişim belirteci süre sonu içerir. Bu, uygulamanıza akıllı erişim belirteçleri önbellek açık ayrıştırma gerek kalmadan gerçekleştirmek için erişim belirteci sağlar.

Ardından uygulamanız HTTP isteklerini erişim belirteçleri bekliyor Azure AD ile korunan bir API ise, doğrulama ve İnceleme aldığınız belirteçlerin gerçekleştirmeniz gerekir. Uygulamanızı, kaynaklara erişmek için kullanmadan önce erişim belirteci doğrulaması gerçekleştirmeniz gerekir. Doğrulama hakkında daha fazla bilgi için lütfen bkz [doğrulama belirteçleri](#validating-tokens). . NET'i kullanmaya nasıl yapılacağı hakkında daha fazla bilgi için bkz: [Azure AD'den taşıyıcı belirteçlerini kullanarak bir Web API'sini korumak](quickstart-v1-dotnet-webapi.md).

## <a name="refresh-tokens"></a>Yenileme belirteçlerini

Yenileme belirteçler, uygulamanız OAuth 2.0 akışı yeni erişim belirteçleri almak için kullanabileceğiniz güvenlik belirteçleri. Kullanıcı etkileşimi gerektirmeden bir kullanıcı adına kaynakları için uzun süreli erişim elde etmek uygulamanızı sağlar.

Çok kaynaklı yenileme belirteçleri. Bir kaynak için bir belirteç isteği sırasında alınan bir yenileme belirteci erişebileceği, tamamen farklı bir kaynağa erişim belirteçleri için yani kullanılamadı. Bunu yapmak için ayarlanmış `resource` hedeflenen kaynak isteği parametresi.

Uygulamanıza tamamen opak yenileme belirteçleri. Uzun süreli olduklarından, ancak uygulamanızı bir yenileme belirteci süre boyunca en son, bekleyebileceğiniz yazılmamalıdır. Yenileme belirteçleri geçersiz olabilir, çeşitli nedenlerle - zamanında herhangi bir anda bkz [belirteç iptali](#token-revocation) bu nedenlerle. Azure AD belirteç uç noktası için bir belirteç isteğini yaparak kullanmak uygulamanızı bir yenileme belirteci geçerli olup olmadığını öğrenmek için tek yolu denemektir.

Yeni bir erişim belirteci için bir yenileme belirteci şifrenizi kullandığınızda, belirteç yanıt olarak yeni bir yenileme belirteci alır. İstekte kullanılan değiştirme, yeni verilen yenileme belirteci kaydetmeniz gerekir. Bu, yenileme belirteçleri için mümkün olan en kısa sürece geçerli kalır garanti eder.

## <a name="validating-tokens"></a>Belirteçleri doğrulama

Bir id_token veya bir access_token doğrulamak için uygulamanızı hem belirtecinin imzası hem de talep doğrulamalıdır. Erişim belirteçleri doğrulamak için uygulamanızı de veren, İzleyici ve imzalama belirteçleri doğrulamalıdır. Bu değerler Openıd keşif belgesi karşı doğrulanması gerekir. Örneğin, belgenin Kiracı bağımsız sürümünü şu konumdadır [ https://login.microsoftonline.com/common/.well-known/openid-configuration ](https://login.microsoftonline.com/common/.well-known/openid-configuration). Azure AD ara yazılım, erişim belirteçleri doğrulamak için yerleşik özelliklere sahip ve üzerinden göz atabileceğiniz bizim [örnekleri](https://docs.microsoft.com/azure/active-directory/active-directory-code-samples) birini tercih ettiğiniz dilde bulunacak. Açıkça bir JWT belirteci doğrulama hakkında daha fazla bilgi için lütfen bkz [el ile JWT doğrulama örnek](https://github.com/Azure-Samples/active-directory-dotnet-webapi-manual-jwt-validation). 

Kitaplıkları ve belirteç doğrulama - kolayca nasıl ele alınacağını gösteren kod örnekleri sunuyoruz. aşağıdaki bilgileri yalnızca temel işlem anlamak istiyorsanız olanlar için sağlanır. Kullanılabilir ayrıca çeşitli üçüncü taraf açık kaynak kitaplıkları için JWT doğrulama - neredeyse her platform ve dil burada için en az bir seçenek yoktur. Azure AD kimlik doğrulama kitaplıkları ve kod örnekleri hakkında daha fazla bilgi için lütfen bkz [Azure AD kimlik doğrulama kitaplıkları](active-directory-authentication-libraries.md).

#### <a name="validating-the-signature"></a>İmza doğrulama

JWT'nin tarafından ayrılmış üç segmentleri içeren `.` karakter. İlk segment olarak bilinen **üstbilgi**olarak ikinci **gövdesi**ve üçüncü olarak **imza**. İmza segment, böylece uygulamanız tarafından güvenilen belirteç özgünlüğünü doğrulamak için kullanılabilir.

Azure AD tarafından verilen belirteçlere RSA 256 gibi sektörde standart asimetrik şifreleme algoritmaları kullanılarak imzalanmış. JWT üstbilgisi, belirteç imzalamak için kullanılan anahtar ve şifreleme yöntemiyle ilgili bilgiler içerir:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "x5t": "iBjL1Rcqzhiy4fpxIxdZqohM2Yk"
  "kid": "iBjL1Rcqzhiy4fpxIxdZqohM2Yk"
}
```

`alg` Talep belirteci, while imzalamak için kullanılan algoritmayı belirtir `x5t` talep, belirteç imzalamak için kullanılan belirli bir ortak anahtar gösterir.

Herhangi belirli bir noktada, Azure AD, belirli bir dizi ortak-özel anahtar çifti herhangi birini kullanarak bir id_token oturum açabilirsiniz. Uygulamanızın bu anahtar değişiklikleri otomatik olarak işlemek için yazılmış olması gerekir böylece azure AD'ye anahtarlarını düzenli aralıklarla, olası kümesini döndürür. 24 saatte bir Azure AD tarafından kullanılan ortak anahtarlar için güncelleştirmeleri denetlemek için makul bir sıklığıdır.

Konumundaki Openıd Connect meta veri belgesi kullanarak imzayı doğrulamak gereken imzalama anahtar verileri elde edebilirsiniz:

```
https://login.microsoftonline.com/common/.well-known/openid-configuration
```

> [!TIP]
> Bu URL'de bir tarayıcı deneyin!

Bu meta veri belgesi Openıd Connect kimlik doğrulaması gerçekleştirmek için gereken çeşitli uç nokta konumu gibi bilgiler birkaç faydalı parçalarını içeren bir JSON nesnesidir. 

Ayrıca bir `jwks_uri`, Belirteçleri imzalamak için kullanılan ortak anahtar kümesini konumunu sağlar. JSON belgesini konumundaki `jwks_uri` tüm zaman içinde belirli o anda kullanımda ortak anahtar bilgilerini içerir. Uygulamanız kullanabilir `kid` hangi ortak anahtarı bu belgedeki belirli bir belirteç imzalamak için kullanılan seçmek için JWT üstbilgisinde talep. Ayrıca, doğru ortak anahtarı ve belirtilen algoritmasını kullanarak imza doğrulaması daha sonra gerçekleştirebilirsiniz.

> [!NOTE]
> Her ikisi de v1.0 uç noktanın döndürdüğü `x5t` ve `kid` talep. `x5t` V2.0 belirteçleri talep eksik. V2.0 uç noktası ile yanıt veren `kid` talep. Bundan sonra kullanılması önerilir `kid` belirtecinizi doğrulamak talep.

Bu belgenin kapsamı dışında olan imzası doğrulama gerçekleştiriliyor - birçok açık kaynak kitaplıkları, gerekirse, bunu yapmanıza yardımcı olmak için kullanılabilir.

#### <a name="validating-the-claims"></a>Talepleri doğrulanıyor

Uygulamanıza bir belirteç (bir id_token kullanıcı oturum açma sonrası, veya bir erişim belirteci olarak HTTP isteğinde bir taşıyıcı belirteç) aldığında, talepleri karşı birkaç denetimleri belirteç gerçekleştirmelisiniz. Bunlar dahil ancak bunlarla sınırlı değildir:

* **İzleyici** - belirteç uygulamanıza verilmesi amaçlanmamıştır doğrulamak için talep.
* **Öncesine** ve **süre sonu** - belirtecin süresinin sona ermediğini doğrulamak için talep.
* **Veren** - belirteç gerçekten uygulamanıza Azure AD tarafından verildiğini doğrulamak için talep.
* **Nonce** - belirteç yeniden yürütme saldırının etkilerini hafifletmek için.
* ve daha fazlası...

Uygulamanız için kimlik belirteçlerini gerçekleştirmesi gereken talep doğrulamaları tam bir listesi için başvurmak [Openıd Connect belirtimi](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation). Bu talepler için beklenen değerle ayrıntıları dahil edilir önceki [id_token](#id-tokens) bölümü.

## <a name="token-revocation"></a>Belirteç iptali

Yenileme belirteçlerini geçersiz veya çeşitli nedenlerle için herhangi bir zamanda iptal edildi. Bu iki ana kategoriye ayrılır: zaman aşımları ve geri alma işlemleri. 

**Belirteci zaman aşımları**

* MaxInactiveTime: yenileme belirteci MaxInactiveTime tarafından dikte zaman içinde kullanılmamış, belirteci yenilemek artık geçerli olmayacak. 
* MaxSessionAge: (Kadar iptal edilen) varsayılan dışında bir şey MaxAgeSessionMultiFactor veya MaxAgeSessionSingleFactor atanmışsa, belirlenen MaxAgeSession * süresi sona erdiğinde daha sonra yeniden kimlik doğrulaması gerekli olacaktır. 
* Örnekler:
  * 5 günlük bir MaxInactiveTime kiracısına sahip kullanıcı için bir hafta tatile oluştu ve bu nedenle AAD kullanıcıdan yeni bir belirteç isteğini 7 gündür görülmeyen değil. Kullanıcı yeni bir belirteç istediğinde, kendi yenileme belirteci iptal edildi ve kendi kimlik bilgileri yeniden girmelisiniz bulabilirsiniz. 
  * 1 günlük bir MaxAgeSessionSingleFactor duyarlı bir uygulamaya sahiptir. Pazartesi ve Salı (25 saat geçtikten sonra) bir kullanıcı oturum açarsa, yeniden kimlik doğrulaması gerekir. 

**İptal etme**

|   | Parola tabanlı tanımlama bilgisi | Parola tabanlı belirteci | Olmayan parola tabanlı tanımlama bilgisi | Olmayan parola tabanlı belirteci | Gizli istemci belirteci| 
|---|-----------------------|----------------------|---------------------------|--------------------------|--------------------------|
|Parola süre sonu| Canlı kalır|Canlı kalır|Canlı kalır|Canlı kalır|Canlı kalır|
|Parola kullanıcı tarafından değiştirildi| İptal edildi | İptal edildi | Canlı kalır|Canlı kalır|Canlı kalır|
|SSPR kullanıcı yok|İptal edildi | İptal edildi | Canlı kalır|Canlı kalır|Canlı kalır|
|Yönetici parolasını sıfırlar.|İptal edildi | İptal edildi | Canlı kalır|Canlı kalır|Canlı kalır|
|Kullanıcı iptal, yenileme belirteçleri [PowerShell aracılığıyla](https://docs.microsoft.com/powershell/module/azuread/revoke-azureadsignedinuserallrefreshtoken) | İptal edildi | İptal edildi |İptal edildi | İptal edildi |İptal edildi | İptal edildi |
|Yönetici, Kiracı için tüm yenileme belirteçleri iptal [PowerShell aracılığıyla](https://docs.microsoft.com/powershell/module/azuread/revoke-azureaduserallrefreshtoken) | İptal edildi | İptal edildi |İptal edildi | İptal edildi |İptal edildi | İptal edildi |
|[Çoklu oturum kullanıma](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code#single-sign-out) Web'de | İptal edildi | Canlı kalır |İptal edildi | Canlı kalır |Canlı kalır |Canlı kalır |

> [!NOTE]
> "Olmayan parola tabanlı" oturum açma kullanıcı almak için bir parola girildiği yaramadı biridir.  Örneğin, yüz tanıma Windows Hello, bir FIDO anahtar veya bir PIN ile kullanma. 
>
> Windows birincil yenileme belirteci ile bilinen bir sorun var.  PRT parola alınır ve ardından kullanıcı oturumu Merhaba, bu PRT, oluşturulma değiştirmez ve kullanıcı parolasını değiştirirse iptal edilir. 

## <a name="sample-tokens"></a>Örnek belirteçleri

Bu bölümde, Azure AD'ye verir SAML ve JWT belirteçleri örneklerini görüntüler. Bu örnekler bağlamında talep görmenize olanak tanır.

### <a name="saml-token"></a>SAML belirteci

Bu, tipik bir SAML belirtecinin bir örnektir.

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
Bir yetkilendirme kodu verme akışı içinde kullanılan bir tipik JSON web token (JWT) örneğidir.
Talepler ek olarak, bir sürüm numarası simgeyi içeren **ver** ve **appidacr**, istemci kimliklerinin nasıl gösterir kimlik doğrulaması bağlamı sınıf başvurusu. Genel bir istemci için değeri 0'dır. Bir istemci kimliği veya gizli kullanıldıysa, değer 1'dir.

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
* Azure AD Graph bkz [ilke işlemleri](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) ve [ilke varlığı](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity)belirteç ömrü ilkesi Azure AD Graph API'si aracılığıyla yönetme hakkında daha fazla bilgi için.
* Daha fazla bilgi ve örnekler de dahil olmak üzere, PowerShell cmdlet'leri aracılığıyla ilkeleri yönetme örnekleri için bkz. [Azure AD'de yapılandırılabilir belirteç ömrünü](../active-directory-configurable-token-lifetimes.md). 
* Ekleme [özel ve isteğe bağlı taleplerin](active-directory-optional-claims.md) uygulamanızın belirteçleri. 
