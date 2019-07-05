---
title: Azure işlevleri hata işleme Kılavuzu | Microsoft Docs
description: İşlevlerinizi yürüttüğünüzde içinde oluşan hataları ve özel bağlama hataları konulara bağlantılar işleme için genel rehberlik sağlar.
services: functions
cloud: ''
documentationcenter: ''
author: craigshoemaker
manager: gwallace
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 02/01/2018
ms.author: cshoe
ms.openlocfilehash: ef8f2d5a63f7924097362f6aa0ebc78cc0f6455f
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67480702"
---
# <a name="azure-functions-error-handling"></a>Azure işlevleri hata işleme

Bu konu, işlevlerinizin yürüttüğünüzde, oluşan hataları ele alma için genel rehberlik sağlar. Ayrıca, oluşabilecek bağlama özgü hatalar açıklayan konulara bağlantılar sağlar. 

## <a name="handing-errors-in-functions"></a>İşleme hataları işlevleri
[!INCLUDE [bindings errors intro](../../includes/functions-bindings-errors-intro.md)]

 
## <a name="binding-error-codes"></a>Bağlama hata kodları

Azure Hizmetleri ile tümleştirme, temel alınan hizmetler API'lerinden kaynaklanan yükseltilmiş hataları olabilir. Hata bağlantıları bu hizmetler bulunabilir için belgeleri kod **özel durumlar ve dönüş kodları** aşağıdaki tetikleyici ve bağlama başvuru konuları bölümünde:

+ [Azure Cosmos DB](functions-bindings-cosmosdb.md#exceptions-and-return-codes)

+ [Blob depolama](functions-bindings-storage-blob.md#exceptions-and-return-codes)

+ [Event Hubs](functions-bindings-event-hubs.md#exceptions-and-return-codes)

+ [Notification Hubs](functions-bindings-notification-hubs.md#exceptions-and-return-codes)

+ [Kuyruk depolama](functions-bindings-storage-queue.md#exceptions-and-return-codes)

+ [Service Bus](functions-bindings-service-bus.md#exceptions-and-return-codes)

+ [Tablo depolama](functions-bindings-storage-table.md#exceptions-and-return-codes)
