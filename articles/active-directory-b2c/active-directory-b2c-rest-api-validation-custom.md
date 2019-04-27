---
title: REST API, Azure Active Directory B2C doğrulama olarak değişimleri talep | Microsoft Docs
description: Azure Active Directory B2C özel ilkeler bir konu.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/24/2017
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: b08c5e6f2bc7d7970c47e14db84f4172e92eb820
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60316876"
---
# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-validation-on-user-input"></a>Çözüm: Kullanıcı girişini doğrulama olarak, Azure AD B2C kullanıcı yolculuğunun talep alışverişlerine REST API tümleştirme

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C altını kimlik deneyimi çerçevesi (IEF) (Azure AD B2C) etkileşim kullanıcı yolculuğu bir RESTful API ile tümleştirmek kimlik Geliştirici sağlar.  

Bu kılavuzun sonunda, RESTful hizmetleriyle etkileşime giren bir Azure AD B2C kullanıcı yolculuğu oluşturmak mümkün olacaktır.

IEF Taleplerde veri gönderip geri Taleplerde veri alır. API ile etkileşim:

- İçinde bir düzenleme adımı'olmuyor bir doğrulama profili olarak veya bir REST API talep değişimi tasarlanabilir.
- Genellikle, kullanıcıdan girdi doğrular. Kullanıcıdan değer reddedilirse, kullanıcı bir hata iletisi döndürmek için Fırsat geçerli bir değer girmek yeniden deneyebilirsiniz.

Ayrıca, bir düzenleme adımı olarak etkileşim tasarlayabilirsiniz. Daha fazla bilgi için [izlenecek yol: REST API talep, Azure AD B2C kullanıcı yolculuğunda düzenleme adımı olarak alışverişlerine tümleştirme](active-directory-b2c-rest-api-step-custom.md).

Doğrulama profili örneği için başlangıç paketi dosyası ProfileEdit.xml profil düzenleme kullanıcı yolculuğu kullanacağız.

