---
title: "Talep eşleme Azure Active Directory'de (genel Önizleme) | Microsoft Docs"
description: "Bu sayfa, Azure Active Directory talep eşleme açıklar."
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: billmath
ms.openlocfilehash: 1bc669dfa5a41e38b35751af62560ff650575a08
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2018
---
# <a name="claims-mapping-in-azure-active-directory-public-preview"></a>Azure Active Directory'de (genel Önizleme) eşleme talepleri

>[!NOTE]
>Bu özellik değiştirir ve yerini [özelleştirme talep](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization) bugün Portalı aracılığıyla sunulan. Bu belgede aynı uygulama hakkında ayrıntılı grafik/PowerShell yöntemi yanı sıra Portalı'nı kullanarak talep özelleştirirseniz için uygulama yapılandırma Portalı'nda yoksayacak verilen belirteçler.
Bu belgede ayrıntılı yöntemler aracılığıyla yapılan yapılandırmalar portalda yansıtılmaz.

Bu özellik, kendi Kiracı içinde belirli bir uygulamayı belirteçlerinde gösterilen talep özelleştirmek için Kiracı yöneticileri tarafından kullanılır. İlkeleri için eşleme talep kullanabilirsiniz:

- Hangi taleplerin belirteçleri dahil edilen seçin.
- Henüz yoksa talep türlerini oluşturun.
- Seçin veya belirli Taleplerde yayılan veri kaynağı olarak değiştirin.

>[!NOTE]
>Bu özellik şu anda genel önizlemede değil. Geri veya herhangi bir değişiklik kaldırmak hazırlıklı olun. Genel Önizleme sırasında herhangi bir Azure Active Directory (Azure AD) Abonelik özelliği kullanılabilir. Ancak, özelliği genel kullanıma sunulduğunda özelliği bazı yönlerini bir Azure Active Directory premium aboneliği gerektirebilir.

## <a name="claims-mapping-policy-type"></a>İlke türü eşleme talepleri
Azure AD'de bir **İlkesi** nesnesi, tek tek uygulamalar veya bir kuruluştaki tüm uygulamaları zorlanan kurallar kümesini temsil eder. Her ilke türü bir benzersiz yapısıyla sonra atanmış olan nesnelere uygulanan özellikler kümesi vardır.

Bir talep ilke eşleme türüdür **İlkesi** belirli uygulamalar için yayınlanan belirteçleri gösterilen talep değiştirir nesnesi.

## <a name="claim-sets"></a>Talep kümeleri
Belirli nasıl ve ne zaman belirteçleri kullanıldıkları tanımlayan talep kümesi vardır.

### <a name="core-claim-set"></a>Çekirdek talep kümesi
Çekirdek talep kümesindeki talepler İlkesi bağımsız olarak her belirteci yok. Bu talep sınırlı kabul edilir ve değiştirilemez.

### <a name="basic-claim-set"></a>Temel talep kümesi
Temel talep kümesi belirteçleri (ek olarak çekirdek talep kümesi) için varsayılan olarak gösterilen talepleri içerir. Bu talep atlanmış veya ilkeleri eşleme talep kullanılarak değiştirilebilir.

### <a name="restricted-claim-set"></a>Kısıtlı talep kümesi
Kısıtlı talep İlkesi kullanılarak değiştirilemez. Veri kaynağı değiştirilemez ve hiçbir dönüştürme bu talepler oluştururken uygulanır.

