---
title: Azure Active Directory B2C'de özel ilkeleri kullanarak bir Amazon hesabıyla oturum açma özelliğini ayarlamak | Microsoft Docs
description: Özel ilkeleri kullanarak Azure Active Directory B2C Amazon hesap ile oturum açma ayarlayın.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/05/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 46b58aad8a5cb71744aca9baaa3a27d4d1efe8e2
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67655249"
---
# <a name="set-up-sign-in-with-an-amazon-account-using-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de özel ilkeleri kullanarak bir Amazon hesabıyla oturum açma özelliğini ayarlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede bir Amazon hesaptan kullanıcılar için oturum açma kullanarak etkinleştirmek gösterilmektedir [özel ilkeler](active-directory-b2c-overview-custom.md) Azure Active Directory (Azure AD) B2C'de.

## <a name="prerequisites"></a>Önkoşullar

- Bölümündeki adımları tamamlamanız [özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md).
- Bir Amazon hesabınız yoksa, tek tek oluşturun [ https://www.amazon.com/ ](https://www.amazon.com/).

## <a name="register-the-application"></a>Uygulamayı kaydetme

Bir Amazon hesaptan kullanıcılar için oturum açma etkinleştirmek için bir Amazon uygulama oluşturmanız gerekir.

1. Oturum [Amazon Geliştirici Merkezi](https://login.amazon.com/) Amazon hesabınızın kimlik bilgileriyle.
2. Zaten yapmadıysanız, tıklayın **Kaydol**, geliştirici kayıt adımları izleyin ve ilkeyi kabul edin.
3. Seçin **yeni uygulamayı kaydedin**.
4. Girin bir **adı**, **açıklama**, ve **gizlilik bildirimi URL'si**ve ardından **Kaydet**. Gizlilik bildirimi, gizlilik bilgileri kullanıcılara sağlar, yönettiğiniz bir sayfadır.
5. İçinde **Web ayarları** bölümünde, değerlerini kopyalamayı **istemci kimliği**. Seçin **Göster gizli** istemci gizli anahtarı alın ve bunu kopyalayın. Her ikisi de kiracınızdaki bir kimlik sağlayıcısı Amazon hesabı yapılandırmak için gerekir. **İstemci gizli anahtarı** bir önemli güvenlik kimlik bilgisidir.
6. İçinde **Web ayarları** bölümünden **Düzenle**ve enter `https://your-tenant-name.b2clogin.com` içinde **izin JavaScript kaynakları** ve `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` içinde **izin verildi URL'leri dönüş**. Değiştirin `your-tenant-name` kiracınızın ada sahip. Azure AD B2C kodunda büyük harfler ile Kiracı tanımlansa bile Kiracı adınızın girerken tamamen küçük harf kullanın.
7. **Kaydet**’e tıklayın.

## <a name="create-a-policy-key"></a>İlke anahtarı oluşturma

Azure AD B2C kiracınıza daha önce kaydettiğiniz istemci gizli anahtarı depolamak gerekir.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
4. Genel bakış sayfasında **kimlik deneyimi çerçevesi**.
5. Seçin **ilke anahtarları** seçip **Ekle**.
6. İçin **seçenekleri**, seçin `Manual`.
7. Girin bir **adı** ilke anahtarı. Örneğin: `AmazonSecret`. Önek `B2C_1A_` anahtarınızı adına otomatik olarak eklenir.
8. İçinde **gizli**, daha önce kaydettiğiniz istemci gizli anahtarı girin.
9. İçin **anahtar kullanımı**seçin `Signature`.
10.           **Oluştur**'a tıklayın.

## <a name="add-a-claims-provider"></a>Bir talep Sağlayıcı Ekle

Bir Amazon hesabıyla oturum açmasını istiyorsanız, Azure AD B2C'yi bir uç nokta ile iletişim kurabilen bir talep sağlayıcısı olarak hesabı tanımlamanız gerekir. Uç nokta, Azure AD B2C tarafından belirli bir kullanıcı yapıldığını doğrulamak için kullanılan bir talepler kümesi sağlar.

Bir talep sağlayıcısı olarak ekleyerek bir Amazon hesap tanımlayabilirsiniz **ClaimsProviders** ilkenizin uzantısı dosyasında öğe.


1. Açık *TrustFrameworkExtensions.xml*.
2. Bulma **ClaimsProviders** öğesi. Yoksa, kök öğe altında ekleyin.
3. Yeni bir **ClaimsProvider** gibi:

    ```xml
    <ClaimsProvider>
      <Domain>amazon.com</Domain>
      <DisplayName>Amazon</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Amazon-OAUTH">
        <DisplayName>Amazon</DisplayName>
        <Protocol Name="OAuth2" />
        <Metadata>
          <Item Key="ProviderName">amazon</Item>
          <Item Key="authorization_endpoint">https://www.amazon.com/ap/oa</Item>
          <Item Key="AccessTokenEndpoint">https://api.amazon.com/auth/o2/token</Item>
          <Item Key="ClaimsEndpoint">https://api.amazon.com/user/profile</Item>
          <Item Key="scope">profile</Item>
          <Item Key="HttpBinding">POST</Item>
          <Item Key="UsePolicyInRedirectUri">0</Item>
          <Item Key="client_id">Your Amazon application client ID</Item>
        </Metadata>
        <CryptographicKeys>
          <Key Id="client_secret" StorageReferenceId="B2C_1A_AmazonSecret" />
        </CryptographicKeys>
        <OutputClaims>
          <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="user_id" />
          <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
          <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
          <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="amazon.com" />
          <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
        </OutputClaims>
          <OutputClaimsTransformations>
          <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
          <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
          <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
        </OutputClaimsTransformations>
        <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

4. Ayarlama **client_id** için uygulama kaydı uygulama kimliği.
5. Dosyayı kaydedin.

### <a name="upload-the-extension-file-for-verification"></a>Doğrulama için uzantı dosyasını karşıya yükle

Artık Azure AD B2C, Azure AD dizini ile iletişim kurma bilebilmesi ilkenizi yapılandırdınız. İlkenizin yalnızca bu sorunları şu ana kadar sahip olmadığını onaylamak için uzantı dosyasını karşıya yüklemeyi deneyin.

1. Üzerinde **özel ilkeleri** sayfa seçin, Azure AD B2C kiracınızın **karşıya yükleme İlkesi**.
2. Etkinleştirme **ilke varsa üzerine**, ardından gözatın ve seçin *TrustFrameworkExtensions.xml* dosya.
3. **Karşıya Yükle**'ye tıklayın.

## <a name="register-the-claims-provider"></a>Talep sağlayıcısı kaydetme

Bu noktada, kimlik sağlayıcısı ayarlandı, ancak oturumu-kaydolma/oturum açma ekranları hiçbirinde kullanılabilir değil. Kullanılabilir hale getirmek için mevcut bir şablonu kullanıcı yolculuğu bir kopyasını oluşturun ve ayrıca Amazon kimlik sağlayıcısına sahip olacak şekilde değiştirin.

1. Açık *TrustFrameworkBase.xml* başlangıç paketi dosyasından.
2. Bul ve tüm içeriğini kopyalayın **UserJourney** içeren öğe `Id="SignUpOrSignIn"`.
3. Açık *TrustFrameworkExtensions.xml* ve bulma **UserJourneys** öğesi. Öğe yoksa bir tane ekleyin.
4. Tüm içeriğini yapıştırın **UserJourney** öğesi alt öğesi olarak kopyaladığınız **UserJourneys** öğesi.
5. Kullanıcı yolculuğu kimliği yeniden adlandırın. Örneğin: `SignUpSignInAmazon`.

### <a name="display-the-button"></a>Bir düğme görüntülemek

**ClaimsProviderSelection** öğedir oturumu-kaydolma/oturum açma ekranı bir kimlik sağlayıcısının düğmesine benzer. Eklerseniz bir **ClaimsProviderSelection** öğesi Amazon hesabınız için yeni bir düğme gösterilir sayfasında bir kullanıcı gölünüzdeki olduğunda.

1. Bulma **OrchestrationStep** içeren öğe `Order="1"` , oluşturduğunuz kullanıcı giden.
2. Altında **ClaimsProviderSelects**, şu öğeyi ekleyin. Değerini **TargetClaimsExchangeId** örneğin uygun bir değere `AmazonExchange`:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="AmazonExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Düğme için bir eylem ile bağlantı kurun.

Yerinde bir düğmeye sahip olduğunuza göre bir eyleme bağlamanız gerekir. Eylem, bu durumda, bir belirteç almak için bir Amazon hesabıyla iletişim kurmak için Azure AD B2C içindir.

1. Bulma **OrchestrationStep** içeren `Order="2"` kullanıcı giden.
2. Aşağıdaki **ClaimsExchange** öğesi için kullanılan kimliği için aynı değeri kullanın sağlamaktan **TargetClaimsExchangeId**:

    ```XML
    <ClaimsExchange Id="AmazonExchange" TechnicalProfileReferenceId="Amazon-OAuth" />
    ```

    Değerini güncelleştirin **TechnicalProfileReferenceId** daha önce oluşturduğunuz teknik profil kimliği. Örneğin: `Amazon-OAuth`.

3. Kaydet *TrustFrameworkExtensions.xml* dosya ve doğrulama için yeniden yükleyin.

## <a name="create-an-azure-ad-b2c-application"></a>Azure AD B2C'yi uygulama oluşturma

Azure AD B2c ile iletişim kiracınızda oluşturduğunuz bir uygulama üzerinden gerçekleşir. Bu bölümde zaten yapmadıysanız, bir test uygulaması oluşturmak için tamamlayabilirsiniz isteğe bağlı adımlar listelenir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
4. Seçin **uygulamaları**ve ardından **Ekle**.
5. Uygulama için bir ad girin, örneğin *testapp1*.
6. İçin **Web uygulaması / Web API'sini**seçin `Yes`yazıp enter `https://jwt.ms` için **yanıt URL'si**.
7.           **Oluştur**'a tıklayın.

## <a name="update-and-test-the-relying-party-file"></a>Güncelleştirme ve bağlı olan taraf dosyayı test etme

Oluşturduğunuz kullanıcı yolculuğu başlatır bağlı olan taraf (RP) dosyasını güncelleştirin.

1. Bir kopyasını *SignUpOrSignIn.xml* çalışma dizininizdeki ve yeniden adlandırın. Örneğin, yeniden adlandırın *SignUpSignInAmazon.xml*.
2. Yeni dosyayı açın ve değeri güncelleştirme **Policyıd** özniteliğini **TrustFrameworkPolicy** benzersiz bir değere sahip. Örneğin: `SignUpSignInAmazon`.
3. Değerini güncelleştirin **PublicPolicyUri** ilkesi için URI ile. Örneğin,`http://contoso.com/B2C_1A_signup_signin_amazon`
4. Değerini güncelleştirin **Referenceıd** özniteliğini **DefaultUserJourney** (SignUpSignAmazon) oluşturduğunuz yeni kullanıcı yolculuğu kimliği eşleştirmek için.
5. Yaptığınız değişiklikleri kaydedin, dosyayı karşıya yükleyin ve ardından listeden yeni ilkeyi seçin.
6. İçinde oluşturduğunuz bir Azure AD B2C uygulamasını seçili olduğundan emin olun **uygulama seçin** alan ve ardından tıklayarak test **Şimdi Çalıştır**.
