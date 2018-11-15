---
title: Azure Cosmos DB'de veritabanı hesaplarını yönetmeyi öğrenin
description: Azure Cosmos DB'de veritabanı hesaplarını yönetmeyi öğrenin
services: cosmos-db
author: christopheranderson
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/17/2018
ms.author: chrande
ms.openlocfilehash: 0683516d16bf1501eee83901c5171811b8c0e44d
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51621556"
---
# <a name="manage-database-accounts-in-azure-cosmos-db"></a>Azure Cosmos DB'de veritabanı hesaplarını yönetme

Bu makalede, çoklu yönlendirmeyi ayarlayın, bir bölge Ekle/Kaldır, birden fazla yazma bölgesini yapılandırmak ve yük devretme önceliklerini kurulumu için Azure Cosmos DB hesabınızı yönetmek açıklar. 

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

### <a id="create-database-account-via-portal"></a>Azure portal

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

### <a id="create-database-account-via-cli"></a>Azure CLI

```bash
# Create an account
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group Name>
```

## <a name="configure-clients-for-multi-homing"></a>İstemcileri birden çok giriş için yapılandırma

### <a id="configure-clients-multi-homing-dotnet"></a>.NET SDK

```csharp
// Create a new Connection Policy
ConnectionPolicy policy = new ConnectionPolicy
    {
        // Note: These aren't required settings for multi-homing,
        // just suggested defaults
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp,
        UseMultipleWriteLocations = true,
    };
// Add regions to Preferred locations
// The name of the location will match what you see in the portal/etc.
policy.PreferredLocations.Add("East US");
policy.PreferredLocations.Add("North Europe");

// Pass the Connection policy with the preferred locations on it to the client.
DocumentClient client = new DocumentClient(new Uri(this.accountEndpoint), this.accountKey, policy);
```

### <a id="configure-clients-multi-homing-java-async"></a>Java Async SDK’sı

```java
ConnectionPolicy policy = new ConnectionPolicy();
policy.setPreferredLocations(Collections.singleton("West US"));
AsyncDocumentClient client =
        new AsyncDocumentClient.Builder()
                .withMasterKey(this.accountKey)
                .withServiceEndpoint(this.accountEndpoint)
                .withConnectionPolicy(policy).build();
```

### <a id="configure-clients-multi-homing-java-sync"></a>Java Sync SDK’sı

```java
ConnectionPolicy connectionPolicy = new ConnectionPolicy();
Collection<String> preferredLocations = new ArrayList<String>();
preferredLocations.add("Australia East");
connectionPolicy.setPreferredLocations(preferredLocations);
DocumentClient client = new DocumentClient(accountEndpoint, accountKey, connectionPolicy);
```

### <a id="configure-clients-multi-homing-javascript"></a>Node.js/JavaScript/TypeScript SDK

```javascript
// Set up the connection policy with your preferred regions
const connectionPolicy: ConnectionPolicy = new ConnectionPolicy();
connectionPolicy.PreferredLocations = ["West US", "Australia East"];

// Pass that connection policy to the client
const client = new CosmosClient({
  endpoint: config.endpoint,
  auth: { masterKey: config.key },
  connectionPolicy
});
```

### <a id="configure-clients-multi-homing-python"></a>Python SDK’sı

```python
connection_policy = documents.ConnectionPolicy()
connection_policy.PreferredLocations = ['West US', 'Japan West']
client = cosmos_client.CosmosClient(self.account_endpoint, {'masterKey': self.account_key}, connection_policy)

```

## <a name="addremove-regions-from-your-database-account"></a>Veritabanı hesabınızda bölge ekleme/çıkarma işlemi gerçekleştirme

### <a id="add-remove-regions-via-portal"></a>Azure portal

1. Azure Cosmos DB hesabınıza gidip **Verileri genel olarak çoğaltma** menüsünü açın.

2. Bölge eklemek için istediğiniz bölgeye karşılık gelen **"+"** etiketine sahip boş altıgenlere tıklayarak haritadan bir veya daha fazla bölge seçin. Bölge eklemek için **+ Bölge ekle** seçeneğini belirleyip açılan menüden bölge seçimi de yapabilirsiniz.

3. Bölgeleri kaldırmak için haritadaki onay işaretli mavi altıgenlerden birine tıklayarak seçimini kaldırın veya sağ taraftaki bölge listesinde bulunan "çöp kutusu" (🗑) simgesini seçin.

4. Değişiklikleri kaydetmek için Kaydet’e tıklayın.

   ![Bölge ekle/kaldır menüsü](./media/how-to-manage-database-account/add-region.png)

Tek bölgeli yazma modunda yazma bölgesini kaldıramazsınız. Geçerli yazma bölgesini silmeden önce farklı bir bölgeye yük devretme gerçekleştirmeniz gerekir.

Çok bölgeli yazma modunda en az bir bölgeye sahip olduğunuz sürece istediğiniz bölgeyi ekleyebilir/kaldırabilirsiniz.

### <a id="add-remove-regions-via-cli"></a>Azure CLI

```bash
# Given an account created with 1 region like so
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations 'eastus=0'

# Add a new region by adding another region to the list
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations 'eastus=0 westus=1'

# Remove a region by removing a region from the list
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations 'westus=0'
```

## <a name="configure-multiple-write-regions"></a>Birden fazla yazma bölgesi yapılandırma

### <a id="configure-multiple-write-regions-portal"></a>Azure portal

