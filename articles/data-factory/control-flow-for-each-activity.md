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
ms.date: 01/23/2019
ms.author: shlo
ms.openlocfilehash: c5c12a66e8f66195a096588d779648d7486ab47b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60808773"
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

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | --------
name | İçin-her etkinliğin adı. | String | Evet
türü | Ayarlanmalıdır **ForEach** | String | Evet
isSequential | Sıralı veya paralel döngü gerçekleştirilip gerçekleştirilmeyeceğini belirtir.  En fazla 20 döngü yinelemesi aynı anda paralel olarak gerçekleştirilebilir). Örneğin, bir ForEach etkinliği bir kopyalama etkinliği ile 10 farklı kaynak ve havuz veri kümeleri üzerinde yineleme varsa **isSequential** False olarak ayarlanırsa, tüm kopyaları aynı anda çalıştırılır. False varsayılan değerdir. <br/><br/> "İsSequential" False olarak ayarlarsanız, birden fazla yürütülebilir dosyaları çalıştırmasına doğru bir yapılandırma olduğundan emin olun. Aksi takdirde, bu özellik yazma çakışmalarını ücretlendirmeden kaçınmak için dikkatli kullanılmalıdır. Daha fazla bilgi için [Paralel yürütme](#parallel-execution) bölümü. | Boolean | Hayır. False varsayılan değerdir.
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

## <a name="aggregating-outputs"></a>Çıkışlar toplama

Toplama çıktılarına __foreach__ etkinlik, lütfen yazılımınız _değişkenleri_ ve _ekleme değişken_ etkinlik.

İlk olarak bildirmek bir `array` _değişkeni_ işlem hattındaki. Ardından, çağırma _ekleme değişken_ içinde her etkinlik __foreach__ döngü. Sonuç olarak, toplama, diziden alabilirsiniz.

## <a name="limitations-and-workarounds"></a>Sınırlamalar ve geçici çözümler

ForEach etkinliği ve önerilen geçici çözümleri bazı sınırlamalar aşağıda verilmiştir.

| Sınırlama | Geçici Çözüm |
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
