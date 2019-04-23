---
title: UserJourneys | Microsoft Docs
description: Azure Active Directory B2C'de özel bir ilke UserJourneys öğesi belirtin.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: ccc1f94b9411a158b5c60509e09bd3edc0a61640
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59795174"
---
# <a name="userjourneys"></a>UserJourneys

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Kullanıcı yolculuklarından bir ilke bir kullanıcı için istenen talep elde etmek bir bağlı taraf uygulaması olanak tanıyan açık yollarını belirtin. Kullanıcının bağlı olan taraf için sunulacak olan talepleri almak için bu yollar alınır. Diğer bir deyişle, bir son kullanıcı Azure AD B2C kimlik deneyimi çerçevesi işlemleri isteği geçer, iş mantığı kullanıcı yolculuklarından tanımlayın.

Bu kullanıcı yolculuklarından ilgi topluluğuna çeşitli yanıtlama tarafların çekirdek gereksinimini karşılamak kullanılabilir şablonları kabul edilebilir. Kullanıcı yolculuklardan, bağlı olan taraf bölümü bir ilke tanımı kolaylaştırır. Bir ilke birden çok kullanıcı yolculuklarından tanımlayabilirsiniz. Her kullanıcı yolculuğunda düzenleme adımlarının dizisidir.

İlke tarafından desteklenen kullanıcı yolculuklarından tanımlamak için bir **UserJourneys** öğesi ilkesi dosyasının üst düzey öğesi altında eklenir. 

**UserJourneys** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşumlar | Açıklama |
| ------- | ----------- | ----------- |
| UserJourney | 1:n | Kullanıcı yolculuğu tüm yapıları için tam kullanıcı Akış gerekli tanımlar. | 

**UserJourney** öğesi aşağıdaki öznitelik içeriyor:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kimlik | Evet | İlkesinde diğer öğelerden başvuru için kullanılan bir kullanıcı yolculuğu tanımlayıcısı. **DefaultUserJourney** öğesinin [bağlı olan taraf İlkesi](relyingparty.md) bu özniteliğe işaret eder. |

**UserJourney** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşumlar | Açıklama |
| ------- | ----------- | ----------- |
| OrchestrationSteps | 1:n | Başarılı bir işlem gelmelidir düzenleme dizisi. Her kullanıcı yolculuğu sırayla yürütülen düzenleme adımlarının sıralanmış bir listesini içerir. Herhangi bir adım başarısız olursa, işlem başarısız olur. |

## <a name="orchestrationsteps"></a>OrchestrationSteps

Kullanıcı yolculuğu için başarılı bir işlem gelmelidir düzenleme dizisi olarak temsil edilir. Herhangi bir adım başarısız olursa, işlem başarısız olur. Yapı taşlarını düzenleme adımları başvurabilir ve ilke dosyasında talep sağlayıcıları izin. Göstermek veya bir kullanıcı deneyimi oluşturmak için sorumludur herhangi bir düzenleme adımı, ayrıca ilgili içerik tanımı tanımlayıcısı bir başvuru içeriyor.

Düzenleme adımlarının koşullu olarak, orchestration adım öğesinde tanımlanan önkoşullara göre çalıştırılabilir. Örneğin, yalnızca belirli bir talep varsa veya bir talebi eşitse veya belirtilen değer için bir düzenleme adımı gerçekleştirmek için denetleyebilirsiniz. 

Düzenleme adımlarının sıralanmış listesini belirtmek için bir **OrchestrationSteps** öğesi ilkesinin bir parçası eklenir. Bu öğe gereklidir.

**OrchestrationSteps** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşumlar | Açıklama |
| ------- | ----------- | ----------- |
| OrchestrationStep | 1:n | Bir sıralı düzenleme adımı. | 

**OrchestrationStep** öğesi aşağıdaki öznitelikler içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| `Order` | Evet | Düzenleme adımlarının sırası. | 
| `Type` | Evet | Düzenleme adımı türü. Olası değerler: <ul><li>**ClaimsProviderSelection** -düzenleme adımı birini seçmek için kullanıcıya çeşitli talep sağlayıcıları sunan gösterir.</li><li>**CombinedSignInAndSignUp** -düzenleme adımı oturum açma ve yerel hesap kaydolma sayfası birleşik sosyal sağlayıcılar sunar gösterir.</li><li>**ClaimsExchange** -düzenleme adımı talepler bir talep sağlayıcı ile değiştirir gösterir.</li><li>**SendClaims** -düzenleme adımı, bir talep veren tarafından verilmiş bir belirteç ile bağlı olan tarafa talep gönderdiğini belirtir.</li></ul> | 
| ContentDefinitionReferenceId | Hayır | Tanımlayıcısını [içerik tanımı](contentdefinitions.md) bu düzenleme adımı ile ilişkili. Çoğunlukla içerik tanım başvurusu tanımlayıcısını otomatik olarak onaylanan teknik profili içinde tanımlanır. Ancak, teknik bir profili olmayan bir şey görüntülemek Azure AD B2C ihtiyacı olduğunda bazı durumlar vardır. Düzenleme adımı türü aşağıdakilerden biri ise iki örnekler verilmiştir: `ClaimsProviderSelection` veya `CombinedSignInAndSignUp`. Teknik profil gerek kalmadan kimlik sağlayıcısı seçim görüntülemek Azure AD B2C gerekir. | 
| CpimIssuerTechnicalProfileReferenceId | Hayır | Düzenleme adımı türü `SendClaims`. Bu özellik, bağlı olan taraf için belirteç veren talep sağlayıcısı teknik profil tanımlayıcısını tanımlar.  Yoksa, bağlı olan taraf belirteci olmadan oluşturulduysa. |