Biz, profil düzenleme kullanıcı tarafından sağlanan adı bir dışlama listesi parçası olmadığını doğrulayabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- Bölümünde anlatıldığı gibi yerel bir hesap oturumu açma kaydolma/oturum açma, tamamlamak için yapılandırılmış bir Azure AD B2C kiracısı [Başlarken](active-directory-b2c-get-started-custom.md).
- İle etkileşim kurmak için bir REST API uç noktası. Bu kılavuz için şu adlı bir tanıtım sitede ayarladığınızdan [WingTipGames](https://wingtipgamesb2c.azurewebsites.net/) bir REST apı'niz ile.

## <a name="step-1-prepare-the-rest-api-function"></a>1. Adım: REST API işlevi hazırlama

> [!NOTE]
> REST API işlevleri kurulumu, bu makalenin kapsamı dışında ' dir. [Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/functions-reference) bulutta RESTful hizmetleri oluşturmak için mükemmel bir araç takımı sunar.

Bir Azure işlevi olarak beklediği talep alan oluşturduk `playerTag`. İşlev, bu talebi var olup olmadığını doğrular. Tam Azure işlevi kodu erişebileceğiniz [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).

```csharp
if (requestContentAsJObject.playerTag == null)
{
  return request.CreateResponse(HttpStatusCode.BadRequest);
}

var playerTag = ((string) requestContentAsJObject.playerTag).ToLower();

if (playerTag == "mcvinny" || playerTag == "msgates123" || playerTag == "revcottonmarcus")
{
  return request.CreateResponse<ResponseContent>(
    HttpStatusCode.Conflict,
    new ResponseContent
    {
      version = "1.0.0",
      status = (int) HttpStatusCode.Conflict,
      userMessage = $"The player tag '{requestContentAsJObject.playerTag}' is already used."
    },
    new JsonMediaTypeFormatter(),
    "application/json");
}

return request.CreateResponse(HttpStatusCode.OK);
```

IEF bekliyor `userMessage` Azure işlevinin döndürdüğü talep. Önceki örnekte 409 çakışma durumu ne zaman döndürülür gibi doğrulama başarısız olursa bu talep kullanıcıya bir dize olarak sunulur.

## <a name="step-2-configure-the-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworkextensionsxml-file"></a>2. Adım: RESTful API talep değişimi TrustFrameworkExtensions.xml dosyanızdaki teknik profili olarak yapılandırma

Teknik profili RESTful hizmeti ile istenen Exchange tam bir yapılandırmadır. TrustFrameworkExtensions.xml dosyasını açın ve içine aşağıdaki XML parçacığını ekleyin `<ClaimsProviders>` öğesi.

> [!NOTE]
> RESTful sağlayıcısı aşağıdaki XML'de `Version=1.0.0.0` protokol olarak açıklanmıştır. Dış hizmetiyle etkileşime gireceğini işlevi olarak düşünün. <!-- TODO: A full definition of the schema can be found...link to RESTful Provider schema definition>-->

```xml
<ClaimsProvider>
    <DisplayName>REST APIs</DisplayName>
    <TechnicalProfiles>
        <TechnicalProfile Id="AzureFunctions-CheckPlayerTagWebHook">
            <DisplayName>Check Player Tag Web Hook Azure Function</DisplayName>
            <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
            <Metadata>
                <Item Key="ServiceUrl">https://wingtipb2cfuncs.azurewebsites.net/api/CheckPlayerTagWebHook?code=L/05YRSpojU0nECzM4Tp3LjBiA2ZGh3kTwwp1OVV7m0SelnvlRVLCg==</Item>
                <Item Key="AuthenticationType">None</Item>
                <Item Key="SendClaimsIn">Body</Item>
                <Item Key="AllowInsecureAuthInProduction">true</Item>
            </Metadata>
            <InputClaims>
                <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="playerTag" />
            </InputClaims>
            <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
        <TechnicalProfile Id="SelfAsserted-ProfileUpdate">
            <ValidationTechnicalProfiles>
                <ValidationTechnicalProfile ReferenceId="AzureFunctions-CheckPlayerTagWebHook" />
            </ValidationTechnicalProfiles>
        </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

`InputClaims` Öğe IEF REST hizmeti için gönderilecek Talepleri tanımlar. Bu örnekte, talep içeriğini `givenName` REST hizmeti gönderilecek `playerTag`. Bu örnekte, talep geri IEF beklemiyor. Bunun yerine, aldığı durum kodlarına göre davranır ve REST hizmeti için bir yanıt bekler.

## <a name="step-3-include-the-restful-service-claims-exchange-in-self-asserted-technical-profile-where-you-want-to-validate-the-user-input"></a>3. Adım: Hizmet RESTful talep değişimi, kullanıcı girişi doğrulamak istediğiniz otomatik olarak onaylanan teknik profiline Ekle

Doğrulama adımını en yaygın kullanımı bir kullanıcıyla etkileşim bulunduğu. Burada kullanıcının beklenmektedir giriş sağlamak için tüm etkileşimler *teknik profiller Self onaylanan*. Bu örnekte, Self Asserted ProfileUpdate teknik profili doğrulama ekleyeceğiz. Bu teknik, profilini bağlı olan taraf (RP) ilke dosyası `Profile Edit` kullanır.

Talep değişimi otomatik olarak onaylanan teknik profile eklemek için:

1. TrustFrameworkBase.xml dosyasını açın ve arama `<TechnicalProfile Id="SelfAsserted-ProfileUpdate">`.
2. Bu teknik profil yapılandırmasını gözden geçirin. Exchange kullanıcı (giriş talep) kullanıcıya sorulur talepleri ve geri otomatik olarak onaylanan sağlayıcısından (çıkış talep) beklenen talepler olarak nasıl tanımlandığını gözlemleyin.
3. Arama `TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate`ve bu profile'nın düzenleme adımı 5 çağrılan fark `<UserJourney Id="ProfileEdit">`.

## <a name="step-4-upload-and-test-the-profile-edit-rp-policy-file"></a>4. Adım: Karşıya yükleme ve test profili düzenleme RP ilke dosyası

1. TrustFrameworkExtensions.xml dosyanın yeni sürümünü yükleyin.
2. Kullanım **Şimdi Çalıştır** profil düzenleme RP ilke dosyası test etmek için.
3. Doğrulama (örneğin, mcvinny) mevcut adlardan birini sağlayarak test **verilen ad** alan. Her şeyin doğru şekilde ayarlanıp ayarlanmadığını, oyuncu etiketi zaten kullanılan kullanıcı bildiren bir ileti almalısınız.

## <a name="next-steps"></a>Sonraki adımlar

[Kullanıcılarınızdan daha fazla bilgi toplamak için profil düzenleme ve kullanıcı kaydı değiştirme](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)

[İzlenecek yol: Bir düzenleme adımı, Azure AD B2C kullanıcı yolculuğunun talep alışverişlerine REST API tümleştirme](active-directory-b2c-rest-api-step-custom.md)
