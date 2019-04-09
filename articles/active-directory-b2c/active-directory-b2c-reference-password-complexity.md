---
title: Parola karmaşıklığını - Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C, tüketicilere tarafından sağlanan parola karmaşıklık gereksinimlerini yapılandırma
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 1d7874d7c8d04e3d3565cdfe2e52e49c538b3091
ms.sourcegitcommit: e43ea344c52b3a99235660960c1e747b9d6c990e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "59009810"
---
# <a name="configure-complexity-requirements-for-passwords-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de parola karmaşıklık gereksinimlerini yapılandırma

Azure Active Directory (Azure AD) B2C, bir hesabı oluştururken bir son kullanıcı tarafından sağlanan parola karmaşıklık gereksinimleri değiştirmeyi destekler. Varsayılan olarak, Azure AD B2C kullanır `Strong` parola. Azure AD B2C, ayrıca müşteriler parola karmaşıklığı denetlemek için yapılandırma seçeneklerini destekler.

## <a name="password-rule-enforcement"></a>Parola kural zorlama

Kaydolma sırasında veya parola sıfırlama, bir son kullanıcı sağlaması gerekir karmaşıklığı kurallarına uyan bir parola. Parola karmaşıklığı kurallarına kullanıcı akışı uygulanır. Bir kullanıcı akışı kaydolma while sırasında başka bir kullanıcı akışı sekiz karakter dizisi kayıt sırasında gerektiren dört basamaklı bir PIN gerekli olması mümkündür. Örneğin, farklı bir parola karmaşıklık kullanıcı akışı yetişkinler alt öğe için kullanabilir.

Parola karmaşıklığını hiç oturum açma sırasında zorlanır. Kullanıcıların hiçbir zaman oturum açma sırasında geçerli karmaşıklık gereksinimini karşılamadığı için parola değiştirmesi istenir.

Parola karmaşıklığı aşağıdaki kullanıcı akışları türlerinde yapılandırılabilir:

- Kaydolma veya oturum açma kullanıcı akışı
- Parola sıfırlama kullanıcı akışı

Özel ilkeleri kullanıyorsanız, aşağıdakileri yapabilirsiniz ([parola karmaşıklığını özel bir ilke yapılandırmak](active-directory-b2c-reference-password-complexity-custom.md)).

## <a name="configure-password-complexity"></a>Parola karmaşıklığını yapılandırın

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
4. Seçin **kullanıcı akışları**.
2. Kullanıcı akışı seçin ve tıklayın **özellikleri**.
3. Altında **parola karmaşıklığını**, değiştirmek için bu kullanıcı akışını için parola karmaşıklığını **basit**, **güçlü**, veya **özel**.

### <a name="comparison-chart"></a>Karşılaştırma grafiği

| Karmaşıklık | Açıklama |
| --- | --- |
| Basit | En az 8 ila 64 karakter bir parola. |
| Güçlü | En az 8 ila 64 karakter bir parola. 3 / 4 küçük harf, küçük büyük harf, sayı veya semboller gerektirir. |
| Özel | Bu seçenek parola karmaşıklık kurallarını üzerinden en fazla denetimi sağlar.  Özel bir uzunluk yapılandırma sağlar.  Yalnızca sayı parolaları (PIN'ler) kabul etmesini sağlar. |

## <a name="custom-options"></a>Özel seçenekleri

### <a name="character-set"></a>Karakter kümesi

Yalnızca rakam (PIN'ler) ya da tam kabul etmenizi sağlayan karakter kümesi.

- **Yalnızca sayı** parola girerken basamak yalnızca (0-9) sağlar.
- **Tüm** herhangi bir harf, sayı veya sembol sağlar.

### <a name="length"></a>Uzunluk

Parola uzunluğu gereksinimleri denetlemenize olanak tanır.

- **En az uzunluk** en az 4 olmalıdır.
- **En fazla uzunluk** büyük veya buna eşit en küçük uzunluk olmalıdır ve en fazla 64 karakter olabilir.

### <a name="character-classes"></a>Karakter sınıfları

Parolada kullanılan farklı karakter türleri denetlemenizi sağlar.

- **2 / 4: Küçük harf karakter, büyük harf karakter, sayı (0-9), sembol** parola içeren en az iki karakter türleri sağlar. Örneğin, bir sayı ve küçük harf karakter.
- **3 / 4: Küçük harf karakter, büyük harf karakter, sayı (0-9), sembol** parola içeren en az iki karakter türleri sağlar. Örneğin, bir sayı, bir küçük harf ve bir büyük harf karakteri.
- **4 / 4: Küçük harf karakter, büyük harf karakter, sayı (0-9), sembol** parola içeren tüm karakter türleri sağlar.

    > [!NOTE]
    > Gerektiren **4 / 4** son kullanıcı sıkıntıya yol açabilir. Bu gereksinim parola entropi iyileştirmez bazı çalışmalar gösterilmiştir. Bkz: [NIST parola yönergeleri](https://pages.nist.gov/800-63-3/sp800-63b.html#appA)