#### <a name="table-1-json-web-token-jwt-restricted-claim-set"></a>Tablo 1: JSON Web Token (JWT) talep kümesi kısıtlanmış.
|Talep türü (ad)|
| ----- |
|_claim_names|
|_claim_sources|
|access_token|
|account_type|
|acr|
|Aktör|
|actortoken|
|AIO|
|altsecid|
|amr|
|app_chain|
|app_displayname|
|app_res|
|appctx|
|appctxsender|
|AppID|
|appidacr|
|onaylama işlemi|
|at_hash|
|aud|
|auth_data|
|auth_time|
|authorization_code|
|azp|
|azpacr|
|c_hash|
|ca_enf|
|Cc|
|cert_token_use|
|client_id|
|cloud_graph_host_name|
|cloud_instance_name|
|cnf|
|Kod|
|denetimler|
|credential_keys|
|CSR|
|csr_type|
|cihaz kimliği|
|dns_names|
|domain_dns_name|
|domain_netbios_name|
|e_exp|
|e-posta|
|endpoint|
|enfpolids|
|exp|
|expires_on|
|grant_type|
|Graf|
|group_sids|
|gruplar|
|hasgroups|
|hash_alg|
|home_oid|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/authenticationinstant|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/AuthenticationMethod|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/Expiration|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/Expired|
|http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/emailaddress|
|http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/Name|
|http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/nameidentifier|
|IAT|
|ıdentityprovider|
|IDP|
|in_corp|
|örnek|
|IpAddr|
|isbrowserhostedapp|
|ISS|
|jwk|
|key_id|
|key_type|
|mam_compliance_url|
|mam_enrollment_url|
|mam_terms_of_use_url|
|mdm_compliance_url|
|mdm_enrollment_url|
|mdm_terms_of_use_url|
|nameid|
|NBF|
|netbios_name|
|nonce|
|OID|
|on_prem_id|
|onprem_sam_account_name|
|onprem_sid|
|openid2_id|
|password|
|platf|
|polids|
|pop_jwk|
|preferred_username|
|previous_refresh_token|
|primary_sid|
|PUID|
|pwd_exp|
|pwd_url|
|redirect_uri|
|refresh_token|
|refreshtoken|
|request_nonce|
|kaynak|
|rol|
|roles|
|Kapsam|
|SCP|
|SID|
|İmza|
|signin_state|
|src1|
|src2|
|Sub|
|tbid|
|tenant_display_name|
|tenant_region_scope|
|thumbnail_photo|
|komutu|
|tokenAutologonEnabled|
|trustedfordelegation|
|unique_name|
|UPN|
|user_setting_sync_url|
|kullanıcı adı|
|uti|
|ver|
|verified_primary_email|
|verified_secondary_email|
|wids|
|win_ver|

#### <a name="table-2-security-assertion-markup-language-saml-restricted-claim-set"></a>Tablo 2: Güvenlik onaylama işlemi biçimlendirme dili (SAML) talep kümesi kısıtlanmış.
|Talep türü (URI)|
| ----- |
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/Expiration|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/Expired|
|http://schemas.microsoft.com/identity/Claims/accesstoken|
|http://schemas.microsoft.com/identity/Claims/openid2_id|
|http://schemas.microsoft.com/identity/Claims/identityprovider|
|http://schemas.microsoft.com/identity/Claims/objectidentifier|
|http://schemas.microsoft.com/identity/Claims/puid|
|http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/nameidentifier [MR1] |
|http://schemas.microsoft.com/identity/Claims/tenantid|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/authenticationinstant|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/AuthenticationMethod|
|http://schemas.microsoft.com/accesscontrolservice/2010/07/Claims/identityprovider|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/groups|
|http://schemas.microsoft.com/Claims/Groups.Link|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/role|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/wids|
|http://schemas.microsoft.com/2014/09/devicecontext/Claims/iscompliant|
|http://schemas.microsoft.com/2014/02/devicecontext/Claims/isknown|
|http://schemas.microsoft.com/2012/01/devicecontext/Claims/ismanaged|
|http://schemas.microsoft.com/2014/03/psso|
|http://schemas.microsoft.com/Claims/authnmethodsreferences|
|http://schemas.xmlsoap.org/ws/2009/09/identity/Claims/Actor|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/samlissuername|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/confirmationkey|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowsaccountname|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/primarygroupsid|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/primarysid|
|http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/authorizationdecision|
|http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/Authentication|
|http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/Sid|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/denyonlyprimarygroupsid|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/denyonlyprimarysid|
|http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/denyonlysid|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/denyonlywindowsdevicegroup|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowsdeviceclaim|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowsdevicegroup|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowsfqbnversion|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowssubauthority|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowsuserclaim|
|http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/x500distinguishedname|
|http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/UPN|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/groupsid|
|http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/SPN|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/ispersistent|
|http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/privatepersonalidentifier|
|http://schemas.microsoft.com/identity/Claims/Scope|

## <a name="claims-mapping-policy-properties"></a>Talep eşleme ilkesi özellikleri
Gelen verileri nerede kaynaklıdır ve hangi taleplerin yayılan Denetim İlkesi eşleme talep özelliklerini kullanın. Hiçbir ilke ayarlanırsa, sistem çekirdek talep kümesi, temel talep kümesi ve uygulama almak için seçtiği isteğe bağlı talepleri içeren belirteçleri.

