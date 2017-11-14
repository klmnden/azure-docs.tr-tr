---
title: "Azure Data Factory veri okumak veya yazmak nasıl bölümlenmiş | Microsoft Docs"
description: "Azure Data Factory sürüm 2 bölümlenmiş veri okunamıyor veya yazılamıyor öğrenin."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: shlo
ms.openlocfilehash: 2066847feb3dcdf36ead8901a679d8cae7a6acde
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2017
---
# <a name="how-to-read-or-write-partitioned-data-in-azure-data-factory-version-2"></a>Azure Data Factory sürüm 2 verileri okumak veya yazmak nasıl bölümlenmiş
Sürüm 1'de, Azure Data Factory veri okunurken veya bölümlenmiş SliceStart/SliceEnd/WindowStart/WindowEnd sistem değişkenleri kullanılarak yazılırken desteklenir. Sürüm 2'de, ardışık düzen parametre ve tetikleyici başlangıç saati ve zamanlanan saat parametresinin değeri kullanarak bu davranışı elde edebilirsiniz. 

## <a name="use-a-pipeline-parameter"></a>Ardışık Düzen parametresini kullanın 
Sürüm 1'de, aşağıdaki örnekte gösterildiği gibi SliceStart sistem değişkeni ve partitionedBy özelliği kullanabilirsiniz: 

```json
"folderPath": "adfcustomerprofilingsample/logs/marketingcampaigneffectiveness/yearno={Year}/monthno={Month}/dayno={Day}/",
"partitionedBy": [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "%M" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "%d" } }
],
```

PartitonedBy özelliği hakkında daha fazla bilgi için bkz: [sürüm 1 Azure Blob bağlayıcı](v1/data-factory-azure-blob-connector.md#dataset-properties) makalesi. 

Sürüm 2'de, bu davranışı elde etmenin bir yolu aşağıdaki eylemleri yapmaktır: 

1. Tanımlayan bir **parametresi kanal** dize türünde. Aşağıdaki örnekte, ardışık düzen parametre adıdır **scheduledRunTime**. 
2. Ayarlama **folderPath** ardışık düzen parametresinin değeri için veri kümesi tanımında. 
3. Ardışık Düzen çalıştırmadan önce parametresi için bir sabit kodlanmış değer geçirin. Veya bir tetikleyicinin başlangıç saati veya zamanlanan süreden çalışma zamanında dinamik olarak geçirin. 

```json
"folderPath": {
      "value": "@concat(pipeline().parameters.blobContainer, '/logs/marketingcampaigneffectiveness/yearno=', formatDateTime(pipeline().parameters.scheduledRunTime, 'yyyy'), '/monthno=', formatDateTime(pipeline().parameters.scheduledRunTime, '%M'), '/dayno=', formatDateTime(pipeline().parameters.scheduledRunTime, '%d'), '/')",
      "type": "Expression"
},
```

## <a name="pass-in-value-from-a-trigger"></a>Değer tetikleyiciden geçirmek
Aşağıdaki tetikleyici tanımı'nda, tetikleyici zamanlanmış süre için bir değer olarak geçirilen **scheduledRunTime** parametresi kanal: 

```json
{
    "name": "MyTrigger",
    "properties": {
       ...
        },
        "pipeline": {
            "pipelineReference": {
                "type": "PipelineReference",
                "referenceName": "MyPipeline"
            },
            "parameters": {
                "scheduledRunTime": "@trigger().scheduledTime"
            }
        }
    }
}
```

## <a name="example"></a>Örnek

Örnek veri kümesi tanımı İşte (adlı bir parametre kullanır: `date`):

```json
{
  "type": "AzureBlob",
  "typeProperties": {
    "folderPath": {
      "value": "@concat(pipeline().parameters.blobContainer, '/logs/marketingcampaigneffectiveness/yearno=', formatDateTime(pipeline().parameters.scheduledRunTime, 'yyyy'), '/monthno=', formatDateTime(pipeline().parameters.scheduledRunTime, '%M'), '/dayno=', formatDateTime(pipeline().parameters.scheduledRunTime, '%d'), '/')",
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
                    "PARTITIONEDOUTPUT": {
                        "value": "@concat('wasb://', pipeline().parameters.blobContainer, '@', pipeline().parameters.blobStorageAccount, '.blob.core.windows.net/logs/partitionedgameevents/')",
                        "type": "Expression"
                    },
                    "Year": {
                        "value": "@formatDateTime(pipeline().parameters.scheduledRunTime, 'yyyy')",
                        "type": "Expression"
                    },
                    "Month": {
                        "value": "@formatDateTime(pipeline().parameters.scheduledRunTime, '%M')",
                        "type": "Expression"
                    },
                    "Day": {
                        "value": "@formatDateTime(pipeline().parameters.scheduledRunTime, '%d')",
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
            "scheduledRunTime": {
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
            },
            "partitionHiveScriptFile": {
                "type": "String"
            }
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
Veri Fabrikası sahip işlem hattı oluşturma izlenecek tam yol için bkz: [hızlı başlangıç: bir veri fabrikası oluşturun](quickstart-create-data-factory-powershell.md). 