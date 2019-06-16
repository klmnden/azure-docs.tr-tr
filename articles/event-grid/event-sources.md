---
title: Azure Event Grid olay kaynakları
description: Azure Event Grid için desteklenen olay kaynakları açıklar
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 02/12/2019
ms.author: spelluru
ms.openlocfilehash: 3611072759c62f42294730405f1dc402c496acce
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66735015"
---
# <a name="event-sources-in-azure-event-grid"></a>Azure Event Grid olay kaynakları

Burada olay gerçekleştiğinde bir olay kaynağıdır. Çeşitli Azure Hizmetleri, olayları göndermek için otomatik olarak yapılandırılır. Olayları gönderme özel uygulamalar oluşturabilirsiniz. Özel uygulamaları, Azure Event Grid olay dağıtım için kullanılacak barındırılması gerekmez.

Bu makale, her bir olay kaynağı için içeriğe bağlantılar sağlar.

## <a name="azure-subscriptions"></a>Azure abonelikleri

İçin bir Azure aboneliği kaynakları değişikliklere yanıt Azure abonelikleri olaylara abone olma.

|Unvan |Açıklama  |
|---------|---------|
| [Öğretici: Event Grid ve Microsoft Teams ile Azure Otomasyonu](ensure-tags-exists-on-new-virtual-machines.md) |Bir olay gönderen bir sanal makine, oluşturun. Sanal Makine etiketleri ve bir Microsoft Teams kanalına gönderilen bir iletinin tetikleyen bir Otomasyon runbook'unu olayı tetikler. |
| [Nasıl yapılır: için portal aracılığıyla olaylara abone olma](subscribe-through-portal.md) | Portalda bir Azure aboneliği için olaylara abone olmak için kullanın. |
| [Azure CLI: bir Azure aboneliği için olaylara abone olma](./scripts/event-grid-cli-azure-subscription.md) |Azure aboneliğinin bir Event Grid aboneliği oluşturur ve bir Web kancası'na olay gönderen örnek betiği. |
| [PowerShell: Azure aboneliği için olaylara abone olma](./scripts/event-grid-powershell-azure-subscription.md)| Azure aboneliğinin bir Event Grid aboneliği oluşturur ve bir Web kancası'na olay gönderen örnek betiği. |
| [Olay şeması](event-schema-subscriptions.md) | Alanları, Azure aboneliği olayları gösterir. |

## <a name="container-registry"></a>Container Kayıt Defteri

Görüntüleri değişikliklere yanıt vermek için kapsayıcı kayıt defteri olaylara abone olun.

