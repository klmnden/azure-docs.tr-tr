---
title: Microsoft Çeviricisi metin API dinamik sözlük | Microsoft Docs
description: Microsoft Çeviricisi metin API dinamik sözlük özelliği nasıl kullanılır.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: a18348c9786669ac41c4e149577d97cd631d5531
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355649"
---
# <a name="how-to-use-the-dynamic-dictionary-feature-of-the-microsoft-translator-text-api"></a>Microsoft Çeviricisi metin API dinamik sözlük özelliği nasıl kullanılır

Bir sözcük veya tümcecik uygulamak istediğiniz çeviri zaten biliyorsanız, istek içinde biçimlendirmesi olarak sağlayabilir. Dinamik sözlük yalnızca uygun ve ürün adları gibi bileşik isimleri için güvenlidir. 

**Sözdizimi:** 

< mstrans:dictionary çeviri "tümceciği çevrilmesi" = > tümcecik < / mstrans:dictionary >

**Örnek: tr-de:**

Kaynak girişi: word < mstrans:dictionary çeviri =\"wordomatic\"> sözcük veya tümcecik < / mstrans:dictionary > bir sözlük girişi.

Hedef çıktı: Das Wort "wordomatic" ist EIN Wörterbucheintrag.

Bu özellik ile ve HTML modu olmadan aynı şekilde çalışır. 

Özellik kullanılmamalıdır. Çeviri özelleştirme uygun ve çok daha iyi şekilde Microsoft Çeviricisi hub'ı kullanmaktır. Hub bağlamını ve istatistiksel olasılıklar tam kullanımını sağlar. Varsa veya iş ya da tümcecik bağlamda gösterir eğitim verileri oluşturmak destekleyebilir kadar daha iyi sonuçlar elde. Hub'ına hakkında daha fazla bilgi bulabilirsiniz [ http://hub.microsofttranslator.com ](http://hub.microsofttranslator.com).

