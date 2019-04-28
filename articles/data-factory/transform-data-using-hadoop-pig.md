---
title: Azure Data Factory'de Hadoop Pig etkinliği kullanarak verileri dönüştürme | Microsoft Docs
description: Pig etkinliği Azure data factory'de bir üzerinde-istek/bilgisayarınızı kendi HDInsight kümesinde Pig betikleri çalıştırmak için nasıl kullanacağınızı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/16/2018
author: nabhishek
ms.author: abnarain
manager: craigg
ms.openlocfilehash: 914bc37552a80886df16ed69fba4e31b3f22ac22
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61399531"
---
# <a name="transform-data-using-hadoop-pig-activity-in-azure-data-factory"></a>Azure Data Factory'de Hadoop Pig etkinliği kullanarak verileri dönüştürme
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-pig-activity.md)
> * [Geçerli sürüm](transform-data-using-hadoop-pig.md)

Data Factory, HDInsight Pig etkinliği [işlem hattı](concepts-pipelines-activities.md) üzerinde Pig sorguları yürüten [kendi](compute-linked-services.md#azure-hdinsight-linked-service) veya [üzerine](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) HDInsight kümesi. Bu makalede yapılar [veri dönüştürme etkinlikleri](transform-data.md) makalesi, veri dönüştürme ve desteklenen dönüştürme etkinliklerinin genel bir bakış sunar.

Azure Data Factory kullanmaya yeni başladıysanız, okumak [Azure Data Factory'ye giriş](introduction.md) yapıp [öğretici: verileri dönüştürme](tutorial-transform-data-spark-powershell.md) bu makaleyi okuduktan önce. 

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
## <a name="syntax-details"></a>Söz dizimi ayrıntıları

| Özellik            | Açıklama                              | Gerekli |
| ------------------- | ---------------------------------------- | -------- |
| ad                | Etkinliğin adı                     | Evet      |
| açıklama         | Etkinliğin ne için kullanıldığını açıklayan metin | Hayır       |
| type                | Hive etkinliği için etkinlik türü HDinsightPig olduğu. | Evet      |
| linkedServiceName   | Data Factory öğesinde bağlantılı hizmet olarak HDInsight kümesine başvuru kayıtlı. Bu bağlı hizmeti hakkında bilgi edinmek için [işlem bağlı Hizmetleri](compute-linked-services.md) makalesi. | Evet      |
| scriptLinkedService | Bir Azure depolama bağlı hizmeti başvuru yürütülecek Pig betiği depolamak için kullanılır. Bu bağlı hizmeti belirtmezseniz, Azure depolama bağlı HDInsight bağlı hizmette tanımlanan hizmeti kullanılır. | Hayır       |
| ScriptPath          | ScriptLinkedService tarafından başvurulan Azure storage'da depolanan betik dosyasının yolunu belirtin. Dosya adı büyük/küçük harfe duyarlıdır. | Hayır       |
| getDebugInfo        | Günlük dosyaları Azure depolama için ne zaman kopyalanır belirtir HDInsight küme tarafından kullanılan (veya) scriptLinkedService tarafından belirtilen. İzin verilen değerler: None, her zaman veya hata. Varsayılan değer: Yok. | Hayır       |
| bağımsız değişkenler           | Hadoop işi için bağımsız değişkenleri dizisini belirtir. Bağımsız değişkenleri, her görev için komut satırı bağımsız değişkenleri geçirilir. | Hayır       |
| tanımlar             | Parametreler, Pig betiği içinde başvurmak için anahtar/değer çiftleri belirtin. | Hayır       |

## <a name="next-steps"></a>Sonraki adımlar
Anlatan farklı yollarla verileri dönüştürmek aşağıdaki makalelere bakın: 

* [U-SQL etkinliği](transform-data-using-data-lake-analytics.md)
* [Hive etkinliği](transform-data-using-hadoop-hive.md)
* [MapReduce etkinliği](transform-data-using-hadoop-map-reduce.md)
* [Hadoop akış etkinliğinde](transform-data-using-hadoop-streaming.md)
* [Spark etkinliği](transform-data-using-spark.md)
* [.NET özel etkinliği](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning Batch yürütme etkinliği](transform-data-using-machine-learning.md)
* [Saklı yordam etkinliği](transform-data-using-stored-procedure.md)
