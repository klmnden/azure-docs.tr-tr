---
title: Azure Cosmos DB hesapları için çok yöneticili etkinleştir
description: Bu makalede, Azure portalı, PowerShell, CLI veya Azure Resource Manager şablonu ile bir Azure Cosmos DB hesabı oluştururken çok yöneticili desteğini nasıl etkinleştireceğinizi açıklanır.
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: b6fc04fc728f753dc8a3900b26c6c01f03cc7710
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49077247"
---
# <a name="enable-multi-master-for-azure-cosmos-db-accounts"></a>Azure Cosmos DB hesapları için çok yöneticili etkinleştir

Çok yöneticili desteği, Azure Cosmos DB hesabı oluşturma sırasında etkinleştirilir. Azure portalı, PowerShell, CLI veya Azure Resource Manager şablonu kullanarak bir Azure Cosmos DB hesabı oluşturulabilir.

> [!IMPORTANT]
> Şu anda çok yöneticili desteği yalnızca yeni Azure Cosmos DB hesapları etkinleştirilebilir. Var olan Azure Cosmos DB hesapları özelliğini kullanamazsınız. Biz varolan hesapları için destek sağlamak için çalışıyoruz ve kullanılabilir olduğunda bu destek duyuracağız.

Çok yöneticili destekle bir Azure Cosmos DB hesabı oluşturduktan sonra veritabanları, kapsayıcıları oluşturma, belgeleri karşıya yüklemesine ve çakışma çözüm ilkeleri atayabilir. Çok yöneticili ve kod örnekleri, çakışma çözümü için bkz: [çok yöneticili kod örnekleri](multi-master-conflict-resolution.md#code-samples) makalesi.

## <a name="enable-multi-master-using-azure-portal"></a>Azure portalını kullanarak çok yöneticili etkinleştir

1. [Azure portalda](https://portal.azure.com/) oturum açma

2. Seç'e tıklayın **kaynak Oluştur > veritabanları > Azure Cosmos DB**.

3. İçinde **yeni hesabı** bölmesinde, yeni bir Azure Cosmos DB hesabı için aşağıdaki ayarları girin:

   |**Ayar**  |**Önerilen değer** |**Açıklama**|
   |---------|---------|---------|
   |Abonelik   | {Subscription}  |Bu Azure Cosmos DB hesabı için kullanılacak Azure aboneliğini seçin.  |
   |Kaynak Grubu  |   {Kaynak grubunuzun adı}    |  Mevcut bir kaynak grubunu seçin ya da seçin **Yeni Oluştur**, ardından hesabınız için yeni bir kaynak grubu adı girin. |
   |Hesap Adı | {hesap adınız}   |  Azure Cosmos DB hesabınızı tanımlamak için benzersiz bir ad girin.        |
   |API  |   Herhangi biri   |  API türü seçin.   |
   |Konum  | Herhangi bir bölge seçin   | Azure Cosmos DB hesabınızın barındırılacağı coğrafi konumu seçin. Bu hesap, birden çok bölgede olacağından, herhangi bir bölgeyi seçebilirsiniz.  |
   |Coğrafi yedekliliği etkinleştir   |  Etkinleştirme  |  Aşağıda seçili birden çok ana etkinleştirmek için seçin.   |
   |Çoklu Yöneticiyi Etkinleştir | Etkinleştirme  | Bu hesap için birden çok ana etkinleştirmek için seçin. |


## <a name="using-multi-master-in-sdks"></a>Çok yöneticili SDK'lar

Etkin çoklu yönetici hesabı ile uygulamalarınız içinde çok mastering, aşağıda gösterildiği gibi ConnectionPolicy yararlanarak yararlanabilirsiniz.

```csharp
ConnectionPolicy policy = new ConnectionPolicy
{
   ConnectionMode = ConnectionMode.Direct,
   ConnectionProtocol = Protocol.Tcp,
   UseMultipleWriteLocations = true,
};
policy.PreferredLocations.Add(LocationNames.WestUS);
policy.PreferredLocations.Add(LocationNames.NorthEurope);
policy.PreferredLocations.Add(LocationNames.SoutheastAsia);
```

## <a name="enable-multi-master-using-powershell"></a>Etkinleştirme çok yöneticili PowerShell'i kullanma

Ayarlayarak çok yöneticili etkinleştirilmiş bir Cosmos DB hesabı oluşturabilirsiniz `enableMultipleWriteLocations` "true" parametresi. Çok yöneticili etkin bir Azure Cosmos DB hesabı oluşturmak için bir PowerShell penceresi açın ve aşağıdaki betiği çalıştırın:

```azurepowershell-interactive
$locations = @(@{"locationName"="East US"; "failoverPriority"=0},
             @{"locationName"="West US"; "failoverPriority"=1})

$iprangefilter = "<ip-range-filter>"

$consistencyPolicy = @{"defaultConsistencyLevel"="Session";
                       "maxIntervalInSeconds"= "10";
                       "maxStalenessPrefix"="200"}

$CosmosDBProperties = @{"databaseAccountOfferType"="Standard";
                        "locations"=$locations;
                        "consistencyPolicy"=$consistencyPolicy;
                        "ipRangeFilter"=$iprangefilter;
                        "enableMultipleWriteLocations"="true"}

New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
  -ApiVersion "2015-04-08" `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -Name "myCosmosDbAccount" `
  -Properties $CosmosDBProperties
```

## <a name="enable-multi-master-using-cli"></a>Etkinleştirme çok yöneticili CLI kullanma

Çok yöneticili "true" birden çok yazma konumları etkinleştirme parametresi ayarlanarak etkinleştirebilirsiniz. Çok yöneticili etkin bir Azure Cosmos DB hesabı oluşturmak için Azure CLI'yi açın veya cloud shell ve aşağıdaki komutu çalıştırın:

```azurecli-interactive
az cosmosdb create \
   –-name "myCosmosDbAccount" \
   --resource-group "myResourceGroup" \
   --default-consistency-level "Session" \
   --enable-automatic-failover "true" \
   --locations "EastUS=0" "WestUS=1" \
   --enable-multiple-write-locations true \
```

## <a name="enable-multi-master-using-resource-manager-template"></a>Çok yöneticili kullanarak etkinleştirin Resource Manager şablonu

Aşağıdaki JSON kod, bir Azure Cosmos DB hesabı dağıtmak için kullanabileceğiniz örnek Resource Manager şablonudur. Resource Manager şablon biçimi ve söz dizimi hakkında bilgi edinmek için [Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) belgeleri. Bu şablonda fark için anahtar "enableMultipleWriteLocations" parametredir: true.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "name": {
            "type": "String"
        },
        "location": {
            "type": "String"
        },
        "locationName": {
            "type": "String"
        },
        "defaultExperience": {
            "type": "String"
        }
    },
    "resources": [
        {
            "type": "Microsoft.DocumentDb/databaseAccounts",
            "kind": "GlobalDocumentDB",
            "name": "[parameters('name')]",
            "apiVersion": "2015-04-08",
            "location": "[parameters('location')]",
            "tags": {
                "defaultExperience": "[parameters('defaultExperience')]"
            },
            "properties": {
                "databaseAccountOfferType": "Standard",
                "locations": [
                    {
                        "id": "[concat(parameters('name'), '-', parameters('location'))]",
                        "failoverPriority": 0,
                        "locationName": "[parameters('locationName')]"
                    }
                ],
                "isVirtualNetworkFilterEnabled": false,
                "enableMultipleWriteLocations": true,
                "virtualNetworkRules": [],
                "dependsOn": []
            }
        }
    ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Cosmos DB hesapları için çok yöneticili desteğini nasıl etkinleştireceğinizi öğrendiniz. Ardından, aşağıdaki kaynakları arayın:

* [çok yöneticili açık kaynak NoSQL veritabanları ile kullanma](multi-master-oss-nosql.md)

* [Azure Cosmos DB'de çakışma çözümü anlama](multi-master-conflict-resolution.md)
