---
title: Azure Cosmos DB kapsayıcıları bölümlenmemiş bölümlenmiş kapsayıcılara geçirme
description: Bölümlenmiş kapsayıcılarına bölümlenmemiş tüm mevcut kapsayıcıları geçirmeyi öğrenin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mjbrown
ms.openlocfilehash: e8aaee8cb7df794652b2c560c5250921c1a4e060
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65160643"
---
# <a name="migrate-non-partitioned-containers-to-partitioned-containers"></a>Bölümlenmemiş kapsayıcıları bölümlenmiş kapsayıcılara geçirme

Azure Cosmos DB bölüm anahtarı olmayan oluşturma kapsayıcıları destekler. Şu anda Azure CLI ve Azure Cosmos DB sahip bir sürüme daha az veya eşit 2.x SDK'ları (.Net, Java, NodeJs) kullanarak kapsayıcıları bölümlenmemiş oluşturabilirsiniz. Bölümlenmemiş kapsayıcılar Azure portalını kullanarak oluşturamazsınız. Ancak, böyle bölümlenmemiş kapsayıcılar esnek değildir ve 10 GB ve aktarım hızı sınırı 10 K RU/sn depolama kapasitesini düzelttik.

Bölümlenmemiş kapsayıcıları eski ve bölümlenmiş kapsayıcılarına depolamayı ve aktarım hızı için mevcut bölümlenmemiş kapsayıcılarınızı geçirmeniz gerekir. Azure Cosmos DB bölümlenmemiş kapsayıcılarınızı bölümlenmiş kapsayıcılara geçirmek için sistem tarafından tanımlanan mekanizması sağlar. Bu belgede açıklanmaktadır bölümlenmemiş tüm mevcut kapsayıcıları bölümlenmiş kapsayıcılarına otomatik geçişi nasıl. Yalnızca tüm dillerde SDK'lar V3 sürümünü kullanıyorsanız otomatik geçiş özelliğinin yararlanabilirsiniz.

> [!NOTE] 
> Şu anda, bu belgede açıklanan adımları kullanarak Azure Cosmos DB MongoDB ve Gremlin API hesapları geçiremezsiniz. 

## <a name="migrate-container-using-the-system-defined-partition-key"></a>Sistem tarafından tanımlanan bölüm anahtarı kullanarak kapsayıcı geçirme

Geçişi desteklemek için Azure Cosmos DB adlı bir sistem tarafından tanımlanan bölüm anahtarı tanımlar `/_partitionkey` bir bölüm anahtarı olmayan tüm kapsayıcıları üzerinde. Bölüm anahtar tanımı, kapsayıcıları geçirildikten sonra değiştiremezsiniz. Örneğin, bölümlenmiş bir kapsayıcıya geçirilen kapsayıcı tanımı aşağıdaki gibi olur: 

```json
{
    "Id": "CollId" 
  "partitionKey": {
    "paths": [
      "/_partitionKey"
    ],
    "kind": "Hash"
  },
}
```
 
Kapsayıcı geçirildikten sonra doldurarak belgeler oluşturabilirsiniz `_partitionKey` bir belge özelliklerinin yanı sıra özelliği. `_partitionKey` Özelliğini kullanarak, belgeleriniz bölüm anahtarı temsil eder. 

Doğru bölüm anahtarının seçilmesi, sağlanan aktarım hızını en iyi şekilde yararlanmak önemlidir. Daha fazla bilgi için [bir bölüm anahtarı seçme](partitioning-overview.md) makalesi. 

> [!NOTE]
> Yalnızca SDK'ları en son/V3 sürümünü tüm dillerde kullanıyorsanız sistem tanımlı bölüm anahtarına yararlanabilir.

Aşağıdaki örnek, sistem tanımlı bölüm anahtarına bir belge oluşturun ve bu belgeyi okumak için bir örnek kod gösterir:

**Belge JSON temsili**

