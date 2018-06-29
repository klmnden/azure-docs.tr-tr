---
title: Hadoop akış etkinliği Azure Data Factory kullanarak veri dönüştürme | Microsoft Docs
description: Hadoop akış etkinliği Azure Data Factory'de bir Hadoop kümesinde çalışan Hadoop akış programlar verileri dönüştürmek için nasıl kullanılacağını açıklar.
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
ms.openlocfilehash: 4c2bf83fec3d8f961a84523365e4a98fe3bf7603
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37052376"
---
# <a name="transform-data-using-hadoop-streaming-activity-in-azure-data-factory"></a>Hadoop akış etkinliği Azure Data Factory kullanarak veri dönüştürme
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-hadoop-streaming-activity.md)
> * [Geçerli sürüm](transform-data-using-hadoop-streaming.md)

Hdınsight akış etkinliğinde Data Factory [ardışık düzen](concepts-pipelines-activities.md) üzerinde Hadoop akış programları yürütür [kendi](compute-linked-services.md#azure-hdinsight-linked-service) veya [isteğe bağlı](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Hdınsight kümesi. Bu makalede derlemeler [veri dönüştürme etkinlikleri](transform-data.md) makalesi, veri dönüştürme ve desteklenen dönüştürme etkinliklerinin genel bir bakış sunar.

Azure Data Factory yeniyseniz okuyun [Azure Data Factory'ye giriş](introduction.md) ve [öğretici: verileri](tutorial-transform-data-spark-powershell.md) bu makaleyi okumadan önce. 

## <a name="json-sample"></a>JSON örneği
```json
{
    "name": "Streaming Activity",
    "description": "Description",
    "type": "HDInsightStreaming",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "mapper": "MyMapper.exe",
        "reducer": "MyReducer.exe",
        "combiner": "MyCombiner.exe",
        "fileLinkedService": {
            "referenceName": "MyAzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "filePaths": [
            "<containername>/example/apps/MyMapper.exe",
            "<containername>/example/apps/MyReducer.exe",
            "<containername>/example/apps/MyCombiner.exe"
        ],
        "input": "wasb://<containername>@<accountname>.blob.core.windows.net/example/input/MapperInput.txt",
        "output": "wasb://<containername>@<accountname>.blob.core.windows.net/example/output/ReducerOutput.txt",
        "commandEnvironment": [
            "CmdEnvVarName=CmdEnvVarValue"
        ],
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

| Özellik          | Açıklama                              | Gerekli |
| ----------------- | ---------------------------------------- | -------- |
| ad              | Etkinlik adı                     | Evet      |
| açıklama       | Etkinlik hangi amaçla kullanıldığına açıklayan metin | Hayır       |
| type              | Hadoop akış etkinliği için etkinlik türü HDInsightStreaming değil. | Evet      |
| linkedServiceName | Bir Hdınsight kümesine başvuru veri fabrikasında bağlı hizmet olarak kayıtlı. Bu bağlantılı hizmeti hakkında bilgi edinmek için [işlem bağlı Hizmetleri](compute-linked-services.md) makalesi. | Evet      |
| Eşleyici            | Yürütülebilir Eşleyici adını belirtir. | Evet      |
| reducer           | Yürütülebilir reducer adını belirtir. | Evet      |
| birleştirici          | Yürütülebilir Birleştirici adını belirtir. | Hayır       |
| fileLinkedService | Bir Azure depolama bağlı hizmeti başvuru Eşleyici, birleştirici ve Reducer programların çalıştırılmasına depolamak için kullanılır. Bu bağlı hizmetin belirtmezseniz, Azure depolama bağlı Hdınsight bağlı hizmeti tanımlanan hizmeti kullanılır. | Hayır       |
| filePath          | Eşleyici, birleştirici, bir dizi yolu sağlar ve Azure depolama alanında depolanan Reducer programları fileLinkedService tarafından başvurulan. Bu yol büyük/küçük harfe duyarlıdır. | Evet      |
| input             | Giriş dosyası WASB yolunu Eşleştiricisi belirtir. | Evet      |
| çıkış            | Çıktı dosyası WASB yolunu için Reducer belirtir. | Evet      |
| Getdebugınfo      | Günlük dosyaları için Azure Storage zaman kopyalanır belirtir Hdınsight kümesi tarafından kullanılan (veya) scriptLinkedService tarafından belirtilen. İzin verilen değerler: None, her zaman veya hata. Varsayılan değer: yok. | Hayır       |
| bağımsız değişkenler         | Bir Hadoop işi için bağımsız değişkenleri dizisini belirtir. Bağımsız değişkenler, her görevin komut satırı bağımsız değişkenleri olarak geçirilir. | Hayır       |
| tanımlar           | Parametreler, Hive betiğini içinde başvurmak için anahtar/değer çiftleri olarak belirtin. | Hayır       | 

## <a name="next-steps"></a>Sonraki adımlar
Diğer yollarla verileri dönüştürmek açıklanmaktadır aşağıdaki makalelere bakın: 

* [U-SQL etkinliği](transform-data-using-data-lake-analytics.md)
* [Hive etkinliği](transform-data-using-hadoop-hive.md)
* [Pig etkinliği](transform-data-using-hadoop-pig.md)
* [MapReduce etkinliği](transform-data-using-hadoop-map-reduce.md)
* [Spark etkinliği](transform-data-using-spark.md)
* [.NET özel etkinliği](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning toplu iş yürütme etkinliği](transform-data-using-machine-learning.md)
* [Saklı yordam etkinliği](transform-data-using-stored-procedure.md)
