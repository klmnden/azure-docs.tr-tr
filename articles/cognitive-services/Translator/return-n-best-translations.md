---
title: İade Microsoft Çeviricisi metin API ile N en iyi Çeviriler | Microsoft Docs
description: Microsoft Çeviricisi metin API kullanarak N en iyi Çeviriler döndür.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: 3eafe50f69ae1a6748342e64a414ecee4467d0d1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352600"
---
# <a name="how-to-return-n-best-translations"></a>N en iyi Çeviriler döndürmek nasıl

> [!NOTE]
> Bu yöntem kullanım dışıdır. Çevirici metin API V3.0 içinde kullanılabilir değil.

Bir isteğe bağlı mantıksal bayrak "IncludeMultipleMTAlternatives" Microsoft Çeviricisi API GetTranslations() ve GetTranslationsArray() yöntemleri içerir.
Yöntemi, delta Çeviricisi altyapısı N en iyi listesinden burada sağlanan maxTranslations alternatifleri kadar döndürür.

İmza değil:

**Sözdizimi**

| C# |
|:---|
| GetTranslationsResponse Microsoft.Translator.GetTranslations(appId, text, from, to, maxTranslations, options); |

**Parametreler**

| Parametre | Açıklama |
|:---|:---|
| AppID | **Gerekli** Authorization Üstbilgisi kullanılırsa, AppID alanı boş bırakın başka belirtin "Bearer" içeren bir dize + "" + erişim belirteci.|
| metin | **Gerekli** çevirmek için metin temsil eden dize. Metin boyutu 10000 karakteri aşmamalıdır.|
| başlangıç | **Gerekli** çevirmek için metnin dil kodunu temsil eden dize. |
| - | **Gerekli** metne çevirmek için dil kodu temsil eden dize. |
| maxTranslations | **Gerekli** çevirileri döndürülecek en büyük sayısını temsil eden bir tamsayı. |
| seçenekler | **İsteğe bağlı** aşağıda listelenen değerler içeren bir TranslateOptions nesne. Bunlar tüm isteğe bağlıdır ve varsayılan en yaygın ayarlar.

* Kategori: Desteklenen tek ve varsayılan olarak, "Genel" seçeneğidir.
* ContentType: Desteklenen tek ve "metin/düz" varsayılan seçenektir.
* Durum: correlate istek ve yanıt yardımcı olmak için kullanıcı durumu. Aynı içeriği yanıt olarak döndürülür.
* IncludeMultipleMTAlternatives: birden fazla alternatifleri MT altyapısı döndürülmeyeceğini belirleyen bayrak. Varsayılan, false ve yalnızca 1 alternatif içerir.

## <a name="ratings"></a>Derecelendirme
Derecelendirmeleri gibi uygulanır: 5 derecesi en iyi otomatik çeviri içeriyor.
Otomatik olarak oluşturulan (N-en iyi) çeviri alternatifleri 0 derecesi yoksa ve 100 eşleşme derecesi.

## <a name="number-of-alternatives"></a>Alternatifleri sayısı
Döndürülen alternatifleri sayısı kadar maxTranslations olmakla birlikte, daha az olabilir.

## <a name="language-pairs"></a>Dil çiftleri
Bu işlevsellik, Basitleştirilmiş ve Geleneksel Çince, her iki yönde arasında çevirileri kullanımda değil. Diğer tüm Microsoft Translator desteklenen dil çiftleri için kullanılabilir.
