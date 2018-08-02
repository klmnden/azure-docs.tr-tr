---
title: Azure Video Indexer kavramları | Microsoft Docs
description: Bu konuda, Video Indexer hizmeti bazı kavramlarını açıklar.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 07/31/2018
ms.author: juliako
ms.openlocfilehash: 224c8b05027f51fb99c8d58be34c3604032c0f77
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39399504"
---
# <a name="video-indexer-concepts"></a>Video Indexer kavramları
 
Bu konuda, Video Indexer hizmeti bazı kavramlarını açıklar.
    
## <a name="summarized-insights"></a>Özetlenen öngörüleri

Özetlenen ınsights içeren toplu bir görünümünü verilerin: yüz, anahtar sözcükler yaklaşımları. Örneğin, her zaman aralıkları binlerce giderek ve bunun içinde hangi yüzleri olan denetimi yerine özetlenen öngörüleri içeren tüm yüzleri ve her biri, içinde göründüğü zaman aralıkları ve % zaman gösterilir.

## <a name="topicskeywords"></a>Konuları/anahtar sözcükleri

Video Indexer metni ayıklar, anahtar ifadeleri listesinde konuları/anahtar sözcüklerdir. Örneğin, Scott Guthrie video aşağıdaki konular/anahtar sözcükler içerebilir: güvenlik, Azure, Microsoft Cloud, gelir.

## <a name="sentiments"></a>Yaklaşımlar

Video Indexer dökümleri çözümler yaklaşımları de algılar. Örneğin, "Bu heyecan verici bir olaydır" pozitif yaklaşımı olur.

## <a name="time-range-vs-adjusted-time-range"></a>zaman aralığı ve ayarlanmış bir zaman aralığı

TimeRange özgün video zaman aralığıdır. AdjustedTimeRange geçerli çalma göreli zaman aralığı. Bir çalma listesi farklı videoları farklı satırlardan oluşturma olduğundan, 1 saatlik bir video alabilir ve bunu, örneğin, 10:00-10:15 yalnızca 1 satırından kullanın. Bu durumda olduğu zaman aralığı 10:00-10:15, ancak adjustedTimeRange 00:00-00 1 satır içeren bir çalma listesi olacaktır: 15.
 
## <a name="blocks"></a>blokları

Bloklar, Git verilerine daha kolay hale getirmek için yöneliktir. Örneğin, blok konuşmacıları değiştirin ya da uzun duraklama temel alınarak bölünmesine.

## <a name="next-steps"></a>Sonraki adımlar

Çalışmaya başlama hakkında daha fazla bilgi için bkz: [kaydolun ve ilk videonuzu karşıya nasıl](video-indexer-get-started.md).

## <a name="see-also"></a>Ayrıca bkz.

[Video Indexer genel bakış](video-indexer-overview.md)
