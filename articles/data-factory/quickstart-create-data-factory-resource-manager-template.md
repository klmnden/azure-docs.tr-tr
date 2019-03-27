---
title: Resource Manager şablonu kullanarak Azure veri fabrikası oluşturma | Microsoft Docs
description: Bu öğreticide, bir Azure Resource Manager şablonu kullanarak örnek bir Azure Data Factory işlem hattı oluşturacaksınız.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: quickstart
ms.date: 02/20/2019
author: gauravmalhot
ms.author: gamal
manager: craigg
ms.openlocfilehash: 2b25dff29563dcf44077465f3e563d04f04b3119
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58483123"
---
# <a name="tutorial-create-an-azure-data-factory-using-azure-resource-manager-template"></a>Öğretici: Azure Resource Manager şablonu kullanarak bir Azure veri fabrikası oluşturma

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-build-your-first-pipeline-using-arm.md)
> * [Geçerli sürüm](quickstart-create-data-factory-resource-manager-template.md)

Bu hızlı başlangıçta, Azure Resource Manager şablonu kullanarak bir Azure veri fabrikasını nasıl oluşturacağınız ve izleyeceğiniz açıklanmaktadır. Bu veri fabrikasında oluşturduğunuz işlem hattı, verileri Azure blob depolama alanındaki bir klasörden başka bir klasöre **kopyalar**. Hakkında bir öğretici için **dönüştürme** Azure Data Factory kullanarak verileri görmek [Öğreticisi: Spark kullanarak verileri dönüştürme](transform-data-using-spark.md).

