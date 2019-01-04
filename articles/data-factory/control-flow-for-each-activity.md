---
title: ForEach etkinliği, Azure Data Factory | Microsoft Docs
description: Her etkinlik için işlem hattınızda yinelenen bir denetim akışını tanımlar. Bir koleksiyon yineleme için kullanılır ve belirtilen etkinlikleri yürütün.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: shlo
ms.openlocfilehash: 90c36e728a8ec91606f93c080258eeca9c3825e6
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54020787"
---
# <a name="foreach-activity-in-azure-data-factory"></a>ForEach etkinliği, Azure Data Factory
ForEach etkinliği, işlem hattınızda yinelenen bir denetim akışını tanımlar. Bu etkinlik bir koleksiyon üzerinde yinelemek için kullanılır ve bir döngüde belirtilen etkinlikleri yürütür. Bu etkinliğin döngü uygulaması, programlama dillerindeki Foreach döngü yapısına benzer.

## <a name="syntax"></a>Sözdizimi
Özellikler, makalenin sonraki bölümlerinde açıklanmıştır. Items özelliğini koleksiyonudur ve koleksiyondaki her öğe kullanılarak adlandırılır `@item()` aşağıdaki sözdiziminde gösterildiği gibi:  

```json
{  
   "name":"MyForEachActivityName",
   "type":"ForEach",
   "typeProperties":{  
      "isSequential":"true",
        "items": {
            "value": "@pipeline().parameters.mySinkDatasetFolderPathCollection",
            "type": "Expression"
        },
      "activities":[  
         {  
            "name":"MyCopyActivity",
            "type":"Copy",
            "typeProperties":{  
               ...
            },
            "inputs":[  
               {  
                  "referenceName":"MyDataset",
                  "type":"DatasetReference",
                  "parameters":{  
                     "MyFolderPath":"@pipeline().parameters.mySourceDatasetFolderPath"
                  }
               }
            ],
            "outputs":[  
               {  
                  "referenceName":"MyDataset",
                  "type":"DatasetReference",
                  "parameters":{  
                     "MyFolderPath":"@item()"
                  }
               }
            ]
         }
      ]
   }
}

```

## <a name="type-properties"></a>Tür özellikleri

