---
title: Azure Active Directory B2C'de yaş geçidi etkinleştirme | Microsoft Docs
description: Uygulamanızı kullanarak reşit olmayanların tanımlama hakkında bilgi edinin.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 083ed7209efd88d3d221b55cfb53fe3998dd2987
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64703278"
---
# <a name="enable-age-gating-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de yaş geçidi etkinleştir

>[!IMPORTANT]
>Bu özellik genel önizleme aşamasındadır. Özelliği, üretim uygulamaları için kullanmayın. 
>

Azure Active Directory (Azure AD) B2C'de yaş geçidi uygulamanızı kullanmak istediğiniz reşit olmayanların tanımlamanızı sağlar. Küçük uygulamasına açmasını engellemeyi seçebilirsiniz. Kullanıcılar da uygulamaya geri dönün ve kendi yaş grubu ve ebeveyn izni durumlarını tanımlar. Azure AD B2C reşit olmayanların ebeveyn izni olmadan engelleyebilir. Azure AD B2C ile reşit olmayanların karar uygulamaya izin vermek için de ayarlanabilir.

İçinde yaş geçidi etkinleştirdikten sonra [kullanıcı akışı](active-directory-b2c-reference-policies.md), kullanıcıların ne zaman geliştirilen ve hangi ülke sorulan Canlı. Bilgileri daha önce girilen edilmemiş bir kullanıcı oturum açtığında, oturum açtığında girmeniz gerekir. Bir kullanıcı her oturum açtığında kuralları uygulanır.

Küçük oldukları olup olmadığını belirlemek için kullanıcının girdiği bilgileri Azure AD B2C kullanır. **Yaş** hesaplarındaki alan güncelleştirildikten sonra. Değer olabilir `null`, `Undefined`, `Minor`, `Adult`, ve `NotAdult`.  **Yaş** ve **consentProvidedForMinor** alanları değerini hesaplamak için kullanılan ardından **legalAgeGroupClassification**.

Yaş geçidi iki yaş değerleri içerir: artık kişidir yaş kabul küçük ve yaş, küçük ebeveyn izni olmalıdır. Aşağıdaki tabloda, küçük ve önemsiz gerektirmeden onayı tanımlamak için kullanılan yaş kuralları listeler.

| Ülke | Ülke adı | Alt onay yaş | Küçük yaş |
| ------- | ------------ | ----------------- | --------- |
| Varsayılan | None | None | 18 |
| AE | Birleşik Arap Emirlikleri | None | 21 |
| AT | Avusturya | 14 | 18 |
| BE | Belçika | 14 | 18 |
| BG | Bulgaristan | 16 | 18 |
| BH | Bahreyn | None | 21 |
| CM | Kamerun | None | 21 |
| CY | Kıbrıs | 16 | 18 |
| CZ | Çek Cumhuriyeti | 16 | 18 |
| DE | Almanya | 16 | 18 |
| DK | Danimarka | 16 | 18 |
| EE | Estonya | 16 | 18 |
| EG | Mısır | None | 21 |
| ES | İspanya | 13 | 18 |
| GS | Fransa | 16 | 18 |
| GB | Birleşik Krallık | 13 | 18 |
| GR | Yunanistan | 16 | 18 |
| HR | Hırvatistan | 16 | 18 |
| HU | Macaristan | 16 | 18 |
| IE | İrlanda | 13 | 18 |
| BT | İtalya | 16 | 18 |
| KR | Kore Cumhuriyeti | 14 | 18 |
| LT | Litvanya | 16 | 18 |
| LU | Lüksemburg | 16 | 18 |
| LV | Letonya | 16 | 18 |
| MT | Malta | 16 | 18 |
| NA | Namibya | None | 21 |
| NL | Hollanda | 16 | 18 |
| PL | Polonya | 13 | 18 |
| PT | Portekiz | 16 | 18 |
| RO | Romanya | 16 | 18 |
| SE | İsveç | 13 | 18 |
| SG | Singapur | None | 21 |
| SI | Slovenya | 16 | 18 |
| SK | Slovakya | 16 | 18 |
| TD | Çad | None | 21 |
| TH | Tayland | None | 20 |
| TW | Tayvan | None | 20 | 
| ABD | Amerika Birleşik Devletleri | 13 | 18 |

## <a name="age-gating-options"></a>Yaş geçidi seçenekleri
 
### <a name="allowing-minors-without-parental-consent"></a>Reşit olmayanların ebeveyn izni olmadan izin verme

İzin vermek ya da kaydolma, oturum açma kullanıcı akışları için veya her ikisi de olmadan reşit olmayanların uygulamanıza izin vermeyi seçebilirsiniz. Reşit olmayanların ebeveyn izni olmadan normal ve Azure AD B2C ile bir kimlik belirteci sorunları olarak kaydolun veya oturum izin verilir **legalAgeGroupClassification** talep. Bu talep, ebeveyn izni toplama ve güncelleştirme gibi kullanıcınız deneyimini tanımlar **consentProvidedForMinor** alan.

### <a name="blocking-minors-without-parental-consent"></a>Reşit olmayanların ebeveyn izni olmadan engelleme

Kaydolma, oturum açma ya da her ikisini de izin kullanıcı akışları için reşit olmayanların uygulamadan onayınız olmadan engellemeyi seçebilirsiniz. Engellenen kullanıcılar Azure AD B2C'yi işlemek için aşağıdaki seçenekler kullanılabilir:

- Uygulamaya bir JSON gönder - Bu seçenek, bir ikincil engellenen uygulama geri yanıt gönderir.
- Bir hata sayfası - kullanıcının uygulama erişemeyeceklerini bildiren bir sayfası gösterilmektedir gösterir.

## <a name="set-up-your-tenant-for-age-gating"></a>Kiracı yaş geçidi için ayarlama

Kullanıcı akışı yaş geçidi kullanmak için ek özellikler sağlamak için kiracınızda yapılandırmanız gerekir.

1. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menüdeki. Kiracınızın içeren dizini seçin. 
2. Seçin **tüm hizmetleri** Azure portalının sol üst köşedeki arayın ve seçin **Azure AD B2C**.
3. Seçin **özellikleri** sol taraftaki menüden kiracınız için.
2. Altında **yaş geçidi** bölümünde, tıklayarak **yapılandırma**.
3. İşlemin tamamlanmasını bekleyin ve Kiracı yaş geçidi için ayarlanır.

## <a name="enable-age-gating-in-your-user-flow"></a>Kullanıcı akışınızı yaş geçidi etkinleştir

Kiracınızı kullanmak yaş geçidi kadar ayarlanmış sonra daha sonra bu özelliği kullanabilirsiniz [kullanıcı akışları](user-flow-versions.md) , burada etkinleştirilir. Aşağıdaki adımlarla yaş geçidi sağlar:

1. Etkin yaş geçidi olan kullanıcı akışı oluşturun.
2. Kullanıcı akışını oluşturduktan sonra seçin **özellikleri** menüsünde.
3. İçinde **yaş geçidi** bölümünden **etkin**.
4. Ardından nasıl reşit olmayanların tanımlayan kullanıcıları yönetmek istediğinize karar verin. İçin **kaydolma veya oturum açma**, seçtiğiniz `Allow minors to access your application` veya `Block minors from accessing your application`. Reşit olmayanların engelleme seçili ise, seçtiğiniz `Send a JSON back to the application` veya `Show an error message`. 




