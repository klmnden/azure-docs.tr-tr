---
title: Konuşma Öğrenici denetim akışı - Microsoft Bilişsel hizmetler | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici denetim akışı hakkında bilgi edinin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 07f4506b7dd0ac8ca0462e2a418983e561859c91
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55208389"
---
## <a name="control-flow"></a>Denetim akışı

Bu belgede gösterilen haliyle konuşma Öğrenici ', (CI) denetim akışı açıklanır diyagram aşağıda.

![](media/controlflow.PNG)

1. Kullanıcı bir terimini veya tümceciğini bot Örneğin, 'Seattle hava durumu nedir?' girer.
1. CL varlıkları ayıklayan makine öğrenme modeli kullanıcı girişine geçirir.
    - Bu model, derleme tarafından konuşma Öğrenici ve www.luis.ai tarafından barındırılan
1. Ayıklanan tüm varlıkları ve kullanıcının metin girişinin botun kodunuzda varlık algılama geri çağırma yöntemine geçirilir.
    - Bu kod varlık kümesi/clear/işlemek değerleri olabilir.
1. CL sinir ağı sonra varlık ayıklama ve kullanıcı girişi ve bot içinde tanımlanan tüm eylemler puanları çıkışını alır
    - Bu örnekte, en yüksek olasılık eylem hava durumu tahminini sağlamaktır:

    ![](media/controlflow_forecast.PNG)

1. Seçili eylemi, bu durumda, hava durumu tahminini almak için bir API çağrısı gerektirir. 
1. CL kullanarak kayıtlı bu API. Ardından AddCallback yöntemi çağrılır.  Bu API sonucunu sonra kullanıcıya bir ileti--Örneğin, 'Sunny 67'ın yüksek.' döndürülür
1. Ardından sinir ağı için önceki adımlara göre sonraki eylemi belirlemek için çağrı yapılır.
1. Sinir ağı, ardından sonraki olası eylemler kümesini tahmin eder ve seçili eylem, bu durumda, 'Başka bir şey mi?' kullanıcıya

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma Öğrenici ile öğretmeyi nasıl ](./how-to-teach-cl.md)
