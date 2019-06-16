---
title: Koşullar ve PredicateValidations - Azure Active Directory B2C | Microsoft Docs
description: Sosyal hesap kimlik deneyimi çerçevesi şema, Azure Active Directory B2C için dönüşüm örnekleri talepleri.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 06879164c6f72891b734da077c667c6f90448fe4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66512959"
---
# <a name="predicates-and-predicatevalidations"></a>Koşullar ve PredicateValidations

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

**Doğrulamaları** ve **PredicateValidations** öğeleri düzgün şekilde biçimlendirilmiş veriler yalnızca Azure Active Directory (Azure AD) B2C kiracınızda oturum girildiğinden emin olmak için bir doğrulama işlemini gerçekleştirmek etkinleştirme .  

Aşağıdaki diyagramda, öğeleri arasındaki ilişki gösterilmektedir:  

![Koşullar](./media/predicates/predicates.png)

## <a name="predicates"></a>Koşullar  

**Koşul** öğesi döndürür ve bir talep türünün değerini denetlemek için bir temel doğrulama tanımlar `true` veya `false`. Belirtilen bir kullanarak doğrulama gerçekleştirilir **yöntemi** öğesi ve bir dizi **parametre** yönteme ilgili öğeleri. Örneğin, bir koşul, talep dize uzunluğu belirtilen minimum ve maksimum parametreleri aralığında olup ya da bir karakter kümesi bir dize talep değeri içerip içermediğini kontrol edebilirsiniz. **UserHelpText** öğe denetimi başarısız olursa bu hata iletisi için kullanıcılara sağlar. Değerini **UserHelpText** öğesi yerelleştirilmiş kullanarak [dil özelleştirme](localization.md).

**Doğrulamaları** öğesi aşağıdaki öğeyi içerir:

| Öğe | Örnekleri | Açıklama |
| ------- | ----------- | ----------- |
| Karşılaştırma | 1:n | Koşullar listesi. | 

**Koşul** öğesi aşağıdaki öznitelikler içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kimlik | Evet | Koşul için kullanılan tanımlayıcıdır. Diğer öğeleri tanımlayıcısı bu ilkede kullanabilirsiniz. |
| Yöntem | Evet | Doğrulama için kullanılacak yöntem türü. Olası değerler: **IsLengthRange**, **MatchesRegex**, **IncludesCharacters**, veya **IsDateRange**. **IsLengthRange** değeri bir dize talep değeri uzunluğu belirtilen minimum ve maksimum parametreleri aralığı içinde olup olmadığını denetler. **MatchesRegex** değeri bir dize talep değeri normal bir ifade ile eşleşip eşleşmediğini denetler. **IncludesCharacters** değeri bir dize talep değeri bir karakter kümesi içerip içermediğini denetler. **IsDateRange** değerini bir tarih talep değeri bir dizi belirtilen minimum ve maksimum parametreleri arasında olup olmadığını denetler. |

**Koşul** öğesi aşağıdaki öğeleri içerir:

| Öğe | Örnekleri | Açıklama |
| ------- | ----------- | ----------- |
| UserHelpText | 1:1 | Denetimi başarısız olursa, kullanıcılar için bir hata iletisi. Bu dize kullanarak yerelleştirilebilen [dil özelleştirme](localization.md) |
| Parametreler | 1:1 | Dize doğrulama yöntemi türü parametreleri. | 

**Parametreleri** öğesi aşağıdaki öğeleri içerir:

| Öğe | Örnekleri | Açıklama |
| ------- | ----------- | ----------- |
| Parametre | 1:n | Dize doğrulama yöntemi türü parametreleri. | 

**Parametre** öğesi aşağıdaki öznitelikler içerir:

| Öğe | Örnekleri | Açıklama |
| ------- | ----------- | ----------- |
| Kimlik | 1:1 | Parametre tanımlayıcısı. |

Aşağıdaki örnekte gösterildiği bir `IsLengthRange` parametrelerle yöntemi `Minimum` ve `Maximum` dize uzunluğu aralığını belirtin:

```XML
<Predicate Id="IsLengthBetween8And64" Method="IsLengthRange">
  <UserHelpText>The password must be between 8 and 64 characters.</UserHelpText>
    <Parameters>
      <Parameter Id="Minimum">8</Parameter>
      <Parameter Id="Maximum">64</Parameter>
  </Parameters>
</Predicate>
```

Aşağıdaki örnekte gösterildiği bir `MatchesRegex` yöntemi parametresi ile `RegularExpression` normal bir ifade belirtir:

