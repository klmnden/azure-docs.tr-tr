---
title: Azure AD uygulamanız için isteğe bağlı bir talep sağla öğrenin | Microsoft Docs
description: Azure Active Directory tarafından verilen SAML 2.0 ve JSON Web belirteçleri (JWT) belirteçleri talepleri özel veya ek eklemek için bir kılavuz.
documentationcenter: na
author: rwike77
services: active-directory
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/22/2019
ms.author: ryanwi
ms.reviewer: paulgarn, hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8c0e5035331cbe4f54926f0ae60ae0c5c31f6a9a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66119715"
---
# <a name="how-to-provide-optional-claims-to-your-azure-ad-app"></a>Nasıl yapılır: Azure AD uygulamanız için isteğe bağlı bir talep sağla

Bu özellik, uygulama geliştiriciler tarafından hangi kullanıcıların uygulamaya gönderilen belirteçlerde istediği talepleri belirtmek için kullanılır. İsteğe bağlı talepleri kullanabilirsiniz:

- Ek talep belirteçleri uygulamanıza eklemek için seçin.
- Azure AD belirteçleri döndüren belirli talep davranışını değiştirin.
- Ekleme ve özel talepler uygulamanıza erişebilirsiniz.

Standart talep listesi için bkz. [erişim belirteci](access-tokens.md) ve [id_token](id-tokens.md) belgeleri talepleri. 

İsteğe bağlı bir talep hem v1.0 ve v2.0 biçim belirteçleri, aynı zamanda SAML belirteçlerini desteklendiğinden, v1.0, v2.0 için taşırken bunlar değerlerine çoğunu sağlar. Hedeflerinden [Azure AD v2.0 uç noktası](active-directory-appmodel-v2-overview.md) istemciler tarafından en iyi performansı elde etmek için daha küçük simge boyutları. Sonuç olarak, eski erişim ve kimlik belirteçlerini dahil birkaç talep artık v2.0 belirteçleri varsa ve için özellikle uygulama başına temelinde sorulması gerekir.

**Tablo 1: Uygulanabilirlik**

| Hesap türü | V1.0 belirteçleri | V2.0 belirteçleri  |
|--------------|---------------|----------------|
| Kişisel Microsoft hesabı  | Yok  | Desteklenen|
| Azure AD hesabı      | Desteklenen | Desteklenen |

## <a name="v10-and-v20-optional-claims-set"></a>V1.0 ve isteğe bağlı taleplerin V2.0 ayarlayın

