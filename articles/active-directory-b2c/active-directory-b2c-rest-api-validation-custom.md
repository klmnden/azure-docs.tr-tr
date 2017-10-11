---
title: "Azure Active Directory B2C: REST API alışverişleri doğrulama talep | Microsoft Docs"
description: "Bir konu Azure Active Directory B2C özel ilkeler hakkında"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/24/2017
ms.author: joroja
ms.openlocfilehash: eb44a0d2234c9ee3801d8b3a1655d877aa2f4fef
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-validation-on-user-input"></a>İzlenecek yol: Kullanıcı girişi doğrulama olarak Azure AD B2C kullanıcı Yolculuğunuzun REST API talep alışverişlerine tümleştirme

Azure Active Directory B2C altını çizen kimlik deneyimi Framework (IEF) (Azure AD B2C) etkileşim kullanıcı gezisine bir RESTful API'si ile tümleştirmek kimlik Geliştirici sağlar.  

Bu kılavuzun sonunda RESTful hizmetlerle etkileşimde bulunan bir Azure AD B2C kullanıcı gezisine oluşturmak mümkün olacaktır.

IEF Taleplerde verileri gönderir ve geri Taleplerde verilerini alır. API ile etkileşim:

- Bir REST API talep exchange veya orchestration adım içinde olur bir doğrulama profili olarak tasarlanmış olabilir.
- Genellikle, kullanıcıdan girdi doğrular. Kullanıcı değerinden reddedilirse, kullanıcı bir hata iletisi döndürmek için fırsat ile geçerli bir değer girmesini yeniden deneyebilirsiniz.

Orchestration adım olarak etkileşim de tasarlayabilirsiniz. Daha fazla bilgi için bkz: [izlenecek yol: REST API tümleştirme talep Azure AD B2C kullanıcı Yolculuğunuzun bir orchestration adım olarak alışverişlerine](active-directory-b2c-rest-api-step-custom.md).

Doğrulama profil örneği için başlangıç paketi dosyasında ProfileEdit.xml profil düzenleme kullanıcı gezisine kullanacağız.

Profil düzenleme kullanıcı tarafından sağlanan adı bir çıkarma listesi parçası olmadığından emin olun.

## <a name="prerequisites"></a>Ön koşullar

- Bölümünde açıklandığı gibi bir yerel hesap oturumu-up/oturum açmayı tamamlamak için yapılandırılmış bir Azure AD B2C kiracısı [Başlarken](active-directory-b2c-get-started-custom.md).
- Etkileşim için bir REST API uç noktası. Bu kılavuzda, biz adında bir gösteri sitesi ayarladınız [WingTipGames](https://wingtipgamesb2c.azurewebsites.net/) bir REST API hizmetiyle.

## <a name="step-1-prepare-the-rest-api-function"></a>1. adım: REST API işlevi hazırlama

> [!NOTE]
> REST API işlevleri bu makalenin kapsamı dışındadır kurulması. [Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/functions-reference) RESTful hizmetlerini bulutta oluşturmak için mükemmel bir araç sağlar.

Olarak bekliyor bir talep aldığında bir Azure işlevi oluşturduk `playerTag`. Bu talep var olup olmadığını işlevini doğrular. Tam Azure işlev kodu erişebilirsiniz [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).

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

IEF bekliyor `userMessage` talep, Azure işlevi döndürür. Bu talep 409 çakışma durum önceki örnekte döndürüldüğünde gibi doğrulama başarısız olursa kullanıcıya bir dize olarak sunulur.

## <a name="step-2-configure-the-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworkextensionsxml-file"></a>2. adım: RESTful API'si talep exchange TrustFrameworkExtensions.xml dosyanızdaki teknik bir profil olarak yapılandırın.

İstenen RESTful hizmeti ile exchange tam yapılandırmasını bir teknik profilidir. TrustFrameworkExtensions.xml dosyasını açın ve aşağıdaki XML parçacığını içine ekleyin `<ClaimsProviders>` öğesi.

> [!NOTE]
> Aşağıdaki XML, RESTful sağlayıcısı `Version=1.0.0.0` protokol olarak açıklanmıştır. Dış hizmetiyle etkileşime gireceğini işlevi olarak düşünün. <!-- TODO: A full definition of the schema can be found...link to RESTful Provider schema definition>-->

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

`InputClaims` Öğesi IEF REST hizmeti gönderilecek Talepleri tanımlar. Bu örnekte, talep içeriğini `givenName` REST hizmeti olarak gönderilir `playerTag`. Bu örnekte, talep geri IEF beklemiyor. Bunun yerine, aldığı durum kodlarına dayalı davranır ve REST hizmeti yanıt bekler.

## <a name="step-3-include-the-restful-service-claims-exchange-in-self-asserted-technical-profile-where-you-want-to-validate-the-user-input"></a>3. adım: RESTful Hizmeti talepleri exchange kullanıcı girişi doğrulamak istediğiniz Self sürülen teknik profiline Ekle

Doğrulama adımını en yaygın kullanımı bir kullanıcıyla etkileşim bağlıdır. Burada kullanıcının beklenmektedir giriş sağlamak için tüm etkileşimler *teknik profilleri otomatik olarak uygulanan*. Bu örnekte, kendi Asserted ProfileUpdate teknik profili doğrulama ekleyeceğiz. Bu teknik olduğu profilini bağlı olan taraf (RP) ilke dosyası `Profile Edit` kullanır.

Talep exchange Self sürülen teknik profiline eklemek için:

1. TrustFrameworkBase.xml dosyasını açın ve arama `<TechnicalProfile Id="SelfAsserted-ProfileUpdate">`.
2. Bu teknik profil yapılandırmasını gözden geçirin. Kullanıcı ile exchange kullanıcısı (giriş talep) istenir talepleri ve geri Self sürülen Sağlayıcısı'ndan (çıkış talep) beklenen talepler olarak nasıl tanımlanır gözlemleyin.
3. Arama `TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate`ve bu profili orchestration 6. adım çağrılır fark `<UserJourney Id="ProfileEdit">`.

## <a name="step-4-upload-and-test-the-profile-edit-rp-policy-file"></a>4. adım: Karşıya yükleme ve test profil düzenleme RP ilke dosyası

1. TrustFrameworkExtensions.xml dosyasının yeni sürümünü yükleyin.
2. Kullanım **Şimdi Çalıştır** profil düzenleme RP ilke dosyası test etmek için.
3. Doğrulama var olan adlar (örneğin, mcvinny) birini sağlayarak test **verilen ad** alan. Her şeyin doğru şekilde ayarlanmış olması durumunda kullanıcı player etiketi zaten kullanıldığını bildiren bir ileti alırsınız.

## <a name="next-steps"></a>Sonraki adımlar

[Kullanıcılardan ek bilgi toplamak için profil düzenleme ve kullanıcı kaydı değiştirin](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)

[İzlenecek yol: Azure AD B2C kullanıcı Yolculuğunuzun REST API talep alışverişlerine orchestration adım olarak tümleştirin.](active-directory-b2c-rest-api-step-custom.md)
