---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: 1b55576581ebddc90c0af6b99a04dbe66d2e3b87
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39331037"
---
<!-- N.B. no header, language-agnostic -->

[Speech SDK'sı](~/articles/cognitive-services/speech-service/speech-sdk.md) amaçlardan tutun konuşma tanıma, konuşma tanıma hizmeti tarafından desteklenen tanımak için bir yol sağlar ve [Language Understanding hizmeti (LUIS)](https://luis.ai).

1. Language Understanding hizmeti abonelik anahtarı sağlayan bir konuşma fabrikası oluşturun ve [bölge](~/articles/cognitive-services/speech-service/regions.md#regions-for-intent-recognition).


1. Hedefi bir tanıyıcı konuşma fabrikadan alın.
   Bir tanıyıcı, cihazınızın varsayılan mikrofon, bir ses akışı veya ses bir dosyadan kullanabilirsiniz.

1. Olayları zaman uyumsuz işlem için'kurmak isterseniz birbirine bağlayın.
   Geçici ve Nihai sonuç olduğunda tanıyıcı daha sonra olay işleyicileri çağırır.
   Aksi takdirde, uygulamanızı son transkripsiyonu sonuç alırsınız.

1. Tanıma başlatın.
   Tek tanıma, tanıma, komut veya sorgu gibi kullanılmaya `RecognizeAsync()`, tanınan ilk utterance döndürür.
   Uzun süreli tanıma, tanıma gibi kullanılmaya `StartContinuousRecognitionAsync()` ve zaman uyumsuz tanıma sonuçları için olayları oluşturan bağlayın.

Birkaç kod parçacıkları Speech SDK'sı kullanarak niyeti tanıma senaryolar için aşağıda göstereceğiz.

> [!NOTE]
> Lütfen bir abonelik anahtarı edinin.
> Konuşma SDK'sı tarafından desteklenen diğer hizmetleri aksine niyeti tanıma belirli bir abonelik anahtarı gerektirir.
> [Burada](https://www.luis.ai) niyeti tanıma teknolojisi hakkında ek bilgi yanı sıra, bir abonelik anahtarı alma hakkında bilgi bulabilirsiniz.
> Kendi dil anlama abonelik anahtarını değiştirin [aboneliğinizin bölgesi](~/articles/cognitive-services/speech-service/regions.md#regions-for-intent-recognition)ve hedefi modelinizin örneklerdeki uygun yerlerde AppID.
