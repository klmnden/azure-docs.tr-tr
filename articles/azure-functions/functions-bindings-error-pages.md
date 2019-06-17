---
title: Azure işlevleri hata işleme Kılavuzu | Microsoft Docs
description: İşlevlerinizi yürüttüğünüzde içinde oluşan hataları ve özel bağlama hataları konulara bağlantılar işleme için genel rehberlik sağlar.
services: functions
cloud: ''
documentationcenter: ''
author: craigshoemaker
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 02/01/2018
ms.author: cshoe
ms.openlocfilehash: bf54d312de5625a7fa44cea4d5107e83cf15583c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62105517"
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
