---
title: Kimlik deneyimi çerçevesi şema Azure Active Directory B2C için dize talep dönüştürme örnekler | Microsoft Docs
description: Dize dönüşüm örnekleri kimlik deneyimi çerçevesi şema, Azure Active Directory B2C için talep.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 6c6b90cb2b24f6a87504a3a0dbd47f998b67ebc1
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64707469"
---
# <a name="string-claims-transformations"></a>Dize talep dönüşümleri

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory (Azure AD) B2C'de kimlik deneyimi çerçevesi şema dize talep Dönüşümleri kullanma örnekleri sağlar. Daha fazla bilgi için [ClaimsTransformations](claimstransformations.md).

## <a name="assertstringclaimsareequal"></a>AssertStringClaimsAreEqual 

İki talep karşılaştırın ve bunlar belirtilen karşılaştırma inputClaim1, inputClaim2 ve stringComparison göre eşit değilse bir özel durum.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | inputClaim1 | string | Karşılaştırılacak olan ilk talebin türü. |
| Inputclaim | inputClaim2 | string | Karşılaştırılacak talebin türü, ikinci. |
| InputParameter | stringComparison | string | dize karşılaştırması, değerlerden biri: Sıra, Ordinalıgnorecase. |

**AssertStringClaimsAreEqual** talep dönüştürme gelen her zaman yürütülür bir [doğrulama teknik profili](validation-technical-profile.md) çağrılan bir [teknik profilSelfonaylanan](self-asserted-technical-profile.md). **UserMessageIfClaimsTransformationStringsAreNotEqual** otomatik olarak onaylanan teknik profil meta verileri kullanıcıya sunulan hata iletisi denetler.

![AssertStringClaimsAreEqual execution](./media/string-transformations/assert-execution.png)

Kullanabileceğiniz bu talep dönüştürme emin olmak için iki ClaimTypes aynı değere sahip olmalıdır. Aksi takdirde bir hata iletisi oluşturulur. Aşağıdaki örnek denetleyen **strongAuthenticationEmailAddress** ClaimType eşittir **e-posta** ClaimType. Aksi takdirde bir hata iletisi oluşturulur. 

```XML
<ClaimsTransformation Id="AssertEmailAndStrongAuthenticationEmailAddressAreEqual" TransformationMethod="AssertStringClaimsAreEqual">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="strongAuthenticationEmailAddress" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="inputClaim2" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="stringComparison" DataType="string" Value="ordinalIgnoreCase" />
  </InputParameters>
</ClaimsTransformation>
```


**Etkileşimli olmayan oturum açma** doğrulama teknik profil çağrıları **AssertEmailAndStrongAuthenticationEmailAddressAreEqual** talep dönüştürme.
```XML
<TechnicalProfile Id="login-NonInteractive">
  ...
  <OutputClaimsTransformations>
    <OutputClaimsTransformation ReferenceId="AssertEmailAndStrongAuthenticationEmailAddressAreEqual" />
  </OutputClaimsTransformations>
</TechnicalProfile>
```

Doğrulama otomatik olarak onaylanan teknik profil çağırır **etkileşimli olmayan oturum açma** teknik profili.

```XML
<TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
  <Metadata>
    <Item Key="UserMessageIfClaimsTransformationStringsAreNotEqual">Custom error message the email addresses you provided are not the same.</Item>
  </Metadata>
  <ValidationTechnicalProfiles>
    <ValidationTechnicalProfile ReferenceId="login-NonInteractive" />
  </ValidationTechnicalProfiles>
</TechnicalProfile>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **inputClaim1**: someone@contoso.com
  - **inputClaim2**: someone@outlook.com
    - Giriş parametreleri:
  - **stringComparison**: Ordinalıgnorecase
- Sonuç: Durum hatası

## <a name="changecase"></a>ChangeCase 

Düşürmek için sağlanan talebin küçük veya büyük harf ' işleci bağlı olarak değişir.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | inputClaim1 | string | Olması ClaimType değiştirildi. |
| InputParameter | toCase | string | Aşağıdaki değerlerden biri: `LOWER` veya `UPPER`. |
| outputClaim | outputClaim | string | Bu dönüşüm talep sonra üreten ClaimType çağrılmış. |

Bu talep dönüştürmeyi herhangi bir dize düşürmek için ClaimType veya büyük harf değiştirmek için kullanın.  

```XML
<ClaimsTransformation Id="ChangeToLower" TransformationMethod="ChangeCase">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="inputClaim1" />
  </InputClaims>
