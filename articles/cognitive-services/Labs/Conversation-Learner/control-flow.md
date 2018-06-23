---
title: Konuşma öğrenen denetim akışı - Microsoft Bilişsel hizmetler | Microsoft Docs
titleSuffix: Azure
description: Konuşma öğrenen denetim akışı hakkında bilgi edinin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 592ca82db7e0ab3ff89d25b61f38f8b226f3bc86
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353933"
---
## <a name="control-flow"></a>Denetim akışı

Bu belgede görüntülendiği şekilde konuşma öğrenen'ın (CL) denetim akışı açıklanır diyagramı aşağıda.

![](media/controlflow.PNG)

1. Kullanıcı bir terim veya tümcecik bot Örneğin, 'Seattle'da hava durumu nedir?' girer.
1. CL varlıklar ayıklar makine öğrenimi modeline kullanıcı girişine geçirir
    - Bu model konuşma öğrenen tarafından yapıdır ve www.luis.ai tarafından barındırılan
2. Tüm varlıklar ayıklanır ve metin, kullanıcı giriş bot's kodunuzda varlık algılama geri çağırma yöntemine geçirilen.
    - Bu kod kümesi/Temizle/işlemek varlık değerleri olabilir
1. CL sinir ağı ardından varlık ayıklama ve kullanıcı girişi ve tüm eylemleri bot tanımlanan puanları çıktısını alır
    - Bu örnekte, en yüksek olasılık eylem hava tahmini sağlamaktır:

![](media/controlflow_forecast.PNG)

5. Seçilen eylem, bu durumda, hava durumu tahmini almak için bir API çağrısı gerektirir. 
6. CL kullanarak kayıtlı bu API. AddAPICallback yöntemi, daha sonra çağrılır.  Bu API sonucunu daha sonra kullanıcıya bir ileti--Örneğin, 'Sunny 67'yi yüksek.' döndürülür
7. Sonra sinir ağı önceki adımı bağlı olarak bir sonraki eylemi belirlemek için çağrı yapılır.
8. Sinir ağı sonra eylemlerinin bir sonraki kümesini tahmin ve seçilen eylem, bu durumda, 'Başka bir şey?' kullanıcıya görüntülenir

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma öğrenen ile öğretmeyi nasıl ](./how-to-teach-cl.md)
