---
title: "Azure veri fabrikası'nda Sistem değişkenleri | Microsoft Docs"
description: "Bu makalede Azure Data Factory ile desteklenen sistem değişkenleri açıklanmaktadır. Data Factory varlıklarını tanımlarken ifadelerde bu değişkenleri kullanabilirsiniz."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2017
ms.author: shlo
ms.openlocfilehash: 274c003e0697ba08d010c3bf13724461a4b624ee
ms.sourcegitcommit: c50171c9f28881ed3ac33100c2ea82a17bfedbff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2017
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
| @pipeline(). Çalıştırma kodu | Çalıştırma belirli ardışık kimliği | 
| @pipeline(). TriggerType | (El ile Zamanlayıcı) ardışık düzen çağrılan tetikleyici türü | 
| @pipeline(). Tetikleyici No| Ardışık Düzen çağırır Tetikleyici kimliği |
| @pipeline(). Tetikleyiciadı| Ardışık Düzen çağırır Tetikleyici adı |
| @pipeline(). TriggerTime| Saati ardışık çağrılan tetikleyici. Tetikleyici gerçek Mazotlu süresi olan zamanlanan saat dır. Örneğin, `13:20:08.0149599Z` yerine döndürülür`13:20:00.00Z` |

## <a name="trigger-scope"></a>Tetikleyici kapsamı:

| Değişken adı | Açıklama |
| --- | --- |
| () .scheduledTime Tetikle |Tetikleyici çalıştırmak ardışık düzen çağırmak için ne zaman zamanlandığı saat. Örneğin, her 5 dk. harekete için bir tetikleyici, bu değişken döndürecekti `2017-06-01T22:20:00Z`, `2017-06-01T22:25:00Z`, `2017-06-01T22:29:00Z` sırasıyla.|
| () .startTime Tetikle |Saati tetikleyici **gerçekten** çalıştırmak ardışık düzen çağrılacak tetiklenir. Her 5 dk. harekete için bir tetikleyici, bu değişken şöyle bir şey gibi döndürebilir `2017-06-01T22:20:00.4061448Z`, `2017-06-01T22:25:00.7958577Z`, `2017-06-01T22:29:00.9935483Z` sırasıyla.|

## <a name="next-steps"></a>Sonraki adımlar
Bu değişkenler ifadelerinde nasıl kullanıldığı konusunda daha fazla bilgi için bkz: [ifade dili & işlevleri](control-flow-expression-language-functions.md). 
