---
title: Saklı yordamları ve Tetikleyicileri kullanarak Azure Cosmos DB'de JavaScript sorgu API'si yazma
description: Saklı yordamları ve Tetikleyicileri JavaScript sorgu API'si kullanarak Azure Cosmos DB'de yazmayı öğrenin
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/23/2019
ms.author: mjbrown
ms.openlocfilehash: f465ac91936b766d2c19ea8efd67b3acc8df6d75
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66243936"
---
# <a name="how-to-write-stored-procedures-and-triggers-in-azure-cosmos-db-by-using-the-javascript-query-api"></a>Saklı yordamları ve Tetikleyicileri JavaScript sorgu API'si kullanarak Azure Cosmos DB'de yazma

Azure Cosmos DB, saklı yordamları ve Tetikleyicileri yazmak için kullanılan SQL dil bilgileri olmadan fluent bir JavaScript arabirimi kullanarak en iyi duruma getirilmiş sorguları gerçekleştirmeyi sağlar. Azure Cosmos DB'de JavaScript sorgu API'si desteği hakkında daha fazla bilgi edinmek için [çalışma JavaScript dil ile tümleşik sorgu API'si Azure Cosmos DB'de](javascript-query-api.md) makalesi.

## <a id="stored-procedures"></a>Saklı yordamı kullanarak JavaScript sorgu API'si

Aşağıdaki kod örneği bir saklı yordam bağlamında kullanılan JavaScript sorgu API'si nasıl bir örnektir. Saklı yordam giriş parametresi tarafından belirtilen ve kullanarak bir meta veri belgesinin güncelleştiren bir Azure Cosmos DB öğesi ekler `__.filter()` minSize, Maxsıze ve giriş öğesinin boyut özelliği dayalı totalSize yöntemi.

> [!NOTE]
> `__` (çift alt çizgi) olan bir diğer `getContext().getCollection()` JavaScript sorgu API'si kullanılırken.

```javascript
/**
 * Insert an item and update metadata doc: minSize, maxSize, totalSize based on item.size.
 */
function insertDocumentAndUpdateMetadata(item) {
  // HTTP error codes sent to our callback function by CosmosDB server.
  var ErrorCode = {
    RETRY_WITH: 449,
  }

  var isAccepted = __.createDocument(__.getSelfLink(), item, {}, function(err, item, options) {
    if (err) throw err;

    // Check the item (ignore items with invalid/zero size and metadata itself) and call updateMetadata.
    if (!item.isMetadata && item.size > 0) {
      // Get the metadata. We keep it in the same container. it's the only item that has .isMetadata = true.
      var result = __.filter(function(x) {
        return x.isMetadata === true
      }, function(err, feed, options) {
        if (err) throw err;

        // We assume that metadata item was pre-created and must exist when this script is called.
        if (!feed || !feed.length) throw new Error("Failed to find the metadata item.");

        // The metadata item.
        var metaItem = feed[0];

        // Update metaDoc.minSize:
        // for 1st document use doc.Size, for all the rest see if it's less than last min.
        if (metaItem.minSize == 0) metaItem.minSize = item.size;
        else metaItem.minSize = Math.min(metaItem.minSize, item.size);

        // Update metaItem.maxSize.
        metaItem.maxSize = Math.max(metaItem.maxSize, item.size);

        // Update metaItem.totalSize.
        metaItem.totalSize += item.size;

        // Update/replace the metadata item in the store.
        var isAccepted = __.replaceDocument(metaItem._self, metaItem, function(err) {
          if (err) throw err;
          // Note: in case concurrent updates causes conflict with ErrorCode.RETRY_WITH, we can't read the meta again
          //       and update again because due to Snapshot isolation we will read same exact version (we are in same transaction).
          //       We have to take care of that on the client side.
        });
        if (!isAccepted) throw new Error("replaceDocument(metaItem) returned false.");
      });
      if (!result.isAccepted) throw new Error("filter for metaItem returned false.");
    }
  });
  if (!isAccepted) throw new Error("createDocument(actual item) returned false.");
}
```

## <a name="next-steps"></a>Sonraki adımlar

Saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler, Azure Cosmos DB hakkında bilgi edinmek için aşağıdaki makalelere bakın:

* [Saklı yordamlar, Tetikleyiciler, Azure Cosmos DB'de kullanıcı tanımlı işlevleri ile çalışmaya nasıl](how-to-use-stored-procedures-triggers-udfs.md)

* [Azure Cosmos DB'de saklı yordamları nasıl kaydetme ve kullanma](how-to-use-stored-procedures-triggers-udfs.md#stored-procedures)

* Kaydolun ve kullanmak nasıl [öncesi Tetikleyicileri](how-to-use-stored-procedures-triggers-udfs.md#pre-triggers) ve [sonrası Tetikleyicileri](how-to-use-stored-procedures-triggers-udfs.md#post-triggers) Azure Cosmos DB'de

* [Kaydolun ve Azure Cosmos DB'de kullanıcı tanımlı işlevler kullanma](how-to-use-stored-procedures-triggers-udfs.md#udfs)

* [Azure Cosmos DB'de yapay bölümleme anahtarları](synthetic-partition-keys.md)
