---
title: "Hadoop Hive etkinliği Azure Data Factory kullanarak veri dönüştürme | Microsoft Docs"
description: "Hive etkinliği bir Azure data factory'de bir üzerinde-isteğe bağlı/bilgisayarınızı kendi Hdınsight kümesinde Hive sorguları çalıştırmak için nasıl kullanabileceğinizi öğrenin."
services: data-factory
documentationcenter: 
author: shengcmsft
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: shengc
ms.openlocfilehash: 37a29d826a948788c5374ad2cc20b6a2040230ad
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="transform-data-using-hadoop-hive-activity-in-azure-data-factory"></a>Hadoop Hive etkinliği Azure Data Factory kullanarak veri dönüştürme
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-hive-activity.md)
> * [Sürüm 2 - Önizleme](transform-data-using-hadoop-hive.md)

Veri Fabrikası Hdınsight Hive etkinliğiyle [ardışık düzen](concepts-pipelines-activities.md) üzerinde Hive sorguları yürüten [kendi](compute-linked-services.md#azure-hdinsight-linked-service) veya [isteğe bağlı](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Hdınsight kümesi. Bu makalede derlemeler [veri dönüştürme etkinlikleri](transform-data.md) makalesi, veri dönüştürme ve desteklenen dönüştürme etkinliklerinin genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 Hive etkinliğinde](v1/data-factory-hive-activity.md).

Azure Data Factory yeniyseniz okuyun [Azure Data Factory'ye giriş](introduction.md) ve [öğretici: verileri](tutorial-transform-data-spark-powershell.md) bu makaleyi okumadan önce. 

## <a name="syntax"></a>Sözdizimi

```json
{
    "name": "Hive Activity",
    "description": "description",
    "type": "HDInsightHive",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "scriptLinkedService": {
            "referenceName": "MyAzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "scriptPath": "MyAzureStorage\\HiveScripts\\MyHiveSript.hql",
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
| type                | Hive etkinliği için Hdınsighthive etkinliği türüdür | Evet      |
| linkedServiceName   | Bir Hdınsight kümesine başvuru veri fabrikasında bağlı hizmet olarak kayıtlı. Bu bağlantılı hizmeti hakkında bilgi edinmek için [işlem bağlı Hizmetleri](compute-linked-services.md) makalesi. | Evet      |
| scriptLinkedService | Bir Azure depolama bağlı hizmeti başvuru yürütülecek Hive betiğini depolamak için kullanılır. Bu bağlı hizmetin belirtmezseniz, Azure depolama bağlı Hdınsight bağlı hizmeti tanımlanan hizmeti kullanılır. | Hayır       |
| scriptPath          | ScriptLinkedService tarafından başvurulan Azure storage'da depolanan komut dosyası yolunu girin. Dosya adı büyük/küçük harf duyarlıdır. | Evet      |
| Getdebugınfo        | Günlük dosyaları için Azure Storage zaman kopyalanır belirtir Hdınsight kümesi tarafından kullanılan (veya) scriptLinkedService tarafından belirtilen. İzin verilen değerler: None, her zaman veya hata. Varsayılan değer: yok. | Hayır       |
| Bağımsız değişkenler           | Bir Hadoop işi için bağımsız değişkenleri dizisini belirtir. Bağımsız değişkenler, her görevin komut satırı bağımsız değişkenleri olarak geçirilir. | Hayır       |
| tanımlar             | Parametreler, Hive betiğini içinde başvurmak için anahtar/değer çiftleri olarak belirtin. | Hayır       |

## <a name="next-steps"></a>Sonraki adımlar
Diğer yollarla verileri dönüştürmek açıklanmaktadır aşağıdaki makalelere bakın: 

* [U-SQL etkinliği](transform-data-using-data-lake-analytics.md)
* [Pig etkinliği](transform-data-using-hadoop-pig.md)
* [MapReduce activity](transform-data-using-hadoop-map-reduce.md)
* [Hadoop akış etkinliği](transform-data-using-hadoop-streaming.md)
* [Spark etkinliği](transform-data-using-spark.md)
* [.NET özel etkinliği](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning toplu iş yürütme etkinliği](transform-data-using-machine-learning.md)
* [Saklı yordam etkinliği](transform-data-using-stored-procedure.md)

