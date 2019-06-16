---
title: Azure Active Directory B2C'de özel ilkeleri kullanarak parola karmaşıklığını yapılandırma | Microsoft Docs
description: Nasıl özel bir ilke kullanarak Azure Active Directory B2C'de parola karmaşıklık gereksinimlerini yapılandırabilirsiniz.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 04a37e6faf51787457d7ca4ab8434fd253deb2ed
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66509153"
---
# <a name="configure-password-complexity-using-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de özel ilkeleri kullanarak parola karmaşıklığını yapılandırın

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory (Azure AD) B2C'de bir hesabı oluştururken bir kullanıcı tarafından sağlanan parola karmaşıklık gereksinimlerini yapılandırabilirsiniz. Varsayılan olarak, Azure AD B2C kullanır **güçlü** parola. Bu makalede, parola karmaşıklığını yapılandırma işlemi gösterilmektedir [özel ilkeler](active-directory-b2c-overview-custom.md). Parola karmaşıklığı yapılandırmak da mümkündür [kullanıcı akışları](active-directory-b2c-reference-password-complexity.md).

## <a name="prerequisites"></a>Önkoşullar

Bölümündeki adımları tamamlamanız [Active Directory B2C özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md).

## <a name="add-the-elements"></a>Öğeleri Ekle

1. Kopyalama *SignUpOrSignIn.xml* ile başlangıç paketi indirilir ve adlandırın dosya *SingUpOrSignInPasswordComplexity.xml*.
2. Açık *SingUpOrSignInPasswordComplexity.xml* dosya ve değiştirme **Policyıd** ve **PublicPolicyUri** için yeni bir ilke adı. Örneğin, *B2C_1A_signup_signin_password_complexity*.
3. Aşağıdaki **ClaimType** tanımlayıcıların öğelerle `newPassword` ve `reenterPassword`:

    ```XML
    <ClaimsSchema>
      <ClaimType Id="newPassword">
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
      <ClaimType Id="reenterPassword">
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
    </ClaimsSchema>
    ```

4. [Doğrulamaları](predicates.md) , yöntem türleri `IsLengthRange` veya `MatchesRegex`. `MatchesRegex` Türü, normal bir ifadeyle eşleşen için kullanılır. `IsLengthRange` Türünü alan bir minimum ve maksimum dize uzunluğu. Ekleme bir **doğrulamaları** öğesine **BuildingBlocks** aşağıdakilerle yoksa öğe **koşul** öğeleri:

    ```XML
    <Predicates>
      <Predicate Id="PIN" Method="MatchesRegex" HelpText="The password must be a pin.">
        <Parameters>
          <Parameter Id="RegularExpression">^[0-9]+$</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Length" Method="IsLengthRange" HelpText="The password must be between 8 and 16 characters.">
        <Parameters>
          <Parameter Id="Minimum">8</Parameter>
          <Parameter Id="Maximum">16</Parameter>
        </Parameters>
      </Predicate>
    </Predicates>
    ```

5. Her **InputValidation** öğesi tanımlı kullanarak oluşturulan **koşul** öğeleri. Bu öğe benzer Boole toplamalar gerçekleştirmenize olanak tanıyan `and` ve `or`. Ekleme bir **InputValidations** öğesine **BuildingBlocks** aşağıdakilerle yoksa öğe **InputValidation** öğesi:

    ```XML
    <InputValidations>
      <InputValidation Id="PasswordValidation">
        <PredicateReferences Id="LengthGroup" MatchAtLeast="1">
          <PredicateReference Id="Length" />
        </PredicateReferences>
        <PredicateReferences Id="3of4" MatchAtLeast="3" HelpText="You must have at least 3 of the following character classes:">
          <PredicateReference Id="Lowercase" />
          <PredicateReference Id="Uppercase" />
          <PredicateReference Id="Number" />
          <PredicateReference Id="Symbol" />
        </PredicateReferences>
      </InputValidation>
    </InputValidations>
    ```

6. Emin olun **PolicyProfile** teknik profili, aşağıdaki öğeleri içerir:

    ```XML
    <RelyingParty>
      <DefaultUserJourney ReferenceId="SignUpOrSignIn"/>
      <TechnicalProfile Id="PolicyProfile">
        <DisplayName>PolicyProfile</DisplayName>
        <Protocol Name="OpenIdConnect"/>
        <InputClaims>
          <InputClaim ClaimTypeReferenceId="passwordPolicies" DefaultValue="DisablePasswordExpiration, DisableStrongPassword"/>
        </InputClaims>
        <OutputClaims>
          <OutputClaim ClaimTypeReferenceId="displayName"/>
          <OutputClaim ClaimTypeReferenceId="givenName"/>
          <OutputClaim ClaimTypeReferenceId="surname"/>
          <OutputClaim ClaimTypeReferenceId="email"/>
          <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
        </OutputClaims>
        <SubjectNamingInfo ClaimType="sub"/>
      </TechnicalProfile>
    </RelyingParty>
    ```

7. İlke dosyasını kaydedin.

## <a name="test-your-policy"></a>İlkenizi test

Uygulamalarınızı Azure AD B2C'de test etme, döndürülen Azure AD B2C belirteci sahip olmak yararlı olabilir `https://jwt.ms` Taleplerde gözden geçirebilmek için.

### <a name="upload-the-files"></a>Dosyaları karşıya yükleme

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
4. Seçin **kimlik deneyimi çerçevesi**.
5. Özel ilkeler sayfasında tıklayın **karşıya yükleme İlkesi**.
6. Seçin **ilke varsa üzerine**, arayın ve seçin *SingUpOrSignInPasswordComplexity.xml* dosya.
7. **Karşıya Yükle**'ye tıklayın.

### <a name="run-the-policy"></a>İlke çalıştırın

1. Değiştirdiğiniz İlkesi'ni açın. Örneğin, *B2C_1A_signup_signin_password_complexity*.
2. İçin **uygulama**, daha önce kaydettiğiniz uygulamanızı seçin. Belirteç görmek için **yanıt URL'si** göstermelidir `https://jwt.ms`.
3. **Şimdi çalıştır**’a tıklayın.
4. Seçin **şimdi kaydolun**, bir e-posta adresi girin ve yeni bir parola girin. Yönergeler, parola kısıtlamaları sunulur. Kullanıcı bilgilerini girmeyi tamamladığınızda ve ardından **Oluştur**. Döndürülen belirteç içeriğini görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi nasıl [Azure Active Directory B2C'de özel ilkeleri kullanarak parola değişikliği yapılandırma](active-directory-b2c-reference-password-change-custom.md).


