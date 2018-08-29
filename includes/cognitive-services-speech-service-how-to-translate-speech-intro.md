---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: ee50a104a75d3cd5ff958bd49a1ff7010c5d5083
ms.sourcegitcommit: a1140e6b839ad79e454186ee95b01376233a1d1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43144562"
---
<!-- N.B. no header, language-agnostic -->

Microsoft Bilişsel Hizmetler [Speech SDK'sı](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanmanın en basit yolu sağlar **konuşma çevirisi** uygulamanızdaki.
SDK hizmetinin tam işlevsellik sağlar. Konuşma çevirisi gerçekleştirmek için temel işlemi aşağıdaki adımları içerir:

1. Bir konuşma fabrikası oluşturun ve bir konuşma hizmeti abonelik anahtarı veya bir yetkilendirme belirteci sağlayın ve [bölge](~/articles/cognitive-services/speech-service/regions.md) parametre olarak.
   
1. Çeviri tanıyıcı konuşma fabrikadan oluşturun. Kaynak ve hedef çeviri dilleri yapılandırmak, yapabilir, metin ve konuşma çıkış isteyip istemediğinizi belirtin. Çeviri tanıyıcıları kullandığınız ses kaynak temel çeşitli türü vardır.

1. Olayları zaman uyumsuz işlem için'kurmak isterseniz birbirine bağlayın. İsteğe bağlı ses çıkış sentezi etkinliğimize yanı sıra, Ara ve son sonuçları olduğunda tanıyıcı daha sonra olay işleyicileri çağırır. Aksi takdirde, uygulamanızı yalnızca son transkripsiyonu sonucu alır.

1. Tanıma başlatın. Kullanmak için tek çeviri `RecognizeAsync()` ilk tanınan utterance döndüren bir yöntem. Uzun süre çalışan çevirileri için kullanmak `StartContinuousRecognitionAsync()` yöntemi ve zaman uyumsuz tanıma sonuçları için KRAVAT olayları.

Aşağıdaki kod parçacıkları Speech SDK'sı kullanan konuşma çevirisi senaryoları için bkz.

[!INCLUDE [Get a subscription key](cognitive-services-speech-service-get-subscription-key.md)]
