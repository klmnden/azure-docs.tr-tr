---
title: "Hadoop MapReduce etkinliği Azure Data Factory kullanarak veri dönüştürme | Microsoft Docs"
description: "Hadoop MapReduce programlar bir Azure Hdınsight kümesinde bir Azure veri fabrikası'ndan çalıştırılarak verileri işlemek öğrenin."
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
ms.openlocfilehash: c1fbb6864629874ef116cdf81d48df4a9ed5af1f
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="transform-data-using-hadoop-mapreduce-activity-in-azure-data-factory"></a>Hadoop MapReduce etkinliği Azure Data Factory kullanarak veri dönüştürme
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-map-reduce.md)
> * [Sürüm 2 - Önizleme](transform-data-using-hadoop-map-reduce.md)


Veri Fabrikası'nda Hdınsight MapReduce etkinliği [ardışık düzen](concepts-pipelines-activities.md) üzerinde MapReduce program çağırır [kendi](compute-linked-services.md#azure-hdinsight-linked-service) veya [isteğe bağlı](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Hdınsight kümesi. Bu makalede derlemeler [veri dönüştürme etkinlikleri](transform-data.md) makalesi, veri dönüştürme ve desteklenen dönüştürme etkinliklerinin genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 MapReduce etkinliği](v1/data-factory-map-reduce.md).


Azure Data Factory yeniyseniz okuyun [Azure Data Factory'ye giriş](introduction.md) ve öğretici: [öğretici: verileri](tutorial-transform-data-spark-powershell.md) bu makaleyi okumadan önce. 

Bkz: [Pig](transform-data-using-hadoop-pig.md) ve [Hive](transform-data-using-hadoop-hive.md) Pig/Hive çalıştırma hakkında ayrıntılı bilgi için komut dosyalarını bir Hdınsight üzerinde bir ardışık düzen tarafından Hdınsight Pig ve Hive etkinlikleri kullanarak küme. 

## <a name="syntax"></a>Sözdizimi

```json
{
    "name": "Map Reduce Activity",
    "description": "Description",
    "type": "HDInsightMapReduce",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "className": "org.myorg.SampleClass",
        "jarLinkedService": {
            "referenceName": "MyAzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "jarFilePath": "MyAzureStorage/jars/sample.jar",
        "getDebugInfo": "Failure",
        "arguments": [
          "-SampleHadoopJobArgument1"
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
| type              | MapReduce etkinliği için etkinlik türü HDinsightMapReduce değil. | Evet      |
| linkedServiceName | Bir Hdınsight kümesine başvuru veri fabrikasında bağlı hizmet olarak kayıtlı. Bu bağlantılı hizmeti hakkında bilgi edinmek için [işlem bağlı Hizmetleri](compute-linked-services.md) makalesi. | Evet      |
| className         | Yürütülecek sınıfın adı         | Evet      |
| jarLinkedService  | Bir Azure depolama bağlı hizmeti başvuru Jar dosyalarını depolamak için kullanılır. Bu bağlı hizmetin belirtmezseniz, Azure depolama bağlı Hdınsight bağlı hizmeti tanımlanan hizmeti kullanılır. | Hayır       |
| jarFilePath       | JarLinkedService tarafından başvurulan Azure storage'da depolanan Jar dosyalarını yolunu girin. Dosya adı büyük/küçük harf duyarlıdır. | Evet      |
| jarlibs           | Azure depolama jarLinkedService içinde tanımlı depolanan iş tarafından başvurulan Jar kitaplığı dosyalarının yolunu dizisi dize. Dosya adı büyük/küçük harf duyarlıdır. | Hayır       |
| Getdebugınfo      | Günlük dosyaları için Azure Storage zaman kopyalanır belirtir Hdınsight kümesi tarafından kullanılan (veya) jarLinkedService tarafından belirtilen. İzin verilen değerler: None, her zaman veya hata. Varsayılan değer: yok. | Hayır       |
| Bağımsız değişkenler         | Bir Hadoop işi için bağımsız değişkenleri dizisini belirtir. Bağımsız değişkenler, her görevin komut satırı bağımsız değişkenleri olarak geçirilir. | Hayır       |
| tanımlar           | Parametreler, Hive betiğini içinde başvurmak için anahtar/değer çiftleri olarak belirtin. | Hayır       |



## <a name="example"></a>Örnek
Hdınsight MapReduce etkinliği Hdınsight kümesinde herhangi MapReduce jar dosyasını çalıştırmak için kullanabilirsiniz. Aşağıdaki örnek JSON tanımında bir ardışık düzen, Hdınsight etkinliği Mahout JAR dosyasını çalıştırmak için yapılandırılır.

```json   
{
    "name": "MapReduce Activity for Mahout",
    "description": "Custom MapReduce to generate Mahout result",
    "type": "HDInsightMapReduce",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
        "jarLinkedService": {
            "referenceName": "MyStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
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
    }
}
```
MapReduce program için herhangi bir bağımsız değişken belirtebilirsiniz **bağımsız değişkenleri** bölümü. Çalışma zamanında gördüğünüz birkaç ek bağımsız değişkenler (örneğin: mapreduce.job.tags) MapReduce çerçeveden. MapReduce bağımsız değişkenleriyle ayırt etmek için seçeneği ve değer bağımsız değişken olarak aşağıdaki örnekte gösterildiği gibi kullanmayı düşünün (- s,--giriş,--çıkış vb. göre bunların değerleri hemen ardından Seçenekler belirtilmiştir).

## <a name="next-steps"></a>Sonraki adımlar
Diğer yollarla verileri dönüştürmek açıklanmaktadır aşağıdaki makalelere bakın: 

* [U-SQL etkinliği](transform-data-using-data-lake-analytics.md)
* [Hive etkinliği](transform-data-using-hadoop-hive.md)
* [Pig etkinliği](transform-data-using-hadoop-pig.md)
* [Hadoop akış etkinliği](transform-data-using-hadoop-streaming.md)
* [Spark etkinliği](transform-data-using-spark.md)
* [.NET özel etkinliği](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning toplu iş yürütme etkinliği](transform-data-using-machine-learning.md)
* [Saklı yordam etkinliği](transform-data-using-stored-procedure.md)
