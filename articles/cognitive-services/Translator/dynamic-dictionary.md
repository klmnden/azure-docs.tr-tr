---
title: Translator metin API'si dinamik sözlük
titlesuffix: Azure Cognitive Services
description: Translator metin çevirisi API'si, dinamik sözlük özelliği nasıl kullanılır.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: 56558a2da5f29611d90021e9efb292720d1cea35
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46128137"
---
# <a name="how-to-use-the-dynamic-dictionary-feature-of-the-translator-text-api"></a>Translator metin çevirisi API'si, dinamik sözlük özelliği nasıl kullanılır

Bir sözcük veya tümcecik uygulamak istediğiniz çeviri zaten biliyorsanız, istek içinde biçimlendirmesi olarak sağlayabilirsiniz. Dinamik sözlük yalnızca uygun ve ürün adları gibi bileşik isimleri için güvenlidir. 

**Sözdizimi:** 

< mstrans:dictionary çeviri "ifade çevirisi" = > tümcecik < / mstrans:dictionary >

**Örnek: tr-de:**

Giriş kaynağı: sözcük < mstrans:dictionary çeviri =\"wordomatic\"> sözcük veya tümcecik < / mstrans:dictionary > bir sözlük girişi.

Hedef Çıkış: Das Wort "wordomatic" ist EIN Wörterbucheintrag.

Bu özellik ile ve HTML modu olmadan aynı şekilde çalışır. 

Özelliği kullanılmamalıdır. Çeviri özelleştirme uygun ve çok daha iyi özel Translator kullanarak yoludur. Özel Translator bağlamını ve istatistiksel olasılıklar tam olarak kullanmayı sağlar. Varsa veya iş ya da ifade bağlamda gösteren eğitim verilerini oluşturabilirsiniz, çok daha iyi sonuçlar alırsınız. Özel Translator hakkında daha fazla bilgi bulabilirsiniz [ http://aka.ms/CustomTranslator ](http://aka.ms/CustomTranslator).

