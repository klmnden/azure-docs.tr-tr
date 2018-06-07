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
ms.date: 01/10/2018
ms.author: shlo
ms.openlocfilehash: 498ca342976888ab4ae67826689bb5e21325b94d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34618258"
---
# <a name="system-variables-supported-by-azure-data-factory"></a>Azure Data Factory ile desteklenen sistem değişkenleri
Bu makalede Azure Data Factory ile desteklenen sistem değişkenleri açıklanmaktadır. Data Factory varlıklarını tanımlarken ifadelerde bu değişkenleri kullanabilirsiniz. 

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [işlevleri ve değişkenler sürüm 1 veri fabrikasında](v1/data-factory-functions-variables.md).


## <a name="pipeline-scope"></a>Ardışık Düzen kapsam:

| Değişken adı | Açıklama |
| --- | --- |
| @pipeline(). DataFactory |Veri Fabrikası çalıştırmak ardışık düzen adını içinde çalışıyor | 
| @pipeline(). Ardışık Düzen |Ardışık Düzen adı |
| @pipeline().RunId | Çalıştırma belirli ardışık kimliği | 
| @pipeline(). TriggerType | (El ile Zamanlayıcı) ardışık düzen çağrılan tetikleyici türü | 
| @pipeline().TriggerId| Ardışık Düzen çağırır Tetikleyici kimliği |
| @pipeline(). Tetikleyiciadı| Ardışık Düzen çağırır Tetikleyici adı |
| @pipeline(). TriggerTime| Saati ardışık çağrılan tetikleyici. Tetikleyici gerçek Mazotlu süresi olan zamanlanan saat dır. Örneğin, `13:20:08.0149599Z` yerine döndürülür `13:20:00.00Z` |

## <a name="trigger-scope"></a>Tetikleyici kapsamı:

| Değişken adı | Açıklama |
| --- | --- |
| trigger().scheduledTime |Tetikleyici çalıştırmak ardışık düzen çağırmak için ne zaman zamanlandığı saat. Örneğin, her 5 dk. harekete için bir tetikleyici, bu değişken döndürecekti `2017-06-01T22:20:00Z`, `2017-06-01T22:25:00Z`, `2017-06-01T22:29:00Z` sırasıyla.|
| () .startTime Tetikle |Saati tetikleyici **gerçekten** çalıştırmak ardışık düzen çağrılacak tetiklenir. Her 5 dk. harekete için bir tetikleyici, bu değişken şöyle bir şey gibi döndürebilir `2017-06-01T22:20:00.4061448Z`, `2017-06-01T22:25:00.7958577Z`, `2017-06-01T22:29:00.9935483Z` sırasıyla.|

## <a name="next-steps"></a>Sonraki adımlar
Bu değişkenler ifadelerinde nasıl kullanıldığı konusunda daha fazla bilgi için bkz: [ifade dili & işlevleri](control-flow-expression-language-functions.md). 
