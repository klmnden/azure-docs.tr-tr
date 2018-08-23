---
title: Microsoft Translator metin API'si dinamik sözlük | Microsoft Docs
description: Microsoft Translator metin çevirisi API'si, dinamik sözlük özelliği nasıl kullanılır.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: dbc754093827730b8709d67f314e5b327518ef50
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2018
ms.locfileid: "41987586"
---
# <a name="how-to-use-the-dynamic-dictionary-feature-of-the-microsoft-translator-text-api"></a>Dinamik sözlük özelliği Microsoft Translator metin çevirisi API'si kullanma

Bir sözcük veya tümcecik uygulamak istediğiniz çeviri zaten biliyorsanız, istek içinde biçimlendirmesi olarak sağlayabilirsiniz. Dinamik sözlük yalnızca uygun ve ürün adları gibi bileşik isimleri için güvenlidir. 

**Sözdizimi:** 

< mstrans:dictionary çeviri "ifade çevirisi" = > tümcecik < / mstrans:dictionary >

**Örnek: tr-de:**

Giriş kaynağı: sözcük < mstrans:dictionary çeviri =\"wordomatic\"> sözcük veya tümcecik < / mstrans:dictionary > bir sözlük girişi.

Hedef Çıkış: Das Wort "wordomatic" ist EIN Wörterbucheintrag.

Bu özellik ile ve HTML modu olmadan aynı şekilde çalışır. 

Özelliği kullanılmamalıdır. Çeviri özelleştirme uygun ve çok daha iyi özel Translator kullanarak yoludur. Özel Translator bağlamını ve istatistiksel olasılıklar tam olarak kullanmayı sağlar. Varsa veya iş ya da ifade bağlamda gösteren eğitim verilerini oluşturabilirsiniz, çok daha iyi sonuçlar alırsınız. Özel Translator hakkında daha fazla bilgi bulabilirsiniz [ http://aka.ms/CustomTranslator ](http://aka.ms/CustomTranslator).

