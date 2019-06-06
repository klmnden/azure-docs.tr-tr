---
title: ClaimsSchema - Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C'de özel bir ilke ClaimsSchema öğesi belirtin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: e09c4530fc6dce00e6d807908c7de598422a440b
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66511859"
---
# <a name="claimsschema"></a>ClaimsSchema

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

**ClaimsSchema** öğe ilkesinin bir parçası başvurulabilir talep türlerini tanımlar. Talep şema, talep burada bildirdiğiniz yerdir. Talebi ad, son adı, görünen ad, telefon numarası ve daha fazla olabilir. ClaimsSchema öğe listesini içeren **ClaimType** öğeleri. **ClaimType** ögesinin **kimliği** talep adı özniteliği. 

```XML
<BuildingBlocks>
  <ClaimsSchema>
    <ClaimType Id="Id">
      <DisplayName>Surname</DisplayName>
      <DataType>string</DataType>
      <DefaultPartnerClaimTypes>
        <Protocol Name="OAuth2" PartnerClaimType="family_name" />
        <Protocol Name="OpenIdConnect" PartnerClaimType="family_name" />
        <Protocol Name="SAML2" PartnerClaimType="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname" />
      </DefaultPartnerClaimTypes>
      <UserHelpText>Your surname (also known as family name or last name).</UserHelpText>
      <UserInputType>TextBox</UserInputType>
```

## <a name="claimtype"></a>ClaimType

**ClaimType** öğesi aşağıdaki öznitelik içeriyor:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kimlik | Evet | Talep türü için kullanılan tanımlayıcıdır. Diğer öğeleri tanımlayıcısı bu ilkede kullanabilirsiniz. |

**ClaimType** öğesi aşağıdaki öğeleri içerir:

| Öğe | Örnekleri | Açıklama |
| ------- | ----------- | ----------- |
| displayName | 0:1 | Çeşitli ekranında kullanıcılara görüntülenen başlığı. Değer olabilir [yerelleştirilmiş](localization.md). |
| DataType | 0:1 | Talep türü. Veri türlerini boolean, tarih, dateTime, int, long, dize stringCollection, alternativeSecurityIdCollection kullanılabilir. |
| DefaultPartnerClaimTypes | 0:1 | İş ortağı varsayılan talep türleri için belirtilen bir protokol kullanın. Değer, geçersiz kılınabilir **PartnerClaimType** belirtilen **Inputclaim** veya **OutputClaim** öğeleri. Bu öğe, bir protokol için varsayılan adı belirtmek için kullanın.  |
| Maskesi | 0:1 | Talep görüntülenirken uygulanabilir karakter maskeleme isteğe bağlı bir dize. Örneğin, telefon numarası 324-232-4343 XXX-XXX-4343 maskelenmiş olamaz. |
| UserHelpText | 0:1 | Amacı anlamak, kullanıcılar için yararlı olabilecek bir talep türünün açıklaması. Değer olabilir [yerelleştirilmiş](localization.md). |
| UserInputType | 0:1 | El ile için talep türünü talep veri girerken kullanılabilir olması gereken giriş denetimi türü. Bu sayfada tanımlı kullanıcı giriş türleri bakın. |
| Kısıtlama | 0:1 | Normal bir ifade (Regex) ya da kabul edilebilir değerler listesi gibi bu talebi için değer kısıtlaması. Değer olabilir [yerelleştirilmiş](localization.md). |
PredicateValidationReference| 0:1 | Bir başvuru bir **PredicateValidationsInput** öğesi. **PredicateValidationReference** öğeleri düzgün şekilde biçimlendirilmiş veriler yalnızca girildiğinden emin olmak için bir doğrulama işlemini gerçekleştirmek etkinleştirin. Daha fazla bilgi için [doğrulamaları](predicates.md). |

### <a name="defaultpartnerclaimtypes"></a>DefaultPartnerClaimTypes

**DefaultPartnerClaimTypes** aşağıdaki öğe içerebilir:

| Öğe | Örnekleri | Açıklama |
| ------- | ----------- | ----------- |
| Protocol | 0: n | Talep türü adı, varsayılan iş ortakları ile protokollerin listesi. |

**Protokolü** öğesi aşağıdaki öznitelikler içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Name | Evet | Azure AD B2C tarafından desteklenen geçerli bir protokol adı. Olası değerler şunlardır:  OAuth1, OAuth2, SAML2, Openıdconnect, WsFed veya WsTrust. |
| PartnerClaimType | Evet | Kullanılacak talep türü adı. |

Aşağıdaki örnekte olduğu saml2 tabanlı kimlik sağlayıcısı veya bağlı olan taraf uygulaması ile kimlik deneyimi çerçevesi etkileşim kurduğunda **Soyadı** talep eşlendi `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`, Openıdconnect ve OAuth2, talep olduğunu eşlenen `family_name`.

