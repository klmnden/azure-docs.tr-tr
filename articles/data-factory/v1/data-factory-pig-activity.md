---
title: "Pig etkinliği Azure Data Factory kullanarak veri dönüştürme | Microsoft Docs"
description: "Pig etkinlik bir Azure data factory'de bir üzerinde-isteğe bağlı/bilgisayarınızı kendi Hdınsight kümesinde Pig betikleri çalıştırmak için nasıl kullanabileceğinizi öğrenin."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 5af07a1a-2087-455e-a67b-a79841b4ada5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
robots: noindex
ms.openlocfilehash: b179444738584e7033f0872d8ef5b721af1d3970
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="transform-data-using-pig-activity-in-azure-data-factory"></a>Pig etkinliği Azure Data Factory kullanarak veri dönüştürme
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
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [dönüştürme Data Factory sürüm 2 Pig etkinliğini kullanarak verileri](../transform-data-using-hadoop-pig.md).


Veri Fabrikası Hdınsight Pig etkinliğinde [ardışık düzen](data-factory-create-pipelines.md) üzerinde Pig sorguları yürüten [kendi](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) veya [isteğe bağlı](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux tabanlı Hdınsight kümesi. Bu makalede derlemeler [veri dönüştürme etkinlikleri](data-factory-data-transformation-activities.md) makalesi, veri dönüştürme ve desteklenen dönüştürme etkinliklerinin genel bir bakış sunar.

> [!NOTE] 
> Azure Data Factory yeniyseniz okuyun [Azure Data Factory'ye giriş](data-factory-introduction.md) ve öğretici: [ilk veri hattınızı yapı](data-factory-build-your-first-pipeline.md) bu makaleyi okumadan önce. 

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
## <a name="syntax-details"></a>Sözdizimi ayrıntıları
| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| ad |Etkinlik adı |Evet |
| açıklama |Etkinlik hangi amaçla kullanıldığına açıklayan metin |Hayır |
| type |HDinsightPig |Evet |
| Girişleri |Pig etkinlik tarafından kullanılan bir veya daha fazla giriş |Hayır |
| Çıkışları |Pig etkinlik tarafından oluşturulan bir veya daha fazla çıkışları |Evet |
| linkedServiceName |Veri fabrikasında bağlı hizmet olarak kayıtlı bir Hdınsight kümesine başvuru |Evet |
| Komut dosyası |Pig betiği satır içi belirtin |Hayır |
| komut dosyası yolu |Pig betiği bir Azure blob storage'da depolamak ve dosyanın yolunu belirtin. 'Komut dosyası' veya 'scriptPath' özelliğini kullanın. Her ikisi birlikte kullanılamaz. Dosya adı büyük/küçük harf duyarlıdır. |Hayır |
| tanımlar |Pig betiği içinde başvurmak için anahtar/değer çiftleri olarak parametrelerini belirtin |Hayır |

## <a name="example"></a>Örnek
Şimdi, şirketiniz tarafından başlatılan oyunlar oynamak oynatıcıları kullandığı zamanın istediğiniz analytics tanımlamak oyun günlükleri örneği göz önünde bulundurun.

Aşağıdaki örnek oyun günlük bir virgülle (,) dosyasıdır. Aşağıdaki alanları – Profileıd, SessionStart, süre, Srcıpaddress ve GameType içerir.

```
1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
.....
```

**Pig betiği** bu verileri işlemek için:

```
PigSampleIn = LOAD 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/samplein/' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);

GroupProfile = Group PigSampleIn all;

PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);

Store PigSampleOut into 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/sampleoutpig/' USING PigStorage (',');
```

Bir Data Factory işlem hattı bu Pig betiği çalıştırmak için aşağıdaki adımları uygulayın:

1. Kaydetmek için bağlı hizmet oluşturma [kendi Hdınsight işlem kümesi](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) veya yapılandırma [isteğe bağlı Hdınsight işlem kümesi](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Şimdi bu bağlı hizmetin çağrısı **HDInsightLinkedService**.
2. Oluşturma bir [bağlantılı hizmeti](data-factory-azure-blob-connector.md) verileri barındıran Azure Blob Depolama bağlantısını yapılandırmak için. Şimdi bu bağlı hizmetin çağrısı **StorageLinkedService**.
3. Oluşturma [veri kümeleri](data-factory-create-datasets.md) girdi ve çıktı verilerini işaret ediyor. Şimdi girdi veri kümesi çağrısı **PigSampleIn** ve çıktı veri kümesi **PigSampleOut**.
4. Bir dosyayı Azure Blob Storage #2. adımda yapılandırılmış Pig sorgu kopyalayın. Verileri barındıran Azure depolama birinden sorgu dosyası barındıran farklıysa, ayrı bir Azure depolama bağlantılı hizmet oluşturun. Etkinlik yapılandırması bağlantılı hizmetteki bakın. Kullanım ** scriptPath ** pig komut dosyasının yolunu belirtmek için ve **scriptLinkedService**. 
   
   > [!NOTE]
   > Pig betiği satır etkinlik tanımı içinde kullanarak da sağlayabilirsiniz **betik** özelliği. Ancak, biz bu yaklaşım, tüm özel komut dosyası gereksinimlerini kaçış karakteri olarak önerilmez ve hata ayıklama sorunlara neden olabilir. En iyi uygulama #4. adım izlemektir.
   > 
   > 
5. HDInsightPig etkinliği ile işlem hattı oluşturma. Bu etkinlik, Hdınsight kümesinde Pig betiği çalıştırarak giriş verilerini işler.

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
6. Ardışık Düzen dağıtın. Bkz: [ardışık düzen oluşturma](data-factory-create-pipelines.md) Ayrıntılar için makale. 
7. Veri Fabrikası izleme ve yönetim görünümlerini kullanarak işlem hattını izleme. Bkz: [izleme ve Data Factory işlem hatlarını yönetmek](data-factory-monitor-manage-pipelines.md) Ayrıntılar için makale.

## <a name="specifying-parameters-for-a-pig-script"></a>Pig betiği parametrelerini belirtme
Aşağıdaki örneği göz önünde bulundurun: oyun günlükleri günlük Azure Blob depolama alanına alınan ve bölümlenmiş temel alınarak tarih ve saat bir klasörde depolanır. Pig betiği Parametreleştirme ve giriş klasörü konumunu çalışma zamanı sırasında dinamik olarak geçirmek ve ayrıca tarih ve saat ile bölümlenmiş bir çıktı oluşturmak istediğiniz.

Parametreli Pig betiği kullanmak için aşağıdakileri yapın:

* Parametre tanımlayın **tanımlar**.

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
* Pig betiği kullanarak parametreler başvuruda '**$parameterName**' aşağıdaki örnekte gösterildiği gibi:

    ```  
    PigSampleIn = LOAD '$Input' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);    
    GroupProfile = Group PigSampleIn all;        
    PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);        
    Store PigSampleOut into '$Output' USING PigStorage (','); 
    ```
## <a name="see-also"></a>Ayrıca Bkz.
* [Hive etkinliği](data-factory-hive-activity.md)
* [MapReduce etkinliği](data-factory-map-reduce.md)
* [Hadoop akış etkinliği](data-factory-hadoop-streaming-activity.md)
* [Spark programlarını çağırma](data-factory-spark.md)
* [R betiklerini çağırma](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

