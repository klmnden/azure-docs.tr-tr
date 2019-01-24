---
title: BuildingBlocks - Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C'de özel bir ilke BuildingBlocks öğesi belirtin.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 3075c047dd4ff42d0578a6824d95a0f4b822292a
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54848555"
---
# <a name="buildingblocks"></a>BuildingBlocks

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

**BuildingBlocks** öğe içinde eklenen [TrustFrameworkPolicy](trustframeworkpolicy.md) öğesi.

```XML
<TrustFrameworkPolicy
  xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
  xmlns:xsd="https://www.w3.org/2001/XMLSchema"
  xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
  PolicySchemaVersion="0.3.0.0"
  TenantId="mytenant.onmicrosoft.com"
  PolicyId="B2C_1A_TrustFrameworkBase"
  PublicPolicyUri="http://mytenant.onmicrosoft.com/B2C_1A_TrustFrameworkBase">

  <BuildingBlocks>
    <ClaimsSchema>
      ...
    </ClaimsSchema>
    <Predicates>
    ...
    </Predicates>
    <PredicateValidations>
    ...
    </PredicateValidations>
    <ClaimsTransformations>
      ...
    </ClaimsTransformations>
    <ContentDefinitions>
      ...
    </ContentDefinitions>
    <Localization>
      ...
    </Localization>
 </BuildingBlocks>
```

**BuildingBlocks** öğesi tanımlı sırada belirtilen aşağıdaki öğeleri içerir:

- [ClaimsSchema](claimsschema.md) -ilkenin bir parçası başvurulabilir talep türlerini tanımlar. Talep şema, talep türleri bildirme burada yerdir. Bir talep türü, birçok programlama dili bir değişkende benzerdir. Özel ilkeniz tarafından kullanılan iç veri depolamak veya uygulamanızın kullanıcıdan veri toplamak, sosyal kimlik sağlayıcılarını talepleri almak, veri göndermek ve özel bir REST API'si almak için talep türünü kullanın. 

- [Koşullar ve PredicateValidationsInput](predicates.md) -düzgün biçimlendirilmiş veriler yalnızca bir talep girildiğinden emin olmak için bir doğrulama işlemi gerçekleştirmenizi sağlar.
 
- [ClaimsTransformations](claimstransformations.md) -ilkenizde kullanılabilir talep dönüştürmeleri listesini içerir.  Talep dönüştürme bir talep diğerine dönüştürür. Talep dönüştürme bir dönüştürme yöntemi gibi belirtin: 
    - Belirtilen dize talep durumunu değiştirme. Örneğin, bir dize küçük büyük harf olarak değiştiriliyor.
    - İki talep karşılaştırma ve bir talep belirten true ile eşleşen talepleri, aksi takdirde false döndüren.
    - İlkede sağlanan parametre bir dize talep oluşturma.
    - Rastgele sayı oluşturucusunu kullanarak rastgele bir dize oluşturma.
    - Bir talep göre sağlanan biçim dizesi biçimlendiriliyor. Bu dönüşüm C# kullanan `String.Format` yöntemi.

- [ContentDefinitions](contentdefinitions.md) -içeren kullanıcı yolculuğunuza kullanılacak URL'leri HTML5 şablonları için. Özel bir ilke içerik tanımı HTML5 sayfa için belirtilen bir kullanıcı yolculuğu adımda kullanılan URI tanımlar. Örneğin, oturum açma veya kaydolma, parola sıfırlama veya hata sayfaları. LoadUri HTML5 dosyası için geçersiz kılarak görünümünü değiştirebilirsiniz. Veya yeni içerik tanımlarını ihtiyaçlarınıza göre oluşturabilirsiniz. Bu öğe bir yerelleştirme kimliği kullanarak yerelleştirilmiş kaynaklar başvuru içerebilir

- [Yerelleştirme](localization.md) -birden çok dil desteği sağlar. Varsayılan bir dil seçin ve bir ilkede desteklenen dillerin listesini ayarlayın ilkelerinde yerelleştirme desteğini sağlar. Dile özgü dizeleri ve koleksiyonları da desteklenir.


