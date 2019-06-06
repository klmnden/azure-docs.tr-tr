---
title: Azure Active Directory B2C, özel bir ilkede RESTful teknik profil tanımlama | Microsoft Docs
description: Azure Active Directory B2C, özel bir ilkede bir RESTful teknik profili tanımlayın.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 21a2ea861df96a057db0ec13eacd0906ed51fff1
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66512735"
---
# <a name="define-a-restful-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Bir Azure Active Directory B2C özel ilke RESTful teknik profil tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory (Azure AD) B2C, kendi RESTful hizmeti için destek sağlar. Azure AD B2C, RESTful hizmeti giriş verileri koleksiyonu talep ve verileri alan bir çıkış talep koleksiyona geri gönderir. RESTful hizmeti Tümleştirmesi ile şunları yapabilirsiniz:

- **Kullanıcı giriş verilerini doğrulamak** -Azure AD B2C'ye kalıcı hatalı oluşturulmuş veri engeller. Kullanıcıdan değer geçerli değilse, RESTful hizmetiniz bir giriş sağlamaya yönlendiren bir hata iletisi döndürür. Örneğin, kullanıcı tarafından sağlanan e-posta adresi müşterinizin veritabanında bulunduğunu doğrulayın.
- **Giriş talep üzerine** -giriş talep değerleri yeniden biçimlendirmek sağlar. Örneğin, bir kullanıcının tüm küçük adı veya tümüyle büyük harfe girerse, adı büyük harfle yalnızca ilk harfi ile biçimlendirebilirsiniz.
- **Kullanıcı verileri zenginleştirmek** -, daha fazla Kurumsal satır iş kolu uygulamaları ile tümleştirmenize olanak tanır. Örneğin, RESTful hizmetinizi kullanıcının e-posta adresi alın, müşteri veritabanını sorgulama ve kullanıcının bağlılık numaranız Azure AD B2C'ye. İade talepleri depolanan, sonraki düzenleme adımları değerlendirilen veya erişim belirtecinde yer.
- **Özel iş mantığı çalıştırma** - anında iletme bildirimleri göndermek, Kurumsal veritabanlarını güncelleştirmek, bir kullanıcı geçiş işlemi çalıştırın, izinlerini yönetmek, veritabanları denetim sağlar ve başka işlemler gerçekleştirme.

İlkeniz, REST API'nizi giriş talep gönderebilir. REST API, ilke içinde kullanabileceğiniz çıkış talep döndürebilir veya bir hata iletisi atabilirsiniz. RESTful Hizmetleri ile tümleştirme aşağıdaki yollarla tasarlayabilmek için:

- **Doğrulama teknik profili** -doğrulama teknik profili RESTful hizmeti çağırır. Kullanıcı yolculuğu devam etmeden önce doğrulama teknik profili, kullanıcı tarafından sağlanan verileri doğrular. Doğrulama teknik profil ile bir hata iletisi otomatik olarak onaylanan sayfasında görüntüleme ve çıkış talep döndürdü.
- **Talep değişimi** -RESTful hizmeti aracılığıyla bir düzenleme adımı için bir çağrı yapılır. Bu senaryoda, hata iletisini işlemek için hiçbir kullanıcı arabirimi yoktur. REST API'nizi hata verirse, kullanıcı hata iletisi ile bağlı olan taraf uygulamaya yönlendirilir.

## <a name="protocol"></a>Protocol

**Adı** özniteliği **Protokolü** öğesi ayarlanması gerekiyor `Proprietary`. **İşleyici** özniteliği Azure AD B2C tarafından kullanılan protokol işleyicisi bütünleştirilmiş kodun tam adı içermesi gerekir: `Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`.

Aşağıdaki örnek, bir RESTful teknik profili gösterir:

```XML
<TechnicalProfile Id="REST-UserMembershipValidator">
  <DisplayName>Validate user input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...    
```

## <a name="input-claims"></a>Giriş talepleri

**InputClaims** öğesi talep göndermek için REST API için bir listesini içerir. Ayrıca, REST API'de tanımlanan adına, talep adını da eşleştirebilirsiniz. Aşağıdaki örnek, ilkeniz ve REST API arasındaki eşlemeyi gösterir. **GivenName** talep REST API'sine gönderilir **firstName**, ancak **Soyadı** olarak gönderilen **lastName**. **E-posta** talep olarak ayarlanır.

