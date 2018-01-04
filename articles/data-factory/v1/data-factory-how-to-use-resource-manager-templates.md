---
title: "Veri Fabrikası'nda kullanmak Resource Manager şablonları | Microsoft Docs"
description: "Oluşturma ve Data Factory varlıklarını oluşturmak için Azure Resource Manager şablonlarını kullanma hakkında bilgi edinin."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: 
ms.assetid: 37724021-f55f-4e85-9206-6d4a48bda3d8
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2017
ms.author: shlo
robots: noindex
ms.openlocfilehash: e0f4a284a46ba56ba4e3229a72e99efef0cf9dc2
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2017
---
# <a name="use-templates-to-create-azure-data-factory-entities"></a>Azure Data Factory varlıklarını oluşturmak için şablon kullanın
> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. 

## <a name="overview"></a>Genel Bakış
Kendinizi bulabilirsiniz veri tümleştirme ihtiyaçlarınız için Azure Data Factory kullanırken farklı ortamlar veya aynı çözüm içinde art arda aynı görevi uygulama arasında aynı düzeni yeniden kullanma. Şablonları uygulamak ve bu senaryolar kolay bir şekilde yönetmenize yardımcı olur. Azure Data Factory şablonlarında yeniden kullanılırlığı ve yineleme gerektiren senaryolar için idealdir.

Bir kuruluş dünya genelindeki 10 üretim bitkilerin sahip olduğu durum göz önünde bulundurun. Her tesis günlüklerinden ayrı şirket içi SQL Server veritabanında depolanır. Tek bir veri ambarına geçici analiz için bulutta oluşturmak şirket ister. Ayrıca aynı mantığı ancak geliştirme, test ve üretim ortamları için farklı yapılandırmalara sahip olmasını istiyor.

Bu durumda, bir görev aynı ortamda ancak farklı değerleri olan her üretim tesis için 10 veri fabrikaları arasında yinelenmesi gerekir. Uygulamada **yineleme** mevcuttur. Şablon bu genel akış (diğer bir deyişle, her veri fabrikası aynı etkinlikleri sahip ardışık) soyutlaması sağlar, ancak her üretim tesis için ayrı bir parametre dosyası kullanır.

Bu 10 veri fabrikaları farklı ortamlar genelinde birden çok kez dağıtmak kuruluş istediği gibi Ayrıca, şablonları bu kullanabilirsiniz **yeniden kullanılırlığı** geliştirme, test ve üretim ortamları için ayrı parametre dosyaları aynı kullanarak.

