---
title: Azure Active Directory B2C, parola karmaşıklığını | Microsoft Docs
description: Azure Active Directory B2C, tüketicilere tarafından sağlanan parola karmaşıklık gereksinimlerini yapılandırma
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/16/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: b16ac10e10655bbc7e41d9336378228097ca19ff
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51014729"
---
# <a name="azure-ad-b2c-configure-complexity-requirements-for-passwords"></a>Azure AD B2C: parola karmaşıklık gereksinimlerini yapılandırma

> [!NOTE]
> **Bu özellik genel Önizleme aşamasındadır.**

Azure Active Directory B2C (Azure AD B2C) destekleyen bir hesabı oluştururken bir son kullanıcı tarafından sağlanan parola karmaşıklık gereksinimleri değiştirme.  Varsayılan olarak, Azure AD B2C kullanır `Strong` parola.  Azure AD B2C, ayrıca müşteriler parola karmaşıklığı denetlemek için yapılandırma seçeneklerini destekler.

## <a name="when-password-rules-are-enforced"></a>Parola kuralları ne zaman uygulanır

Kaydolma sırasında veya parola sıfırlama, bir son kullanıcı sağlaması gerekir karmaşıklığı kurallarına uyan bir parola.  Parola karmaşıklığı kurallarına ilke uygulanır.  Kaydolma while sırasında başka bir ilke sekiz karakter dizesi kayıt sırasında gerektiren dört basamaklı bir PIN gerektiren bir ilke olması mümkündür.  Örneğin, yetişkinler alt öğe için farklı bir parola karmaşıklık bir ilke kullanabilirsiniz.

Parola karmaşıklığını hiç oturum açma sırasında zorlanır.  Kullanıcıların hiçbir zaman oturum açma sırasında geçerli karmaşıklık gereksinimini karşılamadığı için parola değiştirmesi istenir.

Parola karmaşıklığını yapılandırılabileceği ilke türleri şunlardır:

* Kaydolma veya oturum açma ilkesi
* Parola sıfırlama İlkesi
* Özel ilke ([parola karmaşıklığını özel İlkesi'nde yapılandırma](active-directory-b2c-reference-password-complexity-custom.md))

## <a name="how-to-configure-password-complexity"></a>Parola karmaşıklığını yapılandırma

1. Açık **kaydolma veya oturum açma ilkeleri**.
2. Bir ilke seçin ve tıklayın **Düzenle**.
3. Açık **parola karmaşıklığını**.
4. Bu ilke için bir parola karmaşıklığını değiştirme **basit**, **güçlü**, veya **özel**.

### <a name="comparison-chart"></a>Karşılaştırma grafiği

| Karmaşıklık | Açıklama |
| --- | --- |
| Basit | En az 8 ila 64 karakter bir parola. |
| Güçlü | En az 8 ila 64 karakter bir parola. 3 / 4 küçük harf, küçük büyük harf, sayı veya semboller gerektirir. |
| Özel | Bu seçenek parola karmaşıklık kurallarını üzerinden en fazla denetimi sağlar.  Özel bir uzunluk yapılandırma sağlar.  Yalnızca sayı parolaları (PIN'ler) kabul etmesini sağlar. |

## <a name="options-available-under-custom"></a>Seçeneklerin altında özel

### <a name="character-set"></a>Karakter kümesi

Yalnızca rakam (PIN'ler) ya da tam kabul etmenizi sağlayan karakter kümesi.

* **Yalnızca sayı** parola girerken basamak yalnızca (0-9) sağlar.
* **Tüm** herhangi bir harf, sayı veya sembol sağlar.

### <a name="length"></a>Uzunluk

Parola uzunluğu gereksinimleri denetlemenize olanak tanır.

* **En az uzunluk** en az 4 olmalıdır.
* **En fazla uzunluk** büyük veya buna eşit en küçük uzunluk olmalıdır ve en fazla 64 karakter olabilir.

### <a name="character-classes"></a>Karakter sınıfları

Parolada kullanılan farklı karakter türleri denetlemenizi sağlar.

* **2 / 4: küçük harf karakter, büyük harf karakter, sayı (0-9), sembol** parola içeren en az iki karakter türleri sağlar. Örneğin, bir sayı ve küçük harf karakter.
* **3 / 4: küçük harf karakter, büyük harf karakter, sayı (0-9), sembol** parola içeren en az iki karakter türleri sağlar. Örneğin, bir sayı, bir küçük harf ve bir büyük harf karakteri.
* **4 / 4: küçük harf karakter, büyük harf karakter, sayı (0-9), sembol** parola içeren tüm karakter türleri sağlar.

    > [!NOTE]
    > Gerektiren **4 / 4** son kullanıcı sıkıntıya yol açabilir. Bu gereksinim parola entropi iyileştirmez bazı çalışmalar gösterilmiştir. Bkz: [NIST parola yönergeleri](https://pages.nist.gov/800-63-3/sp800-63b.html#appA)