```XML
<ClaimType Id="surname">
  <DisplayName>Surname</DisplayName>
  <DataType>string</DataType>
  <DefaultPartnerClaimTypes>
    <Protocol Name="OAuth2" PartnerClaimType="family_name" />
    <Protocol Name="OpenIdConnect" PartnerClaimType="family_name" />
    <Protocol Name="SAML2" PartnerClaimType="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname" />
  </DefaultPartnerClaimTypes>
</ClaimType>
```

Sonuç olarak, Azure AD B2C tarafından verilen JWT belirteci yayan `family_name` ClaimType adı yerine **Soyadı**.
 
```JSON
{
  "sub": "6fbbd70d-262b-4b50-804c-257ae1706ef2",
  "auth_time": 1535013501,
  "given_name": "David",
  "family_name": "Williams",
  "name": "David Williams",
}
```

### <a name="mask"></a>Maskesi

**Maskesi** öğesi aşağıdaki öznitelikler içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| `Type` | Evet | Talep maskesi türü. Olası değerler: `Simple` veya `Regex`. `Simple` Değeri basit metin maske dizesi talep önde gelen bölümüne uygulandığını gösterir. `Regex` Değeri belirten bir normal ifade dizesini talep bütün olarak uygulanır.  Varsa `Regex` değeri belirtilirse, isteğe bağlı bir öznitelik, kullanılacak normal ifade ile tanımlanmalıdır. |
| `Regex` | Hayır | Varsa **`Type`** ayarlanır `Regex`, normal ifadeyi belirtin.

Aşağıdaki örnek yapılandırır bir **PhoneNumber** ile talep `Simple` maskesi:

```XML
<ClaimType Id="PhoneNumber">
  <DisplayName>Phone Number</DisplayName>
  <DataType>string</DataType>
  <Mask Type="Simple">XXX-XXX-</Mask>  
  <UserHelpText>Your telephone number.</UserHelpText>
</ClaimType>
```

Kimlik deneyimi çerçevesi, telefon numarası, ilk altı gizleyerek işler:

![Talep türü maskesi ile kullanma](./media/claimsschema/mask.png)

Aşağıdaki örnek yapılandırır bir **AlternateEmail** ile talep `Regex` maskesi:

```XML
<ClaimType Id="AlternateEmail">
  <DisplayName>Please verify the secondary email linked to your account</DisplayName>
  <DataType>string</DataType>
  <Mask Type="Regex" Regex="(?&lt;=.).(?=.*@)">*</Mask>
  <UserInputType>Readonly</UserInputType>
</ClaimType>
```

Kimlik deneyimi çerçevesi e-posta adresini ve e-posta etki alanı adı yalnızca ilk harfini işler:

![Talep türü maskesi ile kullanma](./media/claimsschema/mask-regex.png)


### <a name="restriction"></a>Kısıtlama

**Kısıtlama** öğesi, aşağıdaki öznitelik içerebilir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| MergeBehavior | Hayır | Sabit listesi değerleri ile aynı tanımlayıcıya sahip bir üst ilke içinde bir ClaimType birleştirmek için kullanılan yöntem. Bu öznitelik temel ilkede belirtilen talep üzerine kullanın. Olası değerler: `Append`, `Prepend`, veya `ReplaceAll`. `Append` Değer, üst ilkesinde belirtilen koleksiyonun sonuna eklenecek bir veri koleksiyonudur. `Prepend` Değer, üst ilkesinde belirtilen koleksiyon önce eklenmesi gereken bir veri koleksiyonudur. `ReplaceAll` Yoksayılması gereken üst ilkesinde belirtilen veri koleksiyonunu bir değerdir. |

**Kısıtlama** öğesi aşağıdaki öğeleri içerir:

| Öğe | Örnekleri | Açıklama |
| ------- | ----------- | ----------- |
| Sabit listesi | 1:n | Kullanıcı arabiriminde kullanılabilen seçenekler için açılan bir menüde bir değer gibi talep seçmek kullanıcı. |
| Desen | 1:1 | Kullanılacak normal ifade. |

### <a name="enumeration"></a>Sabit listesi

**Numaralandırma** öğesi aşağıdaki öznitelikler içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Text | Evet | Bu seçenek için kullanıcı arabirimi kullanıcılara gösterilen görüntü dizesi. |
|Değer | Evet | Bu seçeneğin belirlenmesi ile ilişkili talep değeri. |
| SelectByDefault | Hayır | Bu seçenek varsayılan olarak kullanıcı arabiriminde seçili olup olmadığını gösterir. Olası değerler: TRUE veya False. |

