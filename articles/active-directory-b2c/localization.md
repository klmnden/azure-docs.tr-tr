---
title: Yerelleştirme - Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C'de, özel bir ilke Localization öğesi belirtin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: a02983c5019870e8b17db48184b2f238a82f8a40
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66510592"
---
# <a name="localization"></a>Yerelleştirme

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

**Yerelleştirme** öğesi kullanıcı yolculuklarından ilkesinde dillerini veya birden çok yerel destek sağlar. İlkeleri yerelleştirme desteği sağlar:

- Açık bir ilkede desteklenen dillerin listesini ayarlayın ve varsayılan bir dil seçin.
- Dile özgü dizeleri ve Koleksiyonlar sağlar.

```XML
<Localization Enabled="true">
  <SupportedLanguages DefaultLanguage="en" MergeBehavior="ReplaceAll">
    <SupportedLanguage>en</SupportedLanguage>
    <SupportedLanguage>es</SupportedLanguage>
  </SupportedLanguages>
  <LocalizedResources Id="api.localaccountsignup.en">
  <LocalizedResources Id="api.localaccountsignup.es">
  ...
```

**Yerelleştirme** öğesi aşağıdaki öznitelikler içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Enabled | Hayır | Olası değerler: `true` veya `false`. |

**Yerelleştirme** ögesinin XML öğeleri

| Öğe | Örnekleri | Açıklama |
| ------- | ----------- | ----------- |
| SupportedLanguages | 1:n | Desteklenen dillerin listesi. | 
| LocalizedResources | 0: n | Yerelleştirilmiş kaynaklar listesi. |

## <a name="supportedlanguages"></a>SupportedLanguages

**SupportedLanguages** öğesi aşağıdaki öznitelikler içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| DefaultLanguage | Evet | Yerelleştirilmiş kaynaklar için varsayılan olarak kullanılacak dil. |
| MergeBehavior | Hayır | Bir numaralandırma değerlerinden herhangi ClaimType birlikte birleştirilmiş değerlerinin aynı tanımlayıcıya sahip bir üst ilke sunmak. Bu öznitelik temel ilkede belirtilen talep üzerine kullanın. Olası değerler: `Append`, `Prepend`, veya `ReplaceAll`. `Append` Mevcut verilerin toplanmasını üst ilkesinde belirtilen koleksiyonun sonuna eklenecek bir değer belirtir. `Prepend` Mevcut verilerin toplanması üst ilkesinde belirtilen koleksiyon önce eklenmesi gereken bir değer belirtir. `ReplaceAll` Değeri üst ilkede tanımlanan veri koleksiyonu, bunun yerine geçerli ilkede tanımlanan verileri kullanarak dikkate alınması gerektiğini belirtir. |

### <a name="supportedlanguages"></a>SupportedLanguages

**SupportedLanguages** öğesi aşağıdaki öğeleri içerir:

| Öğe | Örnekleri | Açıklama |
| ------- | ----------- | ----------- |
| SupportedLanguage | 1:n | Bir dil etiketi RFC 5646 - tanımlayan diller için etiketleri başına uyan içeriği görüntüler. | 

## <a name="localizedresources"></a>LocalizedResources

**LocalizedResources** öğesi aşağıdaki öznitelikler içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kimlik | Evet | Yerelleştirilmiş kaynaklar benzersiz şekilde tanımlamak için kullanılan tanımlayıcıdır. |

**LocalizedResources** öğesi aşağıdaki öğeleri içerir:

| Öğe | Örnekleri | Açıklama |
| ------- | ----------- | ----------- |
| LocalizedCollections | 0: n | Tüm koleksiyonlar çeşitli kültürde tanımlar. Bir koleksiyon öğeleri ve çeşitli kültürler için kullanılabilecek farklı dizeleri farklı sayıda olabilir. Koleksiyonları örnekleri görüntülenen sabit listeleri talep türleri içerir. Örneğin, bir ülke/bölge listesi açılır listesinde gösterilir. |
| LocalizedStrings | 0: n | Tüm koleksiyonlar, çeşitli kültürlerde görünen Bu dizelerin dışındaki dizelerini tanımlar. |

### <a name="localizedcollections"></a>LocalizedCollections

**LocalizedCollections** öğesi aşağıdaki öğeleri içerir:

