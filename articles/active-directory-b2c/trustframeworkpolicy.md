---
title: TrustFrameworkPolicy - Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C'de özel bir ilkenin TrustFrameworkPolicy öğesi belirtin.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 558e9c3a3bfd43f6ceb958bc3be55d58e1eb7f91
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60360495"
---
# <a name="trustframeworkpolicy"></a>TrustFrameworkPolicy

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Özel bir ilke birbirleriyle hiyerarşik bir zincirinde başvuran bir veya daha fazla XML biçimli dosyaları olarak temsil edilir. XML öğeleri talep şema, talep dönüştürmeleri, içerik tanımları, talep sağlayıcıları, teknik profiller, kullanıcı yolculuğu ve düzenleme adımlarının gibi ilke öğelerini tanımlayın. Her ilke dosyasını en üst düzey içinde tanımlanan **TrustFrameworkPolicy** bir ilke dosyası öğesidir. 

```XML
<TrustFrameworkPolicy
  xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
  xmlns:xsd="https://www.w3.org/2001/XMLSchema"
  xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
  PolicySchemaVersion="0.3.0.0"
  TenantId="mytenant.onmicrosoft.com"
  PolicyId="B2C_1A_TrustFrameworkBase"
  PublicPolicyUri="http://mytenant.onmicrosoft.com/B2C_1A_TrustFrameworkBase">
  ...
```


**TrustFrameworkPolicy** öğesi aşağıdaki öznitelikler içerir:

| Öznitelik | Gerekli | Açıklama |
|---------- | -------- | ----------- |
| PolicySchemaVersion | Evet | İlke yürütmek için kullanılacak olan şema sürümü. Değer olmalıdır `0.3.0.0` |
| TenantObjectId | Hayır | Azure Active Directory (Azure AD) B2C kiracısının benzersiz nesne tanımlayıcısı. |
| TenantId | Evet | Bu ilkenin ait olduğu kiracının benzersiz tanımlayıcısı. |
| PolicyId | Evet | İlke için benzersiz tanımlayıcı. Bu tanımlayıcı getirilmeli *B2C_1A_* |
| PublicPolicyUri | Evet | Kiracı kimliği ve ilke kimliğinin birleşimi İlkesi URI'si |
| DeploymentMode | Hayır | Olası değerler: `Production`, `Debugging`, veya `Development`. `Production` varsayılan değerdir. İlkenizi hata ayıklamak için bu özelliği kullanın. Daha fazla bilgi için [günlüklerini toplama](active-directory-b2c-troubleshoot-custom.md). |
| UserJourneyRecorderEndpoint | Hayır | Uç noktası yüklendiğinde **DeploymentMode** ayarlanır `Development`. Değer olmalıdır `urn:journeyrecorder:applicationinsights`. Daha fazla bilgi için [günlüklerini toplama](active-directory-b2c-troubleshoot-custom.md). |


Aşağıdaki örnek nasıl belirtileceğini gösterir **TrustFrameworkPolicy** öğesi:

``` XML
<TrustFrameworkPolicy
   xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
   xmlns:xsd="https://www.w3.org/2001/XMLSchema"
   xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
   PolicySchemaVersion="0.3.0.0"
   TenantId="mytenant.onmicrosoft.com"
   PolicyId="B2C_1A_TrustFrameworkBase"
   PublicPolicyUri="http://mytenant.onmicrosoft.com/B2C_1A_TrustFrameworkBase">
```

## <a name="inheritance-model"></a>Devralma modeli

Bu tür bir ilke dosyası genellikle bir kullanıcı yolculuğunda kullanılır:

- A **temel** tanımları çoğunu içeren dosya. İlkelerinizi sorun giderme ve uzun süreli bakım ile yardımcı olmak için bu dosyada en az bir dizi değişiklik yaptığınız önerilir.
- Bir **uzantıları** kiracınız için benzersiz yapılandırma değişikliklerini içeren dosya. Bu ilke dosyası, temel dosyanın elde edilir. Yeni işlevler eklemek veya mevcut bir işlevi geçersiz kılma için bu dosyayı kullanın. Örneğin, yeni kimlik sağlayıcıları ile federasyona eklemek için bu dosyayı kullanın.
- A **bağlı olan taraf (RP)** dosya doğrudan bağlı taraf uygulaması, web, mobil veya Masaüstü uygulamaları gibi tarafından çağrılan tek görev odaklı dosyası. Kaydolma veya oturum açma parolasını sıfırlama veya profil düzenleme gibi benzersiz her görev kendi RP ilkesi dosyası gerektirir. Bu ilke dosyası uzantıları dosyasından elde edilir. 

Bir bağlı taraf uygulaması, RP ilke dosyası, belirli bir görevi çağırır. Örneğin, oturum açma akışını başlatmak için. Azure AD B2C'de kimlik deneyimi çerçevesi için tüm öğelere ilk temel dosyasından ve uzantıları dosyasından ekler ve son olarak geçerli ilke etkin derlemek için RP ilke dosyasından. Öğeleri aynı türde ve RP dosya adı uzantıları'nda bu öğeleri geçersiz kılabilir ve uzantıları temel geçersiz kılar. Aşağıdaki diyagramda, ilke dosyalarını ve bağlı olan taraf uygulamaları arasındaki ilişkiyi gösterir.

