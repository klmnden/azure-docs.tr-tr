---
title: Azure Data factory'de etkinlik silme | Microsoft Docs
description: Azure Data factory'de silme etkinliği ile çeşitli dosya depoları dosyaları silmeyi öğrenin.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: douglasl
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/25/2019
ms.openlocfilehash: 00658b650cdc0b1752bb9f2f205420018c1d6edd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61346352"
---
# <a name="delete-activity-in-azure-data-factory"></a>Azure Data factory'de etkinlik Sil

Sil etkinliği dosyaları silmek için Azure Data Factory'de kullanabilirsiniz veya şirket içi depolama klasörlerinden depolar veya Bulut depolama. Temizlemeye veya artık gerekli olmadığında dosyalarını arşivlemek için bu etkinliği kullanın.

> [!WARNING]
> Silinen dosyaları veya klasörleri geri yüklenemez. Sil etkinliği dosyaları veya klasörleri silme için kullanırken dikkatli olun.

## <a name="best-practices"></a>En iyi uygulamalar

Silme etkinliği kullanmak için bazı öneriler şunlardır:

-   Gelecekte geri yüklemeniz gereken durumunda silme etkinliği ile silmeden önce dosyalarınızı yedekleyin.

-   Data Factory depolama Mağazası'ndan klasörleri veya dosyaları silmek için yazma izinlerine sahip olduğundan emin olun.

-   Aynı anda yazılmakta olan dosyaları silme değil emin olun. 

-   Dosya veya klasör bir şirket içi sisteminden silmek istiyorsanız, 3.14 büyük bir sürümle şirket içinde barındırılan Integration runtime'ı kullandığınızdan emin olun.

## <a name="supported-data-stores"></a>Desteklenen veri depolar

-   [Azure Blob Depolama](connector-azure-blob-storage.md)
-   [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md)
-   [Azure Data Lake depolama 2. nesil](connector-azure-data-lake-storage.md)

### <a name="file-system-data-stores"></a>Dosya sistemi veri depoları

-   [Dosya Sistemi](connector-file-system.md)
-   [FTP](connector-ftp.md)
-   [SFTP](connector-sftp.md)
-   [Amazon S3](connector-amazon-simple-storage-service.md)

## <a name="syntax"></a>Sözdizimi

```json
{
    "name": "DeleteActivity",
    "type": "Delete",
    "typeProperties": {
        "dataset": {
            "referenceName": "<dataset name>",
            "type": "DatasetReference"
        },
        "recursive": true/false,
        "maxConcurrentConnections": <number>,
        "enableLogging": true/false,
        "logStorageSettings": {
            "linkedServiceName": {
                "referenceName": "<name of linked service>",
                "type": "LinkedServiceReference"
            },
            "path": "<path to save log file>"
        }
    }
}
```