> [!NOTE]
> Bu makale, Data Factory hizmetine ayrıntılı giriş bilgileri sağlamaz. Azure Data Factory hizmetine giriş bilgileri için bkz. [Azure Data Factory'ye giriş](introduction.md).

[!INCLUDE [data-factory-quickstart-prerequisites](../../includes/data-factory-quickstart-prerequisites.md)]

### <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/install-Az-ps) konusundaki yönergeleri izleyerek en güncel Azure PowerShell modüllerini yükleyin.

## <a name="resource-manager-templates"></a>Resource Manager şablonları

Azure Resource Manager şablonları hakkında genel bir bilgi almak için bkz. [Azure Resource Manager Şablonları Yazma](../azure-resource-manager/resource-group-authoring-templates.md).

Aşağıdaki bölümde, öğreticiyi hızlıca geçip şablonu test etmeniz için Data Factory varlıklarını tanımlamaya yönelik tam bir Resource Manager şablonu verilmektedir. Her bir Data Factory varlığının nasıl tanımlandığını anlamak için [Şablondaki Data Factory varlıkları](#data-factory-entities-in-the-template) bölümüne bakın.

Data Factory kaynaklarını bir şablonda özelliklerini ve JSON söz dizimi hakkında bilgi edinmek için bkz. [Microsoft.DataFactory kaynak türleri](/azure/templates/microsoft.datafactory/allversions).

## <a name="data-factory-json"></a>Data Factory JSON

**C:\ADFTutorial** klasöründe aşağıdaki içeriğe sahip **ADFTutorialARM.json** adlı bir JSON dosyası oluşturun:

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "parameters": {
        "dataFactoryName": {
            "type": "string",
            "metadata": {
                "description": "Name of the data factory. Must be globally unique."
            }
        },
        "dataFactoryLocation": {
            "type": "string",
            "allowedValues": [
                "East US",
                "East US 2",
                "West Europe"
            ],
            "defaultValue": "East US",
            "metadata": {
                "description": "Location of the data factory. Currently, only East US, East US 2, and West Europe are supported. "
            }
        },
        "storageAccountName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Azure storage account that contains the input/output data."
            }
        },
        "storageAccountKey": {
            "type": "securestring",
            "metadata": {
                "description": "Key for the Azure storage account."
            }
        },
        "blobContainer": {
            "type": "string",
            "metadata": {
                "description": "Name of the blob container in the Azure Storage account."
            }
        },
        "inputBlobFolder": {
            "type": "string",
            "metadata": {
                "description": "The folder in the blob container that has the input file."
            }
        },
        "inputBlobName": {
            "type": "string",
            "metadata": {
                "description": "Name of the input file/blob."
            }
        },
        "outputBlobFolder": {
            "type": "string",
            "metadata": {
                "description": "The folder in the blob container that will hold the transformed data."
            }
        },
        "outputBlobName": {
            "type": "string",
            "metadata": {
                "description": "Name of the output file/blob."
            }
        },
        "triggerStartTime": {
            "type": "string",
            "metadata": {
                "description": "Start time for the trigger."
            }
        },
        "triggerEndTime": {
            "type": "string",
            "metadata": {
                "description": "End time for the trigger."
            }
        }
    },
    "variables": {
        "azureStorageLinkedServiceName": "ArmtemplateStorageLinkedService",
        "inputDatasetName": "ArmtemplateTestDatasetIn",
        "outputDatasetName": "ArmtemplateTestDatasetOut",
        "pipelineName": "ArmtemplateSampleCopyPipeline",
        "triggerName": "ArmTemplateTestTrigger"
    },
    "resources": [{
        "name": "[parameters('dataFactoryName')]",
        "apiVersion": "2018-06-01",
        "type": "Microsoft.DataFactory/factories",
        "location": "[parameters('dataFactoryLocation')]",
        "identity": {
            "type": "SystemAssigned"
        },
        "resources": [{
                "type": "linkedservices",
                "name": "[variables('azureStorageLinkedServiceName')]",
                "dependsOn": [
                    "[parameters('dataFactoryName')]"
                ],
                "apiVersion": "2018-06-01",
                "properties": {
                    "type": "AzureStorage",
                    "description": "Azure Storage linked service",
                    "typeProperties": {
                        "connectionString": {
                            "value": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]",
                            "type": "SecureString"
                        }
                    }
                }
            },
            {
                "type": "datasets",
                "name": "[variables('inputDatasetName')]",
                "dependsOn": [
                    "[parameters('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2018-06-01",
                "properties": {
                    "type": "AzureBlob",
                    "typeProperties": {
                        "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'), '/')]",
                        "fileName": "[parameters('inputBlobName')]"
                    },
                    "linkedServiceName": {
                        "referenceName": "[variables('azureStorageLinkedServiceName')]",
                        "type": "LinkedServiceReference"
                    }
                }
            },
            {
                "type": "datasets",
                "name": "[variables('outputDatasetName')]",
                "dependsOn": [
                    "[parameters('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2018-06-01",
                "properties": {
                    "type": "AzureBlob",
                    "typeProperties": {
                        "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'), '/')]",
                        "fileName": "[parameters('outputBlobName')]"
                    },
                    "linkedServiceName": {
                        "referenceName": "[variables('azureStorageLinkedServiceName')]",
                        "type": "LinkedServiceReference"
                    }
                }
            },
            {
                "type": "pipelines",
                "name": "[variables('pipelineName')]",
                "dependsOn": [
                    "[parameters('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]",
                    "[variables('inputDatasetName')]",
                    "[variables('outputDatasetName')]"
                ],
                "apiVersion": "2018-06-01",
                "properties": {
                    "activities": [{
                        "type": "Copy",
                        "typeProperties": {
                            "source": {
                                "type": "BlobSource"
                            },
                            "sink": {
                                "type": "BlobSink"
                            }
                        },
                        "name": "MyCopyActivity",
                        "inputs": [{
                            "referenceName": "[variables('inputDatasetName')]",
                            "type": "DatasetReference"
                        }],
                        "outputs": [{
                            "referenceName": "[variables('outputDatasetName')]",
                            "type": "DatasetReference"
                        }]
                    }]
                }
            },
            {
                "type": "triggers",
                "name": "[variables('triggerName')]",
                "dependsOn": [
                    "[parameters('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]",
                    "[variables('inputDatasetName')]",
                    "[variables('outputDatasetName')]",
                    "[variables('pipelineName')]"
                ],
                "apiVersion": "2018-06-01",
                "properties": {
                    "type": "ScheduleTrigger",
                    "typeProperties": {
                        "recurrence": {
                            "frequency": "Hour",
                            "interval": 1,
                            "startTime": "[parameters('triggerStartTime')]",
                            "endTime": "[parameters('triggerEndTime')]",
                            "timeZone": "UTC"
                        }
                    },
                    "pipelines": [{
                        "pipelineReference": {
                            "type": "PipelineReference",
                            "referenceName": "ArmtemplateSampleCopyPipeline"
                        },
                        "parameters": {}
                    }]
                }
            }
        ]
    }]
}
```

## <a name="parameters-json"></a>Parametreler JSON

Azure Resource Manager şablonuna yönelik parametreleri içeren **ADFTutorialARM-Parameters.json** adlı bir JSON dosyası oluşturun.

> [!IMPORTANT]
> - Bu parametre dosyasında **storageAccountName** ve **storageAccountKey** parametreleri için Azure Storage hesabınızın adını ve anahtarını belirtin. Adftutorial kapsayıcısını oluşturdunuz ve örnek dosyayı (emp.txt) bu Azure blob depolama alanındaki giriş klasörüne yüklediniz.
> - **dataFactoryName** parametresi için veri fabrikasına ait genel olarak benzersiz bir ad belirtin. Örneğin: ARMTutorialFactoryJohnDoe11282017.
> - **triggerStartTime** için geçerli tarihi şu biçimde belirtin: `2017-11-28T00:00:00`.
> - **triggerEndTime** için sonraki günü şu biçimde belirtin: `2017-11-29T00:00:00`. Ayrıca, geçerli UTC saatini işaretleyip bitiş saati için sonraki bir veya iki saati belirtebilirsiniz. Örneğin, geçerli UTC saati gece 01:32 ise bitiş tarihi olarak `2017-11-29:03:00:00` değerini belirtin. Bu durumda tetikleyici, işlem hattını iki kez (gece 2 ve 3’te) çalıştırır.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "dataFactoryName": {
      "value": "<datafactoryname>"
    },
    "dataFactoryLocation": {
      "value": "East US"
    },
    "storageAccountName": {
      "value": "<yourstorageaccountname>"
    },
    "storageAccountKey": {
      "value": "<yourstorageaccountkey>"
    },
    "blobContainer": {
      "value": "adftutorial"
    },
    "inputBlobFolder": {
      "value": "input"
    },
    "inputBlobName": {
      "value": "emp.txt"
    },
    "outputBlobFolder": {
      "value": "output"
    },
    "outputBlobName": {
      "value": "emp.txt"
    },
    "triggerStartTime": {
        "value": "2017-11-28T00:00:00. Set to today"
    },
    "triggerEndTime": {
        "value": "2017-11-29T00:00:00. Set to tomorrow"
    }
  }
}
```

