---
title: Azure Cosmos DB değişiklik akışı ile Azure işlevleri'ni kullanma
description: Kullanım Azure Cosmos DB değişiklik akışı ile Azure işlevleri
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: 08429ca76823b9e6c80a197cc390a5964c4198e6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65969016"
---
# <a name="serverless-event-based-architectures-with-azure-cosmos-db-and-azure-functions"></a>Azure Cosmos DB ile Azure işlevleri ile olay tabanlı sunucusuz mimarileri

Azure işlevleri sağlayan bağlanmak için en basit yolu [değişiklik akışını](change-feed.md). Otomatik olarak tetiklenecek küçük reaktif Azure işlevleri, Azure Cosmos kapsayıcının değişiklik akışı her yeni olay oluşturabilirsiniz.

![Azure Cosmos DB tetikleyicisi ile çalışma olay tabanlı sunucusuz İşlevler](./media/change-feed-functions/functions.png)

İle [Azure Cosmos DB tetikleyicisi](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger), yararlanabileceğiniz [değişiklik akışı İşlemci](./change-feed-processor.md)ölçeklendirme kullanıcının ve güvenilir olay algılama işlevselliği tüm korumak zorunda kalmadan [çalışan Altyapı](./change-feed-processor.md#implementing-the-change-feed-processor-library). Olay kaynağını belirleme işlem hattının rest hakkında endişelenmeden yalnızca Azure işlevinizin mantığına odaklanabilir. Hatta tetikleyici diğer karıştırabilirsiniz [Azure işlevleri bağlamaları](../azure-functions/functions-triggers-bindings.md#supported-bindings).

> [!NOTE]
> Şu anda, Azure Cosmos DB tetikleyicisi ile çekirdek (SQL) API yalnızca kullanılması desteklenir.

## <a name="requirements"></a>Gereksinimler

Sunucusuz olay tabanlı akış uygulamak için gerekir:

* **İzlenen kapsayıcı**: İzlenmekte olan Azure Cosmos kapsayıcı izlenen kapsayıcısıdır ve değişiklik akışı oluşturulduğu veri depolar. Tüm eklemeleri ve değişiklikleri (örneğin, CRUD) izlenen kapsayıcı içinde kapsayıcının değişiklik akışı yansıtılır.
* **Kira kapsayıcı**: Kira kapsayıcı birden çok durumunu korur ve dinamik sunucusuz Azure işlevi, örnekler ve dinamik ölçeklendirme sağlar. Bu kira kapsayıcı el ile veya otomatik olarak Azure Cosmos DB Trigger.To tarafından otomatik olarak oluşturulabilir kira kapsayıcıyı oluşturun, Ayarla *CreateLeaseCollectionIfNotExists* bayrağını [yapılandırma](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger---configuration). Bölümlenmiş kira kapsayıcılardır sahip olması gereken bir `/id` bölüm anahtar tanımı.

## <a name="create-your-azure-cosmos-db-trigger"></a>Azure Cosmos DB tetikleyicisi oluşturma

Bir Azure Cosmos DB tetikleyicisi ile Azure işlevinizi oluşturma, artık tüm Azure işlevleri IDE ve CLI tümleştirmeler desteklenir:

* [Visual Studio Uzantısı](../azure-functions/functions-develop-vs.md) Visual Studio kullanıcılar için.
* [Visual Studio çekirdek uzantısı](https://code.visualstudio.com/tutorials/functions-extension/create-function) Visual Studio Code kullanıcılar için.
* Son olarak [Core CLI Araçları](../azure-functions/functions-run-local.md#create-func) platformlar arası IDE belirsiz bir deneyim.

## <a name="run-your-azure-cosmos-db-trigger-locally"></a>Azure Cosmos DB tetikleyici yerel olarak çalıştırma

Çalıştırabileceğiniz, [Azure işlevi yerel olarak](../azure-functions/functions-develop-local.md) ile [Azure Cosmos DB öykünücüsü'nü](./local-emulator.md) oluşturma ve bir Azure aboneliği veya masraf olmadan, sunucusuz, olay tabanlı akışları geliştirin.

Bulutta Canlı senaryolarını test etmek istiyorsanız, [Cosmos DB'yi ücretsiz deneyin](https://azure.microsoft.com/try/cosmosdb/) olmadan herhangi bir kredi kartı veya Azure aboneliği gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler de akış değiştirme hakkında daha fazla bilgi edinmek artık devam edebilirsiniz:

* [Değişiklik akışı genel bakış](change-feed.md)
* [Değişiklik akışını okumak için yollar](read-change-feed.md)
* [Kullanarak değişiklik akışı işlemci kitaplığı](change-feed-processor.md)
* [Değişiklik ile çalışma konusunda akışı işlemci kitaplığı](change-feed-processor.md)
* [Azure Cosmos DB ile Azure işlevleri'ni kullanarak sunucusuz veritabanı bilgi işlem](serverless-computing-database.md)
