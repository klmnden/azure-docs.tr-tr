---
title: Desteklenen yerel ayarlar ve dilleri içinde Azure üzerinde özel konuşma hizmeti | Microsoft Docs
description: Özel konuşma hizmeti Bilişsel hizmetler, desteklenen diller genel bakış.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ms.openlocfilehash: e5c1f7d9648f404512f366be4f4d16ea0cb0f778
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55217127"
---
# <a name="supported-locales-in-custom-speech-service"></a>Özel konuşma hizmeti, desteklenen yerel ayarlar

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

Özel konuşma hizmeti, şu anda aşağıdaki yerel ayarlarda modellerin özelleştirmeyi destekler:

| Model türü | Dil Desteği |
|----|-----|
| Akustik modeller | ABD İngilizce (en-US) |
| Dil modelleri | ABD İngilizce (en-US), Çince (zh-CN) |

Akustik Model özelleştirme yalnızca ABD İngilizcesi desteklenir, ama Çince akustik verilerini alma çevrimdışı özelleştirilmiş Çince dil modelleri, test amacıyla için desteklenir.

Eylem gerçekleştirilmeden önce uygun dil ayarının seçilmesi gerekir. Geçerli yerel ayar tüm veri, model ve dağıtım sayfalarında tablo başlığında belirtilir. Yerel ayarı değiştirmek için tablonun başlık altında bulunan "Yerel ayarı Değiştir" düğmesine tıklayın. Bu bir yerel ayar doğrulama sayfasına götürür. Tabloya geri dönmek için "OK" (Tamam) düğmesine tıklayın.

Sonraki adımlara takip
* Bilgi [özel akustik model oluşturmak nasıl](cognitive-services-custom-speech-create-acoustic-model.md) tanıma doğruluğunu artırmak için
* Bilgi [özel dil modeli oluşturmak nasıl](cognitive-services-custom-speech-create-language-model.md) , tanıma oranını artırmak için
* İzleyin [transkripsiyonu yönergeleri](cognitive-services-custom-speech-transcription-guidelines.md) verilerinizi hazırlama
