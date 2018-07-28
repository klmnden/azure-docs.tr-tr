---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: 4b823a8dc15d33e0537fae348b34c3a3fee84558
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39331041"
---
<!-- N.B. no header, language-agnostic -->

[Speech SDK'sı](~/articles/cognitive-services/speech-service/speech-sdk.md) konuşma çevirisi kullanmak için basit bir yol sağlar.
SDK hizmetinin tam işlevsellik sağlar.
Konuşma çevirisi gerçekleştirmek için temel işlemi aşağıdaki adımları içerir:

1. Bir konuşma hizmeti abonelik anahtarı sağlayan bir konuşma fabrikası oluşturma ve bir [bölge](~/articles/cognitive-services/speech-service/regions.md).
   Ayrıca kaynak ve hedef çeviri dilleri yanı sıra, metin ve konuşma çıkış isteyip istemediğinizi belirten yapılandırın.

1. Çeviri tanıyıcı konuşma fabrikadan alın.
   Kullanmakta olduğunuz ses kaynak temel çeviri tanıyıcıları çeşitli türdeki vardır.

1. Olayları zaman uyumsuz işlem için'kurmak isterseniz birbirine bağlayın.
   Geçici ve Nihai sonuç olduğunda tanıyıcı daha sonra olay işleyicileri çağırır.
   Aksi takdirde, uygulamanızı son transkripsiyonu sonuç alırsınız.

1. Tanıma ve çeviri başlatın.

Konuşma SDK'sını kullanarak konuşma çevirisi senaryoları için çeşitli kod parçacıklarını aşağıda göstereceğiz.

[!include[Get a Subscription Key](cognitive-services-speech-service-get-subscription-key.md)]
