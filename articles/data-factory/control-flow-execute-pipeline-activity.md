---
title: "Azure Data Factory'de işlem hattı etkinliği yürütmek | Microsoft Docs"
description: "Başka bir Data Factory işlem hattı gelen bir Data Factory işlem hattı çağrılacak yürütme ardışık düzen etkinlik nasıl kullanabileceğinizi öğrenin."
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
ms.date: 09/05/2017
ms.author: shlo
ms.openlocfilehash: 413d7ddf1e5b87f64c0d8e14c0ef4bdefd2890a7
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="execute-pipeline-activity-in-azure-data-factory"></a>Azure Data Factory'de işlem hattı etkinliği yürütmek
Ardışık Düzen yürütme etkinliği başka bir işlem hattı çağırmak Data Factory işlem hattı sağlar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [veri fabrikası V1 belgelerine](v1/data-factory-introduction.md).

## <a name="syntax"></a>Sözdizimi

```json
{
    "name": "MyPipeline",
    "properties": {
        "activities": [
            {
                "name": "ExecutePipelineActivity",
                "type": "ExecutePipeline",
                "typeProperties": {
                    "parameters": {                        
                        "mySourceDatasetFolderPath": {
                            "value": "@pipeline().parameters.mySourceDatasetFolderPath",
                            "type": "Expression"
                        }
                    },
                    "pipeline": {
                        "referenceName": "<InvokedPipelineName>",
                        "type": "PipelineReference"
                    },
                    "waitOnCompletion": true
                 }
            }
        ],
        "parameters": [
            {
                "mySourceDatasetFolderPath": {
                    "type": "String"
                }
            }
        ]
    }
}
```

## <a name="type-properties"></a>Tür özellikleri
Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | --------
ad | Yürütme ardışık düzen etkinlik adı. | Dize | Evet
type | Ayarlanmalıdır: **ExecutePipeline**. | Dize | Evet
Ardışık Düzen | Bu ardışık düzen çağırır bağımlı ardışık ardışık düzen başvuru. Ardışık Düzen başvuru nesnesi iki özelliklere sahiptir: **başvuruadı** ve **türü**. Başvuruadı özelliği başvuru ardışık düzen adını belirtir. Type özelliği PipelineReference için ayarlamanız gerekir. | PipelineReference | Evet
parametreler | Çağrılan ardışık düzene geçirilecek Parametreler | Parametre adları bağımsız değişken değerine eşleyen bir JSON nesnesi | Hayır
waitOnCompletion | Etkinlik yürütme bağımlı ardışık düzen yürütmesi tamamlanmasını bekleyip beklemediğini tanımlar. | Varsayılan false olur. | Boole değeri | Hayır

## <a name="sample"></a>Örnek
Bu senaryo iki ardışık düzenler şunlardır:

- **Ana ardışık düzen** -bu ardışık düzen çağrılan ardışık düzen çağıran bir yürütme ardışık etkinlik vardır. Ana ardışık iki parametre alır: `masterSourceBlobContainer`, `masterSinkBlobContainer`.
- **Çağrılan işlem hattı** -bu ardışık düzen, verileri Azure Blob havuz için bir Azure Blob kaynaktan kopyalayan bir kopyalama etkinliği vardır. Çağrılan işlem hattı iki parametre alır: `sourceBlobContainer`, `sinkBlobContainer`.

### <a name="master-pipeline-definition"></a>Ana ardışık düzen tanımı

```json
{
  "name": "masterPipeline",
  "properties": {
    "activities": [
      {
        "type": "ExecutePipeline",
        "typeProperties": {
          "pipeline": {
            "referenceName": "invokedPipeline",
            "type": "PipelineReference"
          },
          "parameters": {
            "sourceBlobContainer": {
              "value": "@pipeline().parameters.masterSourceBlobContainer",
              "type": "Expression"
            },
            "sinkBlobCountainer": {
              "value": "@pipeline().parameters.masterSinkBlobContainer",
              "type": "Expression"
            }
          },
          "waitOnCompletion": true
        },
        "name": "MyExecutePipelineActivity"
      }
    ],
    "parameters": {
      "masterSourceBlobContainer": {
        "type": "String"
      },
      "masterSinkBlobContainer": {
        "type": "String"
      }
    }
  }
}

```

### <a name="invoked-pipeline-definition"></a>Çağrılan ardışık düzen tanımı

```json
{
  "name": "invokedPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
        "name": "CopyBlobtoBlob",
        "inputs": [
          {
            "referenceName": "SourceBlobDataset",
            "type": "DatasetReference"
          }
        ],
        "outputs": [
          {
            "referenceName": "sinkBlobDataset",
            "type": "DatasetReference"
          }
        ]
      }
    ],
    "parameters": {
      "sourceBlobContainer": {
        "type": "String"
      },
      "sinkBlobContainer": {
        "type": "String"
      }
    }
  }
}

```

**Bağlı hizmet**

```json
{
    "name": "BlobStorageLinkedService",
    "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": {
        "value": "DefaultEndpointsProtocol=https;AccountName=*****",
        "type": "SecureString"
      }
    }
  }
}
```

**Kaynak veri kümesi**
```json
{
    "name": "SourceBlobDataset",
    "properties": {
    "type": "AzureBlob",
    "typeProperties": {
      "folderPath": {
        "value": "@pipeline().parameters.sourceBlobContainer",
        "type": "Expression"
      },
      "fileName": "salesforce.txt"
    },
    "linkedServiceName": {
      "referenceName": "BlobStorageLinkedService",
      "type": "LinkedServiceReference"
    }
  }
}
```

**Veri kümesi havuzu**
```json
{
    "name": "sinkBlobDataset",
    "properties": {
    "type": "AzureBlob",
    "typeProperties": {
      "folderPath": {
        "value": "@pipeline().parameters.sinkBlobContainer",
        "type": "Expression"
      }
    },
    "linkedServiceName": {
      "referenceName": "BlobStorageLinkedService",
      "type": "LinkedServiceReference"
    }
  }
}
```

### <a name="running-the-pipeline"></a>Ardışık Düzen çalıştırma

Bu örnekte ana ardışık düzen çalıştırmak için aşağıdaki değerleri masterSourceBlobContainer ve masterSinkBlobContainer parametrelerini geçirilir: 

```json
{
  "masterSourceBlobContainer": "executetest",
  "masterSinkBlobContainer": "executesink"
}
```

Ana ardışık düzen, aşağıdaki örnekte gösterildiği gibi bu değerleri çağrılan ardışık düzene iletir: 

```json
{
    "type": "ExecutePipeline",
    "typeProperties": {
      "pipeline": {
        "referenceName": "invokedPipeline",
        "type": "PipelineReference"
      },
      "parameters": {
        "sourceBlobContainer": {
          "value": "@pipeline().parameters.masterSourceBlobContainer",
          "type": "Expression"
        },
        "sinkBlobCountainer": {
          "value": "@pipeline().parameters.masterSinkBlobContainer",
          "type": "Expression"
        }
      },

      ....
}

```
## <a name="next-steps"></a>Sonraki adımlar
Data Factory ile desteklenen diğer denetim akışı etkinlikleri bakın: 

- [Her etkinlik için](control-flow-for-each-activity.md)
- [Meta Veri Alma Etkinliği](control-flow-get-metadata-activity.md)
- [Arama Etkinliği](control-flow-lookup-activity.md)
- [Web etkinliği](control-flow-web-activity.md)