> [!IMPORTANT]
> Aynı Data Factory JSON şablonuyla birlikte kullanabileceğiniz geliştirme, test ve üretim ortamları için ayrı parametre JSON dosyalarına sahip olabilirsiniz. Bir Power Shell komut dosyası kullanarak, bu ortamlarda Data Factory varlıklarını dağıtmayı otomatik hale getirebilirsiniz.

## <a name="deploy-data-factory-entities"></a>Data Factory varlıklarını dağıtma

PowerShell’de, bu hızlı başlangıcın önceki bölümlerinde oluşturduğunuz Resource Manager şablonunu kullanarak Data Factory varlıklarını dağıtmak için aşağıdaki komutu çalıştırın.

```powershell
New-AzResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFTutorial\ADFTutorialARM.json -TemplateParameterFile C:\ADFTutorial\ADFTutorialARM-Parameters.json
```

Aşağıdaki örneğe benzer bir çıktı görürsünüz:

```console
DeploymentName          : MyARMDeployment
ResourceGroupName       : ADFTutorialResourceGroup
ProvisioningState       : Succeeded
Timestamp               : 11/29/2017 3:11:13 AM
Mode                    : Incremental
TemplateLink            :
Parameters              :
                          Name                 Type            Value
                          ===============      ============    ==========
                          dataFactoryName      String          <data factory name>
                          dataFactoryLocation  String          East US
                          storageAccountName   String          <storage account name>
                          storageAccountKey    SecureString
                          blobContainer        String          adftutorial
                          inputBlobFolder      String          input
                          inputBlobName        String          emp.txt
                          outputBlobFolder     String          output
                          outputBlobName       String          emp.txt
                          triggerStartTime     String          11/29/2017 12:00:00 AM
                          triggerEndTime       String          11/29/2017 4:00:00 AM

Outputs                 :
DeploymentDebugLogLevel :
```