| Öğe | Örnekleri | Açıklama |
| ------- | ----------- | ----------- |
| LocalizedCollection | 1:n | Desteklenen dillerin listesi. |

#### <a name="localizedcollection"></a>LocalizedCollection

**LocalizedCollection** öğesi aşağıdaki öznitelikler içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ElementType | Evet | ClaimType öğesi veya bir kullanıcı arabirimi öğesi ilke dosyasına başvurur. |
| ElementId | Evet | Zaten bir talep türü bir başvuru içeren bir dize kullanılır ClaimsSchema bölümünde tanımlanan **ElementType** bir ClaimType için ayarlanır. |
| TargetCollection | Evet | Hedef koleksiyonu. |

**LocalizedCollection** öğesi aşağıdaki öğeleri içerir:

| Öğe | Örnekleri | Açıklama |
| ------- | ----------- | ----------- |
| Öğe | 0: n | Kullanıcı için bir açılan bir değer gibi kullanıcı arabiriminde bir talep seçmek kullanılabilen bir seçenek tanımlar. |

**Öğesi** öğesi aşağıdaki öznitelikler içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Text | Evet | Bu seçenek için kullanıcı arabirimi kullanıcılara gösterilecek kolay görünen dize. |
| Değer | Evet | Dize değeri bu seçeneğin belirlenmesi ile ilişkili talep. |

Aşağıdaki örnek kullanımını gösterir **LocalizedCollections** öğesi. İki tane **LocalizedCollection** öğeleri, İngilizce ve İspanyolca için başka bir. Hem **kısıtlama** talep koleksiyonunu `Gender` ile İngilizce ve İspanyolca için öğeleri listesi.

```XML
<LocalizedResources Id="api.selfasserted.en">
 <LocalizedCollections>
   <LocalizedCollection ElementType="ClaimType" ElementId="Gender" TargetCollection="Restriction">
      <Item Text="Female" Value="F" />
      <Item Text="Male" Value="M" />
    </LocalizedCollection>
</LocalizedCollections>

<LocalizedResources Id="api.selfasserted.es">
 <LocalizedCollections>
   <LocalizedCollection ElementType="ClaimType" ElementId="Gender" TargetCollection="Restriction">
      <Item Text="Femenino" Value="F" />
      <Item Text="Masculino" Value="M" />
    </LocalizedCollection>
</LocalizedCollections>

```

### <a name="localizedstrings"></a>LocalizedStrings

**LocalizedStrings** öğesi aşağıdaki öğeleri içerir:

| Öğe | Örnekleri | Açıklama |
| ------- | ----------- | ----------- |
| LocalizedString | 1:n | Yerelleştirilmiş bir dize. |

**LocalizedString** öğesi aşağıdaki öznitelikler içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ElementType | Evet | Bir talep türü öğesi veya bir kullanıcı arabirimi öğesi ilkesinde başvuru. Olası değerler: `ClaimType`, `UxElement`, `ErrorMessage`, `Predicate`, veya. `ClaimType` Değeri Stringıd belirtildiği gibi talep özniteliklerinden biri yerelleştirmek için kullanılır. `UxElement` Değeri bir Stringıd belirtildiği gibi kullanıcı arabirimi öğeleri yerelleştirmek için kullanılır. `ErrorMessage` Değeri Stringıd belirtildiği gibi sistem hata iletilerinden birini yerelleştirmek için kullanılır. `Predicate` Birini yerelleştirmek için kullanılan değer [koşul](predicates.md) Stringıd belirtilen hata iletileri. `InputValidation` Birini yerelleştirmek için kullanılan değer [PredicateValidation](predicates.md) Grup Stringıd belirtilen hata iletileri. |
| ElementId | Evet | Varsa **ElementType** ayarlanır `ClaimType`, `Predicate`, veya `InputValidation`, bu öğe zaten ClaimsSchema bölümünde tanımlanmış bir talep türüne başvuru içeriyor. | 
| StringId | Evet | Varsa **ElementType** ayarlanır `ClaimType`, bu öğe bir talep türü bir özniteliğin bir başvuru içeriyor. Olası değerler: `DisplayName`, `AdminHelpText`, veya `PatternHelpText`. `DisplayName` Değeri talep görünen adı ayarlamak için kullanılır. `AdminHelpText` Değeri talep kullanıcının Yardım metni adını ayarlamak için kullanılır. `PatternHelpText` Değeri talep deseni Yardım metnini ayarlamak için kullanılır. Varsa **ElementType** ayarlanır `UxElement`, bu öğe bir öznitelik bir kullanıcı arabirimi öğesinin bir başvuru içeriyor. Varsa **ElementType** ayarlanır `ErrorMessage`, bu öğe bir hata iletisi tanımlayıcısını belirtir. Bkz: [yerelleştirme dize kimliklerini](localization-string-ids.md) tam listesi için `UxElement` tanımlayıcıları.|


