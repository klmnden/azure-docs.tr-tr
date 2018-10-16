---
title: Özel konuşma hizmeti ölçümleri ve Azure ile ilgili kotalar | Microsoft Docs
description: Azure ölçümleri ve özel konuşma Hizmeti kotaları hakkında bilgiler.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 07/08/2017
ms.author: panosper
ms.openlocfilehash: bd39976691aab0c2333afe9fafc9c5a8cc518b67
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49339568"
---
# <a name="custom-speech-service-meters-and-quotas"></a>Özel konuşma hizmeti ölçümleri ve kotalar

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

Bulut tabanlı özel konuşma hizmeti sayesinde konuşma modelleri için metne dönüştürme konuşma transkripsiyonu özelleştirebilirsiniz.

Özel konuşma hizmeti kullanmaya başlamak için Git [özel konuşma hizmeti portalı](https://cris.ai).

Geçerli fiyatlandırma ölçümleri, Git [özel konuşma hizmeti için Bilişsel hizmetler fiyatlandırması](https://azure.microsoft.com/pricing/details/cognitive-services/custom-speech-service/) sayfası.

## <a name="tiers-explained"></a>Açıklanan katmanları
Test ve yalnızca prototip için ücretsiz F0 katmanı kullanmak önerin. Üretim sistemleri için S2 katmanı kullanmak, biz önerin. S2 katmanı kullanarak dağıtımınıza senaryonuz gerektiren, Ölçek birimleri (su) sayısını ölçeklendirebilirsiniz.

> [!NOTE]
> *Olamaz* F0 katmanı ve S2 katmanı arasında geçirme.
>

## <a name="meters-explained"></a>Açıklanan ölçümleri

### <a name="scale-out"></a>Ölçeklendirme
Ölçeği genişletme, yeni fiyatlandırma modeliyle yayımlandı yeni bir özelliktir. Ölçeği genişletme kullanarak modelinizi işleyebileceği eş zamanlı istek sayısını kontrol edebilirsiniz.

SU ölçü kullanarak eş zamanlı istek ayarlayabilirsiniz **Model dağıtımı oluşturma** görünümü. Daha fazla bilgi için [özel bir konuşmayı metne uç noktası oluşturma](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-endpoint.md). Model tüketen öngörülmesine trafik miktarını bağlı olarak, uygun sayıda SUs seçebilirsiniz. 

> [!NOTE]
> Her ölçek birimi 5 eşzamanlı istek garanti eder. 1 veya daha çok SUs uygun olarak satın alabilirsiniz. SUs sayısı 1 artışlarla artar çünkü eş zamanlı istek sayısını 5 artışlarla artırmak için garanti edilir.
>

### <a name="log-management"></a>Günlük yönetimi
Yeni dağıtılan bir modelde ek bir ücret ödemeden ses izlemeleri kapalı geçiş seçebilirsiniz. Özel konuşma hizmeti adet ses isteği veya dökümleri, modelden günlüğe kaydetmez.

## <a name="next-steps"></a>Sonraki adımlar
Özel konuşma hizmeti kullanma hakkında daha fazla bilgi için Git [özel konuşma hizmeti portalı](https://cris.ai).

* [Kullanmaya başlama](cognitive-services-custom-speech-get-started.md)
* [SSS](cognitive-services-custom-speech-faq.md)
* [Sözlük](cognitive-services-custom-speech-glossary.md)
 