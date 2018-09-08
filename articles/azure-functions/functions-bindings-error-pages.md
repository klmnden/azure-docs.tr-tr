---
title: Azure işlevleri hata işleme Kılavuzu | Microsoft Docs
description: İşlevlerinizi yürüttüğünüzde içinde oluşan hataları ve özel bağlama hataları konulara bağlantılar işleme için genel rehberlik sağlar.
services: functions
cloud: ''
documentationcenter: ''
author: ggailey777
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 02/01/2018
ms.author: glenga; cfowler
ms.openlocfilehash: 5a8dae73c164b319b4c291685deff402f9798364
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44091855"
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
