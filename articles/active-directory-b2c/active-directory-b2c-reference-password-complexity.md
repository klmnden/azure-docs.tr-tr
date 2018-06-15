---
title: Azure Active Directory B2C parola karmaşıklığı | Microsoft Docs
description: Azure Active Directory B2C tüketici tarafından sağlanan parola karmaşıklık gereksinimlerini yapılandırmak nasıl.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 08/16/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 617385bec29e2c09ae18bde3a7878624c0b2461f
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34709844"
---
# <a name="azure-ad-b2c-configure-complexity-requirements-for-passwords"></a>Azure AD B2C: parolaların karmaşıklık gereksinimlerini yapılandırabilirsiniz

> [!NOTE]
> **Bu özellik genel önizlemede değil.**

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

### <a name="length"></a>Uzunluk

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
