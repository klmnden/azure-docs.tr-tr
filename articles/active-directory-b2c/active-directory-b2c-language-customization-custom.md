---
title: Özel ilkeler, Azure Active Directory B2C:Language özelleştirme | Microsoft Docs
description: Nasıl kullanacağınızı öğrenin özel ilkeler birden çok dil için içeriği yerelleştirme
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 11/13/2017
ms.author: davidmu
ms.openlocfilehash: 45cfa152615da1447cc695e0dd201e5b8d046cf4
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="language-customization-in-custom-policies"></a>Özel ilkelerinde dil özelleştirme

> [!NOTE]
> Bu özellik genel önizlemede değil.
> 

Özel ilkelerinde dil özelleştirme yerleşik ilkeleri olduğu gibi aynı şekilde çalışır.  Yerleşik bkz [belgelerine](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-language-customization) nasıl bir dil parametreleri ve tarayıcı ayarlarını göre seçilir içinde davranışı açıklar.

## <a name="enable-supported-languages"></a>Etkinleştirme desteklenen diller
Kullanıcı arabirimi yerel ayarlar belirtilmedi ve bu dillerden biri için kullanıcının tarayıcısına ister, desteklenen diller kullanıcıya gösterilir.  

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

Varsayılan dil ve desteklenen dilleri yerleşik ilkelerinde olduğu gibi aynı şekilde davranır.

## <a name="enable-custom-language-strings"></a>Özel dil dizeleri etkinleştir

Özel dil Dizeler oluşturma iki adımı gerektirir:
1. Düzen `<ContentDefinition>` sayfanın istenen diller için bir kaynak kimliği belirtmek
2. Oluşturma `<LocalizedResources>` içinde karşılık gelen kimlikleri, `<BuildingBlocks>`

Koyabilirsiniz göz önünde bulundurmanız bir `<ContentDefinition>` ve `<BuildingBlock>` uzantısı dosyanıza veya, devralan tüm ilkelerinizi olmayacağı değişiklikleri mi istediğinize bağlı olarak bağlı olan ilke dosyası.

### <a name="edit-the-contentdefinition-for-the-page"></a>Sayfa için ContentDefinition Düzenle

Her bir sayfa için yerelleştirme istediğiniz belirleyebilirsiniz `<ContentDefinition>` her dil kodunu aramak için hangi Türkçe kaynaklar.

```XML
<ContentDefinition Id="api.signuporsignin">
      <LocalizedResourcesReferences>
        <LocalizedResourcesReference Language="fr" LocalizedResourcesReferenceId="fr" />
        <LocalizedResourcesReference Language="en" LocalizedResourcesReferenceId="en" />
      </LocalizedResourcesReferences>
</ContentDefinition>
```

Bu örnekte, Fransızca (fr) ve İngilizce (TR) özel dizeleri birleşik kayıt veya oturum açma sayfasına eklenir.  `LocalizedResourcesReferenceId` Her `LocalizedResourcesReference` kendi yerel ayar ile aynıdır, ancak herhangi bir dize kimliği olarak kullanabilir  Her dil ve sayfa birleşimi için karşılık gelen oluşturmak zorunda `<LocalizedResources>` aşağıda gösterildiği.


### <a name="create-the-localizedresources"></a>LocalizedResources oluşturma

Geçersiz kılmalarınızın bulunan, `<BuildingBlocks>` ve bir `<LocalizedResources>` her bir sayfa ve dil için belirttiğiniz `<ContentDefinition>` her sayfa için.  Her geçersiz kılma olarak belirtilen bir `<LocalizedString>` aşağıdaki örnekteki gibi böyle:

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

**ClaimsProvider** -etiketleri, kimlik sağlayıcısı (Facebook, Google, Azure AD vb.) için **ClaimType** -etiketleri, öznitelikleri ve bunların karşılık gelen için Yardım metni veya alan doğrulama hataları **UxElement** - diğer dize düğmeleri, bağlantılar veya metin gibivarsayılanolarakvarolanöğelerisayfada**ErrorMessage** -Form doğrulama hata iletileri

Emin `StringId`s eşleşen sayfa için karşıya yükleme doğrulamasını ilke tarafından engellendi bu geçersiz kılmaları aksi kullanıyorsanız.  