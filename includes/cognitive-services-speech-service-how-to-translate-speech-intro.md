---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: 74b9da1b3b81f9f35f231ef5caef8eafedc9aefc
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39343041"
---
<!-- N.B. no header, language-agnostic -->

[Speech SDK'sı](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanmanın en basit yolu sağlar **konuşma çevirisi** uygulamanızdaki.
SDK hizmetinin tam işlevsellik sağlar. Konuşma çevirisi gerçekleştirmek için temel işlemi aşağıdaki adımları içerir:

1. Bir konuşma hizmeti abonelik anahtarı veya bir yetkilendirme belirteci sağlayan bir konuşma fabrikası oluşturma ve bir [bölge](~/articles/cognitive-services/speech-service/regions.md).
   
1. Çeviri tanıyıcı konuşma fabrikadan oluşturun. Kaynak ve hedef çeviri dilleri yanı sıra, metin ve konuşma çıkış isteyip istemediğinizi belirten yapılandırabilirsiniz. Kullanmakta olduğunuz ses kaynak temel çeviri tanıyıcıları çeşitli türdeki vardır.

1. Olayları zaman uyumsuz işlem için'kurmak isterseniz birbirine bağlayın. İsteğe bağlı ses çıkış sentezi etkinliğimize yanı sıra, Ara ve son sonuçları olduğunda tanıyıcı daha sonra olay işleyicileri çağırır. Aksi takdirde, uygulamanızı son transkripsiyonu sonuç alırsınız.

1. Tanıma başlatın. Tek çeviri kullanım RecognizeAsync() tanınan ilk utterance döndürür. Uzun süre çalışan çevirileri StartContinuousRecognitionAsync() kullanın ve zaman uyumsuz tanıma sonuçları için olayları oluşturan bağlayın.

Aşağıdaki kod parçacıklarından birini Speech SDK'sı kullanarak konuşma çevirisi senaryoları için bkz.

[!include[Get a Subscription Key](cognitive-services-speech-service-get-subscription-key.md)]
