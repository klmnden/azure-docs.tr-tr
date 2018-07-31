---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: d7d73d9c5e85ac550b24c73797d536dc4d0fc6ed
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39346275"
---
<!-- N.B. no header, no intents here, language-agnostic -->

[Speech SDK'sı](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanmanın en basit yolu sağlar **Konuşmayı metne dönüştürme** uygulamanızda tam işlevselliğe sahip.

1. Bir konuşma hizmeti abonelik anahtarı veya bir yetkilendirme belirteci sağlayan bir konuşma fabrikası oluşturma ve bir [bölge](~/articles/cognitive-services/speech-service/regions.md). Standart hizmet uç noktası belirtmek için özel bir uç noktası da belirtebilirsiniz.

1. Konuşma tanıyıcının konuşma fabrikadan alın. Çıkış biçimi yanı sıra giriş dili yapılandırabilirsiniz.  Bir tanıyıcı, cihazınızın varsayılan mikrofon, bir ses akışı veya ses bir dosyadan kullanabilirsiniz.

1. Olayları zaman uyumsuz işlem için'kurmak isterseniz birbirine bağlayın. Geçici ve Nihai sonuç olduğunda tanıyıcı daha sonra olay işleyicileri çağırır.  Aksi takdirde, uygulamanızı son transkripsiyonu sonuç alırsınız.

1. Tanıma başlatın. Tek tanıma, tanıma, komut veya sorgu gibi kullanılmaya `RecognizeAsync()`, tanınan ilk utterance döndürür. Uzun süreli tanıma, tanıma gibi kullanılmaya `StartContinuousRecognitionAsync()` ve zaman uyumsuz tanıma sonuçları için olayları oluşturan bağlayın.

Aşağıdaki kod parçacıklarından birini Speech SDK'sı kullanarak konuşma tanıma senaryoları için bkz.

[!include[Get a Subscription Key](cognitive-services-speech-service-get-subscription-key.md)]
