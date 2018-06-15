---
title: Azure AD uygulaması isteğe bağlı talepler sağlamak öğrenin | Microsoft Docs
description: Azure Active Directory tarafından yayınlanan SAML 2.0 ve JSON Web belirteçleri (JWT) belirteçleri için özel veya ek talep eklemek için bir kılavuzdur.
documentationcenter: na
author: CelesteDG
services: active-directory
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/24/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: c4670a7e957970acea54ff69d56edcd45092c8fe
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34157240"
---
# <a name="optional-claims-in-azure-ad-preview"></a>İsteğe bağlı Taleplerde Azure AD (Önizleme)

Bu özellik, kullanıcıların uygulamaya gönderilen belirteçlerinde istedikleri talepleri belirtmek için uygulama geliştiriciler tarafından kullanılır. İsteğe bağlı talepleri kullanabilirsiniz:
-   Uygulamanız için belirteçleri dahil etmek için ek talep seçin.
-   Azure AD belirteçleri verir belirli talepler davranışını değiştirin.
-   Ekleme ve özel talepler, uygulamanız için erişim. 

> [!Note]
> Bu özellik şu anda genel önizlemede değil. Geri veya herhangi bir değişiklik kaldırmak hazırlıklı olun. Genel Önizleme sırasında herhangi bir Azure AD Abonelik özelliği kullanılabilir. Ancak, özelliği genel kullanıma sunulduğunda özelliği bazı yönlerini bir Azure AD premium aboneliği gerektirebilir.

Standart talep ve belirteçleri nasıl kullanıldıkları listesi için bkz: [Azure AD tarafından yayınlanan belirteçleri Temelleri](active-directory-token-and-claims.md). 

Hedeflerinden biri [Azure AD v2.0 uç](active-directory-appmodel-v2-overview.md) istemciler tarafından en iyi performansı elde etmek için daha küçük belirteci boyutları.  Sonuç olarak, önceden erişim ve kimlik belirteçlerini dahil birkaç talep artık v2.0 belirteçleri varsa ve için özellikle uygulama başına temelinde sorulan gerekir.  

**Tablo 1: Uygulanabilirlik**

| Hesap türü | V1.0 uç noktası                      | V2.0 uç noktası  |
|--------------|------------------------------------|----------------|
| MSA          | Yok - RPS biletleri yerine kullanılır | Gelen desteği |
| AAD          | Desteklenen                          | Desteklenen      |

