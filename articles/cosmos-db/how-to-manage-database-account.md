---
title: Azure Cosmos DB'de veritabanı hesaplarını yönetmeyi öğrenin
description: Azure Cosmos DB'de veritabanı hesaplarını yönetmeyi öğrenin
author: rimman
ms.service: cosmos-db
ms.topic: sample
ms.date: 04/08/2019
ms.author: rimman
ms.openlocfilehash: b2b5e58ca480aa3abaa0766319977b8d1160ebeb
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59283010"
---
# <a name="manage-an-azure-cosmos-account"></a>Bir Azure Cosmos hesabı yönetme

Bu makalede, Azure Cosmos hesabın nasıl yönetileceği açıklanır. Çoklu yönlendirmeyi ayarlayın, ekleme veya bir bölgeyi kaldırabilir, birden fazla yazma bölgesini yapılandırmak ve yük devretme önceliklerini ayarlamak öğreneceksiniz. 

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

### <a id="create-database-account-via-portal"></a>Azure portal

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

### <a id="create-database-account-via-cli"></a>Azure CLI

```bash
# Create an account
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group Name>
```

## <a name="configure-clients-for-multi-homing"></a>İstemcileri birden çok giriş için yapılandırma

### <a id="configure-clients-multi-homing-dotnet"></a>.NET SDK'sı v2

```csharp
ConnectionPolicy policy = new ConnectionPolicy
    {
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp,
        UseMultipleWriteLocations = true
    };
policy.SetCurrentLocation("West US 2");

// Pass the connection policy with the preferred locations on it to the client.
DocumentClient client = new DocumentClient(new Uri(this.accountEndpoint), this.accountKey, policy);
```

### <a id="configure-clients-multi-homing-dotnet-v3"></a>.NET SDK'sı v3 (Önizleme)

```csharp
CosmosConfiguration config = new CosmosConfiguration("endpoint", "key");
config.UseCurrentRegion("West US");
CosmosClient client = new CosmosClient(config);
```

### <a id="configure-clients-multi-homing-java-async"></a>Java Async SDK’sı

```java
ConnectionPolicy policy = new ConnectionPolicy();
policy.setUsingMultipleWriteLocations(true);
policy.setPreferredLocations(Collections.singletonList(region));

AsyncDocumentClient client =
    new AsyncDocumentClient.Builder()
        .withMasterKeyOrResourceToken(this.accountKey)
        .withServiceEndpoint(this.accountEndpoint)
        .withConsistencyLevel(ConsistencyLevel.Eventual)
        .withConnectionPolicy(policy).build();
```

### <a id="configure-clients-multi-homing-javascript"></a>Node.js/JavaScript/TypeScript SDK

```javascript
const connectionPolicy: ConnectionPolicy = new ConnectionPolicy();
connectionPolicy.UseMultipleWriteLocations = true;
connectionPolicy.PreferredLocations = [region];

const client = new CosmosClient({
  endpoint: config.endpoint,
  auth: { masterKey: config.key },
  connectionPolicy,
  consistencyLevel: ConsistencyLevel.Eventual
});
```

### <a id="configure-clients-multi-homing-python"></a>Python SDK’sı

```python
connection_policy = documents.ConnectionPolicy()
connection_policy.UseMultipleWriteLocations = True
connection_policy.PreferredLocations = [region]

client = cosmos_client.CosmosClient(self.account_endpoint, {'masterKey': self.account_key}, connection_policy, documents.ConsistencyLevel.Session)
```

## <a name="addremove-regions-from-your-database-account"></a>Veritabanı hesabınızda bölge ekleme/çıkarma işlemi gerçekleştirme

### <a id="add-remove-regions-via-portal"></a>Azure portal

1. Azure Cosmos hesabınıza gidin ve açmak **verileri genel olarak çoğaltma** menüsü.

2. Bölge ekleme için ile harita üzerinde altıgenlerin seçin **+** , istenen bölgelerin karşılık gelen etiket. Alternatif olarak, bir bölge eklemek için seçin **+ Ekle bölge** seçenek ve açılan menüden bir bölge seçin.

3. Bölge kaldırmak için onay işaretleriyle mavi altıgenlerin seçerek bir veya daha fazla bölge eşlemesinden temizleyin. Veya "Çöp" seçin (🗑) sağ taraftaki bölge yanındaki simge.

4. Değişikliklerinizi kaydetmek için seçmeniz **Tamam**.

   ![Ekleme veya bölgeler menü Kaldır](./media/how-to-manage-database-account/add-region.png)

Tek bir bölgede yazma modu, yazma bölgesi kaldırılamaz. Geçerli yazma bölgesine silmeden önce farklı bir bölgeye yük devretme gerekir.

