---
title: Azure Service Bus Mesajlaşma varlıklarını askıya alma | Microsoft Docs
description: Askıya alma ve Azure Service Bus ileti varlıkları yeniden etkinleştirin.
services: service-bus-messaging
documentationcenter: ''
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: e2ffda3141462d19557af3af26c117ee505c40ab
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59047356"
---
# <a name="suspend-and-reactivate-messaging-entities-disable"></a>Askıya alma ve mesajlaşma varlıkları (devre dışı bırak) yeniden etkinleştirme

Kuyruklar, konular ve abonelikler geçici olarak askıya alınabilir. Askıya alma varlığın tüm iletileri depolama alanında tutulur devre dışı bırakılmış bir duruma koyar. Ancak, iletileri kaldırıldı veya eklenemez ve ilgili protokol işlemlerini hataları yield.

Bir varlık askıya alma, genellikle Acil Yönetim amaçları için gerçekleştirilir. Senaryolardan biri Kuyruk iletilerini alan hatalı bir alıcı dağıtılan işlem başarısız olur ve henüz yanlış iletileri tamamlar ve bunları kaldırır. Bu davranışı tanı koydu, kuyruk için devre dışı bırakılabilir Düzeltilen kod dağıtılır ve başka hatalı kod tarafından kaynaklanan veri kayıplarını engellenebilir kadar alır.

Kullanıcı veya sistem tarafından askıya alma veya yeniden etkinleştirme gerçekleştirilebilir. Sistem, harcama limiti aboneliği ulaşma gibi grave yönetim nedenlerden dolayı varlıkları yalnızca askıya alır. Sistemi devre dışı varlıklar kullanıcı tarafından yeniden etkinleştirilemez, ancak askıya alınma nedenini ele alındığında geri yüklenir.

Portalda, **özellikleri** ilgili varlığın bölüm sağlar durumunu değiştirme; bir kuyruk için iki durumlu düğme aşağıdaki ekran görüntüsünde gösterilmektedir:

![][1]

Portal, yalnızca tamamen kuyrukları devre dışı bırakma verir. Ayrıca gönderme devre dışı bırakma ve alma işlemleri ayrı olarak Service Bus'ı kullanarak [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) API'ler .NET Framework SDK veya Azure CLI veya Azure PowerShell ile Azure Resource Manager şablonu ile.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="suspension-states"></a>Askıya alma durumları

Bir kuyruk için ayarlanabilir durumlar şunlardır:

-   **Etkin**: Sıranın etkin değil.
-   **Devre dışı bırakılmış**: Sıranın askıya alınır.
-   **SendDisabled**: Sıranın kısmen, izin verilen Al ile askıya alınır.
-   **ReceiveDisabled**: Sıranın kısmen, gönderme işlemine izin askıya alınır.

Abonelikler ve konular, yalnızca **etkin** ve **devre dışı bırakılmış** ayarlanabilir.

[EntityStatus](/dotnet/api/microsoft.servicebus.messaging.entitystatus) numaralandırması da bir dizi yalnızca sistem tarafından ayarlanabilir geçiş durumları tanımlar. Bir kuyruk devre dışı bırakmak için PowerShell komutunu aşağıdaki örnekte gösterilmiştir. Ayarı yeniden etkinleştirme komut eşdeğer `Status` için **etkin**.

```powershell
$q = Get-AzServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue

$q.Status = "Disabled"

Set-AzServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue -QueueObj $q
```

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşması hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/entity-suspend/queue-disable.png