|Unvan |Açıklama  |
|---------|---------|
| [Hızlı Başlangıç: kapsayıcı kayıt defteri olayları gönderme](../container-registry/container-registry-event-grid-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Kapsayıcı kayıt defteri olayları göndermek için Azure CLI kullanma işlemi gösterilmektedir. |
| [Olay şeması](event-schema-container-registry.md) | Alanlar, kapsayıcı kayıt defteri olaylarını gösterir. |

## <a name="custom-topics"></a>Özel konular

Uygulama olaylarına yanıt vermek için özel konular için abone olun.

|Unvan  |Açıklama  |
|---------|---------|
| [Hızlı Başlangıç: oluşturma ve Azure CLI ile özel olaylarını yönlendirme](custom-event-quickstart.md) | Özel olayları göndermek için Azure CLI kullanma işlemi gösterilmektedir. |
| [Hızlı Başlangıç: oluşturma ve Azure PowerShell ile özel olaylarını yönlendirme](custom-event-quickstart-powershell.md) | Özel olayları göndermek için Azure PowerShell kullanmayı gösterir. |
| [Hızlı Başlangıç: oluşturma ve Azure portalı ile özel olaylarını yönlendirme](custom-event-quickstart-portal.md) | Özel olayları göndermek için portalı kullanmayı gösterir. |
| [Hızlı Başlangıç: Azure kuyruk depolama için özel olayları yönlendirmek](custom-event-to-queue-storage.md) | Bir kuyruk depolama için özel olayları göndermek nasıl açıklar. |
| [Nasıl yapılır: özel konuya abone gönderin](post-to-custom-topic.md) | Özel konuya bir olay göndermek nasıl gösterir. |
| [Azure CLI: Event Grid özel konusu oluşturma](./scripts/event-grid-cli-create-custom-topic.md)|Örnek betik özel bir konu oluşturur. Betik, uç nokta ve anahtarı alır.|
| [Azure CLI: özel bir konu için olaylara abone olma](./scripts/event-grid-cli-subscribe-custom-topic.md)|Örnek betik bir özel konu için bir abonelik oluşturur. Bu, bir Web kancası için olaylar gönderir.|
| [PowerShell: Event Grid özel konusu oluşturma](./scripts/event-grid-powershell-create-custom-topic.md)|Örnek betik özel bir konu oluşturur. Betik, uç nokta ve anahtarı alır.|
| [PowerShell: özel bir konu için olaylara abone olma](./scripts/event-grid-powershell-subscribe-custom-topic.md)|Örnek betik bir özel konu için bir abonelik oluşturur. Bu, bir Web kancası için olaylar gönderir.|
| [Resource Manager şablonu: özel konu ve Web kancası uç noktası](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid) | Resource Manager şablonu bir özel konu ve abonelik, özel bir konu oluşturur. Bu, bir Web kancası için olaylar gönderir. |
|
| [Resource Manager şablonu: özel konu ve olay hub'ları uç noktası](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-event-hubs-handler)| Resource Manager şablonu kullanarak özel bir konu için bir abonelik oluşturur. Bu olayları Azure Event Hubs'a gönderir. |
| [Olay şeması](event-schema.md) | Alanları, özel olayları gösterir. |

## <a name="event-hubs"></a>Event Hubs

Yanıt dosyası olaylarını yakalamak için Event Hubs olaylarına abone olun. Event Hubs bir olay kaynağı veya olay işleyicisi olarak görev yapabilir. Aşağıdaki makaleleri Event Hubs kaynağı olarak kullanma işlemini gösterir.

|Unvan  |Açıklama  |
|---------|---------|
| [Öğretici: bir veri ambarına büyük veri akışı](event-grid-event-hubs-integration.md) | Event Grid, Event Hubs yakalama dosyası oluşturduğunda, bir işlev uygulaması için bir olay gönderir. Uygulama yakalama dosyasını alır ve bir veri ambarı'na veri geçirir. |
| [Olay şeması](event-schema-event-hubs.md) | Event Hubs olay alanlarını gösterir. |

İşleyici olarak Event Hubs örnekleri için bkz: [Event Hubs işleyici](event-handlers.md#event-hubs).

## <a name="iot-hub"></a>IoT Hub

Oluşturulan, silinen, bağlı, bağlantısı kesilmiş cihaza yanıt vermek için IOT Hub olaylarını ve telemetri olayları için abone olun.

|Unvan  |Açıklama  |
|---------|---------|
| [Logic Apps kullanarak Azure IOT Hub olayları hakkında e-posta bildirimleri gönder](publish-iot-hub-events-to-logic-apps.md) | IOT Hub'ınıza her bir cihaz eklendiğinde bir mantıksal uygulama bir bildirim e-posta gönderir. |
| [Tetikleyici eylemlere Event Grid kullanarak IOT Hub olaylarına tepki verme](../iot-hub/iot-hub-event-grid.md) | IOT Hub Event Grid ile tümleştirme genel bakış. |
| [Olay şeması](event-schema-iot-hub.md) | Alanlar, IOT hub'ına olayları gösterir. |
| [Sipariş bağlı cihaz ve cihaz olayları bağlantısı kesildi](../iot-hub/iot-hub-how-to-order-connection-state-events.md) | Cihaz bağlantı durumu olaylarını sipariş işlemi gösterilmektedir. |

## <a name="media-services"></a>Media Services

Media Services olaylarını, iş durumu olaylara yanıt vermek için abone olun.

|Unvan  |Açıklama  |
|---------|---------|
| [Genel Bakış: Media Services olaylara tepki verme](../media-services/latest/reacting-to-media-services-events.md) | Media Services Event Grid ile tümleştirme genel bakış. |
| [Öğretici: CLI kullanarak bir özel web uç noktası için Azure Media Services olaylarını yönlendirme](../media-services/latest/job-state-events-cli-how-to.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Media Services'dan olayları göndermek nasıl gösterir. |
| [Olay şeması](../media-services/latest/media-services-event-schemas.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Alanlar, Media Services olaylarını gösterir. |

## <a name="resource-groups"></a>Kaynak grupları

Kaynak grubu olayları değişiklikleri kaynaklar bir kaynak grubu üzerinde yanıt vermek için abone olun.

|Unvan  |Açıklama  |
|---------|---------|
| [Öğretici: Azure Event Grid ve Logic Apps ile sanal makine değişikliklerini izleme](monitor-virtual-machine-changes-event-grid-logic-app.md) | Mantıksal uygulama, bir sanal makine yapılan değişiklikleri izler ve bu değişiklikler hakkında e-posta gönderir. |
| [Azure CLI: bir kaynak grubu için olaylara abone olma](./scripts/event-grid-cli-resource-group.md)| Örnek betik bir kaynak grubu için olaylara abone olur. Bu, bir Web kancası için olaylar gönderir. |
| [Azure CLI: için bir kaynak grubu ve filtre bir kaynak için olaylara abone olma](./scripts/event-grid-cli-resource-group-filter.md) | Bir kaynak grubu için olaylara abone olur ve bir kaynak için olayları filtreler örnek betiği. |
| [PowerShell: bir kaynak grubu için olaylara abone olma](./scripts/event-grid-powershell-resource-group.md) | Örnek betik bir kaynak grubu için olaylara abone olur. Bu, bir Web kancası için olaylar gönderir. |
| [PowerShell: için bir kaynak grubu ve filtre bir kaynak için olaylara abone olma](./scripts/event-grid-powershell-resource-group-filter.md) | Bir kaynak grubu için olaylara abone olur ve bir kaynak için olayları filtreler örnek betiği. |
| [Resource Manager şablonu: kaynak aboneliği](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-resource-events-to-webhook) | Azure abonelik veya kaynak grubu için olaylara abone olur. Bu, bir Web kancası için olaylar gönderir. |
| [Olay Şeması](event-schema-resource-groups.md) | Alanlar, kaynak grubu olayları gösterir. |

## <a name="service-bus"></a>Service Bus

Etkin dinleyici olmadan iletilerini yanıtlamak için Service Bus olayları abone olmak.

|Unvan  |Açıklama  |
|---------|---------|
| [Öğretici: Azure Service Bus-Azure Event Grid tümleştirmesi örnekleri](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Event Grid, işlev uygulaması ve mantıksal uygulama için hizmet veri yolu konusu'ndan iletileri gönderir. |
| [Genel Bakış: Azure Service Bus-Event Grid tümleştirmesi](../service-bus-messaging/service-bus-to-event-grid-integration-concept.md) | Service Bus Event Grid ile tümleştirme genel bakış. |
| [Olay şeması](event-schema-service-bus.md) | Alanlar, Service Bus olayları gösterir. |

## <a name="storage"></a>Depolama

Blob oluşturulur ve Silinen olayları yanıtlamak için Blob Depolama olaylarına abone olun.

|Unvan  |Açıklama  |
|---------|---------|
| [Hızlı Başlangıç: Azure CLI ile bir özel web uç noktası için Blob Depolama olaylarını yönlendirme](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Blob Depolama olaylarını bir Web kancası'na göndermek için Azure CLI kullanma işlemi gösterilmektedir. |
| [Hızlı Başlangıç: PowerShell ile bir özel web uç noktası için Blob Depolama olaylarını yönlendirme](../storage/blobs/storage-blob-event-quickstart-powershell.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Blob Depolama olaylarını bir Web kancası'na göndermek için Azure PowerShell kullanmayı gösterir. |
| [Hızlı Başlangıç: oluşturma ve Azure portalı ile Blob Depolama olaylarını yönlendirme](blob-event-quickstart-portal.md) | Blob Depolama olaylarını bir Web kancası'na göndermek için portalı kullanmayı gösterir. |
| [Azure CLI: Blob Depolama hesabı için olaylara abone olma](./scripts/event-grid-cli-blob.md) | Örnek betik bir Blob Depolama hesabı için bir olaya abone olur. Bu olay için bir Web kancası gönderir. |
| [PowerShell: Blob Depolama hesabı için olaylara abone olma](./scripts/event-grid-powershell-blob.md) | Örnek betik bir Blob Depolama hesabı için bir olaya abone olur. Bu olay için bir Web kancası gönderir. |
| [Resource Manager şablonu: BLOB Depolama ve abonelik oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-subscription-and-storage) | Bir Azure Blob depolama hesabı dağıtır ve o depolama hesabı için olaylara abone olur. Bu, bir Web kancası için olaylar gönderir. |
| [Genel Bakış: Blob Depolama olaylarına tepki verme](../storage/blobs/storage-blob-event-overview.md) | Blob Depolama, Event Grid ile tümleştirme genel bakış. |
| [Olay şeması](event-schema-blob-storage.md) | Blob Depolama olaylarına alanları gösterir. |

## <a name="maps"></a>Haritalar
Azure haritalar olayları döndürürüz olaylara yanıt vermek için abone olun. Örneğin, bir cihaz girer ya da bir bölge sınırının çıkar her zaman bir uygulama bir e-posta bildirimi teslim edebilen.

|Unvan  |Açıklama  |
|---------|---------|
| [Event Grid kullanarak Azure haritalar olaylarına tepki verme](../azure-maps/azure-maps-event-grid-integration.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Azure haritalar'ı Event Grid ile tümleştirme genel bakış. |
| [Öğretici: Bir bölge sınırının ayarlayın](../azure-maps/tutorial-geofence.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Bu öğreticide Azure haritalar'ı kullanarak döndürürüz ayarlamak için temel adımlarında size kılavuzluk eder. Azure Event Grid döndürürüz sonuçları akış ve bölge sınırının sonuçlarına dayalı bir bildirim ayarlamak için kullanın. |
| [Olay şeması](event-schema-azure-maps.md) | Azure haritalar olayları alanları gösterir. |

## <a name="app-configuration"></a>Uygulama Yapılandırması
Azure uygulama yapılandırma olayları, anahtar-değer değişikliği olaylara yanıt vermek için abone olun.

|Unvan | Açıklama |
|---------|---------|
| [Event Grid kullanarak Azure uygulama yapılandırma olaylarına tepki verme](../azure-app-configuration/concept-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Uygulama yapılandırması Azure Event Grid ile tümleştirme genel bakış. |
| [Hızlı Başlangıç: Azure CLI ile bir özel web uç noktası için Azure uygulama yapılandırma olaylarını yönlendirme](../azure-app-configuration/howto-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Bir Web kancası için Azure uygulama yapılandırması olayları göndermek için Azure CLI kullanma işlemi gösterilmektedir. |
| [Olay şeması](event-schema-app-configuration.md) | Alanları, Azure uygulama yapılandırması olayları gösterir. |


## <a name="next-steps"></a>Sonraki adımlar

* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Event Grid ile hızla çalışmaya başlamak için bkz: [Azure Event Grid ile özel olaylar oluşturma ve yönlendirme](custom-event-quickstart.md).
