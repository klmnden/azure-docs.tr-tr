---
title: Uygulamaları Azure Active Directory'de SCIM'yi kullanma sağlanmasını otomatikleştirmek | Microsoft Docs
description: Azure Active Directory Kullanıcıları ve grupları SCIM Protokolü belirtiminde tanımlanan arabirimi ile bir web hizmeti tarafından fronted uygulama veya kimlik deposuna otomatik olarak sağlayabilirsiniz
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 4/03/2019
ms.author: celested
ms.reviewer: asmalser
ms.custom: aaddev;it-pro;seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: a404b5e6769c7bb91b4f7b5830cea18372ec456d
ms.sourcegitcommit: 045406e0aa1beb7537c12c0ea1fbf736062708e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "59007146"
---
# <a name="using-system-for-cross-domain-identity-management-scim-to-automatically-provision-users-and-groups-from-azure-active-directory-to-applications"></a>Kullanıcılar ve grupların Azure Active Directory'den uygulamalara otomatik olarak sağlamak için sistem etki alanları arası Kimlik Yönetimi (SCIM) kullanma

## <a name="overview"></a>Genel Bakış

SCIM standartlaştırılmış protokolü ve kimlik sistemleri arasında nasıl yönetildiğini sürücü büyük tutarlılık amaçlayan şema ' dir. Bir uygulama için kullanıcı yönetimi SCIM uç nokta desteklediğinde, Azure AD kullanıcı sağlama hizmeti oluşturmak, değiştirmek veya atanan kullanıcılar ve gruplar için bu endpoint delete isteklerini gönderebilirsiniz. 

Uygulamaları Azure AD'ye destekleyen birçok [otomatik kullanıcı hazırlama, önceden tümleştirilmiş](../saas-apps/tutorial-list.md) SCIM kullanıcı almak için gereken araçları değişiklik bildirimleri gibi uygulayın.  Bunlara ek olarak, müşterilerin belirli profilini destekleyen uygulamalar bağlanabilir [SCIM 2.0 protokolü belirtimi](https://tools.ietf.org/html/rfc7644) Azure portalında genel "galeri dışı" tümleştirme seçeneğini kullanma. 

Bu makalenin odaklandığı SCIM 2.0, Azure AD galeri dışı uygulamalar için kendi genel SCIM Bağlayıcısı bir parçası olarak uygulayan profilini açıktır. Ancak, başarılı bir uygulamayı test etme SCIM'yi destekleyen genel Azure AD ile bir adım olarak kullanıcı sağlamayı destekleyen Azure AD galeri listelenen bir uygulamayı edinmenin Bağlayıcıdır. Uygulamanızın Azure AD uygulama galerisinde listelenmesini daha fazla bilgi için bkz: [Microsoft uygulama ağı](https://microsoft.sharepoint.com/teams/apponboarding/Apps/SitePages/Default.aspx).
 

>[!IMPORTANT]
>Azure AD SCIM uygulama davranışını son 18 Aralık 2018'de güncelleştirildi. Değişiklikler hakkında daha fazla bilgi için bkz: [SCIM 2.0 protokolü uyumluluk Azure AD kullanıcı sağlama hizmetinin](application-provisioning-config-problem-scim-compatibility.md).

![][0]
*Şekil 1: SCIM uygulayan bir uygulama veya kimlik deposu için Azure Active Directory'den sağlama*

Bu makalede, dört bölümlere ayrılır:

