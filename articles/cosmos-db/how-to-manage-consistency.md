---
title: Azure Cosmos DB'de tutarlılığı yönetmeyi öğrenin
description: Azure Cosmos DB'de tutarlılığı yönetmeyi öğrenin
author: christopheranderson
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/17/2018
ms.author: chrande
ms.openlocfilehash: 7dfc299c32b25ddf939aa3efcb927697307887a2
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58904330"
---
# <a name="manage-consistency-levels-in-azure-cosmos-db"></a>Azure Cosmos DB'deki tutarlılık düzeylerini yönetme

Bu makalede, Azure Cosmos DB'deki tutarlılık düzeyleri yönetme konusunda açıklanmaktadır. Varsayılan tutarlılık düzeyini yapılandırma, varsayılan tutarlılığı geçersiz kılmak, el ile oturum belirteçlerini yönetmenizi ve Probabilistically sınırlanmış eskime durumu (PBS) ölçüm anlama konusunda bilgi edinin.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="configure-the-default-consistency-level"></a>Varsayılan tutarlılık düzeyini yapılandırma

Varsayılan tutarlılık düzeyini, istemcilerin varsayılan tutarlılık düzeyidir. İstemciler bunu geçersiz kılabilirsiniz.

### <a name="cli"></a>CLI

```bash
# create with a default consistency
az cosmosdb create --name <name of Cosmos DB Account> --resource-group <resource group name> --default-consistency-level Strong

# update an existing account's default consistency
az cosmosdb update --name <name of Cosmos DB Account> --resource-group <resource group name> --default-consistency-level BoundedStaleness
```

### <a name="powershell"></a>PowerShell

Bu örnekte, Doğu ABD ve Batı ABD bölgelerinde etkin çok ana şablon ile yeni bir Azure Cosmos DB hesabı oluşturur. Varsayılan tutarlılık ilkesi oturum olarak ayarlanır.

```azurepowershell-interactive
$locations = @(@{"locationName"="East US"; "failoverPriority"=0},
             @{"locationName"="West US"; "failoverPriority"=1})

$iprangefilter = ""

$consistencyPolicy = @{"defaultConsistencyLevel"="Session"}

$CosmosDBProperties = @{"databaseAccountOfferType"="Standard";
                        "locations"=$locations;
                        "consistencyPolicy"=$consistencyPolicy;
                        "ipRangeFilter"=$iprangefilter;
                        "enableMultipleWriteLocations"="true"}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
  -ApiVersion "2015-04-08" `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -Name "myCosmosDbAccount" `
  -Properties $CosmosDBProperties
```

### <a name="portal"></a>Portal

Görüntülemek veya varsayılan tutarlılık düzeyini değiştirmek için Azure portalında oturum açın. Azure Cosmos DB hesabınızın bulup açın **varsayılan tutarlılık** bölmesi. Yeni varsayılan olarak istediğiniz ve ardından tutarlılık düzeyini seçin **Kaydet**.

