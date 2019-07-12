---
title: Proje akustik hazırlama çözümleme
titlesuffix: Azure Cognitive Services
description: Bu kavramsal genel bakış akustik saklanacağı kaba ve ince çözümleri arasındaki farkı açıklar.
services: cognitive-services
author: KyleStorck
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 04/05/2019
ms.author: kylesto
ms.openlocfilehash: c4f4581beb26eb63392644b40b1e5f16dae0481d
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67849950"
---
# <a name="project-acoustics-bake-resolution"></a>Proje akustik hazırlama çözümleme
Bu kavramsal genel bakış akustik saklanacağı kaba ve ince çözümleri arasındaki farkı açıklar. Baking iş akışının araştırmaları adımı sırasında bu ayarı seçin.

## <a name="Coarse-vs-Fine-Resolution"></a>Kaba vs ayrıntılı çözümleme

Kaba ve ince çözümleme ayarları arasındaki tek fark, benzetim gerçekleştirildiği sıklığıdır. İnce bir kaba olarak iki kez olarak yüksek sıklıkta kullanır. Bu etkileri birçok akustik benzetim vardır:

* Dalga boyu iki kez olarak ince uzun geneldir ve bu nedenle voxels iki katı kadar büyük.
* Benzetim zaman kaba bir hazırlama yaklaşık 16 kat daha iyi bir hazırlama hızlandırma voxel boyutu, doğrudan ilgilidir.
* Portalları (örneğin, kapılar veya windows) voxel boyutundan daha küçük benzetimi yapılabilir. Kaba bir ayar değil benzetimi için daha küçük bu portallar bazılarını neden olabilir; Bu nedenle, bunlar çalışma zamanında ses aracılığıyla geçirin olmaz. Bu voxels görüntüleyerek olup olmadığını görebilirsiniz.
* Köşe ve kenarlar daha az diffraction alt benzetimi sıklığı sonuçlanır.
* Ses kaynakları (yani geometry içeren voxels) "dolu" voxels içinde bulunamıyor. Bu, herhangi bir ses sonuçlanır. Bunlar, kaba ince ayar kullanırken olduğundan daha büyük voxels içinde olmadıklarından ses kaynakları yerleştirmek daha zordur.
* Daha büyük voxels daha aşağıda gösterildiği gibi portallarda oturum intrude. İyi bir çözüm kullanarak aynı ortaklıklarına ortam hazırlayan ikinci olmakla birlikte ilk görüntü kaba, kullanılarak oluşturuldu. Kırmızı işaretler tarafından belirtildiği gibi ince ayarını kullanarak ortaklıklarına ortam hazırlayan çok daha az giriş yok. Kırmızı çizgi voxel boyutu tarafından tanımlanan etkili akustik portalı olsa da, geometri tarafından tanımlanan aynıdır, mavi bir çizgi ortaklıklarına ortam hazırlayan kullanır. Bu yetkisiz erişim verilen bir durumda nasıl oynatılacağını tamamen geometri nesnelerinizi sahnedeki konumlarını ve boyutu tarafından belirlenir Portal ile nasıl voxels hizaya bağlıdır.

![Kaba voxels Unreal içinde bir kapısı doldurma ekran görüntüsü](media/unreal-coarse-bake.png)

![Unreal içinde bir kapısı de ince voxels ekran görüntüsü](media/unreal-fine-bake.png)

## <a name="next-steps"></a>Sonraki adımlar

Kaba ve ince çözümleme ayarlarını kullanarak kendiniz deneyin bizim [Unreal](unreal-baking.md) veya [Unity](unity-baking.md) eklentiler.