<InputParameters>
  <InputParameter Id="toCase" DataType="string" Value="LOWER" />
</InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **e-posta**: SomeOne@contoso.com
- Giriş parametreleri:
    - **toCase**: LOWER
- Çıkış talep:
  - **e-posta**: someone@contoso.com

## <a name="createstringclaim"></a>CreateStringClaim 

İlkede sağlanan giriş parametresi bir dize talep oluşturur.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
|----- | ----------------------- | --------- | ----- |
| InputParameter | value | string | Ayarlanacak dize |
| outputClaim | createdClaim | string | Giriş parametresinde belirtilen değerle sonra bu dönüşüm talep üreten ClaimType çağrılmış. |

Bu dönüşüm talep kullanımı bir dize ClaimType değeri ayarlayın.

```XML
<ClaimsTransformation Id="CreateTermsOfService" TransformationMethod="CreateStringClaim">
  <InputParameters>
    <InputParameter Id="value" DataType="string" Value="Contoso terms of service..." />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="TOS" TransformationClaimType="createdClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş parametresi:
    - **Değer**: Hizmet contoso kullanım koşulları...
- Çıkış talep:
    - **createdClaim**: TOS ClaimType "Contoso hizmet kullanım koşullarını..." değeri içeriyor.

## <a name="compareclaims"></a>CompareClaims

Tek bir dize talep diğerine eşit olup olmadığını belirler. Değerine sahip yeni bir Boole ClaimType sonucudur `true` veya `false`.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | inputClaim1 | string | İlk talep karşılaştırılacak olan türü. |
| Inputclaim | inputClaim2 | string | İkinci talep karşılaştırılacak olan türü. |
| InputParameter | İşleci | string | Olası değerler: `EQUAL` veya `NOT EQUAL`. |
| InputParameter | IgnoreCase | boole | Bu karşılaştırma karşılaştırılan dizelerin durumunu yoksay olup olmadığını belirtir. |
| outputClaim | outputClaim | boole | Bu dönüşüm talep sonra üreten ClaimType çağrılmış. |

Bu talep dönüştürme talep için başka bir talep eşit olup olmadığını denetlemek için kullanın. Örneğin, aşağıdaki, dönüştürme denetimleri talep değerini **e-posta** talep eşittir **Verified.Email** talep.

```XML
<ClaimsTransformation Id="CheckEmail" TransformationMethod="CompareClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="Email" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="Verified.Email" TransformationClaimType="inputClaim2" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="operator" DataType="string" Value="NOT EQUAL" />
    <InputParameter Id="ignoreCase" DataType="string" Value="true" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="SameEmailAddress" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **inputClaim1**: someone@contoso.com
  - **inputClaim2**: someone@outlook.com
- Giriş parametreleri:
    - **İşleç**:  EŞİT DEĞİLDİR
    - **ignoreCase**: true
- Çıkış talep:
    - **outputClaim**: true

## <a name="compareclaimtovalue"></a>CompareClaimToValue

Bir talep değerinin giriş parametresi değerine eşit olup olmadığını belirler.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | inputClaim1 | string | Karşılaştırılacak olduğu talebin türü. |
| InputParameter | İşleci | string | Olası değerler: `EQUAL` veya `NOT EQUAL`. |
| InputParameter | compareTo | string | dize karşılaştırması, değerlerden biri: Sıra, Ordinalıgnorecase. |
| InputParameter | IgnoreCase | boole | Bu karşılaştırma karşılaştırılan dizelerin durumunu yoksay olup olmadığını belirtir. |
| outputClaim | outputClaim | boole | Bu dönüşüm talep sonra üreten ClaimType çağrılmış. |

Kullanabileceğiniz bu talep, belirtilen bir değere eşit olup olmadığını denetlemek için dönüştürme talep. Örneğin, aşağıdaki, dönüştürme denetimleri talep değerini **termsOfUseConsentVersion** talep eşittir `v1`.

