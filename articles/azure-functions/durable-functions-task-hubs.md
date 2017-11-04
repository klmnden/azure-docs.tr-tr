---
title: "Görev hub dayanıklı işlevlerinde - Azure"
description: "Bilgi dayanıklı işlevleri uzantısı Azure işlevleri için hangi görev hub bulunmaktadır. Nasıl yapılandırılacağını öğrenmek görev hub'ları yapılandırın."
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: b241bad7b0060551eba5e78efbb1b729bf5d0098
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2017
---
# <a name="task-hubs-in-durable-functions-azure-functions"></a>Görev hub dayanıklı işlevlerinde (Azure işlevleri)

A *görev hub* içinde [dayanıklı işlevleri](durable-functions-overview.md) düzenlemelerin için kullanılan Azure Storage kaynakları için mantıksal bir kapsayıcısıdır. Aynı görevi hub'ına ait olduğunda orchestrator ve etkinlik işlevleri yalnızca birbirleriyle etkileşim kuramazlar.

Her işlev uygulaması ayrı görev hub sahiptir. Birden çok işlev uygulamalarının bir depolama hesabı paylaşıyorsanız, birden çok görev hub depolama hesabını içerir. Aşağıdaki diyagram, paylaşılan ve ayrılmış depolama hesaplarındaki işlev uygulaması başına bir görev hub gösterir.

![Diyagram gösteren paylaşılan ve depolama hesapları ayrılmış.](media/durable-functions-task-hubs/task-hubs-storage.png)

## <a name="azure-storage-resources"></a>Azure Storage kaynakları

Bir görev hub'ı aşağıdaki depolama kaynaklarını oluşur: 

* Bir veya daha fazla denetim sıralar.
* Bir iş öğesi sırası.
* Bir geçmiş tablosu.
* Bir veya daha fazla kira BLOB içeren bir depolama kapsayıcısı.

Orchestrator veya etkinlik işlevleri çalıştırdığınızda veya çalıştırmak için zamanlanan tüm bu kaynaklar varsayılan Azure depolama hesabında otomatik olarak oluşturulur. [Performansı ve ölçeği](durable-functions-perf-and-scale.md) makale, bu kaynakların nasıl kullanıldığını açıklar.

## <a name="task-hub-names"></a>Görev hub adları

Görev hub içinde bildirilen bir ad tarafından tanımlanır *host.json* , aşağıdaki örnekte gösterildiği gibi dosya:

```json
{
  "durableTask": {
    "HubName": "MyTaskHub"
  }
}
```

Görev hub adları bir harfle başlamalı ve yalnızca harf ve sayılardan oluşur. Belirtilmezse, varsayılan ad olan **DurableFunctionsHub**.

> [!NOTE]
> Paylaşılan depolama hesabında birden çok görev hub olduğunda ne bir görev hub diğerlerinden ayıran adıdır. Birden çok işlev uygulamalarının bir paylaşılan depolama hesabı paylaşımı varsa, her görev hub için farklı adlar yapılandırmak zorunda *host.json* dosyaları.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Sürüm oluşturma nasıl ele alınacağını öğrenin](durable-functions-versioning.md)
