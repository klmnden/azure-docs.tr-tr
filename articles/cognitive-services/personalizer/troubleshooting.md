---
title: Sorun giderme
titleSuffix: Azure Cognitive Services
description: Bu makalede Personalizer hakkında sorular sorun giderme bulunabilir.
author: edjez
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: article
ms.date: 06/15/2019
ms.author: edjez
ms.openlocfilehash: 5136bd295c12c4439a894b4dcf0b868d32ce43ca
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313142"
---
# <a name="personalizer-troubleshooting"></a>Personalizer sorunlarını giderme

Bu makale, Personalizer ilgili sorun giderme sık sorulan soruların yanıtlarını içerir.

## <a name="learning-loop"></a>Öğrenme döngüsü

### <a name="the-learning-loop-doesnt-seem-to-learn-how-do-i-fix-this"></a>Bilgi edinmek için öğrenme döngüsü görülüyor. Bunu nasıl düzeltirim?

Derece çağrıları etkin şekilde önceliklendirmek önce öğrenme döngüsü birkaç bin ödül çağrıları gerekir. 

Nasıl öğrenme döngünüzü şu anda davrandığını hakkında emin değilseniz çalıştırma bir [çevrimdışı değerlendirme](concepts-offline-evaluation.md)ve düzeltilmiş öğrenme ilke uygulayın. 

## <a name="next-steps"></a>Sonraki adımlar

[Model güncelleştirme sıklığını yapılandırma](how-to-settings.md#model-update-frequency)