### <a name="include-basic-claim-set"></a>Temel talep kümesi içerir

**Dize:** IncludeBasicClaimSet

**Veri türü:** Boolean (True veya False)

**Özet:** bu özelliği, bu ilkeden etkilenen belirteçlerinde temel talep kümesine dahil edilip edilmediğini belirler. 

- TRUE olarak temel talep kümesindeki tüm talepler ilkeden etkilenen belirteçlerinde gösterilen durumunda. 
- Tek tek aynı ilkesini talep şema özelliğinde eklenmediği sürece temel talep kümesindeki talepler False olarak ayarlanırsa belirteçleri değilse.

>[!NOTE] 
>Çekirdek talep kümesindeki talepler bu özelliği ayarlamak bağımsız olarak her belirteci yok. 

### <a name="claims-schema"></a>Talep şeması

**Dize:** ClaimsSchema

**Veri türü:** bir veya daha fazla talep şema girişlerle JSON blob

**Özet:** çekirdek talep kümesi ve hangi taleplerin temel talep kümesine ek olarak ilke tarafından etkilenen belirteçleri yok Bu özellik tanımlar.
Bu özelliği içinde tanımlı her talep şema giriş için belirli bilgiler gereklidir. Burada veri geldiği belirtmeniz gerekir (**değeri** veya **kaynak/kimliği çifti**), ve hangi veri talep olarak gösterilen (**talep türü**).

### <a name="claim-schema-entry-elements"></a>Talep şema girişi öğeleri

**Değer:** Değer öğesini talep yayınlaması için verileri olarak statik bir değer tanımlar.

**Kaynak/Kimliği çifti:** Source ve kimliği öğelerini tanımlama gelen talep verileri nerede kaynaklıdır. 

Source öğesi aşağıdakilerden birini ayarlamanız gerekir: 


- "kullanıcı": talep verileri kullanıcı nesnesinin bir özelliğidir. 
- "uygulama": talep verileri uygulama (istemci) hizmet asıl üzerindeki bir özelliktir. 
- "kaynak": talep verileri kaynak hizmet sorumlusu üzerindeki bir özelliktir.
- "İzleyici": talep verileri (istemci veya kaynak hizmet sorumlusu) belirteci İzleyici olan hizmet sorumlusu üzerindeki bir özelliktir.
- "Şirket": talep verileri kaynak kiracının şirket nesne üzerinde bir özelliktir.
- "dönüştürmesi": talep dönüştürme veridir talep (Bu makalenin sonraki bölümlerinde "dönüştürme talep" bölümüne bakın). 

Kaynak dönüştürme ise **TransformationID** öğesi de bu talep tanımını eklenmesi gerekir.

ID öğesi, kaynak bilgisayarda hangi özelliğinin değeri için talep sağlar tanımlar. Aşağıdaki tablo kimliği kaynak her bir değer için geçerli değerleri listeler.

