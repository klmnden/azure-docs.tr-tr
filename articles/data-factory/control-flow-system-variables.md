---
title: Azure Data factory'de sistem değişkenlerini | Microsoft Docs
description: Bu makalede Azure Data Factory tarafından desteklenen sistem değişkenleri açıklanır. Data Factory varlıklarını tanımlarken ifadelerinde bu değişkenleri kullanabilirsiniz.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/12/2018
ms.author: shlo
ms.openlocfilehash: 9a4d5acfe16a2fdbb3b631cb8baf6cb8e90a7d58
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60935713"
---
# <a name="system-variables-supported-by-azure-data-factory"></a>Azure Data Factory tarafından desteklenen sistem değişkenleri
Bu makalede Azure Data Factory tarafından desteklenen sistem değişkenleri açıklanır. Data Factory varlıklarını tanımlarken ifadelerinde bu değişkenleri kullanabilirsiniz.

## <a name="pipeline-scope"></a>İşlem hattı kapsam
Bu sistem değişkenlerini herhangi bir işlem hattı JSON başvurulabilir.

| Değişken adı | Açıklama |
| --- | --- |
| @pipeline().DataFactory |Data factory işlem hattı adı içinde çalışıyor |
| @pipeline().Pipeline |İşlem hattı adı |
| @pipeline().RunId | Özel işlem hattı çalıştırma kimliği |
| @pipeline(). TriggerType | Çağrılan işlem hattı (el ile Zamanlayıcı) tetikleyici türü |
| @pipeline().TriggerId| İşlem hattını çağıran bir tetikleyici kimliği |
| @pipeline().TriggerName| İşlem hattını çağıran bir tetikleyici adı |
| @pipeline().TriggerTime| Saati işlem hattını çağıran bir tetikleyici. Tetikleme zamanı gerçek tetiklenme saati, zamanlanan saat ' dir. Örneğin, `13:20:08.0149599Z` yerine döndürülür `13:20:00.00Z` |

## <a name="schedule-trigger-scope"></a>Zamanlama tetikleyicisi kapsamı
Tetikleyici türü ise bu sistem değişkenleri JSON tetikleyici içinde herhangi bir yerde başvurulabilir: "ScheduleTrigger."

| Değişken adı | Açıklama |
| --- | --- |
| @trigger() .scheduledTime |Zaman zaman tetikleyici işlem hattı çalıştırması çağırmak için zamanlandı. Örneğin, her 5 dakikada bir çalıştırılan bir tetikleyici için bu değişkeni döndürecekti `2017-06-01T22:20:00Z`, `2017-06-01T22:25:00Z`, `2017-06-01T22:29:00Z` sırasıyla.|
| @trigger() .startTime |Saati tetikleyici **gerçekten** tetiklenen işlem hattı çalıştırması çağırmak için. Örneğin, her 5 dakikada bir çalıştırılan bir tetikleyici için bu değişkeni aşağıdakine benzer döndürme olasılığı `2017-06-01T22:20:00.4061448Z`, `2017-06-01T22:25:00.7958577Z`, `2017-06-01T22:29:00.9935483Z` sırasıyla.|

## <a name="tumbling-window-trigger-scope"></a>Atlayan pencere tetikleyicisi kapsamı
Tetikleyici türü ise bu sistem değişkenleri JSON tetikleyici içinde herhangi bir yerde başvurulabilir: "TumblingWindowTrigger."

| Değişken adı | Açıklama |
| --- | --- |
| @trigger(). outputs.windowStartTime |Tetikleyici işlem hattı çalıştırma başlatmak üzere zamanlandı zaman penceresi başlangıcı. Atlayan pencere tetikleyicisi "saatlik" sıklığını varsa bu saat başına zaman olacaktır.|
| @trigger(). outputs.windowEndTime |Tetikleyici, işlem hattı çalıştırması çağırmak için zamanlandı olduğunda pencerenin sonu. Atlayan pencere tetikleyicisi "saatlik" sıklığını varsa bu saatin sonunu zaman olacaktır.|
## <a name="next-steps"></a>Sonraki adımlar
Bu değişkenler, ifadelerin nasıl kullanıldığı hakkında daha fazla bilgi için bkz: [ifade dili & işlevleri](control-flow-expression-language-functions.md).
