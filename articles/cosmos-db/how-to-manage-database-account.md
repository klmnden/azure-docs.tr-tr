---
title: Azure Cosmos DB'de veritabanı hesaplarını yönetmeyi öğrenin
description: Azure Cosmos DB'de veritabanı hesaplarını yönetmeyi öğrenin
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/23/2019
ms.author: mjbrown
ms.openlocfilehash: 0abeb3235f296e2dc873bcfe88910cdd12555d71
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476208"
---
# <a name="manage-an-azure-cosmos-account"></a>Azure Cosmos hesabını yönetme

Bu makalede, Azure portalı, Azure PowerShell, Azure CLI ve Azure Resource Manager şablonlarını kullanarak bir Azure Cosmos hesapta çeşitli görevleri yönetmek açıklar.

## <a name="create-an-account"></a>Hesap oluşturma

### <a id="create-database-account-via-portal"></a>Azure portal

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

### <a id="create-database-account-via-cli"></a>Azure CLI

```azurecli-interactive
# Create an account
$resourceGroupName = 'myResourceGroup'
$accountName = 'myaccountname' # must be lower case.

az cosmosdb create \
   --name $accountName \
   --resource-group $resourceGroupName \
   --kind GlobalDocumentDB \
   --default-consistency-level Session \
   --locations regionName=WestUS failoverPriority=0 isZoneRedundant=False \
   --locations regionName=EastUS failoverPriority=1 isZoneRedundant=False \
   --enable-multiple-write-locations true
```

### <a id="create-database-account-via-ps"></a>Azure PowerShell
```azurepowershell-interactive
# Create an Azure Cosmos account for Core (SQL) API
$resourceGroupName = "myResourceGroup"
$location = "West US"
$accountName = "mycosmosaccount" # must be lower case.

$locations = @(
    @{ "locationName"="West US"; "failoverPriority"=0 },
    @{ "locationName"="East US"; "failoverPriority"=1 }
)

$consistencyPolicy = @{
    "defaultConsistencyLevel"="BoundedStaleness";
    "maxIntervalInSeconds"=300;
    "maxStalenessPrefix"=100000
}

$CosmosDBProperties = @{
    "databaseAccountOfferType"="Standard";
    "locations"=$locations;
    "consistencyPolicy"=$consistencyPolicy;
    "enableMultipleWriteLocations"="true"
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Location $location `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

### <a id="create-database-account-via-arm-template"></a>Azure Resource Manager şablonu

Bu Azure Resource Manager şablonu iki bölgeleri ve tutarlılık düzeyi, otomatik yük devretme ve çok yöneticili seçmek için seçenekleri ile yapılandırılmış herhangi bir desteklenen API için bir Azure Cosmos hesabı oluşturun. Bu şablonu dağıtmak için Benioku sayfasında azure'a Dağıt tıklayarak [oluşturma Azure Cosmos hesabı](https://github.com/Azure/azure-quickstart-templates/tree/master/101-cosmosdb-create-multi-region-account)

## <a name="addremove-regions-from-your-database-account"></a>Veritabanı hesabınızda bölge ekleme/çıkarma işlemi gerçekleştirme

### <a id="add-remove-regions-via-portal"></a>Azure portal

