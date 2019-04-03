---
title: Belirli bir uygulamayı Azure AD kiracısı (genel Önizleme) içinde belirteçlerinde yayılan talep özelleştirme
description: Bu sayfa, Azure Active Directory'de talep eşlemesi açıklar.
services: active-directory
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2019
ms.author: celested
ms.reviewer: paulgarn, hirsin, jeedes, luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: e492586f0c70d4cd3013ef8f466afd6bb2bb65ac
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58884086"
---
# <a name="how-to-customize-claims-emitted-in-tokens-for-a-specific-app-in-a-tenant-preview"></a>Nasıl yapılır: Bir kiracıdaki (Önizleme) belirli bir uygulamayı belirteçlerinde yayılan talep özelleştirme

> [!NOTE]
> Bu özellik değiştirir ve yerine geçen [özelleştirme talep](active-directory-saml-claims-customization.md) bugün Portalı aracılığıyla sunulan. Bu belgede ayrıntılandırılan graf/PowerShell yöntemin yanı sıra portalı kullanarak talep özelleştirirseniz aynı uygulama için uygulama yapılandırma Portalı'nda yoksayacak verilen belirteçler. Bu belgede ayrıntılandırılan yöntemleri aracılığıyla yapılan yapılandırmalar portalda yansıtılmaz.

Bu özellik, Kiracı yöneticileri tarafından belirteçlerinde kiracısındaki belirli bir uygulama için gösterilen talep özelleştirmek için kullanılır. Talep eşleme ilkelerini kullanabilirsiniz:

- Hangi taleplerin belirteçlerinde içerdiği seçin.
- Henüz yoksa talep türlerini oluşturun.
- Seçin veya belirli Taleplerde yayılan veri kaynağı olarak değiştirin.

> [!NOTE]
> Bu özellik şu anda genel Önizleme aşamasındadır. Değişiklikleri geri almaya veya kaldırmaya hazırlıklı olun. Bu özellik, genel Önizleme sırasında herhangi bir Azure Active Directory (Azure AD) aboneliği kullanılabilir. Ancak, bazı yönlerini özellik özelliği genel kullanıma sunulduğunda, bir Azure AD premium aboneliği gerektirebilir. Bu özellik, WS-Federasyon, SAML, OAuth ve Openıd Connect protokolleri için yapılandırma talep eşleme ilkelerini destekler.

## <a name="claims-mapping-policy-type"></a>Talep eşleme ilkesi türü

Azure AD'de bir **ilke** nesne ayrı ayrı uygulamaların veya kuruluştaki tüm uygulamalar uygulanan kurallar kümesini temsil eder. Her ilke türü, ardından atanmış olan nesnelere uygulanan özellik kümesi ile benzersiz bir yapıya sahiptir.

Bir talep türü olan ilke eşleştirme **ilke** yayılan özel uygulamalar için verilen belirteçlere talep değiştiren bir nesne.

## <a name="claim-sets"></a>Talep kümeleri

Belirli nasıl ve ne zaman belirteçlerinde kullanıldıklarından tanımlayan talep kümesi vardır.

| Talep kümesi | Açıklama |
|---|---|
| Çekirdek talep kümesi | İlkeyi bağımsız olarak her belirteci yok. Bu talep, kısıtlı kabul edilir ve değiştirilemez. |
| Temel talep kümesi | Belirteçleri (ek olarak çekirdek talep kümesi) için varsayılan olarak yayılan talepleri içerir. İlkeleri eşleme talep kullanarak talepleri temel değiştirmek ya da atlayın. |
| Kısıtlı talep kümesi | İlke kullanılarak değiştirilemez. Veri kaynağı değiştirilemez ve hiçbir dönüştürme yapılmadı, bu talepler oluştururken uygulanır. |

### <a name="table-1-json-web-token-jwt-restricted-claim-set"></a>Tablo 1: JSON Web Token (JWT) talep kümesi kısıtlı

