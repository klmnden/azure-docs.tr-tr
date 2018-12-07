---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: 31482cc7850c574b952c454021af729da324ba15
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52978784"
---
<!-- N.B. no header, no intents here, language-agnostic -->

Bilişsel Hizmetler [Speech SDK'sı](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanmanın en basit yolu sağlar **Konuşmayı metne dönüştürme** uygulamanızda tam işlevselliğe sahip.

1. Bir konuşma yapılandırması oluşturun ve bir konuşma hizmeti abonelik anahtarı (veya bir yetkilendirme belirteci) sağlar ve bir [bölge](~/articles/cognitive-services/speech-service/regions.md) parametre olarak. Yapılandırma, gerektiği gibi değiştirin. Örneğin, standart hizmet uç noktası belirtmek için özel bir uç noktanın sağlayın veya okunan giriş dili veya çıkış biçimi seçin.

1. Konuşma tanıyıcının konuşma yapılandırmasından oluşturun. Varsayılan mikrofonunuza (örneğin, bir ses akışı veya ses dosyası) dışında bir kaynaktan tanımak istiyorsanız, bir ses yapılandırmasını sağlar.

1. Olayları zaman uyumsuz işlem için'kurmak isterseniz birbirine bağlayın. Geçici ve Nihai sonuç olduğunda tanıyıcı daha sonra olay işleyicileri çağırır. Aksi takdirde, uygulamanızı yalnızca son transkripsiyonu sonucu alır.

1. Tanıma başlatın. Tek tanıma, tanıma, komut veya sorgu gibi kullanılmaya `RecognizeOnceAsync()` (veya eşdeğer bir dil) yöntemi. Bu yöntem ilk tanınan utterance döndürür. Döküm gibi uzun süreli tanıma için kullandığınız `StartContinuousRecognitionAsync()` (veya eşdeğer bir dil) yöntemi. Zaman uyumsuz tanıma sonuçları için olayları oluşturan birbirine bağlayın.

Konuşma tanıma Speech SDK'sı kullanan bir senaryolar için aşağıdaki kod parçacıklarına bakın.

[!INCLUDE [Get a subscription key](cognitive-services-speech-service-get-subscription-key.md)]
