---
title: Hadoop Pig etkinliği Azure Data Factory kullanarak veri dönüştürme | Microsoft Docs
description: Pig etkinlik bir Azure data factory'de bir üzerinde-isteğe bağlı/bilgisayarınızı kendi Hdınsight kümesinde Pig betikleri çalıştırmak için nasıl kullanabileceğinizi öğrenin.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/16/2018
ms.author: douglasl
ms.openlocfilehash: 9e769cc436011defe89b12680150e6f9c3b3faf8
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37049330"
---
# <a name="transform-data-using-hadoop-pig-activity-in-azure-data-factory"></a>Hadoop Pig etkinliği Azure Data Factory kullanarak veri dönüştürme
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-pig-activity.md)
> * [Geçerli sürüm](transform-data-using-hadoop-pig.md)

Veri Fabrikası Hdınsight Pig etkinliğinde [ardışık düzen](concepts-pipelines-activities.md) üzerinde Pig sorguları yürüten [kendi](compute-linked-services.md#azure-hdinsight-linked-service) veya [isteğe bağlı](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Hdınsight kümesi. Bu makalede derlemeler [veri dönüştürme etkinlikleri](transform-data.md) makalesi, veri dönüştürme ve desteklenen dönüştürme etkinliklerinin genel bir bakış sunar.

Azure Data Factory yeniyseniz okuyun [Azure Data Factory'ye giriş](introduction.md) ve [öğretici: verileri](tutorial-transform-data-spark-powershell.md) bu makaleyi okumadan önce. 

## <a name="syntax"></a>Sözdizimi

```json
{
    "name": "Pig Activity",
    "description": "description",
    "type": "HDInsightPig",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "scriptLinkedService": {
            "referenceName": "MyAzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "scriptPath": "MyAzureStorage\\PigScripts\\MyPigSript.pig",
        "getDebugInfo": "Failure",
        "arguments": [
            "SampleHadoopJobArgument1"
        ],
        "defines": {
            "param1": "param1Value"
        }
    }   
}
```
## <a name="syntax-details"></a>Sözdizimi ayrıntıları

| Özellik            | Açıklama                              | Gerekli |
| ------------------- | ---------------------------------------- | -------- |
| ad                | Etkinlik adı                     | Evet      |
| açıklama         | Etkinlik hangi amaçla kullanıldığına açıklayan metin | Hayır       |
| type                | Hive etkinliği için etkinlik türü HDinsightPig değil. | Evet      |
| linkedServiceName   | Bir Hdınsight kümesine başvuru veri fabrikasında bağlı hizmet olarak kayıtlı. Bu bağlantılı hizmeti hakkında bilgi edinmek için [işlem bağlı Hizmetleri](compute-linked-services.md) makalesi. | Evet      |
| scriptLinkedService | Bir Azure depolama bağlı hizmeti başvuru yürütülecek Pig betiği depolamak için kullanılır. Bu bağlı hizmetin belirtmezseniz, Azure depolama bağlı Hdınsight bağlı hizmeti tanımlanan hizmeti kullanılır. | Hayır       |
| scriptPath          | ScriptLinkedService tarafından başvurulan Azure storage'da depolanan komut dosyası yolunu girin. Dosya adı büyük/küçük harf duyarlıdır. | Hayır       |
| Getdebugınfo        | Günlük dosyaları için Azure Storage zaman kopyalanır belirtir Hdınsight kümesi tarafından kullanılan (veya) scriptLinkedService tarafından belirtilen. İzin verilen değerler: None, her zaman veya hata. Varsayılan değer: yok. | Hayır       |
| bağımsız değişkenler           | Bir Hadoop işi için bağımsız değişkenleri dizisini belirtir. Bağımsız değişkenler, her görevin komut satırı bağımsız değişkenleri olarak geçirilir. | Hayır       |
| tanımlar             | Parametreler, Pig komut dosyası içinde başvurmak için anahtar/değer çiftleri olarak belirtin. | Hayır       |

## <a name="next-steps"></a>Sonraki adımlar
Diğer yollarla verileri dönüştürmek açıklanmaktadır aşağıdaki makalelere bakın: 

* [U-SQL etkinliği](transform-data-using-data-lake-analytics.md)
* [Hive etkinliği](transform-data-using-hadoop-hive.md)
* [MapReduce etkinliği](transform-data-using-hadoop-map-reduce.md)
* [Hadoop akış etkinliği](transform-data-using-hadoop-streaming.md)
* [Spark etkinliği](transform-data-using-spark.md)
* [.NET özel etkinliği](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning toplu iş yürütme etkinliği](transform-data-using-machine-learning.md)
* [Saklı yordam etkinliği](transform-data-using-stored-procedure.md)
