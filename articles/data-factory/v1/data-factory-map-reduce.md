---
title: Azure Data factory'den MapReduce programını çağırma
description: Bir Azure HDInsight kümesinde bir Azure data factory'deki MapReduce programlarını çalıştırılarak verileri işlemek nasıl öğrenin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: c34db93f-570a-44f1-a7d6-00390f4dc0fa
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 715c595f7a8757842ddf10de1c5d5c0a905e9d53
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60824227"
---
# <a name="invoke-mapreduce-programs-from-data-factory"></a>Data factory'den MapReduce programlarını çağırma
> [!div class="op_single_selector" title1="Dönüştürme etkinlikleri"]
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
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [MapReduce etkinliği, Data Factory kullanarak verileri dönüştürme](../transform-data-using-hadoop-map-reduce.md).


HDInsight MapReduce etkinliği bir Data factory'de [işlem hattı](data-factory-create-pipelines.md) MapReduce programlarını üzerinde çalıştırır [kendi](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) veya [üzerine](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux tabanlı HDInsight kümesi. Bu makalede yapılar [veri dönüştürme etkinlikleri](data-factory-data-transformation-activities.md) makalesi, veri dönüştürme ve desteklenen dönüştürme etkinliklerinin genel bir bakış sunar.

> [!NOTE] 
> Azure Data Factory kullanmaya yeni başladıysanız, okumak [Azure Data Factory'ye giriş](data-factory-introduction.md) ve öğretici uygulayın: [İlk veri işlem hattı oluşturma](data-factory-build-your-first-pipeline.md) bu makaleyi okuduktan önce.  

## <a name="introduction"></a>Giriş
Bir Azure data factory'de bir işlem hattı, bağlantılı depolama Hizmetleri'ndeki veri bağlı işlem hizmetlerini kullanarak işler. Bu etkinliklerin nerede her etkinlik bir özel işleme işlemi gerçekleştirir dizisi içerir. Bu makalede, HDInsight MapReduce etkinliği kullanmayı açıklar.

Bkz: [Pig](data-factory-pig-activity.md) ve [Hive](data-factory-hive-activity.md) Pig/Hive çalıştırma hakkında ayrıntılı bilgi için bir Windows/Linux tabanlı HDInsight komut bir işlem hattından HDInsight Pig ve Hive etkinlikleri kullanarak küme. 

## <a name="json-for-hdinsight-mapreduce-activity"></a>HDInsight MapReduce etkinliği için JSON
HDInsight etkinliği JSON tanımında: 

1. Ayarlama **türü** , **etkinlik** için **HDInsight**.
2. İçin sınıf adını **className** özelliği.
3. Dosya adı dahil olmak üzere JAR dosyasını yolunu belirtin **jarFilePath** özelliği.
4. Belirtmek için JAR dosyasını içeren Azure Blob Depolama'ya başvuran bağlı hizmetin **jarLinkedService** özelliği.   
5. MapReduce programı için herhangi bir bağımsız değişken belirtin **bağımsız değişkenleri** bölümü. Çalışma zamanında, gördüğünüz bazı ek bağımsız değişkenler (örneğin: mapreduce.job.tags) MapReduce çerçeveden. MapReduce bağımsız değişkenleriyle değişkenleriniz ayırt etmek için hem seçeneği hem de değer bağımsız değişken olarak aşağıdaki örnekte gösterildiği gibi kullanmayı düşünün (- s, giriş,--çıktısı, vb. değerlerine göre hemen ardından seçenekleri olan).

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
   HDInsight MapReduce etkinliği bir HDInsight kümesi üzerinde bir MapReduce jar dosyasını çalıştırmak için kullanabilirsiniz. Aşağıdaki örnek JSON tanımında bir işlem hattı, bir Mahout JAR dosyasını çalıştırmak için HDInsight faaliyet yapılandırılır.

## <a name="sample-on-github"></a>Github'daki örnek
HDInsight MapReduce etkinliği kullanmaya yönelik bir örnek indirebilirsiniz: [Data Factory örnekleri github'da](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON/MapReduce_Activity_Sample).  

## <a name="running-the-word-count-program"></a>Sözcük sayısını programı çalıştırma
Bu örnekteki işlem hattı sözcük sayımı Map/Reduce program, Azure HDInsight kümesinde çalışır.   

### <a name="linked-services"></a>Bağlı Hizmetler
İlk olarak Azure HDInsight kümesi için Azure data factory tarafından kullanılan Azure depolama bağlamak için bağlı hizmet oluşturun. Kopyala/yapıştır aşağıdaki kod, değiştirilecek unutmadığınızdan **hesap adı** ve **hesap anahtarı** adını ve anahtarını Azure depolama ile. 

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

#### <a name="azure-hdinsight-linked-service"></a>Azure HDInsight bağlı hizmeti
Ardından, Azure HDInsight kümenizi Azure veri fabrikasına bağlamak için bağlı hizmet oluşturun. Kopyala/yapıştır aşağıdaki kod, yerini **HDInsight küme adı** HDInsight kümesi ve kullanıcı adı ve parola değerleri değiştirme ada sahip.   

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
Bu örnekteki işlem hattı hiç giriş almaz. HDInsight MapReduce etkinliği için bir çıktı veri kümesi belirt Bu veri kümesi yalnızca bir işlem hattı zamanlama sürücü için gereken işlevsiz veri kümesidir.  

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
Bu örnekteki işlem hattı, türü yalnızca bir etkinlik içerir: HDInsightMapReduce. Json'da önemli özelliklerinden bazıları şunlardır: 

| Özellik | Notlar |
|:--- |:--- |
| type |Türü ayarlanmalıdır **HDInsightMapReduce**. |
| className |Sınıf adı: **wordcount** |
| jarFilePath |Sınıfını içeren jar dosyası yolu. Kopyala/yapıştır aşağıdaki kod, kümenin adını değiştirmeyi unutmayın. |
| jarLinkedService |Jar dosyasını içeren azure depolama bağlı hizmeti. Bu bağlı hizmeti, HDInsight kümesi ile ilişkili depolama ifade eder. |
| arguments |Wordcount program iki bağımsız değişkeni, girdi ve çıktı alır. Giriş dosyası davinci.txt dosyasıdır. |
| frequency/interval |Bu özelliklerin değerlerini, çıktı veri kümesi eşleştirin. |
| linkedServiceName |daha önce oluşturmuştunuz HDInsight bağlı hizmetini ifade eder. |

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

[developer-reference]: https://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: https://go.microsoft.com/fwlink/?LinkId=517456


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[adfgetstartedmonitoring]:data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#monitor-pipelines 

[Developer Reference]: https://go.microsoft.com/fwlink/?LinkId=516908
[Azure Portal]: https://portal.azure.com

## <a name="see-also"></a>Ayrıca Bkz.
* [Hive etkinliği](data-factory-hive-activity.md)
* [Pig etkinliği](data-factory-pig-activity.md)
* [Hadoop akış etkinliğinde](data-factory-hadoop-streaming-activity.md)
* [Spark programlarını çağırma](data-factory-spark.md)
* [R betiklerini çağırma](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