#### <a name="table-3-valid-id-values-per-source"></a>Tablo 3: Kaynak başına geçerli kimlik değeri
|Kaynak|Kimlik|Açıklama|
|-----|-----|-----|
|Kullanıcı|Soyadı|Aile adı|
|Kullanıcı|givenName|Ad|
|Kullanıcı|görünen adı|Görünen Ad|
|Kullanıcı|objectID|ObjectID|
|Kullanıcı|Posta|E-posta adresi|
|Kullanıcı|userPrincipalName|Kullanıcı Asıl Adı|
|Kullanıcı|Bölüm|Bölüm|
|Kullanıcı|onpremisessamaccountname|Şirket içi Sam hesap adı|
|Kullanıcı|netbiosname|NetBIOS adı|
|Kullanıcı|DNSEtkiAlanıAdı|DNS etki alanı adı|
|Kullanıcı|onpremisesecurityidentifier|Şirket içi güvenlik tanımlayıcısı|
|Kullanıcı|Şirket adı|Kuruluş Adı|
|Kullanıcı|streetAddress|Posta Adresi|
|Kullanıcı|posta kodu|Posta Kodu|
|Kullanıcı|preferredlanguange|Tercih Edilen Dil|
|Kullanıcı|onpremisesuserprincipalname|Şirket içi UPN|
|Kullanıcı|mailnickname|Posta takma adı|
|Kullanıcı|extensionattribute1|1 uzantısı özniteliği|
|Kullanıcı|extensionattribute2|Uzantı özniteliği 2|
|Kullanıcı|extensionattribute3|3 uzantısı özniteliği|
|Kullanıcı|extensionattribute4|Uzantı özniteliği 4|
|Kullanıcı|extensionattribute5|Uzantı özniteliği 5|
|Kullanıcı|extensionattribute6|6 uzantısı özniteliği|
|Kullanıcı|extensionattribute7|Uzantı özniteliği 7|
|Kullanıcı|extensionattribute8|Uzantı özniteliği 8|
|Kullanıcı|extensionattribute9|Uzantı özniteliği 9|
|Kullanıcı|extensionattribute10|Uzantı özniteliği 10|
|Kullanıcı|extensionattribute11|Uzantı özniteliği 11|
|Kullanıcı|extensionattribute12|Uzantı özniteliği 12|
|Kullanıcı|extensionattribute13|Uzantı özniteliği 13|
|Kullanıcı|extensionattribute14|Uzantı özniteliği 14|
|Kullanıcı|extensionattribute15|Uzantı özniteliği 15|
|Kullanıcı|othermail|Diğer posta|
|Kullanıcı|Ülke|Ülke|
|Kullanıcı|city|Şehir|
|Kullanıcı|durum|Durum|
|Kullanıcı|İş Unvanı|İş Unvanı|
|Kullanıcı|EmployeeID|Çalışan Kimliği|
|Kullanıcı|facsimiletelephonenumber|Faks telefon numarası|
|Uygulama, kaynak, hedef kitle|görünen adı|Görünen Ad|
|Uygulama, kaynak, hedef kitle|ait nesneleri|ObjectID|
|Uygulama, kaynak, hedef kitle|etiketler|Hizmet sorumlusu etiketi|
|Şirket|tenantcountry|Kiracının ülke|

**TransformationID:** yalnızca Source öğesi "Dönüşümü" ayarlandıysa TransformationID öğesi sağlanmalıdır.

- Bu öğe kimliği öğesi dönüştürme girişi ile eşleşmesi gerekir **ClaimsTransformation** bu talep verileri nasıl oluşturulacağını tanımlar özelliği.

**Talep türü:** **JwtClaimType** ve **SamlClaimType** öğeleri tanımlamak için bu talep şema giriş başvuruyor talep.

- JwtClaimType içinde Jwt'ler yayınlaması için talep adını içermelidir.
- SamlClaimType URI SAML belirteçleri yayınlaması için bir talep içermelidir.

>[!NOTE]
>Adları ve URI'ler kısıtlı talep kümesindeki talep için talep türü öğeleri kullanılamaz. Daha fazla bilgi için bu makalenin sonraki bölümlerinde "Özel durumlar ve kısıtlamaları" bölümüne bakın.

### <a name="claims-transformation"></a>Talep dönüştürme

**Dize:** ClaimsTransformation

**Veri türü:** bir veya daha fazla dönüştürme girişlerle JSON blob 

**Özet:** talep şemasında belirtilen talepler için çıktı verileri oluşturmak için kaynak verileri, ortak dönüştürmeleri uygulamak için bu özelliği kullanın.

**Kimliği:** TransformationID talep şema girişinde bu dönüşüm girişinin başvurmak için kimliği öğesini kullanın. Bu değer her dönüştürme giriş Bu ilke içinde benzersiz olması gerekir.

**TransformationMethod:** tanımlayan TransformationMethod öğe talebi için verileri oluşturmak için hangi işlemi gerçekleştirilir.

Seçilen yönteme bağlı olarak, bir dizi girişleri ve çıkışları beklenir. Bunlar kullanılarak tanımlanmış **InputClaims**, **InputParameters** ve **OutputClaims** öğeleri.

#### <a name="table-4-transformation-methods-and-expected-inputs-and-outputs"></a>Tablo 4: Dönüştürme yöntemleri ve beklenen girişleri ve çıkışları
|TransformationMethod|Beklenen Giriş|Beklenen çıktı|Açıklama|
|-----|-----|-----|-----|
|Birleştir|dize1, dize2, ayırıcı|outputClaim|Birleşimler, ayırıcı arasında kullanarak dizeleri girin. Örneğin: Dize1: "foo@bar.com", dize2: "korumalı alan", ayırıcı: "." sonuçları içinde outputClaim: "foo@bar.com.sandbox"|
|ExtractMailPrefix|Posta|outputClaim|Bir e-posta adresi yerel parçası ayıklar. Örneğin: posta: "foo@bar.com" sonuçları içinde outputClaim: "foo". Orignal giriş dizesi olduğu gibi döndürülür Hayır @ işareti varsa, ise.|