Aşağıdaki örnek yerelleştirilmiş bir kayıt sayfasını gösterir. İlk üç **LocalizedString** değerleri talep özniteliğini ayarlayın. Üçüncü devam düğmesine değerini değiştirir. Bir hata iletisi değiştirir.

```XML
<LocalizedResources Id="api.selfasserted.en">
  <LocalizedStrings>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="DisplayName">Email</LocalizedString>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="UserHelpText">Please enter your email</LocalizedString>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="PatternHelpText">Please enter a valid email address</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="button_continue">Create new account</LocalizedString>
   <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfClaimsPrincipalAlreadyExists">The account you are trying to create already exists, please sign-in.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```

Aşağıdaki örnek yerelleştirilmiş bir gösterir **UserHelpText** , **koşul** kimlikli `IsLengthBetween8And64`. Ve yerelleştirilmiş bir **UserHelpText** , **PredicateGroup** kimlikli `CharacterClasses` , **PredicateValidation** kimlikli `StrongPassword`.

```XML
<PredicateValidation Id="StrongPassword">
  <PredicateGroups>
    ...
    <PredicateGroup Id="CharacterClasses">
    ...
    </PredicateGroup>
  </PredicateGroups>
</PredicateValidation>

...

<Predicate Id="IsLengthBetween8And64" Method="IsLengthRange">
  ...
</Predicate>
...


<LocalizedString ElementType="InputValidation" ElementId="StrongPassword" StringId="CharacterClasses">The password must have at least 3 of the following:</LocalizedString>

<LocalizedString ElementType="Predicate" ElementId="IsLengthBetween8And64" StringId="HelpText">The password must be between 8 and 64 characters.</LocalizedString>              
```

## <a name="set-up-localization"></a>Yerelleştirme ayarlayın

Bu makalede kullanıcı yolculuklarından ilkesinde birden çok yerel ayarları veya dillerini destekleyen gösterilmektedir. Yerelleştirme üç adımı gerektirir: ayarı açık desteklenen dillerin listesini dile özgü dizeleri ve Koleksiyonlar sağlar ve sayfa için ContentDefinition düzenleyin.

### <a name="set-up-the-explicit-list-of-supported-languages"></a>Açık desteklenen dillerin listesi ayarlamak

Altında **BuildingBlocks** öğe, Ekle **yerelleştirme** desteklenen dillerin listesini içeren öğe. Aşağıdaki örnek, hem (varsayılan) İngilizce ve İspanyolca için yerelleştirme desteğini tanımla gösterilmektedir:

```XML
<Localization Enabled="true">
  <SupportedLanguages DefaultLanguage="en" MergeBehavior="ReplaceAll">
    <SupportedLanguage>en</SupportedLanguage>
    <SupportedLanguage>es</SupportedLanguage>
  </SupportedLanguages>
</Localization>
```

### <a name="provide-language-specific-strings-and-collections"></a>Dile özgü dizeleri ve koleksiyonları belirtin 