İsteğe bağlı varsayılan olarak kullanılabilir talepler için uygulamaları kullanmaya kümesini aşağıda listelenmiştir. Uygulamanız için isteğe bağlı özel talepler eklemek için bkz [dizin genişletmeleri](#configuring-directory-extension-optional-claims)aşağıdaki. Talepleri eklerken **erişim belirteci**, bu istenen erişim belirteçleri için geçerli *için* uygulama (bir web API), olanları *tarafından* uygulama. Bu, API'nizi erişen istemci ne olursa olsun, doğru verilere API'nizi karşı kimlik doğrulaması yapmak için kullandıkları erişim belirteci mevcut olmasını sağlar.

> [!NOTE]
> Bu talep çoğunu içinde Jwt'ler v1.0 ve v2.0 belirteçleri, ancak değil SAML belirteçlerini dışında belirteç Türü sütununda belirtilmedikçe dahil edilebilir. Tüketici hesapları "Kullanıcı türü" sütunu işaretlendiğinden, bu talepler kümesini destekler.  Birçok listelenen talep tüketici kullanıcılar için geçerli değildir (Kiracı, bu nedenle sahip `tenant_ctry` değeri yok).  

**Tablo 2: Talep kümesine V1.0 ve isteğe bağlı V2.0**

| Ad                       |  Açıklama   | Belirteç türü | Kullanıcı türü | Notlar  |
|----------------------------|----------------|------------|-----------|--------|
| `auth_time`                | Zaman zaman son kullanıcı kimlik doğrulaması. Bkz: Openıd Connect belirtimi.| JWT        |           |  |
| `tenant_region_scope`      | Kaynak Kiracı bölgesi | JWT        |           | |
| `home_oid`                 | Konuk kullanıcılar için kullanıcının giriş kiracısında kullanıcının nesne kimliği.| JWT        |           | |
| `sid`                      | Oturum başına kullanıcı oturumu kapatma için kullanılan oturum kimliği. | JWT        |  Kişisel ve Azure AD hesapları.   |         |
| `platf`                    | Cihaz platformu    | JWT        |           | Cihaz türü doğrulayabilirsiniz yönetilen cihazlar için kısıtlı.|
| `verified_primary_email`   | Kullanıcının PrimaryAuthoritativeEmail kaynağı      | JWT        |           |         |
| `verified_secondary_email` | Kullanıcının SecondaryAuthoritativeEmail kaynağı   | JWT        |           |        |
| `enfpolids`                | Uygulanan ilkeyi kimlikleri. Geçerli kullanıcı için değerlendirilen kimlikleri ilke listesi. | JWT |  |  |
| `vnet`                     | VNET belirleyici bilgi. | JWT        |           |      |
| `fwd`                      | IP adresi.| JWT    |   | İstekte bulunan istemciye (bir sanal ağ içindeki olduğunda) özgün IPv4 adresini ekler |
| `ctry`                     | Kullanıcının ülke | JWT |  | Azure AD döndürür `ctry` varsa ve FR, JP, SZ ve benzeri gibi bir standart iki harfli ülke kodu talep değeri isteğe bağlı bir talep. |
| `tenant_ctry`              | Kaynak kiracının ülke | JWT | | |
| `xms_pdl`          | Tercih edilen veri konumu   | JWT | | Çoklu coğrafi kiracılar için gösteren kullanıcının bulunduğu coğrafi bölge 3 harfli kod budur. Daha fazla bilgi için bkz. [tercih edilen veri konumu hakkında Azure AD Connect belgelerini](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-feature-preferreddatalocation).<br/>Örneğin: `APC` Asya Pasifik için. |
| `xms_pl`                   | Kullanıcı tercih edilen dil  | JWT ||Kullanıcının, uygulamanın tercih edilen dil, ayarlayın. Konuk erişimi senaryoları kendi giriş kiracısında kaynağı. Tümünü CC biçimlendirilmiş ("en-us"). |
| `xms_tpl`                  | Kiracı tercih edilen dil| JWT | | Kaynak Kiracı kişinin tercih ettiği dili, ayarlayın. Biçimlendirilmiş LL ("tr"). |
| `ztdid`                    | Sıfır dokunma dağıtım kimliği | JWT | | Cihaz kimliği için kullanılan [Windows AutoPilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-10-autopilot) |
| `email`                    | Adreslenebilir e-posta kullanıcı varsa, bu kullanıcı için.  | JWT, SAML | MSA, AAD | Kullanıcı kiracıda bir konuk ise bu değer varsayılan olarak dahil edilir.  Yönetilen kullanıcılar için (Bu Kiracı içindeki), bu isteğe bağlı bir talep aracılığıyla veya yalnızca v2.0 OpenID kapsamı ile istenmesi gerekir.  Yönetilen kullanıcılar için e-posta adresi olarak [Office Yönetim Portalı](https://portal.office.com/adminportal/home#/users).| 
| `groups`| İsteğe bağlı Grup talepleri için biçimlendirme |JWT, SAML| |GroupMembershipClaims ayarı ile birlikte kullanılan [uygulama bildirimini](reference-app-manifest.md), hangi ayarlanmalıdır de. Ayrıntılar için bkz. [grup talepleri](#Configuring-group-optional claims) aşağıda. Grup talepleri hakkında daha fazla bilgi için bkz [grup talepleri yapılandırma](../hybrid/how-to-connect-fed-group-claims.md)
| `acct`             | Kiracıdaki kullanıcıların hesap durumu. | JWT, SAML | | Kullanıcı, kiracısının üyesi ise, değer `0`. Bir konuk olmaları durumunda değerdir `1`. |
| `upn`                      | UserPrincipalName talep. | JWT, SAML  |           | Bu talep otomatik olarak dahil olsa da, Konuk kullanıcı durumda davranışını değiştirmek için ek özellikler eklemek için isteğe bağlı bir talep olarak belirtebilirsiniz.  |

### <a name="v20-optional-claims"></a>İsteğe bağlı taleplerin v2.0

Bu talepler her zaman v1.0 Azure AD belirteçleri dahil, ancak v2.0 belirteçlerinde istenmedikçe dahil değildir. Bu talepler yalnızca (kimlik ve erişim belirteçler) Jwt'ler için geçerlidir. 

**Tablo 3: Yalnızca v2.0 isteğe bağlı talepleri**

| JWT talep     | Ad                            | Açıklama                                | Notlar |
|---------------|---------------------------------|-------------|-------|
| `ipaddr`      | IP Adresi                      | Oturum açtığınız istemci IP adresi.   |       |
| `onprem_sid`  | Şirket içi güvenlik tanımlayıcısı |                                             |       |
| `pwd_exp`     | Parola süre sonu zamanı        | Parola süresinin dolma datetime. |       |
| `pwd_url`     | Parola URL'sini değiştirme             | Kullanıcının parolasını değiştirmek için ziyaret edebileceği bir URL.   |   |
| `in_corp`     | İç şirket ağı        | Sinyaller istemci ve şirket ağından oturum açılıyor. Değilseniz, talep dahil değildir.   |  Kapatarak tabanlı [güvenilen IP'ler](../authentication/howto-mfa-mfasettings.md#trusted-ips) MFA ayarları.    |
| `nickname`    | Takma adı                        | İlk veya son adından ayrı kullanıcı için ek bir ad. | 
| `family_name` | Soyadı                       | Son adını, soyadını veya kullanıcının aile adı kullanıcı nesnesinde tanımlanan sağlar. <br>"family_name": "Mert" | MSA ve AAD desteklenen   |
| `given_name`  | Ad                      | İlk sağlar veya "kullanıcı adını, kullanıcı nesnesindeki kümesi olarak verilen".<br>"given_name": "Ferdi"                   | MSA ve AAD desteklenen  |
| `upn`         | Kullanıcı Asıl Adı | Username_hint parametresiyle birlikte kullanılabilecek kullanıcı için bir tanımlayıcı.  Kullanıcı için kalıcı bir tanımlayıcı değil ve anahtar verileri kullanılmamalıdır. | Bkz: [ek özellikler](#additional-properties-of-optional-claims) aşağıda talep yapılandırma. |

### <a name="additional-properties-of-optional-claims"></a>İsteğe bağlı taleplerin ek özellikler

Bazı isteğe bağlı bir talep, talep döndürülen şeklini değiştirmek için yapılandırılabilir. Bu ek özellikler genellikle farklı veri beklentileri ile şirket içi uygulamaların taşınmasına yardımcı olmak için kullanılır (örneğin, `include_externally_authenticated_upn_without_hash` karma işareti işleyemiyor istemcilerle yardımcı olur (`#`) UPN içinde)

**Tablo 4: İsteğe bağlı bir talep yapılandırma değerleri**

| Özellik adı  | Ek özellik adı | Açıklama |
|----------------|--------------------------|-------------|
| `upn`          |                          | V1.0 ve v2.0 belirteçleri ve SAML hem JWT yanıtları için kullanılabilir. |
|                | `include_externally_authenticated_upn`  | Kaynak kiracıda depolanmış olarak UPN Konuk içerir. Örneğin, `foo_hometenant.com#EXT#@resourcetenant.com` |             
|                | `include_externally_authenticated_upn_without_hash` | Aynı yukarıdaki karma işaretler dışında (`#`) alt çizgi ile değiştirilir (`_`), örneğin `foo_hometenant.com_EXT_@resourcetenant.com` |

#### <a name="additional-properties-example"></a>Ek özellikleri örneği

```json
 "optionalClaims": 
   {
       "idToken": [ 
             { 
                "name": "upn", 
            "essential": false,
                "additionalProperties": [ "include_externally_authenticated_upn"]  
              }
        ]
}
```

Bu OptionalClaims nesne başka bir upn ile ek giriş kiracısında ve kaynak Kiracı bilgileri içerecek şekilde istemciye döndürülen kimlik belirteci neden olur. Bu, yalnızca değiştirecek `upn` (farklı bir IDP kimlik doğrulaması kullanan için) kiracıda bir Konuk kullanıcı, belirteçte talep. 

## <a name="configuring-optional-claims"></a>İsteğe bağlı bir talep yapılandırma

(Aşağıdaki örneğe bakın) uygulama bildirimini değiştirerek, uygulamanız için isteğe bağlı bir talep yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Azure AD uygulama bildirim makaleyi anlama](reference-app-manifest.md).

> [!IMPORTANT]
> Erişim belirteçleri **her zaman** kaynak, istemci bildirimi kullanarak oluşturulur.  İstekteki bunu `...scope=https://graph.microsoft.com/user.read...` kaynak grafiğidir.  Bu nedenle, erişim belirtecinin değil istemci bildirimi grafiğin bildiri kullanarak oluşturulur.  Uygulamanız için bildirim değiştirme hiçbir zaman belirteçleri grafik farklı görünmesine neden olur.  Doğrulamak için `accessToken` değişiklikler etkili, uygulama, olmayan başka bir uygulama için bir belirteç isteyin.  

**Örnek şeması:**

```json
"optionalClaims":  
   {
      "idToken": [
            {
                  "name": "auth_time", 
                  "essential": false
             }
      ],
      "accessToken": [
             {
                    "name": "ipaddr", 
                    "essential": false
              }
      ],
      "saml2Token": [
              {
                    "name": "upn", 
                    "essential": false
               },
               {
                    "name": "extension_ab603c56068041afb2f6832e2a17e237_skypeId",
                    "source": "user", 
                    "essential": false
               }
       ]
   }
```

### <a name="optionalclaims-type"></a>OptionalClaims türü

Bir uygulama tarafından istenen isteğe bağlı talepleri bildirir. Bir uygulama güvenlik belirteci Hizmeti'nden alabileceği her üç tür belirteçleri (kimlik, erişim belirteci, SAML 2 token) döndürülecek isteğe bağlı taleplerin yapılandırabilirsiniz. Uygulamayı farklı bir belirteç türlerinin döndürülecek isteğe bağlı bir talep kümesini yapılandırabilirsiniz. Uygulama varlığın OptionalClaims özelliği OptionalClaims bir nesnedir.

**Tablo 5: OptionalClaims türü özellikleri**

| Ad        | Tür                       | Açıklama                                           |
|-------------|----------------------------|-------------------------------------------------------|
| `idToken`     | Koleksiyon (OptionalClaim) | İsteğe bağlı talep kimliği JWT belirteci döndürdü. |
| `accessToken` | Koleksiyon (OptionalClaim) | JWT erişim belirtecinde verilen talepleri isteğe bağlı. |
| `saml2Token`  | Koleksiyon (OptionalClaim) | SAML belirtecinde verilen talepleri isteğe bağlı.   |

### <a name="optionalclaim-type"></a>OptionalClaim türü

Bir uygulama veya hizmet sorumlusu ile ilişkili isteğe bağlı bir talep içerir. Idtoken accessToken ve saml2Token özelliklerini [OptionalClaims](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#optionalclaims-type) türü OptionalClaim koleksiyonudur.
Belirli bir talep tarafından destekleniyorsa, ayrıca AdditionalProperties alanını kullanarak OptionalClaim davranışını değiştirebilirsiniz.

**Tablo 6: OptionalClaim türü özellikleri**

| Ad                 | Tür                    | Açıklama                                                                                                                                                                                                                                                                                                   |
|----------------------|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `name`                 | Edm.String              | İsteğe bağlı bir talep adı.                                                                                                                                                                                                                                                                           |
| `source`               | Edm.String              | Kaynağı (dizin nesnesi) talep. Önceden tanımlanmış talep ve kullanıcı tanımlı talep uzantı özelliklerinden vardır. Kaynak değeri null ise, talep önceden tanımlanmış isteğe bağlı bir talep var. Kullanıcı kaynak değeri ise name özelliği kullanıcı nesnesi uzantı özelliği değer. |
| `essential`            | Edm.Boolean             | Değer true ise, istemci tarafından belirtilen talep son kullanıcı tarafından istenen belirli görev için bir yumuşak yetkilendirme deneyimi sağlamak gereklidir. Varsayılan değer false'tur.                                                                                                             |
| `additionalProperties` | Koleksiyon (Edm.String) | Talep ek özellikleri. Bu koleksiyonda bir özellik varsa, belirtilen ad özelliği isteğe bağlı bir talep davranışını değiştirir.                                                                                                                                               |
## <a name="configuring-directory-extension-optional-claims"></a>Dizin uzantısı isteğe bağlı talep yapılandırma

Standart isteğe bağlı talep kümesine ek olarak, belirteçleri directory şema uzantıları içerecek şekilde yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Directory şema uzantıları](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions). Bu özellik, uygulamanızı – örneğin kullanabileceğiniz ek kullanıcı bilgileri, bir ek tanımlayıcı veya kullanıcı önemli yapılandırma seçeneğini eklemek için yararlıdır. 

> [!Note]
> Uygulamanızı bildirim istekleri, özel uzantı ve bir MSA kullanıcısı oturum uygulamanıza directory şema uzantıları yalnızca AAD özelliği olduğundan, bu uzantıları döndürülmez.

### <a name="directory-extension-formatting"></a>Biçimlendirme dizini uzantısı

Uzantı öznitelikleri için uzantı tam adını kullanın (biçimde: `extension_<appid>_<attributename>`) uygulama bildiriminde. `<appid>` Talep isteyen uygulama kimliği ile eşleşmelidir. 

JWT içinde bu talepler aşağıdaki ad biçimiyle yayılan: `extn.<attributename>`.

SAML belirteçlerini içinde bu talepler aşağıdaki URI biçimi ile yayılan: `http://schemas.microsoft.com/identity/claims/extn.<attributename>`

## <a name="configuring-group-optional-claims"></a>Grup isteğe bağlı talepleri yapılandırma

   > [!NOTE]
   > Kullanıcılar ve şirket içinden eşitlenen gruplar için Grup adları yaymak için genel Önizleme özelliğidir

Bu bölümde şirket içi Windows Active Directory'den eşitlenen öznitelikler için varsayılan grup objectID gelen grup talepleri kullanılan grup öznitelikleri değiştirmek için isteğe bağlı bir talep altındaki yapılandırma seçenekleri ele alınmaktadır.
> [!IMPORTANT]
> Bkz: [Azure Active Directory ile uygulamaları için Grup talepleri yapılandırma](../hybrid/how-to-connect-fed-group-claims.md) önemli uyarılar genel Önizleme aşamasında şirket içi öznitelik grubu talepler dahil olmak üzere daha fazla ayrıntı için.

1. Portalda Azure Active Directory -> -> Uygulama kayıtları seçin -> uygulama bildirimi ->

2. Grup üyeliğini talep groupMembershipClaim değiştirerek etkinleştir

   Geçerli değerler şunlardır:

   - "Tüm"
   - "IDAP"
   - "DistributionList"
   - "DirectoryRole"

   Örneğin:

   ```json
   "groupMembershipClaims": "SecurityGroup"
   ```

   Grup içinde grup ObjectIDs derleyicisindeki varsayılan talep değeri.  Şirket içi grup öznitelikleri içeren veya rol talep türünü değiştirmek için talep değerini değiştirmek için OptionalClaims yapılandırma şu şekilde kullanın:

3. Grup adı yapılandırması isteğe bağlı talepleri ayarlayın.

   Belirteç grupları şirket içi AD grup öznitelikleri isteğe bağlı bir talep bölümünde hangi belirteç türü isteğe bağlı bir talep uygulanması gerektiğini belirtmek, istenen isteğe bağlı bir talep ve istenen herhangi bir ek özellik adını içerecek şekilde isterseniz.  Birden çok belirteç türleri listelenir:

   - OIDC kimlik belirteci için ilgili Idtoken
   - OAuth/OIDC erişim belirteci için accessToken
   - SAML belirteçleri Saml2Token.

   > [!NOTE]
   > Biçim belirteçleri Saml2Token türü SAML1.1 hem SAML2.0 için geçerlidir

   İlgili her belirteç türü için bildirim OptionalClaims bölümde kullanmak üzere gruplar talep değiştirin. OptionalClaims Şeması aşağıdaki gibidir:

   ```json
   {
   "name": "groups",
   "source": null,
   "essential": false,
   "additionalProperties": []
   }
   ```

   | İsteğe bağlı bir talep şeması | Değer |
   |----------|-------------|
   | **Adı:** | "Grupları" olmalıdır |
   | **Kaynak:** | Kullanılmıyor. Atlayın veya null |
   | **gerekli:** | Kullanılmıyor. Atlarsanız veya false değerini belirtin |
   | **additionalProperties:** | Ek özellikler listesi.  Geçerli seçenekler: "sam_account_name", "dns_domain_and_sam_account_name", "netbios_domain_and_sam_account_name", "emit_as_roles" |

   AdditionalProperties içinde yalnızca bir "sam_account_name", "dns_domain_and_sam_account_name", "netbios_domain_and_sam_account_name" gereklidir.  Birden fazla varsa, ilk kullanılır ve diğerleri yoksayılır.

   Bazı uygulamalar, rol talep kullanıcı grup bilgilerini gerektirir.  Bir gruptan bir rol talep için talep için talep türünü değiştirmek için "emit_as_roles" için ek özellikleri ekleyin.  Grubu değerlerini, rol talebi yayılan.

   > [!NOTE]
   > "Emit_as_roles" kullanılırsa, kullanıcının atandığı herhangi bir uygulama rolü yapılandırılmış rol talebi görünmüyor

**Örnekler:** Grup adları dnsDomainName\sAMAccountName biçimde OAuth erişim belirteçleri olarak grupları yayma

```json
"optionalClaims": {
    "accessToken": [{
        "name": "groups",
        "additionalProperties": ["dns_domain_and_sam_account_name"]
    }]
}
 ```

SAML ve OIDC kimlik belirteçlerini rol talebi olarak netbiosDomain\sAMAccountName biçiminde döndürülecek grubu adları göstermek için:

```json
"optionalClaims": {
    "saml2Token": [{
        "name": "groups",
        "additionalProperties": ["netbios_name_and_sam_account_name", "emit_as_roles"]
    }],

    "idToken": [{
        "name": "groups",
        "additionalProperties": ["netbios_name_and_sam_account_name", "emit_as_roles"]
    }]
 }

 ```

## <a name="optional-claims-example"></a>İsteğe bağlı bir talep örneği

Bu bölümde, isteğe bağlı bir talep özelliği, uygulamanız için nasıl kullanabileceğinizi görmek için bir senaryo size yol.
Özellikleri etkinleştirmek ve isteğe bağlı talepleri yapılandırmak için uygulamanın kimlik yapılandırmasını güncelleştirmek için kullanılabilir birden çok seçenek vardır:
-   Uygulama bildirimini değiştirebilirsiniz. Aşağıdaki örnekte, yapılandırma adımları için bu yöntemi kullanır. Okuma [Azure AD uygulama bildirimi belge anlama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest) ilk bildirimi için giriş.
-   Kullanan bir uygulamayı yazmak mümkündür [Graph API'si](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api) uygulamanızı güncelleştirmek için. [Varlık ve karmaşık tür başvurusu](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#optionalclaims-type) Graph API Başvurusu Kılavuzu, isteğe bağlı talepleri yapılandırmaya yardımcı olabilir.

**Örnek:** Aşağıdaki örnekte, erişim kimliği ve SAML talepleri eklemek için bir uygulamanın bildirim değiştirecek uygulama için amaçlanan belirteçleri.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Kimlik doğrulaması yaptınız sonra sayfanın sağ üst köşesinde seçerek Azure AD kiracınızı seçin.
1. Seçin **uygulama kayıtları** sol taraftaki.
1. İsteğe bağlı talepler için listeden yapılandırın ve üzerine tıklayarak istediğiniz uygulamayı bulun.
1. Uygulama sayfasında **bildirim** satır içi bildirim düzenleyicisini açın. 
1. Bu düzenleyici kullanılarak bildirimde doğrudan düzenleyebilirsiniz. Bildirim için şema izleyen [uygulama varlığı](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest)ve otomatik biçimleri bildirim kez kaydedildi. Yeni öğeleri eklenecek `OptionalClaims` özelliği.

    ```json
      "optionalClaims": 
      {
            "idToken": [ 
                  { 
                        "name": "upn", 
                        "essential": false, 
                        "additionalProperties": [ "include_externally_authenticated_upn"]  
                  }
            ],
            "accessToken": [ 
                  {
                        "name": "auth_time", 
                        "essential": false
                  }
            ],
            "saml2Token": [ 
                  { 
                        "name": "extension_ab603c56068041afb2f6832e2a17e237_skypeId",
                        "source": "user", 
                        "essential": true
                  }
            ]
      }

    ```

    Bu durumda, isteğe bağlı farklı talepler her uygulama alabilir belirteci türüne eklendi. Kimlik belirteçlerini artık tam formunda Federasyon kullanıcıları için UPN içerecektir (`<upn>_<homedomain>#EXT#@<resourcedomain>`). Bu uygulama için diğer istemcilerin istek erişim belirteçleri artık auth_time talebi içerir. SAML belirteçlerini skypeId directory şema uzantısı şimdi içerecektir (Bu örnekte, bu uygulama için uygulama kimliği ab603c56068041afb2f6832e2a17e237 olduğu). SAML belirteçlerini Skype kimliği olarak açığa çıkarır `extension_skypeId`.

1. Aktualizuje SE manifest tamamladığınızda, tıklayın **Kaydet** bildirimi kaydetmek için

## <a name="next-steps"></a>Sonraki adımlar

Azure AD tarafından sağlanan standart talepler hakkında daha fazla bilgi edinin.

- [Kimliği belirteçleri](id-tokens.md)
- [Erişim belirteçleri](access-tokens.md)
