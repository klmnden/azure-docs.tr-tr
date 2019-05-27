---
title: Azure Data Factory'de yürütme veri akışı etkinliği | Microsoft Docs
description: Gelen veri yürütmek nasıl bir veri fabrikası işlem hattı akar.
services: data-factory
documentationcenter: ''
author: kromerm
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/22/2019
ms.author: makromer
ms.openlocfilehash: b0a6c6feae11f8daeed54c5e763dbff3aa711652
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66153536"
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
          "coreCount": 8,
      }
}

```

## <a name="type-properties"></a>Tür özellikleri

* ```dataflow``` yürütmek istediğiniz veri akışını varlık adı
* ```compute``` Spark yürütme ortamını açıklar
* ```coreCount``` Veri akışınız için bu etkinliği yürütme atamak için çekirdek sayısı

![Veri akışını yürütecek](media/data-flow/activity-data-flow.png "veri akışını yürütecek")

### <a name="run-on"></a>Çalıştırma yeri

Bu, veri akışı yürütülmesi için işlem ortamını seçin. Azure otomatik olarak çözülmeli varsayılan Integration Runtime varsayılandır. Bu seçenek veri fabrikası ile aynı bölgede Spark ortamında veri akışını yürütecek. İşlem türü, işlem ortamı için başlatma birkaç dakika sürer anlamına gelir. bir işi küme olacaktır.

### <a name="debugging-pipelines-with-data-flows"></a>Bir veri akışı işlem hatlarında hata ayıklama

![Düğme hata ayıklama](media/data-flow/debugbutton.png "Hata Ayıkla düğmesine")

Veri akışı hata ayıklama veri akışlarınızı çalıştırma bir işlem hattı hata ayıklama etkileşimli olarak test etmek için warmed küme yararlanmak için kullanın. İçinde bir işlem hattı, bir veri akışı test etmek için işlem hattı hata ayıklama seçeneğini kullanın.

### <a name="run-on"></a>Çalıştırma yeri

Bu veri akışı, Etkinlik yürütme için kullanılacak hangi Integration Runtime tanımlayan gerekli bir alandır. Varsayılan olarak, varsayılan otomatik Çözümle Azure tümleştirme çalışma zamanının Data Factory kullanır. Ancak, kendi Azure tümleştirme belirli bölgeleri tanımlamak, veri akış Etkinlik yürütme için işlem türü, çekirdek sayısı ve TTL çalışma zamanları oluşturabilirsiniz.

8 çekirdek genel bilgi işlem, bir TTL 60 dakika ile yürütme veri akışı için varsayılan ayardır.

Veri akışı etkinlikleriniz için Spark yürütme ortamı üzerinde kontrol sizde. İçinde [Azure tümleştirme çalışma zamanı](concepts-integration-runtime.md) işlem türü (genel amaçlı, bellek için iyileştirilmiş ve işlem için iyileştirilmiş) çalışan çekirdek sayısı ve zaman yaşam yürütme altyapısı, veri akışı işlem ile eşleşecek şekilde ayarlamak için ayarlar gereksinimleri. Ayrıca, TTL ayarlama için iş yürütmeleri hemen kullanılamıyorsa, normal bir küme tutmanıza olanak sağlar.

![Azure tümleştirme çalışma zamanı](media/data-flow/ir-new.png "Azure tümleştirme çalışma zamanı")

### <a name="staging-area"></a>Hazırlama alanı

Azure veri ambarı'na verilerinizi indirme, Polybase toplu iş yükünüz için bir hazırlama konumu seçmeniz gerekir.

## <a name="parameterized-datasets"></a>Parametreli veri kümeleri

Parametreli veri kümeleri kullanıyorsanız, parametre değerlerini ayarlamak emin olun.

![Veri akışı parametrelerin](media/data-flow/params.png "parametreleri")

### <a name="debugging-parameterized-data-flows"></a>Hata ayıklama parametreli veri akışları

Yalnızca bir veri akışı işlem hattı yürütme veri akışı etkinliği kullanarak çalıştırma hata ayıklama parametreli kümelerinden ile hata ayıklaması yapabilirsiniz. Şu anda, ADF veri akışını etkileşimli hata ayıklama oturumlarında, parametreli veri kümeleriyle çalışmaz. İşlem hattı yürütme ve çalıştırmaların hata ayıklama parametreleriyle çalışır.

Tasarım zamanında tam meta veri sütunu yayma sahip statik bir veri kümesi, veri akışı oluşturmak iyi bir uygulamadır. Veri akışı işlem hattınızı kullanıma hazır hale getirme, statik veri kümesi bir dinamik parametreli veri kümesi ile değiştirin.

## <a name="next-steps"></a>Sonraki adımlar
Data Factory tarafından desteklenen diğer denetim akışı etkinlikleri bakın: 

- [If Koşulu Etkinliği](control-flow-if-condition-activity.md)
- [İşlem Hattı Yürütme Etkinliği](control-flow-execute-pipeline-activity.md)
- [Her etkinlik için](control-flow-for-each-activity.md)
- [Meta Veri Alma Etkinliği](control-flow-get-metadata-activity.md)
- [Arama Etkinliği](control-flow-lookup-activity.md)
- [Web etkinliği](control-flow-web-activity.md)
- [Bitiş Etkinliği](control-flow-until-activity.md)