* **[Kullanıcılar ve gruplar 2.0 SCIM'yi destekleyen üçüncü taraf uygulamaları için sağlama](#provisioning-users-and-groups-to-applications-that-support-scim)**  - kuruluşunuz SCIM 2.0, Azure AD profilini destekleyen uygular, hem de otomatik hale getirme başlatabildiğinizi bir üçüncü taraf uygulama kullanıyorsa Hazırlama ve kullanıcıları ve grupları bugün sağlamayı.

* **[Azure AD SCIM uygulama anlama](#understanding-the-azure-ad-scim-implementation)**  -2.0 SCIM kullanıcı yönetim API'sini destekleyen bir uygulama oluşturuyorsanız, bu bölümde ayrıntılı olarak Azure AD SCIM istemci nasıl uygulandığını ve nasıl modellemelidir açıklanmaktadır SCIM protokolünüzü istek ve yanıtları işleme.
  
* **[Microsoft CLI kitaplıklar kullanılarak bir SCIM uç noktası oluşturmaya](#building-a-scim-endpoint-using-microsoft-cli-libraries)**  -kod örnekleri ile birlikte ortak dil altyapısı (CLI) kitaplıkları size SCIM uç nokta geliştirip SCIM iletileri Çevir konusunda gösterir.  

* **[Kullanıcı ve Grup şema başvurusu](#user-and-group-schema-reference)**  -galeri dışı uygulamalar için Azure AD SCIM uygulaması tarafından desteklenen kullanıcı ve Grup şemasını açıklar. 

## <a name="provisioning-users-and-groups-to-applications-that-support-scim"></a>Kullanıcılar ve gruplar SCIM'yi destekleyen uygulamalar için hazırlama
Azure AD, otomatik olarak sağlama atanmış kullanıcılara ve gruplara belirli profilini kullanan uygulamalar yapılandırılabilir [SCIM 2.0 protokolünü](https://tools.ietf.org/html/rfc7644). Profil ayrıntılarını bölümünde belgelendirilen [Azure AD SCIM uygulama anlama](#understanding-the-azure-ad-scim-implementation).

Uygulama sağlayıcınıza veya bilgilerinin bu gereksinimleri ile uyumluluk için uygulama sağlayıcının belgelerine başvurun.

>[!IMPORTANT]
>Azure AD SCIM uygulama sağlama sürekli kullanıcılar Azure AD arasında eşitlenmiş kalmasını sağlamak için tasarlanan hizmeti, Azure AD kullanıcısı üzerinde kurulmuştur ve hedef uygulama ve belirli bir dizi standart işlemlerini uygular. Azure AD SCIM istemci davranışını anlamak için bu davranışların anlamak önemlidir. Daha fazla bilgi için [kullanıcı sağlama sırasında ne olur?](user-provisioning.md#what-happens-during-provisioning).

### <a name="getting-started"></a>Başlarken
Bu makalede açıklanan SCIM profilini destekleyen uygulamalar, Azure Active Directory Azure AD uygulama galerisinde bulunan "galeri dışı uygulama" özelliğini kullanarak bağlanabilir. Bağlantı kurulduktan sonra Azure AD eşitleme işlemi burada, uygulamanın SCIM uç noktası için atanan kullanıcılar ve gruplar, sorgular ve oluşturuyor veya bunları göre atama ayrıntıları 40 dakikada bir çalışır.

**SCIM'yi destekleyen bir uygulamaya bağlanmak için:**

1. Oturum [Azure Active Directory portalında](https://aad.portal.azure.com). 

1. Seçin **kurumsal uygulamalar** sol bölmeden. Galeriden eklenen uygulamaları dahil olmak üzere tüm yapılandırılmış uygulamaların bir listesi gösterilir.

1. Seçin **+ yeni uygulama** > **tüm** > **galeri dışı uygulama**.

1. Uygulamanız için bir ad girin ve seçin **Ekle** uygulama nesnesi oluşturulamıyor. Yeni uygulama, kurumsal uygulamalar listesine eklenir ve uygulama yönetimi ekranına açar.
    
   ![][1]
   *Şekil 2: Azure AD uygulama Galerisi*
    
1. Uygulama Yönetimi ekranında seçin **sağlama** sol bölmesinde.
1. İçinde **sağlama modu** menüsünde **otomatik**.
    
   ![][2]
   *Şekil 3: Azure portalında sağlama yapılandırma*
    
1. İçinde **Kiracı URL'si** uygulamanın SCIM uç nokta URL'sini girin. Örnek: https://api.contoso.com/scim/v2/
1. SCIM uç noktanın bir OAuth taşıyıcı belirtecinden bir veren Azure AD dışındaki gerektiriyorsa, gerekli OAuth taşıyıcı belirteci sonra isteğe bağlı kopyalayın **gizli belirteç** alan. Bu alan boş bırakılırsa, Azure AD, her isteği ile Azure AD tarafından verilen bir OAuth taşıyıcı belirtecini içerir. Kimlik sağlayıcısı olarak Azure AD'yi kullanan uygulamalar, bu Azure AD tarafından verilen belirteci doğrulayabilirsiniz.
1. Seçin **Test Bağlantısı** için Azure Active Directory SCIM uç noktaya bağlanmayı deneyin. Deneme başarısız olursa hata bilgileri görüntülenir.  

    >[!NOTE]
    >**Bağlantıyı Sına** SCIM uç nokta Azure AD yapılandırmasında seçili eşleşen özellik olarak rastgele bir GUID kullanarak mevcut olmayan bir kullanıcı için sorgular. Beklenen doğru yanıt, bir boş SCIM ListResponse iletisiyle HTTP 200 OK. 

1. Uygulama başarılı olmasına bağlanma girişimleri ardından seçerseniz **Kaydet** yönetici kimlik bilgilerini kaydetmek için.
1. İçinde **eşlemeleri** bölümünde, iki seçilebilir öznitelik eşlemelerini kümesi vardır: biri kullanıcı, nesneyi, diğeri için Grup nesneleri. Uygulamanızı Azure Active Directory'den eşitlenen öznitelikler gözden geçirmek için her birini seçin. Seçilen öznitelikler **eşleşen** özellikleri, kullanıcıları ve grupları güncelleştirme işlemleri için uygulamanızda eşleştirmek için kullanılır. Seçin **Kaydet** değişiklikleri uygulamak için.

    >[!NOTE]
    >İsteğe bağlı olarak, "eşleme grupları" devre dışı bırakarak Grup nesnelerini eşitlemeyi devre dışı bırakın. 

1. Altında **ayarları**, **kapsam** alanı, hangi kullanıcıların ve grupların eşitlenmesi tanımlar. Seçin **eşitleme yalnızca atanan kullanıcılar ve gruplar** içinde atanan kullanıcıların ve grupların yalnızca eşitlenecek (önerilen) **kullanıcılar ve gruplar** sekmesi.
1. Yapılandırma tamamlandıktan sonra ayarlanmış **sağlama durumu** için **üzerinde**.
1. Seçin **Kaydet** Azure AD sağlama hizmeti başlatılamadı. 
1. Eşitleme yalnızca kullanıcılar ve gruplar (önerilen) atadıysanız, seçtiğinizden emin olun **kullanıcılar ve gruplar** sekmesini ve kullanıcıları veya grupları eşitlemek istediğiniz atayın.

İlk eşitleme başlatıldıktan sonra seçebileceğiniz **denetim günlükleri** ilerleme durumunu izlemek için sol panelde, uygulamanızdan sağlama hizmeti tarafından gerçekleştirilen tüm eylemler gösterilir. Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](check-status-user-account-provisioning.md).

> [!NOTE]
> İlk eşitleme hizmeti çalışıyor sürece yaklaşık 40 dakikada oluşan sonraki eşitlemeler uzun sürer. 

## <a name="understanding-the-azure-ad-scim-implementation"></a>Azure AD SCIM uygulama anlama

SCIM 2.0 kullanıcı yönetim API'sini destekleyen bir uygulama oluşturuyorsanız, bu bölümde ayrıntılı olarak Azure AD SCIM istemci nasıl uygulandığını ve SCIM protokolünüzü nasıl model açıklar ve yanıtları işleme isteği. SCIM uç noktanızı uyguladık sonra önceki bölümde açıklanan yordamı kullanarak test edebilirsiniz.

İçinde [SCIM 2.0 protokolü belirtimi](http://www.simplecloud.info/#Specification), uygulamanız bu gereksinimleri karşılaması gerekir:

* Kullanıcılar ve isteğe bağlı olarak ayrıca bölümüne göre gruplar oluşturmayı destekler [3.3 SCIM Protokolü](https://tools.ietf.org/html/rfc7644#section-3.3).  
* Başına olarak kullanıcılar veya gruplar PATCH isteklerinde ile değiştirme destekler [SCIM Protokolü 3.5.2 bölümünde](https://tools.ietf.org/html/rfc7644#section-3.5.2).  
* Bir kullanıcı veya grup daha önce oluşturduğunuz için bilinen bir kaynak alma destekler olarak başına [SCIM Protokolü 3.4.1 bölümünde](https://tools.ietf.org/html/rfc7644#section-3.4.1).  
* Kullanıcılar veya gruplar, bölüm uyarınca sorgulanmasını destekler [SCIM Protokolü 3.4.2](https://tools.ietf.org/html/rfc7644#section-3.4.2).  Varsayılan olarak, kullanıcılar tarafından alınan kendi `id` ve tarafından sorgulanan kendi `username` ve `externalid`, ve grupları tarafından sorgulanan `displayName`.  
* Kullanıcı kimliği ve SCIM Protokolü 3.4.2 bölümünü göre Yöneticisi tarafından sorgulanmasını destekler.  
* Grup Kimliği ve bölüm 3.4.2 SCIM Protokolü göre bir üyesi tarafından sorgulanmasını destekler.  
* Kimlik doğrulama ve yetkilendirme, uygulamanıza Azure ad için bir tek taşıyıcı belirteci kabul eder.

Azure AD ile uyumluluğu sağlamak için SCIM'yi endpoint uygularken aşağıdaki genel yönergeleri izleyin:

* `id` tüm kaynaklar için gerekli bir özelliktir. Her kaynak bir kaynak sağlamak döndüren her yanıt, dışında bu özelliğe sahip. `ListResponse` sıfır üyelere sahip.
* Sorgu/filtresi isteğine yanıt olarak her zaman olmalıdır bir `ListResponse`.
* SCIM uygulaması PATCH isteklerinde destekliyorsa desteklenen isteğe bağlıdır, ancak yalnızca gruplarıdır.
* Tüm kaynak düzeltme eki yanıta dahil etmek gerekli değildir.
* Microsoft Azure AD, yalnızca aşağıdaki işleçleri kullanır:  
     - `eq`
     - `and`
* Belirli bir düzeltme eki SCIM yapısal öğelere büyük küçük harfe duyarlı bir eşleşme gerektirmeyen `op` tanımlandığı gibi işlem değerleri https://tools.ietf.org/html/rfc7644#section-3.5.2. Azure AD olarak 'Durdur' değerini yayan `Add`, `Replace`, ve `Remove`.
* Microsoft Azure AD, bir rastgele kullanıcı ve Grup uç noktası ve kimlik bilgilerinin geçerli olduğundan emin olmak için getirilecek istekleri sağlar. Bir parçası olarak yapılır **Test Bağlantısı** içindeki akış [Azure portalında](https://portal.azure.com). 
* Uygulama üzerinde eşleşen bir öznitelik olarak kaynaklar üzerinde sorgulanabilir özniteliği ayarlanmalıdır [Azure portalında](https://portal.azure.com). Daha fazla bilgi için [kullanıcı sağlama öznitelik eşlemelerini özelleştirme](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-customizing-attribute-mappings)

### <a name="user-provisioning-and-de-provisioning"></a>Kullanıcı hazırlama ve sağlamayı
Aşağıdaki çizimde gösterildiği Azure Active Directory SCIM hizmete bir kullanıcının, uygulamanızın kimlik deposu içinde yaşam döngüsünü yönetirken gönderdiği iletileri.  

![][4]
*Şekil 4: Kullanıcı hazırlama ve sağlamayı dizisi*

### <a name="group-provisioning-and-de-provisioning"></a>Grup sağlama ve sağlamayı
Hazırlama ve sağlamayı grup isteğe bağlıdır. Uygulanan ve etkin olduğunda, aşağıdaki resimde, Azure iletileri gösterir. AD, uygulamanızın kimlik deposu olarak bir grubunun yaşam döngüsünü yönetirken SCIM hizmetine gönderir.  Bu iletileri iki yolla kullanıcılar hakkındaki iletileri farklıdır: 

* Grupları almak için istekleri isteğine yanıt olarak sağlanan herhangi bir kaynaktan çıkarılacak üyeleri öznitelik olduğunu belirtin.  
* Bir başvuru özniteliği, belirli bir değere sahip olup olmadığını belirlemek üzere istekleri üyeleri özniteliği hakkında isteklerdir.  

![][5]
*Şekil 5: Grup sağlama ve sağlamayı dizisi*

### <a name="scim-protocol-requests-and-responses"></a>SCIM protokol istekleri ve yanıtları
Bu bölümde, örnek SCIM istekleri beklenen yanıt örneği ve Azure AD SCIM istemci tarafından yayılan sağlar. En iyi sonuçlar için uygulamanızın şu biçimde bu istekler işlemek ve beklenen yanıt yayma kodunu döndürmelidir.

>[!IMPORTANT]
>Nasıl ve ne zaman Azure AD kullanıcı sağlama hizmeti aşağıda açıklanan işlemleri yayan anlamak için bkz [kullanıcı sağlama sırasında ne olur?](user-provisioning.md#what-happens-during-provisioning).

- [Kullanıcı işlemleri](#user-operations)
  - [Kullanıcı Oluştur](#create-user)
    - [İstek](#request)
    - [Yanıt](#response)
  - [Kullanıcı Al](#get-user)
    - [İstek](#request-1)
    - [Yanıt](#response-1)
  - [Kullanıcı tarafından sorgu Al](#get-user-by-query)
    - [İstek](#request-2)
    - [Yanıt](#response-2)
  - [Kullanıcı sorgusu - sıfır sonuçlarını Al](#get-user-by-query---zero-results)
    - [İstek](#request-3)
    - [Yanıt](#response-3)
  - [[Birden çok değerli özellikler] kullanıcı güncelleştirme](#update-user-multi-valued-properties)
    - [İstek](#request-4)
    - [Yanıt](#response-4)
  - [Kullanıcı güncelleştirme [tek değerli özellikler]](#update-user-single-valued-properties)
    - [İstek](#request-5)
    - [Yanıt](#response-5)
  - [Kullanıcıyı Silme](#delete-user)
    - [İstek](#request-6)
    - [Yanıt](#response-6)
- [Grup işlemleri](#group-operations)
  - [Grup Oluşturma](#create-group)
    - [İstek](#request-7)
    - [Yanıt](#response-7)
  - [Grubu Al](#get-group)
    - [İstek](#request-8)
    - [Yanıt](#response-8)
  - [DisplayName tarafından grubunu Al](#get-group-by-displayname)
    - [İstek](#request-9)
    - [Yanıt](#response-9)
  - [Güncelleştirme grubu [olmayan üye öznitelikleri]](#update-group-non-member-attributes)
    - [İstek](#request-10)
    - [Yanıt](#response-10)
  - [Güncelleştirme grubu [üye ekleme]](#update-group-add-members)
    - [İstek](#request-11)
    - [Yanıt](#response-11)
  - [Güncelleştirme grubu [Kaldır üyeleri]](#update-group-remove-members)
    - [İstek](#request-12)
    - [Yanıt](#response-12)
  - [Grubu Silme](#delete-group)
    - [İstek](#request-13)
    - [Yanıt](#response-13)

### <a name="user-operations"></a>Kullanıcı işlemleri

* Kullanıcılar tarafından sorgulanabilir `userName` veya `email[type eq "work"]` öznitelikleri.  

#### <a name="create-user"></a>Kullanıcı Oluştur

###### <a name="request"></a>İstek
*POST/kullanıcılar*
```json
{
    "schemas": [
        "urn:ietf:params:scim:schemas:core:2.0:User",
        "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"],
    "externalId": "0a21f0f2-8d2a-4f8e-bf98-7363c4aed4ef",
    "userName": "Test_User_ab6490ee-1e48-479e-a20b-2d77186b5dd1",
    "active": true,
    "emails": [{
        "primary": true,
        "type": "work",
        "value": "Test_User_fd0ea19b-0777-472c-9f96-4f70d2226f2e@testuser.com"
    }],
    "meta": {
        "resourceType": "User"
    },
    "name": {
        "formatted": "givenName familyName",
        "familyName": "familyName",
        "givenName": "givenName"
    },
    "roles": []
}
```

##### <a name="response"></a>Yanıt
*HTTP/1.1 201 oluşturuldu*
```json
{
    "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
    "id": "48af03ac28ad4fb88478",
    "externalId": "0a21f0f2-8d2a-4f8e-bf98-7363c4aed4ef",
    "meta": {
        "resourceType": "User",
        "created": "2018-03-27T19:59:26.000Z",
        "lastModified": "2018-03-27T19:59:26.000Z"
    },
    "userName": "Test_User_ab6490ee-1e48-479e-a20b-2d77186b5dd1",
    "name": {
        "formatted": "givenName familyName",
        "familyName": "familyName",
        "givenName": "givenName",
    },
    "active": true,
    "emails": [{
        "value": "Test_User_fd0ea19b-0777-472c-9f96-4f70d2226f2e@testuser.com",
        "type": "work",
        "primary": true
    }]
}
```


#### <a name="get-user"></a>Kullanıcı Al

###### <a name="request"></a>İstek
*/Users/5d48a0a8e9f04aa38008 Al* 

###### <a name="response"></a>Yanıt
*HTTP/1.1 200 TAMAM*
```json
{
    "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
    "id": "5d48a0a8e9f04aa38008",
    "externalId": "58342554-38d6-4ec8-948c-50044d0a33fd",
    "meta": {
        "resourceType": "User",
        "created": "2018-03-27T19:59:26.000Z",
        "lastModified": "2018-03-27T19:59:26.000Z"
    },
    "userName": "Test_User_feed3ace-693c-4e5a-82e2-694be1b39934",
    "name": {
        "formatted": "givenName familyName",
        "familyName": "familyName",
        "givenName": "givenName",
    },
    "active": true,
    "emails": [{
        "value": "Test_User_22370c1a-9012-42b2-bf64-86099c2a1c22@testuser.com",
        "type": "work",
        "primary": true
    }]
}
```
#### <a name="get-user-by-query"></a>Kullanıcı tarafından sorgu Al

##### <a name="request"></a>İstek
*GET/kullanıcılar? filtre kullanıcıadı eq "Test_User_dfeef4c5-5681-4387-b016-bdf221e82081" =*

##### <a name="response"></a>Yanıt
*HTTP/1.1 200 TAMAM*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:ListResponse"],
    "totalResults": 1,
    "Resources": [{
        "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
        "id": "2441309d85324e7793ae",
        "externalId": "7fce0092-d52e-4f76-b727-3955bd72c939",
        "meta": {
            "resourceType": "User",
            "created": "2018-03-27T19:59:26.000Z",
            "lastModified": "2018-03-27T19:59:26.000Z"
            
        },
        "userName": "Test_User_dfeef4c5-5681-4387-b016-bdf221e82081",
        "name": {
            "familyName": "familyName",
            "givenName": "givenName"
        },
        "active": true,
        "emails": [{
            "value": "Test_User_91b67701-697b-46de-b864-bd0bbe4f99c1@testuser.com",
            "type": "work",
            "primary": true
        }]
    }],
    "startIndex": 1,
    "itemsPerPage": 20
}

```

#### <a name="get-user-by-query---zero-results"></a>Kullanıcı sorgusu - sıfır sonuçlarını Al

##### <a name="request"></a>İstek
*GET/kullanıcılar? filtre kullanıcıadı eq "mevcut olmayan kullanıcı" =*

##### <a name="response"></a>Yanıt
*HTTP/1.1 200 TAMAM*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:ListResponse"],
    "totalResults": 0,
    "Resources": [],
    "startIndex": 1,
    "itemsPerPage": 20
}

```

#### <a name="update-user-multi-valued-properties"></a>[Birden çok değerli özellikler] kullanıcı güncelleştirme

##### <a name="request"></a>İstek
*/ Kullanıcı/6764549bef60420686bc HTTP/1.1 düzeltme eki*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:PatchOp"],
    "Operations": [
            {
            "op": "Replace",
            "path": "emails[type eq \"work\"].value",
            "value": "updatedEmail@microsoft.com"
            },
            {
            "op": "Replace",
            "path": "name.familyName",
            "value": "updatedFamilyName"
            }
    ]
}
```

##### <a name="response"></a>Yanıt
*HTTP/1.1 200 TAMAM*
```json
{
    "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
    "id": "6764549bef60420686bc",
    "externalId": "6c75de36-30fa-4d2d-a196-6bdcdb6b6539",
    "meta": {
        "resourceType": "User",
        "created": "2018-03-27T19:59:26.000Z",
        "lastModified": "2018-03-27T19:59:26.000Z"
    },
    "userName": "Test_User_fbb9dda4-fcde-4f98-a68b-6c5599e17c27",
    "name": {
        "formatted": "givenName updatedFamilyName",
        "familyName": "updatedFamilyName",
        "givenName": "givenName"
    },
    "active": true,
    "emails": [{
        "value": "updatedEmail@microsoft.com",
        "type": "work",
        "primary": true
    }]
}
```

#### <a name="update-user-single-valued-properties"></a>Kullanıcı güncelleştirme [tek değerli özellikler]

##### <a name="request"></a>İstek
*/ Kullanıcı/5171a35d82074e068ce2 HTTP/1.1 düzeltme eki*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:PatchOp"],
    "Operations": [{
        "op": "Replace",
        "path": "userName",
        "value": "5b50642d-79fc-4410-9e90-4c077cdd1a59@testuser.com"
    }]
}
```

##### <a name="response"></a>Yanıt
*HTTP/1.1 200 TAMAM*
```json
{
    "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
    "id": "5171a35d82074e068ce2",
    "externalId": "aa1eca08-7179-4eeb-a0be-a519f7e5cd1a",
    "meta": {
        "resourceType": "User",
        "created": "2018-03-27T19:59:26.000Z",
        "lastModified": "2018-03-27T19:59:26.000Z"
        
    },
    "userName": "5b50642d-79fc-4410-9e90-4c077cdd1a59@testuser.com",
    "name": {
        "formatted": "givenName familyName",
        "familyName": "familyName",
        "givenName": "givenName",
    },
    "active": true,
    "emails": [{
        "value": "Test_User_49dc1090-aada-4657-8434-4995c25a00f7@testuser.com",
        "type": "work",
        "primary": true
    }]
}
```

#### <a name="delete-user"></a>Kullanıcıyı Silme

##### <a name="request"></a>İstek
*/Users/5171a35d82074e068ce2 HTTP/1.1 Sil*

##### <a name="response"></a>Yanıt
*HTTP/1.1 204 No Content*

### <a name="group-operations"></a>Grup işlemleri

* Grupları her zaman bir boş üyeleri listesi ile oluşturulan.
* Grupları tarafından sorgulanabilir `displayName` özniteliği.
* Güncelleştirme grubu PATCH isteği için yield bir *HTTP 204 İçerik yok* yanıt. Tüm üyelerin listesini gövde döndüren önerilir değil.
* Grubun tüm üyelerinin döndüren desteklemek için gerekli değildir.

#### <a name="create-group"></a>Grup Oluşturma

##### <a name="request"></a>İstek
*POST /Groups HTTP/1.1*
```json
{
    "schemas": ["urn:ietf:params:scim:schemas:core:2.0:Group", "http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/2.0/Group"],
    "externalId": "8aa1a0c0-c4c3-4bc0-b4a5-2ef676900159",
    "id": "c4d56c3c-bf3b-4e96-9b64-837018d6060e",
    "displayName": "displayName",
    "members": [],
    "meta": {
        "resourceType": "Group"
    }
}
```

##### <a name="response"></a>Yanıt
*HTTP/1.1 201 oluşturuldu*
```json
{
    "schemas": ["urn:ietf:params:scim:schemas:core:2.0:Group"],
    "id": "927fa2c08dcb4a7fae9e",
    "externalId": "8aa1a0c0-c4c3-4bc0-b4a5-2ef676900159",
    "meta": {
        "resourceType": "Group",
        "created": "2018-03-27T19:59:26.000Z",
        "lastModified": "2018-03-27T19:59:26.000Z"
        
    },
    "displayName": "displayName",
    "members": []
}
```

#### <a name="get-group"></a>Grubu Al

##### <a name="request"></a>İstek
*GET/Grup/40734ae655284ad3abcc? excludedAttributes üyeleri HTTP/1.1 =*

##### <a name="response"></a>Yanıt
*HTTP/1.1 200 TAMAM*
```json
{
    "schemas": ["urn:ietf:params:scim:schemas:core:2.0:Group"],
    "id": "40734ae655284ad3abcc",
    "externalId": "60f1bb27-2e1e-402d-bcc4-ec999564a194",
    "meta": {
        "resourceType": "Group",
        "created": "2018-03-27T19:59:26.000Z",
        "lastModified": "2018-03-27T19:59:26.000Z"
    },
    "displayName": "displayName",
}
```

#### <a name="get-group-by-displayname"></a>DisplayName tarafından grubunu Al

##### <a name="request"></a>İstek
*GET /Groups? excludedAttributes üyeleri & Filtresi = "displayName" HTTP/1.1 displayName eq =*

##### <a name="response"></a>Yanıt
*HTTP/1.1 200 TAMAM*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:ListResponse"],
    "totalResults": 1,
    "Resources": [{
        "schemas": ["urn:ietf:params:scim:schemas:core:2.0:Group"],
        "id": "8c601452cc934a9ebef9",
        "externalId": "0db508eb-91e2-46e4-809c-30dcbda0c685",
        "meta": {
            "resourceType": "Group",
            "created": "2018-03-27T22:02:32.000Z",
            "lastModified": "2018-03-27T22:02:32.000Z",
            
        },
        "displayName": "displayName",
    }],
    "startIndex": 1,
    "itemsPerPage": 20
}
```
#### <a name="update-group-non-member-attributes"></a>Güncelleştirme grubu [olmayan üye öznitelikleri]

##### <a name="request"></a>İstek
*/ Grup/fa2ce26709934589afc5 HTTP/1.1 düzeltme eki*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:PatchOp"],
    "Operations": [{
        "op": "Replace",
        "path": "displayName",
        "value": "1879db59-3bdf-4490-ad68-ab880a269474updatedDisplayName"
    }]
}
```

##### <a name="response"></a>Yanıt
*HTTP/1.1 204 No Content*

### <a name="update-group-add-members"></a>Güncelleştirme grubu [üye ekleme]

##### <a name="request"></a>İstek
*/ Grup/a99962b9f99d4c4fac67 HTTP/1.1 düzeltme eki*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:PatchOp"],
    "Operations": [{
        "op": "Add",
        "path": "members",
        "value": [{
            "$ref": null,
            "value": "f648f8d5ea4e4cd38e9c"
        }]
    }]
}
```

##### <a name="response"></a>Yanıt
*HTTP/1.1 204 No Content*

#### <a name="update-group-remove-members"></a>Güncelleştirme grubu [Kaldır üyeleri]

##### <a name="request"></a>İstek
*/ Grup/a99962b9f99d4c4fac67 HTTP/1.1 düzeltme eki*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:PatchOp"],
    "Operations": [{
        "op": "Remove",
        "path": "members",
        "value": [{
            "$ref": null,
            "value": "f648f8d5ea4e4cd38e9c"
        }]
    }]
}
```

##### <a name="response"></a>Yanıt
*HTTP/1.1 204 No Content*

#### <a name="delete-group"></a>Grubu Silme

##### <a name="request"></a>İstek
*/Groups/cdb1ce18f65944079d37 HTTP/1.1 Sil*

##### <a name="response"></a>Yanıt
*HTTP/1.1 204 No Content*


## <a name="building-a-scim-endpoint-using-microsoft-cli-libraries"></a>Microsoft CLI kitaplıklar kullanılarak bir SCIM uç noktası oluşturma
Azure Active Directory ile arabirimleri SCIM bir web hizmeti oluşturarak, neredeyse tüm uygulama veya kimlik deposu için otomatik kullanıcı sağlamayı etkinleştirebilirsiniz.

Şekli aşağıda verilmiştir:

1. Azure AD kod örnekleri aşağıda açıklanan bulunan Microsoft.SystemForCrossDomainIdentityManagement, adlı ortak dil altyapısı (CLI) kitaplığı sağlar. Sistem Tümleştiriciler ve geliştiriciler bu kitaplığı oluşturmak ve herhangi bir uygulamanın kimlik deposu için Azure AD bağlanan bir SCIM tabanlı web hizmeti uç noktasına dağıtmak için kullanabilirsiniz.
2. Eşlemeleri, web hizmeti, standart kullanıcı şeması uygulamanın gerektirdiği protokolü ve kullanıcı şeması eşlemek için uygulanır. 
3. Uç nokta URL'si, Azure ad uygulama galerisinde özel bir uygulama bir parçası olarak kaydedilir.
4. Kullanıcıları ve grupları, Azure AD'de bu uygulama atanır. Atama sırasında hedef uygulama için eşitlenmesi gereken bir kuyruğun içine koyarlar. Sıranın işleme eşitleme işlemi 40 dakikada bir çalışır.

### <a name="code-samples"></a>Kod Örnekleri
Bu işlemi kolaylaştırmak için [kod örnekleri](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) bir SCIM oluşturduğunuz web hizmeti uç noktası ve otomatik sağlama göstermek, sağlanır. Kullanıcılar ve gruplar temsil eden virgülle ayrılmış değerler satırlarla dosya tutan bir sağlayıcısının örnektir.    

**Önkoşullar**

* Visual Studio 2013 veya üzeri
* [.NET için Azure SDK](https://azure.microsoft.com/downloads/)
* SCIM uç noktası olarak kullanılacak ASP.NET framework 4. 5'i destekleyen Windows makinesi. Bu makine bulut üzerinden erişilebilir olmalıdır.
* [Bir Azure aboneliği deneme sürümü ya da lisanslı bir Azure AD Premium sürümü ile](https://azure.microsoft.com/services/active-directory/)

### <a name="getting-started"></a>Başlarken
Azure ad sağlama isteklerini kabul edebilen bir SCIM uç noktası uygulamak için en kolay yolu, derleme ve dağıtma sağlanan kullanıcılar bir virgülle ayrılmış değer (CSV) dosyasına çıkarır kodu örneği sağlamaktır.

#### <a name="to-create-a-sample-scim-endpoint"></a>Bir örnek SCIM uç noktası oluşturma

1. Kod örnek paketini [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)
1. Paketin sıkıştırmasını açın ve Windows makinenizde C:\AzureAD-BYOA-Provisioning-Samples\ gibi bir konuma yerleştirin.
1. Bu klasörde, Visual Studio'da FileProvisioning\Host\FileProvisioningService.csproj projesi başlatın.
1. Seçin **Araçları** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**, için aşağıdaki komutları yürütün Çözüm başvurularını çözümlemek için FileProvisioningService proje:

   ```
    Update-Package -Reinstall
   ```

1. FileProvisioningService projeyi derleyin.
1. Windows komut istemi uygulamasını (Yönetici) olarak açmak ve kullanmak **cd** komut için dizini değiştirmek için **\AzureAD-BYOA-Provisioning-Samples\FileProvisioning\Host\bin\Debug**klasör.
1. Aşağıdaki komutu çalıştırın değiştirerek `<ip-address>` Windows makinenin IP adresi veya etki alanı adı ile:

   ```
    FileSvc.exe http://<ip-address>:9000 TargetFile.csv
   ```

1. Windows altında **Windows ayarları** > **ağ ve Internet ayarları**seçin **Windows Güvenlik Duvarı**  >   **Gelişmiş ayarlar**, oluşturup bir **gelen kuralı** , 9000 numaralı bağlantı noktasına gelen erişim sağlar.
1. Windows makine bir yönlendiricinin arkasında ise, yönlendirici ağ erişim çevirisi, bağlantı noktası İnternet'e 9000 ve bağlantı noktası 9000 arasında Windows makinede çalışacak şekilde yapılandırılması gerekir. Bu yapılandırma, bulutta Bu uç noktasına erişmek Azure AD için gereklidir.

#### <a name="to-register-the-sample-scim-endpoint-in-azure-ad"></a>Örnek SCIM uç nokta Azure AD'ye kaydetme

1. Oturum [Azure Active Directory portalında](https://aad.portal.azure.com). 

1. Seçin **kurumsal uygulamalar** sol bölmeden. Galeriden eklenen uygulamaları dahil olmak üzere tüm yapılandırılmış uygulamaların bir listesi gösterilir.

1. Seçin **+ yeni uygulama** > **tüm** > **galeri dışı uygulama**.

1. Uygulamanız için bir ad girin ve seçin **Ekle** uygulama nesnesi oluşturulamıyor. Oluşturulan uygulama nesnesi, için sağlama ve uygulama için çoklu oturum açmayı ve yalnızca SCIM uç noktası hedef uygulamayı temsil etmek üzere tasarlanmıştır.

1. Uygulama Yönetimi ekranında seçin **sağlama** sol bölmesinde.

1. İçinde **sağlama modu** menüsünde **otomatik**.
    
   ![][2]
   *Şekil 6: Azure portalında sağlama yapılandırma*
    
1. İçinde **Kiracı URL'si** internet açık URL ve bağlantı noktası SCIM uç noktanızı girin. Giriş aşağıdakine benzer olan http://testmachine.contoso.com:9000 veya http://\<IP adresi >: 9000 / burada \<IP adresi > olan internet açık IP adresi. 

1. SCIM uç noktanın bir OAuth taşıyıcı belirtecinden bir veren Azure AD dışındaki gerektiriyorsa, gerekli OAuth taşıyıcı belirteci sonra isteğe bağlı kopyalayın **gizli belirteç** alan. Bu alan boş bırakılırsa, Azure AD her isteği ile Azure AD tarafından verilen bir OAuth taşıyıcı belirtecini içerir. Azure AD kimlik sağlayıcısı bu Azure AD'ye doğrulayabilirsiniz olarak kullanan uygulamalar-belirteç.

1. Seçin **Test Bağlantısı** için Azure Active Directory SCIM uç noktaya bağlanmayı deneyin. Deneme başarısız olursa hata bilgileri görüntülenir.  

    >[!NOTE]
    >**Bağlantıyı Sına** SCIM uç nokta Azure AD yapılandırmasında seçili eşleşen özellik olarak rastgele bir GUID kullanarak mevcut olmayan bir kullanıcı için sorgular. Beklenen doğru yanıt, bir boş SCIM ListResponse iletisiyle HTTP 200 OK.

1. Uygulama başarılı olmasına bağlanma girişimleri ardından seçerseniz **Kaydet** yönetici kimlik bilgilerini kaydetmek için.

1. İçinde **eşlemeleri** bölümünde, iki seçilebilir öznitelik eşlemelerini kümesi vardır: biri kullanıcı, nesneyi, diğeri için Grup nesneleri. Uygulamanızı Azure Active Directory'den eşitlenen öznitelikler gözden geçirmek için her birini seçin. Seçilen öznitelikler **eşleşen** özellikleri, kullanıcıları ve grupları güncelleştirme işlemleri için uygulamanızda eşleştirmek için kullanılır. Seçin **Kaydet** değişiklikleri uygulamak için.

1. Altında **ayarları**, **kapsam** alanı grupları ve hangi kullanıcıların eşitleneceğini tanımlar. Seçin **"eşitleme yalnızca atanan kullanıcılar ve gruplar** içinde atanan kullanıcıların ve grupların yalnızca eşitlenecek (önerilen) **kullanıcılar ve gruplar** sekmesi.

1. Yapılandırma tamamlandıktan sonra ayarlanmış **sağlama durumu** için **üzerinde**.

1. Seçin **Kaydet** Azure AD sağlama hizmeti başlatılamadı. 

1. Eşitleme yalnızca kullanıcılar ve gruplar (önerilen) atadıysanız, seçtiğinizden emin olun **kullanıcılar ve gruplar** sekmesini ve kullanıcıları veya grupları eşitlemek istediğiniz atayın.

İlk eşitleme başlatıldıktan sonra seçebileceğiniz **denetim günlükleri** ilerleme durumunu izlemek için sol panelde, uygulamanızdan sağlama hizmeti tarafından gerçekleştirilen tüm eylemler gösterilir. Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](check-status-user-account-provisioning.md).

Örnek doğrulama son adım, Windows makinenizde \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug klasöründeki TargetFile.csv dosya açmaktır. Sağlama işlemi çalıştırıldıktan sonra bu dosya tüm ayrıntılarını atanan ve kullanıcıları ve grupları sağlanan gösterir.

### <a name="development-libraries"></a>Geliştirme kitaplıkları
SCIM Belirtimi'ne kendi web hizmeti geliştirmek için önce aşağıdaki kitaplıklar geliştirme süreci hızlandırmak için Microsoft tarafından sağlanan tanıyın: 

- Ortak dil altyapısı (CLI) kitaplıkları, C# gibi bu altyapısının temel dilleri ile kullanım için sunulur. Bu kitaplıklar Microsoft.SystemForCrossDomainIdentityManagement.Service, biri aşağıdaki çizimde gösterilen bir arabirim Microsoft.SystemForCrossDomainIdentityManagement.IProvider, bildirir. Kitaplıklar kullanılarak bir geliştirici için genel bir sağlayıcı olarak başvurulabilir bir sınıf, arabirim uygulayabilir. SCIM belirtimine uyan bir web hizmetini dağıtma geliştirici kitaplıkları sağlar. Web hizmeti Internet Information Services veya herhangi bir yürütülebilir CLI derleme içinde ya da barındırılabilir. İstek, geliştirici tarafından bazı kimlik deposu üzerinde çalışılacak programlanmak sağlayıcının yöntemlere yapılan çağrılar veri dönüştürülür.
  
   ![][3]
  
- [Express route işleyicileri](https://expressjs.com/guide/routing.html) bir node.js web hizmeti çağrıları (SCIM belirtimi tarafından tanımlanan) temsil eden node.js istek nesneleri ayrıştırmak için kullanılabilir yapılır.   

### <a name="building-a-custom-scim-endpoint"></a>Bir özel SCIM uç noktası oluşturma
CLI kitaplıkları kullanan geliştiriciler, Internet Information Services veya herhangi bir yürütülebilir CLI derleme içinde hizmetlerini barındırabilirler. İşte adresten yürütülebilir bir derleme içinde bir hizmet barındırma için örnek kod http://localhost:9000: 

    private static void Main(string[] arguments)
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IProvider and 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
      new DevelopersMonitor();
    Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
      new DevelopersProvider(arguments[1]);
    Microsoft.SystemForCrossDomainIdentityManagement.Service webService = null;
    try
    {
        webService = new WebService(monitor, provider);
        webService.Start("http://localhost:9000");

        Console.ReadKey(true);
    }
    finally
    {
        if (webService != null)
        {
            webService.Dispose();
            webService = null;
        }
    }
    }

    public class WebService : Microsoft.SystemForCrossDomainIdentityManagement.Service
    {
    private Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor;
    private Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider;

    public WebService(
      Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitoringBehavior, 
      Microsoft.SystemForCrossDomainIdentityManagement.IProvider providerBehavior)
    {
        this.monitor = monitoringBehavior;
        this.provider = providerBehavior;
    }

    public override IMonitor MonitoringBehavior
    {
        get
        {
            return this.monitor;
        }

        set
        {
            this.monitor = value;
        }
    }

    public override IProvider ProviderBehavior
    {
        get
        {
            return this.provider;
        }

        set
        {
            this.provider = value;
        }
    }
    }

Bu hizmet kök sertifika yetkilisi şu adlardan biri olduğu bir HTTP adresi ve sunucu kimlik doğrulama sertifikasında şunlar olmalıdır: 

* CNNIC
* Comodo
* CyberTrust
* DigiCert
* GeoTrust
* GlobalSign
* Go Daddy
* VeriSign
* WoSign

Bir sunucu kimlik doğrulama sertifikası için bir bağlantı noktası ağ Kabuğu yardımcı programını kullanarak Windows konağında bağlanabilir: 

    netsh http add sslcert ipport=0.0.0.0:443 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF}  

Rastgele bir genel benzersiz tanımlayıcı olsa da AppID bağımsız değişkeni için sağlanan değer, burada certhash bağımsız değişkeni için sağlanan sertifikanın parmak izini değerdir.  

Internet Information Services hizmetinde barındırmak için bir geliştirici bir CLI kod kitaplığı başlangıç derleme varsayılan ad alanını adlı bir sınıf ile derleme.  Böyle bir sınıfın bir örneği aşağıdadır: 

    public class Startup
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor and  
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter starter;

    public Startup()
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
          new DevelopersMonitor();
        Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
          new DevelopersProvider();
        this.starter = 
          new Microsoft.SystemForCrossDomainIdentityManagement.WebApplicationStarter(
            provider, 
            monitor);
    }

    public void Configuration(
      Owin.IAppBuilder builder) // Defined in Owin.dll.  
    {
        this.starter.ConfigureApplication(builder);
    }
    }

### <a name="handling-endpoint-authentication"></a>İşleme uç nokta kimlik doğrulaması
Azure Active Directory gelen istekleri bir OAuth 2.0 taşıyıcı belirteci içerir.   İstek alma herhangi bir hizmeti, Azure Active Directory Graph'i web hizmetine erişim için beklenen Azure Active Directory kiracısı için Azure Active Directory olarak veren kimlik doğrulamalıdır.  Belirteci, verenin "ISS" gibi bir ISS talep tanımlanır: "https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/".  Bu örnekte, talep değeri, temel adresini https://sts.windows.net, göreli adres sırada cbb1a5ac-f33b-45fa-9bf5-f37db0fed422 segment veren, Azure Active Directory tanımlar, Azure Active Directory Kiracı için benzersiz bir tanımlayıcıdır. belirteci veren.  Azure Active Directory Graph'i web hizmetine erişmek için belirteç verildiyse 00000002-0000-0000-c000-000000000000, bu hizmet tanımlayıcısı belirtecin aud talep değerine olması gerekir.  Tek bir kiracıda kayıtlı uygulamaların her biri aynı alabilirsiniz `iss` SCIM isteklerle talep.

SCIM hizmeti oluşturmak için Microsoft tarafından sağlanan CLI kitaplıkları kullanan geliştiriciler Microsoft.Owin.Security.ActiveDirectory paket, aşağıdaki adımları kullanarak Azure Active Directory isteklerinden kimlik doğrulaması yapabilir: 

1. Sağlayıcı, bu hizmeti başlatıldığında çağrılacak bir yöntem dönüş sağlayarak Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior özelliği uygulayın: 

   ```
     public override Action\<Owin.IAppBuilder, System.Web.Http.HttpConfiguration.HttpConfiguration\> StartupBehavior
     {
       get
       {
         return this.OnServiceStartup;
       }
     }

     private void OnServiceStartup(
       Owin.IAppBuilder applicationBuilder,  // Defined in Owin.dll.  
       System.Web.Http.HttpConfiguration configuration)  // Defined in System.Web.Http.dll.  
     {
     }
   ```

2. Herhangi bir hizmet uç noktaları Azure AD Graph web hizmetine erişim için belirtilen bir kiracı için Azure Active Directory tarafından verilmiş bir belirteç pul olarak kimlik doğrulaması yapılan tüm istekleri için bu yönteme aşağıdaki kodu ekleyin: 

   ```
     private void OnServiceStartup(
       Owin.IAppBuilder applicationBuilder IAppBuilder applicationBuilder, 
       System.Web.Http.HttpConfiguration HttpConfiguration configuration)
     {
       // IFilter is defined in System.Web.Http.dll.  
       System.Web.Http.Filters.IFilter authorizationFilter = 
         new System.Web.Http.AuthorizeAttribute(); // Defined in System.Web.Http.dll.configuration.Filters.Add(authorizationFilter);

       // SystemIdentityModel.Tokens.TokenValidationParameters is defined in    
       // System.IdentityModel.Token.Jwt.dll.
       SystemIdentityModel.Tokens.TokenValidationParameters tokenValidationParameters =     
         new TokenValidationParameters()
         {
           ValidAudience = "00000002-0000-0000-c000-000000000000"
         };

       // WindowsAzureActiveDirectoryBearerAuthenticationOptions is defined in 
       // Microsoft.Owin.Security.ActiveDirectory.dll
       Microsoft.Owin.Security.ActiveDirectory.
       WindowsAzureActiveDirectoryBearerAuthenticationOptions authenticationOptions =
         new WindowsAzureActiveDirectoryBearerAuthenticationOptions()    {
         TokenValidationParameters = tokenValidationParameters,
         Tenant = "03F9FCBC-EA7B-46C2-8466-F81917F3C15E" // Substitute the appropriate tenant’s 
                                                       // identifier for this one.  
       };

       applicationBuilder.UseWindowsAzureActiveDirectoryBearerAuthentication(authenticationOptions);
     }
   ```

### <a name="handling-provisioning-and-deprovisioning-of-users"></a>İşleme sağlama ve sağlamayı kaldırma kullanıcıları

1. Azure Active Directory, Azure AD'de kullanıcı mailNickname özniteliğinin değeri ile eşleşen bir externalID öznitelik değeri olan bir kullanıcı için hizmet sorgular. Sorgu, burada görüntülerle jyoung bir kullanıcının Azure Active Directory'de bir mailNickname örneğidir, bu örnek gibi bir Köprü Metni Aktarım Protokolü (HTTP) isteği olarak ifade edilir.

    >[!NOTE]
    > Bu yalnızca örnektir. Tüm kullanıcıların mailNickname özniteliğine sahip olacağı ve bir kullanıcının değer dizininde benzersiz olmayabilir. Ayrıca, (olan bu durumda externalID) eşleştirmek için kullanılan öznitelik yapılandırılabilir [Azure AD'ye öznitelik eşlemelerini](customize-application-attributes.md).

   ````
    GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
    Authorization: Bearer ...
   ````
   Hizmet SCIM Hizmetleri uygulamak için Microsoft tarafından sağlanan CLI kitaplıklar kullanılarak oluşturulduysa, isteği hizmet sağlayıcısının sorgu yöntemine bir çağrı çevrilir.  Aşağıda, bu yöntem imzası verilmiştir: 
   ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Protocol.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource[]> Query(
      Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters parameters, 
      string correlationIdentifier);
   ````
   Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters arabirim tanımı aşağıda verilmiştir: 
   ````
    public interface IQueryParameters: 
      Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
        System.Collections.Generic.IReadOnlyCollection <Microsoft.SystemForCrossDomainIdentityManagement.IFilter> AlternateFilters 
        { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
      system.Collections.Generic.IReadOnlyCollection<string> ExcludedAttributePaths 
      { get; }
      System.Collections.Generic.IReadOnlyCollection<string> RequestedAttributePaths 
      { get; }
      string SchemaIdentifier 
      { get; }
    }

   ```
     GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
     Authorization: Bearer ...
   ```

   If the service was built using the Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services, then the request is translated into a call to the Query method of the service’s provider.  Here is the signature of that method: 

   ```
     // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
     // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
     // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
     // Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters is defined in 
     // Microsoft.SystemForCrossDomainIdentityManagement.Protocol.  
 
     System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource[]>  Query(
       Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters parameters, 
       string correlationIdentifier);
   ```

   Here is the definition of the Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters interface: 

   ```
     public interface IQueryParameters: 
       Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
     {
         System.Collections.Generic.IReadOnlyCollection  <Microsoft.SystemForCrossDomainIdentityManagement.IFilter> AlternateFilters 
         { get; }
     }

     public interface Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
     {
       system.Collections.Generic.IReadOnlyCollection<string> ExcludedAttributePaths 
       { get; }
       System.Collections.Generic.IReadOnlyCollection<string> RequestedAttributePaths 
       { get; }
       string SchemaIdentifier 
       { get; }
     }

     public interface Microsoft.SystemForCrossDomainIdentityManagement.IFilter
     {
         Microsoft.SystemForCrossDomainIdentityManagement.IFilter AdditionalFilter 
           { get; set; }
         string AttributePath 
           { get; } 
         Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator FilterOperator 
           { get; }
         string ComparisonValue 
           { get; }
     }

     public enum Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator
     {
         Equals
     }
   ```

   In the following sample of a query for a user with a given value for the externalId attribute, values of the arguments passed to the Query method are: 
   * parameters.AlternateFilters.Count: 1
   * parameters.AlternateFilters.ElementAt(0).AttributePath: "externalId"
   * parameters.AlternateFilters.ElementAt(0).ComparisonOperator: ComparisonOperator.Equals
   * parameters.AlternateFilter.ElementAt(0).ComparisonValue: "jyoung"
   * correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin.RequestId"] 

2. If the response to a query to the web service for a user with an externalId attribute value that matches the mailNickname attribute value of a user doesn't return any users, then Azure Active Directory requests that the service provision a user corresponding to the one in Azure Active Directory.  Here is an example of such a request: 

   ````
    Https://.../scim/Users HTTP/1.1 YETKİLENDİRME: Taşıyıcı...  İçerik türü: uygulama/scım + json {"şemaları": ["urn: ietf:params:scim:schemas:core:2.0:User", "urn: ietf:params:scim:schemas:extension:enterprise:2.0User"] "externalID =": "jyoung", "userName": "jyoung", "etkin": true, "Adres": null    "displayName": "Oyun Young", "e-postaları": [{"type": "İş", "value": "jyoung@Contoso.com", "birincil": true}], "meta": {"resourceType": "Kullanıcı"}, "name": {"familyName": "Küçük", "givenName": "Oyun"}, "PhoneNumber": "preferredLa null nguage": null,"title":"departman"null: null,"Yönetici": null}
   ````
   The CLI libraries provided by Microsoft for implementing SCIM services would translate that request into a call to the Create method of the service’s provider.  The Create method has this signature: 
   ````
    System.Threading.Tasks.Tasks mscorlib.dll içinde tanımlanır.  
    İçinde tanımlı Microsoft.SystemForCrossDomainIdentityManagement.Resource / / Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  

    System.Threading.Tasks.Task < Microsoft.SystemForCrossDomainIdentityManagement.Resource > oluşturun (Microsoft.SystemForCrossDomainIdentityManagement.Resource kaynak, dize correlationIdentifier);
   ````
   In a request to provision a user, the value of the resource argument is an instance of the Microsoft.SystemForCrossDomainIdentityManagement. Core2EnterpriseUser class, defined in the Microsoft.SystemForCrossDomainIdentityManagement.Schemas library.  If the request to provision the user succeeds, then the implementation of the method is expected to return an instance of the Microsoft.SystemForCrossDomainIdentityManagement. Core2EnterpriseUser class, with the value of the Identifier property set to the unique identifier of the newly provisioned user.  

3. To update a user known to exist in an identity store fronted by an SCIM, Azure Active Directory proceeds by requesting the current state of that user from the service with a request such as: 
   ````
    ~/Scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1 yetkilendirme alın: Taşıyıcı...
   ````
   In a service built using the CLI libraries provided by Microsoft for implementing SCIM services, the request is translated into a call to the Retrieve method of the service’s provider.  Here is the signature of the Retrieve method: 
   ````
    System.Threading.Tasks.Tasks mscorlib.dll içinde tanımlanır.  
    Microsoft.SystemForCrossDomainIdentityManagement.Resource ve / / Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters / / Microsoft.SystemForCrossDomainIdentityManagement.Schemas içinde tanımlanır.  
    System.Threading.Tasks.Task < Microsoft.SystemForCrossDomainIdentityManagement.Resource > Al (Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters parametreleri, dize correlationIdentifier);

    Ortak arabirim Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters:   
        IRetrievalParameters {Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier Resourceıdentifier {get;}} genel arabiriminin Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier {dize tanımlayıcısı {get; kümesi;} dize Microsoft.SystemForCrossDomainIdentityManagement.SchemaIdentifier {get; kümesi;}}
   ````
   In the example of a request to retrieve the current state of a user, the values of the properties of the object provided as the value of the parameters argument are as follows: 
  
   * Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
   * SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"

4. If a reference attribute is to be updated, then Azure Active Directory queries the service to determine whether the current value of the reference attribute in the identity store fronted by the service already matches the value of that attribute in Azure Active Directory. For users, the only attribute of which the current value is queried in this way is the manager attribute. Here is an example of a request to determine whether the manager attribute of a particular user object currently has a certain value: 

   If the service was built using the CLI libraries provided by Microsoft for implementing SCIM services, then the request is translated into a call to the Query method of the service’s provider. The value of the properties of the object provided as the value of the parameters argument are as follows: 
  
   * parameters.AlternateFilters.Count: 2
   * parameters.AlternateFilters.ElementAt(x).AttributePath: "ID"
   * parameters.AlternateFilters.ElementAt(x).ComparisonOperator: ComparisonOperator.Equals
   * parameters.AlternateFilter.ElementAt(x).ComparisonValue: "54D382A4-2050-4C03-94D1-E769F1D15682"
   * parameters.AlternateFilters.ElementAt(y).AttributePath: "manager"
   * parameters.AlternateFilters.ElementAt(y).ComparisonOperator: ComparisonOperator.Equals
   * parameters.AlternateFilter.ElementAt(y).ComparisonValue: "2819c223-7f76-453a-919d-413861904646"
   * parameters.RequestedAttributePaths.ElementAt(0): "ID"
   * parameters.SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"

   Here, the value of the index x can be 0 and the value of the index y can be 1, or the value of x can be 1 and the value of y can be 0, depending on the order of the expressions of the filter query parameter.   

5. Here is an example of a request from Azure Active Directory to an SCIM service to update a user: 
   ````
    Düzeltme eki ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1 yetkilendirme: Taşıyıcı...  İçerik türü: uygulama/scım + json {"şemaları": ["urn: ietf:params:scim:api:messages:2.0:PatchOp"], "İşlemler": [{"op": "Ekle", "path": "Yönetici", "value": [{"$ref": "http://.../scim/Users/2819c223-7f76-453a-919d-413861904646", "value": "2819c223-7f76-453a-919d-413861904646"}]}]}
   ````
   The Microsoft CLI libraries for implementing SCIM services would translate the request into a call to the Update method of the service’s provider. Here is the signature of the Update method: 
   ````
    System.Threading.Tasks.Tasks ve / / System.Collections.Generic.IReadOnlyCollection<T> / / mscorlib.dll içinde tanımlanır.  
    Microsoft.SystemForCrossDomainIdentityManagement.IPatch, / / Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase, / / Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, / / Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation, / / Microsoft.SystemForCrossDomainIdentityManagement.OperationName, / / Microsoft.SystemForCrossDomainIdentityManagement.IPath ve / / Microsoft.SystemForCrossDomainIdentityManagement.OperationValue / veya tüm tanımlandığı Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 

    System.Threading.Tasks.Task güncelleştirme (Microsoft.SystemForCrossDomainIdentityManagement.IPatch düzeltme eki, dize correlationIdentifier);

    Ortak arabirim Microsoft.SystemForCrossDomainIdentityManagement.IPatch {Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase PatchRequest {get; kümesi;} Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier Resourceıdentifier {get; kümesi;}        
    }

    Genel sınıf PatchRequest2:    Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase {genel System.Collections.Generic.IReadOnlyCollection < Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation > işlemlerini {get;}


   Hizmet SCIM Hizmetleri uygulamak için Microsoft tarafından sağlanan ortak dil altyapısı kitaplıklar kullanılarak oluşturulduysa, isteği hizmet sağlayıcısının sorgu yöntemine bir çağrı çevrilir. Parametre bağımsız değişkenin değeri sağlanan nesne özelliklerini değerini aşağıdaki gibidir: 
  
* Parametreler. AlternateFilters.Count: 2
* parameters.AlternateFilters.ElementAt(x).AttributePath: "Kimlik"
* parameters.AlternateFilters.ElementAt(x).ComparisonOperator: ComparisonOperator.Equals
* Parametreler. AlternateFilter.ElementAt(x). ComparisonValue:  "54D382A4-2050-4C03-94D1-E769F1D15682"
* Parametreler. AlternateFilters.ElementAt(y). AttributePath: "Yönetici"
* parameters.AlternateFilters.ElementAt(y).ComparisonOperator: ComparisonOperator.Equals
* parameters.AlternateFilter.ElementAt(y).ComparisonValue:  "2819c223-7f76-453a-919d-413861904646"
* Parametreler. RequestedAttributePaths.ElementAt(0): "Kimlik"
* Parametreler. SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

  Burada x değeri 0 olabilir ve dizin y değeri 1 ' olabilir veya x değerini 1 olabilir ve y değeri filtre sorgu parametresinin ifadelerin düzene bağlı olarak 0 olabilir.   

1. İsteğinin bir örneği bir Azure Active Directory'den kullanıcıyı güncelleştirmek için SCIM'yi hizmetine şöyledir: 

   ```
     PATCH ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
     Authorization: Bearer ...
     Content-type: application/scim+json
     {
       "schemas": 
       [
         "urn:ietf:params:scim:api:messages:2.0:PatchOp"],
       "Operations":
       [
         {
           "op":"Add",
           "path":"manager",
           "value":
             [
               {
                 "$ref":"http://.../scim/Users/2819c223-7f76-453a-919d-413861904646",
                 "value":"2819c223-7f76-453a-919d-413861904646"}]}]}
   ```

   SCIM Hizmetleri uygulamak için Microsoft ortak dil altyapısı kitaplıkları hizmet sağlayıcısının güncelleştirme yöntemine bir çağrı isteği çevirir. Güncelleştirme metodun imzası şu şekildedir: 

   ```
     // System.Threading.Tasks.Tasks and 
     // System.Collections.Generic.IReadOnlyCollection<T>
     // are defined in mscorlib.dll.  
     // Microsoft.SystemForCrossDomainIdentityManagement.IPatch, 
     // Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase, 
     // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
     // Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation, 
     // Microsoft.SystemForCrossDomainIdentityManagement.OperationName, 
     // Microsoft.SystemForCrossDomainIdentityManagement.IPath and 
     // Microsoft.SystemForCrossDomainIdentityManagement.OperationValue 
     // are all defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 

     System.Threading.Tasks.Task Update(
       Microsoft.SystemForCrossDomainIdentityManagement.IPatch patch, 
       string correlationIdentifier);

     public interface Microsoft.SystemForCrossDomainIdentityManagement.IPatch
     {
     Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase 
       PatchRequest 
         { get; set; }
     Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
       ResourceIdentifier 
         { get; set; }        
     }

     public class PatchRequest2: 
       Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase
     {
     public System.Collections.Generic.IReadOnlyCollection
       <Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation> 
         Operations
         { get;}

     public void AddOperation(
       Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation operation);
     }

     public class PatchOperation
     {
     public Microsoft.SystemForCrossDomainIdentityManagement.OperationName 
       Name
       { get; set; }

     public Microsoft.SystemForCrossDomainIdentityManagement.IPath 
       Path
       { get; set; }

     public System.Collections.Generic.IReadOnlyCollection
       <Microsoft.SystemForCrossDomainIdentityManagement.OperationValue> Value
       { get; }

     public void AddValue(
       Microsoft.SystemForCrossDomainIdentityManagement.OperationValue value);
     }

     public enum OperationName
     {
       Add,
       Remove,
       Replace
     }

     public interface IPath
     {
       string AttributePath { get; }
       System.Collections.Generic.IReadOnlyCollection<IFilter> SubAttributes { get; }
       Microsoft.SystemForCrossDomainIdentityManagement.IPath ValuePath { get; }
     }

     public class OperationValue
     {
       public string Reference
       { get; set; }

       public string Value
       { get; set; }
     }
   ```

    Kullanıcıyı güncelleştirmek için bir istek örnekte düzeltme eki bağımsız değişkenin değeri sağlanan nesne bu özelliği değer vardır: 
  
   * ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
   * ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"
   * (PatchRequest PatchRequest2 olarak). Operations.Count: 1
   * (PatchRequest PatchRequest2 olarak). Operations.ElementAt(0). OperationName: OperationName.Add
   * (PatchRequest PatchRequest2 olarak). Operations.ElementAt(0). Path.AttributePath: "Yönetici"
   * (PatchRequest PatchRequest2 olarak). Operations.ElementAt(0). Value.Count: 1
   * (PatchRequest PatchRequest2 olarak). Operations.ElementAt(0). Value.ElementAt(0). Başvuru: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646
   * (PatchRequest PatchRequest2 olarak). Operations.ElementAt(0). Value.ElementAt(0). Değer: 2819c223-7f76-453a-919d-413861904646

1. Bir kullanıcı bir SCIM hizmeti tarafından fronted kimlik mağazadan sağlamasını için Azure AD gibi bir istek gönderir: 

   ```
     DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
     Authorization: Bearer ...
   ```

   Hizmet SCIM Hizmetleri uygulamak için Microsoft tarafından sağlanan ortak dil altyapısı kitaplıklar kullanılarak oluşturulduysa, isteği hizmet sağlayıcısının silme yöntemine bir çağrı çevrilir.   Bu yöntem, bu imzaya sahip: 

   ```
     // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
     // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
     // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
     System.Threading.Tasks.Task Delete(
       Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
         resourceIdentifier, 
       string correlationIdentifier);
   ```

   Resourceıdentifier bağımsız değişkenin değeri sağlanan nesne bir isteğin kullanıcı sağlamasını örnekte bu özellik değerleri vardır: 

1. Bir kullanıcı bir SCIM hizmeti tarafından fronted kimlik mağazadan sağlamasını için Azure AD gibi bir istek gönderir: 
   ````
    DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
   ````
   Hizmet SCIM Hizmetleri uygulamak için Microsoft tarafından sağlanan CLI kitaplıklar kullanılarak oluşturulduysa, isteği hizmet sağlayıcısının silme yöntemine bir çağrı çevrilir.   Bu yöntem, bu imzaya sahip: 
   ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
    System.Threading.Tasks.Task Delete(
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
        resourceIdentifier, 
      string correlationIdentifier);
   ````
   Resourceıdentifier bağımsız değişkenin değeri sağlanan nesne bir isteğin kullanıcı sağlamasını örnekte bu özellik değerleri vardır: 
  
   * ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
   * ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

## <a name="user-and-group-schema-reference"></a>Kullanıcı ve Grup şema başvurusu
Azure Active Directory kaynakları SCIM web hizmetleri için iki tür sağlayabilirsiniz.  Bu kaynak türleri, kullanıcılar ve gruplar şunlardır.  

Kullanıcı kaynaklarını şema tanımlayıcısı tarafından tanımlanan `urn:ietf:params:scim:schemas:extension:enterprise:2.0:User`, bu protokolü belirtimi dahil: https://tools.ietf.org/html/rfc7643.  Kullanıcıların Azure Active Directory'de kullanıcı kaynaklarını özniteliklerini özniteliklerinin varsayılan eşleme Tablo 1'de sağlanır.  

Grup kaynaklarının şema tanımlayıcısı tarafından tanımlanan `urn:ietf:params:scim:schemas:core:2.0:Group`. Tablo 2 Varsayılan eşleme gruplarının özniteliklerin Azure Active Directory'de Grup kaynak öznitelikleri gösterir.  

### <a name="table-1-default-user-attribute-mapping"></a>Tablo 1: Varsayılan kullanıcı öznitelik eşlemesi

| Azure Active Directory kullanıcısı | "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User" |
| --- | --- |
| IsSoftDeleted |etkin |
| displayName |displayName |
| Faks TelephoneNumber |PhoneNumber [türü eq "faks"] .value |
| givenName |name.givenName |
| İş Unvanı |başlık |
| posta |e-postaları [türü eq "İş"] .value |
| mailNickname |externalId |
| yönetici |yönetici |
| Mobil |PhoneNumber [türü eq "mobile"] .value |
| objectId |Kimlik |
| posta kodu |adresler [türü eq "İş"] .postalCode |
| Ara sunucu adresleri |e-postaları [type eq "diğer"]. Değer |
| fiziksel teslim OfficeName |adresler [type eq "diğer"]. Biçimlendirilmiş |
| streetAddress |adresler [türü eq "İş"] .streetAddress |
| Soyadı |name.familyName |
| telefon numarası |PhoneNumber [türü eq "İş"] .value |
| Kullanıcı PrincipalName |Kullanıcı adı |

### <a name="table-2-default-group-attribute-mapping"></a>Tablo 2: Varsayılan grubu öznitelik eşlemesi

| Azure Active Directory grubu | urn:ietf:params:scim:schemas:core:2.0:Group |
| --- | --- |
| displayName |externalId |
| posta |e-postaları [türü eq "İş"] .value |
| mailNickname |displayName |
| üyeler |üyeler |
| objectId |Kimlik |
| proxyAddresses |e-postaları [type eq "diğer"]. Değer |


## <a name="related-articles"></a>İlgili makaleler
* [Kullanıcı sağlama/sağlamayı kaldırma SaaS uygulamaları için otomatik hale getirin](user-provisioning.md)
* [Kullanıcı sağlama için öznitelik eşlemelerini özelleştirme](customize-application-attributes.md)
* [Öznitelik Eşlemeleri için İfadeler Yazma](functions-for-customizing-application-data.md)
* [Kullanıcı sağlama için kapsam oluşturma filtresi](define-conditional-rules-for-provisioning-user-accounts.md)
* [Hesap Sağlama Bildirimleri](user-provisioning.md)
* [SaaS Uygulamalarını Tümleştirme Hakkında Öğreticiler Listesi](../saas-apps/tutorial-list.md)

<!--Image references-->
[0]: ./media/use-scim-to-provision-users-and-groups/scim-figure-1.png
[1]: ./media/use-scim-to-provision-users-and-groups/scim-figure-2a.png
[2]: ./media/use-scim-to-provision-users-and-groups/scim-figure-2b.png
[3]: ./media/use-scim-to-provision-users-and-groups/scim-figure-3.png
[4]: ./media/use-scim-to-provision-users-and-groups/scim-figure-4.png
[5]: ./media/use-scim-to-provision-users-and-groups/scim-figure-5.png
