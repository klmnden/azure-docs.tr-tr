---
title: Araştırma - Personalizer
titleSuffix: Azure Cognitive Services
description: Kullanıcı davranışını değiştiren gibi araştırması ile Personalizer iyi sonuçlar göndermeye devam edebilir. Bir araştırma ayarını seçerek bir iş Kullanıcı etkileşimlerine oranı hakkında ile keşfetmek için modeli geliştirmek için kararıdır.
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/13/2019
ms.author: edjez
ms.openlocfilehash: 0e11dd962f4cc1ff9b1938b309a1b405dbf20232
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65607073"
---
# <a name="exploration-and-exploitation"></a>Araştırma ve kötüye kullanımlara

Kullanıcı davranışını değiştiren gibi araştırması ile Personalizer iyi sonuçlar göndermeye devam edebilir.

Personalizer derece bir çağrı aldığında, bir RewardActionID, aşağıdakilerden birini döndürür:
* Geçerli makine öğrenme modeli temelinde en olası kullanıcı davranışı eşleşecek şekilde yararlanılması kullanır.
* İçinde olasılığı en yüksek dereceli eylemi eşleşmeyen araştırma kullanır.

<!--
Returning the most probable action is called *exploit* behavior. Returning a different action is called *exploration*.
-->
Personalizer şu anda kullandığı algoritma adı verilen *epsilon doyumsuz* keşfetmek için. 

## <a name="choosing-an-exploration-setting"></a>Bir inceleme ayarı seçme

Azure portalının içinde araştırması için trafik yüzdesi yapılandırma **ayarları** Personalizer sayfası. Bu ayar, keşfi gerçekleştirmek derece çağrıları yüzdesini belirler. 

Personalizer keşfedin veya bu olasılık derece her çağrıda ile yararlanma belirler. Bu davranışı bazı a farklıdır / bir işlemden belirli kullanıcı kimlikleri kilitleme B çerçeveleri.

## <a name="best-practices-for-choosing-an-exploration-setting"></a>Bir inceleme ayarı seçmek için en iyi yöntemler

<!--
@edjez - you say what not to do, but make no recommendations of what **to** do. 
-->

Bir araştırma ayarını seçerek bir iş Kullanıcı etkileşimlerine oranı hakkında ile keşfetmek için modeli geliştirmek için kararıdır. 

Sıfır ayarı Personalizer avantajlarının birçoğundan faydalanılmasını negate. Bu ayar, daha iyi Kullanıcı etkileşimlerine bulmak için hiçbir kullanıcı etkileşimlerine Personalizer kullanır. Bu model stagnation, kayması ve sonuçta düşük performansa neden olur.

Çok yüksek bir ayar, kullanıcı davranışından öğrenme avantajlarını negate. % 100'e ayarlanması, sabit bir rastgele seçim anlamına gelir ve kullanıcıların öğrenilen herhangi bir davranış, sonucu etkiler değil.

Olup Personalizer keşfetme veya kötüye görüntülediğiniz tabanlı uygulama davranışını değiştirilmemesi önemlidir. Bu, sonuçta olası performans azalır sapmaları öğrenme yol açar.

## <a name="next-steps"></a>Sonraki adımlar

[Pekiştirmeye dayalı öğrenme](concepts-reinforcement-learning.md) 