## <a name="start-the-trigger"></a>Tetikleyiciyi başlatma

Şablon aşağıdaki Data Factory varlıklarını dağıtır:

- Azure Storage bağlı hizmeti
- Azure Blob veri kümeleri (girdi ve çıktı)
- Kopyalama etkinliği içeren işlem hattı
- İşlem hattını tetikleyen tetikleyici

Dağıtılan tetikleyici durdurulmuş durumdadır. Tetikleyici başlatmanın yollarından biri kullanılacak **başlangıç AzDataFactoryV2Trigger** PowerShell cmdlet'i. Aşağıdaki yordamda ayrıntılı adımlar verilmektedir:

1. PowerShell penceresinde kaynak grubunun adını tutacak bir değişken oluşturun. Aşağıdaki komutu PowerShell penceresine kopyalayıp ENTER tuşuna basın. Bir yeni AzResourceGroupDeployment komutu için farklı bir kaynak grubu adı belirttiyseniz değeri burada güncelleştirin.

    ```powershell
    $resourceGroupName = "ADFTutorialResourceGroup"
    ```
2. Veri fabrikasının adını tutacak bir değişken oluşturun. ADFTutorialARM-Parameters.json dosyasında belirttiğiniz adın aynısını belirtin.

    ```powershell
    $dataFactoryName = "<yourdatafactoryname>"
    ```
3. Tetikleyicinin adı için bir değişken ayarlayın. Tetikleyicinin adı, Resource Manager şablon dosyasında (ADFTutorialARM.json) sabit kodlanmıştır.

    ```powershell
    $triggerName = "ArmTemplateTestTrigger"
    ```
4. Veri fabrikanızın ve tetikleyicinin adını belirttikten sonra aşağıdaki PowerShell komutunu çalıştırarak **tetikleyicinin durumunu** alın:

    ```powershell
    Get-AzDataFactoryV2Trigger -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Name $triggerName
    ```

    Örnek çıktı aşağıdaki gibidir:

    ```json
    TriggerName       : ArmTemplateTestTrigger
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ARMFactory1128
    Properties        : Microsoft.Azure.Management.DataFactory.Models.ScheduleTrigger
    RuntimeState      : Stopped
    ```
    
    Tetikleyicinin çalışma zamanı durumunun **Durduruldu** olduğuna dikkat edin.
5. **Tetikleyiciyi başlatın**. Tetikleyici, şablonda tanımlanan işlem hattını belirtilen saatte çalıştırır. Diğer bir deyişle, bu komutu öğleden sonra 2:25’te yürüttüyseniz, tetikleyici işlem hattını ilk kez öğleden sonra 3’te çalıştırır. Ardından, saatlik belirttiğiniz bitiş zamanı tetikleyicisinin kadar işlem hattını çalışır.

    ```powershell
    Start-AzDataFactoryV2Trigger -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -TriggerName $triggerName
    ```
    
    Örnek çıktı aşağıdaki gibidir:
    
    ```console
    Confirm
    Are you sure you want to start trigger 'ArmTemplateTestTrigger' in data factory 'ARMFactory1128'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y
    True
    ```