**InputClaims:** veri bir talep şema girdisinden dönüştürme için iletmek için bir InputClaims öğesini kullanın. İki özniteliklere sahiptir: **ClaimTypeReferenceId** ve **TransformationClaimType**.

- **ClaimTypeReferenceId** uygun giriş talep bulmak için talep şema girişi kimliği öğesi ile birleştirilir. 
- **TransformationClaimType** bu girişi için benzersiz bir ad vermek için kullanılır. Bu ad dönüştürme yönteminde beklenen girişleri eşleşmelidir.

**InputParameters:** InputParameters öğenin dönüşümü için sabit bir değer geçirmek için kullanın. İki özniteliklere sahiptir: **değeri** ve **kimliği**.

- **Değer** geçirilecek gerçek sabit değer.
- **Kimliği** bu girişi için benzersiz bir ad vermek için kullanılır. Bu ad dönüştürme yönteminde beklenen girişleri eşleşmelidir.

**OutputClaims:** bir dönüşüm tarafından oluşturulan verileri tutmak için bir OutputClaims öğesini kullanın ve bir talep şema girişine bağlayın. İki özniteliklere sahiptir: **ClaimTypeReferenceId** ve **TransformationClaimType**.

- **ClaimTypeReferenceId** uygun çıkış talep bulmak için talep şema girişi Kimliğini ile birleştirilir.
- **TransformationClaimType** Bu çıktı için benzersiz bir ad vermek için kullanılır. Bu ad dönüştürme yöntemi için beklenen çıkış biriyle eşleşmelidir.

### <a name="exceptions-and-restrictions"></a>Özel durumlar ve kısıtlamaları

**SAML NameID ve UPN:** içinden kaynağı NameID ve UPN değerleri ve izin verilen talep dönüştürmeleri öznitelikler sınırlıdır.

#### <a name="table-5-attributes-allowed-as-a-data-source-for-saml-nameid"></a>Tablo 5: bir veri kaynağı olarak SAML NameID için izin verilen öznitelikleri
|Kaynak|Kimlik|Açıklama|
|-----|-----|-----|
|Kullanıcı|Posta|E-posta adresi|
|Kullanıcı|userPrincipalName|Kullanıcı Asıl Adı|
|Kullanıcı|onpremisessamaccountname|Şirket içi Sam hesap adı|
|Kullanıcı|EmployeeID|Çalışan Kimliği|
|Kullanıcı|extensionattribute1|1 uzantısı özniteliği|
|Kullanıcı|extensionattribute2|Uzantı özniteliği 2|
|Kullanıcı|extensionattribute3|3 uzantısı özniteliği|
|Kullanıcı|extensionattribute4|Uzantı özniteliği 4|
|Kullanıcı|extensionattribute5|Uzantı özniteliği 5|
|Kullanıcı|extensionattribute6|6 uzantısı özniteliği|
|Kullanıcı|extensionattribute7|Uzantı özniteliği 7|
|Kullanıcı|extensionattribute8|Uzantı özniteliği 8|
|Kullanıcı|extensionattribute9|Uzantı özniteliği 9|
|Kullanıcı|extensionattribute10|Uzantı özniteliği 10|
|Kullanıcı|extensionattribute11|Uzantı özniteliği 11|
|Kullanıcı|extensionattribute12|Uzantı özniteliği 12|
|Kullanıcı|extensionattribute13|Uzantı özniteliği 13|
|Kullanıcı|extensionattribute14|Uzantı özniteliği 14|
|Kullanıcı|extensionattribute15|Uzantı özniteliği 15|

#### <a name="table-6-transformation-methods-allowed-for-saml-nameid"></a>Tablo 6: SAML NameID için izin verilen dönüştürme yöntemleri
|TransformationMethod|Kısıtlamalar|
| ----- | ----- |
|ExtractMailPrefix|Hiçbiri|
|Birleştir|Birleştirilen soneki doğrulanmış bir etki alanı kaynak Kiracı olmalıdır.|