## <a name="standard-optional-claims-set"></a>Standart isteğe bağlı talep kümesi
İsteğe bağlı varsayılan olarak kullanılabilir talep kullanan uygulamalar için kümesi altında listelenir.  Uygulamanız için özel isteğe bağlı talepleri eklemek için bkz: [dizin uzantıları](active-directory-optional-claims.md#Configuring-custom-claims-via-directory-extensions), aşağıdaki. 

> [!Note]
>Bu talep çoğunluğu Jwt'ler, ancak değil SAML belirteçleri dışında belirteç Türü sütununda belirtilmedikçe dahil edilebilir.  Ayrıca, isteğe bağlı talepleri yalnızca AAD kullanıcılar için şu anda desteklenen olsa da, MSA destek ekleniyor.  MSA v2.0 uç desteği isteğe bağlı talep olduğunda, kullanıcı türü sütun bir talep AAD veya MSA bir kullanıcı için kullanılabilir olup olmadığını gösterir.  

**Tablo 2: Standart isteğe bağlı talep kümesi**

| Ad                     | Açıklama                                                                                                                                                                                     | Belirteç türü | Kullanıcı Türü | Notlar                                                                                                                                                                                                                                                                                   |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `auth_time`                | Zaman zaman son kullanıcı için kimlik doğrulaması.  Bkz: Openıd Connect belirtimi.                                                                                                                                | JWT        |           |                                                                                                                                                                                                                                                                                         |
| `tenant_region_scope`      | Kaynak Kiracı bölgesi                                                                                                                                                                   | JWT        |           |                                                                                                                                                                                                                                                                                         |
| `signin_state`             | Oturum durumu talep                                                                                                                                                                             | JWT        |           | dönüş değerleri bayrak olarak 6:<br> "dvc_mngd": cihaz yönetilir<br> "dvc_cmp": cihaz uyumlu<br> "dvc_dmjd": cihaz, etki alanına katılmış<br> "dvc_mngd_app": cihaz MDM ile yönetilen<br> "inknownntwk": içinde bilinen bir ağda aygıttır.<br> "kmsi": Koru bana imzalı içinde kullanıldı. <br> |
| `controls`                 | Birden çok değerli talep koşullu erişim ilkeleri tarafından zorlanan oturum denetimleri içeren.                                                                                                       | JWT        |           | 3 değerler:<br> "app_res": uygulamanın daha ayrıntılı sınırlamalar zorlamak için gerekir. <br> "ca_enf": koşullu erişim zorlama ertelendi ve yine de gereklidir. <br> "no_cookie": Bu belirteç exchange tarayıcıda tanımlama bilgisi için yeterli değil. <br>                              |
| `home_oid`                 | Konuk kullanıcılar için kullanıcının ev Kiracı Kullanıcı nesne kimliği.                                                                                                                           | JWT        |           |                                                                                                                                                                                                                                                                                         |
| `sid`                      | Oturum başına kullanıcı signout için kullanılan oturum kimliği.                                                                                                                                                  | JWT        |           |                                                                                                                                                                                                                                                                                         |
| `platf`                    | Cihaz platformu                                                                                                                                                                                 | JWT        |           | Aygıt türü doğrulayabilirsiniz yönetilen cihazlar için kısıtlı.                                                                                                                                                                                                                              |
| `verified_primary_email`   | Kullanıcının PrimaryAuthoritativeEmail kaynaklanan                                                                                                                                               | JWT        |           |                                                                                                                                                                                                                                                                                         |
| `verified_secondary_email` | Kullanıcının SecondaryAuthoritativeEmail kaynaklanan                                                                                                                                             | JWT        |           |                                                                                                                                                                                                                                                                                         |
| `enfpolids`                | Uygulanan ilkeyi kimlikleri. Geçerli kullanıcı için değerlendirilen kimlikleri ilke listesi.                                                                                                         | JWT        |           |                                                                                                                                                                                                                                                                                         |
| `vnet`                     | VNET belirticisi bilgileri.                                                                                                                                                                     | JWT        |           |                                                                                                                                                                                                                                                                                         |
| `fwd`                      | IP adresi.  İstekte bulunan istemci (içinde olduğunda VNET) özgün IPv4 adresini ekler                                                                                                       | JWT        |           |                                                                                                                                                                                                                                                                                         |
| `ctry`                     | Kullanıcının ülke                                                                                                                                                                                  | JWT        |           |                                                                                                                                                                                                                                                                                         |
| `tenant_ctry`              | Kaynak kiracının ülke                                                                                                                                                                       | JWT        |           |                                                                                                                                                                                                                                                                                         |
| `acct`    | Kiracı kullanıcılar hesap durumu.  Kullanıcı Kiracı üyesi ise, değer `0`.  Bir konuk olmaları durumunda değerdir `1`.  | JWT, SAML | | |
| `upn`                      | UserPrincipalName talep.  Bu talep otomatik olarak dahil olsa da, Konuk kullanıcı durumda davranışını değiştirmek için ek özellikler eklemek için isteğe bağlı bir talep olarak belirtebilirsiniz. | JWT, SAML  |           | Ek özellikleri: <br> `include_externally_authenticated_upn` <br> `include_externally_authenticated_upn_without_hash`                                                                                                                                                                 |

### <a name="v20-optional-claims"></a>V2.0 isteğe bağlı talepleri
Bu talepler her zaman v1.0 belirteçleri dahil, ancak v2.0 belirteçlerinden istenen sürece kaldırılır.  Bu talep yalnızca (kimliği belirteçleri ve erişim belirteçleri) Jwt'ler için geçerlidir.  

**Tablo 3: Yalnızca V2.0 isteğe bağlı talepleri**

| JWT talep     | Ad                            | Açıklama                                                                                                                    | Notlar |
|---------------|---------------------------------|--------------------------------------------------------------------------------------------------------------------------------|-------|
| `ipaddr`      | IP Adresi                      | Oturumu istemci IP adresi.                                                                                      |       |
| `onprem_sid`  | Şirket içi güvenlik tanımlayıcısı |                                                                                                                                |       |
| `pwd_exp`     | Parola son kullanma tarihi        | Parola süresinin dolma datetime.                                                                                    |       |
| `pwd_url`     | Parola URL'yi Değiştir             | Kullanıcı parolasını değiştirmek için ziyaret edebileceği bir URL.                                                                        |       |
| `in_corp`     | İç şirket ağı        | İstemcinin kurumsal ağ üzerinden oturum açmayı olmadığını bildirir. Değilse, talep dahil değildir                     |       |
| `nickname`    | Takma ad                        | Kullanıcı için ek bir ad veya Soyadı adından ayırın.                                                             |       |                                                                                                                |       |
| `family_name` | Soyadı                       | Son adını, soyadını veya kullanıcının aile adını Azure AD kullanıcı nesnesinde tanımlanan sağlar. <br>"family_name": "Mert" |       |
| `given_name`  | Ad                      | İlk sağlar ya da "Azure AD kullanıcı nesnesindeki belirlenen kullanıcı adı verilen".<br>"given_name": "Ferdi"                   |       |

### <a name="additional-properties-of-optional-claims"></a>İsteğe bağlı talepler ek özellikler

Bazı isteğe bağlı bir talep, talep döndürülen şeklini değiştirmek için yapılandırılabilir.  Bu ek özellikler çoğunlukla farklı veri beklentilerini ile şirket içi uygulamalar geçişini yardımcı olmak için kullanılır (örneğin, `include_externally_authenticated_upn_without_hash` hashmarks işleyemiyor istemcileriyle yardımcı olur (`#`) UPN içinde)

**Tablo 4: standart isteğe bağlı talep yapılandırma değerleri**

| Özellik adı                                     | Ek özellik adı                                                                                                             | Açıklama |
|---------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|-------------|
| `upn`                                                 |                                                                                                                                      |             |
| | `include_externally_authenticated_upn`              | Kaynak Kiracı depolandığı şekliyle UPN Konuk içerir.  Örneğin, `foo_hometenant.com#EXT#@resourcetenant.com`                            |             
| | `include_externally_authenticated_upn_without_hash` | Yukarıdaki aynı dışındaki hashmarks (`#`) alt çizgi ile değiştirilir (`_`), örneğin `foo_hometenant.com_EXT_@resourcetenant.com` |             

> [!Note]
>Bir ek özellik olmadan upn isteğe bağlı talebi belirtecinde verilen yeni bir talep görmek için herhangi bir davranış – değiştirmez belirtme, en az bir ek özellikleri eklenmesi gerekir. 


#### <a name="additional-properties-example"></a>Ek özellikleri örneği:

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

Bu OptionalClaims nesne başka bir upn ek giriş Kiracı ve kaynak Kiracı bilgileri içerecek şekilde istemciye döndürülen kimliği belirteci neden olur.  

## <a name="configuring-optional-claims"></a>İsteğe bağlı talep yapılandırma

Uygulama bildirimini (aşağıdaki örneğe bakın) değiştirerek, uygulamanız için isteğe bağlı talep yapılandırabilirsiniz. Daha fazla bilgi için bkz: [Azure AD uygulama bildirim makaleyi anlama](active-directory-application-manifest.md).

**Örnek şeması:**

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
                    "name": "upn", 
                    "essential": true
               },
               { 
                    "name": "extension_ab603c56068041afb2f6832e2a17e237_skypeId",
                    "source": "user", 
                    "essential": true
               }
       ]
   }
