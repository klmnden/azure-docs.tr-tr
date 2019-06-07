---
title: Azure Cosmos DB tetikleyicisi bağlantı İlkesi
description: Azure Cosmos DB tetikleyicisi tarafından kullanılan bağlantı ilkesi yapılandırma hakkında bilgi edinin
author: ealsur
ms.service: cosmos-db
ms.topic: sample
ms.date: 06/05/2019
ms.author: maquaran
ms.openlocfilehash: 584d59884b70d2ee8243216e6f907fc9ec2d8ad4
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66755335"
---
# <a name="how-to-configure-the-connection-policy-used-by-azure-cosmos-db-trigger"></a>Azure Cosmos DB tetikleyicisi tarafından kullanılan bağlantı ilkesi yapılandırma

Bu makalede, Azure Cosmos hesabınıza bağlanmak için Azure Cosmos DB tetikleyicisi kullanırken bağlantı ilkesini nasıl yapılandıracağınız açıklanır.

## <a name="why-is-the-connection-policy-important"></a>Bağlantı İlkesi neden önemlidir?

İki bağlantı modunu - doğrudan mod ve ağ geçidi vardır. Bu bağlantı modları hakkında daha fazla bilgi için bkz. [performans ipuçları](./performance-tips.md#networking) makalesi. Varsayılan olarak, **ağ geçidi** Azure Cosmos DB tetikleyicisi tüm bağlantıları kurmak için kullanılır. Ancak, performans temelli senaryolar için en iyi seçenek olmayabilir.

## <a name="changing-the-connection-mode-and-protocol"></a>Protokol ve bağlantı modunu değiştirme

İstemci bağlantı İlkesi – yapılandırmak kullanılabilen iki anahtar yapılandırma ayarları vardır **bağlantı modu** ve **bağlantı protokolü**. Azure Cosmos DB tetikleyicisi ve tüm tarafından kullanılan protokolü ve varsayılan bağlantı modu değiştirebilirsiniz [Azure Cosmos DB bağlamaları](../azure-functions/functions-bindings-cosmosdb-v2.md#output)). Varsayılan ayarları değiştirmek için bulunacak gerekir `host.json` , Azure işlevleri projesi ya da Azure işlev uygulaması ve aşağıdakileri ekleyin [ek ayar](../azure-functions/functions-bindings-cosmosdb-v2.md#hostjson-settings):

```js
{
  "cosmosDB": {
    "connectionMode": "Direct",
    "protocol": "Tcp"
  }
}
```

Burada `connectionMode` istenen bağlantı modu (doğrudan veya ağ geçidi) olmalıdır ve `protocol` istenen bağlantı Protokolü (Tcp veya Https). 

Azure işlevleri projenizi Azure işlevleri V1 çalışma zamanı ile çalışıyorsanız yapılandırılmış bir hafif adı fark, kullanmalısınız `documentDB` yerine `cosmosDB`:

```js
{
  "documentDB": {
    "connectionMode": "Direct",
    "protocol": "Tcp"
  }
}
```

> [!NOTE]
> Azure işlevleri tüketim planı barındırma planı ile çalışırken, her örneği, koruyabilirsiniz soket bağlantılarının miktarında bir sınırı vardır. Direct ile çalışırken / TCP modunu, tasarım daha fazla bağlantı oluşturulur ve can isabet [tüketim planı sınırı](../azure-functions/manage-connections.md#connection-limit), bu durumda, ağ geçidi modunu kullanabilir veya, Azure işlevleri'ni çalıştırma [App Service modu](../azure-functions/functions-scale.md#app-service-plan).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure işlevleri'nde bağlantı sınırları](../azure-functions/manage-connections.md#connection-limit)
* [Azure Cosmos DB performans ipuçları](./performance-tips.md)
* [Kod örnekleri](https://github.com/ealsur/serverless-recipes/tree/master/connectionmode)
