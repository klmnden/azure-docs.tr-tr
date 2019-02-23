---
title: İçerik çeviri - Translator metin çevirisi API'si engelle
titlesuffix: Azure Cognitive Services
description: Translator metin çevirisi API'si ile içeriğin çeviri engelleyin.
services: cognitive-services
author: Jann-Skotdal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: v-jansko
ms.openlocfilehash: cc06e020001e0e0696fe1e89863f7df705d7fe98
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2019
ms.locfileid: "56726985"
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