Ekleme **LocalizedResources** içinde öğeleri **yerelleştirme** öğeden oluşan sonra **SupportedLanguages** öğesi. Eklediğiniz **LocalizedResources** her sayfa (içerik tanım) ve desteklemek istediğiniz herhangi bir dil için öğeleri. Birleşik kaydolma veya oturum açma sayfası, kaydolma ve çok faktörlü kimlik doğrulaması (MFA) sayfaları, İngilizce, İspanyolca ve Fransa özelleştirmek için aşağıdaki ekleyin. **LocalizedResources** öğeleri.  
- Birleşik kaydolma veya oturum açma sayfasında, İngilizce `<LocalizedResources Id="api.signuporsignin.en">`
- Birleşik kaydolma veya oturum açma sayfasında, İspanyolca `<LocalizedResources Id="api.signuporsignin.es">`
- Birleşik kaydolma veya oturum açma sayfasında, Fransa `<LocalizedResources Id="api.signuporsignin.fr">` 
- Kaydolma, İngilizce `<LocalizedResources Id="api.localaccountsignup.en">`
- Kaydolma, İspanyolca `<LocalizedResources Id="api.localaccountsignup.es">`
- Kayıt, Fransa `<LocalizedResources Id="api.localaccountsignup.fr">`
- MFA, İngilizce `<LocalizedResources Id="api.phonefactor.en">`
- MFA, İspanyolca `<LocalizedResources Id="api.phonefactor.es">`
- MFA, Fransa `<LocalizedResources Id="api.phonefactor.fr">`

Her **LocalizedResources** öğesi içerir gerekli tüm **LocalizedStrings** öğeleri birden çok **LocalizedString** öğeleri ve  **LocalizedCollections** öğeleri birden çok **LocalizedCollection** öğeleri.  Aşağıdaki örnek, kayıt sayfasına İngilizce yerelleştirme ekler: 

Not: Bu örnek başvuru yapar `Gender` ve `City` talep türleri. Bu örneği kullanmak için bu talepleri tanımladığınız emin olun. Daha fazla bilgi için [ClaimsSchema](claimsschema.md).

```XML
<LocalizedResources Id="api.localaccountsignup.en">

 <LocalizedCollections>
   <LocalizedCollection ElementType="ClaimType" ElementId="Gender" TargetCollection="Restriction">
      <Item Text="Female" Value="F" />
      <Item Text="Male" Value="M" />
    </LocalizedCollection>
   <LocalizedCollection ElementType="ClaimType" ElementId="City" TargetCollection="Restriction">
      <Item Text="New York" Value="NY" />
      <Item Text="Paris" Value="Paris" />
      <Item Text="London" Value="London" />
    </LocalizedCollection>
  </LocalizedCollections>

  <LocalizedStrings>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="DisplayName">Email</LocalizedString>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="UserHelpText">Please enter your email</LocalizedString>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="PatternHelpText">Please enter a valid email address</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="button_continue">Create new account</LocalizedString>
   <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfClaimsPrincipalAlreadyExists">The account you are trying to create already exists, please sign-in.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```

Kayıt sayfasına yerelleştirme İspanyolca için.

```XML
<LocalizedResources Id="api.localaccountsignup.es">

 <LocalizedCollections>
   <LocalizedCollection ElementType="ClaimType" ElementId="Gender" TargetCollection="Restriction">
      <Item Text="Femenino" Value="F" />
      <Item Text="Masculino" Value="M" />
    </LocalizedCollection>
   <LocalizedCollection ElementType="ClaimType" ElementId="City" TargetCollection="Restriction">
      <Item Text="Nueva York" Value="NY" />
      <Item Text="París" Value="Paris" />
      <Item Text="Londres" Value="London" />
    </LocalizedCollection>
  </LocalizedCollections>

  <LocalizedStrings>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="DisplayName">Dirección de correo electrónico</LocalizedString>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="UserHelpText">Dirección de correo electrónico que puede usarse para ponerse en contacto con usted.</LocalizedString>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="PatternHelpText">Introduzca una dirección de correo electrónico.</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="button_continue">Crear</LocalizedString>
   <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfClaimsPrincipalAlreadyExists">Ya existe un usuario con el id. especificado. Elija otro diferente.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```

### <a name="edit-the-contentdefinition-for-the-page"></a>Sayfa için ContentDefinition Düzenle 

Aramak için dil kodlarını Yerelleştirmek istediğiniz her sayfanın belirtin **ContentDefinition**.

Aşağıdaki örnekte, İngilizce (TR) ve İspanyolca (es) özel dizeleri kayıt sayfasına eklenir. **LocalizedResourcesReferenceId** her **LocalizedResourcesReference** kendi yerel ayar ile aynıdır, ancak herhangi bir dize tanımlayıcı olarak kullanabilirsiniz. Her dil ve sayfa birleşimi için karşılık gelen noktası **LocalizedResources** daha önce oluşturduğunuz.

