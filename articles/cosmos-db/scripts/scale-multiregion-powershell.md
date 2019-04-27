---
title: Azure PowerShell Betiği - Azure Cosmos DB için çok bölgeli çoğaltma
description: Azure PowerShell Betiği Örneği - Azure Cosmos DB için çok bölgeli çoğaltma
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
author: rockboyfor
ms.author: v-yeche
ms.devlang: PowerShell
ms.topic: sample
origin.date: 05/10/2017
ms.date: 04/15/2019
ms.reviewer: sngun
ms.openlocfilehash: 02628401bed6e65784bf7ddc4a7082f617640cf9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60448478"
---
# <a name="replicate-an-azure-cosmos-db-database-account-in-multiple-regions-and-configure-failover-priorities-using-powershell"></a>PowerShell kullanarak bir Azure Cosmos DB veritabanı hesabını birden çok bölgede çoğaltma ve yük devretme önceliklerini yapılandırma

Bu örnek, PowerShell kullanarak herhangi bir türden Azure Cosmos DB veritabanı hesabını birden çok bölgede çoğaltır ve yük devretme önceliklerini yapılandırır. 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Örnek betik

<!--First make chinaeast2 as wirte region-->
<!--Second make chinanorth as write region-->
<!--Last add chinanorth2 new region-->

```powershell
# Set the Azure resource group name and location
$resourceGroupName = "myResourceGroup"
$resourceGroupLocation = "chinaeast2"

# Database name
$DBName = "testdb"
# Distribution locations
$locations = @(@{"locationName"="chinaeast"; 
                 "failoverPriority"=2},
               @{"locationName"="chinanorth"; 
                 "failoverPriority"=1},
               @{"locationName"="chinaeast2"; 
                 "failoverPriority"=0})


# Create the resource group
New-AzResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation

# Consistency policy
$consistencyPolicy = @{"maxIntervalInSeconds"="10"; 
                       "maxStalenessPrefix"="200"}

# DB properties
$DBProperties = @{"databaseAccountOfferType"="Standard";
                  "Kind"="GlobalDocumentDB"; 
                  "locations"=$locations; 
                  "consistencyPolicy"=$consistencyPolicy;}

# Create the database
New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
                    -ApiVersion "2015-04-08" `
                    -ResourceGroupName $resourceGroupName `
                    -Location $resourceGroupLocation `
                    -Name $DBName `
                    -PropertyObject $DBProperties

# Update failoverpolicy to make China North as a write region
$NewfailoverPolicies = @(@{"locationName"="chinanorth"; "failoverPriority"=0}, @{"locationName"="chinaeast2"; "failoverPriority"=1}, @{"locationName"="chinaeast"; "failoverPriority"=2} )

Invoke-AzResourceAction `
    -Action failoverPriorityChange `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName `
    -Name $DBName `
    -Parameters @{"failoverPolicies"=$NewfailoverPolicies}

# Add a new locations with priorities
$newLocations = @(@{"locationName"="chinanorth"; 
                 "failoverPriority"=0},
               @{"locationName"="chinaeast"; 
                 "failoverPriority"=1},
               @{"locationName"="chinanorth2"; 
                 "failoverPriority"=2},
               @{"locationName"="chinaeast2";
                 "failoverPriority"=3})

# Updated properties
$updateDBProperties = @{"databaseAccountOfferType"="Standard";
                        "locations"=$newLocations;}

# Update the database with the properties
Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName `
    -Name $DBName `
    -PropertyObject $UpdateDBProperties

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
| [Set-AzResource](https://docs.microsoft.com/powershell/module/az.resources/set-azresource) | Veritabanı hesabını değiştirir. |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Cosmos DB PowerShell betiği örnekleri, [Azure Cosmos DB PowerShell betiklerinde](../powershell-samples.md) bulunabilir.

<!-- Update_Description: update meta properties -->