```XML
<Predicate Id="PIN" Method="MatchesRegex">
  <UserHelpText>The password must be numbers only.</UserHelpText>
  <Parameters>
    <Parameter Id="RegularExpression">^[0-9]+$</Parameter>
  </Parameters>
</Predicate>
```

Aşağıdaki örnekte gösterildiği bir `IncludesCharacters` yöntemi parametresi ile `CharacterSet` karakter kümesini belirtir:

```XML
<Predicate Id="Lowercase" Method="IncludesCharacters">
  <UserHelpText>a lowercase letter</UserHelpText>
  <Parameters>
    <Parameter Id="CharacterSet">a-z</Parameter>
  </Parameters>
</Predicate>
```

Aşağıdaki örnekte gösterildiği bir `IsDateRange` parametrelerle yöntemi `Minimum` ve `Maximum` bir biçimiyle tarih aralığını belirtin `yyyy-MM-dd` ve `Today`.

```XML
<Predicate Id="DateRange" Method="IsDateRange" HelpText="The date must be between 1970-01-01 and today.">
  <Parameters>
    <Parameter Id="Minimum">1970-01-01</Parameter>
    <Parameter Id="Maximum">Today</Parameter>
  </Parameters>
</Predicate>
```

## <a name="predicatevalidations"></a>PredicateValidations 

Bir talep türüne karşı denetlemek için doğrulama koşullarına tanımlama sırasında **PredicateValidations** Grup koşulları bir talep türü için uygulanabilir bir kullanıcı girdisi doğrulama oluşturmak için bir dizi. Her **PredicateValidation** öğesi içeren bir dizi **PredicateGroup** öğeleri içeren bir dizi **PredicateReference** için bir işaretedenöğeler**Koşul**. Doğrulama geçirmek için talep değerini tüm herhangi bir koşul altında tüm testleri geçmelidir **PredicateGroup** kendi kümesiyle **PredicateReference** öğeleri.

```XML
<PredicateValidations>
  <PredicateValidation Id="">
    <PredicateGroups>
      <PredicateGroup Id="">
        <UserHelpText></UserHelpText>
        <PredicateReferences MatchAtLeast="">
          <PredicateReference Id="" />
          ...
        </PredicateReferences>
      </PredicateGroup>
      ...
    </PredicateGroups>
  </PredicateValidation>
...
</PredicateValidations>
```

**PredicateValidations** öğesi aşağıdaki öğeyi içerir:

| Öğe | Örnekleri | Açıklama |
| ------- | ----------- | ----------- |
| PredicateValidation | 1:n | Koşul doğrulama listesi. | 

**PredicateValidation** öğesi aşağıdaki öznitelik içeriyor:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kimlik | Evet | Koşul doğrulama için kullanılan tanımlayıcıdır. **ClaimType** ilke öğesi bu tanımlayıcıyı kullanabilirsiniz. |

**PredicateValidation** öğesi aşağıdaki öğeyi içerir:

| Öğe | Örnekleri | Açıklama |
| ------- | ----------- | ----------- |
| PredicateGroups | 1:n | Koşul gruplarının listesi. | 

**PredicateGroups** öğesi aşağıdaki öğeyi içerir:

| Öğe | Örnekleri | Açıklama |
| ------- | ----------- | ----------- |
| PredicateGroup | 1:n | Koşullar listesi. | 

**PredicateGroup** öğesi aşağıdaki öznitelik içeriyor:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kimlik | Evet | Koşul grubu için kullanılan tanımlayıcıdır.  |

**PredicateGroup** öğesi aşağıdaki öğeleri içerir:

| Öğe | Örnekleri | Açıklama |
| ------- | ----------- | ----------- |
| UserHelpText | 1:1 |  Hangi bir değer kattığını bilmek, kullanıcılar için yararlı olabilecek koşul açıklamasını bunlar yazmanız gerekir. | 
| PredicateReferences | 1:n | Koşul başvuruları listesi. | 

**PredicateReferences** öğesi aşağıdaki öznitelikler içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| MatchAtLeast | Hayır | Birçok giriş kabul edilmesi için tanımları koşulu, değer en az eşleşmelidir belirtir. |

**PredicateReferences** öğesi aşağıdaki öğeleri içerir:

| Öğe | Örnekleri | Açıklama |
| ------- | ----------- | ----------- |
| PredicateReference | 1:n | Bir koşula yönelik bir başvuru. | 

**PredicateReference** öğesi aşağıdaki öznitelikler içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kimlik | Evet | Koşul doğrulama için kullanılan tanımlayıcıdır.  |


## <a name="configure-password-complexity"></a>Parola karmaşıklığını yapılandırın