```XML
<InputClaims>
  <InputClaim ClaimTypeReferenceId="email" />
  <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="firstName" />
  <InputClaim ClaimTypeReferenceId="surname" PartnerClaimType="lastName" />
</InputClaims>
```

**InputClaimsTransformations** öğe koleksiyonu içerebilir **InputClaimsTransformation** giriş talepleri değiştirebilir veya REST API için göndermeden önce yeni bir tane oluşturmak için kullanılan öğeleri.

## <a name="output-claims"></a>Çıkış talep

**OutputClaims** öğesi talep REST API'si tarafından döndürülen bir listesini içerir. İlkenizde REST API'de tanımlanan adı için tanımlanan talebin eşlemek gerekebilir. Ayarladığınız sürece REST API kimlik sağlayıcısı tarafından döndürülen olmayan talepleri de içerebilir `DefaultValue` özniteliği.

**OutputClaimsTransformations** öğe koleksiyonu içerebilir **OutputClaimsTransformation** çıkış talep değiştirmek veya yenilerini oluşturmak için kullanılan öğeleri.

Aşağıdaki örnek, REST API'si tarafından döndürülen talep gösterir:

- **MembershipId** eşleşen talep **loyaltyNumber** talep adı.

Teknik profili, kimlik sağlayıcısı tarafından döndürülen olmayan talepler, ayrıca döndürür: 

- **LoyaltyNumberIsNew** varsayılan bir değer kümesine sahip talep `true`.

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="loyaltyNumber" PartnerClaimType="MembershipId" />
  <OutputClaim ClaimTypeReferenceId="loyaltyNumberIsNew" DefaultValue="true" />
</OutputClaims>
```

## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ServiceUrl | Evet | REST API uç noktası URL'si. | 
| authenticationType | Evet | RESTful talep sağlayıcısı tarafından gerçekleştirilen kimlik doğrulama türü. Olası değerler: `None`, `Basic`, veya `ClientCertificate`. `None` Değer olmadığını REST API anonim olduğunu belirtir. `Basic` Değeri REST API ile HTTP temel kimlik doğrulaması sağlandığını gösterir. Yalnızca doğrulanmış kullanıcıların, Azure AD B2C'de dahil olmak üzere, API'nizi erişebilir. `ClientCertificate` (Önerilen) değeri gösterir REST API İstemci sertifikası kimlik doğrulaması kullanarak erişimi kısıtlar. Azure AD B2C gibi uygun sertifikaların sahip Hizmetleri, hizmetinizin erişebilirsiniz. | 
| Sendclaimsın | Hayır | Giriş talepleri RESTful talep sağlayıcısını nasıl gönderileceğini belirtir. Olası değerler: `Body` (varsayılan), `Form`, `Header`, veya `QueryString`. `Body` İstek gövdesi JSON biçiminde gönderilen giriş talep değerdir. `Form` , İstek gövdesinde bir ampersan gönderilen ' &' anahtar değeri biçimi ayrılmış giriş talep değerdir. `Header` İstek üstbilgisinde gönderilen giriş talep değerdir. `QueryString` Sorgu dizesi isteğinde gönderilen giriş talep değerdir. | 
| ClaimsFormat | Hayır | Çıkış talep biçimini belirtir. Olası değerler: `Body` (varsayılan), `Form`, `Header`, veya `QueryString`. `Body` İstek gövdesi JSON biçiminde gönderilen çıkış talep değerdir. `Form` , İstek gövdesinde bir ampersan gönderilen ' &' anahtar değeri biçimi çıkış talep değerdir. `Header` İstek üstbilgisinde gönderilen çıkış talep değerdir. `QueryString` Sorgu dizesi isteğinde gönderilen çıkış talep değerdir. | 
| DebugMode | Hayır | Teknik profili, hata ayıklama modunda çalıştırır. REST API, hata ayıklama modunda daha fazla bilgi döndürebilir. Dönen hata iletisi bölümüne bakın. | 

## <a name="cryptographic-keys"></a>Şifreleme anahtarları

Kimlik doğrulaması türü ayarlanırsa `None`, **CryptographicKeys** öğesi kullanılmaz.

```XML
<TechnicalProfile Id="REST-API-SignUp">
  <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/signup</Item>
    <Item Key="AuthenticationType">None</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