![Devralma modeli](./media/trustframeworkpolicy/custom-policy-Inheritance-model.png)

Devralma modeli aşağıdaki gibidir:

- Aynı şemasını ilke üst ve alt ilke var.
- Alt ilke herhangi bir düzeyde, üst ilkeden devralır ve genişletebileceğinizi yeni öğeler ekleyerek.
- Düzey sayısına bir sınır yoktur.

Daha fazla bilgi için [özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md).

## <a name="base-policy"></a>Temel ilke

Bir ilke başka bir ilkeden devralmak için bir **BasePolicy** öğesi bildirilmiş altında **TrustFrameworkPolicy** ilke öğesi. **BasePolicy** öğedir, bu ilke türetilmiştir temel ilke için bir başvuru.  

**BasePolicy** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşumlar | Açıklama |
| ------- | ----------- | --------|
| TenantId | 1:1 | Azure AD B2C kiracınızı tanımlayıcısı. |
| PolicyId | 1:1 | Üst ilke tanımlayıcısı. |


Aşağıdaki örnek, temel bir ilke belirtmek gösterilmektedir. Bu **B2C_1A_TrustFrameworkExtensions** İlkesi türetilen **B2C_1A_TrustFrameworkBase** ilkesi. 

``` XML
<TrustFrameworkPolicy
   xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
   xmlns:xsd="https://www.w3.org/2001/XMLSchema"
   xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
   PolicySchemaVersion="0.3.0.0"
   TenantId="mytenant.onmicrosoft.com"
   PolicyId="B2C_1A_TrustFrameworkExtensions"
   PublicPolicyUri="http://mytenant.onmicrosoft.com/B2C_1A_TrustFrameworkExtensions">

  <BasePolicy>
    <TenantId>yourtenant.onmicrosoft.com</TenantId>
    <PolicyId>B2C_1A_TrustFrameworkBase</PolicyId>
  </BasePolicy>
  ...
</TrustFrameworkPolicy>
```

## <a name="policy-execution"></a>Yürütme İlkesi

Bir web, mobil veya masaüstü uygulaması gibi bir bağlı taraf uygulaması çağırır [bağlı olan taraf (RP) İlkesi](relyingparty.md). RP ilke dosyasını açarken, parola sıfırlama veya bir profil düzenleme gibi belirli bir görevi yürütür. RP ilke bağlı taraf uygulaması alan talepler listesinin verilen belirtecinin bir parçası yapılandırır. Birden çok uygulama aynı ilkesini kullanabilirsiniz. Tüm uygulamaları talepleri ile aynı belirteci alan ve aynı kullanıcı gezintisinde kullanıcı geçer. Tek bir uygulama birden çok ilke kullanabilirsiniz.

RP ilke dosyası içinde belirttiğiniz **DefaultUserJourney** işaret öğesi [UserJourney](userjourneys.md). Kullanıcı yolculuğu genellikle temel veya uzantıları ilkesinde tanımlanır.

B2C_1A_signup_signin İlkesi:

```XML
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn">
  ...
```

B2C_1A_TrustFrameWorkBase veya B2C_1A_TrustFrameworkExtensionPolicy:

```XML
<UserJourneys>
  <UserJourney Id="SignOrSignIn">
  ...
```

Kullanıcı yolculuğu bir kullanıcı geçer, iş mantığı tanımlar. Her kullanıcı yolculuğu kimlik doğrulaması ve bilgi toplama açısından dizisindeki bir eylemler dizisi gerçekleştirir düzenleme adımlarının kümesidir. 

**SocialAndLocalAccounts** ilke dosyasında [başlangıç paketi](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-get-started-custom#download-starter-pack-and-modify-policies) SignUpOrSignIn, ProfileEdit, PasswordReset kullanıcı yolculuklarından içerir. Bir e-posta adresini değiştirmek gibi başka bir senaryo için daha fazla kullanıcı yolculuklarından bağlamak ve bir sosyal hesap veya parola sıfırlama bağlantısını ekleyebilirsiniz. 

Düzenleme adımlarının çağırabilir bir [teknik profil](technicalprofiles.md). Teknik profili, farklı türlerde taraflar ile iletişim kurmak için yerleşik bir mekanizma ile bir çerçeve sunar. Örneğin, bir teknik profili, diğerlerinin yanı sıra bu eylemleri gerçekleştirebilirsiniz:

- Bir kullanıcı deneyimi işleyin.
- Sosyal veya Kurumsal hesabı, facebook gibi Microsoft hesabı, Google, Salesforce veya başka bir kimlik sağlayıcısı oturum açmasına imkan tanıyın.
- Telefon doğrulama için mfa'yı ayarlayın.
- Okuma ve bir Azure AD B2C kimlik deposu gelen ve giden veri yazma.
- Özel bir Restful API hizmetini çağırın.

![Yürütme İlkesi](./media/trustframeworkpolicy/custom-policy-execution.png)

 **TrustFrameworkPolicy** öğesi aşağıdaki öğeleri içerir:

- Yukarıda belirtildiği gibi BasePolicy
- [BuildingBlocks](buildingblocks.md)
- [ClaimsProviders](claimsproviders.md)
- [UserJourneys](userjourneys.md)
- [RelyingParty](relyingparty.md)

