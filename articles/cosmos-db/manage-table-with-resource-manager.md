---
title: Azure Cosmos DB tablo API'si için Azure Resource Manager şablonları
description: Azure Resource Manager şablonları oluşturmak ve Azure Cosmos DB tablo API'si yapılandırmak için kullanın.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mjbrown
ms.openlocfilehash: 82e2a436bf6b25b6164d845d234896390a262292
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65968815"
---
# <a name="manage-azure-cosmos-db-table-api-resources-using-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarını kullanarak Azure Cosmos DB tablo API'si kaynaklarını yönetme

## Azure Cosmos hesabı ve tablo oluşturma <a id="create-resource"></a>

Bir Azure Resource Manager şablonu kullanarak Azure Cosmos DB kaynaklarını oluşturun. Bu şablon, 400 RU/sn aktarım hızı anda bir tablo ile tablo API'si için bir Azure Cosmos hesabı oluşturur. Şablon Kopyalama ve aşağıda gösterildiği gibi dağıtmak veya ziyaret [Azure hızlı başlama Galerisi](https://azure.microsoft.com/resources/templates/101-cosmosdb-table/) ve Azure portalından dağıtın. Ayrıca şablonunu yerel bilgisayarınıza indirin veya yeni bir şablon oluşturmak ve ile yerel bir yol belirtin `--template-file` parametresi.

[!code-json[create-cosmos-table](~/quickstart-templates/101-cosmosdb-table/azuredeploy.json)]

### <a name="deploy-via-powershell"></a>PowerShell ile dağıtma

PowerShell kullanarak Resource Manager şablonu dağıtmak için **kopyalama** seçin ve komut dosyası **deneyin** Azure Cloud Shell'i açmak için. Betik yapıştırmak için kabuk sağ tıklayın ve ardından **yapıştırın**:

```azurepowershell-interactive

$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$location = Read-Host -Prompt "Enter the location (i.e. westus2)"
$primaryRegion = Read-Host -Prompt "Enter the primary region (i.e. westus2)"
$secondaryRegion = Read-Host -Prompt "Enter the secondary region (i.e. eastus2)"
$tableName = Read-Host -Prompt "Enter the table name"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-table/azuredeploy.json" `
    -primaryRegion $primaryRegion `
    -secondaryRegion $secondaryRegion `
    -tableName $tableName

 (Get-AzResource --ResourceType "Microsoft.DocumentDb/databaseAccounts" --ApiVersion "2015-04-08" --ResourceGroupName $resourceGroupName).name
```

Azure Cloud shell'den PowerShell yerine yerel olarak yüklenmiş bir sürümünü kullanmak isterseniz, için olan [yükleme](/powershell/azure/install-az-ps) Azure PowerShell modülü. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın.

### <a name="deploy-via-azure-cli"></a>Azure CLI ile dağıtma

Azure CLI kullanarak Resource Manager şablonu dağıtmak için **kopyalama** seçin ve komut dosyası **deneyin** Azure Cloud Shell'i açmak için. Betik yapıştırmak için kabuk sağ tıklayın ve ardından **yapıştırın**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the table name: ' tableName

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-table/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion tableName=$tableName

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

`az cosmosdb show` Komut sağlanıp sağlanmadığını sonra yeni oluşturulan Azure Cosmos hesabı gösterir. CloudShell kullanmak yerine Azure CLI'yi yerel olarak yüklenmiş bir sürümünü kullanmak isterseniz bkz [Azure komut satırı arabirimi (CLI)](/cli/azure/) makalesi.

## İşleme (RU/s) bir tablo üzerinde güncelleştir <a id="table-ru-update"></a>

Aşağıdaki şablonu, bir tablonun aktarım hızı güncelleştirir. Şablon Kopyalama ve aşağıda gösterildiği gibi dağıtmak veya ziyaret [Azure hızlı başlama Galerisi](https://azure.microsoft.com/resources/templates/101-cosmosdb-table-ru-update/) ve Azure portalından dağıtın. Ayrıca şablonunu yerel bilgisayarınıza indirin veya yeni bir şablon oluşturmak ve ile yerel bir yol belirtin `--template-file` parametresi.

[!code-json[cosmosdb-table-ru-update](~/quickstart-templates/101-cosmosdb-table-ru-update/azuredeploy.json)]

### <a name="deploy-table-throughput-via-powershell"></a>Tablo aktarım hızı PowerShell aracılığıyla dağıtma

PowerShell kullanarak Resource Manager şablonu dağıtmak için **kopyalama** seçin ve komut dosyası **deneyin** Azure Cloud Shell'i açmak için. Betik yapıştırmak için kabuk sağ tıklayın ve ardından **yapıştırın**:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$tableName = Read-Host -Prompt "Enter the table name"
$throughput = Read-Host -Prompt "Enter new throughput for table"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-table-ru-update/azuredeploy.json" `
    -accountName $accountName `
    -tableName $tableName `
    -throughput $throughput
```

### <a name="deploy-table-template-via-azure-cli"></a>Azure CLI aracılığıyla tablo şablonu dağıtma

Azure CLI kullanarak Resource Manager şablonu dağıtmak için seçebileceğiniz **deneyin** Azure Cloud Shell'i açmak için. Betik yapıştırmak için kabuk sağ tıklayın ve ardından **yapıştırın**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the table name: ' tableName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-table-ru-update/azuredeploy.json \
   --parameters accountName=$accountName tableName=$tableName throughput=$throughput
```

## <a name="next-steps"></a>Sonraki Adımlar

Bazı ek kaynaklar aşağıda verilmiştir:

- [Azure Resource Manager belgeleri](/azure/azure-resource-manager/)
- [Azure Cosmos DB kaynak sağlayıcısı şeması](/azure/templates/microsoft.documentdb/allversions)
- [Azure Cosmos DB hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Yaygın Azure Resource Manager dağıtım hatalarını giderme](../azure-resource-manager/resource-manager-common-deployment-errors.md)