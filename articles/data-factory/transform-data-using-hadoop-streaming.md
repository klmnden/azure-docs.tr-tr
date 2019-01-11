---
title: Hadoop akış etkinliğinde, Azure Data Factory kullanarak verileri dönüştürme | Microsoft Docs
description: Hadoop akış etkinliğinde Azure Data Factory'de bir Hadoop kümesinde Hadoop akış programları çalıştırarak verileri dönüştürme için nasıl kullanılacağını açıklar.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/16/2018
ms.author: douglasl
ms.openlocfilehash: b498e09e53f8b0844470bf3948a664d8ad4337b7
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54022236"
---
# <a name="transform-data-using-hadoop-streaming-activity-in-azure-data-factory"></a>Hadoop akış etkinliğinde, Azure Data Factory kullanarak verileri dönüştürme
> [!div class="op_single_selector" title1="Kullanmakta olduğunuz Data Factory servisinin sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-hadoop-streaming-activity.md)
> * [Geçerli sürüm](transform-data-using-hadoop-streaming.md)

HDInsight akış etkinliği bir Data factory'de [işlem hattı](concepts-pipelines-activities.md) üzerinde Hadoop akış programları çalıştırır [kendi](compute-linked-services.md#azure-hdinsight-linked-service) veya [üzerine](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) HDInsight kümesi. Bu makalede yapılar [veri dönüştürme etkinlikleri](transform-data.md) makalesi, veri dönüştürme ve desteklenen dönüştürme etkinliklerinin genel bir bakış sunar.

Azure Data Factory kullanmaya yeni başladıysanız, okumak [Azure Data Factory'ye giriş](introduction.md) yapıp [öğretici: verileri dönüştürme](tutorial-transform-data-spark-powershell.md) bu makaleyi okuduktan önce. 

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

## <a name="syntax-details"></a>Söz dizimi ayrıntıları

| Özellik          | Açıklama                              | Gereklidir |
| ----------------- | ---------------------------------------- | -------- |
| ad              | Etkinliğin adı                     | Evet      |
| açıklama       | Etkinliğin ne için kullanıldığını açıklayan metin | Hayır       |
| type              | Hadoop akış etkinliğinde'için Hdınsightstreaming etkinlik türüdür | Evet      |
| linkedServiceName | Data Factory öğesinde bağlantılı hizmet olarak HDInsight kümesine başvuru kayıtlı. Bu bağlı hizmeti hakkında bilgi edinmek için [işlem bağlı Hizmetleri](compute-linked-services.md) makalesi. | Evet      |
| Eşleyici            | Yürütülebilir Eşleyici adını belirtir | Evet      |
| Azaltıcı           | Yürütülebilir Azaltıcı adını belirtir | Evet      |
| birleştirici          | Birleştirici yürütülebilir adını belirtir | Hayır       |
| fileLinkedService | Bir Azure depolama bağlı hizmeti başvuru Eşleyici birleştirici ve azaltıcı programların depolamak için kullanılır. Bu bağlı hizmeti belirtmezseniz, Azure depolama bağlı HDInsight bağlı hizmette tanımlanan hizmeti kullanılır. | Hayır       |
| dosya yolu          | Yol dizisi Eşleştiricisi için Birleştirici, sağlamak ve Azure Depolama'da depolanan Azaltıcı programlar fileLinkedService tarafından başvurulan. Bu yol büyük/küçük harfe duyarlıdır. | Evet      |
| input             | Giriş dosyası WASB yolu Eşleştiricisi belirtir. | Evet      |
| çıkış            | Çıkış dosyası WASB yolu için Azaltıcı belirtir. | Evet      |
| Getdebugınfo      | Günlük dosyaları Azure depolama için ne zaman kopyalanır belirtir HDInsight küme tarafından kullanılan (veya) scriptLinkedService tarafından belirtilen. İzin verilen değerler: None, her zaman veya hata. Varsayılan değer: Yok. | Hayır       |
| bağımsız değişkenler         | Hadoop işi için bağımsız değişkenleri dizisini belirtir. Bağımsız değişkenleri, her görev için komut satırı bağımsız değişkenleri geçirilir. | Hayır       |
| tanımlar           | Parametreler içinde Hive betiğine başvurmak için anahtar/değer çiftleri belirtin. | Hayır       | 

## <a name="next-steps"></a>Sonraki adımlar
Anlatan farklı yollarla verileri dönüştürmek aşağıdaki makalelere bakın: 

* [U-SQL etkinliği](transform-data-using-data-lake-analytics.md)
* [Hive etkinliği](transform-data-using-hadoop-hive.md)
* [Pig etkinliği](transform-data-using-hadoop-pig.md)
* [MapReduce etkinliği](transform-data-using-hadoop-map-reduce.md)
* [Spark etkinliği](transform-data-using-spark.md)
* [.NET özel etkinliği](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning Batch yürütme etkinliği](transform-data-using-machine-learning.md)
* [Saklı yordam etkinliği](transform-data-using-stored-procedure.md)
