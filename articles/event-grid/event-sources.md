---
title: Azure olay kılavuz olay kaynakları
description: İçin Azure olay kılavuz desteklenen olay kaynakları açıklar
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/25/2018
ms.author: tomfitz
ms.openlocfilehash: f9c3bcb6b92b43fe5b5bad72c99e6ce199c17448
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="event-sources-in-azure-event-grid"></a>Azure olay kılavuzunda olay kaynakları

Burada olay gerçekleştiğinde bir olay kaynağıdır. Çeşitli Azure hizmetlerine olayları göndermek için otomatik olarak yapılandırılır. Olayları göndermek özel uygulamalar da oluşturabilirsiniz. Özel uygulamalar olay kılavuz olay dağıtım için kullanılacak Azure içinde barındırılan gerekmez.

Bu makale, her bir olay kaynağı için içeriğe bağlantılar sağlar.

## <a name="azure-subscriptions"></a>Azure abonelikleri

Kaynakları değişiklikleri bir Azure aboneliği arasında yanıtlamak için Azure abonelikleri olaylara abone olma.

|Unvan |Açıklama  |
|---------|---------|
| [Azure Otomasyonu olay kılavuz ve Microsoft ekipleri ile tümleştirme](ensure-tags-exists-on-new-virtual-machines.md) |Bir olay gönderen bir sanal makine, oluşturun. Sanal Makine etiketleri ve bir Microsoft Teams kanalına gönderilen bir ileti tetikleyen bir Otomasyon runbook'u olay tetiklenir. |
| [Olay şeması](event-schema-subscriptions.md) | Alanları Azure aboneliği olayları gösterir. |

## <a name="custom-topics"></a>Özel konular

Uygulama olaylarına yanıt vermek için özel konular için abone olun.

|Unvan  |Açıklama  |
|---------|---------|
| [Oluşturma ve Azure CLI ile özel olaylar rota](custom-event-quickstart.md) | Azure CLI özel olayları göndermek için nasıl kullanılacağını gösterir. |
| [Oluşturma ve Azure PowerShell ile özel olaylar rota](custom-event-quickstart-powershell.md) | Özel olaylar göndermek için Azure PowerShell kullanmayı gösterir. |
| [Oluşturma ve Azure portal ile özel olaylar rota](custom-event-quickstart-portal.md) | Portal özel olayları göndermek için nasıl kullanılacağını gösterir. |
| [Özel konu başlığına sonrası](post-to-custom-topic.md) | Özel bir konu için bir olay sonrası gösterilmektedir. |
| [Azure kuyruk depolama için rota özel olaylar](custom-event-to-queue-storage.md) | Özel olaylar için bir kuyruk depolama göndermeyi açıklar. |
| [Olay şeması](event-schema.md) | Alanları özel olayları gösterir. |

## <a name="event-hubs"></a>Event Hubs

Dosya olaylarını yakalamak için yanıt olay hub'ları olaylarına abone olma.

|Unvan  |Açıklama  |
|---------|---------|
| [Veri ambarına büyük veri akışı yapma](event-grid-event-hubs-integration.md) | Olay hub'ları yakalama dosyası oluşturduğunda, olay kılavuz işlev uygulaması için bir olay gönderir. Uygulama yakalama dosyasını alır ve bir veri ambarına veri taşır. |
| [Olay şeması](event-schema-event-hubs.md) | Alanları olay hub'ları olayları gösterir. |

## <a name="iot-hub"></a>IoT Hub

Abone IOT Hub'ına olayları cihaza yanıt oluşturulur ve olaylar silinmiş.

|Unvan  |Açıklama  |
|---------|---------|
| [Logic Apps kullanarak Azure IOT Hub olayları hakkında e-posta bildirimleri gönder](publish-iot-hub-events-to-logic-apps.md) | Bir cihaz IOT hub'ınıza eklenen her zaman bir mantıksal uygulama bildirim e-posta gönderir. |
| [Tetikleyici eylemleri olay kılavuz kullanarak IOT hub'ı olaylarına tepki](../iot-hub/iot-hub-event-grid.md) | IOT hub'ları olay kılavuz ile tümleştirme genel bakış. |
| [Olay şeması](event-schema-iot-hub.md) | Alanları IOT hub'da olayları gösterir. |

## <a name="media-services"></a>Media Services

İş durumu olaylara yanıt vermek Media Services olaylara abone olma.

|Unvan  |Açıklama  |
|---------|---------|
| [Media Services olaylarına tepki](../media-services/latest/reacting-to-media-services-events.md) | Media Services olay kılavuz ile tümleştirme genel bakış. |
| [CLI kullanarak bir özel web uç noktası için rota Azure Media Services olayları](../media-services/latest/job-state-events-cli-how-to.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Media Services olayları göndermek nasıl gösterir. |
| [Olay şeması](../media-services/latest/media-services-event-schemas.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Alanları Media Services olayları gösterir. |

## <a name="resource-groups"></a>Kaynak grupları

Bir kaynak grubu kaynakları değişiklikleri yanıtlaması için kaynak grubu olaylara abone olun.

|Unvan  |Açıklama  |
|---------|---------|
| [Azure Event Grid ve Logic Apps ile sanal makine değişikliklerini izleme](monitor-virtual-machine-changes-event-grid-logic-app.md) | Bir mantıksal uygulama, bir sanal makine yapılan değişiklikleri izler ve bu değişiklikleri hakkında e-posta gönderir. |
| [Olay Şeması](event-schema-resource-groups.md) | Alanları kaynak grubu olayları gösterir. |

## <a name="service-bus"></a>Service Bus

Etkin olan bir dinleyici gerek kalmadan, iletilere yanıt verecek şekilde Service Bus olaylara abone olma.

|Unvan  |Açıklama  |
|---------|---------|
| [Azure olay kılavuz tümleştirme örnekler için Azure hizmet veri yolu](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Olay kılavuz, uygulama ve mantıksal uygulama çalışması için Service Bus konusundan iletileri gönderir. |
| [Azure hizmet veri yoluna olay kılavuz tümleştirmesine genel bakış](../service-bus-messaging/service-bus-to-event-grid-integration-concept.md) | Hizmet veri yolu olay kılavuz ile tümleştirme genel bakış. |
| [Olay şeması](event-schema-service-bus.md) | Alanları Service Bus olayları gösterir. |

## <a name="storage"></a>Depolama

Oluşturulan ve Silinen olayları blob yanıtlamak için Blob Depolama olaylarına abone olma.

|Unvan  |Açıklama  |
|---------|---------|
| [Azure CLI ile bir özel web uç noktası için rota Blob Depolama olayları](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Azure CLI blob depolama olayları göndermek için nasıl kullanılacağını gösterir. |
| [PowerShell ile bir özel web uç noktası için rota Blob Depolama olayları](../storage/blobs/storage-blob-event-quickstart-powershell.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Blob depolama olayları göndermek için Azure PowerShell kullanmayı gösterir. |
| [Blob depolama olaylarına yanıt verme](../storage/blobs/storage-blob-event-overview.md) | Blob Depolama olay kılavuz ile tümleştirme genel bakış. |
| [Olay şeması](event-schema-blob-storage.md) | Alanları Blob Storage olayları gösterir. |

## <a name="next-steps"></a>Sonraki adımlar

* Olay kılavuz giriş için bkz: [hakkında olay kılavuz](overview.md).
* Hızlı bir şekilde olay Kılavuzu ile çalışmaya başlamak için bkz: [Azure olay kılavuz oluşturma ve rota özel olaylarla](custom-event-quickstart.md).