```

### <a name="optionalclaims-type"></a>OptionalClaims türü

Bir uygulama tarafından istenen isteğe bağlı talep bildirir. Bir uygulama, isteğe bağlı talep türlerinin her biri üç belirteçleri (kimlik, erişim belirteci, SAML 2 token) içinde döndürülecek güvenlik belirteci Hizmeti'nden alabilir yapılandırabilirsiniz. Uygulama her belirteç türünün döndürülecek isteğe bağlı talep farklı bir kümesini yapılandırabilirsiniz. Uygulama varlığı OptionalClaims özelliğinin OptionalClaims nesne değildir.

**Tablo 5: OptionalClaims türü özellikleri**

| Ad        | Tür                       | Açıklama                                           |
|-------------|----------------------------|-------------------------------------------------------|
| `idToken`     | Koleksiyon (OptionalClaim) | İsteğe bağlı talepler JWT kimliği belirteç döndürdü.     |
| `accessToken` | Koleksiyon (OptionalClaim) | İsteğe bağlı talepleri JWT erişim belirteç döndürdü. |
| `saml2Token`  | Koleksiyon (OptionalClaim) | İsteğe bağlı talepleri SAML belirteç döndürdü.       |


### <a name="optionalclaim-type"></a>OptionalClaim türü

Bir uygulama veya bir hizmet sorumlusu ilişkili isteğe bağlı bir talep içerir. İdToken, accessToken ve saml2Token özelliklerini [OptionalClaims](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#optionalclaims-type) türü OptionalClaim koleksiyonudur.
Belirli bir talep tarafından destekleniyorsa, ayrıca AdditionalProperties alanını kullanarak OptionalClaim davranışını değiştirebilirsiniz.

**Tablo 6: OptionalClaim türü özellikleri**

| Ad                 | Tür                    | Açıklama                                                                                                                                                                                                                                                                                                   |
|----------------------|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `name`                 | Edm.String              | İsteğe bağlı talep adı.                                                                                                                                                                                                                                                                               |
| `source`               | Edm.String              | Kaynağı (dizin nesnesi) talep. Önceden tanımlanmış talepleri ve kullanıcı tanımlı talepleri uzantı özelliklerinden vardır. Kaynak değerin null ise talep önceden tanımlanmış bir isteğe bağlı talep olur. Kaynak değerin kullanıcı ise, name özelliği değerinde kullanıcı nesnesinden uzantısı özelliğidir. |
| `essential`            | Edm.Boolean             | Değer doğru ise, istemci tarafından belirtilen talep son kullanıcı tarafından istenen belirli görev için bir yumuşak yetkilendirme deneyimi sağlamak gereklidir. Varsayılan değer false.                                                                                                                 |
| `additionalProperties` | Koleksiyon (Edm.String) | Ek özellikler talep. Bu koleksiyonda bir özellik varsa, belirtilen ad özelliği isteğe bağlı talep davranışını değiştirir.                                                                                                                                                   |

## <a name="configuring-custom-claims-via-directory-extensions"></a>Dizin uzantıları aracılığıyla özel talep yapılandırma

Standart isteğe bağlı talep kümesi yanı sıra belirteçleri directory şema uzantıları içerecek şekilde de yapılandırılabilir (bkz [Directory şema uzantıları makale](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) daha fazla bilgi için).  Bu özellik, uygulamanızı – örneğin kullanabileceğiniz ek kullanıcı bilgileri, bir ek tanımlayıcısı veya kullanıcı önemli yapılandırma seçeneği eklemek için yararlıdır. 

> [!Note]
> İstekler, uygulama bildirimi özel bir uzantının ve MSA kullanıcı günlüğe kaydeder. directory şema uzantıları uygulamanıza bir salt AAD özelliği olduğundan, bu uzantıları döndürülmez. 

### <a name="values-for-configuring-additional-optional-claims"></a>İsteğe bağlı ek talep yapılandırma değerleri 

Tam adı uzantısı, uzantı öznitelikleri için kullanın (biçimde: `extension_<appid>_<attributename>`) uygulama bildiriminde. `<appid>` Talep isteyen uygulama kimliği eşleşmelidir. 

JWT içinde bu talepler aşağıdaki adı biçimi ile yayılan: `extn.<attributename>`.

SAML belirteçleri içinde bu talepler aşağıdaki URI biçimi ile yayılan: `http://schemas.microsoft.com/identity/claims/extn.<attributename>`