6. Get-AzDataFactoryV2Trigger komutunu tekrar çalıştırarak tetikleyicinin başlatıldığını onaylayın.

    ```powershell
    Get-AzDataFactoryV2Trigger -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -TriggerName $triggerName
    ```
    
    Örnek çıktı aşağıdaki gibidir:
    
    ```console
    TriggerName       : ArmTemplateTestTrigger
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ARMFactory1128
    Properties        : Microsoft.Azure.Management.DataFactory.Models.ScheduleTrigger
    RuntimeState      : Started
    ```

## <a name="monitor-the-pipeline"></a>İşlem hattını izleme

1. [Azure portalında](https://portal.azure.com/) oturum açtıktan sonra **Tüm hizmetler**’e tıklayın, **veri fa** gibi bir anahtar sözcükle arama yapın ve **Veri fabrikaları**’nı seçin.

    ![Veri fabrikaları menüsüne göz atma](media/quickstart-create-data-factory-resource-manager-template/browse-data-factories-menu.png)

2. **Veri Fabrikaları** sayfasında, oluşturduğunuz veri fabrikasına tıklayın. Gerekirse, veri fabrikanızın adıyla listeyi filtreleyin.

    ![Veri fabrikası seçme](media/quickstart-create-data-factory-resource-manager-template/select-data-factory.png)

3. Veri fabrikası sayfasında **İzleme ve Yönetme** kutucuğuna tıklayın.

    ![İzleme ve yönetme kutucuğu](media/quickstart-create-data-factory-resource-manager-template/monitor-manage-tile.png)

4. **Veri Tümleştirme Uygulaması**, web tarayıcısının ayrı bir sekmesinde açılmalıdır. İzleme sekmesi etkin değilse **izleme sekmesine** geçiş yapın. İşlem hattı çalıştırmasının bir **zamanlayıcı tetikleyicisi** ile tetiklendiğine dikkat edin.

    ![İşlem hattı çalıştırmasını izleme](media/quickstart-create-data-factory-resource-manager-template/monitor-pipeline-run.png)

    > [!IMPORTANT]
    > İşlem hattı çalıştırmaları yalnızca saat başı çalıştığını görürsünüz (örneğin: 4'te, 5'te, 6 AM, vs.). Zaman sonraki saate ulaştığında listeyi yenilemek için araç çubuğunda **Yenile**’ye tıklayın.

5. **Eylemler** sütunundaki bağlantıya tıklayın.

    ![İşlem hattı eylemleri bağlantısı](media/quickstart-create-data-factory-resource-manager-template/pipeline-actions-link.png)

6. İşlem hattı çalıştırmasıyla ilişkili etkinlik çalıştırmalarını görürsünüz. Bu hızlı başlangıçta, işlem hattı yalnızca bir etkinlik türü içerir: Kopyalayın. Bu nedenle, bu etkinliğe ait bir çalıştırma görürsünüz.

    ![Etkinlik çalıştırmaları](media/quickstart-create-data-factory-resource-manager-template/activity-runs.png)
7. **Çıktı** sütunu altındaki bağlantıya tıklayın. Kopyalama işleminin çıktısını bir **Çıktı** penceresinde görürsünüz. Tam çıktıyı görmek için ekranı kapla düğmesine tıklayın. Ekranı kaplayan çıktı penceresini veya çıktıyı kapatabilirsiniz.

    ![Çıktı penceresi](media/quickstart-create-data-factory-resource-manager-template/output-window.png)
8. Başarılı/başarısız çalıştırma gördüğünüzde tetikleyiciyi durdurun. Tetikleyici, işlem hattını saatte bir kez çalıştırır. İşlem hattı her çalıştırma için aynı dosyayı girdi klasöründen çıktı klasörüne kopyalar. Tetikleyiciyi durdurmak için PowerShell penceresinde aşağıdaki komutu çalıştırın.
    
    ```powershell
    Stop-AzDataFactoryV2Trigger -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Name $triggerName
    ```

[!INCLUDE [data-factory-quickstart-verify-output-cleanup.md](../../includes/data-factory-quickstart-verify-output-cleanup.md)]

## <a name="data-factory-entities-in-the-template"></a> Varlıklar için JSON tanımları

Aşağıdaki Data Factory varlıkları JSON şablonunda tanımlanır:

- [Azure Storage bağlı hizmeti](#azure-storage-linked-service)
- [Azure Blob girdi veri kümesi](#azure-blob-input-dataset)
- [Azure Blob çıktı veri kümesi](#azure-blob-output-dataset)
- [Kopyalama etkinliği içeren bir veri işlem hattı](#data-pipeline)
- [Tetikleyici](#trigger)

#### <a name="azure-storage-linked-service"></a>Azure Storage bağlı hizmeti

AzureStorageLinkedService, Azure depolama hesabınızı veri fabrikasına bağlar. Bir kapsayıcı oluşturup verileri ön koşulların parçası olarak bu depolama hesabına yüklediniz. Bu bölümde Azure depolama hesabınızın adını ve anahtarını belirtirsiniz. Bir Azure Storage bağlı hizmetini tanımlamak için kullanılan JSON özelliklerine ilişkin ayrıntılar için bkz. [Azure Storage bağlı hizmeti](connector-azure-blob-storage.md#linked-service-properties).

```json
{
    "type": "linkedservices",
    "name": "[variables('azureStorageLinkedServiceName')]",
    "dependsOn": [
        "[parameters('dataFactoryName')]"
    ],
    "apiVersion": "2018-06-01",
    "properties": {
        "type": "AzureStorage",
        "description": "Azure Storage linked service",
        "typeProperties": {
            "connectionString": {
                "value": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]",
                "type": "SecureString"
            }
        }
    }
}
```

ConnectionString, storageAccountName ve storageAccountKey parametrelerini kullanır. Bu parametrelerin değerleri bir yapılandırma dosyası kullanılarak geçirilir. Tanım ayrıca şu değişkenleri kullanır: şablonda tanımlanan azureStorageLinkedService ve dataFactoryName.

#### <a name="azure-blob-input-dataset"></a>Azure blob girdi veri kümesi

Azure Depolama bağlı hizmeti, Data Factory hizmetinin Azure depolama hesabınıza bağlanmak için çalışma zamanında kullandığı bağlantı dizesini belirtir. Azure blob veri kümesi tanımında blob kapsayıcısını, klasörü ve giriş verilerini içeren dosyanın adını belirtirsiniz. Bir Azure Blob veri kümesini tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılar için bkz. [Azure Blob veri kümesi özellikleri](connector-azure-blob-storage.md#dataset-properties).

```json
{
    "type": "datasets",
    "name": "[variables('inputDatasetName')]",
    "dependsOn": [
        "[parameters('dataFactoryName')]",
        "[variables('azureStorageLinkedServiceName')]"
    ],
    "apiVersion": "2018-06-01",
    "properties": {
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'), '/')]",
            "fileName": "[parameters('inputBlobName')]"
        },
        "linkedServiceName": {
            "referenceName": "[variables('azureStorageLinkedServiceName')]",
            "type": "LinkedServiceReference"
        }
    }
},
```

#### <a name="azure-blob-output-dataset"></a>Azure blob çıktı veri kümesi

Azure Blob Depolamada girdi klasöründen kopyalanmış verileri tutan klasörün adını belirtin. Bir Azure Blob veri kümesini tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılar için bkz. [Azure Blob veri kümesi özellikleri](connector-azure-blob-storage.md#dataset-properties).

```json
{
    "type": "datasets",
    "name": "[variables('outputDatasetName')]",
    "dependsOn": [
        "[parameters('dataFactoryName')]",
        "[variables('azureStorageLinkedServiceName')]"
    ],
    "apiVersion": "2018-06-01",
    "properties": {
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'), '/')]",
            "fileName": "[parameters('outputBlobName')]"
        },
        "linkedServiceName": {
            "referenceName": "[variables('azureStorageLinkedServiceName')]",
            "type": "LinkedServiceReference"
        }
    }
}
```

#### <a name="data-pipeline"></a>Veri işlem hattı

Verileri bir Azure blob veri kümesinden başka bir Azure blob veri kümesine kopyalayan bir işlem hattı tanımlayın. Bu örnekte bir işlem hattı tanımlamak için kullanılan JSON öğelerinin açıklamaları için bkz. [İşlem Hattı JSON](concepts-pipelines-activities.md#pipeline-json).

```json
{
    "type": "pipelines",
    "name": "[variables('pipelineName')]",
    "dependsOn": [
        "[parameters('dataFactoryName')]",
        "[variables('azureStorageLinkedServiceName')]",
        "[variables('inputDatasetName')]",
        "[variables('outputDatasetName')]"
    ],
    "apiVersion": "2018-06-01",
    "properties": {
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "name": "MyCopyActivity",
            "inputs": [{
                "referenceName": "[variables('inputDatasetName')]",
                "type": "DatasetReference"
            }],
            "outputs": [{
                "referenceName": "[variables('outputDatasetName')]",
                "type": "DatasetReference"
            }]
        }]
    }
}
```

#### <a name="trigger"></a>Tetikleyici

İşlem hattını saatte bir kez çalıştıran bir tetikleyici tanımlayın. Dağıtılan tetikleyici durdurulmuş durumdadır. Kullanarak tetikleyiciyi başlatın **başlangıç AzDataFactoryV2Trigger** cmdlet'i. Tetikleyiciler hakkında daha fazla bilgi için [İşlem hattı yürütme ve tetikleyicileri](concepts-pipeline-execution-triggers.md#triggers) makalesine bakın.

```json
{
    "type": "triggers",
    "name": "[variables('triggerName')]",
    "dependsOn": [
        "[parameters('dataFactoryName')]",
        "[variables('azureStorageLinkedServiceName')]",
        "[variables('inputDatasetName')]",
        "[variables('outputDatasetName')]",
        "[variables('pipelineName')]"
    ],
    "apiVersion": "2018-06-01",
    "properties": {
        "type": "ScheduleTrigger",
        "typeProperties": {
            "recurrence": {
                "frequency": "Hour",
                "interval": 1,
                "startTime": "2017-11-28T00:00:00",
                "endTime": "2017-11-29T00:00:00",
                "timeZone": "UTC"
            }
        },
        "pipelines": [{
            "pipelineReference": {
                "type": "PipelineReference",
                "referenceName": "ArmtemplateSampleCopyPipeline"
            },
            "parameters": {}
        }]
    }
}
```

## <a name="reuse-the-template"></a>Şablonu yeniden kullanma

Bu öğreticide, Data Factory varlıkları tanımlamaya yönelik bir şablon ve parametrelerin değerlerini geçirmeye yönelik bir şablon oluşturdunuz. Data Factory varlıklarını farklı ortamlara dağıtmak üzere aynı şablonu kullanmak için, her bir ortama yönelik bir parametre dosyası oluşturur ve bu ortama dağıtırken kullanırsınız.

Örnek:

```powershell
New-AzResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Dev.json

New-AzResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Test.json

New-AzResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Production.json
```

Birinci komut geliştirme ortamına, ikinci komut test ortamına ve üçüncü komut üretim ortamına yönelik parametre dosyasını kullanır.

Ayrıca tekrarlanan görevleri gerçekleştirmek için şablonu yeniden kullanabilirsiniz. Örneğin, aynı mantığı uygulamasına karşın her veri fabrikasının farklı Azure depolama hesaplarını kullandığı bir veya daha fazla işlem hattı ile çok sayıda veri fabrikası oluşturun. Bu senaryoda, aynı şablonu veri fabrikaları oluşturmaya yönelik farklı parametre dosyaları ile aynı ortamda (geliştirme, test veya üretim) kullanırsınız.

## <a name="next-steps"></a>Sonraki adımlar

Bu örnekteki işlem hattı, verileri bir konumdan Azure blob depolama alanındaki başka bir konuma kopyalar. Daha fazla senaryoda Data Factory’yi kullanma hakkında bilgi almak için [öğreticileri](tutorial-copy-data-dot-net.md) okuyun.
