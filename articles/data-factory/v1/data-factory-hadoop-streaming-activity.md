---
title: Hadoop akış etkinliğinde - Azure'ı kullanarak verileri dönüştürme | Microsoft Docs
description: Hadoop akış etkinliğinde ' Azure data factory'de bir üzerinde-istek/bilgisayarınızı kendi HDInsight kümesi üzerinde Hadoop akış programları çalıştırarak verileri dönüştürme için nasıl kullanacağınızı öğrenin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 4c3ff8f2-2c00-434e-a416-06dfca2c41ec
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: dd00c0a2998009ce6c39ca19abb25a2548682cee
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60486354"
---
# <a name="transform-data-using-hadoop-streaming-activity-in-azure-data-factory"></a>Hadoop akış etkinliğinde, Azure Data Factory kullanarak verileri dönüştürme
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive etkinliği](data-factory-hive-activity.md) 
> * [Pig etkinliği](data-factory-pig-activity.md)
> * [MapReduce etkinliği](data-factory-map-reduce.md)
> * [Hadoop akış etkinliğinde](data-factory-hadoop-streaming-activity.md)
> * [Spark etkinliği](data-factory-spark.md)
> * [Machine Learning Batch Yürütme Etkinliği](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning Kaynak Güncelleştirme Etkinliği](data-factory-azure-ml-update-resource-activity.md)
> * [Saklı Yordam Etkinliği](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL Etkinliği](data-factory-usql-activity.md)
> * [.NET özel etkinliği](data-factory-use-custom-activities.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [Hadoop akış etkinliğinde Data Factory kullanarak verileri dönüştürme](../transform-data-using-hadoop-streaming.md).


HDInsightStreamingActivity etkinlik kullanabileceğiniz bir Hadoop akış işi bir Azure Data Factory işlem hattından çağırma. Aşağıdaki JSON kod parçacığında, bir işlem hattı JSON dosyasında HDInsightStreamingActivity kullanmaya ilişkin sözdizimini gösterir. 

HDInsight akış etkinliği bir Data factory'de [işlem hattı](data-factory-create-pipelines.md) üzerinde Hadoop akış programları çalıştırır [kendi](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) veya [üzerine](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux tabanlı HDInsight kümesi. Bu makalede yapılar [veri dönüştürme etkinlikleri](data-factory-data-transformation-activities.md) makalesi, veri dönüştürme ve desteklenen dönüştürme etkinliklerinin genel bir bakış sunar.

> [!NOTE] 
> Azure Data Factory kullanmaya yeni başladıysanız, okumak [Azure Data Factory'ye giriş](data-factory-introduction.md) ve öğretici uygulayın: [İlk veri işlem hattı oluşturma](data-factory-build-your-first-pipeline.md) bu makaleyi okuduktan önce. 

## <a name="json-sample"></a>JSON örneği
HDInsight kümesi, örnek programlar (wc.exe ve cat.exe) ve veri (davinci.txt) ile otomatik olarak doldurulur. HDInsight kümesi tarafından kullanılan kapsayıcının adını varsayılan olarak, küme adıdır. Örneğin, küme adınızı myhdicluster ise, ilişkili blob kapsayıcının adını myhdicluster olacaktır. 

```JSON
{
    "name": "HadoopStreamingPipeline",
    "properties": {
        "description": "Hadoop Streaming Demo",
        "activities": [
            {
                "type": "HDInsightStreaming",
                "typeProperties": {
                    "mapper": "cat.exe",
                    "reducer": "wc.exe",
                    "input": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                    "output": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                    "filePaths": [
                        "<nameofthecluster>/example/apps/wc.exe",
                        "<nameofthecluster>/example/apps/cat.exe"
                    ],
                    "fileLinkedService": "AzureStorageLinkedService",
                    "getDebugInfo": "Failure"
                },
                "outputs": [
                    {
                        "name": "StreamingOutputDataset"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "RunHadoopStreamingJob",
                "description": "Run a Hadoop streaming job",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2014-01-04T00:00:00Z",
        "end": "2014-01-05T00:00:00Z"
    }
}
```

Aşağıdaki noktalara dikkat edin:

1. Ayarlama **linkedServiceName** akış mapreduce işi çalıştırıldığı küme için HDInsight'ı işaret eden bağlı hizmetin adı.
2. Etkinlik türünü ayarlayın **Hdınsightstreaming**.
3. İçin **Eşleyici** özelliği Eşleyici yürütülebilir adını belirtin. Bu örnekte cat.exe yürütülebilir eşleyicisidir.
4. İçin **Azaltıcı** özelliği Azaltıcı yürütülebilir adını belirtin. Bu örnekte wc.exe yürütülebilir Azaltıcı ' dir.
5. İçin **giriş** türü özelliği, Eşleştiricisi (konum dahil) giriş dosyası belirtin. Örnekte: `wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt`: adfsample blob kapsayıcısını, örnek/data/Gutenberg klasördür ve davinci.txt blob.
6. İçin **çıkış** türü özelliği, çıktı dosyası (konum dahil) için Azaltıcı belirtin. Hadoop akışı tanımlı işlemin çıktısını, bu özellik için belirtilen konuma yazılır.
7. İçinde **filePaths** bölümünde, Eşleyici ve azaltıcı yürütülebilir dosyaları yollarını belirtin. Örnekte: "adfsample/example/apps/wc.exe" adfsample blob kapsayıcısını, örnek/uygulamaları klasördür ve wc.exe çalıştırılabilir.
8. İçin **fileLinkedService** özelliğini temsil eden filePaths bölümünde belirtilen dosyalar içeren bir Azure depolama Azure depolama bağlı hizmeti belirtin.
9. İçin **bağımsız değişkenleri** özelliği, iş akışında bağımsız değişkenleri belirtin.
10. **Getdebugınfo** özelliği isteğe bağlı bir öğedir. Başarısız olduğunda ayarlandığında, günlükleri yalnızca başarısız olduğunda indirilir. Her zaman olarak ayarlandığında, günlükleri yürütme durumu bağımsız olarak daima yüklenir.

> [!NOTE]
> Örnekte gösterildiği gibi bir çıktı veri kümesi için Hadoop akış etkinliğinde'için belirttiğiniz **çıkarır** özelliği. Bu veri kümesi yalnızca bir işlem hattı zamanlama sürücü için gereken işlevsiz veri kümesidir. Herhangi bir giriş veri kümesi için etkinliğin belirtmenize gerek olmayan **girişleri** özelliği.  
> 
> 

## <a name="example"></a>Örnek
Bu kılavuzdaki işlem hattı sözcük sayımı akış Map/Reduce program, Azure HDInsight kümesinde çalışır. 

### <a name="linked-services"></a>Bağlı hizmetler
#### <a name="azure-storage-linked-service"></a>Azure Storage bağlı hizmeti
İlk olarak Azure HDInsight kümesi için Azure data factory tarafından kullanılan Azure depolama bağlamak için bağlı hizmet oluşturun. Hesap adı ve hesap anahtarı adını ve anahtarını Azure depolama ile değiştirmek, kopyala/yapıştır aşağıdaki kod, unutmayın. 

```JSON
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>"
        }
    }
}
```

#### <a name="azure-hdinsight-linked-service"></a>Azure HDInsight bağlı hizmeti
Ardından, Azure HDInsight kümenizi Azure veri fabrikasına bağlamak için bağlı hizmet oluşturun. Kopyala/yapıştır aşağıdaki kod, HDInsight küme adı, HDInsight kümenizin adıyla değiştirin ve kullanıcı adı ve parola değerlerini değiştirin. 

```JSON
{
    "name": "HDInsightLinkedService",
    "properties": {
        "type": "HDInsight",
        "typeProperties": {
            "clusterUri": "https://<HDInsight cluster name>.azurehdinsight.net",
            "userName": "admin",
            "password": "**********",
            "linkedServiceName": "StorageLinkedService"
        }
    }
}
```

### <a name="datasets"></a>Veri kümeleri
#### <a name="output-dataset"></a>Çıktı veri kümesi
Bu örnekteki işlem hattı hiç giriş almaz. HDInsight akış etkinliği için bir çıktı veri kümesi belirt Bu veri kümesi yalnızca bir işlem hattı zamanlama sürücü için gereken işlevsiz veri kümesidir. 

```JSON
{
    "name": "StreamingOutputDataset",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "adftutorial/streamingdata/",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            },
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

### <a name="pipeline"></a>İşlem hattı
Bu örnekteki işlem hattı, türü yalnızca bir etkinlik içerir: **Hdınsightstreaming**. 

HDInsight kümesi, örnek programlar (wc.exe ve cat.exe) ve veri (davinci.txt) ile otomatik olarak doldurulur. HDInsight kümesi tarafından kullanılan kapsayıcının adını varsayılan olarak, küme adıdır. Örneğin, küme adınızı myhdicluster ise, ilişkili blob kapsayıcının adını myhdicluster olacaktır.  

```JSON
{
    "name": "HadoopStreamingPipeline",
    "properties": {
        "description": "Hadoop Streaming Demo",
        "activities": [
            {
                "type": "HDInsightStreaming",
                "typeProperties": {
                    "mapper": "cat.exe",
                    "reducer": "wc.exe",
                    "input": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                    "output": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                    "filePaths": [
                        "<blobcontainer>/example/apps/wc.exe",
                        "<blobcontainer>/example/apps/cat.exe"
                    ],
                    "fileLinkedService": "StorageLinkedService"
                },
                "outputs": [
                    {
                        "name": "StreamingOutputDataset"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "RunHadoopStreamingJob",
                "description": "Run a Hadoop streaming job",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-01-03T00:00:00Z",
        "end": "2017-01-04T00:00:00Z"
    }
}
```
## <a name="see-also"></a>Ayrıca Bkz.
* [Hive etkinliği](data-factory-hive-activity.md)
* [Pig etkinliği](data-factory-pig-activity.md)
* [MapReduce etkinliği](data-factory-map-reduce.md)
* [Spark programlarını çağırma](data-factory-spark.md)
* [R betiklerini çağırma](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

