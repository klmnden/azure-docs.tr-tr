---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: 1103e5a217ca4972cc1c983a7ad0d07b0797e9e9
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43122649"
---
<!-- N.B. no header, no intents here, language-agnostic -->

Bilişsel Hizmetler [Speech SDK'sı](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanmanın en basit yolu sağlar **Konuşmayı metne dönüştürme** uygulamanızda tam işlevselliğe sahip.

1. Bir konuşma fabrikası oluşturun ve bir konuşma hizmeti abonelik anahtarı (veya bir yetkilendirme belirteci) sağlar ve bir [bölge](~/articles/cognitive-services/speech-service/regions.md) parametre olarak. Standart hizmet uç noktası belirtmek için özel bir uç nokta sağlar.

1. Konuşma tanıyıcının konuşma fabrikadan alın. Giriş dili ve çıkış biçimi yapılandırabilirsiniz. Bir tanıyıcı, cihazınızın varsayılan mikrofon, bir ses akışı veya ses bir dosyadan kullanabilirsiniz.

1. Olayları zaman uyumsuz işlem için'kurmak isterseniz birbirine bağlayın. Geçici ve Nihai sonuç olduğunda tanıyıcı daha sonra olay işleyicileri çağırır. Aksi takdirde, uygulamanızı yalnızca son transkripsiyonu sonucu alır.

1. Tanıma başlatın. Tek tanıma, tanıma, komut veya sorgu gibi kullanılmaya `RecognizeAsync()` yöntemi. Bu yöntem ilk tanınan utterance döndürür. Döküm gibi uzun süreli tanıma için kullandığınız `StartContinuousRecognitionAsync()` yöntemi. Zaman uyumsuz tanıma sonuçları için olayları oluşturan birbirine bağlayın.

Konuşma tanıma Speech SDK'sı kullanan bir senaryolar için aşağıdaki kod parçacıklarına bakın.

[!INCLUDE [Get a subscription key](cognitive-services-speech-service-get-subscription-key.md)]
