---
title: Desteklenen yerel ayarlar ve dilleri içinde Azure üzerinde özel konuşma hizmeti | Microsoft Docs
description: Özel konuşma hizmeti Bilişsel hizmetler, desteklenen diller genel bakış.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ms.openlocfilehash: 8493aeb3c1c2436900ae626ad0f34cbe8b060163
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49342177"
---
# <a name="supported-locales-in-custom-speech-service"></a>Özel konuşma hizmeti, desteklenen yerel ayarlar

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

Özel konuşma hizmeti, şu anda aşağıdaki yerel ayarlarda modellerin özelleştirmeyi destekler:

| Model türü | Dil desteği |
|----|-----|
| Akustik modeller | ABD İngilizce (en-US) |
| Dil modelleri | ABD İngilizce (en-US), Çince (zh-CN) |

Akustik Model özelleştirme yalnızca ABD İngilizcesi desteklenir, ama Çince akustik verilerini alma çevrimdışı özelleştirilmiş Çince dil modelleri, test amacıyla için desteklenir.

Eylem gerçekleştirilmeden önce uygun dil ayarının seçilmesi gerekir. Geçerli yerel ayar tüm veri, model ve dağıtım sayfalarında tablo başlığında belirtilir. Yerel ayarı değiştirmek için tablonun başlık altında bulunan "Yerel ayarı Değiştir" düğmesine tıklayın. Bu bir yerel ayar doğrulama sayfasına götürür. Tabloya geri dönmek için "OK" (Tamam) düğmesine tıklayın.

Sonraki adımlara takip
* Bilgi [özel akustik model oluşturmak nasıl](cognitive-services-custom-speech-create-acoustic-model.md) tanıma doğruluğunu artırmak için
* Bilgi [özel dil modeli oluşturmak nasıl](cognitive-services-custom-speech-create-language-model.md) , tanıma oranını artırmak için
* İzleyin [transkripsiyonu yönergeleri](cognitive-services-custom-speech-transcription-guidelines.md) verilerinizi hazırlama
