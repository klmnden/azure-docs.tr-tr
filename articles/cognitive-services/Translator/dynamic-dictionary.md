---
title: Dinamik sözlük - Translator metin çevirisi API'si
titlesuffix: Azure Cognitive Services
description: Translator metin çevirisi API'si, dinamik sözlük özelliği nasıl kullanılır.
services: cognitive-services
author: v-pawal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: v-jansko
ms.openlocfilehash: 0b4362d78ef105c249aafb4c6b203f69754a56c8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60507989"
---
# <a name="how-to-use-the-dynamic-dictionary-feature-of-the-translator-text-api"></a>Translator metin çevirisi API'si, dinamik sözlük özelliği nasıl kullanılır

Bir sözcük veya tümcecik uygulamak istediğiniz çeviri zaten biliyorsanız, istek içinde biçimlendirmesi olarak sağlayabilirsiniz. Dinamik sözlük yalnızca uygun ve ürün adları gibi bileşik isimleri için güvenlidir.

**Sözdizimi:**

<mstrans:dictionary translation=”translation of phrase”>phrase</mstrans:dictionary>

**Örnek: tr-de:**

Kaynak girişi: Word < mstrans:dictionary çeviri =\"wordomatic\"> sözcük veya tümcecik < / mstrans:dictionary > bir sözlük girişi.

Hedefi çıktısı: DAS Wort "wordomatic" ist EIN Wörterbucheintrag.

Bu özellik ile ve HTML modu olmadan aynı şekilde çalışır.

Özelliği kullanılmamalıdır. Çeviri özelleştirme uygun ve çok daha iyi özel Translator kullanarak yoludur. Özel Translator bağlamını ve istatistiksel olasılıklar tam olarak kullanmayı sağlar. Varsa veya iş ya da ifade bağlamda gösteren eğitim verilerini oluşturabilirsiniz, çok daha iyi sonuçlar alırsınız. Özel Translator hakkında daha fazla bilgi bulabilirsiniz [ https://aka.ms/CustomTranslator ](https://aka.ms/CustomTranslator).
