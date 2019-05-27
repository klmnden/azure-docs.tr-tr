---
title: Azure Cosmos DB Cassandra API'si için Azure Resource Manager şablonları
description: Azure Resource Manager şablonları oluşturmak ve Azure Cosmos DB Cassandra API'SİNİN yapılandırmak için kullanın.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mjbrown
ms.openlocfilehash: db754adbe60acfa155400910c47de556db793eef
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65968904"
---
# <a name="manage-azure-cosmos-db-cassandra-api-resources-using-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarını kullanarak Azure Cosmos DB Cassandra API'SİNİN kaynaklarını yönetme

## Azure Cosmos hesabı, anahtar alanı ve tablo oluşturma <a id="create-resource"></a>

Bir Azure Resource Manager şablonu kullanarak Azure Cosmos DB kaynaklarını oluşturun. Bu şablon, 400 RU/sn aktarım hızı keyspace düzeyinde paylaşan iki tablo ile Cassandra API'si için bir Azure Cosmos hesabı oluşturur. Şablon Kopyalama ve aşağıda gösterildiği gibi dağıtmak veya ziyaret [Azure hızlı başlama Galerisi](https://azure.microsoft.com/resources/templates/101-cosmosdb-cassandra/) ve Azure portalından dağıtın. Ayrıca şablonunu yerel bilgisayarınıza indirin veya yeni bir şablon oluşturmak ve ile yerel bir yol belirtin `--template-file` parametresi.

[!code-json[create-cosmos-Cassandra](~/quickstart-templates/101-cosmosdb-cassandra/azuredeploy.json)]

## <a name="deploy-with-azure-cli"></a>Azure CLI ile dağıtma

Azure CLI kullanarak Resource Manager şablonu dağıtmak için **kopyalama** seçin ve komut dosyası **deneyin** Azure Cloud Shell'i açmak için. Betik yapıştırmak için kabuk sağ tıklayın ve ardından **yapıştırın**:

```azurecli-interactive

read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the keyset name: ' keysetName
read -p 'Enter the first table name: ' table1Name
read -p 'Enter the second table name: ' table2Name

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-cassandra/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion keysetName=$keysetName \
   table1Name=$table1Name table2Name=$table2Name

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

`az cosmosdb show` Komut sağlanıp sağlanmadığını sonra yeni oluşturulan Azure Cosmos hesabı gösterir. CloudShell kullanmak yerine Azure CLI'yi yerel olarak yüklenmiş bir sürümünü kullanmak isterseniz bkz [Azure komut satırı arabirimi (CLI)](/cli/azure/) makalesi.

## İşleme (RU/s) üzerinde bir anahtar alanı güncelleştirme <a id="keyspace-ru-update"></a>

Aşağıdaki şablonu, aktarım hızı bir anahtar alanı güncelleştirir. Şablon Kopyalama ve aşağıda gösterildiği gibi dağıtmak veya ziyaret [Azure hızlı başlama Galerisi](https://azure.microsoft.com/resources/templates/101-cosmosdb-cassandra-keyspace-ru-update/) ve Azure portalından dağıtın. Ayrıca şablonunu yerel bilgisayarınıza indirin veya yeni bir şablon oluşturmak ve ile yerel bir yol belirtin `--template-file` parametresi.

[!code-json[cosmosdb-cassandra-keyspace-ru-update](~/quickstart-templates/101-cosmosdb-cassandra-keyspace-ru-update/azuredeploy.json)]

### <a name="deploy-keyspace-template-via-azure-cli"></a>Azure CLI ile anahtar alanı şablonu dağıtma

Azure CLI kullanarak Resource Manager şablonu dağıtmak için seçebileceğiniz **deneyin** Azure Cloud Shell'i açmak için. Betik yapıştırmak için kabuk sağ tıklayın ve ardından **yapıştırın**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the keyspace name: ' keyspaceName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-cassandra-keyspace-ru-update/azuredeploy.json \
   --parameters accountName=$accountName keyspaceName=$keyspaceName throughput=$throughput
```

## İşleme (RU/s) bir tablo üzerinde güncelleştir <a id="table-ru-update"></a>

Aşağıdaki şablonu, bir tablonun aktarım hızı güncelleştirir. Şablon Kopyalama ve aşağıda gösterildiği gibi dağıtmak veya ziyaret [Azure hızlı başlama Galerisi](https://azure.microsoft.com/resources/templates/101-cosmosdb-cassandra-table-ru-update/) ve Azure portalından dağıtın. Ayrıca şablonunu yerel bilgisayarınıza indirin veya yeni bir şablon oluşturmak ve ile yerel bir yol belirtin `--template-file` parametresi.

[!code-json[cosmosdb-cassandra-table-ru-update](~/quickstart-templates/101-cosmosdb-cassandra-table-ru-update/azuredeploy.json)]

### <a name="deploy-table-template-via-azure-cli"></a>Azure CLI aracılığıyla tablo şablonu dağıtma

Azure CLI kullanarak Resource Manager şablonu dağıtmak için seçebileceğiniz **deneyin** Azure Cloud Shell'i açmak için. Betik yapıştırmak için kabuk sağ tıklayın ve ardından **yapıştırın**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the keyspace name: ' keyspaceName
read -p 'Enter the table name: ' tableName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-cassandra-table-ru-update/azuredeploy.json \
   --parameters accountName=$accountName keyspaceName=$keyspaceName tableName=$tableName throughput=$throughput
```

## <a name="next-steps"></a>Sonraki Adımlar

Bazı ek kaynaklar aşağıda verilmiştir:

- [Azure Resource Manager belgeleri](/azure/azure-resource-manager/)
- [Azure Cosmos DB kaynak sağlayıcısı şeması](/azure/templates/microsoft.documentdb/allversions)
- [Azure Cosmos DB hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Yaygın Azure Resource Manager dağıtım hatalarını giderme](../azure-resource-manager/resource-manager-common-deployment-errors.md)