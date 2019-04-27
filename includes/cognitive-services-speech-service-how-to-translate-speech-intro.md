---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
origin.date: 09/24/2018
ms.date: 04/01/2019
ms.author: v-biyu
ms.openlocfilehash: ab4795325f733d15403b53f027daa4fb8c824cc6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60531012"
---
<!-- N.B. no header, language-agnostic -->

Microsoft Bilişsel Hizmetler [Speech SDK'sı](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanmanın en basit yolu sağlar **konuşma çevirisi** uygulamanızdaki.
SDK hizmetinin tam işlevsellik sağlar. Konuşma çevirisi gerçekleştirmek için temel işlemi aşağıdaki adımları içerir:

1. Konuşma çevirisi yapılandırmasını oluşturun ve bir konuşma hizmeti abonelik anahtarı (veya bir yetkilendirme belirteci) sağlar ve bir [bölge](~/articles/cognitive-services/speech-service/regions.md) parametre olarak. Yapılandırma, gerektiği gibi değiştirin. Örneğin, kaynak ve hedef çeviri dilleri yapılandırmak, yapabilir metin ve konuşma çıkış isteyip istemediğinizi belirtin.

1. Çeviri tanıyıcı konuşma çevirisi yapılandırmasından oluşturun. Varsayılan mikrofonunuza (örneğin, bir ses akışı veya ses dosyası) dışında bir kaynaktan tanımak istiyorsanız, bir ses yapılandırmasını sağlar.

1. Olayları zaman uyumsuz işlem için'kurmak isterseniz birbirine bağlayın. İsteğe bağlı ses çıkış sentezi etkinliğimize yanı sıra, Ara ve son sonuçları olduğunda tanıyıcı daha sonra olay işleyicileri çağırır. Aksi takdirde, uygulamanızı yalnızca son transkripsiyonu sonucu alır.

1. Tanıma başlatın. Kullanmak için tek çeviri `RecognizeOnceAsync()` ilk tanınan utterance döndüren bir yöntem. Uzun süre çalışan çevirileri için kullanmak `StartContinuousRecognitionAsync()` yöntemi ve zaman uyumsuz tanıma sonuçları için KRAVAT olayları.

Aşağıdaki kod parçacıkları Speech SDK'sı kullanan konuşma çevirisi senaryoları için bkz.

[!INCLUDE [Get a subscription key](cognitive-services-speech-service-get-subscription-key.md)]