| Talep türü (ad) |
| ----- |
| _claim_names |
| _claim_sources |
| access_token |
| account_type |
| acr |
| Aktör |
| actortoken |
| aıo'yu |
| altsecid |
| amr |
| app_chain |
| app_displayname |
| app_res |
| appctx |
| appctxsender |
| Uygulama Kimliği |
| appidacr |
| onaylama |
| at_hash |
| aud |
| auth_data |
| auth_time |
| authorization_code |
| azp |
| azpacr |
| c_hash |
| ca_enf |
| cc |
| cert_token_use |
| client_id |
| cloud_graph_host_name |
| cloud_instance_name |
| cnf |
| kod |
| denetimler |
| credential_keys |
| CSR |
| csr_type |
| cihaz kimliği |
| dns_names |
| domain_dns_name |
| domain_netbios_name |
| e_exp |
| e-posta |
| endpoint |
| enfpolids |
| exp |
| expires_on |
| grant_type değeri |
| Graf |
| group_sids |
| gruplar |
| hasgroups |
| hash_alg |
| home_oid |
| `http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationinstant` |
| `http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod` |
| `http://schemas.microsoft.com/ws/2008/06/identity/claims/expiration` |
| `http://schemas.microsoft.com/ws/2008/06/identity/claims/expired` |
| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` |
| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` |
| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` |
| IAT |
| ıdentityprovider |
| IDP |
| in_corp |
| örnek |
| IpAddr |
| isbrowserhostedapp |
| ISS |
| jwk |
| key_id |
| key_type |
| mam_compliance_url |
| mam_enrollment_url |
| mam_terms_of_use_url |
| mdm_compliance_url |
| mdm_enrollment_url |
| mdm_terms_of_use_url |
| nameıd |
| nbf |
| netbios_name |
| nonce |
| OID |
| on_prem_id |
| onprem_sam_account_name |
| onprem_sid |
| openid2_id |
| password |
| platf |
| polids |
| pop_jwk |
| preferred_username |
| previous_refresh_token |
| primary_sid |
| PUID |
| pwd_exp |
| pwd_url |
| redirect_uri |
| refresh_token |
| refreshtoken |
| request_nonce |
| kaynak |
| rol |
| roles |
| scope |
| SCP |
| SID |
| İmza |
| signin_state |
| src1 |
| src2 |
| alt |
| tbid |
| tenant_display_name |
| tenant_region_scope |
| thumbnail_photo |
| TID |
| tokenAutologonEnabled |
| trustedfordelegation |
| unique_name |
| UPN |
| user_setting_sync_url |
| kullanıcı adı |
| utı |
| ver |
| verified_primary_email |
| verified_secondary_email |
| wids |
| win_ver |

### <a name="table-2-saml-restricted-claim-set"></a>Tablo 2: SAML talep kümesi kısıtlı

| Talep türü (URI) |
| ----- |
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/expiration`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/expired`|
|`http://schemas.microsoft.com/identity/claims/accesstoken`|
|`http://schemas.microsoft.com/identity/claims/openid2_id`|
|`http://schemas.microsoft.com/identity/claims/identityprovider`|
|`http://schemas.microsoft.com/identity/claims/objectidentifier`|
|`http://schemas.microsoft.com/identity/claims/puid`|
|`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier [MR1]`|
|`http://schemas.microsoft.com/identity/claims/tenantid`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationinstant`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod`|
|`http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/groups`|
|`http://schemas.microsoft.com/claims/groups.link`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/role`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/wids`|
|`http://schemas.microsoft.com/2014/09/devicecontext/claims/iscompliant`|
|`http://schemas.microsoft.com/2014/02/devicecontext/claims/isknown`|
|`http://schemas.microsoft.com/2012/01/devicecontext/claims/ismanaged`|
|`http://schemas.microsoft.com/2014/03/psso`|
|`http://schemas.microsoft.com/claims/authnmethodsreferences`|
|`http://schemas.xmlsoap.org/ws/2009/09/identity/claims/actor`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/samlissuername`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/confirmationkey`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarygroupsid`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`|
|`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/authorizationdecision`|
|`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/authentication`|
|`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/sid`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/denyonlyprimarygroupsid`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/denyonlyprimarysid`|
|`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/denyonlysid`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/denyonlywindowsdevicegroup`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdeviceclaim`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsfqbnversion`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/windowssubauthority`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsuserclaim`|
|`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/x500distinguishedname`|
|`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`|
|`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/ispersistent`|
|`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier`|
|`http://schemas.microsoft.com/identity/claims/scope`|