Ekleyebilir veya en az bir bölge varsa herhangi bir bölgeyi kaldırmak yazma modu, birden çok bölgede.

### <a id="add-remove-regions-via-cli"></a>Azure CLI

```bash
# Create an account with 1 region
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations eastus=0

# Add a region
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations eastus=0 westus=1

# Remove a region
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations westus=0
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

Aşağıdaki JSON kodunu örneğidir bir [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) şablonu. Bir Azure Cosmos hesapla dağıtmak için kullanabileceğiniz [sınırlanmış eskime durumu tutarlılık düzeyi](consistency-levels.md). En fazla eskime aralığı 5 saniye olarak ayarlanır. İzin en fazla eski istek sayısını 100 olarak ayarlanır. Resource Manager şablonu biçimini ve söz dizimi hakkında bilgi edinmek için [Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).

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

1. Azure Cosmos hesabınıza gidin ve açmak **verileri genel olarak çoğaltma** menüsü.

2. Menüsünün üstünde, seçin **el ile yük devretme**.

   ![Verileri genel olarak çoğaltma menüsü](./media/how-to-manage-database-account/replicate-data-globally.png)

3. Üzerinde **el ile yük devretme** menüsünde Yeni yazma bölgenizi seçin. Bu seçenek yazma bölgenizi değiştirir anladığınızı belirten onay kutusunu işaretleyin.

4. Yük devretmeyi tetiklemek için seçin **Tamam**.

   ![El ile yük devretme portal menüsü](./media/how-to-manage-database-account/manual-failover.png)

### <a id="enable-manual-failover-via-cli"></a>Azure CLI

```bash
# Given your account currently has regions with priority: eastus=0 westus=1
# Change the priority order to trigger a failover of the write region
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations westus=0 eastus=1
```

## <a id="automatic-failover"></a>Azure Cosmos DB hesabınız için otomatik yük devretmeyi etkinleştir

### <a id="enable-automatic-failover-via-portal"></a>Azure portal

1. Azure Cosmos DB hesabınızdan açın **verileri genel olarak çoğaltma** bölmesi. 

2. Bölmenin en üstünde seçin **otomatik yük devretme**.

   ![Verileri genel olarak çoğaltma menüsü](./media/how-to-manage-database-account/replicate-data-globally.png)

3. Üzerinde **otomatik yük devretme** bölmesinde emin olun **etkinleştirmek otomatik yük devretme** ayarlanır **ON**. 

4. **Kaydet**’i seçin.

   ![Otomatik yük devretme portal menüsü](./media/how-to-manage-database-account/automatic-failover.png)

Bu menüde, yük devretme önceliklerini de ayarlayabilirsiniz.

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

1. Azure Cosmos hesabınızdan açın **verileri genel olarak çoğaltma** bölmesi. 

2. Bölmenin en üstünde seçin **otomatik yük devretme**.

   ![Verileri genel olarak çoğaltma menüsü](./media/how-to-manage-database-account/replicate-data-globally.png)

3. Üzerinde **otomatik yük devretme** bölmesinde emin olun **etkinleştirmek otomatik yük devretme** ayarlanır **ON**. 

4. Yük devretme önceliğini değiştirmek için satırın üzerine geldiğinizde görüntülenen sol tarafındaki üç nokta okuma bölgeleri sürükleyin. 

5. **Kaydet**’i seçin.

   ![Otomatik yük devretme portal menüsü](./media/how-to-manage-database-account/automatic-failover.png)

Bu menüden yazma bölgesini değiştiremezsiniz. Yazma bölgesini el ile değiştirmek için el ile yük devretme gerçekleştirmeniz gerekir.

### <a id="set-failover-priorities-via-cli"></a>Azure CLI

```bash
# Assume region order is initially eastus=0 westus=1 automatic failover on account creation
az cosmosdb failover-priority-change --name <Azure Cosmos account name> --resource-group <Resource Group name> --failover-policies westus=0 eastus=1
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makaleleri okuyun:

* [Tutarlılık yönetme](how-to-manage-consistency.md)
* [Bölgeler arasında çakışmalar yönetme](how-to-manage-conflicts.md)
* [Genel dağıtım - başlık altında](global-dist-under-the-hood.md)
* [Çok yöneticili uygulamalarınızda yapılandırma](how-to-multi-master.md)
* [Birden çok giriş için istemcileri yapılandırma](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [Bölge ekleme veya Azure Cosmos DB hesabınızdan kaldırma](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [Bir özel çakışma çözüm ilkesi oluşturma](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)

