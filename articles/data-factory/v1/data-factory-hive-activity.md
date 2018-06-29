---
title: Hive etkinliği - Azure kullanarak veri dönüştürme | Microsoft Docs
description: Hive etkinliği bir Azure data factory'de bir üzerinde-isteğe bağlı/bilgisayarınızı kendi Hdınsight kümesinde Hive sorguları çalıştırmak için nasıl kullanabileceğinizi öğrenin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 80083218-743e-4da8-bdd2-60d1c77b1227
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: e8d3b83c8508ae5913975edcbf89f4e70a8b08be
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37050857"
---
# <a name="transform-data-using-hive-activity-in-azure-data-factory"></a>Hive etkinliği Azure Data Factory kullanarak veri dönüştürme 
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
> Bu makale, veri fabrikası 1 sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [dönüştürme veri fabrikasında Hive etkinliğini kullanarak verileri](../transform-data-using-hadoop-hive.md).

Veri Fabrikası Hdınsight Hive etkinliğiyle [ardışık düzen](data-factory-create-pipelines.md) üzerinde Hive sorguları yürüten [kendi](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) veya [isteğe bağlı](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux tabanlı Hdınsight kümesi. Bu makalede derlemeler [veri dönüştürme etkinlikleri](data-factory-data-transformation-activities.md) makalesi, veri dönüştürme ve desteklenen dönüştürme etkinliklerinin genel bir bakış sunar.

> [!NOTE] 
> Azure Data Factory yeniyseniz okuyun [Azure Data Factory'ye giriş](data-factory-introduction.md) ve öğretici: [ilk veri hattınızı yapı](data-factory-build-your-first-pipeline.md) bu makaleyi okumadan önce. 

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
## <a name="syntax-details"></a>Sözdizimi ayrıntıları
| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| ad |Etkinlik adı |Evet |
| açıklama |Etkinlik hangi amaçla kullanıldığına açıklayan metin |Hayır |
| type |Hdınsighthive |Evet |
| girişler |Hive etkinlik tarafından kullanılan girişleri |Hayır |
| çıkışlar |Hive etkinliği tarafından üretilen çıkış |Evet |
| linkedServiceName |Veri fabrikasında bağlı hizmet olarak kayıtlı bir Hdınsight kümesine başvuru |Evet |
| komut dosyası |Hive betiği satır içi belirtin |Hayır |
| komut dosyası yolu |Hive betiği bir Azure blob storage'da depolamak ve dosyanın yolunu belirtin. 'Komut dosyası' veya 'scriptPath' özelliğini kullanın. Her ikisi birlikte kullanılamaz. Dosya adı büyük/küçük harf duyarlıdır. |Hayır |
| tanımlar |'Hiveconf' kullanarak Hive betiğini içinde başvurmak için anahtar/değer çiftleri olarak parametrelerini belirtin |Hayır |

## <a name="example"></a>Örnek
Şimdi, şirketiniz tarafından başlatılan oyunlar oynamak kullanıcılar tarafından harcanan süre istediğiniz analytics tanımlamak oyun günlükleri örneği göz önünde bulundurun. 

Aşağıdaki günlük virgül bir örnek oyun günlük olduğu (`,`) ayrılmış ve aşağıdaki alanları – Profileıd, SessionStart, süre, Srcıpaddress ve GameType içerir.

```
1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
.....
```

**Hive betiği** bu verileri işlemek için:

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

Bir Data Factory işlem hattı bu Hive betiğini çalıştırmak için aşağıdakileri yapmanız gerekir

1. Kaydetmek için bağlı hizmet oluşturma [kendi Hdınsight işlem kümesi](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) veya yapılandırma [isteğe bağlı Hdınsight işlem kümesi](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Şimdi bu bağlı hizmetin "HDInsightLinkedService" çağırın.
2. Oluşturma bir [bağlantılı hizmeti](data-factory-azure-blob-connector.md) verileri barındıran Azure Blob Depolama bağlantısını yapılandırmak için. Şimdi bu bağlı hizmetin "StorageLinkedService" çağırın
3. Oluşturma [veri kümeleri](data-factory-create-datasets.md) girdi ve çıktı verilerini işaret ediyor. Girdi veri kümesi "HiveSampleIn" şimdi arayın ve çıkış veri kümesi "HiveSampleOut"
4. Kopya Azure Blob Depolama dosyası olarak Hive sorgusu #2. adımda yapılandırılmış. verileri barındırmak için depolama alanı bu sorgu dosyası barındırma farklı ise, ayrı bir Azure depolama bağlantılı hizmet oluşturun ve ona etkinliğin bakın. Kullanım **scriptPath** sorgu dosyası yığın yolunu belirtmek için ve **scriptLinkedService** komut dosyasını içeren Azure depolama belirtmek için. 
   
   > [!NOTE]
   > Kullanarak Hive betiği satır etkinlik tanımı içinde sağlayabilirsiniz **betik** özelliği. Biz bu yaklaşım, komut dosyası JSON belgesi gereksinimlerini kaçış içinde bulunan tüm özel karakterleri olarak önerilmez ve hata ayıklama sorunlara neden olabilir. En iyi uygulama #4. adım izlemektir.
   > 
   > 
5. Hdınsighthive etkinliği ile işlem hattı oluşturun. Etkinlik işlemler / veri dönüşümler.

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
6. Ardışık Düzen dağıtın. Bkz: [ardışık düzen oluşturma](data-factory-create-pipelines.md) Ayrıntılar için makale. 
7. Veri Fabrikası izleme ve yönetim görünümlerini kullanarak işlem hattını izleme. Bkz: [izleme ve Data Factory işlem hatlarını yönetmek](data-factory-monitor-manage-pipelines.md) Ayrıntılar için makale. 

## <a name="specifying-parameters-for-a-hive-script"></a>Bir Hive betiği parametrelerini belirtme
Bu örnekte, oyun günlükleri Azure Blob depolama alanına günlük alınan ve tarih ve saat ile bölümlenmiş bir klasörde depolanır. Hive betiğini Parametreleştirme ve giriş klasörü konumunu çalışma zamanı sırasında dinamik olarak geçirmek ve ayrıca tarih ve saat ile bölümlenmiş bir çıktı oluşturmak istediğiniz.

Parametreli Hive betiğini kullanmak için aşağıdakileri yapın

* Parametre tanımlayın **tanımlar**.

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
* Parametresini kullanarak Hive betiği başvuran **${hiveconf:parameterName}**. 
  
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
* [Hadoop akış etkinliği](data-factory-hadoop-streaming-activity.md)
* [Spark programlarını çağırma](data-factory-spark.md)
* [R betiklerini çağırma](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

