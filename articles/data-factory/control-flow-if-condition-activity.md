---
title: Varsa Condition etkinliği Azure Data factory'de | Microsoft Docs
description: IF koşulu etkinliği, bir koşula göre işlem akışını denetlemenize olanak tanır.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
ms.openlocfilehash: 52f96b8fc2a1288c652169817a3a73d7b26caac9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66153477"
---
# <a name="if-condition-activity-in-azure-data-factory"></a>Azure Data factory'de etkinlik koşul
If Koşulu etkinliği, programlama dilerindeki If deyimiyle aynı işlevselliği sağlar. Koşul `true` sonucunu verdiğinde bir dizi etkinliği, `false` sonucu verdiğinde ise başka bir dizi etkinliği değerlendirmeye alır. 

## <a name="syntax"></a>Sözdizimi

```json

{
    "name": "<Name of the activity>",
    "type": "IfCondition",
    "typeProperties": {
            "expression": {
            "value": "<expression that evaluates to true or false>",
            "type": "Expression"
            },

            "ifTrueActivities": [
            {
                "<Activity 1 definition>"
            },
            {
                "<Activity 2 definition>"
            },
            {
                "<Activity N definition>"
            }
        ],

        "ifFalseActivities": [
            {
                "<Activity 1 definition>"
            },
            {
                "<Activity 2 definition>"
            },
            {
                "<Activity N definition>"
            }
            ]
    }
}
```

## <a name="type-properties"></a>Tür özellikleri

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | --------
name | IF-condition etkinliği adı. | String | Evet
türü | Ayarlanmalıdır **IfCondition** | String | Evet
İfade | True veya false değerlendirmelidir ifadesi | İfade sonucu ile Boole türü | Evet
ifTrueActivities | İfade sonucunu verdiğinde çalıştırılan etkinlik kümesini `true`. | Dizi | Evet
ifFalseActivities | İfade sonucunu verdiğinde çalıştırılan etkinlik kümesini `false`. | Dizi | Evet

## <a name="example"></a>Örnek
Bu örnekteki işlem hattı, verileri bir girdi klasöründen bir çıktı klasörüne kopyalar. Çıkış klasörü, işlem hattı parametre değeri tarafından belirlenir: routeSelection. RouteSelection değeri true ise, veriler için outputPath1 kopyalanır. Ve routeSelection değeri false ise, veriler için outputPath2 kopyalanır. 

> [!NOTE]
> Bu bölümde, JSON tanımları ve işlem hattını çalıştırmak için örnek PowerShell komutları sağlanır. Azure PowerShell ve JSON tanımları'ı kullanarak Data Factory işlem hattı oluşturmak için adım adım yönergeler içeren bir kılavuz için bkz. [öğretici: Azure PowerShell kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-powershell.md).

### <a name="pipeline-with-if-condition-activity-adfv2quickstartpipelinejson"></a>IF-Condition etkinliği (c:\adfv2quickstartpsh) içeren işlem hattı

```json
{
    "name": "Adfv2QuickStartPipeline",
    "properties": {
        "activities": [
            {
                "name": "MyIfCondition",
                "type": "IfCondition",
                "typeProperties": {
                    "expression":  {
                        "value":  "@bool(pipeline().parameters.routeSelection)", 
                        "type": "Expression"
                     },
                                           
                    "ifTrueActivities": [
                        {
                            "name": "CopyFromBlobToBlob1",
                            "type": "Copy",
                            "inputs": [
                                {
                                    "referenceName": "BlobDataset",
                                    "parameters": {
                                        "path": "@pipeline().parameters.inputPath"
                                    },
                                    "type": "DatasetReference"
                                }
                            ],
                            "outputs": [
                                {
                                    "referenceName": "BlobDataset",
                                    "parameters": {
                                        "path": "@pipeline().parameters.outputPath1"
                                    },
                                    "type": "DatasetReference"
                                }
                            ],
                            "typeProperties": {
                                "source": {
                                    "type": "BlobSource"
                                },
                                "sink": {
                                    "type": "BlobSink"
                                }
                            }
                        }                                   
                    ],
                    "ifFalseActivities": [
                        {
                            "name": "CopyFromBlobToBlob2",
                            "type": "Copy",
                            "inputs": [
                                {
                                    "referenceName": "BlobDataset",
                                    "parameters": {
                                        "path": "@pipeline().parameters.inputPath"
                                    },
                                    "type": "DatasetReference"
                                }
                            ],
                            "outputs": [
                                {
                                    "referenceName": "BlobDataset",
                                    "parameters": {
                                        "path": "@pipeline().parameters.outputPath2"
                                    },
                                    "type": "DatasetReference"
                                }
                            ],
                            "typeProperties": {
                                "source": {
                                    "type": "BlobSource"
                                },
                                "sink": {
                                    "type": "BlobSink"
                                }
                            }
                        }
                    ]                    
                }
            }
        ],
        "parameters": {
            "inputPath": {
                "type": "String"
            },
            "outputPath1": {
                "type": "String"
            },
            "outputPath2": {
                "type": "String"
            },
            "routeSelection": {
                "type": "String"
            }                        
        }
    }
}
```

