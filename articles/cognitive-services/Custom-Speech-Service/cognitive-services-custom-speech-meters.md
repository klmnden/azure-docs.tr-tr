---
title: Özel konuşma hizmet Ölçümler ve Azure ile ilgili kotalar | Microsoft Docs
description: Azure Ölçümler ve özel konuşma hizmetinin kotaları hakkında bilgiler.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 07/08/2017
ms.author: panosper
ms.openlocfilehash: d2225dec818c600febfad2f9ebc42594f6ac09ac
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351472"
---
# <a name="custom-speech-service-meters-and-quotas"></a>Özel konuşma hizmet Ölçümler ve kotaları

Bulut tabanlı özel konuşma hizmetiyle konuşma modelleri konuşma metin transcription için özelleştirebilirsiniz.

Özel konuşma hizmeti kullanmaya başlamak için Git [özel konuşma hizmet portalı](https://cris.ai).

Geçerli fiyatlandırma ölçümler, Git [Bilişsel hizmetler için özel konuşma hizmeti fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/custom-speech-service/) sayfası.

## <a name="tiers-explained"></a>Açıklanan katmanları
Test etme ve yalnızca prototip için ücretsiz F0 katmanı kullanmak önerin. Üretim sistemleri için biz S2 katmanı kullanmak üzere önerin. S2 katmanı kullanarak dağıtımınızı senaryonuz gerektirir ölçek birimleri (SUs) sayıda ölçeklendirebilirsiniz.

> [!NOTE]
> *Olamaz* F0 katmanı ve S2 katmanı arasında geçirme.
>

## <a name="meters-explained"></a>Açıklanan ölçümler

### <a name="scale-out"></a>Ölçeklendirme
Ölçek çıkışı, yeni fiyatlandırma modeli ile yayımlandı yeni bir özelliktir. Ölçeği Genişlet kullanarak modelinizi işleyebilir eşzamanlı istek sayısını kontrol edebilirsiniz.

SU ölçü kullanarak eşzamanlı istek ayarlayabilirsiniz **oluşturma modeli dağıtım** görünümü. Daha fazla bilgi için bkz: [özel bir konuşma metin uç noktası oluşturma](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-endpoint.md). Model tüketen envisage trafik miktarını bağlı olarak, SUs uygun sayıda seçebilirsiniz. 

> [!NOTE]
> Her bir ölçek birimi 5 eşzamanlı istek güvence altına alır. Uygun olarak 1 veya daha çok SUs satın alabilirsiniz. 1 artışlarla SUs sayısını artırır çünkü eşzamanlı istek sayısı 5 artışlarla artırmak garanti.
>

### <a name="log-management"></a>Günlüğü Yönetimi
Ek bir ücret ödemeden yeni dağıtılan bir model için ses izleri kapalı geçiş yapmak için tercih edebilirsiniz. Özel konuşma hizmet ses istekleri ya da dökümleri bu modelden günlüğe kaydetmez.

## <a name="next-steps"></a>Sonraki adımlar
Özel konuşma hizmetini kullanma hakkında daha fazla bilgi için Git [özel konuşma hizmet portalı](https://cris.ai).

* [Kullanmaya başlama](cognitive-services-custom-speech-get-started.md)
* [SSS](cognitive-services-custom-speech-faq.md)
* [Sözlük](cognitive-services-custom-speech-glossary.md)
 