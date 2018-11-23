---
title: Video Indexer kavramları
titlesuffix: Azure Media Services
description: Bu konuda, Video Indexer hizmeti bazı kavramlarını açıklar.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 11/19/2018
ms.author: juliako
ms.openlocfilehash: fde98dac45eeda9ec2c842b9d8e0f93134ab8092
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2018
ms.locfileid: "52292463"
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