```csharp
DeviceInformationItem = new DeviceInformationItem
{
   "id": "elevator/PugetSound/Building44/Floor1/1",
   "deviceId": "3cf4c52d-cc67-4bb8-b02f-f6185007a808",
   "_partitionKey": "3cf4c52d-cc67-4bb8-b02f-f6185007a808"
} 

public class DeviceInformationItem
{
    [JsonProperty(PropertyName = "id")]
    public string Id { get; set; }

    [JsonProperty(PropertyName = "deviceId")]
    public string DeviceId { get; set; }

    [JsonProperty(PropertyName = "_partitionKey")]
    public string PartitionKey {get {return this.DeviceId; set; }
}

CosmosContainer migratedContainer = database.Containers["testContainer"];

DeviceInformationItem deviceItem = new DeviceInformationItem() {
  Id = "1234", 
  DeviceId = "3cf4c52d-cc67-4bb8-b02f-f6185007a808"
} 

CosmosItemResponse<DeviceInformationItem > response = 
  await migratedContainer.Items.CreateItemAsync(
    deviceItem.PartitionKey, 
    deviceItem
  );

// Read back the document providing the same partition key
CosmosItemResponse<DeviceInformationItem> readResponse = 
  await migratedContainer.Items.ReadItemAsync<DeviceInformationItem>( 
    partitionKey:deviceItem.PartitionKey, 
    id: device.Id
  ); 

```

Tam bir örnek için bkz. [.Net örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/CodeSamples) GitHub deposu. 
                      
## <a name="migrate-the-documents"></a>Geçirme belgeleri

Kapsayıcı tanımı bir bölüm anahtarı özelliğiyle geliştirilmiştir, ancak kapsayıcı içindeki belgeler otomatik olmayan geçiş. Sistem bölümü anahtar özelliği anlamına gelir `/_partitionKey` yolu varolan belgeleri için otomatik olarak eklenmez. Bir bölüm anahtarı oluşturulan belgeleri okuyarak varolan belgeleri bölümlemek ve bunları geri ile yeniden yazma için ihtiyaç duyduğunuz `_partitionKey` belgelerde özelliği. 

## <a name="access-documents-that-dont-have-a-partition-key"></a>Bir bölüm anahtarı olmayan erişim belgeleri

Uygulamaları "CosmosContainerSettings.NonePartitionKeyValue" adlı özel bir sistem özelliği'ni kullanarak bölüm anahtarına sahip olmayan var olan belgelere erişebilir, geçirilmeyen belgeleri değerini budur. Bu özellik tüm CRUD ve sorgu işlemleri kullanabilirsiniz. Aşağıdaki örnek, tek bir belge NonePartitionKey okumak için bir örneği gösterilmektedir. 

```csharp
CosmosItemResponse<DeviceInformationItem> readResponse = 
await migratedContainer.Items.ReadItemAsync<DeviceInformationItem>( 
  partitionKey: CosmosContainerSettings.NonePartitionKeyValue, 
  id: device.Id
); 

```

Belgeleri bölümlemek konusunda tam örnek için bkz [.Net örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/CodeSamples) GitHub deposu. 

## <a name="compatibility-with-sdks"></a>SDK'ları ile uyumluluk

Azure Cosmos DB SDK'ları V2.x.x ve V1.x.x gibi eski sürümü sistem tarafından tanımlanan bölüm anahtarı özelliği desteklemez. Bu nedenle, kapsayıcı tanımı daha eski bir SDK'sından okuma, herhangi bir bölüm anahtarı tanımı içermiyor ve bu kapsayıcıların tam olarak eskisi gibi davranır. SDK'ları eski sürümü ile oluşturulan uygulamalar herhangi bir değişiklik olduğundan bölümlenmemiş çalışmaya devam edin. 

Geçirilen kapsayıcı SDK'sının en son/V3 sürümüne göre tüketilir ve sistem tarafından tanımlanan bölüm anahtarı yeni belgelerde doldurma Başlat, erişemiyor (okuma, güncelleştirme, silme, sorgu) gibi daha eski SDK'larından artık belgeler.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB'de bölümleme](partitioning-overview.md)
* [İstek birimleri Azure cosmos DB](request-units.md)
* [Kapsayıcılar ve veritabanları sağlama aktarım hızı](set-throughput.md)
* [Azure Cosmos hesabıyla çalışma](account-overview.md)