```XML
<ContentDefinition Id="api.localaccountsignup">
...
  <LocalizedResourcesReferences MergeBehavior="Prepend">
    <LocalizedResourcesReference Language="en" LocalizedResourcesReferenceId="api.localaccountsignup.en" />
    <LocalizedResourcesReference Language="es" LocalizedResourcesReferenceId="api.localaccountsignup.es" />
  </LocalizedResourcesReferences>
</ContentDefinition>
```

Aşağıdaki örnek, son XML gösterir:

```XML
<BuildingBlocks>
  <ContentDefinitions>
    <ContentDefinition Id="api.localaccountsignup">
      <!-- Other content definitions elements... -->
      <LocalizedResourcesReferences MergeBehavior="Prepend">
         <LocalizedResourcesReference Language="en" LocalizedResourcesReferenceId="api.localaccountsignup.en" />
         <LocalizedResourcesReference Language="es" LocalizedResourcesReferenceId="api.localaccountsignup.es" />
      </LocalizedResourcesReferences>
    </ContentDefinition>
    <!-- More content definitions... -->
  </ContentDefinitions>

  <Localization Enabled="true">

    <SupportedLanguages DefaultLanguage="en" MergeBehavior="ReplaceAll">
      <SupportedLanguage>en</SupportedLanguage>
      <SupportedLanguage>es</SupportedLanguage>
      <!-- More supported language... -->
    </SupportedLanguages>

    <LocalizedResources Id="api.localaccountsignup.en">
      <LocalizedCollections>
        <LocalizedCollection ElementType="ClaimType" ElementId="Gender" TargetCollection="Restriction">
          <Item Text="Female" Value="F" />
          <Item Text="Male" Value="M" />
          <!-- More items... -->
        </LocalizedCollection>
        <LocalizedCollection ElementType="ClaimType" ElementId="City" TargetCollection="Restriction">
          <Item Text="New York" Value="NY" />
          <Item Text="Paris" Value="Paris" />
          <Item Text="London" Value="London" />
        </LocalizedCollection>
        <!-- More localized collections... -->
      </LocalizedCollections>
      <LocalizedStrings>
        <LocalizedString ElementType="ClaimType" ElementId="email" StringId="DisplayName">Email</LocalizedString>
      <LocalizedString ElementType="ClaimType" ElementId="email" StringId="UserHelpText">Please enter your email</LocalizedString>
        <LocalizedString ElementType="ClaimType" ElementId="email" StringId="PatternHelpText">Please enter a valid email address</LocalizedString>
        <LocalizedString ElementType="UxElement" StringId="button_continue">Create new account</LocalizedString>
       <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfClaimsPrincipalAlreadyExists">The account you are trying to create already exists, please sign-in.</LocalizedString>
        <!-- More localized strings... -->
      </LocalizedStrings>
    </LocalizedResources>

    <LocalizedResources Id="api.localaccountsignup.es">
      <LocalizedCollections>
       <LocalizedCollection ElementType="ClaimType" ElementId="Gender" TargetCollection="Restriction">
          <Item Text="Femenino" Value="F" />
          <Item Text="Masculino" Value="M" />
        </LocalizedCollection>
        <LocalizedCollection ElementType="ClaimType" ElementId="City" TargetCollection="Restriction">
          <Item Text="Nueva York" Value="NY" />
          <Item Text="París" Value="Paris" />
          <Item Text="Londres" Value="London" />
        </LocalizedCollection>
      </LocalizedCollections>
      <LocalizedStrings>
        <LocalizedString ElementType="ClaimType" ElementId="email" StringId="DisplayName">Dirección de correo electrónico</LocalizedString>
        <LocalizedString ElementType="ClaimType" ElementId="email" StringId="UserHelpText">Dirección de correo electrónico que puede usarse para ponerse en contacto con usted.</LocalizedString>
        <LocalizedString ElementType="ClaimType" ElementId="email" StringId="PatternHelpText">Introduzca una dirección de correo electrónico.</LocalizedString>
        <LocalizedString ElementType="UxElement" StringId="button_continue">Crear</LocalizedString>
      <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfClaimsPrincipalAlreadyExists">Ya existe un usuario con el id. especificado. Elija otro diferente.</LocalizedString>
      </LocalizedStrings>
    </LocalizedResources>
    <!-- More localized resources... -->
  </Localization>
</BuildingBlocks>
```