## <a name="templating-with-azure-resource-manager"></a>Azure Resource Manager ile şablonu oluşturma
[Azure Resource Manager şablonları](../../azure-resource-manager/resource-group-overview.md#template-deployment) Azure veri fabrikası'nda şablon elde etmek için kullanışlı bir yoludur. Resource Manager şablonları bir JSON dosyası aracılığıyla altyapısı ve Azure çözümünüzü yapılandırmasını tanımlayın. Azure Resource Manager şablonları tüm/çoğu Azure hizmetler ile çalışmak için bu yaygın olarak kolayca Azure varlıklarınızın tüm kaynakları yönetmek için kullanılabilir. Bkz: [Azure Resource Manager şablonları yazma](../../azure-resource-manager/resource-group-authoring-templates.md) Resource Manager şablonları hakkında daha fazla genel bilgi edinmek için.

## <a name="tutorials"></a>Öğreticiler
Aşağıdaki öğreticileri Resource Manager şablonları kullanarak Data Factory varlıklarını oluşturmak adım adım yönergeler için bkz:

* [Öğretici: Azure Resource Manager şablonu kullanarak veri kopyalamak için bir işlem hattı oluşturma](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
* [Öğretici: Azure Resource Manager şablonu kullanarak verileri işlemek için bir işlem hattı oluşturma](data-factory-build-your-first-pipeline.md)

## <a name="data-factory-templates-on-github"></a>Veri Fabrikası şablonları github'da
Github'da aşağıdaki Azure hızlı başlangıç şablonlarını gözden geçirin:

* [Verileri Azure Blob depolama alanından Azure SQL veritabanına kopyalamak için bir veri fabrikası oluşturun](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy)
* [Azure Hdınsight kümesinde Hive etkinliği ile bir veri fabrikası oluşturun](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation)
* [Azure BLOB'larını Salesforce verileri kopyalamak için bir veri fabrikası oluşturun](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy)
* [Etkinlikler zincir bir veri fabrikası oluşturun: verileri Azure BLOB'larını bir FTP sunucusundan kopyalar, verileri dönüştürmek için isteğe bağlı Hdınsight kümesinde bir hive betiği çağırır ve sonucu Azure SQL veritabanına kopyalar](https://github.com/Azure/azure-quickstart-templates/tree/master/201-data-factory-ftp-hive-blob)

Azure Data Factory şablonlarınızı paylaşmak çekinmeyin [Azure Hızlı Başlangıç](https://azure.microsoft.com/documentation/templates/). Başvurmak [katkı Kılavuzu](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE) bu havuzda paylaşılan şablonları geliştirme sırasında.

Aşağıdaki bölümler, Veri Fabrikası Kaynakları bir Resource Manager şablonunda tanımlama hakkında ayrıntılı bilgi sağlar.

## <a name="defining-data-factory-resources-in-templates"></a>Veri Fabrikası Kaynakları Tanımlama
Veri Fabrikası tanımlamak için üst düzey şablon şöyledir:

```JSON
"$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
"contentVersion": "1.0.0.0",
"parameters": { ...
},
"variables": { ...
},
"resources": [
{
    "name": "[parameters('dataFactoryName')]",
    "apiVersion": "[variables('apiVersion')]",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "westus",
    "resources": [
    { "type": "linkedservices",
        ...
    },
    {"type": "datasets",
        ...
    },
    {"type": "dataPipelines",
        ...
    }
}
```

### <a name="define-data-factory"></a>Veri fabrikası tanımlama
Resource Manager şablonunda bir veri fabrikasını aşağıdaki örnekte gösterildiği gibi tanımlayın:

```JSON
"resources": [
{
    "name": "[variables('<mydataFactoryName>')]",
    "apiVersion": "2015-10-01",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "East US"
}
```
DataFactoryName "değişkenler" olarak tanımlanır:

```JSON
"dataFactoryName": "[concat('<myDataFactoryName>', uniqueString(resourceGroup().id))]",
```

### <a name="define-linked-services"></a>Bağlı hizmetler tanımlayın

```JSON
"type": "linkedservices",
"name": "[variables('<LinkedServiceName>')]",
"apiVersion": "2015-10-01",
"dependsOn": [ "[variables('<dataFactoryName>')]" ],
"properties": {
    ...
}
```

Bkz: [depolama bağlantılı hizmeti](data-factory-azure-blob-connector.md#azure-storage-linked-service) veya [işlem bağlı Hizmetleri](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) dağıtmak istediğiniz belirli bağlantılı hizmeti için JSON özellikleri hakkında ayrıntılı bilgi için. "DependsOn" parametresi karşılık gelen veri fabrikasının adını belirtir. Azure Storage için bağlı hizmet tanımlama örneği aşağıdaki JSON tanımında gösterilir:

### <a name="define-datasets"></a>Veri kümelerini tanımlayın

```JSON
"type": "datasets",
"name": "[variables('<myDatasetName>')]",
"dependsOn": [
    "[variables('<dataFactoryName>')]",
    "[variables('<myDatasetLinkedServiceName>')]"
],
"apiVersion": "2015-10-01",
"properties": {
    ...
}
```
Başvurmak [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) dağıtmak istediğiniz belirli dataset türü için JSON özellikleri hakkında ayrıntılı bilgi için. Bağlantılı hizmetinin Not "dependsOn" parametresi depolama alanını ve karşılık gelen veri fabrikası adını belirtir. Azure blob depolama dataset türünü tanımlayan bir örneği aşağıdaki JSON tanımında gösterilir:

```JSON
"type": "datasets",
"name": "[variables('storageDataset')]",
"dependsOn": [
    "[variables('dataFactoryName')]",
    "[variables('storageLinkedServiceName')]"
],
"apiVersion": "2015-10-01",
"properties": {
"type": "AzureBlob",
"linkedServiceName": "[variables('storageLinkedServiceName')]",
"typeProperties": {
    "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
    "fileName": "[parameters('sourceBlobName')]",
    "format": {
        "type": "TextFormat"
    }
},
"availability": {
    "frequency": "Hour",
    "interval": 1
}
```

### <a name="define-pipelines"></a>Ardışık Düzen tanımlayın

```JSON
"type": "dataPipelines",
"name": "[variables('<mypipelineName>')]",
"dependsOn": [
    "[variables('<dataFactoryName>')]",
    "[variables('<inputDatasetLinkedServiceName>')]",
    "[variables('<outputDatasetLinkedServiceName>')]",
    "[variables('<inputDataset>')]",
    "[variables('<outputDataset>')]"
],
"apiVersion": "2015-10-01",
"properties": {
    activities: {
        ...
    }
}
```

Başvurmak [ardışık düzen tanımlama](data-factory-create-pipelines.md#pipeline-json) belirli ardışık düzen ve dağıtmak istediğiniz etkinlikleri tanımlamak için JSON özellikleri hakkında ayrıntılı bilgi için. Veri Fabrikası adı "dependsOn" parametresi belirtir ve ilgili tüm hizmetleri veya veri kümeleri bağlı unutmayın. Azure Blob depolama alanından Azure SQL veritabanına verileri kopyalayan bir işlem hattı örneği aşağıdaki JSON parçacığında gösterilir:

```JSON
"type": "datapipelines",
"name": "[variables('pipelineName')]",
"dependsOn": [
    "[variables('dataFactoryName')]",
    "[variables('azureStorageLinkedServiceName')]",
    "[variables('azureSqlLinkedServiceName')]",
    "[variables('blobInputDatasetName')]",
    "[variables('sqlOutputDatasetName')]"
],
"apiVersion": "2015-10-01",
"properties": {
    "activities": [
    {
        "name": "CopyFromAzureBlobToAzureSQL",
        "description": "Copy data frm Azure blob to Azure SQL",
        "type": "Copy",
        "inputs": [
            {
                "name": "[variables('blobInputDatasetName')]"
            }
        ],
        "outputs": [
            {
                "name": "[variables('sqlOutputDatasetName')]"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "BlobSource"
            },
            "sink": {
                "type": "SqlSink",
                "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
            },
            "translator": {
                "type": "TabularTranslator",
                "columnMappings": "Column0:FirstName,Column1:LastName"
            }
        },
        "Policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 3,
            "timeout": "01:00:00"
        }
    }
    ],
    "start": "2016-10-03T00:00:00Z",
    "end": "2016-10-04T00:00:00Z"
}
```
## <a name="parameterizing-data-factory-template"></a>Veri Fabrikası şablon kümesini parametreleştirme
Kümesini parametreleştirme en iyi uygulamalar için bkz: [en iyi uygulamalar Azure Resource Manager şablonları oluşturmak için](../../azure-resource-manager/resource-manager-template-best-practices.md). Özellikle değişkenleri yerine kullanılabilir genel olarak, parametre kullanımı, en aza. Yalnızca aşağıdaki senaryolarda parametreler sağlar:

* Ayarlar ortamı tarafından değişir (örnek: geliştirme, test ve üretim)
* Gizli (parolalar gibi)

Gizli gelen çekme gerekiyorsa [Azure anahtar kasası](../../key-vault/key-vault-get-started.md) şablonları kullanarak Azure Data Factory varlıklarını dağıtırken belirtin **anahtar kasası** ve **gizli anahtar adı** aşağıdaki örnekte gösterildiği gibi:

```JSON
"parameters": {
    "storageAccountKey": {
        "reference": {
            "keyVault": {
                "id":"/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.KeyVault/vaults/<keyVaultName>",
             },
            "secretName": "<secretName>"
           },
       },
       ...
}
```

> [!NOTE]
> Mevcut veri fabrikaları şablonları verme şu anda henüz desteklenmiyor olsa da, çalışır durumdur.
>
>
