---
title: Azure Video dizin oluşturucu kavramları | Microsoft Docs
description: Bu konuda Video dizin oluşturucu hizmeti bazı kavramlarını açıklar.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 04/17/2017
ms.author: juliako
ms.openlocfilehash: 01d92a6b55d2fb2c09cee333f482d79d2cdf763c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354574"
---
# <a name="video-indexer-concepts"></a>Video dizin oluşturucu kavramları
 
Bu konuda Video dizin oluşturucu hizmeti bazı kavramlarını açıklar.
    
## <a name="summarized-insights"></a>Özetlenen Öngörüler

Özetlenen Öngörüler verileri birleşik bir görünümünü içerir: yazıtipleri, anahtar sözcükler düşüncelerin. Örneğin, her zaman aralıklarına binlerce giderek ve hangi yüzeyleri içinde olan denetimi, yerine özetlenen Öngörüler tüm yüzeyleri içerir ve her biri, görünür zaman aralıkları ve zaman yüzdesi gösterilir.

## <a name="topicskeywords"></a>Konular/anahtar sözcükler

Konuları/Video dizin oluşturucu metinden ayıklar anahtar tümcecikleri listesinde anahtar sözcükler. Örneğin, aşağıdaki konuları/anahtar sözcükler Scott Guthrie video içerebilir: güvenlik, Azure, Microsoft Cloud, gelir.

## <a name="sentiments"></a>Yaklaşımlar

Video dizin oluşturucu dökümleri çözümler düşüncelerin de algılar. Örneğin, "Bu çok heyecan verici bir olaydır" pozitif düşünceleri olur.

## <a name="time-range-vs-adjusted-time-range"></a>zaman aralığı ayarlanan zaman aralığı karşılaştırması

TimeRange özgün videoda zaman aralığıdır. AdjustedTimeRange geçerli çalma göreli zaman aralığıdır. Farklı videolar farklı satırlarından bir çalma listesi oluşturabilirsiniz olduğundan, 1 saatlik video alın ve bunu, örneğin, 10:00-10:15 yalnızca 1 satırından kullanın. Bu durumda olduğu zaman aralığı 10:00-10:15 ancak adjustedTimeRange 00:00-00 1 satır içeren bir çalma listesi olacaktır: 15.
 
## <a name="blocks"></a>Blokları

Blokları verilerine Git kolaylaştırmak için yöneliktir. Örneğin, blok konuşmacılar değiştirmek veya uzun bir duraklama göre ayrılmış.

## <a name="next-steps"></a>Sonraki adımlar

Kullanmaya başlama hakkında daha fazla bilgi için bkz: [kaydolun ve ilk videonuzu karşıya yüklemek nasıl](video-indexer-get-started.md).

## <a name="see-also"></a>Ayrıca bkz.

[Video dizin oluşturucu genel bakış](video-indexer-overview.md)

