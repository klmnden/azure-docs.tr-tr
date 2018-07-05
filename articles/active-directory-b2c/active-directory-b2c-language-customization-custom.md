---
title: Dil özelleştirme özel ilkeleri Azure Active Directory B2C | Microsoft Docs
description: Nasıl kullanacağınızı öğrenin birden çok dil için özel ilkeler, içeriği yerelleştirmek.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/13/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 6269ac65e5db20521346d5312bcbadd0905c36e2
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37440573"
---
# <a name="language-customization-in-custom-policies"></a>Dil özelleştirme, özel ilkeler

> [!NOTE]
> Bu özellik genel Önizleme aşamasındadır.
> 

Dil özelleştirme özel ilkeleri yerleşik ilkeleri olduğu gibi çalışır.  Yerleşik bkz [belgeleri](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-language-customization) nasıl bir dil parametreleri ve tarayıcı ayarlarını göre seçilir, davranışı açıklar.

## <a name="enable-supported-languages"></a>Etkinleştirme desteklenen diller
Kullanıcı arabirimi yerel belirtilmedi ve bu dillerden biri için kullanıcının tarayıcıyı ister, desteklenen diller için kullanıcıya gösterilir.  

Desteklenen diller tanımlanmış `<BuildingBlocks>` şu biçimde:

```XML
<BuildingBlocks>
  <Localization>
    <SupportedLanguages DefaultLanguage="en">
      <SupportedLanguage>qps-ploc</SupportedLanguage>
      <SupportedLanguage>en</SupportedLanguage>
    </SupportedLanguages>
  </Localization>
</BuildingBlocks>
```

Varsayılan dil ve desteklenen dilleri, yerleşik ilkeleri gibi aynı şekilde davranır.

## <a name="enable-custom-language-strings"></a>Özel dil dizeleri etkinleştir

Özel dil dizeleri oluşturmak için iki adım gerekir:
1. Düzen `<ContentDefinition>` sayfanın istediğiniz dil için kaynak kimliği belirlemek
2. Oluşturma `<LocalizedResources>` içinde karşılık gelen kimlikleri, `<BuildingBlocks>`

Koyabilirsiniz akılda tutulması bir `<ContentDefinition>` ve `<BuildingBlock>` hem, uzantı dosyası ya da değişiklikleri tüm türetilen ilkelerinizi olabileceği için istediğinize bağlı olarak bağlı olan ilke dosyası.

### <a name="edit-the-contentdefinition-for-the-page"></a>Sayfa için ContentDefinition Düzenle

Her sayfa için Yerelleştirmek istediğiniz belirleyebilirsiniz `<ContentDefinition>` her dil kodunu aramak için hangi dil kaynakları.

```XML
<ContentDefinition Id="api.signuporsignin">
      <LocalizedResourcesReferences>
        <LocalizedResourcesReference Language="fr" LocalizedResourcesReferenceId="fr" />
        <LocalizedResourcesReference Language="en" LocalizedResourcesReferenceId="en" />
      </LocalizedResourcesReferences>
</ContentDefinition>
```

Bu örnekte, Fransızca (fr) ve İngilizce (TR) özel dizeleri birleşik kaydolma veya oturum açma sayfasına eklenir.  `LocalizedResourcesReferenceId` Her `LocalizedResourcesReference` kendi yerel ayar ile aynıdır, ancak herhangi bir dize kimliği olarak kullanabilir  Her dil ve sayfa birleşimi için karşılık gelen oluşturmak zorunda `<LocalizedResources>` aşağıda gösterilmektedir.


### <a name="create-the-localizedresources"></a>LocalizedResources oluşturma

Geçersiz kılmaları bulunan, `<BuildingBlocks>` ve bir `<LocalizedResources>` her bir sayfasını ve dil için belirttiğiniz `<ContentDefinition>` her sayfa için.  Her bir geçersiz kılma olarak belirtilen bir `<LocalizedString>` aşağıdaki örnekte olduğu gibi:

```XML
<BuildingBlocks>
  <Localization>
    <LocalizedResources Id="en">
      <LocalizedStrings>
        <LocalizedString ElementType="ClaimsProvider" StringId="SignInWithLogonNameExchange">Local Account Sign-in</LocalizedString>
        <LocalizedString ElementType="ClaimType" ElementId="UserId" StringId="DisplayName">Username</LocalizedString>
        <LocalizedString ElementType="ClaimType" ElementId="UserId" StringId="UserHelpText">Username used for signing in.</LocalizedString>
        <LocalizedString ElementType="ClaimType" ElementId="UserId" StringId="PatternHelpText">The username you provided is not valid.</LocalizedString>
        <LocalizedString ElementType="UxElement" StringId="button_signin">Sign In Now</LocalizedString>
        <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfInvalidPassword">Your password is incorrect.</LocalizedString>
      </LocalizedStrings>
    </LocalizedResources>
  </Localization>
</BuildingBlocks>
```

Dize öğeleri sayfada dört tür vardır:

**ClaimsProvider** -(Facebook, Google, vb. Azure AD) kimlik sağlayıcılarınızın etiketlerini **ClaimType** -etiketler, öznitelikleri ve bunlara karşılık gelen için Yardım metni veya alan doğrulama hatalarını **UxElement** - diğer dize düğmeler, bağlantılar veya metin gibivarsayılanolarakvarolanöğelerisayfada**ErrorMessage** -Form doğrulama hatası iletileri

Emin `StringId`s karşıya yükleme sırasında doğrulama İlkesi tarafından engellendi bu geçersiz kılmaları aksi kullanmakta olduğunuz sayfayı eşleşen.  