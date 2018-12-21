---
title: Azure PowerShell Betiği-oluşturma MongoDB için bir Azure Cosmos DB API hesabı
description: Azure PowerShell betik örneği - MongoDB için Azure Cosmos DB API hesabı oluşturma
services: cosmos-db
ms.service: cosmos-db
author: SnehaGunda
ms.author: sngun
ms.custom: mvc
ms.devlang: PowerShell
ms.topic: sample
ms.date: 05/29/2018
ms.openlocfilehash: 08a0994f2da7dcee5c9e9498d68767f458a82acb
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53715180"
---
# <a name="create-a-azure-cosmos-db-account-for-mongodb-api-using-powershell"></a>PowerShell kullanarak MongoDB API'si için bir Azure Cosmos DB hesabı oluşturma

Bu örnek PowerShell Betiği bir Azure Cosmos DB MongoDB API hesabı oluşturur. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/cosmosdb/create-and-configure-mongodb-database/create-and-configure-mongodb-database.ps1?highlight=9,12-15,18,21-23,26-29,32-37 "Create an Azure Cosmos DB account")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Betik örneği çalıştırıldıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [New-AzureRmResource](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresource?view=azurermps-3.8.0) | Bir veritabanı veya elastik havuz barındıran bir mantıksal sunucu oluşturur. |
| [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Cosmos DB PowerShell betiği örnekleri, [Azure Cosmos DB PowerShell betiklerinde](../powershell-samples.md) bulunabilir.