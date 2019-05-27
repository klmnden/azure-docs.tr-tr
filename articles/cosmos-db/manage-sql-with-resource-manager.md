---
title: Oluşturma ve Azure Resource Manager şablonları kullanarak Azure Cosmos DB yönetme
description: Azure Resource Manager şablonları oluşturma ve Azure Cosmos DB SQL API (çekirdek) için yapılandırma kullanma
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mjbrown
ms.openlocfilehash: a3798ac0c73c7bc6c4012dbb089275254f4c3504
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65968841"
---
# <a name="manage-azure-cosmos-db-sql-core-api-resources-using-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarını kullanarak Azure Cosmos DB SQL (çekirdek) API kaynaklarını yönetme

## Bir Azure Cosmos hesabı, veritabanı ve kapsayıcı oluşturma <a id="create-resource"></a>

Bir Azure Resource Manager şablonu kullanarak Azure Cosmos DB kaynaklarını oluşturun. Bu şablon, 400 RU/sn aktarım hızı ve veritabanı düzeyinde paylaşan iki kapsayıcı içeren bir Azure Cosmos hesabı oluşturur. Şablon Kopyalama ve aşağıda gösterildiği gibi dağıtmak veya ziyaret [Azure hızlı başlama Galerisi](https://azure.microsoft.com/resources/templates/101-cosmosdb-sql/) ve Azure portalından dağıtın. Ayrıca şablonunu yerel bilgisayarınıza indirin veya yeni bir şablon oluşturmak ve ile yerel bir yol belirtin `--template-file` parametresi.

[!code-json[create-cosmosdb-sql](~/quickstart-templates/101-cosmosdb-sql/azuredeploy.json)]

### <a name="deploy-via-powershell"></a>PowerShell ile dağıtma

PowerShell kullanarak Resource Manager şablonu dağıtmak için **kopyalama** seçin ve komut dosyası **deneyin** Azure Cloud Shell'i açmak için. Betik yapıştırmak için kabuk sağ tıklayın ve ardından **yapıştırın**:

```azurepowershell-interactive

$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$location = Read-Host -Prompt "Enter the location (i.e. westus2)"
$primaryRegion = Read-Host -Prompt "Enter the primary region (i.e. westus2)"
$secondaryRegion = Read-Host -Prompt "Enter the secondary region (i.e. eastus2)"
$databaseName = Read-Host -Prompt "Enter the database name"
$container1Name = Read-Host -Prompt "Enter the first container name"
$container2Name = Read-Host -Prompt "Enter the second container name"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-sql/azuredeploy.json" `
    -accountName $accountName `
    -location $location `
    -primaryRegion $primaryRegion `
    -secondaryRegion $secondaryRegion `
    -databaseName $databaseName `
    -container1Name $container1Name `
    -container2Name $container2Name

 (Get-AzResource --ResourceType "Microsoft.DocumentDb/databaseAccounts" --ApiVersion "2015-04-08" --ResourceGroupName $resourceGroupName).name
```

Azure Cloud shell'den PowerShell yerine yerel olarak yüklenmiş bir sürümünü kullanmak isterseniz, için olan [yükleme](/powershell/azure/install-az-ps) Azure PowerShell modülü. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın.

### <a name="deploy-via-azure-cli"></a>Azure CLI ile dağıtma

Azure CLI kullanarak Resource Manager şablonu dağıtmak için seçebileceğiniz **deneyin** Azure Cloud Shell'i açmak için. Betik yapıştırmak için kabuk sağ tıklayın ve ardından **yapıştırın**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the database name: ' databaseName
read -p 'Enter the first container name: ' container1Name
read -p 'Enter the second container name: ' container2Name

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-sql/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion databaseName=$databaseName \
   container1Name=$container1Name container2Name=$container2Name

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

`az cosmosdb show` Komut sağlanıp sağlanmadığını sonra yeni oluşturulan Azure Cosmos hesabı gösterir. CloudShell kullanmak yerine Azure CLI'yi yerel olarak yüklenmiş bir sürümünü kullanmak isterseniz bkz [Azure komut satırı arabirimi (CLI)](/cli/azure/) makalesi.

## İşleme (RU/s) üzerinde bir veritabanı güncelleştirmesi <a id="database-ru-update"></a>

Aşağıdaki şablonu aktarım hızını bir veritabanını güncelleştirir. Şablon Kopyalama ve aşağıda gösterildiği gibi dağıtmak veya ziyaret [Azure hızlı başlama Galerisi](https://azure.microsoft.com/resources/templates/101-cosmosdb-sql-database-ru-update/) ve Azure portalından dağıtın. Ayrıca şablonunu yerel bilgisayarınıza indirin veya yeni bir şablon oluşturmak ve ile yerel bir yol belirtin `--template-file` parametresi.

[!code-json[cosmosdb-sql-database-ru-update](~/quickstart-templates/101-cosmosdb-sql-database-ru-update/azuredeploy.json)]

### <a name="deploy-database-template-via-powershell"></a>Veritabanı şablonu PowerShell aracılığıyla dağıtma

PowerShell kullanarak Resource Manager şablonu dağıtmak için **kopyalama** seçin ve komut dosyası **deneyin** Azure Cloud Shell'i açmak için. Betik yapıştırmak için kabuk sağ tıklayın ve ardından **yapıştırın**:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$databaseName = Read-Host -Prompt "Enter the database name"
$throughput = Read-Host -Prompt "Enter new throughput for database"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-sql-database-ru-update/azuredeploy.json" `
    -accountName $accountName `
    -databaseName $databaseName `
    -throughput $throughput
```

### <a name="deploy-database-template-via-azure-cli"></a>Azure CLI aracılığıyla veritabanı şablonu dağıtma

Azure CLI kullanarak Resource Manager şablonu dağıtmak için seçebileceğiniz **deneyin** Azure Cloud Shell'i açmak için. Betik yapıştırmak için kabuk sağ tıklayın ve ardından **yapıştırın**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the database name: ' databaseName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-sql-database-ru-update/azuredeploy.json \
   --parameters accountName=$accountName databaseName=$databaseName throughput=$throughput
```

## Güncelleştirme bir kapsayıcısında aktarım hızını (RU/sn) <a id="container-ru-update"></a>

Aşağıdaki şablonu, bir kapsayıcının aktarım hızını güncelleştirir. Şablon Kopyalama ve aşağıda gösterildiği gibi dağıtmak veya ziyaret [Azure hızlı başlama Galerisi](https://azure.microsoft.com/resources/templates/101-cosmosdb-sql-container-ru-update/) ve Azure portalından dağıtın. Ayrıca şablonunu yerel bilgisayarınıza indirin veya yeni bir şablon oluşturmak ve ile yerel bir yol belirtin `--template-file` parametresi.

[!code-json[cosmosdb-sql-container-ru-update](~/quickstart-templates/101-cosmosdb-sql-container-ru-update/azuredeploy.json)]

### <a name="deploy-container-template-via-powershell"></a>Kapsayıcı şablonu PowerShell aracılığıyla dağıtma

PowerShell kullanarak Resource Manager şablonu dağıtmak için **kopyalama** seçin ve komut dosyası **deneyin** Azure Cloud Shell'i açmak için. Betik yapıştırmak için kabuk sağ tıklayın ve ardından **yapıştırın**:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$databaseName = Read-Host -Prompt "Enter the database name"
$containerName = Read-Host -Prompt "Enter the container name"
$throughput = Read-Host -Prompt "Enter new throughput for container"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-sql-container-ru-update/azuredeploy.json" `
    -accountName $accountName `
    -databaseName $databaseName `
    -containerName $containerName `
    -throughput $throughput
```

### <a name="deploy-container-template-via-azure-cli"></a>Azure CLI aracılığıyla kapsayıcı şablonu dağıtma

Azure CLI kullanarak Resource Manager şablonu dağıtmak için seçebileceğiniz **deneyin** Azure Cloud Shell'i açmak için. Betik yapıştırmak için kabuk sağ tıklayın ve ardından **yapıştırın**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the database name: ' databaseName
read -p 'Enter the container name: ' containerName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-sql-container-ru-update/azuredeploy.json \
   --parameters accountName=$accountName databaseName=$databaseName containerName=$containerName throughput=$throughput
```

## <a name="next-steps"></a>Sonraki Adımlar

Bazı ek kaynaklar aşağıda verilmiştir:

- [Azure Resource Manager belgeleri](/azure/azure-resource-manager/)
- [Azure Cosmos DB kaynak sağlayıcısı şeması](/azure/templates/microsoft.documentdb/allversions)
- [Azure Cosmos DB hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Yaygın Azure Resource Manager dağıtım hatalarını giderme](../azure-resource-manager/resource-manager-common-deployment-errors.md)