Bir veritabanı oluşturduğunuzda **Çok bölgeli yazma** ayarının etkin olduğundan emin olun.

![Azure Cosmos hesap oluşturma ekran görüntüsü](./media/how-to-manage-database-account/account-create.png)

### <a id="configure-multiple-write-regions-cli"></a>Azure CLI

```bash
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group name> --enable-multiple-write-locations true
```

### <a id="configure-multiple-write-regions-arm"></a>Resource Manager şablonu

Aşağıdaki JSON kodu örnek bir Resource Manager şablonudur. Sınırlanmış eskime durumu, en fazla eskime aralığı 5 saniye ile en fazla 100 kaydırmadan kaçınma şansınız eski istek sayısı olarak bir Azure Cosmos hesapla tutarlılık İlkesi dağıtmak için kullanabilirsiniz. Resource Manager şablon biçimi ve söz dizimi hakkında bilgi edinmek için [Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) belgelerine bakın.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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
                "consistencyPolicy": {
                    "defaultConsistencyLevel": "BoundedStaleness",
                    "maxIntervalInSeconds": 5,
                    "maxStalenessPrefix": 100
                },
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


## <a id="manual-failover"></a>Azure Cosmos hesabınız için el ile yük devretme etkinleştir

### <a id="enable-manual-failover-via-portal"></a>Azure portal

1. Azure Cosmos hesabınıza gidin ve açmak **"verileri genel olarak çoğaltma"** menüsü.

2. Menünün en üstündeki **"El ile Yük Devretme"** düğmesine tıklayın.

   ![Verileri genel olarak çoğaltma menüsü](./media/how-to-manage-database-account/replicate-data-globally.png)

3. **"El ile Yük Devretme"** menüsünde yeni yazma bölgenizi seçin ve bu seçeneğin yazma bölgenizi değiştireceğini anladığınızı belirten kutuyu işaretleyin.

4. Yük devretmeyi başlatmak için Tamam'a tıklayın.

   ![El ile yük devretme portal menüsü](./media/how-to-manage-database-account/manual-failover.png)

### <a id="enable-manual-failover-via-cli"></a>Azure CLI

```bash
# Given your account currently has regions with priority like so: 'eastus=0 westus=1'
# Change the priority order to trigger a failover of the write region
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations 'eastus=1 westus=0'
```

## <a id="automatic-failover"></a>Azure Cosmos hesabınız için otomatik yük devretmeyi etkinleştir

### <a id="enable-automatic-failover-via-portal"></a>Azure portal

1. Azure Cosmos hesabınızdan açın **"verileri genel olarak çoğaltma"** bölmesi. 

2. Bölmenin en üstündeki **"Otomatik Yük Devretme"** düğmesine tıklayın.

   ![Verileri genel olarak çoğaltma menüsü](./media/how-to-manage-database-account/replicate-data-globally.png)

3. **"Otomatik Yük Devretme"** bölmesinde **Otomatik Yük Devretmeyi Etkinleştir** ayarının **AÇIK** olduğundan emin olun. 

4. Menünün alt kısmındaki Kaydet’e tıklayın.

   ![Otomatik yük devretme portal menüsü](./media/how-to-manage-database-account/automatic-failover.png)

Bu menüde yük devretme önceliklerinizi de ayarlayabilirsiniz.

### <a id="enable-automatic-failover-via-cli"></a>Azure CLI

```bash
# Enable automatic failover on account creation
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group name> --enable-automatic-failover true

# Enable automatic failover on an existing account
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --enable-automatic-failover true

# Disable automatic failover on an existing account
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --enable-automatic-failover false
```

## <a name="set-failover-priorities-for-your-azure-cosmos-account"></a>Azure Cosmos hesabınız için yük devretme önceliklerini ayarlayın

### <a id="set-failover-priorities-via-portal"></a>Azure portal

1. Azure Cosmos hesabınızdan açın **"verileri genel olarak çoğaltma"** bölmesi. 

2. Bölmenin en üstündeki **"Otomatik Yük Devretme"** düğmesine tıklayın.

   ![Verileri genel olarak çoğaltma menüsü](./media/how-to-manage-database-account/replicate-data-globally.png)

3. **"Otomatik Yük Devretme"** bölmesinde **Otomatik Yük Devretmeyi Etkinleştir** ayarının **AÇIK** olduğundan emin olun. 

4. Okuma bölgelerinin üzerine geldiğinizde satırın sol tarafında görünen üç nokta simgesine tıklayıp sürükleyerek yük devretme önceliğini değiştirebilirsiniz. 

5. Menünün alt kısmındaki Kaydet’e tıklayın.

   ![Otomatik yük devretme portal menüsü](./media/how-to-manage-database-account/automatic-failover.png)

Bu menüden yazma bölgesini değiştiremezsiniz. Yazma bölgesini el ile değiştirmek için el ile yük devretme gerçekleştirmeniz gerekir.

### <a id="set-failover-priorities-via-cli"></a>Azure CLI

```bash
az cosmosdb failover-priority-change --name <Azure Cosmos account name> --resource-group <Resource Group name> --failover-policies 'eastus=0 westus=2 southcentralus=1'
```

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki belgeleri kullanarak Azure Cosmos DB'deki tutarlılık düzeyleri ve veri çakışmalarını yönetme hakkında bilgi edinebilirsiniz:

* [Tutarlılığı yönetme](how-to-manage-consistency.md)
* [Bölgeler arasındaki çakışmaları yönetme](how-to-manage-conflicts.md)