## <a name="claims-mapping-policy-properties"></a>Talep eşleme ilkesi özellikleri

Hangi talepleri gönderilir ve verilerin nereden geldiğini denetlemek için bir talep İlkesi eşlemesi özelliklerini kullanın. Bir ilke ayarlanmazsa, çekirdek içeren sistem sorunları belirteçleri kümesi, temel talep kümesini ve talep [isteğe bağlı bir talep](active-directory-optional-claims.md) almak üzere uygulama tarafından seçilen.

### <a name="include-basic-claim-set"></a>Temel talep kümesi Ekle

**Dize:** IncludeBasicClaimSet

**Veri türü:** Boole (True veya False)

**Özet:** Bu özellik, bu ilkeden etkilenen belirteçleri temel talep kümesine dahil olup olmadığını belirler.

- TRUE olarak temel talep kümesindeki tüm talepler ilkeden etkilenen belirteçlerinde çıkarılırsa. 
- Tek tek aynı ilkeyi talep şema özelliğinde eklenen sürece temel talep kümesine talep False olarak ayarlanırsa belirteçleri, değilse.

> [!NOTE] 
> Çekirdek talep kümesine talep bu özelliği ayarlamak bağımsız olarak her belirteci yok. 

### <a name="claims-schema"></a>Talep şeması

**Dize:** ClaimsSchema

**Veri türü:** Bir veya daha fazla talep şema girişi JSON blob

**Özet:** Bu özellik, hangi taleplerin temel talep kümesine hem de çekirdek talep kümesine ek olarak, ilke tarafından etkilenen simgelerin yok tanımlar.
Bu özellikte tanımlanan her talep şema giriş için belirli bilgileri gereklidir. Gelen verilerin nereden geldiğini belirtin (**değer** veya **kaynak/ID çifti**), ve hangi veri talep olarak gösterilir (**talep türü**).

### <a name="claim-schema-entry-elements"></a>Talep şema girişi öğeleri

**Değer:** Değer öğesini statik bir değer olarak talebi yayılan verileri tanımlar.

**Kaynak/çifti kimliği:** Gelen talep verileri nerede kaynağı kaynak ve kimliği öğelerini tanımlayın. 

Kaynak öğesi aşağıdaki değerlerden birine ayarlayın: 

- "kullanıcı": Kullanıcı nesnesindeki bir özellik talep verilerdir. 
- "uygulama": Talep verileri, uygulama (istemci) hizmet sorumlusu üzerindeki bir özelliktir. 
- "kaynak": Talep verileri kaynak hizmet sorumlusu üzerindeki bir özelliktir.
- "audience": Talep verileri hedef kitlesi belirtecin (istemci veya kaynak hizmet sorumlusu) hizmet sorumlusu üzerindeki bir özelliktir.
- "Şirket": Talep verileri kaynak kiracının şirket nesne üzerindeki bir özelliktir.
- "dönüştürme": Talep dönüştürme talep verilerdir (Bu makalenin sonraki bölümlerinde "talep dönüştürme" bölümüne bakın).

Kaynak, dönüştürme işlemi ise **TransformationID** öğesi bu talep tanımında de dahil edilir.

Kaynak hangi özelliğinin değeri için talep sağlar. kimlik öğesi tanımlar. Aşağıdaki tabloda, her kaynak değeri için geçerli kimlik değerleri listelenmektedir.

