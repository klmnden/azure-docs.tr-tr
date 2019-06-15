---
title: Azure Cosmos DB MongoDB API'si için Azure Resource Manager şablonları
description: Azure Resource Manager şablonları oluşturmak ve MongoDB için Azure Cosmos DB API yapılandırmak için kullanın.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mjbrown
ms.openlocfilehash: 99f1e41107c277c8b3f1b21f81952d5d5cadaa29
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65968876"
---
# <a name="manage-azure-cosmos-db-mongodb-api-resources-using-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarını kullanarak Azure Cosmos DB MongoDB API kaynaklarını yönetme

## Azure Cosmos DB API MongoDB hesabı, veritabanı ve koleksiyonu oluşturma <a id="create-resource"></a>

Bir Azure Resource Manager şablonu kullanarak Azure Cosmos DB kaynaklarını oluşturun. Bu şablon, 400 RU/sn aktarım hızı ve veritabanı düzeyinde paylaşan iki koleksiyon ile MongoDB API'si için bir Azure Cosmos hesabı oluşturur. Şablon Kopyalama ve aşağıda gösterildiği gibi dağıtmak veya ziyaret [Azure hızlı başlama Galerisi](https://azure.microsoft.com/resources/templates/101-cosmosdb-mongodb/) ve Azure portalından dağıtın. Ayrıca şablonunu yerel bilgisayarınıza indirin veya yeni bir şablon oluşturmak ve ile yerel bir yol belirtin `--template-file` parametresi.

[!code-json[create-cosmos-mongo](~/quickstart-templates/101-cosmosdb-mongodb/azuredeploy.json)]

### <a name="deploy-via-azure-cli"></a>Azure CLI ile dağıtma

Azure CLI kullanarak Resource Manager şablonu dağıtmak için **kopyalama** seçin ve komut dosyası **deneyin** Azure Cloud Shell'i açmak için. Betik yapıştırmak için kabuk sağ tıklayın ve ardından **yapıştırın**:

```azurecli-interactive

read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the database name: ' databaseName
read -p 'Enter the first collection name: ' collection1Name
read -p 'Enter the second collection name: ' collection2Name

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
  --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-mongodb/azuredeploy.json \
  --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion \
  databaseName=$databaseName collection1Name=$collection1Name collection2Name=$collection2Name

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

`az cosmosdb show` Komut sağlanıp sağlanmadığını sonra yeni oluşturulan Azure Cosmos hesabı gösterir. CloudShell kullanmak yerine Azure CLI'yi yerel olarak yüklenmiş bir sürümünü kullanmak isterseniz bkz [Azure komut satırı arabirimi (CLI)](/cli/azure/) makalesi.

## İşleme (RU/s) üzerinde bir veritabanı güncelleştirmesi <a id="database-ru-update"></a>

Aşağıdaki şablonu aktarım hızını bir veritabanını güncelleştirir. Şablon Kopyalama ve aşağıda gösterildiği gibi dağıtmak veya ziyaret [Azure hızlı başlama Galerisi](https://azure.microsoft.com/resources/templates/101-cosmosdb-mongodb-database-ru-update/) ve Azure portalından dağıtın. Ayrıca şablonunu yerel bilgisayarınıza indirin veya yeni bir şablon oluşturmak ve ile yerel bir yol belirtin `--template-file` parametresi.

[!code-json[cosmosdb-mongodb-database-ru-update](~/quickstart-templates/101-cosmosdb-mongodb-database-ru-update/azuredeploy.json)]

### <a name="deploy-database-template-via-azure-cli"></a>Azure CLI aracılığıyla veritabanı şablonu dağıtma

Azure CLI kullanarak Resource Manager şablonu dağıtmak için seçebileceğiniz **deneyin** Azure Cloud Shell'i açmak için. Betik yapıştırmak için kabuk sağ tıklayın ve ardından **yapıştırın**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the database name: ' databaseName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-mongodb-database-ru-update/azuredeploy.json \
   --parameters accountName=$accountName databaseName=$databaseName throughput=$throughput
```

## İşleme (RU/s) bir koleksiyon üzerinde güncelleştir <a id="collection-ru-update"></a>

Aşağıdaki şablonu bir toplamanın güncelleştirir. Şablon Kopyalama ve aşağıda gösterildiği gibi dağıtmak veya ziyaret [Azure hızlı başlama Galerisi](https://azure.microsoft.com/resources/templates/101-cosmosdb-mongodb-collection-ru-update/) ve Azure portalından dağıtın. Ayrıca şablonunu yerel bilgisayarınıza indirin veya yeni bir şablon oluşturmak ve ile yerel bir yol belirtin `--template-file` parametresi.

[!code-json[cosmosdb-mongodb-collection-ru-update](~/quickstart-templates/101-cosmosdb-mongodb-collection-ru-update/azuredeploy.json)]

### <a name="deploy-collection-template-via-azure-cli"></a>Azure CLI aracılığıyla koleksiyon şablonu dağıtma

Azure CLI kullanarak Resource Manager şablonu dağıtmak için seçebileceğiniz **deneyin** Azure Cloud Shell'i açmak için. Betik yapıştırmak için kabuk sağ tıklayın ve ardından **yapıştırın**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the database name: ' databaseName
read -p 'Enter the collection name: ' collectionName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-mongodb-collection-ru-update/azuredeploy.json \
   --parameters accountName=$accountName databaseName=$databaseName collectionName=$collectionName throughput=$throughput
```

## <a name="next-steps"></a>Sonraki Adımlar

Bazı ek kaynaklar aşağıda verilmiştir:

- [Azure Resource Manager belgeleri](/azure/azure-resource-manager/)
- [Azure Cosmos DB kaynak sağlayıcısı şeması](/azure/templates/microsoft.documentdb/allversions)
- [Azure Cosmos DB hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Yaygın Azure Resource Manager dağıtım hatalarını giderme](../azure-resource-manager/resource-manager-common-deployment-errors.md)