</TechnicalProfile>
```

Kimlik doğrulaması türü ayarlanırsa `Basic`, **CryptographicKeys** öğesi aşağıdaki öznitelikler içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| BasicAuthenticationUsername | Evet | Kimlik doğrulaması için kullanılan kullanıcı adı. | 
| BasicAuthenticationPassword | Evet | Kimlik doğrulaması için kullanılan parola. |

Aşağıdaki örnek, temel kimlik doğrulaması teknik bir profille gösterir:

```XML
<TechnicalProfile Id="REST-API-SignUp">
  <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/signup</Item>
    <Item Key="AuthenticationType">Basic</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="BasicAuthenticationUsername" StorageReferenceId="B2C_1A_B2cRestClientId" />
    <Key Id="BasicAuthenticationPassword" StorageReferenceId="B2C_1A_B2cRestClientSecret" />
  </CryptographicKeys>
</TechnicalProfile>
```

Kimlik doğrulaması türü ayarlanırsa `ClientCertificate`, **CryptographicKeys** öğesi aşağıdaki öznitelik içeriyor:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ClientCertificate | Evet | X509 kimliğini doğrulamak için kullanılacak sertifikayı (RSA anahtar kümesi). | 

```XML
<TechnicalProfile Id="REST-API-SignUp">
  <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/signup</Item>
    <Item Key="AuthenticationType">ClientCertificate</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="ClientCertificate" StorageReferenceId="B2C_1A_B2cRestClientCertificate" />
  </CryptographicKeys>
</TechnicalProfile>
```

## <a name="returning-error-message"></a>Hata iletisini döndürüyor

' Kullanıcı CRM sistemde bulunamadı gibi' bir hata iletisi döndürmek, REST API'nizi gerekebilir. Hata aşağıdaki özniteliklere sahip bir HTTP 409 hata iletisi (çakışma yanıt durum kodu) REST API döndürmelidir oluşur:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| version | Evet | 1.0.0 | 
| durum | Evet | 409 | 
| code | Hayır | Bir hata kodu, RESTful uç noktası sağlayıcı olduğunda görüntülenen `DebugMode` etkinleştirilir. | 
| requestId | Hayır | Bir istek tanımlayıcısı olan RESTful uç noktası sağlayıcısından olduğunda görüntülenen `DebugMode` etkinleştirilir. | 
| userMessage | Evet | Kullanıcıya gösterilen bir hata iletisi. | 
| developerMessage | Hayır | Sorun ve olan Bunu düzeltmek nasıl ayrıntılı bir açıklamasını olduğunda görüntülenen `DebugMode` etkinleştirilir. | 
| Daha fazla bilgi | Hayır | Olan ek bilgi için işaret eden bir URI olduğunda görüntülenen `DebugMode` etkinleştirilir. | 

Aşağıdaki örnek JSON biçimli bir hata iletisi döndüren bir REST API gösterir:

```JSON
{
  "version": "1.0.0",
  "status": 409,
  "code": "API12345",
  "requestId": "50f0bd91-2ff4-4b8f-828f-00f170519ddb",
  "userMessage": "Message for the user", 
  "developerMessage": "Verbose description of problem and how to fix it.", 
  "moreInfo": "https://restapi/error/API12345/moreinfo" 
}
```

Aşağıdaki örnek, bir hata iletisi döndüren bir C# sınıfı gösterir:

```C#
public class ResponseContent
{
  public string version { get; set; }
  public int status { get; set; }
  public string code { get; set; }
  public string userMessage { get; set; }
  public string developerMessage { get; set; }
  public string requestId { get; set; }
  public string moreInfo { get; set; }
}
```

## <a name="examples"></a>Örnekler:
- [Kullanıcı girişini doğrulama, Azure AD B2C kullanıcı yolculuğunun talep alışverişlerine REST API tümleştirme](active-directory-b2c-custom-rest-api-netfw.md) 
- [HTTP temel kimlik doğrulaması kullanarak, RESTful Hizmetleri güvenli hale getirme](active-directory-b2c-custom-rest-api-netfw-secure-basic.md)
- [İstemci sertifikaları kullanarak, RESTful hizmeti güvenli hale getirme](active-directory-b2c-custom-rest-api-netfw-secure-cert.md)
- [İzlenecek yol: Kullanıcı girişini doğrulama olarak, Azure AD B2C kullanıcı yolculuğunun talep alışverişlerine REST API tümleştirme](active-directory-b2c-rest-api-validation-custom.md)

 














