---
title: Video Indexer kavramları
titlesuffix: Azure Cognitive Services
description: Bu konuda, Video Indexer hizmeti bazı kavramlarını açıklar.
services: cognitive services
author: juliako
manager: cgronlun
ms.service: cognitive-services
ms.component: video-indexer
ms.topic: conceptual
ms.date: 09/15/2018
ms.author: juliako
ms.openlocfilehash: 740f13e90397650ed9274937b16254e46c6deced
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "45984132"
---
# <a name="video-indexer-concepts"></a>Video Indexer kavramları
 
Bu makalede, Video Indexer hizmeti bazı kavramlar açıklanır.
    
## <a name="summarized-insights"></a>Özetlenen öngörüleri

Özetlenen ınsights içeren toplu bir görünümünü verilerin: yüz, konular, duyguları. Örneğin, her zaman aralıkları binlerce giderek ve bunun içinde hangi yüzleri olan denetimi yerine özetlenen öngörüleri içeren tüm yüzleri ve her biri, içinde göründüğü zaman aralıkları ve % zaman gösterilir.

## <a name="time-range-vs-adjusted-time-range"></a>zaman aralığı ve ayarlanmış bir zaman aralığı

TimeRange özgün video zaman aralığıdır. AdjustedTimeRange geçerli çalma göreli zaman aralığı. Bir çalma listesi farklı videoları farklı satırlardan oluşturma olduğundan, 1 saatlik bir video alabilir ve bunu, örneğin, 10:00-10:15 yalnızca 1 satırından kullanın. Bu durumda olduğu zaman aralığı 10:00-10:15, ancak adjustedTimeRange 00:00-00 1 satır içeren bir çalma listesi olacaktır: 15.
 
## <a name="blocks"></a>blokları

Bloklar, Git verilerine daha kolay hale getirmek için yöneliktir. Örneğin, blok konuşmacıları değiştirin ya da uzun duraklama temel alınarak bölünmesine.

## <a name="next-steps"></a>Sonraki adımlar

Çalışmaya başlama hakkında daha fazla bilgi için bkz: [kaydolun ve ilk videonuzu karşıya nasıl](video-indexer-get-started.md).

## <a name="see-also"></a>Ayrıca bkz.

[Video Indexer genel bakış](video-indexer-overview.md)