### <a name="custom-signing-key"></a>Özel anahtar imzalama
Özel bir imzalama anahtarı etkili bir ilke eşleme talepler için hizmet sorumlusu nesnesi atanmalıdır. İlke tarafından etkilenip etkilenmediğinizi yayınlanan tüm belirteçleri bu anahtarla imzalanmıştır. Uygulamaları yapılandırılmış, belirteçleri kabul etmek için bu anahtarı ile imzalanmış. Bu bildirim sağlar belirteçleri ilke eşleme talep oluşturucusu tarafından değiştirilmiş. Bu uygulamaları kötü amaçlı aktörler tarafından oluşturulan ilkeler eşleme talep önler.

### <a name="cross-tenant-scenarios"></a>Çapraz Kiracı senaryoları
İlkeleri eşleme talep Konuk kullanıcılar için geçerli değildir. Konuk kullanıcı kendi hizmet sorumlusuna atanan ilke eşleme taleplerle bir uygulamaya erişmeye çalışırsa, varsayılan belirteci verilir (ilkenin hiçbir etkisi yoktur).

## <a name="claims-mapping-policy-assignment"></a>İlke ataması eşleme talepleri
İlkeleri eşleme talepleri yalnızca hizmet asıl nesneleri atanabilir.

### <a name="example-claims-mapping-policies"></a>Örnek eşleme ilkeleri talepleri

Azure AD'de belirteçleri belirli hizmet asıl adı için gösterilen talep özelleştirdiğinizde birçok senaryo mümkündür. Bu bölümde, ilke türü eşleme talep kullanma kavramak yardımcı olabilecek bazı yaygın senaryolar üzerinden yol.

#### <a name="prerequisites"></a>Önkoşullar
Aşağıdaki örneklerde, oluşturmak, güncelleştirmek, bağlantı ve ilkelerini için hizmet asıl adı silin. Azure AD ile yeni başladıysanız, bu örnekleri ile devam etmeden önce Azure AD kiracısı alma hakkında bilgi edinin öneririz. 

Başlamak için aşağıdaki adımları uygulayın:


1. En son karşıdan [Azure AD PowerShell modülü genel Önizleme sürümü](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.127).
2.  Azure AD Yönetici hesabınızla oturum açmak için Bağlan komutunu çalıştırın. Her zaman bu komutu çalıştırmak yeni bir oturum başlatın.
    
     ``` powershell
    Connect-AzureAD -Confirm
    
    ```
3.  Kuruluşunuzda oluşturulan tüm ilkeleri görmek için aşağıdaki komutu çalıştırın. İlkelerinizi beklendiği gibi oluşturulan denetlemek için aşağıdaki senaryolarda, çoğu işlemlerinden sonra bu komutu çalıştırmanız önerilir.
   
    ``` powershell
        Get-AzureADPolicy
    
    ```
#### <a name="example-create-and-assign-a-policy-to-omit-the-basic-claims-from-tokens-issued-to-a-service-principal"></a>Örnek: Oluşturun ve temel bir hizmet sorumlusu yayınlanan belirteçleri talepleri atlamak için bir ilke atayın.
Bu örnekte, bağlantılı hizmet asıl adı için yayınlanan belirteçleri temel talep kaldırır bir ilke oluşturun.


1. İlke eşleme bir talep oluşturmak. Bu ilke, belirli bağlı hizmet asıl adı, gelen belirteçleri temel talep kaldırır.
    1. Bir ilke oluşturmak için bu komutu çalıştırın: 
    
     ``` powershell
    New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"false"}}') -DisplayName "OmitBasicClaims” -Type "ClaimsMappingPolicy"
    ```
    2. Yeni ilke görmek ve objectID ilkeyi almak üzere aşağıdaki komutu çalıştırın:
    
     ``` powershell
    Get-AzureADPolicy
    ```
2.  İlke, hizmet sorumlusu atayın. Hizmetinizin objectID asıl almanız gerekir. 
    1.  Kuruluşunuzun tüm hizmet asıl adı görmek için Microsoft Graph sorgulayabilirsiniz. Veya, Azure AD hesabınıza Azure AD Graph Explorer'da oturum açın.
    2.  Hizmet sorumlusu objectID sahip olduğunuzda, aşağıdaki komutu çalıştırın:  
     
     ``` powershell
    Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
    ```
