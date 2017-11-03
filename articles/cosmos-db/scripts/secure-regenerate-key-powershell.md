---
title: "Azure PowerShell Betiği yeniden Azure Cosmos DB hesap anahtarı | Microsoft Docs"
description: "Azure PowerShell Betiği örnek - üretme Azure Cosmos DB hesap anahtarı"
services: cosmos-db
documentationcenter: cosmosdb
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: cosmosdb
ms.workload: database
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 187d7b0839e1cd94122d4455c11eda05673f5acc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="regenerate-an-azure-cosmos-db-account-key-using-powershell"></a>PowerShell kullanarak Azure Cosmos DB hesap anahtarı yeniden oluştur

Bu örnek, her türlü Azure CLI kullanarak Azure Cosmos DB hesap anahtarı oluşturur.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/cosmosdb/regenerate-account-keys/regenerate-account-keys.ps1?highlight=36-41 "Regenerate Azure Cosmos DB account keys")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Komut dosyası örneği çalıştırdıktan sonra kaynak grubu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanılabilir.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyasını aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [Yeni-AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [AzureRmResource yeni](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresource?view=azurermps-3.8.0) | Bir veritabanı veya esnek havuzu barındıran bir mantıksal sunucu oluşturur. |
| [Çağırma AzureRmResourceAction](https://docs.microsoft.com/powershell/module/azurerm.resources/invoke-azurermresourceaction?view=azurermps-3.8.0) | Azure CosmosDB hesabındaki bir eylemi çağırır. |
| [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/remove-azurermresourcegroup) | Tüm iç içe kaynaklar dahil olmak üzere bir kaynak grubu siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](https://docs.microsoft.com/powershell/).

Ek Azure Cosmos DB PowerShell komut dosyası örnekleri bulunabilir [Azure Cosmos DB PowerShell komut dosyalarını](../powershell-samples.md).