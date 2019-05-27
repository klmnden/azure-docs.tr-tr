---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: c1784111cd2fc2c93b67510f310b9e513cf2b86e
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66132476"
---
Azure işlevleri [Tetikleyicileri ve bağlamaları](../articles/azure-functions/functions-triggers-bindings.md) çeşitli Azure Hizmetleri ile iletişim kurar. Bu hizmetler ile tümleştirdiğinizde, temel alınan Azure hizmetlerini API'lerinden kaynaklanan yükseltilmiş hataları olabilir. REST veya istemci kitaplıklarını kullanarak, işlev kodunuzun diğer hizmetleriyle iletişim kurmak çalıştığınızda hatalar da meydana gelebilir. Veri kaybını önlemek ve işlevlerinizi iyi davranışı sağlamak için kaynak hatalarını işlemek önemlidir.

Aşağıdaki tetikleyicilerden yerleşik yeniden deneme desteği vardır:

* [Azure Blob Depolama](../articles/azure-functions/functions-bindings-storage-blob.md)
* [Azure kuyruk depolama](../articles/azure-functions/functions-bindings-storage-queue.md)
* [Azure Service Bus (kuyruk/konu)](../articles/azure-functions/functions-bindings-service-bus.md)

Varsayılan olarak, bu Tetikleyiciler en fazla beş kez yeniden denenir. Beşinci yeniden denedikten sonra bu Tetikleyiciler için özel bir ileti yazmak [zehirli kuyruk](../articles/azure-functions/functions-bindings-storage-queue.md#trigger---poison-messages).

Diğer işlevler Tetikleyicileri için yoktur yerleşik bir yeniden deneme işlevi yürütme sırasında hatalar oluştuğunda. Bir hata işlevinizde gerçekleşmelidir tetikleyici bilgi kaybını önlemek için hataları yakalamak için işlev kodunuzu try-catch bloklarını kullanmanızı öneririz. Bir hata oluştuğunda, özel "zehirli" ileti kuyruğuna tetikleyicisini işleve geçirilen bilgileri yazın. Bu yaklaşım tarafından kullanılan hizmet örneğiyle aynı olan [Blob Depolama tetikleyicisi](../articles/azure-functions/functions-bindings-storage-blob.md#trigger---poison-blobs).

Bu şekilde, hataları nedeniyle kaybolur ve depolanan bilgileri kullanarak zehirli kuyruktan iletilerini işlemek için başka bir işlev kullanmayı daha sonraki bir zamanda bunları yeniden deneme olayları tetiklemeyi yakalayabilirsiniz.  