![Azure Portalı'nda tutarlılığı menüsü](./media/how-to-manage-consistency/consistency-settings.png)

## <a name="override-the-default-consistency-level"></a>Varsayılan tutarlılık düzeyini geçersiz kılma

İstemcileri, hizmet tarafından ayarlanmış varsayılan tutarlılık düzeyini geçersiz kılabilirsiniz. Bu seçenek, tüm istemci veya istek başına ayarlanabilir.

### <a id="override-default-consistency-dotnet"></a>.NET SDK

```csharp
// Override consistency at the client level
ConsistencyPolicy consistencyPolicy = new ConsistencyPolicy
    {
        DefaultConsistencyLevel = ConsistencyLevel.BoundedStaleness,
        MaxStalenessIntervalInSeconds = 5,
        MaxStalenessPrefix = 100
    };
documentClient = new DocumentClient(new Uri(endpoint), authKey, connectionPolicy, consistencyPolicy);

// Override consistency at the request level via request options
RequestOptions requestOptions = new RequestOptions { ConsistencyLevel = ConsistencyLevel.Strong };

var response = await client.CreateDocumentAsync(collectionUri, document, requestOptions);
```

### <a id="override-default-consistency-java-async"></a>Java Async SDK’sı

```java
// Override consistency at the client level
ConnectionPolicy policy = new ConnectionPolicy();

AsyncDocumentClient client =
        new AsyncDocumentClient.Builder()
                .withMasterKey(this.accountKey)
                .withServiceEndpoint(this.accountEndpoint)
                .withConsistencyLevel(ConsistencyLevel.Eventual)
                .withConnectionPolicy(policy).build();
```

### <a id="override-default-consistency-java-sync"></a>Java Sync SDK’sı

```java
// Override consistency at the client level
ConnectionPolicy connectionPolicy = new ConnectionPolicy();
DocumentClient client = new DocumentClient(accountEndpoint, accountKey, connectionPolicy, ConsistencyLevel.Strong);
```

### <a id="override-default-consistency-javascript"></a>Node.js/JavaScript/TypeScript SDK

```javascript
// Override consistency at the client level
const client = new CosmosClient({
  /* other config... */
  consistencyLevel: ConsistencyLevel.Strong
});

// Override consistency at the request level via request options
const { body } = await item.read({ consistencyLevel: ConsistencyLevel.Eventual });
```

### <a id="override-default-consistency-python"></a>Python SDK’sı

```python
# Override consistency at the client level
connection_policy = documents.ConnectionPolicy()
client = cosmos_client.CosmosClient(self.account_endpoint, {'masterKey': self.account_key}, connection_policy, documents.ConsistencyLevel.Strong)
```

## <a name="utilize-session-tokens"></a>Oturum belirteçlerini kullanma

Oturum belirteçlerini el ile yönetmeniz için yanıttan Oturum belirteci alma ve bunları her istek için ayarlayın. Oturum belirteçlerini el ile yönetmeniz gerekmiyorsa, bu örnekleri kullanın gerekmez. SDK'sı oturumu belirteçleri otomatik olarak izler. Oturum belirteci el ile varsayılan olarak ayarlamazsanız, SDK'sı en son oturum belirteci kullanır.

### <a id="utilize-session-tokens-dotnet"></a>.NET SDK

```csharp
var response = await client.ReadDocumentAsync(
                UriFactory.CreateDocumentUri(databaseName, collectionName, "SalesOrder1"));
string sessionToken = response.SessionToken;

RequestOptions options = new RequestOptions();
options.SessionToken = sessionToken;
var response = await client.ReadDocumentAsync(
                UriFactory.CreateDocumentUri(databaseName, collectionName, "SalesOrder1"), options);
```

### <a id="utilize-session-tokens-java-async"></a>Java Async SDK’sı

```java
// Get session token from response
RequestOptions options = new RequestOptions();
options.setPartitionKey(new PartitionKey(document.get("mypk")));
Observable<ResourceResponse<Document>> readObservable = client.readDocument(document.getSelfLink(), options);
readObservable.single()           // we know there will be one response
  .subscribe(
      documentResourceResponse -> {
          System.out.println(documentResourceResponse.getSessionToken());
      },
      error -> {
          System.err.println("an error happened: " + error.getMessage());
      });

// Resume the session by setting the session token on RequestOptions
RequestOptions options = new RequestOptions();
requestOptions.setSessionToken(sessionToken);
Observable<ResourceResponse<Document>> readObservable = client.readDocument(document.getSelfLink(), options);
```

### <a id="utilize-session-tokens-java-sync"></a>Java Sync SDK’sı

```java
// Get session token from response
ResourceResponse<Document> response = client.readDocument(documentLink, null);
String sessionToken = response.getSessionToken();

// Resume the session by setting the session token on the RequestOptions
RequestOptions options = new RequestOptions();
options.setSessionToken(sessionToken);
ResourceResponse<Document> response = client.readDocument(documentLink, options);
```

### <a id="utilize-session-tokens-javascript"></a>Node.js/JavaScript/TypeScript SDK

```javascript
// Get session token from response
const { headers, item } = await container.items.create({ id: "meaningful-id" });
const sessionToken = headers["x-ms-session-token"];

// Immediately or later, you can use that sessionToken from the header to resume that session.
const { body } = await item.read({ sessionToken });
```

### <a id="utilize-session-tokens-python"></a>Python SDK’sı

```python
// Get the session token from the last response headers
item = client.ReadItem(item_link)
session_token = client.last_response_headers["x-ms-session-token"]

// Resume the session by setting the session token on the options for the request
options = {
    "sessionToken": session_token
}
item = client.ReadItem(doc_link, options)
```

## <a name="monitor-probabilistically-bounded-staleness-pbs-metric"></a>Olasılığa Dayalı Sınırlanmış Eskime Durumu (PBS) ölçümünü izleme

PBS ölçüm görüntülemek için Azure portalında Azure Cosmos DB hesabınıza gidin. Açık **ölçümleri** bölmesi ve select **tutarlılık** sekmesi. Ara adlı grafik **dayalı iş yükünüze dayalı kesinlikle tutarlı okumaların olasılığı (PBS bakın)**.

![Azure portalında PBS grafiği](./media/how-to-manage-consistency/pbs-metric.png)

Bu ölçüm görmek için Azure Cosmos DB ölçümleri menüyü kullanın. Bunu Azure izleme ölçümleri deneyim gösterilmesini.

## <a name="next-steps"></a>Sonraki adımlar

Veri çakışmalarını yönetme hakkında daha fazla bilgi edinmek veya Azure Cosmos DB'de sonraki en önemli kavram taşımanız gerekir. Aşağıdaki makalelere bakın:

* [Bölgeler arasındaki çakışmaları yönetme](how-to-manage-conflicts.md)
* [Bölümleme ve veri dağıtımı](partition-data.md)
