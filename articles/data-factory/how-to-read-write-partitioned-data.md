---
title: Azure Data factory'de veri okumak veya yazmak nasıl bölümlenmiş | Microsoft Docs
description: Okuma veya Azure Data Factory'de bölümlenmiş verileri yazma konusunda bilgi edinin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: shlo
ms.openlocfilehash: f2d780900a0cd24f2d70499573a4055dc836ae0b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61466995"
---
# <a name="how-to-read-or-write-partitioned-data-in-azure-data-factory"></a>Azure Data factory'de veri okumak veya yazmak nasıl bölümlenir

Azure Data Factory sürüm 1, size okuma veya bölümlenmiş verileri kullanarak yazma **SliceStart**, **SliceEnd**, **WindowStart**, ve **WindowEnd** sistem değişkenleri. Veri Fabrikası'nın geçerli sürümünde, parametre değeri bir işlem hattı parametre ve bir tetikleyicinin başlangıç saati veya zamanlanan saati'ı kullanarak bu davranışı elde edebilirsiniz. 

## <a name="use-a-pipeline-parameter"></a>Bir işlem hattı parametresini kullanın 

Data Factory sürüm 1'da, kullanabileceğinizi **partitionedBy** özelliği ve **SliceStart** aşağıdaki örnekte gösterildiği gibi sistem değişkeni: 

```json
"folderPath": "adfcustomerprofilingsample/logs/marketingcampaigneffectiveness/{Year}/{Month}/{Day}/",
"partitionedBy": [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "%M" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "%d" } }
],
```

Hakkında daha fazla bilgi için **partitonedBy** özelliğine bakın [veri kopyalama ya da Azure Blob depolamadan Azure Data Factory kullanarak](v1/data-factory-azure-blob-connector.md#dataset-properties). 

Veri Fabrikası'nın geçerli sürümünde bu davranışı elde etmek için: 

1. Tanımlayan bir *parametresi ardışık düzen* türü **dize**. Aşağıdaki örnekte, işlem hattı parametresi adıdır **windowStartTime**. 
2. Ayarlama **folderPath** işlem hattı parametresinin değeri başvurmak için veri kümesi tanımında. 
3. İsteğe bağlı işlem hattı çağırdığınızda, gerçek parametre değerini geçirin. Ayrıca, bir tetikleyici başlangıç zamanı veya çalışma zamanında dinamik olarak zamanlanmış zamanı geçirebilirsiniz. 

```json
"folderPath": {
      "value": "adfcustomerprofilingsample/logs/marketingcampaigneffectiveness/@{formatDateTime(pipeline().parameters.windowStartTime, 'yyyy/MM/dd')}/",
      "type": "Expression"
},
```

## <a name="pass-in-a-value-from-a-trigger"></a>Bir tetikleyici ile bir değer geçirin

Aşağıdaki atlayan pencere tetikleyicisi tanımında, işlem hattı parametresi için bir değer olarak penceresi başlangıç zamanı tetikleyicisinin geçirilen **windowStartTime**: 

```json
{
    "name": "MyTrigger",
    "properties": {
        "type": "TumblingWindowTrigger",
        "typeProperties": {
            "frequency": "Hour",
            "interval": "1",
            "startTime": "2018-05-15T00:00:00Z",
            "delay": "00:10:00",
            "maxConcurrency": 10
        },
        "pipeline": {
            "pipelineReference": {
                "type": "PipelineReference",
                "referenceName": "MyPipeline"
            },
            "parameters": {
                "windowStartTime": "@trigger().outputs.windowStartTime"
            }
        }
    }
}
```

## <a name="example"></a>Örnek

Bir örnek veri kümesi tanımı aşağıda verilmiştir:

```json
{
  "name": "SampleBlobDataset",
  "type": "AzureBlob",
  "typeProperties": {
    "folderPath": {
      "value": "adfcustomerprofilingsample/logs/marketingcampaigneffectiveness/@{formatDateTime(pipeline().parameters.windowStartTime, 'yyyy/MM/dd')}/",
      "type": "Expression"
    },
    "format": {
      "type": "TextFormat",
      "columnDelimiter": ","
    }
  },
  "structure": [
    { "name": "ProfileID", "type": "String" },
    { "name": "SessionStart", "type": "String" },
    { "name": "Duration", "type": "Int32" },
    { "name": "State", "type": "String" },
    { "name": "SrcIPAddress", "type": "String" },
    { "name": "GameType", "type": "String" },
    { "name": "Multiplayer", "type": "String" },
    { "name": "EndRank", "type": "String" },
    { "name": "WeaponsUsed", "type": "Int32" },
    { "name": "UsersInteractedWith", "type": "String" },
    { "name": "Impressions", "type": "String" }
  ],
  "linkedServiceName": {
    "referenceName": "churnStorageLinkedService",
    "type": "LinkedServiceReference"
  }
}
```

İşlem hattı tanımı: 

```json
{
    "properties": {
        "activities": [{
            "type": "HDInsightHive",
            "typeProperties": {
                "scriptPath": {
                    "value": "@concat(pipeline().parameters.blobContainer, '/scripts/', pipeline().parameters.partitionHiveScriptFile)",
                    "type": "Expression"
                },
                "scriptLinkedService": {
                    "referenceName": "churnStorageLinkedService",
                    "type": "LinkedServiceReference"
                },
                "defines": {
                    "RAWINPUT": {
                        "value": "@concat('wasb://', pipeline().parameters.blobContainer, '@', pipeline().parameters.blobStorageAccount, '.blob.core.windows.net/logs/', pipeline().parameters.inputRawLogsFolder, '/')",
                        "type": "Expression"
                    },
                    "Year": {
                        "value": "@formatDateTime(pipeline().parameters.windowStartTime, 'yyyy')",
                        "type": "Expression"
                    },
                    "Month": {
                        "value": "@formatDateTime(pipeline().parameters.windowStartTime, 'MM')",
                        "type": "Expression"
                    },
                    "Day": {
                        "value": "@formatDateTime(pipeline().parameters.windowStartTime, 'dd')",
                        "type": "Expression"
                    }
                }
            },
            "linkedServiceName": {
                "referenceName": "HdiLinkedService",
                "type": "LinkedServiceReference"
            },
            "name": "HivePartitionGameLogs"
        }],
        "parameters": {
            "windowStartTime": {
                "type": "String"
            },
            "blobStorageAccount": {
                "type": "String"
            },
            "blobContainer": {
                "type": "String"
            },
            "inputRawLogsFolder": {
                "type": "String"
            }
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bir işlem hattına sahip bir veri fabrikası oluşturma hakkında tam bir kılavuz için bkz: [hızlı başlangıç: Veri Fabrikası oluşturma](quickstart-create-data-factory-powershell.md). 