İfade için başka bir örnek verilmiştir: 

```json
"expression":  {
    "value":  "@pipeline().parameters.routeSelection == 1", 
    "type": "Expression"
}
```


### <a name="azure-storage-linked-service-azurestoragelinkedservicejson"></a>Azure depolama bağlı hizmeti (C:\adfv2tutorial)

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": {
                "value": "DefaultEndpointsProtocol=https;AccountName=<Azure Storage account name>;AccountKey=<Azure Storage account key>",
                "type": "SecureString"
            }
        }
    }
}
```

### <a name="parameterized-azure-blob-dataset-blobdatasetjson"></a>Parametreli Azure Blob veri kümesi (C:\adfv2quickstartpsh)
İşlem hattı ayarlar **folderPath** ya da değerine **outputPath1** veya **outputPath2** işlem hattı parametresi. 

```json
{
    "name": "BlobDataset",
    "properties": {
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": {
                "value": "@{dataset().path}",
                "type": "Expression"
            }
        },
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "parameters": {
            "path": {
                "type": "String"
            }
        }
    }
}
```

### <a name="pipeline-parameter-json-pipelineparametersjson"></a>İşlem hattı parametresi JSON (C:\adfv2quickstartpsh)

```json
{
    "inputPath": "adftutorial/input",
    "outputPath1": "adftutorial/outputIf",
    "outputPath2": "adftutorial/outputElse",
    "routeSelection": "false"
}
```

### <a name="powershell-commands"></a>PowerShell komutları

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu komutlar, JSON dosyaları klasöre kaydettiğiniz varsayın: C:\ADF. 

```powershell
Connect-AzAccount
Select-AzSubscription "<Your subscription name>"

$resourceGroupName = "<Resource Group Name>"
$dataFactoryName = "<Data Factory Name. Must be globally unique>";
Remove-AzDataFactoryV2 $dataFactoryName -ResourceGroupName $resourceGroupName -force


Set-AzDataFactoryV2 -ResourceGroupName $resourceGroupName -Location "East US" -Name $dataFactoryName
Set-AzDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureStorageLinkedService" -DefinitionFile "C:\ADF\AzureStorageLinkedService.json"
Set-AzDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "BlobDataset" -DefinitionFile "C:\ADF\BlobDataset.json"
Set-AzDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "Adfv2QuickStartPipeline" -DefinitionFile "C:\ADF\Adfv2QuickStartPipeline.json"
$runId = Invoke-AzDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName "Adfv2QuickStartPipeline" -ParameterFile C:\ADF\PipelineParameters.json
while ($True) {
    $run = Get-AzDataFactoryV2PipelineRun -ResourceGroupName $resourceGroupName -DataFactoryName $DataFactoryName -PipelineRunId $runId

    if ($run) {
        if ($run.Status -ne 'InProgress') {
            Write-Host "Pipeline run finished. The status is: " $run.Status -foregroundcolor "Yellow"
            $run
            break
        }
        Write-Host  "Pipeline is running...status: InProgress" -foregroundcolor "Yellow"
    }

    Start-Sleep -Seconds 30
}
Write-Host "Activity run details:" -foregroundcolor "Yellow"
$result = Get-AzDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)
$result

Write-Host "Activity 'Output' section:" -foregroundcolor "Yellow"
$result.Output -join "`r`n"

Write-Host "\nActivity 'Error' section:" -foregroundcolor "Yellow"
$result.Error -join "`r`n"
```

## <a name="next-steps"></a>Sonraki adımlar
Data Factory tarafından desteklenen diğer denetim akışı etkinlikleri bakın: 

- [İşlem Hattı Yürütme Etkinliği](control-flow-execute-pipeline-activity.md)
- [Her etkinlik için](control-flow-for-each-activity.md)
- [Meta Veri Alma Etkinliği](control-flow-get-metadata-activity.md)
- [Arama Etkinliği](control-flow-lookup-activity.md)
- [Web etkinliği](control-flow-web-activity.md)