1. [Azure portalda](https://portal.azure.com) oturum açın. 

1. Azure Cosmos hesabınıza gidin ve açmak **verileri genel olarak çoğaltma** menüsü.

1. Bölge ekleme için ile harita üzerinde altıgenlerin seçin **+** , istenen bölgelerin karşılık gelen etiket. Alternatif olarak, bir bölge eklemek için seçin **+ Ekle bölge** seçenek ve açılan menüden bir bölge seçin.

1. Bölge kaldırmak için onay işaretleriyle mavi altıgenlerin seçerek bir veya daha fazla bölge eşlemesinden temizleyin. Veya "Çöp" seçin (🗑) sağ taraftaki bölge yanındaki simge.

1. Değişikliklerinizi kaydetmek için seçmeniz **Tamam**.

   ![Ekleme veya bölgeler menü Kaldır](./media/how-to-manage-database-account/add-region.png)

Tek bir bölgede yazma modu, yazma bölgesi kaldırılamaz. Geçerli yazma bölgesine silmeden önce farklı bir bölgeye yük devretme gerekir.

Ekleyebilir veya en az bir bölge varsa herhangi bir bölgeyi kaldırmak yazma modu, birden çok bölgede.

### <a id="add-remove-regions-via-cli"></a>Azure CLI

```azurecli-interactive
$resourceGroupName = 'myResourceGroup'
$accountName = 'myaccountname'

# Create an account with 1 region
az cosmosdb create --name $accountName --resource-group $resourceGroupName --locations regionName=westus failoverPriority=0 isZoneRedundant=False

# Add a region
az cosmosdb update --name $accountName --resource-group $resourceGroupName --locations regionName=westus failoverPriority=0 isZoneRedundant=False --locations regionName=EastUS failoverPriority=1 isZoneRedundant=False

# Remove a region
az cosmosdb update --name $accountName --resource-group $resourceGroupName --locations regionName=westus failoverPriority=0 isZoneRedundant=False
```

### <a id="add-remove-regions-via-ps"></a>Azure PowerShell

```azurepowershell-interactive
# Create an account with 1 region
$resourceGroupName = "myResourceGroup"
$location = "West US"
$accountName = "mycosmosaccount" # must be lower case.

$locations = @( @{ "locationName"="West US"; "failoverPriority"=0 } )
$consistencyPolicy = @{ "defaultConsistencyLevel"="Session" }
$CosmosDBProperties = @{
    "databaseAccountOfferType"="Standard";
    "locations"=$locations;
    "consistencyPolicy"=$consistencyPolicy
}
New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Location $location `
    -Name $accountName -PropertyObject $CosmosDBProperties

# Add a region
$account = Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Name $accountName

$locations = @( 
    @{ "locationName"="West US"; "failoverPriority"=0 },
    @{ "locationName"="East Us"; "failoverPriority"=1 } 
)

$account.Properties.locations = $locations
$CosmosDBProperties = $account.Properties

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties

# Azure Resource Manager does not wait on the resource update
Write-Host "Confirm region added before continuing..."

# Remove a region
$account = Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Name $accountName

$locations = @( @{ "locationName"="West US"; "failoverPriority"=0 } )

$account.Properties.locations = $locations
$CosmosDBProperties = $account.Properties

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

## <a id="configure-multiple-write-regions"></a>Birden fazla yazma bölgesini yapılandırma

### <a id="configure-multiple-write-regions-portal"></a>Azure portal

Açık **genel veri çoğaltma** sekmenize **etkinleştirme** çok bölgeli yazma etkinleştirmek için. Çok bölgeli yazma etkinleştirildikten sonra hesabı şu anda sahip olduğunuz tüm okuma bölgeleri okuma haline gelir ve bölgeleri yazma. 

> [!NOTE]
> Çok bölgeli yazma etkinleştirildikten sonra devre dışı bırakılamıyor. 

![Azure Cosmos hesabı çok yöneticili ekran yapılandırır.](./media/how-to-manage-database-account/single-to-multi-master.png)

Lütfen ulaşın askcosmosdb@microsoft.com bu özellik hakkındaki diğer sorular için diğer ad. 

### <a id="configure-multiple-write-regions-cli"></a>Azure CLI

```azurecli-interactive
$resourceGroupName = 'myResourceGroup'
$accountName = 'myaccountname'
az cosmosdb update --name $accountName --resource-group $resourceGroupName --enable-multiple-write-locations true
```

### <a id="configure-multiple-write-regions-ps"></a>Azure PowerShell

```azurepowershell-interactive
# Update an Azure Cosmos account from single to multi-master

$account = Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Name $accountName

$account.Properties.enableMultipleWriteLocations = "true"
$CosmosDBProperties = $account.Properties

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

### <a id="configure-multiple-write-regions-arm"></a>Resource Manager şablonu

Bir hesap tek ana çok asıl hesap ve ayarı oluşturmak için kullanılan Resource Manager şablonu dağıtarak geçirilebilir `enableMultipleWriteLocations: true`. Aşağıdaki Azure Resource Manager şablonu, bir Azure Cosmos hesabı SQL API'si için bir tek bölge ve çok yöneticili etkin ile dağıtan bir çıplak en düşük şablonudur.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "name": {
            "type": "String"
        },
        "location": {
            "type": "String",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "resources": [
        {
            "type": "Microsoft.DocumentDb/databaseAccounts",
            "kind": "GlobalDocumentDB",
            "name": "[parameters('name')]",
            "apiVersion": "2015-04-08",
            "location": "[parameters('location')]",
            "tags": {},
            "properties": {
                "databaseAccountOfferType": "Standard",
                "consistencyPolicy": { "defaultConsistencyLevel": "Session" },
                "locations": [
                    {
                        "locationName": "[parameters('location')]",
                        "failoverPriority": 0
                    }
                ],
                "enableMultipleWriteLocations": true
            }
        }
    ]
}
```

## <a id="automatic-failover"></a>Azure Cosmos hesabınız için otomatik yük devretmeyi etkinleştir

Otomatik Yük devretme seçeneğini bir bölge kullanılamaz duruma gelmesi durumunda yük devretme önceliği en yüksek kullanıcı eylemi ile bölgeye yük devretme için Azure Cosmos DB sağlar. Otomatik Yük devretme etkinleştirildiğinde, bölge öncelik değiştirilebilir. Hesabın, otomatik yük devretmeyi etkinleştirmek için iki veya daha fazla bölgede olmalıdır.

### <a id="enable-automatic-failover-via-portal"></a>Azure portal

1. Azure Cosmos hesabınızdan açın **verileri genel olarak çoğaltma** bölmesi.

2. Bölmenin en üstünde seçin **otomatik yük devretme**.

   ![Verileri genel olarak çoğaltma menüsü](./media/how-to-manage-database-account/replicate-data-globally.png)

3. Üzerinde **otomatik yük devretme** bölmesinde emin olun **etkinleştirmek otomatik yük devretme** ayarlanır **ON**. 

4. **Kaydet**’i seçin.

   ![Otomatik yük devretme portal menüsü](./media/how-to-manage-database-account/automatic-failover.png)

### <a id="enable-automatic-failover-via-cli"></a>Azure CLI

```azurecli-interactive
# Enable automatic failover on an existing account
$resourceGroupName = 'myResourceGroup'
$accountName = 'myaccountname'

az cosmosdb update --name $accountName --resource-group $resourceGroupName --enable-automatic-failover true
```

### <a id="enable-automatic-failover-via-ps"></a>Azure PowerShell

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$account = Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName

$account.Properties.enableAutomaticFailover="true";
$CosmosDBProperties = $account.Properties;

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

## <a name="set-failover-priorities-for-your-azure-cosmos-account"></a>Azure Cosmos hesabınız için yük devretme önceliklerini ayarlayın

Bir Cosmos hesabı otomatik yük devretme için yapılandırdıktan sonra Yük devretme önceliğini bölgeler için değiştirilebilir.

> [!IMPORTANT]
> Yazma bölgesi (yük devretme öncelik sıfır) değiştirilemiyor. hesap otomatik yük devretme için yapılandırıldığında. Yazma bölgesini değiştirmek için otomatik yük devretme devre dışı bırakın ve elle yük devretme yapmanız gerekir.

### <a id="set-failover-priorities-via-portal"></a>Azure portal

1. Azure Cosmos hesabınızdan açın **verileri genel olarak çoğaltma** bölmesi.

2. Bölmenin en üstünde seçin **otomatik yük devretme**.

   ![Verileri genel olarak çoğaltma menüsü](./media/how-to-manage-database-account/replicate-data-globally.png)

3. Üzerinde **otomatik yük devretme** bölmesinde emin olun **etkinleştirmek otomatik yük devretme** ayarlanır **ON**.

4. Yük devretme önceliğini değiştirmek için satırın üzerine geldiğinizde görüntülenen sol tarafındaki üç nokta okuma bölgeleri sürükleyin.

5. **Kaydet**’i seçin.

   ![Otomatik yük devretme portal menüsü](./media/how-to-manage-database-account/automatic-failover.png)

### <a id="set-failover-priorities-via-cli"></a>Azure CLI

```azurecli-interactive
# Assume region order is initially eastus=0 westus=1 southeastasia=2 on account creation
$resourceGroupName = 'myResourceGroup'
$accountName = 'myaccountname'

az cosmosdb failover-priority-change --name $accountName --resource-group $resourceGroupName --failover-policies eastus=0 southeastasia=1 westus=2
```

### <a id="set-failover-priorities-via-ps"></a>Azure PowerShell

```azurepowershell-interactive
# Assume account currently has regions with priority: West US = 0, East US = 1, Southeast Asia = 2
$resourceGroupName = "myResourceGroup"
$accountName = "myaccountname"

$failoverPolicies = @(
    @{ "locationName"="West US"; "failoverPriority"=0 },
    @{ "locationName"="Southeast Asia"; "failoverPriority"=1 },
    @{ "locationName"="East US"; "failoverPriority"=2 }
)

Invoke-AzResourceAction -Action failoverPriorityChange `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName -Parameters $failoverPolicies
```

## <a id="manual-failover"></a>Bir Azure Cosmos hesapta el ile yük devretme gerçekleştirme

> [!IMPORTANT]
> Azure Cosmos hesabı bu işleminin başarılı olması el ile yük devretme için yapılandırılmış olmalıdır.

Elle yük devretme gerçekleştirmek için işlem hesabın yazma bölgesini değiştirmek içerir (yük devretme öncelik = 0) hesabı için yapılandırılmış başka bir bölgeye.

> [!NOTE]
> Çok yöneticili hesapları el ile yük devredilemez. Azure Cosmos SDK'sını kullanarak uygulamalar için SDK'sı bir bölge kullanılamaz duruma geldiğinde algılayabilir, ardından sonraki en yakın bölgeyi çok girişli API SDK'yı kullanıyorsanız, otomatik olarak yeniden yönlendirme.

### <a id="enable-manual-failover-via-portal"></a>Azure portal

1. Azure Cosmos hesabınıza gidin ve açmak **verileri genel olarak çoğaltma** menüsü.

2. Menüsünün üstünde, seçin **el ile yük devretme**.

   ![Verileri genel olarak çoğaltma menüsü](./media/how-to-manage-database-account/replicate-data-globally.png)

3. Üzerinde **el ile yük devretme** menüsünde Yeni yazma bölgenizi seçin. Bu seçenek yazma bölgenizi değiştirir anladığınızı belirten onay kutusunu işaretleyin.

4. Yük devretmeyi tetiklemek için seçin **Tamam**.

   ![El ile yük devretme portal menüsü](./media/how-to-manage-database-account/manual-failover.png)

### <a id="enable-manual-failover-via-cli"></a>Azure CLI

```azurecli-interactive
# Assume account currently has regions with priority: eastus=0 westus=1
# Change the priority order to trigger a failover of the write region
$resourceGroupName = 'myResourceGroup'
$accountName = 'myaccountname'

az cosmosdb update --name $accountName --resource-group $resourceGroupName --locations regionName=westus failoverPriority=0 isZoneRedundant=False --locations regionName=eastus failoverPriority=1 isZoneRedundant=False
```

### <a id="enable-manual-failover-via-ps"></a>Azure PowerShell

```azurepowershell-interactive
# Assume account currently has regions with priority: West US = 0, East US = 1
# Change the priority order to trigger a failover of the write region
$resourceGroupName = "myResourceGroup"
$accountName = "myaccountname"

$account = Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName

$locations = @(
    @{ "locationName"="East US"; "failoverPriority"=0 },
    @{ "locationName"="West US"; "failoverPriority"=1 }
)

$account.Properties.locations=$locations;
$CosmosDBProperties = $account.Properties;

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi ve Azure Cosmos hesabı hem de veritabanı ve kapsayıcıları yönetme hakkında daha fazla örnek için aşağıdaki makaleyi okuyun:

* [Azure Cosmos DB, Azure PowerShell kullanarak yönetme](manage-with-powershell.md)
* [Azure CLI kullanarak Azure Cosmos DB'yi yönetmeyi](manage-with-cli.md)