```XML
<ClaimsTransformation Id="IsTermsOfUseConsentRequiredForVersion" TransformationMethod="CompareClaimToValue">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="termsOfUseConsentVersion" TransformationClaimType="inputClaim1" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="compareTo" DataType="string" Value="V1" />
    <InputParameter Id="operator" DataType="string" Value="not equal" />
    <InputParameter Id="ignoreCase" DataType="string" Value="true" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="termsOfUseConsentRequired" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek
- Giriş talepleri:
    - **inputClaim1**: v1
- Giriş parametreleri:
    - **CompareTo**: V1
    - **İşleç**: EŞİTTİR 
    - **ignoreCase**: true
- Çıkış talep:
    - **outputClaim**: true

## <a name="createrandomstring"></a>CreateRandomString

Rastgele sayı oluşturucusunu kullanarak rastgele bir dize oluşturur. Rasgele sayı üreteci türü ise `integer`isteğe bağlı çekirdek parametresi ve en fazla sağlanabilir. Bir dize isteğe bağlı biçim parametresini kullanmadan biçimlendirilmiş çıktı sağlar ve isteğe bağlı base64 parametresi çıkış base64 kodlu randomGeneratorType [GUID, tamsayı] outputClaim (dize) olup olmadığını belirtir.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| InputParameter | randomGeneratorType | string | Oluşturulacak, rastgele bir değer belirtir `GUID` (genel benzersiz Tanımlayıcı) veya `INTEGER` (sayı). |
| InputParameter | stringFormat | string | [İsteğe bağlı] Rastgele bir değeri biçimi. |
| InputParameter | Base64 | boole | [İsteğe bağlı] Rastgele bir değeri, base64 dönüştürün. Dize biçimi uygulanırsa, değerin dize biçimi sonra base64 kodlanmış. |
| InputParameter | maximumNumber | int | [İsteğe bağlı] İçin `INTEGER` yalnızca randomGeneratorType. Maksimum sayısını belirtin. |
| InputParameter | Çekirdek  | int | [İsteğe bağlı] İçin `INTEGER` yalnızca randomGeneratorType. Çekirdek için rastgele bir değer belirtin. Not: aynı çekirdek aynı rastgele sayı dizisi üretir. |
| outputClaim | outputClaim | string | Bu dönüşüm talep sonra üretilecek ClaimTypes çağrılmış. Rastgele bir değer. |

Aşağıdaki örnek, genel olarak benzersiz bir kimliği oluşturur. Başka bir talep dönüştürme rastgele UPN (kullanıcı asıl adı) oluşturmak için kullanılır.

```XML
<ClaimsTransformation Id="CreateRandomUPNUserName" TransformationMethod="CreateRandomString">
  <InputParameters>
    <InputParameter Id="randomGeneratorType" DataType="string" Value="GUID" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="upnUserName" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```
### <a name="example"></a>Örnek

- Giriş parametreleri:
    - **randomGeneratorType**: GUID
- Çıkış talep: 
    - **outputClaim**: bc8bedd2-aaa3-411e-bdee-2f1810b73dfc

Aşağıdaki örnek, 0 ile 1000 arasında rastgele bir tamsayı değeri oluşturur. Değer {rastgele değer} OTP_ için biçimlendirilir.

```XML
<ClaimsTransformation Id="SetRandomNumber" TransformationMethod="CreateRandomString">
  <InputParameters>
    <InputParameter Id="randomGeneratorType" DataType="string" Value="INTEGER" />
    <InputParameter Id="maximumNumber" DataType="int" Value="1000" />
    <InputParameter Id="stringFormat" DataType="string" Value="OTP_{0}" />
    <InputParameter Id="base64" DataType="boolean" Value="false" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="randomNumber" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş parametreleri:
    - **randomGeneratorType**: INTEGER
    - **maximumNumber**: 1000
    - **stringFormat**: OTP_{0}
    - **Base64**: false
- Çıkış talep: 
    - **outputClaim**: OTP_853


## <a name="formatstringclaim"></a>FormatStringClaim

Bir talep göre sağlanan biçim dizesi biçimi. Bu dönüşüm C# kullanan `String.Format` yöntemi.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | Inputclaim |string |Dize biçimi davranan ClaimType {0} parametresi. |
| InputParameter | stringFormat | string | Dize biçimi de dahil olmak üzere {0} parametresi. |
| outputClaim | outputClaim | string | Bu dönüşüm talep sonra üreten ClaimType çağrılmış. |

Bu talep herhangi bir parametre ile dize biçimine dönüştürme kullanım {0}. Aşağıdaki örnek, oluşturur bir **userPrincipalName**. Tüm sosyal kimlik sağlayıcısı teknik profiller, gibi `Facebook-OAUTH` çağrıları **CreateUserPrincipalName** oluşturmak için bir **userPrincipalName**.   

