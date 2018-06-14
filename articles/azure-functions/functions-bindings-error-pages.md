---
title: Azure işlevleri hata Kılavuzu işleme | Microsoft Docs
description: İşlevlerinizi yürütme zaman içinde oluşan hataları ve bağlama özgü hatalar konulara bağlantılar işlemek için genel rehberlik sağlar.
services: functions
cloud: ''
documentationcenter: ''
author: ggailey777
manager: cfowler
ms.assetid: ''
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: article
ms.date: 02/01/2018
ms.author: glenga; cfowler
ms.openlocfilehash: 82cdc62b3070811186583fdf1ce5e6ce421ebc34
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
ms.locfileid: "29118492"
---
# <a name="azure-functions-error-handling"></a>Azure işlevleri hata işleme

Bu konu işlevlerinizi yürüttüğünüzde oluşan hataları işlemek için genel rehberlik sağlar. Ayrıca, oluşabilecek bağlama özgü hatalar açıklayan konulara bağlantılar da sağlar. 

## <a name="handing-errors-in-functions"></a>Hataları işlevlerinde teslim etme
[!INCLUDE [bindings errors intro](../../includes/functions-bindings-errors-intro.md)]

 
## <a name="binding-error-codes"></a>Bağlama hata kodları

Azure hizmetleriyle tümleştirme, temel alınan hizmetler API'lerden kaynaklanan ortaya hatalarının olabilir. Bu hizmetleri bulunabilir için hata bağlantılar kod belgelerine **özel durumlar ve dönüş kodları** aşağıdaki tetikleyici ve bağlama başvuru konuları bölümünde:

+ [Azure Cosmos DB](functions-bindings-cosmosdb.md#exceptions-and-return-codes)

+ [Blob depolama](functions-bindings-storage-blob.md#exceptions-and-return-codes)

+ [Event Hubs](functions-bindings-event-hubs.md#exceptions-and-return-codes)

+ [Notification Hubs](functions-bindings-notification-hubs.md#exceptions-and-return-codes)

+ [Kuyruk depolama](functions-bindings-storage-queue.md#exceptions-and-return-codes)

+ [Service Bus](functions-bindings-service-bus.md#exceptions-and-return-codes)

+ [Tablo depolama](functions-bindings-storage-table.md#exceptions-and-return-codes)