Aşağıdaki örnek yapılandırır bir **Şehir** açılan liste talep kümesine varsayılan değeri `New York`:

```XML
<ClaimType Id="city">
  <DisplayName>city where you work</DisplayName>
  <DataType>string</DataType>
  <UserInputType>DropdownSingleSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Bellevue" Value="bellevue" SelectByDefault="false" />
    <Enumeration Text="Redmond" Value="redmond" SelectByDefault="false" />
    <Enumeration Text="New York" Value="new-york" SelectByDefault="true" />
  </Restriction>
</ClaimType>
```
New York için açılan Şehir listedeki varsayılan değeri ayarlayın:

![Açılan Şehir listesi](./media/claimsschema/dropdownsingleselect.png)


### <a name="pattern"></a>Desen

**Deseni** öğesi aşağıdaki öznitelikleri içerebilir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Yanıtta normal ifade | Evet | Normal ifade geçerli olması için bu tür talepler eşleşmesi gerekir. |
| HelpText | Hayır | Desen veya bu talebi için normal bir ifade. |

Aşağıdaki örnek yapılandırır bir **e-posta** normal ifade giriş doğrulama ve Yardım metni ile talep:

```XML
<ClaimType Id="email">
  <DisplayName>Email Address</DisplayName>
  <DataType>string</DataType>
  <DefaultPartnerClaimTypes>
    <Protocol Name="OpenIdConnect" PartnerClaimType="email" />
  </DefaultPartnerClaimTypes>
  <UserHelpText>Email address that can be used to contact you.</UserHelpText>
  <UserInputType>TextBox</UserInputType>
  <Restriction>
    <Pattern RegularExpression="^[a-zA-Z0-9.!#$%&amp;'^_`{}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$" HelpText="Please enter a valid email address." />
    </Restriction>
 </ClaimType>
```

Kimlik deneyimi çerçevesi ile e-posta biçimini giriş doğrulama e-posta adresi talep oluşturur:

![Talep türü deseni ile kullanma](./media/claimsschema/pattern.png)

## <a name="userinputtype"></a>UserInputType

Azure AD B2C, çeşitli el ile veri talep için talep türü girerken kullanılabilir bir metin kutusu, parola ve açılan liste gibi kullanıcı giriş türlerini destekler. Belirtmelisiniz **UserInputType** topladığınız durumlar bilgi kullanıcıdan kullanarak bir [teknik profil otomatik olarak onaylanan](self-asserted-technical-profile.md).

### <a name="textbox"></a>TextBox

**TextBox** kullanıcı girişi türü tek satırlı metin kutusu sağlamak amacıyla kullanılır.

![Talep türü textbox ile kullanma](./media/claimsschema/textbox.png)

```XML
<ClaimType Id="displayName">
  <DisplayName>Display Name</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your display name.</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```

### <a name="emailbox"></a>EmailBox

**EmailBox** kullanıcı girişi türü temel e-posta giriş alanı sağlamak için kullanılır.

![Talep türü emailbox ile kullanma](./media/claimsschema/emailbox.png)

```XML
<ClaimType Id="email">
  <DisplayName>Email Address</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Email address that can be used to contact you.</UserHelpText>
  <UserInputType>EmailBox</UserInputType>
  <Restriction>
    <Pattern RegularExpression="^[a-zA-Z0-9!#$%&amp;'+^_`{}~-]+(?:\.[a-zA-Z0-9!#$%&amp;'+^_`{}~-]+)*@(?:[a-zA-Z0-9](?:[a-zA-Z0-9-]*[a-zA-Z0-9])?\.)+[a-zA-Z0-9](?:[a-zA-Z0-9-]*[a-zA-Z0-9])?$" HelpText="Please enter a valid email address." />
  </Restriction>
</ClaimType>
```

### <a name="password"></a>Parola

**Parola** kullanıcı girişi türü kullanıcı tarafından girilen parola kaydetmek için kullanılır.

![Talep türü ile parola kullanarak](./media/claimsschema/password.png)

```XML
<ClaimType Id="password">
  <DisplayName>Password</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Enter password</UserHelpText>
  <UserInputType>Password</UserInputType>
</ClaimType>
```

### <a name="datetimedropdown"></a>DateTimeDropdown

**DateTimeDropdown** kullanıcı girişi türü bir dizi bir gün, ay ve yıl seçmek için açılır listeleri sağlamak amacıyla kullanılır. Minimum ve maksimum tarih değerleri denetlemek için koşulları ve PredicateValidations öğeleri kullanabilirsiniz. Daha fazla bilgi için **tarih aralığını yapılandırma** bölümünü [doğrulamaları ve PredicateValidations](predicates.md).

![Talep türü datetimedropdown ile kullanma](./media/claimsschema/datetimedropdown.png)

```XML
<ClaimType Id="dateOfBirth">
  <DisplayName>Date Of Birth</DisplayName>
  <DataType>date</DataType>
  <UserHelpText>The date on which you were born.</UserHelpText>
  <UserInputType>DateTimeDropdown</UserInputType>
