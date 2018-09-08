---
title: Dayanıklı işlevler - Azure, görev hub'ları
description: Bilgi dayanıklı işlevler uzantısını Azure işlevleri için bir görev hub bulunmaktadır. Yapılandırma konusunda bilgi görev hub'ları yapılandırın.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 53c70d4407222a80a9bc948db51294cd3e4e1bb4
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44092844"
---
# <a name="task-hubs-in-durable-functions-azure-functions"></a>Dayanıklı işlevler (Azure işlevleri), görev hub'ları

A *görev hub* içinde [dayanıklı işlevler](durable-functions-overview.md) düzenlemeleri için kullanılan Azure Storage kaynakları için mantıksal bir kapsayıcıdır. Aynı görev hub'ına ait oldukları zaman orchestrator ve etkinlik işlevleri yalnızca birbiriyle etkileşim kurabilir.

Her işlev uygulaması ayrı görev hub sahiptir. Depolama hesabı, birden fazla işlev uygulamasına bir depolama hesabı paylaşıyorsa, birden çok görev merkezleri içerir. Aşağıdaki diyagram, paylaşılan ve ayrılmış depolama hesaplarında işlev uygulaması başına tek bir görev hub gösterir.

![Gösteren diyagram, paylaşılan ve ayrılmış depolama hesapları.](media/durable-functions-task-hubs/task-hubs-storage.png)

## <a name="azure-storage-resources"></a>Azure Storage kaynakları

Bir görev hub'ı aşağıdaki depolama kaynaklarını oluşur: 

* Bir veya daha fazla denetim sıralar.
* Bir iş öğesi kuyruk.
* Bir geçmiş tablosu.
* Bir örnek tablo.
* Bir veya daha fazla kira bloblarını içeren bir depolama kapsayıcısı.

Orchestrator veya etkinlik işlevlerini çalıştırdığınızda veya çalıştırmak için zamanlanan tüm bu kaynaklar varsayılan Azure depolama hesabında otomatik olarak oluşturulur. [Performansı ve ölçeği](durable-functions-perf-and-scale.md) makalede, bu kaynakları nasıl kullanıldığı açıklanmaktadır.

## <a name="task-hub-names"></a>Görev hub adları

Görev merkezleri içinde bildirilen bir ad tarafından tanımlanır *host.json* aşağıdaki örnekte gösterildiği gibi dosya:

```json
{
  "durableTask": {
    "HubName": "MyTaskHub"
  }
}
```

Görev hub adları bir harf ile başlamalı ve yalnızca harf ve sayı oluşur. Belirtilmezse, varsayılan addır **DurableFunctionsHub**.

> [!NOTE]
> Paylaşılan depolama hesabında birden fazla görev merkezleri olduğunda ne bir görev hub birbirinden ayırır adıdır. Paylaşılan depolama hesabı paylaşımı birden fazla işlev uygulaması varsa, her görev hub için farklı adlar yapılandırmak zorunda *host.json* dosyaları.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Sürüm oluşturma nasıl ele alınacağını öğrenin](durable-functions-versioning.md)
