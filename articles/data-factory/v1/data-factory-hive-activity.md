---
title: Hive etkinliği - Azure'ı kullanarak verileri dönüştürme | Microsoft Docs
description: Hive etkinliği Azure data factory'de bir üzerinde-istek/bilgisayarınızı kendi HDInsight kümesinde Hive sorguları çalıştırmak için nasıl kullanacağınızı öğrenin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 80083218-743e-4da8-bdd2-60d1c77b1227
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: a63ef969f17fc48145174d99fec53e77b61885a4
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827983"
---
# <a name="transform-data-using-hive-activity-in-azure-data-factory"></a>Azure Data Factory'de Hive etkinliğini kullanarak verileri dönüştürme 
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
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [Data Factory'de Hive etkinliğini kullanarak verileri dönüştürme](../transform-data-using-hadoop-hive.md).

HDInsight Hive etkinliği bir Data factory'de [işlem hattı](data-factory-create-pipelines.md) üzerinde Hive sorguları yürüten [kendi](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) veya [üzerine](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux tabanlı HDInsight kümesi. Bu makalede yapılar [veri dönüştürme etkinlikleri](data-factory-data-transformation-activities.md) makalesi, veri dönüştürme ve desteklenen dönüştürme etkinliklerinin genel bir bakış sunar.

> [!NOTE] 
> Azure Data Factory kullanmaya yeni başladıysanız, okumak [Azure Data Factory'ye giriş](data-factory-introduction.md) ve öğretici uygulayın: [İlk veri işlem hattı oluşturma](data-factory-build-your-first-pipeline.md) bu makaleyi okuduktan önce. 

## <a name="syntax"></a>Sözdizimi

```JSON
{
    "name": "Hive Activity",
    "description": "description",
    "type": "HDInsightHive",
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
      "script": "Hive script",
      "scriptPath": "<pathtotheHivescriptfileinAzureblobstorage>",
      "defines": {
        "param1": "param1Value"
      }
    },
   "scheduler": {
      "frequency": "Day",
      "interval": 1
    }
}
```
## <a name="syntax-details"></a>Söz dizimi ayrıntıları
| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| name |Etkinliğin adı |Evet |
| description |Etkinliğin ne için kullanıldığını açıklayan metin |Hayır |
| türü |HDinsightHive |Evet |
| inputs |Hive etkinliği tarafından tüketilen girişleri |Hayır |
| outputs |Hive etkinliği tarafından üretilen çıkış |Evet |
| linkedServiceName |Data Factory öğesinde bağlantılı hizmet olarak kayıtlı HDInsight kümesine başvuru |Evet |
| betiğini çalıştırın |Hive betiği satır içi belirtin |Hayır |
| ScriptPath |Hive betiği bir Azure blob depolama alanında Store ve dosyanın yolunu belirtin. 'Script' veya 'scriptPath' özelliğini kullanın. Her ikisi de birlikte kullanılamaz. Dosya adı büyük/küçük harfe duyarlıdır. |Hayır |
| defines |Hive betiği 'hiveconf' kullanarak içinde başvurmak için anahtar/değer çiftleri parametrelerini belirtin |Hayır |

## <a name="example"></a>Örnek
İstediğiniz analytics şirketiniz tarafından başlatılan oyun oynama kullanıcılar tarafından harcanan süreyi belirlemek oyun günlüğünü bir örnek düşünelim. 

Aşağıdaki günlük virgül bir örnek oyun günlük olduğu (`,`) ayrılmış ve – Profileıd, SessionStart, süre, Srcıpaddress ve GameType aşağıdaki alanları içerir.

```
1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
.....
```

**Hive betiği** bu veriyi işlemek için:

```
DROP TABLE IF EXISTS HiveSampleIn; 
CREATE EXTERNAL TABLE HiveSampleIn 
(
    ProfileID        string, 
    SessionStart     string, 
    Duration         int, 
    SrcIPAddress     string, 
    GameType         string
) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/samplein/'; 

DROP TABLE IF EXISTS HiveSampleOut; 
CREATE EXTERNAL TABLE HiveSampleOut 
(    
    ProfileID     string, 
    Duration     int
) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/sampleout/';

INSERT OVERWRITE TABLE HiveSampleOut
Select 
    ProfileID,
    SUM(Duration)
FROM HiveSampleIn Group by ProfileID
```

Bir Data Factory işlem hattı, bu Hive betiğini çalıştırmak için aşağıdakileri yapmanız gerekir

1. Kaydetmek için bağlı hizmet oluşturma [kendi HDInsight işlem kümesi](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) veya yapılandırma [isteğe bağlı HDInsight işlem kümesi](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Bu bağlı hizmeti "HDInsightLinkedService" olarak adlandıralım.
2. Oluşturma bir [bağlı hizmet](data-factory-azure-blob-connector.md) verileri barındıran Azure Blob Depolama bağlantısını yapılandırmak için. Bu bağlı hizmeti "StorageLinkedService" adlandıralım
3. Oluşturma [veri kümeleri](data-factory-create-datasets.md) girdi ve çıktı verilerini gösteren. Giriş veri kümesi "HiveSampleIn" adlandıralım ve çıktı veri kümesi "HiveSampleOut"
4. Hive sorgusu olarak Azure Blob depolamaya bir dosya kopyalama #2. adımda yapılandırılmış. Depolama verileri barındırmak için bu sorgu dosyasını barındıran farklı ise, ayrı bir Azure depolama bağlı hizmeti oluşturma ve etkinliğin başvurduğu. Kullanım **scriptPath** hive sorgu dosyası yolu belirtmek için ve **scriptLinkedService** komut dosyasını içeren Azure depolama belirtmek için. 
   
   > [!NOTE]
   > Kullanarak Hive betiği satır içi etkinliği tanımındaki sağlayabilirsiniz **betik** özelliği. Biz, tüm özel komut dosyası içinde JSON belgesi gerekir kaçış karakteri olarak bu yaklaşım önerilmez ve hata ayıklama sorunlara neden olabilir. #4. adım izlemek için en iyi yöntem olacaktır.
   > 
   > 
5. Hdınsighthive etkinliğiyle bir işlem hattı oluşturma. Etkinlik verileri işler / dönüştürür.

    ```JSON   
    {   
        "name": "HiveActivitySamplePipeline",
        "properties": {
        "activities": [
            {
                "name": "HiveActivitySample",
                "type": "HDInsightHive",
                "inputs": [
                {
                    "name": "HiveSampleIn"
                }
                ],
                "outputs": [
                {
                    "name": "HiveSampleOut"
                }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                    "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                    "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
            ]
        }
    }
    ```
6. İşlem hattı dağıtın. Bkz: [komut zincirleri oluşturma](data-factory-create-pipelines.md) makale Ayrıntılar için. 
7. Data factory izleme ve yönetim görünümlerini kullanarak işlem hattını izleyeceksiniz. Bkz: [izleme ve Data Factory işlem hatlarını yönetmek](data-factory-monitor-manage-pipelines.md) makale Ayrıntılar için. 

## <a name="specifying-parameters-for-a-hive-script"></a>Bir Hive betiği parametrelerini belirtme
Bu örnekte, oyun günlüğünü günlük Azure Blob Depolama'ya içe alınan ve tarih ve saat ile bölümlenmiş bir klasörde depolanır. Hive betiğinin Parametreleştirme ve giriş klasörü konumu çalışma zamanı sırasında dinamik olarak geçirmek de tarih ve saat ile bölümlenmiş çıktı oluşturmak istiyorsunuz.

Parametreli bir Hive betiği kullanmak için aşağıdakileri yapın

* Parametreleri tanımlayın **tanımlar**.

    ```JSON  
    {
        "name": "HiveActivitySamplePipeline",
          "properties": {
        "activities": [
             {
                "name": "HiveActivitySample",
                "type": "HDInsightHive",
                "inputs": [
                      {
                        "name": "HiveSampleIn"
                      }
                ],
                "outputs": [
                      {
                        "name": "HiveSampleOut"
                    }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                      "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                      "scriptLinkedService": "StorageLinkedService",
                      "defines": {
                        "Input": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)",
                        "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)"
                      },
                       "scheduler": {
                          "frequency": "Hour",
                          "interval": 1
                    }
                }
              }
        ]
      }
    }
    ```
* Parametresini kullanarak Hive Betiğinde bakın **${hiveconf:parameterName}** . 
  
    ```
    DROP TABLE IF EXISTS HiveSampleIn; 
    CREATE EXTERNAL TABLE HiveSampleIn 
    (
        ProfileID     string, 
        SessionStart     string, 
        Duration     int, 
        SrcIPAddress     string, 
        GameType     string
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Input}'; 

    DROP TABLE IF EXISTS HiveSampleOut; 
    CREATE EXTERNAL TABLE HiveSampleOut 
    (
        ProfileID     string, 
        Duration     int
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Output}';

    INSERT OVERWRITE TABLE HiveSampleOut
    Select 
        ProfileID,
        SUM(Duration)
    FROM HiveSampleIn Group by ProfileID
    ```
  ## <a name="see-also"></a>Ayrıca Bkz.
* [Pig etkinliği](data-factory-pig-activity.md)
* [MapReduce etkinliği](data-factory-map-reduce.md)
* [Hadoop akış etkinliğinde](data-factory-hadoop-streaming-activity.md)
* [Spark programlarını çağırma](data-factory-spark.md)
* [R betiklerini çağırma](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