#### <a name="table-3-valid-id-values-per-source"></a>Tablo 3: Kaynak başına geçerli kimlik değerleri

| Kaynak | Kimlik | Açıklama |
|-----|-----|-----|
| Kullanıcı | Soyadı | Aile adı |
| Kullanıcı | givenName | Verilen Ad |
| Kullanıcı | DisplayName | Görünen Ad |
| Kullanıcı | Nesne Kimliği | ObjectID |
| Kullanıcı | posta | E-posta adresi |
| Kullanıcı | userPrincipalName | Kullanıcı Asıl Adı |
| Kullanıcı | Bölüm|Bölüm|
| Kullanıcı | onpremisessamaccountname | Şirket içi SAM hesabı adı |
| Kullanıcı | netbiosname| NetBIOS adı |
| Kullanıcı | dnsdomainname | DNS Etki Alanı Adı |
| Kullanıcı | onpremisesecurityidentifier | Şirket içi güvenlik tanımlayıcısı |
| Kullanıcı | Şirket adı| Kuruluş Adı |
| Kullanıcı | streetAddress | Posta Adresi |
| Kullanıcı | posta kodu | Posta Kodu |
| Kullanıcı | preferredlanguange | Tercih Edilen Dil |
| Kullanıcı | onpremisesuserprincipalname | Şirket içi UPN |
| Kullanıcı | mailnickname | Posta Takma Adı |
| Kullanıcı | extensionattribute1 | Uzantı özniteliğine 1 |
| Kullanıcı | extensionattribute2 | Uzantı özniteliğine 2 |
| Kullanıcı | extensionattribute3 | Uzantı özniteliğine 3 |
| Kullanıcı | extensionattribute4 | Uzantı özniteliğine 4 |
| Kullanıcı | extensionattribute5 | Uzantı özniteliğine 5 |
| Kullanıcı | extensionattribute6 | Uzantı özniteliğine 6 |
| Kullanıcı | extensionattribute7 | Uzantı özniteliğine 7 |
| Kullanıcı | extensionattribute8 | Uzantı özniteliğine 8 |
| Kullanıcı | extensionattribute9 | Uzantı özniteliğine 9 |
| Kullanıcı | extensionattribute10 | Uzantı özniteliğine 10 |
| Kullanıcı | extensionattribute11 | Uzantı özniteliğine 11 |
| Kullanıcı | extensionattribute12 | Uzantı özniteliğine 12 |
| Kullanıcı | extensionattribute13 | Uzantı özniteliğine 13 |
| Kullanıcı | extensionattribute14 | Uzantı özniteliğine 14 |
| Kullanıcı | extensionattribute15 | Uzantı özniteliğine 15 |
| Kullanıcı | othermail | Diğer e-posta |
| Kullanıcı | Ülke | Ülke |
| Kullanıcı | city | Şehir |
| Kullanıcı | durum | Durum |
| Kullanıcı | İş Unvanı | İş Unvanı |
| Kullanıcı | EmployeeID | Çalışan Kimliği |
| Kullanıcı | facsimiletelephonenumber | Faks telefon numarası |
| Uygulama, kaynak, hedef kitle | DisplayName | Görünen Ad |
| Uygulama, kaynak, hedef kitle | eşitlemek | ObjectID |
| Uygulama, kaynak, hedef kitle | etiketler | Hizmet sorumlusu etiketi |
| Şirket | tenantcountry | Kiracının ülke |

**TransformationID:** Yalnızca kaynak öğesi "Dönüşümü" olarak ayarlanırsa TransformationID öğesi sağlanmalıdır.

- Bu öğe dönüştürme girişin kimlik öğesi eşleşmelidir **ClaimsTransformation** özelliği bu talep verileri nasıl oluşturulacağını tanımlar.

**Talep türü:** **JwtClaimType** ve **SamlClaimType** öğesi tanımlayabilir, bu talebi şema giriş başvurduğu talep.

- JwtClaimType Jwt'ler içinde derleyicisindeki talep adını içermelidir.
- SamlClaimType SAML belirteçleri derleyicisindeki talep URI'sini içermelidir.

