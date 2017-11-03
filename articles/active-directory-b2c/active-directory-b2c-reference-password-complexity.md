---
title: "Parola karmaşıklığını - Azure AD B2C | Microsoft Docs"
description: "Azure Active Directory B2C tüketici tarafından sağlanan parola karmaşıklık gereksinimlerini yapılandırma"
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: 53ef86c4-1586-45dc-9952-dbbd62f68afc
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: saeeda
ms.openlocfilehash: 800199b3d1d91d700d26f988a4d49713028de1ce
ms.sourcegitcommit: d6ad3203ecc54ab267f40649d3903584ac4db60b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2017
---
# <a name="azure-ad-b2c-configure-complexity-requirements-for-passwords"></a>Azure AD B2C: parolaların karmaşıklık gereksinimlerini yapılandırabilirsiniz

> [!NOTE]
> **Bu özelliğin önizlemede değil.**  Kişi [ AADB2CPreview@microsoft.com ](mailto:AADB2CPreview@microsoft.com) bu özellik ile etkin test Kiracı sağlamak için.

Azure Active Directory B2C (Azure AD B2C) destekleyen bir hesap oluşturulurken bir son kullanıcı tarafından sağlanan parola karmaşıklık gereksinimlerini değiştirme.  Varsayılan olarak, Azure AD B2C kullanır `Strong` parolalar.  Azure AD B2C da müşterilerin kullanabileceğiniz parolaların karmaşıklık denetlemek için yapılandırma seçeneklerini destekler.

## <a name="when-password-rules-are-enforced"></a>Parola kuralları zaman uygulanır

Kayıt sırasında veya parola sıfırlama, son kullanıcının gerekir sağlayın karmaşıklık kurallarını karşılayan bir parola.  Parola karmaşıklık kurallarını ilke uygulanır.  Kaydolma while sırasında başka bir ilke sekiz karakter dizesi kayıt sırasında gerektirir dört basamaklı bir PIN gerektiren bir ilke olması mümkündür.  Örneğin, çocuklar için yetişkinler için farklı parola karmaşıklık ile bir ilke kullanabilirsiniz.

Parola karmaşıklığını hiçbir zaman oturum açma sırasında uygulanır.  Kullanıcıların hiçbir zaman oturum açma sırasında geçerli karmaşıklık gereksinimini karşılamıyor olduğundan, parolalarını değiştirip istenir.

Parola karmaşıklığını yapılandırılabileceği ilke türleri şunlardır:

* Kaydolma veya oturum açma ilkesi
* Parola sıfırlama İlkesi
* Özel ilke ([özel İlkesi'nde parola karmaşıklığını yapılandırma](active-directory-b2c-reference-password-complexity-custom.md))

## <a name="how-to-configure-password-complexity"></a>Parola karmaşıklığını yapılandırma

1. Aşağıdaki adımları izleyin [Azure AD B2C ayarlarına gidin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
1. Açık **oturum açma veya kaydolma ilkeleri**.
1. Bir ilke seçin ve tıklayın **Düzenle**.
1. Açık **parola karmaşıklığını**.
1. Bu ilke için parola karmaşıklığını değiştirme **basit**, **güçlü**, veya **özel**.

### <a name="comparison-chart"></a>Karşılaştırma grafiği

| Karmaşıklık | Açıklama |
| --- | --- |
| Basit | En az 8-64 karakter olan bir parola. |
| Güçlü | En az 8-64 karakter olan bir parola. Küçük harfler, büyük harf, sayı veya simgeleri 3 dışı 4 gerektirir. |
| Özel | Bu seçenek parola karmaşıklık kurallarını üzerinde çoğu denetim sağlar.  Bu, özel bir uzunluk yapılandırılmasına olanak tanır.  Yalnızca sayı parolalar (PIN'ler) kabul etmesini sağlar. |

## <a name="options-available-under-custom"></a>Kullanılabilir seçenekler özel altında

### <a name="character-set"></a>Karakter kümesi

Basamak yalnızca (PIN'ler) ya da tam kabul etmenizi sağlayan karakter kümesi.

* **Yalnızca sayı** parola yazarken rakam yalnızca (0-9) sağlar.
* **Tüm** herhangi bir harf, rakam veya sembol sağlar.

### <a name="length"></a>uzunluğu

Parola uzunluğu gereksinimlerini denetlemenize olanak verir.

* **En az uzunluk** en az 4 olmalıdır.
* **En fazla uzunluk** sıfırdan büyük veya en az uzunluk eşit olmalı ve en fazla 64 karakter olabilir.

### <a name="character-classes"></a>Karakter sınıfları

Parolada kullanılan farklı karakter türlerini denetlemenize olanak verir.

* **4 2: küçük harf karakter, büyük harf karakter, sayı (0-9), simge** parola en az iki karakter türleri içeriyor sağlar. Örneğin, bir sayı ve küçük harf karakter.
* **3, 4: küçük harf karakter, büyük harf karakter, sayı (0-9), simge** parola en az iki karakter türleri içeriyor sağlar. Örneğin, bir sayı, bir küçük harf ve bir büyük harf karakter.
* **4'ün 4: küçük harf karakter, büyük harf karakter, sayı (0-9), simge** parola içeren tüm karakter türleri için sağlar.

    > [!NOTE]
    > Gerektiren **4 4** son kullanıcı aksiliklerin neden olabilir. Bazı olay incelemeleri, bu gereksinim parola entropi iyileştirmez göstermiştir. Bkz: [NIST parola yönergeleri](https://pages.nist.gov/800-63-3/sp800-63b.html#appA)