#### <a name="example-create-and-assign-a-policy-to-include-the-employeeid-and-tenantcountry-as-claims-in-tokens-issued-to-a-service-principal"></a>Örnek: Oluşturun ve bir hizmet sorumlusu yayınlanan belirteçleri Taleplerde olarak EmployeeID ve TenantCountry eklemek için bir ilke atayın.
Bu örnekte, bağlantılı hizmet asıl adı için yayınlanan belirteçleri EmployeeID ve TenantCountry ekleyen bir ilke oluşturun. EmployeeID SAML belirteçleri ve Jwt'ler adı talep türü olarak yayılır. TenantCountry SAML belirteçleri ve Jwt'ler ülke talep türü olarak yayılır. Bu örnekte, belirteçleri ayarlamak temel talep dahil etmek devam ediyoruz.

1. İlke eşleme bir talep oluşturmak. Belirli hizmet sorumluları için bağlantılı Bu ilkeyi belirteçleri EmployeeID ve TenantCountry talep ekler.
    1. Bir ilke oluşturmak için bu komutu çalıştırın:  
     
     ``` powershell
    New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"true", "ClaimsSchema": [{"Source":"user","ID":"employeeid","SamlClaimType":"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name","JwtClaimType":"name"},{"Source":"company","ID":" tenantcountry ","SamlClaimType":" http://schemas.xmlsoap.org/ws/2005/05/identity/claims/country ","JwtClaimType":"country"}]}}') -DisplayName "ExtraClaimsExample” -Type "ClaimsMappingPolicy"
    ```
    
    2. Yeni ilke görmek ve objectID ilkeyi almak üzere aşağıdaki komutu çalıştırın:
     
     ``` powershell  
    Get-AzureADPolicy
    ```
2.  İlke, hizmet sorumlusu atayın. Hizmetinizin objectID asıl almanız gerekir. 
    1.  Kuruluşunuzun tüm hizmet asıl adı görmek için Microsoft Graph sorgulayabilirsiniz. Veya, Azure AD hesabınıza Azure AD Graph Explorer'da oturum açın.
    2.  Hizmet sorumlusu objectID sahip olduğunuzda, aşağıdaki komutu çalıştırın:  
     
     ``` powershell
    Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
    ```
#### <a name="example-create-and-assign-a-policy-that-uses-a-claims-transformation-in-tokens-issued-to-a-service-principal"></a>Örnek: Oluşturun ve bir hizmet sorumlusu yayınlanan belirteçleri talep dönüştürme kullanan bir ilke atayın.
Bu örnekte, "JoinedData" için bağlı hizmet asıl adı verilen Jwt'ler için bir özel talep yayar bir ilke oluşturun. Bu talep ".sandbox" kullanıcı nesnesiyle extensionattribute1 öznitelikte depolanan veri birleştirilmesiyle oluşturulan bir değer içeriyor. Bu örnekte, belirteçleri ayarlamak temel talep dışlayın.


1. İlke eşleme bir talep oluşturmak. Belirli hizmet sorumluları için bağlantılı Bu ilkeyi belirteçleri EmployeeID ve TenantCountry talep ekler.
    1. Bir ilke oluşturmak için bu komutu çalıştırın: 
     
     ``` powershell
    New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"true", "ClaimsSchema":[{"Source":"user","ID":"extensionattribute1"},{"Source":"transformation","ID":"DataJoin","TransformationId":"JoinTheData","JwtClaimType":"JoinedData"}],"ClaimsTransformations":[{"ID":"JoinTheData","TransformationMethod":"Join","InputClaims":[{"ClaimTypeReferenceId":"extensionattribute1","TransformationClaimType":"string1"}], "InputParameters": [{"ID":"string2","Value":"sandbox"},{"ID":"separator","Value":"."}],"OutputClaims":[{"ClaimTypeReferenceId":"DataJoin","TransformationClaimType":"outputClaim"}]}]}}') -DisplayName "TransformClaimsExample" -Type "ClaimsMappingPolicy" 
    ```
    
    2. Yeni ilke görmek ve objectID ilkeyi almak üzere aşağıdaki komutu çalıştırın: 
     
     ``` powershell
    Get-AzureADPolicy
    ```
2.  İlke, hizmet sorumlusu atayın. Hizmetinizin objectID asıl almanız gerekir. 
    1.  Kuruluşunuzun tüm hizmet asıl adı görmek için Microsoft Graph sorgulayabilirsiniz. Veya, Azure AD hesabınıza Azure AD Graph Explorer'da oturum açın.
    2.  Hizmet sorumlusu objectID sahip olduğunuzda, aşağıdaki komutu çalıştırın: 
     
     ``` powershell
    Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
    ```