**OrchestrationStep** öğesi, aşağıdaki öğeleri içerebilir:

| Öğe | Oluşumlar | Açıklama |
| ------- | ----------- | ----------- | 
| Önkoşulları | 0: n | Düzenleme adımı yürütmek karşılanması gereken önkoşulları listesi. | 
| ClaimsProviderSelections | 0: n | Düzenleme adımı için talep sağlayıcısı seçim listesi. | 
| ClaimsExchanges | 0: n | Düzenleme adımı için talep değişimleri listesi. | 

### <a name="preconditions"></a>Önkoşulları

**Önkoşulları** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşumlar | Açıklama |
| ------- | ----------- | ----------- | 
| Önkoşulu | 0: n | Kullanılan teknik profil bağlı olarak ya da yönlendiren istemci talep değişimi için bir sunucu çağrısı yapar ve Talep sağlayıcı seçimi göre. | 


#### <a name="precondition"></a>Önkoşulu

**Önkoşulu** öğesi aşağıdaki öznitelik içeriyor:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| `Type` | Evet | Onay veya bu önkoşulu için gerçekleştirmek için sorgu türü. Değer olabilir **ClaimsExist**, kullanıcının geçerli talep kümesinde belirtilen talep varsa eylemlerin gerçekleştirilmesi gereken belirtir veya **ClaimEquals**, hangi belirtir eylemleri Belirtilen talep varsa ve değeri belirtilen değere eşittir gerçekleştirilmelidir. |
| `ExecuteActionsIf` | Evet | True veya false test önkoşuluna eylemleri gerçekleştirilen karar vermek için kullanın. | 

**Önkoşulu** öğeleri aşağıdaki öğeleri içerir:

| Öğe | Oluşumlar | Açıklama |
| ------- | ----------- | ----------- |
| Değer | 1:n | İçin Sorgulanacak ClaimTypeReferenceId. Başka bir değeri öğenin denetlenecek değer içeriyor.</li></ul>|
| Eylem | 1:1 | Önkoşul denetimi içinde bir düzenleme adımı doğru olması durumunda gerçekleştirilmesi gereken eylem. Varsa değerini `Action` ayarlanır `SkipThisOrchestrationStep`, ilişkili `OrchestrationStep` değil yürütülmelidir. | 

#### <a name="preconditions-examples"></a>Önkoşulları örnekleri

Aşağıdaki önkoşullara kullanıcının objectID var olup olmadığını denetler. Kullanıcı yolculuğunda kullanıcının yerel hesabı kullanarak oturum açmanız seçildi. ObjectID varsa, bu düzenleme adımı atlayın.

```XML
<OrchestrationStep Order="2" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
      <Value>objectId</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="FacebookExchange" TechnicalProfileReferenceId="Facebook-OAUTH" />
    <ClaimsExchange Id="SignUpWithLogonEmailExchange" TechnicalProfileReferenceId="LocalAccountSignUpWithLogonEmail" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Aşağıdaki önkoşullara kullanıcı sosyal hesabınızla oturum olup olmadığını denetler. Dizindeki kullanıcı hesabı bulunacak girişiminde yapılır. Kullanıcı oturum açtığında ya da yerel bir hesap ile kaydolduğunda bu düzenleme adımı atlayın.

```XML
<OrchestrationStep Order="3" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
      <Value>authenticationSource</Value>
      <Value>localAccountAuthentication</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="AADUserReadUsingAlternativeSecurityId" TechnicalProfileReferenceId="AAD-UserReadUsingAlternativeSecurityId-NoError" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Önkoşulları birden çok önkoşulları kontrol edebilirsiniz. Aşağıdaki örnek, 'objectID' veya 'e-posta' var olup olmadığını denetler. İlk koşul true ise, Yolculuğunuzun sonraki düzenleme adımı için atlar.