## <a name="optional-claims-example"></a>İsteğe bağlı talep örneği

Bu bölümde, uygulamanız için isteğe bağlı talep özelliği nasıl kullanabileceğinizi görmek için bir senaryo size yol.
Özellikleri etkinleştirmek ve isteğe bağlı talepleri yapılandırmak için bir uygulamanın kimlik yapılandırmasını güncelleştirmek için kullanılabilir birden çok seçenek vardır:
-   Uygulama bildirimini değiştirebilirsiniz. Aşağıdaki örnek, yapılandırmayı gerçekleştirmek için bu yöntemi kullanır. Okuma [Azure AD uygulama bildirim belgesi anlama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest) ilk giriş bildirim için.
-   Kullanan bir uygulama yazmak mümkündür [grafik API'si](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api) uygulamanızı güncelleştirmek için. [Varlık ve karmaşık türü başvurusu](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#optionalclaims-type) grafik API'si Başvurusu'nda Kılavuzu, isteğe bağlı talep yapılandırma ile yardımcı olabilir.

**Örnek:** aşağıdaki örnekte, erişim, kimlik ve SAML talepleri eklemek için bir uygulama bildirimi değiştirecek uygulama için amaçlanan belirteçleri.
1.  [Azure Portal](https://portal.azure.com) oturum açın.
2.  Kimlik doğruladınız sonra sayfanın sağ üst köşesinden seçerek Azure AD kiracınıza seçin.
3.  Seçin **Azure AD uzantısı** Masası'ndan sol gezinti çubuğunda **uygulama kayıtlar**.
4.  İsteğe bağlı talepleri için listede yapılandırmak ve tıklayın istediğiniz uygulamayı bulun.
5.  Uygulama sayfasından tıklatın **bildirim** satır içi bildirim Düzenleyicisi'ni açın. 
6.  Bu düzenleyiciyi kullanarak bildirim doğrudan düzenleyebilirsiniz. Bildirim için bir şemayı izlediğinden [uygulama varlığı](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity)ve otomatik biçimleri bildirim kez kaydedildi. Yeni elemets eklenir `OptionalClaims` özelliği.

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
      Bu durumda, farklı isteğe bağlı talepler her uygulama alabilir belirteç türü için eklendi. Kimlik belirteçlerini şimdi tam formunda Federasyon kullanıcıları için UPN içerir (`<upn>_<homedomain>#EXT#@<resourcedomain>`). Erişim belirteçleri şimdi auth_time talep alır. SAML belirteçleri şimdi skypeId directory şema uzantısını içerir (Bu örnekte, bu uygulama için uygulama kimliği ab603c56068041afb2f6832e2a17e237 değil).  SAML belirteçleri Skype kimliği olarak açığa çıkarır `extension_skypeId`.

7.  Bildirim güncelleştirme bittiğinde, tıklatın **kaydetmek** bildirimi kaydetmek için


## <a name="related-content"></a>İlgili içerik
* Daha fazla bilgi edinmek [standart talep](active-directory-token-and-claims.md) Azure AD tarafından sağlanan. 
