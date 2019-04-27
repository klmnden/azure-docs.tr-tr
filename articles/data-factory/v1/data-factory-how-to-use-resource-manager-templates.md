---
title: Resource Manager şablonlarını kullanma Data factory'de | Microsoft Docs
description: Oluşturmak ve Data Factory varlıkları oluşturmak için Azure Resource Manager şablonlarını kullanma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.assetid: 37724021-f55f-4e85-9206-6d4a48bda3d8
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: ca8b3930b9d9f708d83dc760be3ee89737b074dc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60583375"
---
# <a name="use-templates-to-create-azure-data-factory-entities"></a>Azure Data Factory varlıkları oluşturmak için şablonları kullanma
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. 

## <a name="overview"></a>Genel Bakış
Azure Data Factory veri tümleştirme gereksinimleriniz için kullanılırken kendinizi bulabilirsiniz farklı ortamlar veya aynı çözüm içinde art arda aynı görevi uygulama genelinde aynı düzeni yeniden kullanılıyor. Şablonları uygulamak ve bu senaryolar kolay bir şekilde yönetmenize yardımcı olur. Azure Data factory'de şablonları çalışmalarında ve yineleme gerektiren senaryolar için idealdir.

Bir kuruluş dünya genelindeki 10 üretim tesisleri sahip olduğu durum göz önünde bulundurun. Her tesis günlüklerinden ayrı şirket içi SQL Server veritabanında depolanır. Geçici analiz için bulutta bir tek veri ambarı oluşturmak şirket istemektedir. Ayrıca aynı mantığı ancak geliştirme, test ve üretim ortamları için farklı yapılandırmalara sahip olmasını istiyor.

Bu durumda, bir görev aynı ortamdaki ancak farklı değerlerle her üretim tesisindeki için 10 veri fabrikaları üzerinde yinelenmesi gerekir. Aslında, **yineleme** mevcuttur. Şablon oluşturma, bu genel akış (aynı etkinlikler bölümünde her veri fabrikasının sahip başka bir deyişle, işlem hatları) soyutlamasıdır sağlar, ancak her üretim tesisindeki için ayrı bir parametre dosyası kullanılmaktadır.

Bu 10 veri fabrikaları farklı ortamlarda birden çok kez dağıtmak kuruluş istediği gibi Ayrıca, şablon bu kullanabileceğiniz **çalışmalarında** yararlanarak geliştirme için ayrı parametre dosyaları tarafından test edin ve Üretim ortamları.