> [!NOTE]
> Adları ve talep sınırlı talep kümesindeki bir URI'leri talep türü öğeler için kullanılamaz. Daha fazla bilgi için bu makalenin devamındaki "Özel durumlar ve sınırlamaları" bölümüne bakın.

### <a name="claims-transformation"></a>Talepleri dönüştürme

**Dize:** ClaimsTransformation

**Veri türü:** Bir veya daha fazla dönüştürme girişi JSON blob 

**Özet:** Kaynak verileri, talepleri talep şemasında belirtilen için çıktı verilerini oluşturmak için yaygın dönüşümleri uygulamak için bu özelliği kullanın.

**KİMLİĞİ:** Bu dönüştürme giriş TransformationID talep şema giriş başvurmak için kimlik öğesi kullanın. Bu değer, bu ilke içinde her dönüştürme giriş için benzersiz olmalıdır.

**TransformationMethod:** Tanımlayan TransformationMethod öğe talebi için verileri oluşturmak için hangi işlem gerçekleştirilir.

Seçtiğiniz yönteme bağlı olarak, bir dizi giriş ve çıkışları bekleniyor. Giriş ve çıkışları kullanarak tanımladığınız **InputClaims**, **InputParameters** ve **OutputClaims** öğeleri.

#### <a name="table-4-transformation-methods-and-expected-inputs-and-outputs"></a>Tablo 4: Dönüştürme yöntemleri ve beklenen giriş ve çıkışları

|TransformationMethod|Beklenen Giriş|Beklenen çıktı|Açıklama|
|-----|-----|-----|-----|
|Birleştir|dize1 dize2, ayırıcı|outputClaim|Birleştirmeler arasında bir ayırıcı kullanarak dizeleri girin. Örneğin: Dize1: "foo@bar.com", dize2: "korumalı alan", ayırıcı: "." outputClaim sonuçları: "foo@bar.com.sandbox"|
|ExtractMailPrefix|posta|outputClaim|Yerel bir e-posta adresi bölümünü ayıklar. Örneğin: posta: "foo@bar.com" outputClaim sonuçları: "foo". Hayır ise \@ oturum varsa ve ardından özgün giriş dizesi olarak döndürülür.|

**InputClaims:** Veri dönüştürme için bir talep şema girdisinden geçirmek için bir InputClaims öğesini kullanın. Bu iki öznitelikleri: **ClaimTypeReferenceId** ve **TransformationClaimType**.

- **ClaimTypeReferenceId** uygun giriş talep bulmak için talep şema giriş öğesinin kimliği ile birleştirilir. 
- **TransformationClaimType** bu giriş için benzersiz bir ad vermek için kullanılır. Bu ad, dönüştürme yöntemi için beklenen girişleri biriyle eşleşmelidir.

**InputParameters:** Dönüştürme için sabit bir değer geçirmek için bir InputParameters öğesini kullanın. Bu iki öznitelikleri: **Değer** ve **kimliği**.

- **Değer** geçirilecek gerçek sabit değer.
- **Kimliği** girişi benzersiz bir ad vermek için kullanılır. Adı bir dönüştürme yöntemi için beklenen girişleri eşleşmelidir.

**OutputClaims:** Bir talep şema girişine bağlayın ve OutputClaims öğenin dönüştürme tarafından oluşturulan verileri tutmak için kullanın. Bu iki öznitelikleri: **ClaimTypeReferenceId** ve **TransformationClaimType**.

- **ClaimTypeReferenceId** uygun çıkış talep bulmak için şema giriş talep Kimliğini ile birleştirilir.
- **TransformationClaimType** çıkış için benzersiz bir ad vermek için kullanılır. Adı dönüştürme yöntemi için beklenen çıkış biriyle eşleşmelidir.

### <a name="exceptions-and-restrictions"></a>Özel durumlar ve kısıtlamaları

