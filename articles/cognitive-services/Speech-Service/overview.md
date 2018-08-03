---
title: Konuşma hizmeti (Önizleme) nedir?
description: 'Konuşma hizmeti, Microsoft Bilişsel hizmetler, parçası ayrı olarak önceden kullanılabilen çeşitli Azure konuşma Hizmetleri sahip: Bing konuşma (metin okuma ve konuşma tanıma kapsayan), özel konuşma tanıma ve konuşma çevirisi.'
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 0cd6d984ac9329112aa388e8d8ee808d4c3e6227
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39445643"
---
# <a name="what-is-the-speech-service-preview"></a>Konuşma hizmeti (Önizleme) nedir?

Konuşma hizmeti, Cortana ve Microsoft Office gibi diğer Microsoft ürünlerinde kullanılan teknolojileri tarafından desteklenir.  Bu aynı hizmet Bilişsel hizmet olarak tüm müşteriler için kullanılabilir. 

> [!NOTE]
> Konuşma hizmeti, şu anda genel Önizleme aşamasındadır. Burada düzenli olarak güncelleştirmeleri belgeler, ek kod örnekleri ve daha fazlası için döndürür.

Bir aboneliği ile Konuşma hizmetimiz geliştiriciler, uygulamaları güçlü konuşma özellikli özellikler sağlamak için kolay bir yol sağlar. Uygulamalarınızı, sesli komut, döküm, yazdırma, konuşma sentezi ve çeviri artık özelliğini.

## <a name="speech-service-features"></a>Konuşma hizmeti özellikleri

|İşlev|Açıklama|
|-|-|
|[Konuşma metin](speech-to-text.md)| Ses akışları, uygulamanızın girdi olarak kabul edebilen bir metne dönüştürür. Ayrıca tümleşir [Language Understanding hizmeti](https://docs.microsoft.com/azure/cognitive-services/luis/) kullanıcının amacını konuşma türetme (LUIS).|
|[Metin okuma](text-to-speech.md)| Düz metin, ses dosyası uygulamanızda teslim görünen doğal konuşma dönüştürür. Birden çok ses, cinsiyet veya vurgu, değişen birçok desteklenen diller için kullanılabilir. |
|[Konuşma çevirisi](speech-translation.md)| Neredeyse gerçek zamanlı Akış ses çevirmek veya kayıtlı konuşma işlemek için kullanılabilir. |
|[Konuşma cihaz SDK'sı](speech-devices-sdk.md)| Birleşik konuşma hizmeti sunulmasıyla birlikte, Microsoft ve iş ortakları için geliştirme konuşma özellikli cihazlar için iyileştirilmiş bir tümleşik donanım/yazılım platformu sunar |

## <a name="using-the-speech-service"></a>Konuşma hizmeti kullanma

Konuşma hizmeti iki şekilde kullanılabilir. [SDK'sı](speech-sdk.md) ağ protokolleri ayrıntılarını dengelediği. [REST API](rest-apis.md) herhangi bir programlama dilinde çalışır, ancak SDK tarafından sunulan tüm işlevleri sağlamaz.

|<br>Yöntem|Konuşma<br>Metni|Metni<br>Konuşma|Konuşma<br>Çeviri|<br>Açıklama|
|-|-|-|-|-|
|[SDK'ları](speech-sdk.md)|Evet|Hayır|Evet|Geliştirmeyi kolaylaştıran belirli programlama dilleri için kitaplıkları.|
|[REST](rest-apis.md)|Evet|Evet|Hayır|Konuşma uygulamanıza eklemek kolaylaştıran bir basit HTTP tabanlı API'ler.|

## <a name="next-steps"></a>Sonraki adımlar

Konuşma hizmeti için bir ücretsiz deneme aboneliği anahtar alın.

> [!div class="nextstepaction"]
> [Konuşma hizmeti ücretsiz olarak deneyin](get-started.md)