```XML
<ClaimsTransformation Id="CreateUserPrincipalName" TransformationMethod="FormatStringClaim">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="upnUserName" TransformationClaimType="inputClaim" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="stringFormat" DataType="string" Value="cpim_{0}@{RelyingPartyTenantId}" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="userPrincipalName" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **Inputclaim**: 5164db16-3eee-4629-bfda-dcc3326790e9
- Giriş parametreleri:
    - **stringFormat**: cpim_{0}@{RelyingPartyTenantId}
- Çıkış talep:
  - **outputClaim**: cpim_5164db16-3eee-4629-bfda-dcc3326790e9@b2cdemo.onmicrosoft.com

## <a name="formatstringmultipleclaims"></a>FormatStringMultipleClaims

Sağlanan biçim dizesi göre iki talep biçiminde. Bu dönüşüm C# kullanan **String.Format** yöntemi.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | Inputclaim |string | Dize biçimi davranan ClaimType {0} parametresi. |
| Inputclaim | Inputclaim | string | Dize biçimi davranan ClaimType {1} parametresi. |
| InputParameter | stringFormat | string | Dize biçimi de dahil olmak üzere {0} ve {1} parametreleri. |
| outputClaim | outputClaim | string | Bu dönüşüm talep sonra üreten ClaimType çağrılmış. |

Bu talep herhangi iki parametre ile dize biçimine dönüştürme kullanım {0} ve {1}. Aşağıdaki örnek, oluşturur bir **displayName** ile belirtilen biçim:

```XML
<ClaimsTransformation Id="CreateDisplayNameFromFirstNameAndLastName" TransformationMethod="FormatStringMultipleClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="givenName" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="surName" TransformationClaimType="inputClaim2" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="stringFormat" DataType="string" Value="{0} {1}" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="displayName" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **inputClaim1**: ALi
    - **inputClaim2**: Fernando
- Giriş parametreleri:
    - **stringFormat**: {0} {1}
- Çıkış talep:
    - **outputClaim**: Joe Fernando

## <a name="getmappedvaluefromlocalizedcollection"></a>GetMappedValueFromLocalizedCollection

Öğeyi bakan bir talep **kısıtlama** koleksiyonu.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | mapFromClaim | string | İçinde denetlenmesi metni içeren talep **restrictionValueClaim** ile talep **kısıtlama** koleksiyonu.  |
| outputClaim | restrictionValueClaim | string | İçeren talep **kısıtlama** koleksiyonu. Talep dönüştürme çağrıldıktan sonra bu talep değerini seçilen öğenin değerini içerir. |

Aşağıdaki örnekte hata anahtara göre hata iletisi açıklaması arar. **ResponseMsg** talep hata iletileri son kullanıcıya sunmak veya bağlı olan tarafa gönderilecek koleksiyonunu içerir.

```XML
<ClaimType Id="responseMsg">
  <DisplayName>Error message: </DisplayName>
  <DataType>string</DataType>
  <UserInputType>Paragraph</UserInputType>
  <Restriction>
    <Enumeration Text="B2C_V1_90001" Value="You cant sign in because you are a minor" />
    <Enumeration Text="B2C_V1_90002" Value="This action can only be performed by gold members" />
    <Enumeration Text="B2C_V1_90003" Value="You have not been enabled for this operation" />
  </Restriction>
</ClaimType>
```
Talep dönüştürme öğenin metni arar ve değeri döndürür. Kısıtlama kullanarak yerelleştirilmiş ise, `<LocalizedCollection>`, talep dönüştürme yerelleştirilen değeri döndürür.

```XML
<ClaimsTransformation Id="GetResponseMsgMappedToResponseCode" TransformationMethod="GetMappedValueFromLocalizedCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="responseCode" TransformationClaimType="mapFromClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="responseMsg" TransformationClaimType="restrictionValueClaim" />         
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **mapFromClaim**: B2C_V1_90001
- Çıkış talep:
    - **restrictionValueClaim**: Yapılamıyor küçük olduğundan oturum açın.

## <a name="lookupvalue"></a>LookupValue

Başka bir talep değerine göre değerler listesinden bir talep değeri aramak.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | inputParameterId | string | Arama değerini içeren talep |
| InputParameter | |string | InputParameters koleksiyonu. |
| InputParameter | errorOnFailedLookup | boole | Eşleşen hiçbir arama çalışılırken bir hata döndürülür olup olmadığını denetliyor. |
| outputClaim | inputParameterId | string | Bu dönüşüm talep sonra üretilecek ClaimTypes çağrılmış. Eşleşen kimlik değeri |

Aşağıdaki örnek InputParameters koleksiyonlardan biri etki alanı adını arar. Talep dönüştürme tanımlayıcı etki alanı adıyla arar ve (uygulama kimliği) değerini döndürür.

```XML
 <ClaimsTransformation Id="DomainToClientId" TransformationMethod="LookupValue">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="domainName" TransformationClaimType="inputParameterId" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="contoso.com" DataType="string" Value="13c15f79-8fb1-4e29-a6c9-be0d36ff19f1" />
    <InputParameter Id="microsoft.com" DataType="string" Value="0213308f-17cb-4398-b97e-01da7bd4804e" />
    <InputParameter Id="test.com" DataType="string" Value="c7026f88-4299-4cdb-965d-3f166464b8a9" />
    <InputParameter Id="errorOnFailedLookup" DataType="boolean" Value="false" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="domainAppId" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation> 
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **inputParameterId**: test.com
- Giriş parametreleri:
    - **contoso.com**: 13c15f79-8fb1-4e29-a6c9-be0d36ff19f1
    - **Microsoft.com**: 0213308f-17cb-4398-b97e-01da7bd4804e
    - **Test.com**: c7026f88-4299-4cdb-965d-3f166464b8a9
    - **errorOnFailedLookup**: false