İle **doğrulamaları** ve **PredicateValidationsInput** bir hesabı oluştururken bir kullanıcı tarafından sağlanan parola karmaşıklık gereksinimleri kontrol edebilirsiniz. Varsayılan olarak, güçlü parolalar Azure AD B2C kullanır. Azure AD B2C, ayrıca müşteriler parola karmaşıklığı denetlemek için yapılandırma seçeneklerini destekler. Bu koşul öğeleri kullanarak parola karmaşıklığını tanımlayabilirsiniz: 

- **IsLengthBetween8And64** kullanarak `IsLengthRange` yöntemi doğrular parola 8 ile 64 karakter arasında olmalıdır.
- **Küçük harf** kullanarak `IncludesCharacters` yöntemi doğrular parola küçük harf içeriyor.
- **Büyük harf** kullanarak `IncludesCharacters` yöntemi doğrular parola bir büyük harf içeriyor.
- **Sayı** kullanarak `IncludesCharacters` yöntemi doğrular parola rakam içeriyor.
- **Sembol** kullanarak `IncludesCharacters` yöntemi doğrular parola şu simgelerden birini içeriyor `@#$%^&*\-_+=[]{}|\:',?/~"();!`
- **PIN** kullanarak `MatchesRegex` yöntemi, parola yalnızca sayılar içerdiğini doğrular.
- **AllowedAADCharacters** kullanarak `MatchesRegex` yöntemi, parola yalnızca geçersiz karakter sağlandığını doğrular.
- **DisallowedWhitespace** kullanarak `MatchesRegex` yöntemi doğrular parola başlamak değil veya bir boşluk karakteri ile bitemez.

