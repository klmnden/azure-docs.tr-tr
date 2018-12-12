---
title: Bölge - konuşma Hizmetleri
titlesuffix: Azure Cognitive Services
description: Konuşma hizmeti bölgeleri için başvuru.
services: cognitive-services
author: mahilleb-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: mahilleb
ms.custom: seodec18
ms.openlocfilehash: d41213d72d40555d8dc5aeab76040fc556dae774
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53091654"
---
# <a name="speech-service-supported-regions"></a>Konuşma hizmeti desteklenen bölgeler

Konuşma hizmeti, uygulamanızın sesi metne dönüştürün, gizli metin okuma ve konuşma çevirisi gerçekleştirin sağlar. Hizmet, REST API'leri ve Speech SDK'sı için benzersiz uç noktaları ile birden fazla bölgede kullanılabilir.

Aboneliğiniz için bölge eşleşen uç nokta kullandığınızdan emin olun.

## <a name="speech-sdk"></a>Konuşma SDK'sı

İçinde [konuşma hizmeti SDK'sı](speech-sdk.md), bölgelerin bir dize olarak belirtilen (örneğin, bir parametre olarak `SpeechConfig.FromSubscription` konuşma SDK for C#).

### <a name="speech-recognition-and-translation"></a>Konuşma tanıma ve çeviri

Bu bölgeler için Speech SDK'sı kullanılabilir **konuşma tanıma** ve **çeviri**:

  Bölge | Konuşma SDK parametresi | Konuşma özelleştirme portalı
 ------|-------|--------
 Batı ABD | `westus` | https://westus.cris.ai
 Batı ABD 2 | `westus2` | https://westus2.cris.ai
 Doğu ABD | `eastus` | https://eastus.cris.ai
 Doğu ABD 2 | `eastus2` | https://eastus2.cris.ai
 Doğu Asya | `eastasia` | https://eastasia.cris.ai
 Güneydoğu Asya | `southeastasia` | https://southeastasia.cris.ai
 Kuzey Avrupa | `northeurope` | https://northeurope.cris.ai
 Batı Avrupa | `westeurope` | https://westeurope.cris.ai


### <a name="intent-recognition"></a>Amaç tanıma

**Amaç tanıma** Speech SDK'sı paylaşımları için LUIS bölgeleri destekler. Kullanılabilir bölgelerin tam listesi için bkz: [bölgeler ve uç noktaları - LUIS yayımlama](https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions)

Kullanılabilir bölgeler için **niyeti tanıma** Speech SDK'sı listelenir [Language Understanding hizmeti bölge sayfası](/azure/cognitive-services/luis/luis-reference-regions).

Listelenen her yayımlama bölge için sağlanan kullanın **API bölge adı**. Örneğin, `westus` Batı ABD.

## <a name="rest-apis"></a>REST API'leri

Konuşma hizmeti de konuşma metin ve metin okuma istekleri için REST uç noktalarını kullanıma sunar.

### <a name="speech-to-text"></a>Konuşmayı Metne Dönüştürme

Konuşmayı metne referans belgeleri için bkz. [REST API'leri](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#speech-to-text).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

### <a name="text-to-speech"></a>Metin okuma

Metin okuma referans belgeleri için bkz. [REST API'leri](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#speech-to-text).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]
