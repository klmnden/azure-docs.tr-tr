---
title: Azure Data Factory veri okumak veya yazmak nasıl bölümlenmiş | Microsoft Docs
description: Azure Data Factory'de bölümlenmiş veri okunamıyor veya yazılamıyor öğrenin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: shlo
ms.openlocfilehash: 59644f3318e2bf9c4f0ea6c3f5699fe1d19f2089
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37053719"
---
# <a name="how-to-read-or-write-partitioned-data-in-azure-data-factory"></a>Azure Data Factory veri okumak veya yazmak nasıl bölümlenmiş
Sürüm 1'de, Azure Data Factory veri okunurken veya bölümlenmiş SliceStart/SliceEnd/WindowStart/WindowEnd sistem değişkenleri kullanılarak yazılırken desteklenir. Veri Fabrikası geçerli sürümde parametresinin değeri bir ardışık düzen parametre ve tetikleyici başlangıç saati ve zamanlanan saat'ı kullanarak bu davranışı elde edebilirsiniz. 

## <a name="use-a-pipeline-parameter"></a>Ardışık Düzen parametresini kullanın 
Sürüm 1'de, aşağıdaki örnekte gösterildiği gibi SliceStart sistem değişkeni ve partitionedBy özelliği kullanabilirsiniz: 

```json
"folderPath": "adfcustomerprofilingsample/logs/marketingcampaigneffectiveness/{Year}/{Month}/{Day}/",
"partitionedBy": [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "%M" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "%d" } }
],
```

PartitonedBy özelliği hakkında daha fazla bilgi için bkz: [sürüm 1 Azure Blob bağlayıcı](v1/data-factory-azure-blob-connector.md#dataset-properties) makalesi. 

Veri Fabrikası geçerli sürümünde bu davranışı elde etmenin bir yolu aşağıdaki eylemleri yapmaktır: 

1. Tanımlayan bir **parametresi kanal** dize türünde. Aşağıdaki örnekte, ardışık düzen parametre adıdır **windowStartTime**. 
2. Ayarlama **folderPath** ardışık düzen parametresinin değeri başvurmak için veri kümesi tanımında. 
3. Ardışık Düzen isteğe bağlı çağrılırken parametresi için gerçek değer geçti veya bir tetikleyicinin başlangıç saati ve zamanlanan saat çalışma zamanında dinamik olarak geçirin. 

```json
"folderPath": {
      "value": "adfcustomerprofilingsample/logs/marketingcampaigneffectiveness/@{formatDateTime(pipeline().parameters.windowStartTime, 'yyyy/MM/dd')}/",
      "type": "Expression"
},
```

## <a name="pass-in-value-from-a-trigger"></a>Değer tetikleyiciden geçirmek
Aşağıdaki dönen pencere tetikleyici tanımında, ardışık düzen parametresi için bir değer olarak penceresi başlangıç zamanı tetikleyicinin geçirilen **windowStartTime**: 

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

Örnek veri kümesi tanımı aşağıda verilmiştir:

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

Ardışık düzen tanımı: 

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
Veri Fabrikası sahip işlem hattı oluşturma izlenecek tam yol için bkz: [hızlı başlangıç: bir veri fabrikası oluşturun](quickstart-create-data-factory-powershell.md). 