- Çıkış talep:
    - **outputClaim**: c7026f88-4299-4cdb-965d-3f166464b8a9

## <a name="nullclaim"></a>NullClaim

Belirli bir talep değerini temizleyin.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| outputClaim | claim_to_null | string | Talep değeri NULL olmalıdır. |

Bu talep dönüştürme talep özellik paketi gereksiz verileri kaldırmak için kullanın. Bu nedenle, oturum tanımlama bilgisinin daha küçük olur. Aşağıdaki örnek, değeri kaldırır `TermsOfService` talep türü.

```XML
<ClaimsTransformation Id="SetTOSToNull" TransformationMethod="NullClaim">
  <OutputClaims>
  <OutputClaim ClaimTypeReferenceId="TermsOfService" TransformationClaimType="claim_to_null" />
  </OutputClaims>
</ClaimsTransformation>
```

- Giriş talepleri:
    - **outputClaim**: Contoso uygulamasına Hoş Geldiniz. Göz atın ve bu Web sitesi kullanmak devam ederseniz, uyumlu ve aşağıdaki hüküm ve koşulların bağlayıcılığını kabul etmiş olursunuz...
- Çıkış talep:
    - **outputClaim**: NULL

## <a name="parsedomain"></a>ParseDomain

Bir e-posta adresi etki alanı kısmını alır.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | EmailAddress | string | E-posta adresini içeren ClaimType. |
| outputClaim | etki alanı | string | Bu dönüşüm talep sonra üreten ClaimType çağrıldıktan - etki alanı. |

Kullanım bu etki alanı adından sonra ayrıştırılacak dönüştürme talep @ sembolünü kullanıcının. Bu, kişisel bilgileri (PII) gelen denetim verilerini kaldırılmasında yararlı olabilir. Aşağıdaki talep dönüştürmenin nasıl ayrıştıracağını etki alanı adından gösterir bir **e-posta** talep.

