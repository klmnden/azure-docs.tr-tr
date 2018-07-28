---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: ca342a812f6a8de19c732b5df4fab14a825f6c51
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39331043"
---
<!-- N.B. no header, no intents here, language-agnostic -->

[Speech SDK'sı](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanmanın en basit yolu sağlar **Konuşmayı metne dönüştürme** uygulamanızda tam işlevselliğe sahip.

1. Bir konuşma hizmeti abonelik anahtarı veya bir yetkilendirme belirteci sağlayan bir konuşma fabrikası oluşturma ve bir [bölge](~/articles/cognitive-services/speech-service/regions.md).
   Tanıma dili veya kendi konuşma tanıma modelleri için özel bir uç nokta gibi seçeneklere de yapılandırabilirsiniz.

1. Konuşma tanıyıcının konuşma fabrikadan alın.
   Bir tanıyıcı, cihazınızın varsayılan mikrofon, bir ses akışı veya ses bir dosyadan kullanabilirsiniz.

1. Olayları zaman uyumsuz işlem için'kurmak isterseniz birbirine bağlayın.
   Geçici ve Nihai sonuç olduğunda tanıyıcı daha sonra olay işleyicileri çağırır.
   Aksi takdirde, uygulamanızı son transkripsiyonu sonuç alırsınız.

1. Tanıma başlatın.
   Tek tanıma, tanıma, komut veya sorgu gibi kullanılmaya `RecognizeAsync()`, tanınan ilk utterance döndürür.
   Uzun süreli tanıma, tanıma gibi kullanılmaya `StartContinuousRecognitionAsync()` ve zaman uyumsuz tanıma sonuçları için olayları oluşturan bağlayın.

Konuşma SDK'sını kullanarak konuşma tanıma senaryoları için çeşitli kod parçacıklarını aşağıda göstereceğiz.

[!include[Get a Subscription Key](cognitive-services-speech-service-get-subscription-key.md)]
