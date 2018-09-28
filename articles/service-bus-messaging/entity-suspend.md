---
title: Azure Service Bus Mesajlaşma varlıklarını askıya alma | Microsoft Docs
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
ms.date: 09/26/2018
ms.author: spelluru
ms.openlocfilehash: bd75fd5c34a3518f6da0dbd56eca2ad152d60ba9
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47405725"
---
# <a name="suspend-and-reactivate-messaging-entities-disable"></a>Askıya alma ve mesajlaşma varlıkları (devre dışı bırak) yeniden etkinleştirme

Kuyruklar, konular ve abonelikler geçici olarak askıya alınabilir. Askıya alma varlığın tüm iletileri depolama alanında tutulur devre dışı bırakılmış bir duruma koyar. Ancak, iletileri kaldırıldı veya eklenemez ve ilgili protokol işlemlerini hataları yield.

Bir varlık askıya alma, genellikle Acil Yönetim amaçları için gerçekleştirilir. Senaryolardan biri Kuyruk iletilerini alan hatalı bir alıcı dağıtılan işlem başarısız olur ve henüz yanlış iletileri tamamlar ve bunları kaldırır. Bu davranışı tanı koydu, kuyruk için devre dışı bırakılabilir Düzeltilen kod dağıtılır ve başka hatalı kod tarafından kaynaklanan veri kayıplarını engellenebilir kadar alır.

Kullanıcı veya sistem tarafından askıya alma veya yeniden etkinleştirme gerçekleştirilebilir. Sistem, harcama limiti aboneliği ulaşma gibi grave yönetim nedenlerden dolayı varlıkları yalnızca askıya alır. Sistemi devre dışı varlıklar kullanıcı tarafından yeniden etkinleştirilemez, ancak askıya alınma nedenini ele alındığında geri yüklenir.

Portalda, **özellikleri** ilgili varlığın bölüm etkinleştirir durumunu değiştirme; bir kuyruk için iki durumlu düğme aşağıdaki ekran görüntüsünde gösterilmektedir:

![][1]

Portal, yalnızca tamamen kuyrukları devre dışı bırakma verir. Ayrıca gönderme devre dışı bırakma ve alma işlemleri ayrı olarak Service Bus'ı kullanarak [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) API'ler .NET Framework SDK veya Azure CLI veya Azure PowerShell ile Azure Resource Manager şablonu ile.

## <a name="suspension-states"></a>Askıya alma durumları

Bir kuyruk için ayarlanabilir durumlar şunlardır:

-   **Etkin**: sıra etkindir.
-   **Devre dışı bırakılmış**: sıra askıya alındı.
-   **SendDisabled**: sıra kısmen, izin verilen Al ile askıya alındı.
-   **ReceiveDisabled**: sıra kısmen, gönderme işlemine izin askıya alındı.

Abonelikler ve konular, yalnızca **etkin** ve **devre dışı bırakılmış** ayarlanabilir.

[EntityStatus](/dotnet/api/microsoft.servicebus.messaging.entitystatus) numaralandırması da bir dizi yalnızca sistem tarafından ayarlanabilir geçiş durumları tanımlar. Bir kuyruk devre dışı bırakmak için PowerShell komutunu aşağıdaki örnekte gösterilmiştir. Ayarı yeniden etkinleştirme komut eşdeğer `Status` için **etkin**.

```powershell
$q = Get-AzureRmServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue

$q.Status = "Disabled"

Set-AzureRmServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue -QueueObj $q
```

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşması hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus ile ilgili temel bilgiler](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/entity-suspend/queue-disable.png

