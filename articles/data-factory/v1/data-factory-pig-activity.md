---
title: Pig etkinliği Azure Data Factory kullanarak verileri dönüştürme | Microsoft Docs
description: Pig etkinliği Azure data factory'de bir üzerinde-istek/bilgisayarınızı kendi HDInsight kümesinde Pig betikleri çalıştırmak için nasıl kullanacağınızı öğrenin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 5af07a1a-2087-455e-a67b-a79841b4ada5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 78ee2c1ce402a29f1a9dfdd29f31daef09134eba
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60611329"
---
# <a name="transform-data-using-pig-activity-in-azure-data-factory"></a>Pig etkinliği Azure Data Factory kullanarak verileri dönüştürme
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
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [Pig etkinliği, Data Factory kullanarak verileri dönüştürme](../transform-data-using-hadoop-pig.md).


Data Factory, HDInsight Pig etkinliği [işlem hattı](data-factory-create-pipelines.md) üzerinde Pig sorguları yürüten [kendi](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) veya [üzerine](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux tabanlı HDInsight kümesi. Bu makalede yapılar [veri dönüştürme etkinlikleri](data-factory-data-transformation-activities.md) makalesi, veri dönüştürme ve desteklenen dönüştürme etkinliklerinin genel bir bakış sunar.

> [!NOTE] 
> Azure Data Factory kullanmaya yeni başladıysanız, okumak [Azure Data Factory'ye giriş](data-factory-introduction.md) ve öğretici uygulayın: [İlk veri işlem hattı oluşturma](data-factory-build-your-first-pipeline.md) bu makaleyi okuduktan önce. 

## <a name="syntax"></a>Sözdizimi

```JSON
{
  "name": "HiveActivitySamplePipeline",
  "properties": {
    "activities": [
      {
        "name": "Pig Activity",
        "description": "description",
        "type": "HDInsightPig",
        "inputs": [
          {
            "name": "input tables"
          }
        ],
        "outputs": [
          {
            "name": "output tables"
          }
        ],
        "linkedServiceName": "MyHDInsightLinkedService",
        "typeProperties": {
          "script": "Pig script",
          "scriptPath": "<pathtothePigscriptfileinAzureblobstorage>",
          "defines": {
            "param1": "param1Value"
          }
        },
        "scheduler": {
          "frequency": "Day",
          "interval": 1
        }
      }
    ]
  }
}
```

## <a name="syntax-details"></a>Söz dizimi ayrıntıları

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| name |Etkinliğin adı |Evet |
| description |Etkinliğin ne için kullanıldığını açıklayan metin |Hayır |
| type |HDinsightPig |Evet |
| inputs |Pig etkinlik tarafından kullanılan bir veya daha fazla giriş |Hayır |
| outputs |Pig etkinliği tarafından üretilen bir veya daha fazla çıkışı |Evet |
| linkedServiceName |Data Factory öğesinde bağlantılı hizmet olarak kayıtlı HDInsight kümesine başvuru |Evet |
| script |Pig betiği satır içi belirtin |Hayır |
| scriptPath |Pig betiği bir Azure blob depolama alanında Store ve dosyanın yolunu belirtin. 'Script' veya 'scriptPath' özelliğini kullanın. Her ikisi de birlikte kullanılamaz. Dosya adı büyük/küçük harfe duyarlıdır. |Hayır |
| defines |Pig betiği içinde başvurmak için anahtar/değer çiftleri parametrelerini belirtin |Hayır |

## <a name="example"></a>Örnek
İstediğiniz analytics şirketiniz tarafından başlatılan oyun oynama oyuncu tarafından harcanan süreyi belirlemek oyun günlüklerinin bir örnek düşünelim.

Aşağıdaki örnek oyun günlük bir virgülle (,) dosyasıdır. Bu, aşağıdaki alanları – Profileıd, SessionStart, süre, Srcıpaddress ve GameType içerir.

```
1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
.....
```

**Pig betiği** bu veriyi işlemek için:

```
PigSampleIn = LOAD 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/samplein/' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);

GroupProfile = Group PigSampleIn all;

PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);

Store PigSampleOut into 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/sampleoutpig/' USING PigStorage (',');
```

Bir Data Factory işlem hattı, bu Pig betiği yürütmek için aşağıdaki adımları uygulayın:

1. Kaydetmek için bağlı hizmet oluşturma [kendi HDInsight işlem kümesi](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) veya yapılandırma [isteğe bağlı HDInsight işlem kümesi](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Bu bağlı hizmeti adlandıralım **HDInsightLinkedService**.
2. Oluşturma bir [bağlı hizmet](data-factory-azure-blob-connector.md) verileri barındıran Azure Blob Depolama bağlantısını yapılandırmak için. Bu bağlı hizmeti adlandıralım **StorageLinkedService**.
3. Oluşturma [veri kümeleri](data-factory-create-datasets.md) girdi ve çıktı verilerini gösteren. Giriş veri kümesi adlandıralım **PigSampleIn** ve çıktı veri kümesi **PigSampleOut**.
4. Bir dosyada Azure Blob Depolama #2. adımda yapılandırılmış Pig sorgu kopyalayın. Verileri barındıran Azure depolama birinden sorgu dosyasını barındıran farklı ise, ayrı bir Azure depolama bağlı hizmet oluşturun. Etkinlik yapılandırması bağlı hizmette bakın. Kullanım **scriptPath** pig betik dosyasının yolunu belirtmek için ve **scriptLinkedService**. 
   
   > [!NOTE]
   > Kullanarak Pig betiği satır içi etkinliği tanımındaki sağlayabilirsiniz **betik** özelliği. Ancak, size tüm özel karakterleri kaçış için betik gereksinimleri olarak bu yaklaşım önerilmez ve hata ayıklama sorunlara neden olabilir. #4. adım izlemek için en iyi yöntem olacaktır.
   >
   >
5. HDInsightPig etkinlikli işlem hattı oluşturursunuz. Bu etkinlik, HDInsight kümesinde Pig betiği çalıştırarak, girdi verilerini işleyen.

    ```JSON
    {
      "name": "PigActivitySamplePipeline",
      "properties": {
        "activities": [
          {
            "name": "PigActivitySample",
            "type": "HDInsightPig",
            "inputs": [
              {
                "name": "PigSampleIn"
              }
            ],
            "outputs": [
              {
                "name": "PigSampleOut"
              }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "typeproperties": {
              "scriptPath": "adfwalkthrough\\scripts\\enrichlogs.pig",
              "scriptLinkedService": "StorageLinkedService"
            },
            "scheduler": {
              "frequency": "Day",
              "interval": 1
            }
          }
        ]
      }
    }
    ```
6. İşlem hattı dağıtın. Bkz: [komut zincirleri oluşturma](data-factory-create-pipelines.md) makale Ayrıntılar için. 
7. Data factory izleme ve yönetim görünümlerini kullanarak işlem hattını izleyeceksiniz. Bkz: [izleme ve Data Factory işlem hatlarını yönetmek](data-factory-monitor-manage-pipelines.md) makale Ayrıntılar için.

## <a name="specifying-parameters-for-a-pig-script"></a>Pig betiği parametrelerini belirtme
Aşağıdaki örneği göz önünde bulundurun: oyun günlüğünü Azure Blob Depolama'ya günlük içe alınan ve depolanan bir klasörde bölümlenmiş göre tarih ve saat. Pig betiği Parametreleştirme ve giriş klasörü konumu çalışma zamanı sırasında dinamik olarak geçirmek de tarih ve saat ile bölümlenmiş çıktı oluşturmak istiyorsunuz.

Parametreli Pig betiği'ni kullanmak için aşağıdakileri yapın:

* Parametreleri tanımlayın **tanımlar**.

    ```JSON
    {
      "name": "PigActivitySamplePipeline",
      "properties": {
        "activities": [
          {
            "name": "PigActivitySample",
            "type": "HDInsightPig",
            "inputs": [
              {
                "name": "PigSampleIn"
              }
            ],
            "outputs": [
              {
                "name": "PigSampleOut"
              }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "typeproperties": {
              "scriptPath": "adfwalkthrough\\scripts\\samplepig.hql",
              "scriptLinkedService": "StorageLinkedService",
              "defines": {
                "Input": "$$Text.Format('wasb: //adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0: yyyy}/monthno={0:MM}/dayno={0: dd}/',SliceStart)",
                "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)"
              }
            },
            "scheduler": {
              "frequency": "Day",
              "interval": 1
            }
          }
        ]
      }
    }
    ```
* Pig betik kullanarak parametreleri başvuran ' **$parameterName**' aşağıdaki örnekte gösterildiği gibi:

    ```
    PigSampleIn = LOAD '$Input' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);
    GroupProfile = Group PigSampleIn all;
    PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);
    Store PigSampleOut into '$Output' USING PigStorage (','); 
    ```

## <a name="see-also"></a>Ayrıca Bkz.
* [Hive etkinliği](data-factory-hive-activity.md)
* [MapReduce etkinliği](data-factory-map-reduce.md)
* [Hadoop akış etkinliğinde](data-factory-hadoop-streaming-activity.md)
* [Spark programlarını çağırma](data-factory-spark.md)
* [R betiklerini çağırma](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)