</ClaimType>
```

### <a name="radiosingleselect"></a>RadioSingleSelect

**RadioSingleSelect** kullanıcı girişi türü, bir seçenek belirlemek kullanıcıya izin veren radyo düğmeleri koleksiyonunu sağlamak amacıyla kullanılır.

![Talep türü radiodsingleselect ile kullanma](./media/claimsschema/radiosingleselect.png)

```XML
<ClaimType Id="color">
  <DisplayName>Preferred color</DisplayName>
  <DataType>string</DataType>
  <UserInputType>RadioSingleSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Blue" Value="Blue" SelectByDefault="false" />
    <Enumeration Text="Green " Value="Green" SelectByDefault="false" />
    <Enumeration Text="Orange" Value="Orange" SelectByDefault="true" />
  </Restriction>
</ClaimType>    
```

### <a name="dropdownsingleselect"></a>DropdownSingleSelect

**DropdownSingleSelect** kullanıcı girişi türü, bir seçenek belirlemek kullanıcıya izin veren bir açılır liste kutusundan sağlamak amacıyla kullanılır.

![Talep türü dropdownsingleselect ile kullanma](./media/claimsschema/dropdownsingleselect.png)

```XML
<ClaimType Id="city">
  <DisplayName>City where you work</DisplayName>
  <DataType>string</DataType>
  <UserInputType>DropdownSingleSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Bellevue" Value="bellevue" SelectByDefault="false" />
    <Enumeration Text="Redmond" Value="redmond" SelectByDefault="false" />
    <Enumeration Text="New York" Value="new-york" SelectByDefault="true" />
  </Restriction>
</ClaimType>
```

### <a name="checkboxmultiselect"></a>CheckboxMultiSelect

**CheckboxMultiSelect** kullanıcı girişi türü birden fazla seçenek seçmesini sağlayan onay kutularından oluşan bir koleksiyon sağlamak amacıyla kullanılır.

![Talep türü checkboxmultiselect ile kullanma](./media/claimsschema/checkboxmultiselect.png)

```XML
<ClaimType Id="languages">
  <DisplayName>Languages you speak</DisplayName>
  <DataType>string</DataType>
  <UserInputType>CheckboxMultiSelect</UserInputType>
  <Restriction>
    <Enumeration Text="English" Value="English" SelectByDefault="true" />
    <Enumeration Text="France " Value="France" SelectByDefault="false" />
    <Enumeration Text="Spanish" Value="Spanish" SelectByDefault="false" />
  </Restriction>
</ClaimType>
```

### <a name="readonly"></a>salt okunur

**Salt okunur** kullanıcı girişi türü ve talep değeri görüntülemek için salt okunur bir alan sağlamak amacıyla kullanılır.

![Talep türü salt okunur ile kullanma](./media/claimsschema/readonly.png)

```XML
<ClaimType Id="membershipNumber">
  <DisplayName>Membership number</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your membership number (read only)</UserHelpText>
  <UserInputType>Readonly</UserInputType>
</ClaimType>
```


### <a name="paragraph"></a>Paragraf

**Paragraf** kullanıcı girişi türü yalnızca bir paragraf etiketinin metin gösteren bir alan sağlamak amacıyla kullanılır. Örneğin, &lt;p&gt;metin&lt;/p&gt;.

![Talep türü paragraf ile kullanma](./media/claimsschema/paragraph.png)

```XML
<ClaimType Id="responseMsg">
  <DisplayName>Error message: </DisplayName>
  <DataType>string</DataType>
  <AdminHelpText>A claim responsible for holding response messages to send to the relying party</AdminHelpText>
  <UserHelpText>A claim responsible for holding response messages to send to the relying party</UserHelpText>
  <UserInputType>Paragraph</UserInputType>
  <Restriction>
    <Enumeration Text="B2C_V1_90001" Value="You cant sign in because you are a minor" />
    <Enumeration Text="B2C_V1_90002" Value="This action can only be performed by gold members" />
    <Enumeration Text="B2C_V1_90003" Value="You have not been enabled for this operation" />
  </Restriction>
</ClaimType>
```

Biri görüntülenecek **numaralandırma** değerler bir **responseMsg** kullanın, talep `GetMappedValueFromLocalizedCollection` veya `CreateStringClaim` dönüştürme talep. Daha fazla bilgi için [dize talep dönüşümleri](string-transformations.md) 
