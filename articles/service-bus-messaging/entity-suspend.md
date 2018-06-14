---
title: Azure Service Bus Mesajlaşma varlıkları askıya alma | Microsoft Docs
description: Askıya alma ve Azure Service Bus ileti varlıkları yeniden etkinleştirin.
services: service-bus-messaging
documentationcenter: ''
author: clemensv
manager: timlt
editor: ''
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: sethm
ms.openlocfilehash: 1984b113f695107f8d4d80e5bbf25c7dc39d13f6
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
ms.locfileid: "28197035"
---
# <a name="suspend-and-reactivate-messaging-entities-disable"></a>Askıya alma ve mesajlaşma varlıkları (devre dışı bırak) yeniden etkinleştirin

Kuyruklar, konu başlıkları ve abonelikleri geçici olarak askıya alınabilir. Askıya tüm iletileri deposunda saklanır devre dışı durumuna varlığı yerleştirir. Ancak, iletileri kaldırılamaz veya eklenen ve ilgili protokol işlemlerini hataları verim.

Bir varlık askıya genellikle Acil Yönetimsel nedenlerden dolayı gerçekleştirilir. Bir senaryo sıra dışı iletileri alan hatalı bir alıcı dağıtılan başarısız işlem, henüz yanlış iletileri tamamlandıktan ve bunları kaldırır. Bu davranışı tanı koydu, sıranın devre dışı bırakılabilir düzeltilmiş kod dağıtılır ve daha fazla hatalı kodla kaynaklanan veri kaybı önlenmiş kadar alır.

Kullanıcı veya sistem tarafından askıya alma veya yeniden etkinleştirme gerçekleştirilebilir. Sistem, yalnızca varlık harcama sınırı abonelik basarsa gibi işareti Yönetimsel nedenlerden dolayı askıya alır. Devre dışı sistem varlıkları kullanıcı tarafından yeniden Etkinleştirilemeyecek ancak askıya nedenini ele yüklediğinizde geri.

Portalda, **özellikleri** ilgili varlığın bölüm sağlar durumunu değiştirme; aşağıdaki ekran görüntüsünde iki durumlu bir kuyruk için gösterir:

![][1]

Portal, yalnızca tamamen sıralarını devre dışı bırakma verir. Ayrıca gönderme devre dışı bırakma ve alma işlemleri ayrı ayrı hizmet veri yolu kullanarak [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) API'leri .NET Framework SDK veya Azure CLI veya Azure PowerShell ile bir Azure Resource Manager şablonu ile.

## <a name="suspension-states"></a>Askıya alma durumları

Bir kuyruk için ayarlanabilir durumlar şunlardır:

-   **Etkin**: sırası etkindir.
-   **Devre dışı**: sıranın askıya alınır.
-   **SendDisabled**: sıranın kısmen, izin verilen alma ile askıya alındı.
-   **ReceiveDisabled**: sıranın kısmen, izin verilen gönderme ile askıya alındı.

Abonelikler ve yalnızca konularını **etkin** ve **devre dışı** ayarlanabilir.

[EntityStatus](/dotnet/api/microsoft.servicebus.messaging.entitystatus) numaralandırma ayrıca yalnızca sistem tarafından ayarlanabilir geçici durumlar kümesini tanımlar. Bir kuyruk devre dışı bırakmak için PowerShell komutunu aşağıdaki örnekte gösterilir. Yeniden etkinleştirme komutu ayarlama, eşdeğerdir `Status` için **etkin**.

```powershell
$q = Get-AzureRmServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue

$q.Status = "Disabled"

Set-AzureRmServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue -QueueObj $q
```

## <a name="next-steps"></a>Sonraki adımlar

Service Bus Mesajlaşma hizmeti hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [Service Bus ile ilgili temel bilgiler](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/entity-suspend/queue-disable.png

