---
title: "Azure Data Factory MapReduce programdan çağırma"
description: "MapReduce programlar bir Azure Hdınsight kümesinde bir Azure veri fabrikası'ndan çalıştırılarak verileri işlemek öğrenin."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: c34db93f-570a-44f1-a7d6-00390f4dc0fa
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2017
ms.author: shlo
robots: noindex
ms.openlocfilehash: e5fd49c6b269b5f247440c2bc91680fc77fc296c
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="invoke-mapreduce-programs-from-data-factory"></a>Veri Fabrikası MapReduce programlardan çağırma
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
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [dönüştürme Data Factory sürüm 2 MapReduce etkinliği kullanarak verileri](../transform-data-using-hadoop-map-reduce.md).


Veri Fabrikası'nda Hdınsight MapReduce etkinliği [ardışık düzen](data-factory-create-pipelines.md) üzerinde MapReduce programları yürütür [kendi](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) veya [isteğe bağlı](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux tabanlı Hdınsight kümesi. Bu makalede derlemeler [veri dönüştürme etkinlikleri](data-factory-data-transformation-activities.md) makalesi, veri dönüştürme ve desteklenen dönüştürme etkinliklerinin genel bir bakış sunar.

> [!NOTE] 
> Azure Data Factory yeniyseniz okuyun [Azure Data Factory'ye giriş](data-factory-introduction.md) ve öğretici: [ilk veri hattınızı yapı](data-factory-build-your-first-pipeline.md) bu makaleyi okumadan önce.  

## <a name="introduction"></a>Giriş
Bir Azure data factory işlem hattı bağlantılı işlem hizmetlerini kullanarak bağlantılı depolama Hizmetleri'nde verilerini işler. Burada her etkinlik özel işleme işlemi gerçekleştiren etkinlikler dizisini içerir. Bu makalede, Hdınsight MapReduce etkinliği kullanmayı açıklar.

Bkz: [Pig](data-factory-pig-activity.md) ve [Hive](data-factory-hive-activity.md) Pig/Hive çalıştırma hakkında ayrıntılı bilgi için komut dosyaları Windows/Linux tabanlı Hdınsight üzerinde bir ardışık düzen tarafından Hdınsight Pig ve Hive etkinlikleri kullanarak küme. 

## <a name="json-for-hdinsight-mapreduce-activity"></a>Hdınsight MapReduce etkinliği için JSON
Hdınsight etkinliği JSON tanımında: 

1. Ayarlama **türü** , **etkinlik** için **Hdınsight**.
2. Sınıfı için adını belirtin **className** özelliği.
3. Dosya adı dahil olmak üzere JAR dosyasının yolunu belirtin **jarFilePath** özelliği.
4. Azure Blob Storage için JAR dosyasını içeren başvurduğu bağlantılı hizmet belirtin **jarLinkedService** özelliği.   
5. MapReduce program için herhangi bir bağımsız değişken belirtin **bağımsız değişkenleri** bölümü. Çalışma zamanında gördüğünüz birkaç ek bağımsız değişkenler (örneğin: mapreduce.job.tags) MapReduce çerçeveden. MapReduce bağımsız değişkenleriyle ayırt etmek için seçeneği ve değer bağımsız değişken olarak aşağıdaki örnekte gösterildiği gibi kullanmayı düşünün (- s,--giriş,--çıkış vb. göre bunların değerleri hemen ardından Seçenekler belirtilmiştir).

    ```JSON   
    {
        "name": "MahoutMapReduceSamplePipeline",
        "properties": {
            "description": "Sample Pipeline to Run a Mahout Custom Map Reduce Jar. This job calcuates an Item Similarity Matrix to determine the similarity between 2 items",
            "activities": [
                {
                    "type": "HDInsightMapReduce",
                    "typeProperties": {
                        "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
                        "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
                        "jarLinkedService": "StorageLinkedService",
                        "arguments": [
                            "-s",
                            "SIMILARITY_LOGLIKELIHOOD",
                            "--input",
                            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input",
                            "--output",
                            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/",
                            "--maxSimilaritiesPerItem",
                            "500",
                            "--tempDir",
                            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"
                        ]
                    },
                    "inputs": [
                        {
                            "name": "MahoutInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "MahoutOutput"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "MahoutActivity",
                    "description": "Custom Map Reduce to generate Mahout result",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2017-01-03T00:00:00Z",
            "end": "2017-01-04T00:00:00Z"
        }
    }
    ```
Hdınsight MapReduce etkinliği Hdınsight kümesinde herhangi MapReduce jar dosyasını çalıştırmak için kullanabilirsiniz. Aşağıdaki örnek JSON tanımında bir ardışık düzen, Hdınsight etkinliği Mahout JAR dosyasını çalıştırmak için yapılandırılır.

## <a name="sample-on-github"></a>Github'da örnek
Hdınsight MapReduce etkinliğinden kullanmak için bir örnek indirebilirsiniz: [veri fabrikası örneği github'daki](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON/MapReduce_Activity_Sample).  

## <a name="running-the-word-count-program"></a>Sözcük sayısını programı çalıştırma
Ardışık Düzen Bu örnekte, Azure Hdınsight kümesinde sözcük sayımı harita/azaltın programı çalıştırır.   

### <a name="linked-services"></a>Bağlı hizmetler
İlk olarak, Azure data factory Azure Hdınsight küme tarafından kullanılan Azure Storage bağlamak için bağlı hizmet oluşturun. Kopyala/yapıştır aşağıdaki kod, değiştirmek unutmadığınızdan **hesap adı** ve **hesap anahtarı** adı ile Azure depolama alanınızı anahtarı. 

#### <a name="azure-storage-linked-service"></a>Azure Storage bağlı hizmeti

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
Ardından, Azure Hdınsight kümenizi Azure data factory'ye bağlamak için bağlı hizmet oluşturun. Kopyala/yapıştır aşağıdaki kod, yerini **Hdınsight küme adı** Hdınsight kümesi ve kullanıcı adı ve parola değerleri değiştirme ada sahip.   

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
Ardışık Düzen Bu örnekte, tüm girişleri almaz. Hdınsight MapReduce etkinliği için bir çıkış veri kümesi belirtin. Bu veri kümesi yalnızca ardışık düzen zamanlama sürücü için gereken bir kukla dataset ' dir.  

```JSON
{
    "name": "MROutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "fileName": "WordCountOutput1.txt",
            "folderPath": "example/data/",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

### <a name="pipeline"></a>İşlem hattı
Bu örnekte ardışık türünde yalnızca bir etkinlik vardır: HDInsightMapReduce. JSON önemli özelliklerin bazıları şunlardır: 

| Özellik | Notlar |
|:--- |:--- |
| type |Türü ayarlamak **HDInsightMapReduce**. |
| className |Sınıf adıdır: **wordcount** |
| jarFilePath |Sınıf içeren jar dosyasının yolu. Kopyala/yapıştır aşağıdaki kod, kümenin adını değiştirmeyi unutmayın. |
| jarLinkedService |Jar dosyasını içeren azure depolama bağlı hizmeti. Bu bağlı hizmetin Hdınsight kümesi ile ilişkili depolama ifade eder. |
| Bağımsız değişkenler |Wordcount program iki bağımsız değişken, bir giriş ve çıkış alır. Giriş dosyası davinci.txt dosyasıdır. |
| frequency/interval |Bu özelliklerin değerlerini çıktı veri kümesi eşleşmesi. |
| linkedServiceName |daha önce oluşturmuştunuz Hdınsight bağlı hizmeti anlamına gelmektedir. |

```JSON
{
    "name": "MRSamplePipeline",
    "properties": {
        "description": "Sample Pipeline to Run the Word Count Program",
        "activities": [
            {
                "type": "HDInsightMapReduce",
                "typeProperties": {
                    "className": "wordcount",
                    "jarFilePath": "<HDInsight cluster name>/example/jars/hadoop-examples.jar",
                    "jarLinkedService": "StorageLinkedService",
                    "arguments": [
                        "/example/data/gutenberg/davinci.txt",
                        "/example/data/WordCountOutput1"
                    ]
                },
                "outputs": [
                    {
                        "name": "MROutput"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "MRActivity",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2014-01-03T00:00:00Z",
        "end": "2014-01-04T00:00:00Z"
    }
}
```

## <a name="run-spark-programs"></a>Spark programları çalıştırma
MapReduce etkinliğini kullanarak HDInsight Spark kümenizde Spark programları çalıştırabilirsiniz. Ayrıntılar için bkz. [Azure Data Factory’den Spark programlarını çağırma](data-factory-spark.md).  

[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[adfgetstartedmonitoring]:data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#monitor-pipelines 

[Developer Reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[Azure Portal]: http://portal.azure.com

## <a name="see-also"></a>Ayrıca Bkz.
* [Hive etkinliği](data-factory-hive-activity.md)
* [Pig etkinliği](data-factory-pig-activity.md)
* [Hadoop akış etkinliği](data-factory-hadoop-streaming-activity.md)
* [Spark programlarını çağırma](data-factory-spark.md)
* [R betiklerini çağırma](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

