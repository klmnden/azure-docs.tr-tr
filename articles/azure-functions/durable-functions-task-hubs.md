---
title: Görev hub dayanıklı işlevlerinde - Azure
description: Bilgi dayanıklı işlevleri uzantısı Azure işlevleri için hangi görev hub bulunmaktadır. Nasıl yapılandırılacağını öğrenmek görev hub'ları yapılandırın.
services: functions
author: cgillum
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 563667684accf8b434052cd412bf6e93c77ea63a
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33762335"
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
* Bir örnek tablo.
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