Özellik | Açıklama | İzin verilen değerler | Gereklidir
-------- | ----------- | -------------- | --------
ad | İçin-her etkinliğin adı. | Dize | Evet
type | Ayarlanmalıdır **ForEach** | Dize | Evet
isSequential | Sıralı veya paralel döngü gerçekleştirilip gerçekleştirilmeyeceğini belirtir.  En fazla 20 döngü yinelemesi aynı anda paralel olarak gerçekleştirilebilir). Örneğin, bir ForEach etkinliği bir kopyalama etkinliği ile 10 farklı kaynak ve havuz veri kümeleri üzerinde yineleme varsa **isSequential** False olarak ayarlanırsa, tüm kopyaları aynı anda çalıştırılır. False varsayılan değerdir. <br/><br/> "İsSequential" False olarak ayarlarsanız, birden fazla yürütülebilir dosyaları çalıştırmasına doğru bir yapılandırma olduğundan emin olun. Aksi takdirde, bu özellik yazma çakışmalarını ücretlendirmeden kaçınmak için dikkatli kullanılmalıdır. Daha fazla bilgi için [Paralel yürütme](#parallel-execution) bölümü. | Boole | Hayır. False varsayılan değerdir.
batchCount | (İsSequential false olarak ayarlandığında) paralel yürütme sayısını kontrol etmek için kullanılacak toplu iş sayısı. | Tamsayı (maksimum 50) | Hayır. Varsayılan 20'dir.
Öğeler | Bir JSON dizisi üzerinde yinelenir döndüren bir ifade. | (Bir JSON dizisi döndüren) ifadesi | Evet
Etkinlikler | Yürütülecek etkinlikler. | Etkinlikler Listesi | Evet

## <a name="parallel-execution"></a>Paralel yürütme
Varsa **isSequential** 20 eş zamanlı yinelemeler için en fazla paralel etkinlik yinelenir false olarak ayarlayın. Bu ayarı dikkatli kullanılmalıdır. Eş zamanlı yinelemeleri aynı klasöre ancak farklı dosyalar yazıyorsanız, bu iyi bir yaklaşımdır. Eş zamanlı yinelemeleri tam aynı dosyaya eşzamanlı olarak yazıyorsanız, bu yaklaşım büyük olasılıkla bir hataya neden olur. 

## <a name="iteration-expression-language"></a>Yineleme ifade dili
ForEach etkinliği, üzerinde özellik için yinelendiğinde için bir dizi sağlamak **öğeleri**. " Kullanım `@item()` ForEach etkinliği, tek bir sabit listesi yinelemek için. Örneğin, varsa **öğeleri** dizisi: [1, 2, 3], `@item()` ilk yinelemeyi ikinci Yineleme 2 ve 3'te üçüncü yineleme 1 döndürür.

## <a name="iterating-over-a-single-activity"></a>Tek bir etkinlik yineleme yapma
**Senaryo:** Azure Blob aynı kaynak dosyada Azure Blob içinde birden çok hedef dosyaları kopyalayın.

### <a name="pipeline-definition"></a>İşlem hattı

```json
{
    "name": "<MyForEachPipeline>",
    "properties": {
        "activities": [
            {
                "name": "<MyForEachActivity>",
                "type": "ForEach",
                "typeProperties": {
                    "isSequential": "true",
                    "items": {
                        "value": "@pipeline().parameters.mySinkDatasetFolderPath",
                        "type": "Expression"
                    },
                    "activities": [
                        {
                            "name": "MyCopyActivity",
                            "type": "Copy",
                            "typeProperties": {
                                "source": {
                                    "type": "BlobSource",
                                    "recursive": "false"
                                },
                                "sink": {
                                    "type": "BlobSink",
                                    "copyBehavior": "PreserveHierarchy"
                                }
                            },
                            "inputs": [
                                {
                                    "referenceName": "<MyDataset>",
                                    "type": "DatasetReference",
                                    "parameters": {
                                        "MyFolderPath": "@pipeline().parameters.mySourceDatasetFolderPath"
                                    }
                                }
                            ],
                            "outputs": [
                                {
                                    "referenceName": "MyDataset",
                                    "type": "DatasetReference",
                                    "parameters": {
                                        "MyFolderPath": "@item()"
                                    }
                                }
                            ]
                        }
                    ]
                }
            }
        ],
        "parameters": {
            "mySourceDatasetFolderPath": {
                "type": "String"
            },
            "mySinkDatasetFolderPath": {
                "type": "String"
            }
        }
    }
}

```

### <a name="blob-dataset-definition"></a>BLOB veri kümesi tanımı

```json
{  
   "name":"<MyDataset>",
   "properties":{  
      "type":"AzureBlob",
      "typeProperties":{  
         "folderPath":{  
            "value":"@dataset().MyFolderPath",
            "type":"Expression"
         }
      },
      "linkedServiceName":{  
         "referenceName":"StorageLinkedService",
         "type":"LinkedServiceReference"
      },
      "parameters":{  
         "MyFolderPath":{  
            "type":"String"
         }
      }
   }
}

```

### <a name="run-parameter-values"></a>Parametre değerlerini çalıştırın

```json
{
    "mySourceDatasetFolderPath": "input/",
    "mySinkDatasetFolderPath": [ "outputs/file1", "outputs/file2" ]
}

```

## <a name="iterate-over-multiple-activities"></a>Birden çok etkinlik yineleme yapma
Birden çok etkinlik yinelemek mümkündür (örneğin: kopyalama ve web etkinlikleri) içinde bir ForEach etkinliği. Bu senaryoda, birden çok ayrı bir işlem hattı etkinliklerine göz soyut öneririz. Daha sonra kullanabileceğiniz [ExecutePipeline etkinliğinde](control-flow-execute-pipeline-activity.md) işlem hattındaki ForEach etkinliği, birden çok etkinliği olan ayrı bir işlem hattını çağırmasını ile. 


### <a name="syntax"></a>Sözdizimi

```json
{
  "name": "masterPipeline",
  "properties": {
    "activities": [
      {
        "type": "ForEach",
        "name": "<MyForEachMultipleActivities>"
        "typeProperties": {
          "isSequential": true,
          "items": {
            ...
          },
          "activities": [
            {
              "type": "ExecutePipeline",
              "name": "<MyInnerPipeline>"
              "typeProperties": {
                "pipeline": {
                  "referenceName": "<copyHttpPipeline>",
                  "type": "PipelineReference"
                },
                "parameters": {
                  ...
                },
                "waitOnCompletion": true
              }
            }
          ]
        }
      }
    ],
    "parameters": {
      ...
    }
  }
}

```
### <a name="example"></a>Örnek
**Senaryo:** Bir ForEach etkinliği ile işlem hattı yürütme etkinliği içinde bir InnerPipeline üzerinden yineleme yapma. İç işlem hattı ile parametreli şema tanımları kopyalar.

#### <a name="master-pipeline-definition"></a>Ana işlem hattı

```json
{
  "name": "masterPipeline",
  "properties": {
    "activities": [
      {
        "type": "ForEach",
        "name": "MyForEachActivity",
        "typeProperties": {
          "isSequential": true,
          "items": {
            "value": "@pipeline().parameters.inputtables",
            "type": "Expression"
          },
          "activities": [
            {
              "type": "ExecutePipeline",
              "typeProperties": {
                "pipeline": {
                  "referenceName": "InnerCopyPipeline",
                  "type": "PipelineReference"
                },
                "parameters": {
                  "sourceTableName": {
                    "value": "@item().SourceTable",
                    "type": "Expression"
                  },
                  "sourceTableStructure": {
                    "value": "@item().SourceTableStructure",
                    "type": "Expression"
                  },
                  "sinkTableName": {
                    "value": "@item().DestTable",
                    "type": "Expression"
                  },
                  "sinkTableStructure": {
                    "value": "@item().DestTableStructure",
                    "type": "Expression"
                  }
                },
                "waitOnCompletion": true
              },
              "name": "ExecuteCopyPipeline"
            }
          ]
        }
      }
    ],
    "parameters": {
      "inputtables": {
        "type": "Array"
      }
    }
  }
}

```

#### <a name="inner-pipeline-definition"></a>İç işlem hattı

```json
{
  "name": "InnerCopyPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            }
          },
          "sink": {
            "type": "SqlSink"
          }
        },
        "name": "CopyActivity",
        "inputs": [
          {
            "referenceName": "sqlSourceDataset",
            "parameters": {
              "SqlTableName": {
                "value": "@pipeline().parameters.sourceTableName",
                "type": "Expression"
              },
              "SqlTableStructure": {
                "value": "@pipeline().parameters.sourceTableStructure",
                "type": "Expression"
              }
            },
            "type": "DatasetReference"
          }
        ],
        "outputs": [
          {
            "referenceName": "sqlSinkDataset",
            "parameters": {
              "SqlTableName": {
                "value": "@pipeline().parameters.sinkTableName",
                "type": "Expression"
              },
              "SqlTableStructure": {
                "value": "@pipeline().parameters.sinkTableStructure",
                "type": "Expression"
              }
            },
            "type": "DatasetReference"
          }
        ]
      }
    ],
    "parameters": {
      "sourceTableName": {
        "type": "String"
      },
      "sourceTableStructure": {
        "type": "String"
      },
      "sinkTableName": {
        "type": "String"
      },
      "sinkTableStructure": {
        "type": "String"
      }
    }
  }
}

```

#### <a name="source-dataset-definition"></a>Kaynak veri kümesi tanımı

```json
{
  "name": "sqlSourceDataset",
  "properties": {
    "type": "SqlServerTable",
    "typeProperties": {
      "tableName": {
        "value": "@dataset().SqlTableName",
        "type": "Expression"
      }
    },
    "structure": {
      "value": "@dataset().SqlTableStructure",
      "type": "Expression"
    },
    "linkedServiceName": {
      "referenceName": "sqlserverLS",
      "type": "LinkedServiceReference"
    },
    "parameters": {
      "SqlTableName": {
        "type": "String"
      },
      "SqlTableStructure": {
        "type": "String"
      }
    }
  }
}

```

#### <a name="sink-dataset-definition"></a>Havuz veri kümesi tanımı

```json
{
  "name": "sqlSinkDataSet",
  "properties": {
    "type": "AzureSqlTable",
    "typeProperties": {
      "tableName": {
        "value": "@dataset().SqlTableName",
        "type": "Expression"
      }
    },
    "structure": {
      "value": "@dataset().SqlTableStructure",
      "type": "Expression"
    },
    "linkedServiceName": {
      "referenceName": "azureSqlLS",
      "type": "LinkedServiceReference"
    },
    "parameters": {
      "SqlTableName": {
        "type": "String"
      },
      "SqlTableStructure": {
        "type": "String"
      }
    }
  }
}

```

#### <a name="master-pipeline-parameters"></a>Ana işlem hattı parametreleri
```json
{
    "inputtables": [
        {
            "SourceTable": "department",
            "SourceTableStructure": [
              {
                "name": "departmentid",
                "type": "int"
              },
              {
                "name": "departmentname",
                "type": "string"
              }
            ],
            "DestTable": "department2",
            "DestTableStructure": [
              {
                "name": "departmentid",
                "type": "int"
              },
              {
                "name": "departmentname",
                "type": "string"
              }
            ]
        }
    ]
    
}

```
## <a name="aggregating-metric-output"></a>Ölçüm çıkış toplama
ForEach bir tüm yinelemeleri çıktısını toplamak için ifade `@activity('NameofInnerActivity')`. Örneğin, "MyCopyActivity" yinelenir. bir ForEach etkinliği, söz dizimi olması: `@activity('MyCopyActivity')`. Çıkış, belirli bir yinelemeye ayrıntılarını vererek her bir öğesi ile bir dizidir.

> [!NOTE]
> Belirli bir yinelemeye hakkında ayrıntılar istiyorsanız, söz dizimi şu şekilde olacaktır: `@activity('NameofInnerActivity')[0]` son yineleme için. Sayı, dizinin belirli yinelemeye erişmek için köşeli ayraç kullanın. Belirli bir yinelemeye belirli bir özelliğine erişmek için kullanmanız gerekir: `@activity('NameofInnerActivity')[0].output` veya `@activity('NameofInnerActivity')[0].pipelineName`.

**Tüm yinelemelerin dizi çıkış ayrıntıları:**
```json
[    
    {      
        "pipelineName": "db1f7d2b-dbbd-4ea8-964e-0d9b2d3fe676",      
        "jobId": "a43766cb-ba13-4c68-923a-8349af9a76a3",      
        "activityRunId": "217526fa-0218-42f1-b85c-e0b4f7b170ce",      
        "linkedServiceName": "ADFService",      
        "status": "Succeeded",      
        "statusCode": null,      
        "output": 
            {        
                "progress": 100,        
                "loguri": null,        
                "dataRead": "6.00 Bytes",        
                "dataWritten": "6.00 Bytes",        
                "regionOrGateway": "West US",        
                "details": "Data Read: 6.00 Bytes, Written: 6.00 Bytes",        
                "copyDuration": "00:00:05",        
                "dataVolume": "6.00 Bytes",        
                "throughput": "1.16 Bytes/s",       
                 "totalDuration": "00:00:10"      
            },      
        "resumptionToken": 
            {       
                "ExecutionId": "217526fa-0218-42f1-b85c-e0b4f7b170ce",        
                "ResumptionToken": 
                    {          
                        "in progress": "217526fa-0218-42f1-b85c-e0b4f7b170ce/wu/cloud/"       
                    },        
                "ExtendedProperties": 
                    {          
                        "dataRead": "6.00 Bytes",          
                        "dataWritten": "6.00 Bytes",          
                        "regionOrGateway": "West US",          
                        "details": "Data Read: 6.00 Bytes, Written: 6.00 Bytes",          
                        "copyDuration": "00:00:05",          
                        "dataVolume": "6.00 Bytes",          
                        "throughput": "1.16 Bytes/s",          
                        "totalDuration": "00:00:10"        
                    }      
            },      
        "error": null,      
        "executionStartTime": "2017-08-01T04:17:27.5747275Z",      
        "executionEndTime": "2017-08-01T04:17:46.4224091Z",     
        "duration": "00:00:18.8476816"    
    },
    {      
        "pipelineName": "db1f7d2b-dbbd-4ea8-964e-0d9b2d3fe676",      
        "jobId": "54232-ba13-4c68-923a-8349af9a76a3",      
        "activityRunId": "217526fa-0218-42f1-b85c-e0b4f7b170ce",      
        "linkedServiceName": "ADFService",      
        "status": "Succeeded",      
        "statusCode": null,      
        "output": 
            {        
                "progress": 100,        
                "loguri": null,        
                "dataRead": "6.00 Bytes",        
                "dataWritten": "6.00 Bytes",        
                "regionOrGateway": "West US",        
                "details": "Data Read: 6.00 Bytes, Written: 6.00 Bytes",        
                "copyDuration": "00:00:05",        
                "dataVolume": "6.00 Bytes",        
                "throughput": "1.16 Bytes/s",       
                 "totalDuration": "00:00:10"      
            },      
        "resumptionToken": 
            {       
                "ExecutionId": "217526fa-0218-42f1-b85c-e0b4f7b170ce",        
                "ResumptionToken": 
                    {          
                        "in progress": "217526fa-0218-42f1-b85c-e0b4f7b170ce/wu/cloud/"       
                    },        
                "ExtendedProperties": 
                    {          
                        "dataRead": "6.00 Bytes",          
                        "dataWritten": "6.00 Bytes",          
                        "regionOrGateway": "West US",          
                        "details": "Data Read: 6.00 Bytes, Written: 6.00 Bytes",          
                        "copyDuration": "00:00:05",          
                        "dataVolume": "6.00 Bytes",          
                        "throughput": "1.16 Bytes/s",          
                        "totalDuration": "00:00:10"        
                    }      
            },      
        "error": null,      
        "executionStartTime": "2017-08-01T04:18:27.5747275Z",      
        "executionEndTime": "2017-08-01T04:18:46.4224091Z",     
        "duration": "00:00:18.8476816"    
    }
]

```

## <a name="limitations-and-workarounds"></a>Sınırlamalar ve geçici çözümler

ForEach etkinliği ve önerilen geçici çözümleri bazı sınırlamalar aşağıda verilmiştir.

| Sınırlama | Geçici çözüm |
|---|---|
| Başka bir ForEach döngüsü içinde bir ForEach döngüsü (ya da Until döngüsü) iç içe olamaz. | Burada bir iç işlem hattı ile iç içe döngü üzerinden dış işlem hattı çalıştırmasıyla dış ForEach döngüsü yinelenir iki düzeyli işlem hattı tasarım. |
| ForEach etkinliği limitinde `batchCount` 50 paralel işleme ve en fazla 100.000 öğe. | Burada dış işlem hattı ForEach etkinliği ile bir iç işlem hattı yinelenir iki düzeyli işlem hattı tasarım. |
| | |

## <a name="next-steps"></a>Sonraki adımlar
Data Factory tarafından desteklenen diğer denetim akışı etkinlikleri bakın: 

- [İşlem Hattı Yürütme Etkinliği](control-flow-execute-pipeline-activity.md)
- [Meta Veri Alma Etkinliği](control-flow-get-metadata-activity.md)
- [Arama Etkinliği](control-flow-lookup-activity.md)
- [Web etkinliği](control-flow-web-activity.md)