**SAML Nameıd ve UPN:** İçinden Nameıd ve UPN değerleri ve izin verilen talep dönüştürmeleri kaynağı öznitelikleri sınırlıdır. Tablo 5 ve 6 izin verilen değerleri görmek için tablo bakın.

#### <a name="table-5-attributes-allowed-as-a-data-source-for-saml-nameid"></a>Tablo 5: Veri kaynağı olarak izin verilen SAML Nameıd için öznitelikler

|Kaynak|Kimlik|Açıklama|
|-----|-----|-----|
| Kullanıcı | posta|E-posta adresi|
| Kullanıcı | userPrincipalName|Kullanıcı Asıl Adı|
| Kullanıcı | onpremisessamaccountname|Şirket içi Sam hesabı adı|
| Kullanıcı | EmployeeID|Çalışan Kimliği|
| Kullanıcı | extensionattribute1 | Uzantı özniteliğine 1 |
| Kullanıcı | extensionattribute2 | Uzantı özniteliğine 2 |
| Kullanıcı | extensionattribute3 | Uzantı özniteliğine 3 |
| Kullanıcı | extensionattribute4 | Uzantı özniteliğine 4 |
| Kullanıcı | extensionattribute5 | Uzantı özniteliğine 5 |
| Kullanıcı | extensionattribute6 | Uzantı özniteliğine 6 |
| Kullanıcı | extensionattribute7 | Uzantı özniteliğine 7 |
| Kullanıcı | extensionattribute8 | Uzantı özniteliğine 8 |
| Kullanıcı | extensionattribute9 | Uzantı özniteliğine 9 |
| Kullanıcı | extensionattribute10 | Uzantı özniteliğine 10 |
| Kullanıcı | extensionattribute11 | Uzantı özniteliğine 11 |
| Kullanıcı | extensionattribute12 | Uzantı özniteliğine 12 |
| Kullanıcı | extensionattribute13 | Uzantı özniteliğine 13 |
| Kullanıcı | extensionattribute14 | Uzantı özniteliğine 14 |
| Kullanıcı | extensionattribute15 | Uzantı özniteliğine 15 |

#### <a name="table-6-transformation-methods-allowed-for-saml-nameid"></a>Tablo 6: SAML Nameıd için izin verilen dönüştürme yöntemleri

| TransformationMethod | Kısıtlamalar |
| ----- | ----- |
| ExtractMailPrefix | None |
| Birleştir | Birleştirilen soneki kaynak kiracının doğrulanmış bir etki alanı olmalıdır. |

### <a name="custom-signing-key"></a>Özel anahtar imzalama

Özel bir imzalama anahtarı etkili bir ilke eşleştirme talepler için hizmet sorumlusu nesnesi atanması gerekir. İlkeden etkilenen verilen tüm belirteçler özel bir imzalama anahtarıyla imzalanır ve uygulamaları belirteçleri kabul edecek şekilde yapılandırılması gerekir imzalama anahtarı ile imzalanmış. Bu belirteçler ilke eşleştirme talep oluşturucusu tarafından değiştirilmiş ve uygulamaları, kötü amaçlı aktörler tarafından oluşturulan ilkeler eşleme talep önler bildirim sağlar.

### <a name="cross-tenant-scenarios"></a>Kiracılar arası senaryoları

İlkeleri eşleme talep Konuk kullanıcılar için geçerli değildir. Konuk kullanıcı, kendi hizmet Sorumlusu'na atanan ilke eşleştirme önermelerle bir uygulamaya erişmeye çalışırsa, varsayılan belirteç verilir (ilke etkiye sahip değildir).

## <a name="claims-mapping-policy-assignment"></a>İlke ataması eşleme talepleri

Talep eşleme ilkeleri, yalnızca hizmet sorumlusu nesneleri atanabilir.

### <a name="example-claims-mapping-policies"></a>Örnek eşleme ilkeleri talepleri

