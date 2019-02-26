---
title: Azure Data Factory'de yürütme veri akışı etkinliği | Microsoft Docs
description: Veri akışları yürütme veri akışı etkinliğini çalıştırır.
services: data-factory
documentationcenter: ''
author: kromerm
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/22/2019
ms.author: makromer
ms.openlocfilehash: bc72fe2492d2eb38d60c6e96dcca35af5fb825ec
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2019
ms.locfileid: "56808710"
---
# <a name="execute-data-flow-activity-in-azure-data-factory"></a>Azure Data Factory'de yürütme veri akışı etkinliği
Yürütme veri akışı etkinliği hattının hata ayıklama (sanal) ve tetiklenen işlem hattı çalıştırmaları, ADF veri akışı çalıştırmak için kullanın.

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

## <a name="syntax"></a>Sözdizimi

```json
{
    "name": "MyDataFlowActivity",
    "type": "ExecuteDataFlow",
    "typeProperties": {
      "dataflow": {
         "referenceName": "dataflow1",
         "type": "DataFlowReference"
      },
        "compute": {
          "computeType": "General",
          "dataTransformationUnits": 4,
          "coreCount": 8,
          "numberOfNodes": 0
      }
}

```

## <a name="type-properties"></a>Tür özellikleri

* ```dataflow``` yürütmek istediğiniz veri akışını varlık adı
* ```compute``` Spark yürütme ortamını açıklar

![Veri akışını yürütecek](media/data-flow/activity-data-flow.png "veri akışını yürütecek")

### <a name="run-on"></a>Çalıştırma yeri

Bu, veri akışı yürütülmesi için işlem ortamını seçin. Azure otomatik olarak çözülmeli varsayılan Integration Runtime varsayılandır. Bu seçenek veri fabrikası ile aynı bölgede Spark ortamında veri akışını yürütecek. İşlem türü, işlem ortamı için başlatma birkaç dakika sürer anlamına gelir. bir işi küme olacaktır.

Adanmış bir IR seçerseniz tutturulmuş bir bölgeyle yeni bir Azure IR oluşturma ve veri akışı gereksinimlerinizi karşılayacak boyutları işlem kullanabilirsiniz. Döndürme yukarı bu seçeneği hemen ilk işini dağıtıldıktan sonra Başlangıç olacak etkileşimli kümeleri. Bu küme, son iş yürütüldükten sonra TTL süresi dolana kadar geçerli kalır.

### <a name="compute-type"></a>İşlem türü

Genel amaçlı, işlem için iyileştirilmiş veya bellek için iyileştirilmiş veri akışınız gereksinimlerinize bağlı olarak, seçebilirsiniz.

### <a name="core-count"></a>Çekirdek sayısı

Kaç tane çekirdeğim işe atamak istediğiniz seçin. Küçük işler için daha az çekirdek daha iyi çalışır.

### <a name="staging-area"></a>Hazırlama alanı

Azure veri ambarı'na verilerinizi indirme, Polybase toplu iş yükünüz için bir hazırlama konumu seçmeniz gerekir.

## <a name="parameterized-datasets"></a>Parametreli veri kümeleri

Parametreli veri kümeleri kullanıyorsanız, parametre değerlerini ayarlamak emin olun.

![Veri akışı parametrelerin](media/data-flow/params.png "parametreleri")

### <a name="debugging-parameterized-data-flows"></a>Hata ayıklama parametreli veri akışları

Yalnızca bir veri akışı işlem hattı yürütme veri akışı etkinliği kullanarak çalıştırma hata ayıklama parametreli kümelerinden ile hata ayıklaması yapabilirsiniz. Şu anda, ADF veri akışını etkileşimli hata ayıklama oturumlarında, parametreli veri kümeleriyle çalışmaz. İşlem hattı yürütme ve çalıştırmaların hata ayıklama parametreleriyle çalışır.

## <a name="next-steps"></a>Sonraki adımlar
Data Factory tarafından desteklenen diğer denetim akışı etkinlikleri bakın: 

- [If Koşulu Etkinliği](control-flow-if-condition-activity.md)
- [İşlem Hattı Yürütme Etkinliği](control-flow-execute-pipeline-activity.md)
- [Her etkinlik için](control-flow-for-each-activity.md)
- [Meta Veri Alma Etkinliği](control-flow-get-metadata-activity.md)
- [Arama Etkinliği](control-flow-lookup-activity.md)
- [Web etkinliği](control-flow-web-activity.md)
- [Bitiş Etkinliği](control-flow-until-activity.md)