## <a name="type-properties"></a>Tür özellikleri

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| Veri kümesi | Hangi dosya veya klasör, silinecek belirlemek için veri kümesi başvurusu sağlar | Evet |
| özyinelemeli | Dosya silinmiş yinelemeli olarak alt klasörleri veya yalnızca belirtilen klasör olup olmadığını gösterir.  | Hayır. Varsayılan değer: `false`. |
| MaxConcurrentConnections | Depolama deposu, klasör veya dosyaları silmek için eşzamanlı olarak bağlanmak için bağlantı sayısı.   |  Hayır. Varsayılan değer: `1`. |
| EnableLogging | Silinmiş bir klasör veya dosya adlarını kaydedin gerekip gerekmediğini gösterir. TRUE ise silme etkinliği davranışlarını günlük dosyasını okuyarak izleyebilmeniz için daha fazla günlük dosyasını kaydetmek için bir depolama hesabı sağlamak gerekir. | Hayır |
| logStorageSettings | Yalnızca uygun olduğunda enablelogging = true.<br/><br/>Silme etkinliği tarafından silinmiş klasör veya dosya adlarını içeren bir günlük dosyasını kaydetmek istediğiniz bir grup olabilir depolama özellik belirtilmiş. | Hayır |
| linkedServiceName | Yalnızca uygun olduğunda enablelogging = true.<br/><br/>Bağlı hizmetin adı [Azure depolama](connector-azure-blob-storage.md#linked-service-properties), [Azure Data Lake depolama Gen1](connector-azure-data-lake-store.md#linked-service-properties), veya [Azure Data Lake depolama Gen2](connector-azure-data-lake-storage.md#linked-service-properties) günlük dosyasının depolanacağı klasörü içeren veya dosya adları Silme etkinliği tarafından silindi. | Hayır |
| path | Yalnızca uygun olduğunda enablelogging = true.<br/><br/>Günlük dosyasını depolama hesabınızdaki kaydetmek istediğiniz yola gözatın. Bir yol belirtmezseniz, hizmet sizin için bir kapsayıcı oluşturur. | Hayır |

## <a name="monitoring"></a>İzleme

Burada görmek ve sonuçları Sil etkinliğinin izleme iki yerde vardır: 
-   Sil etkinliğinin çıktısı.
-   Günlük dosyasından.

### <a name="sample-output-of-the-delete-activity"></a>Örnek çıktı silme etkinliği

```json
{ 
  "datasetName": "AmazonS3",
  "type": "AmazonS3Object",
  "prefix": "test",
  "bucketName": "adf",
  "recursive": true,
  "isWildcardUsed": false,
  "maxConcurrentConnections": 2,  
  "filesDeleted": 4,
  "logPath": "https://sample.blob.core.windows.net/mycontainer/5c698705-a6e2-40bf-911e-e0a927de3f07",
  "effectiveIntegrationRuntime": "MyAzureIR (West Central US)",
  "executionDuration": 650
}
```

### <a name="sample-log-file-of-the-delete-activity"></a>Örnek günlük dosya silme etkinliği

| Ad | Kategori | Durum | Hata |
|:--- |:--- |:--- |:--- |
| Test1/yyy.JSON | Dosya | Silinen |  |
| Test2/hello789.txt | Dosya | Silinen |  |
| Test2/test3/hello000.txt | Dosya | Silinen |  |
| test2/test3/zzz.json | Dosya | Silinen |  |

## <a name="examples-of-using-the-delete-activity"></a>Silme etkinliği kullanma örnekleri

### <a name="delete-specific-folders-or-files"></a>Belirli klasörleri veya dosyaları silme

Deponun aşağıdaki klasör yapısına sahiptir:

Kök /<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_1 /<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2 txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_2 /<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4 txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_1 /<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6 txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;7.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_2 /<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8 txt

Şimdi veri kümesinden farklı özellik değeri birleşimi tarafından klasör veya dosyaları silmek için Sil etkinliği ve silme etkinliği kullanıyorsanız:

| folderPath (veri kümesinden) | Dosya adı (veri kümesinden) | özyinelemeli (etkinliğinden Sil) | Çıktı |
|:--- |:--- |:--- |:--- |
| Kök / Folder_A_2 | NULL | False | Kök /<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_1 /<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2 txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_2 /<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>4 txt</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>5.csv</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_1 /<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6 txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;7.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_2 /<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8 txt |
| Kök / Folder_A_2 | NULL | True | Kök /<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_1 /<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2 txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;<strike>Folder_A_2 /</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>4 txt</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>5.csv</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>Folder_B_1 /</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>6 txt</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>7.csv</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>Folder_B_2 /</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>8 txt</strike> |
| Kök / Folder_A_2 | *.txt | False | Kök /<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_1 /<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2 txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_2 /<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>4 txt</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_1 /<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6 txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;7.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_2 /<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8 txt |
| Kök / Folder_A_2 | *.txt | True | Kök /<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_1 /<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2 txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_2 /<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>4 txt</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_1 /<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>6 txt</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;7.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_2 /<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>8 txt</strike> |

### <a name="periodically-clean-up-the-time-partitioned-folder-or-files"></a>Zaman bölümlenmiş klasör veya dosyaları düzenli aralıklarla temizlemek

Saat bölümlenmiş klasör veya dosyaları düzenli aralıklarla temizlemek için bir işlem hattı oluşturabilirsiniz.  Örneğin, klasör yapısı olarak benzerdir: `/mycontainer/2018/12/14/*.csv`.  Zamanlama tetikleyicisi hangi klasöre veya dosyalara her işlem hattı silinmesi gerektiğini belirlemek için ADF sistem değişkeninden yararlanabilirsiniz. 

#### <a name="sample-pipeline"></a>Örnek ardışık düzen

```json
{
    "name": "cleanup_time_partitioned_folder",
    "properties": {
        "activities": [
            {
                "name": "DeleteOneFolder",
                "type": "Delete",
                "policy": {
                    "timeout": "7.00:00:00",
                    "retry": 0,
                    "retryIntervalInSeconds": 30,
                    "secureOutput": false,
                    "secureInput": false
                },
                "typeProperties": {
                    "dataset": {
                        "referenceName": "PartitionedFolder",
                        "type": "DatasetReference",
                        "parameters": {
                            "TriggerTime": {
                                "value": "@formatDateTime(pipeline().parameters.TriggerTime, 'yyyy/MM/dd')",
                                "type": "Expression"
                            }
                        }
                    },
                    "recursive": true,
                    "logStorageSettings": {
                        "linkedServiceName": {
                            "referenceName": "BloblinkedService",
                            "type": "LinkedServiceReference"
                        },
                        "path": "mycontainer/log"
                    },
                    "enableLogging": true
                }
            }
        ],
        "parameters": {
            "TriggerTime": {
                "type": "String"
            }
        }
    },
    "type": "Microsoft.DataFactory/factories/pipelines"
}
```

#### <a name="sample-dataset"></a>Örnek veri kümesi

```json
{
    "name": "PartitionedFolder",
    "properties": {
        "linkedServiceName": {
            "referenceName": "BloblinkedService",
            "type": "LinkedServiceReference"
        },
        "parameters": {
            "TriggerTime": {
                "type": "String"
            }
        },
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": {
                "value": "@concat('mycontainer/',dataset().TriggerTime)",
                "type": "Expression"
            }
        }
    },
    "type": "Microsoft.DataFactory/factories/datasets"
}
```

#### <a name="sample-trigger"></a>Örnek tetikleyicisi

```json
{
    "name": "DailyTrigger",
    "properties": {
        "runtimeState": "Started",
        "pipelines": [
            {
                "pipelineReference": {
                    "referenceName": "cleanup_time_partitioned_folder",
                    "type": "PipelineReference"
                },
                "parameters": {
                    "TriggerTime": "@trigger().scheduledTime"
                }
            }
        ],
        "type": "ScheduleTrigger",
        "typeProperties": {
            "recurrence": {
                "frequency": "Day",
                "interval": 1,
                "startTime": "2018-12-13T00:00:00.000Z",
                "timeZone": "UTC",
                "schedule": {
                    "minutes": [
                        59
                    ],
                    "hours": [
                        23
                    ]
                }
            }
        }
    }
}
```

### <a name="clean-up-the-expired-files-that-were-last-modified-before-201811"></a>2018.1.1 önce en son değiştirilen süresi sona eren dosyaların Temizle

Dosya öznitelik filtresi yararlanarak eski veya süresi dolmuş dosyaları temizlemek için bir işlem hattı oluşturabilirsiniz: "Veri"LastModified.  

#### <a name="sample-pipeline"></a>Örnek ardışık düzen

```json
{
    "name": "CleanupExpiredFiles",
    "properties": {
        "activities": [
            {
                "name": "DeleteFilebyLastModified",
                "type": "Delete",
                "policy": {
                    "timeout": "7.00:00:00",
                    "retry": 0,
                    "retryIntervalInSeconds": 30,
                    "secureOutput": false,
                    "secureInput": false
                },
                "typeProperties": {
                    "dataset": {
                        "referenceName": "BlobFilesLastModifiedBefore201811",
                        "type": "DatasetReference"
                    },
                    "recursive": true,
                    "logStorageSettings": {
                        "linkedServiceName": {
                            "referenceName": "BloblinkedService",
                            "type": "LinkedServiceReference"
                        },
                        "path": "mycontainer/log"
                    },
                    "enableLogging": true
                }
            }
        ]
    }
}
```

#### <a name="sample-dataset"></a>Örnek veri kümesi

```json
{
    "name": "BlobFilesLastModifiedBefore201811",
    "properties": {
        "linkedServiceName": {
            "referenceName": "BloblinkedService",
            "type": "LinkedServiceReference"
        },
        "type": "AzureBlob",
        "typeProperties": {
            "fileName": "*",
            "folderPath": "mycontainer",
            "modifiedDatetimeEnd": "2018-01-01T00:00:00.000Z"
        }
    }
}
```

### <a name="move-files-by-chaining-the-copy-activity-and-the-delete-activity"></a>Kopyalama etkinliği ve silme etkinliği zincirleme dosyaları taşıma

Bir dosyayı dosya kopyalamak için kopyalama etkinliği ve bir işlem hattındaki bir dosyayı silmek için Sil etkinliği kullanarak taşıyabilirsiniz.  Birden çok dosyayı taşımak istediğinizde, GetMetadata etkinliği + filtre etkinliği + Foreach etkinliği + kopyalama etkinliği kullanma + etkinlik aşağıdaki örnekte olduğu gibi silin:

> [!NOTE]
> Yalnızca bir klasör yolu içeren bir veri kümesini tanımlama ve ardından bir kopyalama etkinliği'ni kullanarak tüm klasör taşımak istiyorsanız ve aynı veri kümesine bir klasörü temsil eden başvurmak için silme etkinliği çok dikkatli olmanız gerekir. Yeni dosyalar klasörüne silme işlemi ve kopyalama işlemi arasında gelen olmayacaktır emin olmak sahip olmasıdır.  Klasör kullandığınızda, kopyalama etkinliği yalnızca kopyalama işi tamamlandı ancak silme etkinliği değil stared şu anda gelen yeni dosyalar varsa destinati için kopyalanmaz bu gelen dosya silme etkinliği sileceğini mümkündür henüz klasörün tamamını silerek şirket. 

#### <a name="sample-pipeline"></a>Örnek ardışık düzen

```json
{
    "name": "MoveFiles",
    "properties": {
        "activities": [
            {
                "name": "GetFileList",
                "type": "GetMetadata",
                "typeProperties": {
                    "dataset": {
                        "referenceName": "OneSourceFolder",
                        "type": "DatasetReference"
                    },
                    "fieldList": [
                        "childItems"
                    ]
                }
            },
            {
                "name": "FilterFiles",
                "type": "Filter",
                "dependsOn": [
                    {
                        "activity": "GetFileList",
                        "dependencyConditions": [
                            "Succeeded"
                        ]
                    }
                ],
                "typeProperties": {
                    "items": {
                        "value": "@activity('GetFileList').output.childItems",
                        "type": "Expression"
                    },
                    "condition": {
                        "value": "@equals(item().type, 'File')",
                        "type": "Expression"
                    }
                }
            },
            {
                "name": "ForEachFile",
                "type": "ForEach",
                "dependsOn": [
                    {
                        "activity": "FilterFiles",
                        "dependencyConditions": [
                            "Succeeded"
                        ]
                    }
                ],
                "typeProperties": {
                    "items": {
                        "value": "@activity('FilterFiles').output.value",
                        "type": "Expression"
                    },
                    "batchCount": 20,
                    "activities": [
                        {
                            "name": "CopyAFile",
                            "type": "Copy",
                            "policy": {
                                "timeout": "7.00:00:00",
                                "retry": 0,
                                "retryIntervalInSeconds": 30,
                                "secureOutput": false,
                                "secureInput": false
                            },
                            "typeProperties": {
                                "source": {
                                    "type": "BlobSource",
                                    "recursive": false
                                },
                                "sink": {
                                    "type": "BlobSink"
                                },
                                "enableStaging": false,
                                "dataIntegrationUnits": 0
                            },
                            "inputs": [
                                {
                                    "referenceName": "OneSourceFile",
                                    "type": "DatasetReference",
                                    "parameters": {
                                        "path": "myFolder",
                                        "filename": {
                                            "value": "@item().name",
                                            "type": "Expression"
                                        }
                                    }
                                }
                            ],
                            "outputs": [
                                {
                                    "referenceName": "OneDestinationFile",
                                    "type": "DatasetReference",
                                    "parameters": {
                                        "DestinationFileName": {
                                            "value": "@item().name",
                                            "type": "Expression"
                                        }
                                    }
                                }
                            ]
                        },
                        {
                            "name": "DeleteAFile",
                            "type": "Delete",
                            "dependsOn": [
                                {
                                    "activity": "CopyAFile",
                                    "dependencyConditions": [
                                        "Succeeded"
                                    ]
                                }
                            ],
                            "policy": {
                                "timeout": "7.00:00:00",
                                "retry": 0,
                                "retryIntervalInSeconds": 30,
                                "secureOutput": false,
                                "secureInput": false
                            },
                            "typeProperties": {
                                "dataset": {
                                    "referenceName": "OneSourceFile",
                                    "type": "DatasetReference",
                                    "parameters": {
                                        "path": "myFolder",
                                        "filename": {
                                            "value": "@item().name",
                                            "type": "Expression"
                                        }
                                    }
                                },
                                "logStorageSettings": {
                                    "linkedServiceName": {
                                        "referenceName": "BloblinkedService",
                                        "type": "LinkedServiceReference"
                                    },
                                    "path": "Container/log"
                                },
                                "enableLogging": true
                            }
                        }
                    ]
                }
            }
        ]
    }
}
```

#### <a name="sample-datasets"></a>Örnek veri kümeleri

Veri kümesi, GetMetadata etkinliği tarafından kullanılan dosya listesini numaralandırır.

```json
{
    "name": "OneSourceFolder",
    "properties": {
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "type": "AzureBlob",
        "typeProperties": {
            "fileName": "",
            "folderPath": "myFolder"
        }
    }
}
```

Kopyalama etkinliği ve silme etkinliği tarafından kullanılan veri kaynağı için veri kümesi.

```json
{
    "name": "OneSourceFile",
    "properties": {
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "parameters": {
            "path": {
                "type": "String"
            },
            "filename": {
                "type": "String"
            }
        },
        "type": "AzureBlob",
        "typeProperties": {
            "fileName": {
                "value": "@dataset().filename",
                "type": "Expression"
            },
            "folderPath": {
                "value": "@{dataset().path}",
                "type": "Expression"
            }
        }
    }
}
```

Kopyalama etkinliği tarafından kullanılan verileri hedef veri kümesi.

```json
{
    "name": "OneDestinationFile",
    "properties": {
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "parameters": {
            "DestinationFileName": {
                "type": "String"
            }
        },
        "type": "AzureBlob",
        "typeProperties": {
            "fileName": {
                "value": "@dataset().DestinationFileName",
                "type": "Expression"
            },
            "folderPath": "mycontainer/dest"
        }
    }
}
```
## <a name="known-limitation"></a>Bilinen sınırlama

-   Silme etkinliği silme joker karakteri tarafından açıklanan klasörlerin listesini desteklemez.

-   Dosya öznitelik Filtresi kullanırken: modifiedDatetimeStart ve modifiedDatetimeEnd Silinecek dosyalar seçmek için "dosya adı" ayarladığınızdan emin olun: "*" veri kümesindeki.

## <a name="next-steps"></a>Sonraki adımlar

Azure Data Factory'de dosyaları taşıma hakkında daha fazla bilgi edinin.

-   [Azure Data Factory'de veri aracı kopyalayın](copy-data-tool.md)