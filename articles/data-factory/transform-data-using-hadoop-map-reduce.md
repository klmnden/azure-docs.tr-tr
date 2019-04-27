---
title: Hadoop MapReduce etkinliği Azure Data Factory kullanarak verileri dönüştürme | Microsoft Docs
description: Hadoop MapReduce programları bir Azure HDInsight kümesinde bir Azure data factory'deki çalıştırılarak verileri işlemek nasıl öğrenin.
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
ms.openlocfilehash: ccc194dd4120762a30da3ad28cdabed6faf53ba2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60611510"
---
# <a name="transform-data-using-hadoop-mapreduce-activity-in-azure-data-factory"></a>Hadoop MapReduce etkinliği Azure Data Factory kullanarak verileri dönüştürme
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-map-reduce.md)
> * [Geçerli sürüm](transform-data-using-hadoop-map-reduce.md)

HDInsight MapReduce etkinliği bir Data factory'de [işlem hattı](concepts-pipelines-activities.md) üzerinde bir MapReduce programını çağırır [kendi](compute-linked-services.md#azure-hdinsight-linked-service) veya [üzerine](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) HDInsight kümesi. Bu makalede yapılar [veri dönüştürme etkinlikleri](transform-data.md) makalesi, veri dönüştürme ve desteklenen dönüştürme etkinliklerinin genel bir bakış sunar.

Azure Data Factory kullanmaya yeni başladıysanız, okumak [Azure Data Factory'ye giriş](introduction.md) ve öğretici uygulayın: [Öğretici: verileri dönüştürme](tutorial-transform-data-spark-powershell.md) bu makaleyi okuduktan önce.

Bkz: [Pig](transform-data-using-hadoop-pig.md) ve [Hive](transform-data-using-hadoop-hive.md) Pig/Hive çalıştırma hakkında ayrıntılar için komut dosyalarını bir HDInsight üzerinde bir işlem hattından HDInsight Pig ve Hive etkinlikleri kullanarak küme.

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

## <a name="syntax-details"></a>Söz dizimi ayrıntıları

| Özellik          | Açıklama                              | Gerekli |
| ----------------- | ---------------------------------------- | -------- |
| ad              | Etkinliğin adı                     | Evet      |
| açıklama       | Etkinliğin ne için kullanıldığını açıklayan metin | Hayır       |
| type              | MapReduce etkinliği için etkinlik türü HDinsightMapReduce olduğu. | Evet      |
| linkedServiceName | Data Factory öğesinde bağlantılı hizmet olarak HDInsight kümesine başvuru kayıtlı. Bu bağlı hizmeti hakkında bilgi edinmek için [işlem bağlı Hizmetleri](compute-linked-services.md) makalesi. | Evet      |
| className         | Yürütülecek sınıfı adı         | Evet      |
| jarLinkedService  | Bir Azure depolama bağlı hizmeti başvuru Jar dosyalarını depolamak için kullanılır. Bu bağlı hizmeti belirtmezseniz, Azure depolama bağlı HDInsight bağlı hizmette tanımlanan hizmeti kullanılır. | Hayır       |
| jarFilePath       | JarLinkedService tarafından başvurulan Azure storage'da depolanan Jar dosyalarının yolunu belirtin. Dosya adı büyük/küçük harfe duyarlıdır. | Evet      |
| jarlibs           | Dizi jarLinkedService içinde tanımlanan Azure Depolama'da depolanan bir iş tarafından başvurulan Jar kitaplık dosyalarının yolunu dize. Dosya adı büyük/küçük harfe duyarlıdır. | Hayır       |
| getDebugInfo      | Günlük dosyaları Azure depolama için ne zaman kopyalanır belirtir HDInsight küme tarafından kullanılan (veya) jarLinkedService belirtilir. İzin verilen değerler: None, her zaman veya hata. Varsayılan değer: Yok. | Hayır       |
| bağımsız değişkenler         | Hadoop işi için bağımsız değişkenleri dizisini belirtir. Bağımsız değişkenleri, her görev için komut satırı bağımsız değişkenleri geçirilir. | Hayır       |
| tanımlar           | Parametreler içinde Hive betiğine başvurmak için anahtar/değer çiftleri belirtin. | Hayır       |



## <a name="example"></a>Örnek
HDInsight MapReduce etkinliği bir HDInsight kümesi üzerinde bir MapReduce jar dosyasını çalıştırmak için kullanabilirsiniz. Aşağıdaki örnek JSON tanımında bir işlem hattı, bir Mahout JAR dosyasını çalıştırmak için HDInsight faaliyet yapılandırılır.

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
MapReduce programı için herhangi bir bağımsız değişken belirtebilirsiniz **bağımsız değişkenleri** bölümü. Çalışma zamanında, gördüğünüz bazı ek bağımsız değişkenler (örneğin: mapreduce.job.tags) MapReduce çerçeveden. MapReduce bağımsız değişkenleriyle değişkenleriniz ayırt etmek için hem seçeneği hem de değer bağımsız değişken olarak aşağıdaki örnekte gösterildiği gibi kullanmayı düşünün (- s, giriş,--çıktısı, vb. değerlerine göre hemen ardından seçenekleri olan).

## <a name="next-steps"></a>Sonraki adımlar
Anlatan farklı yollarla verileri dönüştürmek aşağıdaki makalelere bakın:

* [U-SQL etkinliği](transform-data-using-data-lake-analytics.md)
* [Hive etkinliği](transform-data-using-hadoop-hive.md)
* [Pig etkinliği](transform-data-using-hadoop-pig.md)
* [Hadoop akış etkinliğinde](transform-data-using-hadoop-streaming.md)
* [Spark etkinliği](transform-data-using-spark.md)
* [.NET özel etkinliği](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning Batch yürütme etkinliği](transform-data-using-machine-learning.md)
* [Saklı yordam etkinliği](transform-data-using-stored-procedure.md)
