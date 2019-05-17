---
title: Video Indexer kavramları
titlesuffix: Azure Media Services
description: Bu konuda, Video Indexer hizmeti bazı kavramlarını açıklar.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: juliako
ms.openlocfilehash: 156eceba856bf159d4821360639a0641d3ed02be
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65799069"
---
# <a name="video-indexer-concepts"></a>Video Indexer kavramları
 
Bu makalede, Video Indexer hizmeti bazı kavramlar açıklanır.
    
## <a name="summarized-insights"></a>Özetlenen öngörüleri

Özetlenen ınsights içeren toplu bir görünümünü verilerin: yüz, konular, duyguları. Örneğin, her zaman aralıkları binlerce giderek ve bunun içinde hangi yüzleri olan denetimi yerine özetlenen öngörüleri içeren tüm yüzleri ve her biri, içinde göründüğü zaman aralıkları ve % zaman gösterilir.

## <a name="time-range-vs-adjusted-time-range"></a>zaman aralığı ve ayarlanmış bir zaman aralığı

TimeRange özgün video zaman aralığıdır. AdjustedTimeRange geçerli çalma göreli zaman aralığı. Bir çalma listesi farklı videoları farklı satırlardan oluşturma olduğundan, 1 saatlik bir video alabilir ve bunu, örneğin, 10:00-10:15 yalnızca 1 satırından kullanın. Bu durumda olduğu zaman aralığı 10:00-10:15, ancak adjustedTimeRange 00:00-00 1 satır içeren bir çalma listesi olacaktır: 15.
 
## <a name="blocks"></a>blokları

Bloklar, Git verilerine daha kolay hale getirmek için yöneliktir. Örneğin, blok ne zaman konuşmacının değiştiğine ya da uzun duraklamalar olduğuna bağlı olarak çözümlenebilir.

## <a name="next-steps"></a>Sonraki adımlar

Çalışmaya başlama hakkında daha fazla bilgi için bkz: [kaydolun ve ilk videonuzu karşıya nasıl](video-indexer-get-started.md).

## <a name="see-also"></a>Ayrıca bkz.

[Video Indexer’a genel bakış](video-indexer-overview.md)
