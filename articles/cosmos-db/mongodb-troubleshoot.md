---
title: Mongo DB için Azure Cosmos DB'nin API'sindeki sık karşılaşılan hataları giderme
description: Bu belge, MongoDB için Azure Cosmos DB'nin API'SİNDE karşılaşılan genel sorunları gidermek için yollar ele alınmaktadır.
author: roaror
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 06/05/2019
ms.author: roaror
ms.openlocfilehash: 5b3d3993a497240c1ea18f0fcf852c0e834f6e79
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66735714"
---
# <a name="troubleshoot-common-issues-in-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Azure Cosmos DB'nin API'sindeki yaygın sorunlarını giderme

Azure Cosmos DB MongoDB dahil olmak üzere sık kullanılan NoSQL veritabanları kablo protokollerin uygular. Hat üzeri protokol uygulanması nedeniyle, mevcut istemci SDK'ları, sürücüler ve NoSQL veritabanları ile çalışan araçları kullanarak şeffaf bir şekilde Azure Cosmos DB ile etkileşim kurabilir. Azure Cosmos DB, kablo ile uyumlu API'leri herhangi bir NoSQL veritabanı sağlamak için herhangi bir kaynak kod veritabanı kullanmaz. Azure Cosmos DB'ye kablo protokolü sürümleri anlayan herhangi bir MongoDB istemcisi sürücüsünü bağlanabilirsiniz.

Azure Cosmos DB'nin MongoDB API'si, Mongodb'nin kablo protokolünü (sorgu işleçlerini ve 3.4 sürümünde eklenen özellikler şu anda önizleme olarak kullanılabilir) 3.2 sürümü ile uyumlu olsa da, Azure Cosmos DB için karşılık gelen bazı özel hata kodları vardır belirli hataları. Bu makalede, farklı hatalar, hata kodları ve bu hataları gidermek için adımları açıklanmaktadır.

## <a name="common-errors-and-solutions"></a>Genel hatalar ve çözümleri

| Hata               | Kod  | Açıklama  | Çözüm  |
|---------------------|-------|--------------|-----------|
| TooManyRequests     | 16500 | İstek birimleri tüketilen toplam sayısı, koleksiyon için sağlanan istek birimi hızdan daha ve kısıtlanan. | Azure portalından bir kapsayıcı veya bir dizi kapsayıcıları atanan aktarım hızını ölçeklendirmeyi düşünün veya işlemi yeniden deneyin. |
| ExceededMemoryLimit | 16501 | Çok kiracılı bir hizmet, istemcinin bellek birimi işlemi geçti. | Destek ile iletişime geçin veya daha kısıtlayıcı bir sorgu ölçütü ile bir işlem kapsamını azaltın [Azure portalında](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Örnek: `db.getCollection('users').aggregate([{$match: {name: "Andy"}}, {$sort: {age: -1}}]))` |
| MongoDB kablo sürüm sorunları | - | MongoDB sürücüleri eski sürümleri Azure Cosmos hesabın adını, bağlantı dizelerini algılayamaz. | Append *appName = @**accountName** @*  MongoDB bağlantı dizesi, Cosmos DB'nin API'sini sonunda burada ***accountName*** Cosmos DB hesabının adıdır . |


## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [Studio 3T kullanma](mongodb-mongochef.md) Azure Cosmos DB'nin MongoDB API'si ile.
- Bilgi edinmek için nasıl [Robo 3T kullanma](mongodb-robomongo.md) Azure Cosmos DB'nin MongoDB API'si ile.
- MongoDB keşfedin [örnekleri](mongodb-samples.md) Azure Cosmos DB'nin MongoDB API'si ile.