```XML
<ClaimsTransformation Id="SetDomainName" TransformationMethod="ParseDomain">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="emailAddress" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="domainName" TransformationClaimType="domain" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **emailAddress**: joe@outlook.com
- Çıkış talep:
    - **etki alanı**: outlook.com

## <a name="setclaimsifstringsareequal"></a>SetClaimsIfStringsAreEqual

Bir dize talep denetler ve `matchTo` giriş parametresi eşit ve kümeleri çıkış talep mevcut değerle `stringMatchMsg` ve `stringMatchMsgCode` giriş parametreleri olarak ayarlanacak karşılaştırma sonucu çıkış talep birlikte `true` veya `false` karşılaştırma sonucuna göre.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | Inputclaim | string | Karşılaştırılacak olan talep türü. |
| InputParameter | matchTo | string | Dizenin karşılaştırılacağı `inputClaim`. |
| InputParameter | stringComparison | string | Olası değerler: `Ordinal` veya `OrdinalIgnoreCase`. |
| InputParameter | stringMatchMsg | string | Dizeler eşitse ayarlamak için ilk değer. |
| InputParameter | stringMatchMsgCode | string | Dizeler eşitse ayarlamak için ikinci değer. |
| outputClaim | outputClaim1 | string | Dizeler eşittir varsa, bu çıkış talep değerini içeren `stringMatchMsg` giriş parametresi. |
| outputClaim | outputClaim2 | string | Dizeler eşittir varsa, bu çıkış talep değerini içeren `stringMatchMsgCode` giriş parametresi. |
| outputClaim | stringCompareResultClaim | boole | Karşılaştırma sonucu çıkış talep türü olarak ayarlanacak `true` veya `false` karşılaştırma sonucuna göre. |

Kullanabileceğiniz bu talep, belirtilen değere eşit olup olmadığını denetlemek için dönüştürme talep. Örneğin, aşağıdaki, dönüştürme denetimleri talep değerini **termsOfUseConsentVersion** talep eşittir `v1`. Yanıt Evet ise, değere değiştirin `v2`. 

```XML
<ClaimsTransformation Id="CheckTheTOS" TransformationMethod="SetClaimsIfStringsAreEqual">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="termsOfUseConsentVersion" TransformationClaimType="inputClaim" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="matchTo" DataType="string" Value="v1" />
    <InputParameter Id="stringComparison" DataType="string" Value="ordinalIgnoreCase" />
    <InputParameter Id="stringMatchMsg" DataType="string" Value="B2C_V1_90005" />
    <InputParameter Id="stringMatchMsgCode" DataType="string" Value="The TOS is upgraded to v2" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="termsOfUseConsentVersion" TransformationClaimType="outputClaim1" />
    <OutputClaim ClaimTypeReferenceId="termsOfUseConsentVersionUpgradeCode" TransformationClaimType="outputClaim2" />
    <OutputClaim ClaimTypeReferenceId="termsOfUseConsentVersionUpgradeResult" TransformationClaimType="stringCompareResultClaim" />
  </OutputClaims>
</ClaimsTransformation>
```
### <a name="example"></a>Örnek

- Giriş talepleri:
    - **Inputclaim**: v1
- Giriş parametreleri:
    - **matchTo**: V1
    - **stringComparison**: Ordinalıgnorecase 
    - **stringMatchMsg**:  B2C_V1_90005
    - **stringMatchMsgCode**:  TOS v2'ye yükseltilir.
- Çıkış talep:
    - **outputClaim1**: B2C_V1_90005
    - **outputClaim2**: TOS v2'ye yükseltilir.
    - **stringCompareResultClaim**: true

## <a name="setclaimsifstringsmatch"></a>SetClaimsIfStringsMatch

Bir dize talep denetler ve `matchTo` giriş parametresi eşit ve kümeleri çıkış talep mevcut değerle `outputClaimIfMatched` olarak ayarlanacak karşılaştırma sonucu çıkış talep yanı sıra giriş parametresi `true` veya `false` göre Karşılaştırma sonucu.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | claimToMatch | string | Karşılaştırılacak olan talep türü. |
| InputParameter | matchTo | string | Inputclaim ile Karşılaştırılacak dize. |
| InputParameter | stringComparison | string | Olası değerler: `Ordinal` veya `OrdinalIgnoreCase`. |
| InputParameter | outputClaimIfMatched | string | Dizeler eşitse ayarlanacak değer. |
| outputClaim | outputClaim | string | Dizeler eşittir varsa, bu çıkış talep değerini içeren `outputClaimIfMatched` giriş parametresi. Veya dizeler eşleşme yoksa null. |
| outputClaim | stringCompareResultClaim | boole | Karşılaştırma sonucu çıkış talep türü olarak ayarlanacak `true` veya `false` karşılaştırma sonucuna göre. |

Örneğin, aşağıdaki, dönüştürme denetimleri talep değerini **yaş** talep eşittir `Minor`. Değeri Evet ise, dönüş `B2C_V1_90001`. 

```XML
<ClaimsTransformation Id="SetIsMinor" TransformationMethod="SetClaimsIfStringsMatch">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="ageGroup" TransformationClaimType="claimToMatch" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="matchTo" DataType="string" Value="Minor" />
    <InputParameter Id="stringComparison" DataType="string" Value="ordinalIgnoreCase" />
    <InputParameter Id="outputClaimIfMatched" DataType="string" Value="B2C_V1_90001" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isMinor" TransformationClaimType="outputClaim" />
    <OutputClaim ClaimTypeReferenceId="isMinorResponseCode" TransformationClaimType="stringCompareResultClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **claimToMatch**: İkincil
- Giriş parametreleri:
    - **matchTo**: İkincil
    - **stringComparison**: Ordinalıgnorecase 
    - **outputClaimIfMatched**:  B2C_V1_90001
- Çıkış talep:
    - **isMinorResponseCode**: B2C_V1_90001
    - **isMinor**: true

