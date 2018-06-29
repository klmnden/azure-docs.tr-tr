---
title: Azure veri fabrikası'nda Sistem değişkenleri | Microsoft Docs
description: Bu makalede Azure Data Factory ile desteklenen sistem değişkenleri açıklanmaktadır. Data Factory varlıklarını tanımlarken ifadelerde bu değişkenleri kullanabilirsiniz.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/12/2018
ms.author: shlo
ms.openlocfilehash: 43ea8703bdbfc23511c831a5f91c9461948cc254
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37059185"
---
# <a name="system-variables-supported-by-azure-data-factory"></a>Azure Data Factory ile desteklenen sistem değişkenleri
Bu makalede Azure Data Factory ile desteklenen sistem değişkenleri açıklanmaktadır. Data Factory varlıklarını tanımlarken ifadelerde bu değişkenleri kullanabilirsiniz.

## <a name="pipeline-scope"></a>Ardışık Düzen kapsam
Bu sistem değişkenleri herhangi bir yere JSON ardışık düzeninde başvurulabilir.

| Değişken adı | Açıklama |
| --- | --- |
| @pipeline(). DataFactory |Veri Fabrikası çalıştırmak ardışık düzen adını içinde çalışıyor |
| @pipeline(). Ardışık Düzen |Ardışık Düzen adı |
| @pipeline().RunId | Çalıştırma belirli ardışık kimliği |
| @pipeline(). TriggerType | (El ile Zamanlayıcı) ardışık düzen çağrılan tetikleyici türü |
| @pipeline().TriggerId| Ardışık Düzen çağırır Tetikleyici kimliği |
| @pipeline(). Tetikleyiciadı| Ardışık Düzen çağırır Tetikleyici adı |
| @pipeline(). TriggerTime| Saati ardışık çağrılan tetikleyici. Tetikleyici gerçek Mazotlu süresi olan zamanlanan saat dır. Örneğin, `13:20:08.0149599Z` yerine döndürülür `13:20:00.00Z` |

## <a name="schedule-trigger-scope"></a>Zamanlama tetikleyici kapsamı
Tetikleyici türü ise bu sistem değişkenleri herhangi bir yere tetikleyici JSON başvurulabilir: "ScheduleTrigger."

| Değişken adı | Açıklama |
| --- | --- |
| @trigger() .scheduledTime |Tetikleyici çalıştırmak ardışık düzen çağırmak için ne zaman zamanlandığı saat. Örneğin, her 5 dk. harekete için bir tetikleyici, bu değişken döndürecekti `2017-06-01T22:20:00Z`, `2017-06-01T22:25:00Z`, `2017-06-01T22:29:00Z` sırasıyla.|
| @trigger() .startTime |Saati tetikleyici **gerçekten** çalıştırmak ardışık düzen çağrılacak tetiklenir. Her 5 dk. harekete için bir tetikleyici, bu değişken şöyle bir şey gibi döndürebilir `2017-06-01T22:20:00.4061448Z`, `2017-06-01T22:25:00.7958577Z`, `2017-06-01T22:29:00.9935483Z` sırasıyla.|

## <a name="tumbling-window-trigger-scope"></a>Dönen pencere tetikleyici kapsamı
Tetikleyici türü ise bu sistem değişkenleri herhangi bir yere tetikleyici JSON başvurulabilir: "TumblingWindowTrigger."

| Değişken adı | Açıklama |
| --- | --- |
| @trigger(). outputs.windowStartTime |Tetikleyici ardışık düzen Çalıştır çağrılacak zamanlandığı zaman penceresi başlangıcı. Dönen pencere tetikleyici "saatlik" sıklığını varsa bu saat başına zamanında olacaktır.|
| @trigger(). outputs.windowEndTime |Tetikleyici çalıştırmak ardışık düzen çağrılacak zamanlandığı olduğunda pencerenin sonu. Dönen pencere tetikleyici "saatlik" sıklığını varsa bu saatin sonunu zamanında olacaktır.|
## <a name="next-steps"></a>Sonraki adımlar
Bu değişkenler ifadelerinde nasıl kullanıldığı konusunda daha fazla bilgi için bkz: [ifade dili & işlevleri](control-flow-expression-language-functions.md).
