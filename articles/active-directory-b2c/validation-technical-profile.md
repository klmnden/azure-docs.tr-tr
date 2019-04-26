---
title: Azure Active Directory B2C, özel bir ilke doğrulama teknik profil tanımlama | Microsoft Docs
description: Azure Active Directory B2C özel bir ilke Azure Active Directory teknik profili tanımlayın.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 466ed4c2bd353e4a5ec3bec5535b70a90446ee0b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60418305"
---
# <a name="define-a-validation-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel bir ilke doğrulama teknik profil tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Herhangi bir protokolden sıradan bir teknik profili doğrulama teknik profil olduğu gibi [Azure Active Directory](active-directory-technical-profile.md) veya [REST API](restful-technical-profile.md). Doğrulama teknik profili, çıkış talep döndürür veya bir HTTP 409 (çakışma yanıt durum kodu), hata iletisi aşağıdaki verilerle döndürür:

```JSON
{
    "version": "1.0.0",
    "status": 409,
    "userMessage": "Your error message"
}
```

Doğrulama teknik profilinden döndürülen talepler, talep paketi için geri eklenir. Sonraki doğrulama teknik profillerinde bu talepleri kullanabilirsiniz.

Doğrulama teknik profiller içinde göründükleri sırayla yürütülen **ValidationTechnicalProfiles** öğesi. Herhangi bir sonraki doğrulama teknik profil yürütülmesi devam etmelidir doğrulama teknik profili başlatan bir hata veya başarılı olup olmadığını doğrulama teknik profili içinde yapılandırabilirsiniz.

Teknik profil koşullu olarak yürütülebilir bir doğrulama tanımlanan önkoşulları temel **ValidationTechnicalProfile** öğesi. Örneğin, belirli bir talep var olup olmadığını veya talebi eşitse veya belirtilen değer için kontrol edebilirsiniz.

Otomatik olarak onaylanan bir teknik profili, bazıları veya tümü, çıkış talep doğrulamak için kullanılacak bir doğrulama teknik profilini tanımlayabilir. Tüm giriş talepleri başvurulan teknik profili, başvuru doğrulama teknik profili çıkış Taleplerde yer almalıdır.

## <a name="validationtechnicalprofiles"></a>ValidationTechnicalProfiles

**ValidationTechnicalProfiles** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşumlar | Açıklama |
| ------- | ----------- | ----------- |
| ValidationTechnicalProfile | 1:n | Bazı veya tüm başvuru teknik profili, çıkış talep doğrulamak için kullanılan bir teknik profili. |

**ValidationTechnicalProfile** öğesi aşağıdaki öznitelik içeriyor:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ReferenceId | Evet | İlke veya üst ilkede zaten tanımlanmış bir teknik profili tanımlayıcısı. |
|ContinueOnError|Hayır| Bu doğrulama teknik profili, bir hata harekete geçirirse herhangi bir sonraki doğrulama teknik profil doğrulama sürmelidir olup olmadığını belirten. Olası değerler: `true` veya `false` (varsayılan, daha fazla doğrulama profilleri işlenmesini durdurur ve bir hata döndürdü). |
|ContinueOnSuccess | Hayır | Herhangi bir sonraki doğrulama profil doğrulama başarılı olursa bu doğrulama teknik profili sürmelidir olup olmadığını belirten. Olası değerler: `true` veya `false`. Varsayılan `true`, daha fazla doğrulama profilleri işlenmesini devam edeceği anlamına gelir. |

**ValidationTechnicalProfile** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşumlar | Açıklama |
| ------- | ----------- | ----------- |
| Önkoşulları | 0:1 | Yürütülecek doğrulama teknik profil için karşılanması gereken önkoşulları listesi. |

**Önkoşulu** öğesi aşağıdaki öznitelik içeriyor:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| `Type` | Evet | Onay veya için önkoşul gerçekleştirmek için sorgu türü. Her iki `ClaimsExist` kullanıcının geçerli talep kümesinde belirtilen talep varsa, eylemlerin gerçekleştirildiğinden emin olun belirtilen veya `ClaimEquals` belirtilen talep varsa ve değeri eşittir Eylemler gerçekleştirilmelidir belirtilir Belirtilen değer. |
| `ExecuteActionsIf` | Evet | Test true veya false olursa önkoşuluna eylemlerin gerçekleştirilmesi gerekip gerekmediğini gösterir. |

**Önkoşulu** öğesi öğeleri içerir:

| Öğe | Oluşumlar | Açıklama |
| ------- | ----------- | ----------- |
| Değer | 1:n | Denetimi tarafından kullanılan veri. Bu denetim türünde ise `ClaimsExist`, bir ClaimTypeReferenceId sorgulamak için bu alanı belirtir. Onay türü ise `ClaimEquals`, bir ClaimTypeReferenceId sorgulamak için bu alanı belirtir. Başka bir değeri öğenin değeri, Kontrol edilecek içerirken.|
| Eylem | 1:1 | Önkoşul denetimi içinde bir düzenleme adımı doğruysa alınması gereken eylem. Değerini **eylem** ayarlanır `SkipThisValidationTechnicalProfile`. İlişkili doğrulama teknik profil çalıştırılmadı belirtir. |

### <a name="example"></a>Örnek

Aşağıdaki örnek, bu doğrulama teknik profiller kullanır:

1. İlk doğrulama teknik profili, kullanıcı kimlik bilgileri denetler ve bir hata, geçersiz kullanıcı adı veya hatalı parola gibi oluşursa devam etmez.
2. UserType talep mevcut değilse veya userType değerini ise sonraki doğrulama teknik profili, yürütme değil `Partner`. İç müşteri veritabanından kullanıcı profilini okuyun ve REST API Hizmet kullanılamıyor veya herhangi bir iç hata gibi bir hata oluşursa devam etmek doğrulama teknik profil çalışır.
3. Son doğrulama teknik profili, userType talep değil mevcutsa ya da userType değeri yürütme değil `Customer`. Doğrulama teknik profili, dahili partner veritabanından kullanıcı profilini okumaya çalışır ve REST API Hizmet kullanılamıyor veya herhangi bir iç hata gibi bir hata oluşursa devam eder.

```XML
<ValidationTechnicalProfiles>
  <ValidationTechnicalProfile ReferenceId="login-NonInteractive" ContinueOnError="false" />
  <ValidationTechnicalProfile ReferenceId="REST-ReadProfileFromCustomertsDatabase" ContinueOnError="true" >
    <Preconditions>
      <Precondition Type="ClaimsExist" ExecuteActionsIf="false">
        <Value>userType</Value>
        <Action>SkipThisValidationTechnicalProfile</Action>
      </Precondition>
      <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
        <Value>userType</Value>
        <Value>Partner</Value>
        <Action>SkipThisValidationTechnicalProfile</Action>
      </Precondition>
    </Preconditions>
  </ValidationTechnicalProfile>
  <ValidationTechnicalProfile ReferenceId="REST-ReadProfileFromPartnersDatabase" ContinueOnError="true" >
    <Preconditions>
      <Precondition Type="ClaimsExist" ExecuteActionsIf="false">
        <Value>userType</Value>
        <Action>SkipThisValidationTechnicalProfile</Action>
      </Precondition>
      <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
        <Value>userType</Value>
        <Value>Customer</Value>
        <Action>SkipThisValidationTechnicalProfile</Action>
      </Precondition>
    </Preconditions>
  </ValidationTechnicalProfile>
</ValidationTechnicalProfiles>
```