Azure AD'de talep belirteçleri için belirli hizmet sorumluları yayılan özelleştirdiğinizde birçok senaryo mümkündür. Bu bölümde, ilke türü eşleme talep kullanmayı kavrayın yardımcı olabilecek bazı yaygın senaryolar üzerinden inceleyeceğiz.

#### <a name="prerequisites"></a>Önkoşullar

Aşağıdaki örneklerde, oluşturma, güncelleştirme, bağlantı ve ilkeleri için hizmet sorumluları silme. Azure AD'ye yeniyseniz olmasını öneririz, [bir Azure AD kiracısı edinme hakkında bilgi edinin](quickstart-create-new-tenant.md) bu örnekleri ile devam etmeden önce.

Başlamak için aşağıdaki adımları uygulayın:

1. En son sürümünü indirin [Azure AD PowerShell modülünün genel Önizleme sürümü](https://www.powershellgallery.com/packages/AzureADPreview).
1. Azure AD yönetici hesabınızda oturum açmak için Connect komutunu çalıştırın. Her zaman bu komutu çalıştırın, yeni bir oturum başlatın.

   ``` powershell
   Connect-AzureAD -Confirm
   ```
1. Kuruluşunuzda oluşturulan tüm ilkeleri görmek için aşağıdaki komutu çalıştırın. Çoğu işlemlerinin beklendiği gibi ilkelerinizi oluşturulan denetlemek için aşağıdaki senaryolarda bu komut çalıştırmanızı öneririz.

   ``` powershell
   Get-AzureADPolicy
   ```

#### <a name="example-create-and-assign-a-policy-to-omit-the-basic-claims-from-tokens-issued-to-a-service-principal"></a>Örnek: Bir hizmet sorumlusu için verilen belirteçlere gelen temel talepleri atlamak için bir ilkesi oluşturma ve atama

Bu örnekte, temel talep kümesine bağlı hizmet sorumlusu için verilen belirteçlere kaldırır. bir ilke oluşturun.

1. Bir talep İlkesi eşlemesi oluşturun. Bu ilke, belirli bağlı hizmet sorumlularını belirteçleri temel talep kaldırır.
   1. İlkeyi oluşturmak için şu komutu çalıştırın: 
    
      ``` powershell
      New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"false"}}') -DisplayName "OmitBasicClaims" -Type "ClaimsMappingPolicy"
      ```
   2. Yeni ilkeniz bakın ve objectID ilkeyi almak için aşağıdaki komutu çalıştırın:
    
      ``` powershell
      Get-AzureADPolicy
      ```
1. Hizmet sorumlunuzu ilkeyi atayın. Ayrıca hizmetinizin objectID asıl almanız gerekir.
   1. Kuruluşunuzun tüm hizmet sorumlularını görmek için Microsoft Graph sorgulayabilirsiniz. Veya Azure AD hesabınızın Azure AD Graph Explorer'da oturum açın.
   2. Hizmet sorumlusu nesne kimliği varsa, aşağıdaki komutu çalıştırın:  
     
      ``` powershell
      Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
      ```

#### <a name="example-create-and-assign-a-policy-to-include-the-employeeid-and-tenantcountry-as-claims-in-tokens-issued-to-a-service-principal"></a>Örnek: EmployeeID ve TenantCountry bir hizmet sorumlusu için verilen belirteçlere talep olarak dahil etmek için bir ilkesi oluşturma ve atama

Bu örnekte, bağlı hizmet sorumlusu için verilen belirteçlere EmployeeID ve TenantCountry ekleyen bir ilke oluşturun. EmployeeID SAML belirteçlerini hem Jwt'ler adı talep türü olarak yayınlanır. SAML belirteçleri hem Jwt'ler ülke talep türü olarak TenantCountry yayılır. Bu örnekte, belirteçleri ayarlamak temel talep içerecek şekilde devam ediyoruz.

1. Bir talep İlkesi eşlemesi oluşturun. Bu ilke, belirli bir hizmet sorumlusu için bağlantılı, belirteçleri EmployeeID ve TenantCountry talep ekler.
   1. İlkeyi oluşturmak için aşağıdaki komutu çalıştırın:  
     
      ``` powershell
      New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"true", "ClaimsSchema": [{"Source":"user","ID":"employeeid","SamlClaimType":"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name","JwtClaimType":"name"},{"Source":"company","ID":"tenantcountry","SamlClaimType":"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/country","JwtClaimType":"country"}]}}') -DisplayName "ExtraClaimsExample" -Type "ClaimsMappingPolicy"
      ```
    
   2. Yeni ilkeniz bakın ve objectID ilkeyi almak için aşağıdaki komutu çalıştırın:
     
      ``` powershell  
      Get-AzureADPolicy
      ```
1. Hizmet sorumlunuzu ilkeyi atayın. Ayrıca hizmetinizin objectID asıl almanız gerekir. 
   1. Kuruluşunuzun tüm hizmet sorumlularını görmek için Microsoft Graph sorgulayabilirsiniz. Veya Azure AD hesabınızın Azure AD Graph Explorer'da oturum açın.
   2. Hizmet sorumlusu nesne kimliği varsa, aşağıdaki komutu çalıştırın:  
     
      ``` powershell
      Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
      ```

#### <a name="example-create-and-assign-a-policy-that-uses-a-claims-transformation-in-tokens-issued-to-a-service-principal"></a>Örnek: Bir hizmet sorumlusu için verilen belirteçlere talep dönüştürme kullanan bir ilkesi oluşturma ve atama

Bu örnekte, "JoinedData" bağlı hizmet sorumlusu için verilen Jwt'ler için bir özel talep yayan bir ilke oluşturun. Bu talep extensionattribute1 özniteliğinde ".sandbox" ile kullanıcı nesnesinde depolanan verileri birleştirilerek oluşturulan bir değer içerir. Bu örnekte, belirteçleri ayarlamak temel talep tutarız.

1. Bir talep İlkesi eşlemesi oluşturun. Bu ilke, belirli bir hizmet sorumlusu için bağlantılı, belirteçleri EmployeeID ve TenantCountry talep ekler.
   1. İlkeyi oluşturmak için aşağıdaki komutu çalıştırın:
     
      ``` powershell
      New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"true", "ClaimsSchema":[{"Source":"user","ID":"extensionattribute1"},{"Source":"transformation","ID":"DataJoin","TransformationId":"JoinTheData","JwtClaimType":"JoinedData"}],"ClaimsTransformations":[{"ID":"JoinTheData","TransformationMethod":"Join","InputClaims":[{"ClaimTypeReferenceId":"extensionattribute1","TransformationClaimType":"string1"}], "InputParameters": [{"ID":"string2","Value":"sandbox"},{"ID":"separator","Value":"."}],"OutputClaims":[{"ClaimTypeReferenceId":"DataJoin","TransformationClaimType":"outputClaim"}]}]}}') -DisplayName "TransformClaimsExample" -Type "ClaimsMappingPolicy"
      ```
    
   2. Yeni ilkeniz bakın ve objectID ilkeyi almak için aşağıdaki komutu çalıştırın: 
     
      ``` powershell
      Get-AzureADPolicy
      ```
1. Hizmet sorumlunuzu ilkeyi atayın. Ayrıca hizmetinizin objectID asıl almanız gerekir. 
   1. Kuruluşunuzun tüm hizmet sorumlularını görmek için Microsoft Graph sorgulayabilirsiniz. Veya Azure AD hesabınızın Azure AD Graph Explorer'da oturum açın.
   2. Hizmet sorumlusu nesne kimliği varsa, aşağıdaki komutu çalıştırın: 
     
      ``` powershell
      Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
      ```

## <a name="see-also"></a>Ayrıca bkz.

Azure portalı üzerinden SAML belirtecinde verilen talepleri özelleştirme öğrenmek için bkz: [nasıl yapılır: Kurumsal uygulamalar için SAML belirtecinde verilen talepleri özelleştirme](active-directory-saml-claims-customization.md)