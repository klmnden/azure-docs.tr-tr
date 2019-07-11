---
title: Sorun giderme - Personalizer
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
ms.openlocfilehash: be6119d96b89622f45db1099a47e858a5893c2cb
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67722256"
---
# <a name="personalizer-troubleshooting"></a>Personalizer sorunlarını giderme

Bu makale, Personalizer ilgili sorun giderme sık sorulan soruların yanıtlarını içerir.

## <a name="learning-loop"></a>Öğrenme döngüsü

### <a name="the-learning-loop-doesnt-seem-to-learn-how-do-i-fix-this"></a>Bilgi edinmek için öğrenme döngüsü görülüyor. Bunu nasıl düzeltirim?

Derece çağrıları etkin şekilde önceliklendirmek önce öğrenme döngüsü birkaç bin ödül çağrıları gerekir. 

Nasıl öğrenme döngünüzü şu anda davrandığını hakkında emin değilseniz çalıştırma bir [çevrimdışı değerlendirme](concepts-offline-evaluation.md)ve düzeltilmiş öğrenme ilke uygulayın. 

## <a name="next-steps"></a>Sonraki adımlar

[Model güncelleştirme sıklığını yapılandırma](how-to-settings.md#model-update-frequency)