## <a name="templating-with-azure-resource-manager"></a>Azure Resource Manager şablonu oluşturma
[Azure Resource Manager şablonları](../../azure-resource-manager/resource-group-overview.md#template-deployment) şablon Azure Data factory'de elde etmek için harika bir yoludur. Resource Manager şablonları, bir JSON dosyası altyapısını ve yapılandırmasını kullanarak Azure çözümünüzün tanımlayın. Azure Resource Manager şablonları tüm/çoğu Azure hizmetleriyle çalıştığından, kolayca Azure varlıklarınızı tüm kaynakları yönetmek için yaygın olarak kullanılabilmesi için. Bkz: [Azure Resource Manager şablonları yazma](../../azure-resource-manager/resource-group-authoring-templates.md) Resource Manager şablonları hakkında daha fazla genel bilgi edinmek için.

## <a name="tutorials"></a>Öğreticiler
Resource Manager şablonları kullanarak Data Factory varlıkları oluşturmak adım adım yönergeler için aşağıdaki öğreticilere bakın:

* [Öğretici: Azure Resource Manager şablonu kullanarak veri kopyalamak için bir işlem hattı oluşturma](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
* [Öğretici: Azure Resource Manager şablonu kullanarak verileri işlemek için bir işlem hattı oluşturma](data-factory-build-your-first-pipeline.md)

## <a name="data-factory-templates-on-github"></a>Veri Fabrikası şablonlarını github'da
Aşağıdaki Azure hızlı başlangıç şablonları GitHub üzerinde göz atın:

* [Verileri Azure Blob depolama alanından Azure SQL veritabanı'na kopyalamak için bir veri fabrikası oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy)
* [Azure HDInsight kümesinde Hive etkinliği ile bir veri fabrikası oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation)
* [Verileri Azure BLOB'ları için Salesforce kopyalamak için bir veri fabrikası oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy)
* [Zincir etkinlikler veri fabrikası oluşturma: verileri Azure BLOB'ları için bir FTP sunucusundan kopyalar, verileri dönüştürmek için isteğe bağlı HDInsight kümesinde bir hive betiği çağırır ve sonucu Azure SQL veritabanı'na kopyalar](https://github.com/Azure/azure-quickstart-templates/tree/master/201-data-factory-ftp-hive-blob)

Azure Data Factory şablonlarınızı paylaşmak çekinmeyin [Azure Hızlı Başlangıç](https://azure.microsoft.com/documentation/templates/). Başvurmak [katkıda bulunma Kılavuzu](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE) bu depo paylaşılabilir bir şablon geliştirme sırasında.

Aşağıdaki bölümler, Data Factory kaynakları Resource Manager şablonunda tanımlama hakkında ayrıntılı bilgi sağlar.

## <a name="defining-data-factory-resources-in-templates"></a>Data Factory kaynaklarını tanımlama
Bir veri fabrikasını tanımlamaya yönelik üst düzey şablon şöyledir:

```JSON
"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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

### <a name="define-linked-services"></a>Bağlı hizmetler tanımlar

```JSON
"type": "linkedservices",
"name": "[variables('<LinkedServiceName>')]",
"apiVersion": "2015-10-01",
"dependsOn": [ "[variables('<dataFactoryName>')]" ],
"properties": {
    ...
}
```

Bkz: [depolama bağlı hizmeti](data-factory-azure-blob-connector.md#azure-storage-linked-service) veya [işlem bağlı Hizmetleri](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) dağıtmak istediğiniz belirli bir bağlı hizmet JSON özellikleri hakkında ayrıntılar için. "DependsOn" parametresi, ilgili veri fabrikasının adını belirtir. Azure depolama için bağlı hizmet tanımlama örneği aşağıdaki JSON tanımında gösterilmektedir:

### <a name="define-datasets"></a>Veri kümeleri tanımlayın

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
Başvurmak [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) dağıtmak istediğiniz belirli bir veri kümesi türü için JSON özellikleri hakkında ayrıntılar için. Bağlı hizmeti Not "dependsOn" parametresi, depolama ve karşılık gelen bir veri fabrikası adını belirtir. Aşağıdaki JSON tanımında Azure blob depolama, veri kümesi türü tanımlayan bir örnek gösterilir:

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

### <a name="define-pipelines"></a>İşlem hatları tanımlayın

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

Başvurmak [işlem hatları tanımlama](data-factory-create-pipelines.md#pipeline-json) dağıtmak istediğiniz etkinlikleri ve belirli bir işlem hattı tanımlamak için JSON özellikleri hakkında ayrıntılar için. "DependsOn" parametresi veri fabrikasının adını belirtir ve varsa buna karşılık gelen Hizmetleri ya da veri kümeleri bağlı unutmayın. Aşağıdaki JSON kod parçacığında Azure Blob depolama alanından Azure SQL veritabanına veri kopyalayan bir işlem hattının bir örnek gösterilir:

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
## <a name="parameterizing-data-factory-template"></a>Data Factory şablon kümesini parametreleştirme
Kümesini parametreleştirme en iyi uygulamalar için bkz: [Azure Resource Manager şablonları oluşturmaya yönelik en iyi uygulamalar](../../azure-resource-manager/resource-manager-template-best-practices.md). Özellikle değişkenler yerine kullanılabilir, genel olarak, parametre kullanımı, küçültülmesine. Yalnızca aşağıdaki senaryolarda parametreleri belirtin:

* Ayarları ortamı tarafından farklılık gösterir (örnek: geliştirme, test ve üretim)
* Gizli anahtarları (parolalar gibi)

Gizli diziler çekme gerekiyorsa [Azure anahtar kasası](../../key-vault/key-vault-overview.md) şablonları kullanarak Azure Data Factory varlıklarını dağıtırken belirtin **anahtar kasası** ve **gizli dizi adı** gösterildiği gibi Aşağıdaki örnekte:

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
> Var olan veri fabrikaları şablonları dışarı aktarma şu anda henüz desteklenmiyor olsa da, çalışması devam ediyor.
>
>
