---
title: Azure Cosmos kapsayıcılar, Azure portalı ve çeşitli SDK'lar kullanarak büyük bir bölüm anahtarı ile oluşturun.
description: Bir kapsayıcı, Azure portalı ve farklı bir SDK'ları kullanarak büyük bir bölüm anahtarı ile Azure Cosmos DB'de oluşturmayı öğrenin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mjbrown
ms.openlocfilehash: 46df484303237722f4eb66099748f2fcef8240b4
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65205842"
---
# <a name="create-containers-with-large-partition-key"></a>Büyük bir bölüm anahtarı ile kapsayıcıları oluşturma

Azure Cosmos DB, yatay ölçeklemeyi veri elde etmek için karma tabanlı bölümleme şeması kullanır. 3 Mayıs 2019'den önce oluşturulan tüm Azure Cosmos kapsayıcılar bölüm anahtarı ilk 100 bayt olarak karmasını hesaplar bir karma işlevi kullanın. Aynı ilk 100 bayt olan birden çok bölüm anahtarları varsa, bu mantıksal bölümler aynı mantıksal birim hizmet tarafından değerlendirilir. Bu bölüm boyutu kotası yanlış olması ve bölüm anahtarı uygulanan benzersiz dizinler gibi sorunlara neden olabilir. Bu sorunu çözmek için büyük bölüm anahtarlarını sunulmuştur. En fazla 2 KB anahtarlarla destekler büyük bölümü artık Azure Cosmos DB değerleri. 

Büyük bölüm anahtarlarını benzersiz bir karması büyük bölümünden oluşturabilirsiniz karma işlevi genişletilmiş bir sürümü işlevselliğini kullanarak desteklenen en fazla 2 KB anahtarları. Bu karma sürüm senaryoları için bölüm anahtarı boyutunu bağımsız olarak anahtar kardinalite yüksek bölüm de önerilir. Bir bölüm anahtarı kardinalite, örneğin bir kapsayıcıda ~ 30000 mantıksal bölümler sırasına göre benzersiz bir mantıksal bölüm sayısı olarak tanımlanır. Bu makalede bir büyük bölüm anahtarı farklı Sdk'ler ve Azure portalını kullanarak bir kapsayıcı oluşturulacağını açıklar. 

## <a name="create-a-large-partition-key-net-sdk-v2"></a>Büyük bir bölüm anahtarı oluşturma (.Net SDK'sı V2)

Büyük bir bölüm anahtarıyla bir kapsayıcı oluşturmak için .net SDK'sı kullanırken belirtmelidir `PartitionKeyDefinitionVersion.V2` özelliği. Aşağıdaki örnek, Version özelliği PartitionKeyDefinition nesnesi içinde belirtin ve PartitionKeyDefinitionVersion.V2 için ayarlamak gösterilmektedir:

```csharp
DocumentCollection collection = await newClient.CreateDocumentCollectionAsync(
database,
     new DocumentCollection
        {
           Id = Guid.NewGuid().ToString(),
           PartitionKey = new PartitionKeyDefinition
           {
             Paths = new Collection<string> {"/longpartitionkey" },
             Version = PartitionKeyDefinitionVersion.V2
           }
         },
      new RequestOptions { OfferThroughput = 400 });
```

## <a name="create-a-large-partition-key-azure-portal"></a>Büyük bir bölüm anahtarı (Azure portalı) oluşturun 

Azure portalını kullanarak yeni bir kapsayıcı oluştururken bir büyük bölüm anahtarı oluşturmak için işaretleyin **My bölüm anahtarı 100 bayttan büyük** seçeneği. Varsayılan olarak, büyük bölüm anahtarlarını kullanarak tüm yeni kapsayıcılar devre dışı bırakılır. Onay kutusunun işaretini kaldırın büyük bölüm anahtarlarını ihtiyacınız yoksa veya 1.18 eski SDK sürümü üzerinde çalışan uygulamalar varsa.

![Azure portalını kullanarak büyük bölüm anahtarları oluşturma](./media/large-partition-keys/large-partition-key-with-portal.png)


## <a name="supported-sdk-versions"></a>Desteklenen SDK sürümleri

Büyük bölüm anahtarlarını SDK'ları minimum aşağıdaki sürümleriyle desteklenir:

|SDK'sı türü  | En düşük sürüm   |
|---------|---------|
|.NET     |    1.18     |
|Java eşitleme     |   2.4.0      |
|Java zaman uyumsuz   |  2.5.0        |
 
## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB'de bölümleme](partitioning-overview.md)
* [İstek birimleri Azure cosmos DB](request-units.md)
* [Kapsayıcılar ve veritabanları sağlama aktarım hızı](set-throughput.md)
* [Azure Cosmos hesabıyla çalışma](account-overview.md)


