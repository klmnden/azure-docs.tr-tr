---
title: İçerik çeviri - Translator metin çevirisi API'si engelle
titlesuffix: Azure Cognitive Services
description: Translator metin çevirisi API'si ile içeriğin çeviri engelleyin.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 11/20/2018
ms.author: v-jansko
ms.openlocfilehash: 0b227803379ed0a64d17c7a7d19b187250b9f845
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55226925"
---
# <a name="how-to-prevent-translation-of-content-with-the-translator-text-api"></a>Translator metin çevirisi API'si ile içeriğin çeviri engelleme

Böylece çevrilmiş değil Translator metin çevirisi API'si, etiketi içeriği sağlar. Örneğin, kod, marka adı veya yerelleştirilmiş doesn't make Sense bir sözcük/tümcecik etiket isteyebilirsiniz. 

## <a name="methods-for-preventing-translation"></a>Çeviri önleme yöntemleri
1. Çıkış için bir Twitter etiketi @somethingtopassthrough veya #somethingtopassthrough. Kaldırma kaçış çeviri sonra.

2. İçeriğinizi etiketlemek `notranslate`.

   Örnek:

   ```html
   <div class="notranslate">This will not be translated.</div>
   <div>This will be translated. </div>
   ```

3. Kullanım [dinamik sözlük](dynamic-dictionary.md) belirli bir çeviri olarak açıklamayı amaçlamamaktadır.

4. Translator metin çevirisi API'si çeviri için dize geçirmeyin.

5. Özel Translator: Kullanım bir [özel Translator sözlüğünde](custom-translator/what-is-dictionary.md) bir ifade çevirisi ile % 100 olasılık olarak açıklamayı amaçlamamaktadır.


## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Çeviri, Translator API çağrısında kaçının](reference/v3-0-translate.md)