```XML
<Predicates>
  <Predicate Id="IsLengthBetween8And64" Method="IsLengthRange">
    <UserHelpText>The password must be between 8 and 64 characters.</UserHelpText>
    <Parameters>
      <Parameter Id="Minimum">8</Parameter>
      <Parameter Id="Maximum">64</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="Lowercase" Method="IncludesCharacters">
    <UserHelpText>a lowercase letter</UserHelpText>
    <Parameters>
      <Parameter Id="CharacterSet">a-z</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="Uppercase" Method="IncludesCharacters">
    <UserHelpText>an uppercase letter</UserHelpText>
    <Parameters>
      <Parameter Id="CharacterSet">A-Z</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="Number" Method="IncludesCharacters">
    <UserHelpText>a digit</UserHelpText>
    <Parameters>
      <Parameter Id="CharacterSet">0-9</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="Symbol" Method="IncludesCharacters">
    <UserHelpText>a symbol</UserHelpText>
    <Parameters>
      <Parameter Id="CharacterSet">@#$%^&amp;*\-_+=[]{}|\:',?/`~"();!</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="PIN" Method="MatchesRegex">
    <UserHelpText>The password must be numbers only.</UserHelpText>
    <Parameters>
      <Parameter Id="RegularExpression">^[0-9]+$</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="AllowedAADCharacters" Method="MatchesRegex">
    <UserHelpText>An invalid character was provided.</UserHelpText>
    <Parameters>
      <Parameter Id="RegularExpression">(^([0-9A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~"();! ]|(\.(?!@)))+$)|(^$)</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="DisallowedWhitespace" Method="MatchesRegex">
    <UserHelpText>The password must not begin or end with a whitespace character.</UserHelpText>
    <Parameters>
      <Parameter Id="RegularExpression">(^\S.*\S$)|(^\S+$)|(^$)</Parameter>
    </Parameters>
  </Predicate>
```

Temel doğrulamaları tanımladıktan sonra bunları birbirine birleştirmek ve ilkenizde kullanabileceğiniz parola ilkeleri kümesini oluşturun:

- **SimplePassword** DisallowedWhitespace AllowedAADCharacters ve IsLengthBetween8And64 doğrular
- **Güçlüparola** DisallowedWhitespace, AllowedAADCharacters, IsLengthBetween8And64 doğrular. Son grup `CharacterClasses` koşullar ek bir dizi çalıştırır `MatchAtLeast` 3 olarak ayarlayın. Parola 8 ila 16 karakter ve şu karakterlerden birini üç arasında olmalıdır: Küçük, büyük harf, sayı veya simge.
- **CustomPassword** yalnızca DisallowedWhitespace, AllowedAADCharacters doğrular. Bu nedenle, kullanıcı karakterlerin geçerli olduğu sürece herhangi bir uzunlukta bir parola sağlayabilir.

```XML
<PredicateValidations>
  <PredicateValidation Id="SimplePassword">
    <PredicateGroups>
      <PredicateGroup Id="DisallowedWhitespaceGroup">
        <PredicateReferences>
          <PredicateReference Id="DisallowedWhitespace" />
        </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="AllowedAADCharactersGroup">
        <PredicateReferences>
          <PredicateReference Id="AllowedAADCharacters" />
        </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="LengthGroup">
        <PredicateReferences>
          <PredicateReference Id="IsLengthBetween8And64" />
        </PredicateReferences>
      </PredicateGroup>
    </PredicateGroups>
  </PredicateValidation>

  <PredicateValidation Id="StrongPassword">
    <PredicateGroups>
      <PredicateGroup Id="DisallowedWhitespaceGroup">
        <PredicateReferences>
          <PredicateReference Id="DisallowedWhitespace" />
       </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="AllowedAADCharactersGroup">
        <PredicateReferences>
          <PredicateReference Id="AllowedAADCharacters" />
        </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="LengthGroup">
        <PredicateReferences>
          <PredicateReference Id="IsLengthBetween8And64" />
        </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="CharacterClasses">
        <UserHelpText>The password must have at least 3 of the following:</UserHelpText>
        <PredicateReferences MatchAtLeast="3">
          <PredicateReference Id="Lowercase" />
          <PredicateReference Id="Uppercase" />
          <PredicateReference Id="Number" />
          <PredicateReference Id="Symbol" />
        </PredicateReferences>
      </PredicateGroup>
    </PredicateGroups>
  </PredicateValidation>

  <PredicateValidation Id="CustomPassword">
    <PredicateGroups>
      <PredicateGroup Id="DisallowedWhitespaceGroup">
        <PredicateReferences>
          <PredicateReference Id="DisallowedWhitespace" />
        </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="AllowedAADCharactersGroup">
        <PredicateReferences>
          <PredicateReference Id="AllowedAADCharacters" />
        </PredicateReferences>
      </PredicateGroup>
    </PredicateGroups>
  </PredicateValidation>
</PredicateValidations>
```

Talep türünüz ekleme **PredicateValidationReference** öğesi ve SimplePassword, güçlüparola veya CustomPassword gibi koşul doğrulamaları biri olarak tanımlayıcısını belirtin.

```XML
<ClaimType Id="password">
  <DisplayName>Password</DisplayName>
  <DataType>string</DataType>
  <AdminHelpText>Enter password</AdminHelpText>
  <UserHelpText>Enter password</UserHelpText>
  <UserInputType>Password</UserInputType>
  <PredicateValidationReference Id="StrongPassword" />
</ClaimType>
```

Azure AD B2C hata iletisi görüntülendiğinde öğeleri nasıl düzenlendiği gösterir:

![Doğrulama işlemi](./media/predicates/predicates-pass.png)

## <a name="configure-a-date-range"></a>Bir tarih aralığı yapılandırın

İle **doğrulamaları** ve **PredicateValidations** öğeleri minimum ve maksimum tarih değerlerini denetleyebilirsiniz **UserInputType** kullanarak bir `DateTimeDropdown`. Bunu yapmak için oluşturun bir **koşul** ile `IsDateRange` yöntemi ve minimum ve maksimum parametrelerini belirtin.

```XML
<Predicates>
  <Predicate Id="DateRange" Method="IsDateRange">
    <UserHelpText>The date must be between 01-01-1980 and today.</UserHelpText>
    <Parameters>
      <Parameter Id="Minimum">1980-01-01</Parameter>
      <Parameter Id="Maximum">Today</Parameter>
    </Parameters>
  </Predicate>
</Predicates>
```

Ekleme bir **PredicateValidation** başvurusuyla `DateRange` koşul.

```XML
<PredicateValidations>
  <PredicateValidation Id="CustomDateRange">
    <PredicateGroups>
      <PredicateGroup Id="DateRangeGroup">
        <PredicateReferences>
          <PredicateReference Id="DateRange" />
        </PredicateReferences>
      </PredicateGroup>
    </PredicateGroups>
  </PredicateValidation>
</PredicateValidations>
```

Talep türünüz ekleme **PredicateValidationReference** öğe tanımlayıcı olarak belirtin `CustomDateRange`. 
    
```XML
<ClaimType Id="dateOfBirth">
  <DisplayName>Date of Birth</DisplayName>
  <DataType>date</DataType>
  <AdminHelpText>The user's date of birth.</AdminHelpText>
  <UserHelpText>Your date of birth.</UserHelpText>
  <UserInputType>DateTimeDropdown</UserInputType>
  <PredicateValidationReference Id="CustomDateRange" />
</ClaimType>
 ```
