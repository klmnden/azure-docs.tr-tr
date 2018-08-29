---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: a4accb0846ec9cf5efc5ca5aa00c731edd41d4d4
ms.sourcegitcommit: a1140e6b839ad79e454186ee95b01376233a1d1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43143882"
---
<!-- N.B. no header, language-agnostic -->

Microsoft Bilişsel Hizmetler [Speech SDK'sı](~/articles/cognitive-services/speech-service/speech-sdk.md) tanımak için bir yol sağlar **amaçlardan tutun konuşma** ve Bilişsel hizmetler tarafından desteklenen [Language Understanding hizmeti (LUIS)](https://www.luis.ai/home).

1. Bir LUIS abonelik anahtarı ile bir konuşma fabrikası oluşturun ve [bölge](~/articles/cognitive-services/speech-service/regions.md#regions-for-intent-recognition) parametre olarak. LUIS abonelik anahtarını adlı **uç noktası anahtarı** service belgelerinde. Anahtar yazma LUIS kullanamazsınız. (Bu bölümün sonraki kısımlarında nota bakın.)

1. Hedefi bir tanıyıcı konuşma fabrikadan alın. Bir tanıyıcı, cihazınızın varsayılan mikrofon, bir ses akışı veya ses bir dosyadan kullanabilirsiniz.

1. Temel dil anlama modeli alın, **AppID**. İhtiyaç duyduğunuz ıntents ekleyin. 

1. Olayları zaman uyumsuz işlem için'kurmak isterseniz birbirine bağlayın. Tanıyıcı geçici ve son sonuçları sahip olduğunda, daha sonra olay işleyicileri çağırır (hedefleri içerir). Olayları tie yoksa uygulamanız yalnızca son transkripsiyonu sonucu alır.

1. Amaç tanıma başlatın. Tek tanıma, tanıma, komut veya sorgu gibi kullanılmaya `RecognizeAsync()` yöntemi. Bu yöntem ilk tanınan utterance döndürür. Uzun süreli tanıma için kullandığınız `StartContinuousRecognitionAsync()` yöntemi. Zaman uyumsuz tanıma sonuçları için olayları oluşturan birbirine bağlayın.

Aşağıdaki kod parçacıkları Speech SDK'sı kullanan niyeti tanıma senaryolar için bkz. Örnek değerleri kendi LUIS abonelik anahtarı (uç noktası anahtarı) ile değiştirin [aboneliğinizin bölgesi](~/articles/cognitive-services/speech-service/regions.md#regions-for-intent-recognition)ve **AppID** hedefi modelinizin.

> [!NOTE]
> Speech SDK'sı tarafından desteklenen diğer hizmetleri aksine niyeti tanıma bir (LUIS uç noktası anahtarı) belirli bir abonelik anahtarı gerektirir. Amaç tanıma teknolojisi hakkında daha fazla bilgi için bkz: [LUIS Web sitesi](https://www.luis.ai). Alma hakkında bilgi için **uç noktası anahtarı**, bkz: [LUIS uç noktası anahtarı oluşturma](https://docs.microsoft.com/azure/cognitive-services/LUIS/luis-how-to-azure-subscription#create-luis-endpoint-key).
