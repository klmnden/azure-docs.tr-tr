---
title: Uygulamanızı Azure Active Directory B2C için bir erişim belirteci ile özel bir ilke geçirin | Microsoft Docs
description: Nasıl bir erişim belirteci OAuth2.0 kimlik sağlayıcıları için özel bir ilke talebi olarak uygulamanıza Azure Active Directory B2C'de geçirebilirsiniz öğrenin.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 784387b119bff6445015419adfd3bc0e52eee43f
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58402652"
---
# <a name="pass-an-access-token-through-a-custom-policy-to-your-application-in-azure-active-directory-b2c"></a>Uygulamanızı Azure Active Directory B2C için bir erişim belirteci ile özel bir ilke geçirin

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

A [özel ilke](active-directory-b2c-get-started-custom.md) Azure Active Directory (Azure AD) B2C'de, uygulamanızın kullanıcıları ıntune'a kaydolma veya bir kimlik sağlayıcısı ile oturum açmak için bir fırsat sağlar. Bu durumda, Azure AD B2C alır bir [erişim belirteci](active-directory-b2c-reference-tokens.md) kimlik sağlayıcısından gelen. Azure AD B2C kullanıcı hakkında bilgi almak için bu belirteci kullanır. Belirteci aracılığıyla Azure AD B2C'de kayıt uygulamaları geçirmek için özel ilkeniz için bir talep türü ve çıkış talep ekleyin. 

Azure AD B2C'yi destekler erişim belirtecini geçirme [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) ve [Openıd Connect](active-directory-b2c-reference-oidc.md) kimlik sağlayıcıları. Diğer tüm kimlik sağlayıcıları için talep boş olarak döndürülür.

## <a name="prerequisites"></a>Önkoşullar

- Özel ilkeniz, OAuth 2.0 veya Openıd Connect kimlik sağlayıcısı ile yapılandırılır.

## <a name="add-the-claim-elements"></a>Talep öğeleri Ekle 

1. Açık, *TrustframeworkExtensions.xml* dosyasını açıp aşağıdaki **ClaimType** tanımlayıcısına sahip öğe `identityProviderAccessToken` için **ClaimsSchema** öğesi:

    ```XML
    <BuildingBlocks>
      <ClaimsSchema>
        <ClaimType Id="identityProviderAccessToken">
          <DisplayName>Identity Provider Access Token</DisplayName>
          <DataType>string</DataType>
          <AdminHelpText>Stores the access token of the identity provider.</AdminHelpText>
        </ClaimType>
        ...
      </ClaimsSchema>
    </BuildingBlocks>
    ```

2. Ekleme **OutputClaim** öğesine **TechnicalProfile** için erişim belirteci istediğiniz her bir OAuth 2.0 kimlik sağlayıcısı için öğesi. Aşağıdaki örnek Facebook teknik profiline eklenen öğe gösterir:

    ```XML
    <ClaimsProvider>
      <DisplayName>Facebook</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Facebook-OAUTH">
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="identityProviderAccessToken" PartnerClaimType="{oauth2:access_token}" />
          </OutputClaims>
          ...
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

3. Kaydet *TrustframeworkExtensions.xml* dosya.
4. Gibi bağlı olan taraf ilke dosyanızı açın *SignUpOrSignIn.xml*ve ekleme **OutputClaim** öğesine **TechnicalProfile**:

    ```XML
    <RelyingParty>
      <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
      <TechnicalProfile Id="PolicyProfile">
        <OutputClaims>
          <OutputClaim ClaimTypeReferenceId="identityProviderAccessToken" PartnerClaimType="idp_access_token"/>
        </OutputClaims>
        ...
      </TechnicalProfile>
    </RelyingParty>
    ```

5. İlke dosyasını kaydedin.

## <a name="test-your-policy"></a>İlkenizi test

Uygulamalarınızı Azure AD B2C'de test etme, döndürülen Azure AD B2C belirteci sahip olmak yararlı olabilir `https://jwt.ms` Taleplerde gözden geçirebilmek için.

### <a name="upload-the-files"></a>Dosyaları karşıya yükleme

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
4. Seçin **kimlik deneyimi çerçevesi**.
5. Özel ilkeler sayfasında tıklayın **karşıya yükleme İlkesi**.
6. Seçin **ilke varsa üzerine**, arayın ve seçin *TrustframeworkExtensions.xml* dosya.
7. **Karşıya Yükle**'ye tıklayın.
8. 5. adım-7 gibi bağlı olan taraf dosyası için yineleyin *SignUpOrSignIn.xml*.

### <a name="run-the-policy"></a>İlkeyi çalıştır

1. Değiştirdiğiniz İlkesi'ni açın. Örneğin, *B2C_1A_signup_signin*.
2. İçin **uygulama**, daha önce kaydettiğiniz uygulamanızı seçin. Aşağıdaki örnekte, belirteci görmek için **yanıt URL'si** göstermelidir `https://jwt.ms`.
3. **Şimdi çalıştır**’a tıklayın.

    Aşağıdaki örneğe benzer bir şey görmeniz gerekir:

    ![Kodu çözülen belirteci](./media/idp-pass-through-custom/idp-pass-through-custom-token.png)

## <a name="next-steps"></a>Sonraki adımlar

Belirteçleri hakkında daha fazla bilgi [Azure Active Directory belirteci başvurusu](active-directory-b2c-reference-tokens.md).





