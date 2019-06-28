---
title: Azure Data Factory'de Spark etkinliğini kullanarak verileri dönüştürme | Microsoft Docs
description: Spark programları Spark etkinliğini kullanarak bir Azure data factory işlem hattından çalıştırarak verileri dönüştürme hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/31/2018
author: nabhishek
ms.author: abnarain
manager: craigg
ms.openlocfilehash: c493dbc99edc794dd5a261dfc004c2c8c1cb6d52
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67312084"
---
# <a name="transform-data-using-spark-activity-in-azure-data-factory"></a>Azure Data Factory'de Spark etkinliğini kullanarak verileri dönüştürme
> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
> * [Sürüm 1](v1/data-factory-spark.md)
> * [Geçerli sürüm](transform-data-using-spark.md)

Data factory'de Spark etkinliğini [işlem hattı](concepts-pipelines-activities.md) üzerinde bir Spark programını çalıştırır [kendi](compute-linked-services.md#azure-hdinsight-linked-service) veya [üzerine](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) HDInsight kümesi. Bu makalede yapılar [veri dönüştürme etkinlikleri](transform-data.md) makalesi, veri dönüştürme ve desteklenen dönüştürme etkinliklerinin genel bir bakış sunar. Bir isteğe bağlı Spark bağlı hizmeti kullandığınızda, Data Factory bir Spark kümesi, yalnızca verileri işlemek zamanında için otomatik olarak oluşturur ve işleme tamamlandıktan sonra kümeyi siler. 


## <a name="spark-activity-properties"></a>Spark etkinliği özellikleri
Bir Spark etkinliği örnek JSON tanımı aşağıda verilmiştir:    

```json
{
    "name": "Spark Activity",
    "description": "Description",
    "type": "HDInsightSpark",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "sparkJobLinkedService": {
            "referenceName": "MyAzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "rootPath": "adfspark",
        "entryFilePath": "test.py",
        "sparkConfig": {
            "ConfigItem1": "Value"
        },
        "getDebugInfo": "Failure",
        "arguments": [
            "SampleHadoopJobArgument1"
        ]
    }
}
```

Aşağıdaki tabloda JSON tanımında kullanılan JSON özellikleri açıklanmaktadır:

| Özellik              | Açıklama                              | Gerekli |
| --------------------- | ---------------------------------------- | -------- |
| name                  | İşlem hattındaki bir etkinliğin adı.    | Evet      |
| description           | Etkinliğin ne yaptığını açıklayan metin.  | Hayır       |
| türü                  | Spark etkinliği için etkinlik HDInsightSpark türüdür. | Evet      |
| linkedServiceName     | HDInsight Spark bağlı üzerinde Spark programını çalıştırır hizmetin adı. Bu bağlı hizmeti hakkında bilgi edinmek için [işlem bağlı Hizmetleri](compute-linked-services.md) makalesi. | Evet      |
| SparkJobLinkedService | Azure depolama bağlı iş dosyası, bağımlılıklar ve günlükleri Spark tutan hizmeti.  Bu özellik için bir değer belirtmezseniz, HDInsight kümesi ile ilişkili depolama kullanılır. Bu özelliğin değeri yalnızca bir Azure depolama bağlı hizmeti olabilir. | Hayır       |
| rootPath              | Azure Blob kapsayıcısı ve Spark dosyasını içeren klasör. Dosya adı büyük/küçük harfe duyarlıdır. Klasör yapısına bakın (sonraki bölümde) bölümünde bu klasör yapısını hakkındaki ayrıntılar için. | Evet      |
| entryFilePath         | Spark kodun/paketin kök klasörünün göreli yolu. Giriş dosyası, bir Python dosyası ya da bir .jar dosyasını olması gerekir. | Evet      |
| className             | Uygulamanın Java/Spark temel sınıfı      | Hayır       |
| arguments             | Spark programı için komut satırı bağımsız değişkenleri listesi. | Hayır       |
| proxyUser             | Spark programının yürütülecek kimliğine bürünmek için kullanıcı hesabı | Hayır       |
| sparkConfig           | Bu konu başlığı altında listelenen Spark yapılandırma özellikleri için değerleri belirtin: [Spark yapılandırma - uygulama özellikleri](https://spark.apache.org/docs/latest/configuration.html#available-properties). | Hayır       |
| getDebugInfo          | HDInsight kümesi tarafından kullanılan Azure depolama için Spark günlük dosyalarının ne zaman kopyalanır belirtir (veya) sparkJobLinkedService belirtilir. İzin verilen değerler: None, her zaman veya hata. Varsayılan değer: Yok. | Hayır       |

## <a name="folder-structure"></a>klasör yapısı
Spark işlerinde Pig/Hive işlerini daha fazla genişletilebilir. Spark işleri için çoklu bağımlılıklar gibi sağlayabilirsiniz (java sınıf yolu yerleştirilen) paketleri (ise PYTHONPATH üzerinde yerleştirilen) python dosyaları ve diğer dosyaları jar.

HDInsight bağlı hizmeti tarafından başvurulan Azure Blob Depolama alanında aşağıdaki klasör yapısını oluşturun. Ardından, uygun alt klasörleri tarafından temsil edilen kök klasöründe bağımlı dosya yükleme **entryFilePath**. Örneğin, python dosyaları pyFiles alt ve jar dosyaları kök klasörün jar dosyaları dışındaki alt klasörüne yükleyin. Çalışma zamanında Data Factory hizmetinin Azure Blob Depolama alanında aşağıdaki klasör yapısına bekliyor:     

| `Path`                  | Açıklama                              | Gerekli | Type   |
| --------------------- | ---------------------------------------- | -------- | ------ |
| `.` (kök)            | Spark işi depolama bağlı hizmeti kök yolu | Evet      | Klasör |
| &lt;Kullanıcı tanımlı &gt; | Spark işi giriş dosyasına işaret eden yolu | Evet      | Dosya   |
| . / jar'lar                | Bu klasör altındaki tüm dosyaları karşıya yüklenir ve küme java sınıf yolu yerleştirilen | Hayır       | Klasör |
| ./pyFiles             | Bu klasör altındaki tüm dosyaları karşıya yüklenir ve küme ise PYTHONPATH üzerinde yerleştirilen | Hayır       | Klasör |
| . / dosyaları               | Bu klasör altındaki tüm dosyaları karşıya yüklenir ve yürütücü çalışma dizinine yerleştirilir. | Hayır       | Klasör |
| . / arşivleri            | Bu klasör altındaki tüm dosyaları sıkıştırılmamış | Hayır       | Klasör |
| . / günlüğe kaydeder                | Spark kümesi günlükleri içeren klasör. | Hayır       | Klasör |

Azure Blob Depolama HDInsight bağlı hizmeti tarafından başvurulan iki Spark iş dosyalarını içeren bir depolama alanı için bir örnek aşağıda verilmiştir.

```
SparkJob1
    main.jar
    files
        input1.txt
        input2.txt
    jars
        package1.jar
        package2.jar
    logs

SparkJob2
    main.py
    pyFiles
        scrip1.py
        script2.py
    logs
```
## <a name="next-steps"></a>Sonraki adımlar
Anlatan farklı yollarla verileri dönüştürmek aşağıdaki makalelere bakın: 

* [U-SQL etkinliği](transform-data-using-data-lake-analytics.md)
* [Hive etkinliği](transform-data-using-hadoop-hive.md)
* [Pig etkinliği](transform-data-using-hadoop-pig.md)
* [MapReduce etkinliği](transform-data-using-hadoop-map-reduce.md)
* [Hadoop akış etkinliğinde](transform-data-using-hadoop-streaming.md)
* [Spark etkinliği](transform-data-using-spark.md)
* [.NET özel etkinliği](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning Batch yürütme etkinliği](transform-data-using-machine-learning.md)
* [Saklı yordam etkinliği](transform-data-using-stored-procedure.md)
