---
title: En iyi N çevirileri - Translator metin çevirisi API'si döndürür
titlesuffix: Azure Cognitive Services
description: Microsoft Translator metin çevirisi API'si kullanarak en iyi N çevirileri döndürür.
services: cognitive-services
author: rajdeep-in
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: v-pawal
ms.openlocfilehash: 27138fc82515983bb07df845e1204fe04dff915a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66389668"
---
# <a name="how-to-return-n-best-translations"></a>En iyi N çevirileri iade etme

> [!NOTE]
> Bu metot kullanımdan kaldırılmıştır. Translator metin çevirisi API'si, V3.0 içinde kullanılabilir değil.

İsteğe bağlı mantıksal bayrak "IncludeMultipleMTAlternatives" Microsoft Translator API'si GetTranslations() ve GetTranslationsArray() yöntemlerini içerir.
Yöntemi, delta translator altyapısı en iyi N listesinden burada sağlanan maxTranslations alternatifleri kadar döndürür.

İmza değil:

**Söz dizimi**

| C# |
|:---|
| GetTranslationsResponse Microsoft.Translator.GetTranslations(appId, text, from, to, maxTranslations, options); |

**Parametreler**

| Parametre | Açıklama |
|:---|:---|
| appId | **Gerekli** yetkilendirme üst bilgisi kullandıysanız, AppID alanı boş bırakın başka belirtin "Bearer" içeren bir dize + "" + erişim belirteci.|
| metin | **Gerekli** Çevrilecek metin temsil eden bir dize. Metin boyutu 10000 karakterden uzun olmamalıdır.|
| from | **Gerekli** Çevrilecek metin dil kodunu temsil eden bir dize. |
| - | **Gerekli** metne çevirmek için dil kodunu temsil eden bir dize. |
| maxTranslations | **Gerekli** çevirileri döndürülecek en fazla sayısını temsil eden bir tamsayı. |
| options | **İsteğe bağlı** aşağıda listelenen değerler içeren bir TranslateOptions nesne. Bunlar tümü isteğe bağlıdır ve varsayılan en sık kullanılan ayarları için.

* Kategori: Desteklenen tek ve varsayılan olarak, "Genel" seçeneğidir.
* ContentType: Desteklenen tek ve "text/plain" varsayılan seçenektir.
* Durum: Performanstaki istek ve yanıt yardımcı olmak için kullanıcı durumu. Aynı içeriğini yanıta döndürülür.
* IncludeMultipleMTAlternatives: birden fazla alternatifleri MT altyapısından döndürülüp döndürülmeyeceğini belirlemek için bayrak. Varsayılan değer false'tur ve yalnızca 1 seçenek içerir.

## <a name="ratings"></a>Derecelendirme
Derecelendirmeleri şu şekilde uygulanır: En iyi bir otomatik çeviri, 5 derecesi vardır.
Otomatik olarak oluşturulan (en iyi N) çeviri alternatifleri de derecesi 0 ve 100 eşleşme derecesi sahiptir.

## <a name="number-of-alternatives"></a>Alternatifleri sayısı
Döndürülen alternatifleri sayısı kadar maxTranslations olmakla birlikte daha az olabilir.

## <a name="language-pairs"></a>Dil çiftleri
Bu işlev, Basitleştirilmiş ve Geleneksel Çince, her iki yönde de arasında çevirileri için kullanılamıyor. Diğer tüm Microsoft Translator desteklenen dil çiftleri için kullanılabilir.
