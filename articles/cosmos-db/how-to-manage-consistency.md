---
title: Azure Cosmos DB'de tutarlılığı yönetmeyi öğrenin
description: Azure Cosmos DB'de tutarlılığı yönetmeyi öğrenin
services: cosmos-db
author: christopheranderson
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/17/2018
ms.author: chrande
ms.openlocfilehash: be2c68922221af848c9e484d03527d02808c071a
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51283827"
---
# <a name="manage-consistency-levels-in-azure-cosmos-db"></a>Azure Cosmos DB'deki tutarlılık düzeylerini yönetme

Bu makalede varsayılan tutarlılığı ayarlama, istemci tutarlılığını geçersiz kılma, oturum belirteçlerini el ile yönetme ve Olasılığa Dayalı Sınırlanmış Eskime Durumu (PBS) ölçümünü anlama yöntemleri açıklanmaktadır.

## <a name="configure-the-default-consistency-level"></a>Varsayılan tutarlılık düzeyini yapılandırma

Varsayılan tutarlılık düzeyi, istemcilerin varsayılan olarak kullanacağı tutarlılık düzeyidir. Bu düzey istemciler tarafından geçersiz kılınabilir.

### <a name="cli"></a>CLI

```bash
# create with a default consistency
az cosmosdb create --name <name of Cosmos DB Account> --resource-group <resource group name> --default-consistency-level Strong

# update an existing account's default consistency
az cosmosdb update --name <name of Cosmos DB Account> --resource-group <resource group name> --default-consistency-level BoundedStaleness
```

### <a name="powershell"></a>PowerShell

Aşağıdaki örnekte Doğu ABD ve Batı ABD bölgelerinde çoklu ana kaynak özelliği etkinleştirilmiş bir Cosmos DB hesabı oluşturulmakta ve varsayılan tutarlılık ilkesi maksimum eskime aralığı 10 saniye ve maksimum eskime isteği toleransı 200 olan Sınırlanmış Eskime Durumu özelliğine sahip varsayılan tutarlılık ilkesi ayarlanmaktadır.

```azurepowershell-interactive
$locations = @(@{"locationName"="East US"; "failoverPriority"=0},
             @{"locationName"="West US"; "failoverPriority"=1})

$iprangefilter = ""

$consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness";
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

### <a name="portal"></a>Portal

Varsayılan tutarlılık düzeyini görüntülemek veya değiştirmek için Azure portalda oturum açın. Cosmos DB Hesabınızı bulun ve **Varsayılan tutarlılık** bölmesini açın. Buradan istediğiniz tutarlılık düzeyini yeni varsayılan değer olarak belirleyebilir ve Kaydet'e tıklayabilirsiniz.

![Azure portaldaki Tutarlılık menüsünün resmi](./media/how-to-manage-consistency/consistency-settings.png)

## <a name="override-the-default-consistency-level"></a>Varsayılan tutarlılık düzeyini geçersiz kılma

İstemciler, hizmet tarafından belirlenen varsayılan tutarlılık düzeyini geçersiz kılabilir. Bu işlem istemcinin tamamı için veya istek başına gerçekleştirilebilir.

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

Oturum belirteçlerini el ile yönetmek istiyorsanız bunları yanıtlardan alabilir ve her istek için ayrıca belirleyebilirsiniz. Oturum belirteçlerini el ile yönetmeniz gerekmiyorsa aşağıdaki örnekleri kullanmanıza gerek yoktur. Oturum belirteçlerine müdahale etmediğiniz sürece SDK bunları otomatik olarak takip edecek ve en güncel oturum belirtecini kullanacaktır.

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

PBS ölçümünü izlemek için Azure portalda Cosmos DB Hesabınıza gidip **Ölçümler** bölmesini açın. Buradan **Tutarlılık** sekmesine tıklayın ve "**İş yükünüze dayalı olarak kesinlikle tutarlı okumaların olasılığı (bkz. PBS)**" adlı grafiği inceleyin.

![Azure portalda PBS grafiğinin resmi](./media/how-to-manage-consistency/pbs-metric.png)

Bu ölçümü görmek için Cosmos DB ölçümler menüsünü kullanın. Azure İzleyici ölçümleri arasında gösterilmez.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki belgeleri inceleyerek Cosmos DB'deki veri çakışmalarını yönetme veya bir sonraki anahtara geçme kavramı hakkında daha fazla bilgi edinebilirsiniz:

* [Bölgeler arasındaki çakışmaları yönetme](how-to-manage-conflicts.md)
* [Bölümleme ve veri dağıtımı](partition-data.md)
