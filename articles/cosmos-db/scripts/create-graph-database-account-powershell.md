---
title: Azure PowerShell Betiği-Azure Cosmos DB Gremlin API hesap oluşturma
description: Azure PowerShell Betiği Örneği - Azure Cosmos DB Gremlin API hesabı oluşturma
ms.service: cosmos-db
author: rockboyfor
ms.author: v-yeche
ms.subservice: cosmosdb-graph
ms.devlang: PowerShell
ms.topic: sample
origin.date: 05/29/2018
ms.date: 04/15/2019
ms.reviewer: sngun
ms.openlocfilehash: 06f4379a0100b4a0d5335ed6e8c99bb16160715d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60446644"
---
<!--Verify sucessfully-->
# <a name="azure-cosmos-db-create-a-gremlin-api-account-using-powershell"></a>Azure Cosmos DB: PowerShell kullanarak bir Gremlin API hesabı oluşturma

Bu örnek PowerShell betiği bir Azure Cosmos DB Gremlin API hesabı oluşturur. 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Örnek betik

```powershell
# Set the Azure resource group name and location
$resourceGroupName = "myResourceGroupgraph1"
$resourceGroupLocation = "China East"

# Create the resource group
New-AzResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation

# Database name
$DBName = "testdbgraph1"

# Write and read locations and priorities for the database
$locations = @(@{"locationName"="China East"; 
                 "failoverPriority"=0}, 
               @{"locationName"="China North"; 
                  "failoverPriority"=1})

# IP addresses that can access the database through the firewall
$iprangefilter = "10.0.0.1"

# Consistency policy
$consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness";
                       "maxIntervalInSeconds"="10"; 
                       "maxStalenessPrefix"="200"}

# Create a Gremlin API Cosmos DB account
$Capability= "EnableGremlin"

$capabilities= @(@{"name"=$Capability})

# DB properties
$DBProperties = @{"databaseAccountOfferType"="Standard"; 
                          "locations"=$locations; 
                          "consistencyPolicy"=$consistencyPolicy;
                          "capabilities"=$capabilities; 
                          "ipRangeFilter"=$iprangefilter}

# Create the database
New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
                    -ApiVersion "2015-04-08" `
                    -ResourceGroupName $resourceGroupName `
                    -Location $resourceGroupLocation `
                    -Name $DBName `
                    -PropertyObject $DBProperties

```

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Betik örneği çalıştırıldıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [Yeni AzResource](https://docs.microsoft.com/powershell/module/az.resources/new-azresource) | Bir veritabanı veya elastik havuz barındıran bir mantıksal sunucu oluşturur. |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Cosmos DB PowerShell betiği örnekleri, [Azure Cosmos DB PowerShell betiklerinde](../powershell-samples.md) bulunabilir.

<!--Verify sucessfully-->
<!--Update_Description: update link -->