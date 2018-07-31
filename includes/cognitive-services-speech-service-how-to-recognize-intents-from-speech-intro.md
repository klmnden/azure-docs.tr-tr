---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: a3cbfc3677c5b1a19b83a82da9bcd6bed3108152
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39346251"
---
<!-- N.B. no header, language-agnostic -->

[Speech SDK'sı](~/articles/cognitive-services/speech-service/speech-sdk.md) tanımak için bir yol sağlar **amaçlardan tutun konuşma**, konuşma hizmeti tarafından ve [Language Understanding hizmeti (LUIS)](https://luis.ai).

1. Language Understanding hizmeti abonelik anahtarı sağlayan bir konuşma fabrikası oluşturun ve [bölge](~/articles/cognitive-services/speech-service/regions.md#regions-for-intent-recognition). Language Understanding hizmeti abonelik anahtarını adlı **uç noktası anahtarı** service belgelerinde. Language Understanding hizmeti anahtarı yazma kullanamazsınız. Ayrıca bkz: **Not** aşağıda.

1. Hedefi bir tanıyıcı konuşma fabrikadan alın. Bir tanıyıcı, cihazınızın varsayılan mikrofon, bir ses akışı veya ses bir dosyadan kullanabilirsiniz.

1. Dil anlama modeli üzerinde uygulama kimliğine göre alın ve ihtiyaç duyduğunuz hedef ekleme. 

1. Olayları zaman uyumsuz işlem için'kurmak isterseniz birbirine bağlayın. Geçici ve son sonuçları (ıntents dahil) sahip olduğunda tanıyıcı daha sonra olay işleyicileri çağırır. Aksi takdirde, uygulamanızı son transkripsiyonu sonuç alırsınız.

1. Amaç tanıma başlatın. Tek tanıma, tanıma, komut veya sorgu gibi kullanılmaya `RecognizeAsync()`, tanınan ilk utterance döndürür. Uzun süreli tanıma için kullandığınız `StartContinuousRecognitionAsync()` ve zaman uyumsuz tanıma sonuçları için olayları oluşturan bağlayın.

Aşağıdaki kod parçacıklarından birini Speech SDK'sı kullanarak niyeti tanıma senaryolar için bkz. Kendi dil anlama abonelik anahtarı (uç noktası anahtarı) değiştirmek [aboneliğinizin bölgesi](~/articles/cognitive-services/speech-service/regions.md#regions-for-intent-recognition)ve hedefi modelinizin örneklerdeki uygun yerlerde AppID.

> [!NOTE]
> Speech SDK'sı tarafından desteklenen diğer hizmetleri aksine niyeti tanıma bir (Language Understanding Hizmeti uç noktası anahtarı) belirli bir abonelik anahtarı gerektirir. [Burada](https://www.luis.ai) niyeti tanıma teknolojisi hakkında ek bilgi bulabilirsiniz. Almak üzere nasıl **uç noktası anahtarı** açıklanan [burada](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/luis-how-to-azure-subscription#create-luis-endpoint-key).
