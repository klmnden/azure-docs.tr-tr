---
title: Hadoop akış etkinliği - Azure kullanarak veri dönüştürme | Microsoft Docs
description: Hadoop akış etkinliği bir Azure data factory üzerinde-isteğe bağlı/bilgisayarınızı kendi Hdınsight kümesi üzerinde çalışan Hadoop akış programlar verileri dönüştürmek için nasıl kullanabileceğinizi öğrenin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 4c3ff8f2-2c00-434e-a416-06dfca2c41ec
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 3d5832f63a3ebe7583d18fcd863c8cc60b9b045d
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37048773"
---
# <a name="transform-data-using-hadoop-streaming-activity-in-azure-data-factory"></a>Hadoop akış etkinliği Azure Data Factory kullanarak veri dönüştürme
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive etkinliği](data-factory-hive-activity.md) 
> * [Pig etkinliği](data-factory-pig-activity.md)
> * [MapReduce etkinliği](data-factory-map-reduce.md)
> * [Hadoop akış etkinliği](data-factory-hadoop-streaming-activity.md)
> * [Spark etkinliği](data-factory-spark.md)
> * [Machine Learning Batch Yürütme Etkinliği](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning Kaynak Güncelleştirme Etkinliği](data-factory-azure-ml-update-resource-activity.md)
> * [Saklı Yordam Etkinliği](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL Etkinliği](data-factory-usql-activity.md)
> * [.NET özel etkinlik](data-factory-use-custom-activities.md)

> [!NOTE]
> Bu makale, veri fabrikası 1 sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [Hadoop akış etkinliğinde Data Factory kullanarak verileri](../transform-data-using-hadoop-streaming.md).


HDInsightStreamingActivity etkinlik kullanabileceğiniz bir Azure Data Factory işlem hattı Hadoop akış işten çağırma. Aşağıdaki JSON parçacığı HDInsightStreamingActivity bir ardışık düzen JSON dosyası kullanarak sözdizimi gösterilmektedir. 

Hdınsight akış etkinliğinde Data Factory [ardışık düzen](data-factory-create-pipelines.md) üzerinde Hadoop akış programları yürütür [kendi](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) veya [isteğe bağlı](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux tabanlı Hdınsight kümesi. Bu makalede derlemeler [veri dönüştürme etkinlikleri](data-factory-data-transformation-activities.md) makalesi, veri dönüştürme ve desteklenen dönüştürme etkinliklerinin genel bir bakış sunar.

> [!NOTE] 
> Azure Data Factory yeniyseniz okuyun [Azure Data Factory'ye giriş](data-factory-introduction.md) ve öğretici: [ilk veri hattınızı yapı](data-factory-build-your-first-pipeline.md) bu makaleyi okumadan önce. 

## <a name="json-sample"></a>JSON örneği
Hdınsight kümesi örnek programlar (wc.exe ve cat.exe) ve veri (davinci.txt) ile otomatik olarak doldurulur. Varsayılan olarak, Hdınsight kümesi tarafından kullanılan kapsayıcının adını küme adıdır. Örneğin, küme adınızı myhdicluster ise, ilişkili blob kapsayıcısının adı myhdicluster olacaktır. 

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

1. Ayarlama **linkedServiceName** mapreduce iş akışında çalıştırıldığı, Hdınsight işaret bağlı hizmetin adı küme.
2. Etkinlik türünü ayarlayın **HDInsightStreaming**.
3. İçin **Eşleyici** özelliği Eşleyici yürütülebilir adını belirtin. Örnekte, cat.exe yürütülebilir Eşleyici ' dir.
4. İçin **reducer** özelliği reducer yürütülebilir adını belirtin. Örnekte, wc.exe yürütülebilir reducer ' dir.
5. İçin **giriş** türü özelliği, Eşleştiricisi (konum dahil) giriş dosyası belirtin. Örnekte: "wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt": adfsample blob kapsayıcısı, veri/örnek/Gutenberg klasördür ve davinci.txt blob.
6. İçin **çıkış** türü özelliği, çıktı dosyası (konum dahil) için reducer belirtin. Hadoop akış işi çıkışı, bu özellik için belirtilen konuma yazılır.
7. İçinde **filePaths** bölümünde, Eşleyici ve reducer yürütülebilir dosyalar için yollarını belirtin. Örnekte: "adfsample/example/apps/wc.exe" adfsample blob kapsayıcısı, örnek/uygulamaları klasördür ve wc.exe çalıştırılabilir.
8. İçin **fileLinkedService** özelliği filePaths bölümünde belirtilen dosyaları içeren Azure depolama temsil eden Azure Storage bağlı hizmeti belirtin.
9. İçin **bağımsız değişkenleri** özelliği, iş akışında bağımsız değişkenleri belirtin.
10. **Getdebugınfo** özelliği isteğe bağlı bir öğedir. Hata için ayarlandığında, günlükleri yalnızca hatada indirilir. Her zaman olarak ayarlandığında, günlükleri yürütme durumu bağımsız olarak daima yüklenir.

> [!NOTE]
> Örnekte gösterildiği gibi bir çıktı veri kümesi için Hadoop akış etkinliği için belirlediğiniz **çıkarır** özelliği. Bu veri kümesi yalnızca ardışık düzen zamanlama sürücü için gereken bir kukla dataset ' dir. Hiçbir girdi veri kümesi için etkinliği belirtmek gerekmez **girişleri** özelliği.  
> 
> 

## <a name="example"></a>Örnek
Ardışık Düzen bu kılavuzda, Azure Hdınsight kümesinde sözcük sayımı akış Haritası/azaltın programı çalıştırır. 

### <a name="linked-services"></a>Bağlı hizmetler
#### <a name="azure-storage-linked-service"></a>Azure Storage bağlı hizmeti
İlk olarak, Azure data factory Azure Hdınsight küme tarafından kullanılan Azure Storage bağlamak için bağlı hizmet oluşturun. Hesap adı ve hesap anahtarı adı ve Azure depolama alanınızı anahtarı ile değiştirmek, kopyala/yapıştır aşağıdaki kod, unutmayın. 

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

#### <a name="azure-hdinsight-linked-service"></a>Azure Hdınsight bağlı hizmeti
Ardından, Azure Hdınsight kümenizi Azure data factory'ye bağlamak için bağlı hizmet oluşturun. Kopyala/yapıştır aşağıdaki kod, Hdınsight küme adı Hdınsight kümenizin adıyla değiştirin ve kullanıcı adı ve parola değerlerini değiştirin. 

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
Ardışık Düzen Bu örnekte, tüm girişleri almaz. Hdınsight akış etkinliği için bir çıkış veri kümesi belirtin. Bu veri kümesi yalnızca ardışık düzen zamanlama sürücü için gereken bir kukla dataset ' dir. 

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
Bu örnekte ardışık türünde yalnızca bir etkinlik vardır: **HDInsightStreaming**. 

Hdınsight kümesi örnek programlar (wc.exe ve cat.exe) ve veri (davinci.txt) ile otomatik olarak doldurulur. Varsayılan olarak, Hdınsight kümesi tarafından kullanılan kapsayıcının adını küme adıdır. Örneğin, küme adınızı myhdicluster ise, ilişkili blob kapsayıcısının adı myhdicluster olacaktır.  

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