```XML
<OrchestrationStep Order="4" Type="ClaimsExchange">
  <Preconditions>
  <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
      <Value>objectId</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
    <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
      <Value>email</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>      
  <ClaimsExchanges>
    <ClaimsExchange Id="SelfAsserted-SocialEmail" TechnicalProfileReferenceId="SelfAsserted-SocialEmail" />
  </ClaimsExchanges>
</OrchestrationStep>
```

## <a name="claimsproviderselection"></a>ClaimsProviderSelection

Bir düzenleme adımı türü `ClaimsProviderSelection` veya `CombinedSignInAndSignUp` bir kullanıcı ile oturum talep sağlayıcıları listesini içerebilir. Öğelerin sırasını `ClaimsProviderSelections` öğeleri kullanıcıya sunulan kimlik sağlayıcıları sırasını denetler.

**ClaimsProviderSelection** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşumlar | Açıklama |
| ------- | ----------- | ----------- |
| ClaimsProviderSelection | 0: n | Seçilebilir talep sağlayıcıları listesini sağlar.|

**ClaimsProviderSelection** öğesi aşağıdaki öznitelikler içerir: 

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| TargetClaimsExchangeId | Hayır | Talep sağlayıcı seçiminin sonraki düzenleme adımı içinde yürütülen talep değişimi tanımlayıcısı. Bu öznitelik veya ValidationClaimsExchangeId öznitelik belirtilen, ancak ikisi birden değil olmalıdır. | 
| ValidationClaimsExchangeId | Hayır | Talep sağlayıcı seçimi doğrulamak için geçerli düzenleme adımı yürütülür talep değişimi tanımlayıcısı. Bu öznitelik veya TargetClaimsExchangeId öznitelik belirtilen, ancak ikisi birden değil olmalıdır. |

### <a name="claimsproviderselection-example"></a>ClaimsProviderSelection örneği

Aşağıdaki düzenleme adımı, Facebook, LinkedIn, Twitter, Google veya yerel bir hesap açın, oturum açmak kullanıcı seçebilir. Kullanıcı sosyal kimlik sağlayıcıları seçerse, belirtilen seçili talep değişimi ile ikinci düzenleme adımı yürütür `TargetClaimsExchangeId` özniteliği. İkinci düzenleme adımı kullanıcıyı oturum açma işlemini tamamlamak için sosyal kimlik sağlayıcısı yönlendirir. Kullanıcının yerel hesabı ile oturum açmayı seçerse, Azure AD B2C aynı düzenleme adımı (aynı kaydolma sayfası veya oturum açma sayfası) kalır ve ikinci düzenleme adımı atlar.

```XML
<OrchestrationStep Order="1" Type="CombinedSignInAndSignUp" ContentDefinitionReferenceId="api.signuporsignin">
    <ClaimsProviderSelections>
    <ClaimsProviderSelection TargetClaimsExchangeId="FacebookExchange" />
    <ClaimsProviderSelection TargetClaimsExchangeId="LinkedInExchange" />
    <ClaimsProviderSelection TargetClaimsExchangeId="TwitterExchange" />
    <ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
    <ClaimsProviderSelection ValidationClaimsExchangeId="LocalAccountSigninEmailExchange" />
    </ClaimsProviderSelections>
    <ClaimsExchanges>
    <ClaimsExchange Id="LocalAccountSigninEmailExchange" 
                    TechnicalProfileReferenceId="SelfAsserted-LocalAccountSignin-Email" />
    </ClaimsExchanges>
</OrchestrationStep>


<OrchestrationStep Order="2" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
      <Value>objectId</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="FacebookExchange" TechnicalProfileReferenceId="Facebook-OAUTH" />
    <ClaimsExchange Id="SignUpWithLogonEmailExchange" TechnicalProfileReferenceId="LocalAccountSignUpWithLogonEmail" />
    <ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
    <ClaimsExchange Id="LinkedInExchange" TechnicalProfileReferenceId="LinkedIn-OAUTH" />
    <ClaimsExchange Id="TwitterExchange" TechnicalProfileReferenceId="Twitter-OAUTH1" />
  </ClaimsExchanges>
</OrchestrationStep>
```

## <a name="claimsexchanges"></a>ClaimsExchanges

**ClaimsExchanges** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşumlar | Açıklama |
| ------- | ----------- | ----------- |
| ClaimsExchange | 0: n | Kullanılan teknik profil bağlı olarak ya da yönlendiren istemci göre seçilen ClaimsProviderSelection veya talep değişimi için bir sunucu çağrısı yapar. | 

**ClaimsExchange** öğesi aşağıdaki öznitelikler içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kimlik | Evet | Talep değişimi adım tanımlayıcısı. Tanımlayıcı, Talep sağlayıcı seçimden talep değişimi adım ilkede başvurmak için kullanılır. | 
| TechnicalProfileReferenceId | Evet | Yürütülecek teknik profili tanımlayıcısı. |
