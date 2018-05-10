---
title: Azure olay kılavuz olay işleyicileri
description: İçin Azure olay kılavuz desteklenen olay işleyicileri açıklar
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 05/04/2018
ms.author: tomfitz
ms.openlocfilehash: 01a49ec47b406b7b916ecad5ef9ccc3af295fac1
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="event-handlers-in-azure-event-grid"></a>Azure olay kılavuzunda olay işleyicileri

Olay işleyici olay burada gönderilen yerdir. İşleyicisi olayını işlemek için bazı başka bir eylem alır. Çeşitli Azure hizmetlerine olayları işlemek için otomatik olarak yapılandırılır. Tüm Web kancası olayları işlemek için de kullanabilirsiniz. Web kancası olayları işlemek için Azure üzerinde barındırılan gerekmez.

Bu makalede her olay işleyicisi içeriğe bağlantılar sağlar.

## <a name="azure-automation"></a>Azure Otomasyonu

Otomatik runbook olayları işlemek için Azure Otomasyonu kullanın.

|Başlık  |Açıklama  |
|---------|---------|
|[Azure Otomasyonu olay kılavuz ve Microsoft ekipleri ile tümleştirme](ensure-tags-exists-on-new-virtual-machines.md) |Bir olay gönderen bir sanal makine, oluşturun. Sanal Makine etiketleri ve bir Microsoft Teams kanalına gönderilen bir ileti tetikleyen bir Otomasyon runbook'u olay tetiklenir. |

## <a name="azure-functions"></a>Azure İşlevleri

Azure işlevleri sunucusuz olaylarına yanıt olarak kullanın.

|Başlık  |Açıklama  |
|---------|---------|
| [Azure işlevleri için olay kılavuz tetikleyici](../azure-functions/functions-bindings-event-grid.md) | Olay kılavuz tetikleyici işlevler kullanılarak genel bakış. |
| [Karşıya yüklenen görüntüleri yeniden boyutlandırmayı Event Grid kullanarak otomatikleştirme](resize-images-on-storage-blob-upload-event.md) | Kullanıcılar web uygulaması aracılığıyla görüntüleri depolama hesabına yükleyin. Depolama blobu oluşturulduğunda, olay kılavuz karşıya yüklenen resmi yeniden boyutlandırır işlev uygulaması için bir olay gönderir. |
| [Veri ambarına büyük veri akışı yapma](event-grid-event-hubs-integration.md) | Olay hub'ları yakalama dosyası oluşturduğunda, olay kılavuz işlev uygulaması için bir olay gönderir. Uygulama yakalama dosyasını alır ve bir veri ambarına veri taşır. |
| [Azure olay kılavuz tümleştirme örnekler için Azure hizmet veri yolu](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Olay kılavuz, uygulama ve mantıksal uygulama çalışması için Service Bus konusundan iletileri gönderir. |

## <a name="hybrid-connections"></a>Karma Bağlantılar

Bir kurumsal ağ içinde ve genel olarak erişilebilen bir uç nokta yok uygulamalara olayları göndermek için Azure geçişi karma bağlantıları kullanın.

|Başlık  |Açıklama  |
|---------|---------|
| [Karma bağlantı olayları göndermek](custom-event-to-hybrid-connection.md) | Dinleyici uygulama tarafından işlenmesi için mevcut bir karma bağlantıyı için özel bir olay gönderir. |

## <a name="logic-apps"></a>Logic Apps

Logic Apps olaylara yanıt verme için iş süreçlerini otomatikleştirmek için kullanın.

|Başlık  |Açıklama  |
|---------|---------|
| [Azure Event Grid ve Logic Apps ile sanal makine değişikliklerini izleme](monitor-virtual-machine-changes-event-grid-logic-app.md) | Bir mantıksal uygulama, bir sanal makine yapılan değişiklikleri izler ve bu değişiklikleri hakkında e-posta gönderir. |
| [Logic Apps kullanarak Azure IOT Hub olayları hakkında e-posta bildirimleri gönder](publish-iot-hub-events-to-logic-apps.md) | Bir cihaz IOT hub'ınıza eklenen her zaman bir mantıksal uygulama bildirim e-posta gönderir. |
| [Azure olay kılavuz tümleştirme örnekler için Azure hizmet veri yolu](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Olay kılavuz, uygulama ve mantıksal uygulama çalışması için Service Bus konusundan iletileri gönderir. |

## <a name="queue-storage"></a>Kuyruk Depolama

Kuyruk depolama çekebilir gerek olaylarını almak için kullanın.

|Başlık  |Açıklama  |
|---------|---------|
| [Azure CLI ve olay kılavuz Azure kuyruk depolamaya için rota özel olaylar](custom-event-to-queue-storage.md) | Özel olaylar için bir kuyruk depolama göndermeyi açıklar. |

## <a name="webhooks"></a>WebHooks

Olaylara yanıt özelleştirilebilir uç noktaları için Web kancası kullanın.

|Başlık  |Açıklama  |
|---------|---------|
| [Bir HTTP uç noktası olayları alma](receive-events.md) | Bir olay abonelikten olaylarını almak ve almak ve olayları serisini kaldırmak için bir HTTP uç noktası doğrulamak açıklar. |

## <a name="next-steps"></a>Sonraki adımlar

* Olay kılavuz giriş için bkz: [hakkında olay kılavuz](overview.md).
* Hızlı bir şekilde olay Kılavuzu ile çalışmaya başlamak için bkz: [Azure olay kılavuz oluşturma ve rota özel olaylarla](custom-event-quickstart.md).
