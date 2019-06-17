---
title: Azure Event Grid olay işleyicileri
description: Azure Event Grid için desteklenen olay işleyicileri açıklar
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/21/2019
ms.author: spelluru
ms.openlocfilehash: 6093e1017af2fb8c54eaf1c3192f937172567982
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67080550"
---
# <a name="event-handlers-in-azure-event-grid"></a>Azure Event Grid olay işleyicileri

Bir olay işleyicisi, olay burada gönderilir yerdir. Olayı işlemek için bazı başka bir eylem işleyici alır. Çeşitli Azure Hizmetleri, olayları işlemek için otomatik olarak yapılandırılır. Herhangi bir Web kancası olaylarını işlemek için de kullanabilirsiniz. Web kancası olaylarını işlemek için Azure'da barındırılması gerekmez. Event Grid, yalnızca HTTPS Web kancası uç noktalarını destekler.

Bu makale, her bir olay işleyicisi için içeriğe bağlantılar sağlar.

## <a name="azure-automation"></a>Azure Otomasyonu

Otomatik runbook'ları ile olayları işlemek üzere Azure Otomasyonu kullanın.

|Unvan  |Açıklama  |
|---------|---------|
|[Öğretici: Event Grid ve Microsoft Teams ile Azure Otomasyonu](ensure-tags-exists-on-new-virtual-machines.md) |Bir olay gönderen bir sanal makine, oluşturun. Sanal Makine etiketleri ve bir Microsoft Teams kanalına gönderilen bir iletinin tetikleyen bir Otomasyon runbook'unu olayı tetikler. |

## <a name="azure-functions"></a>Azure İşlevleri

Azure işlevleri, sunucusuz olaylara yanıt vermek için kullanın.

İşleyici olarak Azure İşlevleri’ni kullanırken, genel HTTP tetikleyicileri yerine Event Grid tetikleyicisini kullanın. Event Grid, Event Grid İşlevi tetikleyicilerini otomatik olarak doğrular. Genel HTTP tetikleyicileri ile [doğrulama yanıtını](security-authentication.md#webhook-event-delivery) uygulamanız gerekir.

|Unvan  |Açıklama  |
|---------|---------|
| [Azure işlevleri için olay Kılavuzu tetikleyicisi](../azure-functions/functions-bindings-event-grid.md) | Olay Kılavuzu tetikleyicisi işlevlerini kullanarak genel bakış. |
| [Öğretici: Event Grid kullanarak karşıya yüklenen görüntüleri yeniden boyutlandırmayı otomatikleştirin](resize-images-on-storage-blob-upload-event.md) | Kullanıcıların resimler web uygulaması arasında depolama hesabına yükleyin. Bir depolama blobu oluşturulduğunda, Event Grid, karşıya yüklenen görüntüyü yeniden boyutlandıran işlev uygulaması için bir olay gönderir. |
| [Öğretici: bir veri ambarına büyük veri akışı](event-grid-event-hubs-integration.md) | Event Grid, Event Hubs yakalama dosyası oluşturduğunda, bir işlev uygulaması için bir olay gönderir. Uygulama yakalama dosyasını alır ve bir veri ambarı'na veri geçirir. |
| [Öğretici: Azure Service Bus-Azure Event Grid tümleştirmesi örnekleri](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Event Grid, işlev uygulaması ve mantıksal uygulama için hizmet veri yolu konusu'ndan iletileri gönderir. |

## <a name="event-hubs"></a>Event Hubs

Çözümünüzü olayları olayları işleyebileceğinden daha hızlı ulaştırın, Event Hubs'ı kullanın. Uygulamanız kendi zamanlama sırasında bu olayları Event hubs'dan işler. Gelen olayları işlemek için işleme etkinliğiniz ölçeklendirebilirsiniz.

Event Hubs bir olay kaynağı veya olay işleyicisi olarak görev yapabilir. Aşağıdaki makalede bir işleyici Event hubs'ı kullanmayı gösterir.

|Unvan  |Açıklama  |
|---------|---------|
| [Hızlı Başlangıç: Azure Event hubs'a, Azure CLI ve Event Grid ile özel olaylar yol.](custom-event-to-eventhub.md) | Bir uygulama tarafından işlenmesi için olay hub'ına özel bir olay gönderir. |
| [Resource Manager şablonu: özel konu ve olay hub'ları uç noktası](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-event-hubs-handler)| Resource Manager şablonu kullanarak özel bir konu için bir abonelik oluşturur. Bu olayları Azure Event Hubs'a gönderir. |

Event Hubs kaynağı olarak bir örnekleri için bkz: [Event Hubs kaynağı](event-sources.md#event-hubs).

## <a name="hybrid-connections"></a>Karma Bağlantılar

Bir kurumsal ağ içinde ve genel olarak erişilebilen bir uç noktası olmayan uygulamalara olayları göndermek için Azure geçişi karma bağlantıları kullanın.

|Unvan  |Açıklama  |
|---------|---------|
| [Öğretici: karma bağlantı olayları gönderme](custom-event-to-hybrid-connection.md) | Dinleyici uygulama tarafından işlenmesi için mevcut bir karma bağlantı için özel bir olay gönderir. |

## <a name="logic-apps"></a>Logic Apps

Logic Apps olaylarına yanıt verme için iş süreçlerini otomatikleştirmek için kullanın.

|Unvan  |Açıklama  |
|---------|---------|
| [Öğretici: Azure Event Grid ve Logic Apps ile sanal makine değişikliklerini izleme](monitor-virtual-machine-changes-event-grid-logic-app.md) | Mantıksal uygulama, bir sanal makine yapılan değişiklikleri izler ve bu değişiklikler hakkında e-posta gönderir. |
| [Öğretici: mantıksal uygulamalar'ı kullanarak Azure IOT Hub olayları hakkında e-posta bildirimleri gönder](publish-iot-hub-events-to-logic-apps.md) | IOT hub'ınıza her bir cihaz eklendiğinde bir mantıksal uygulama bir bildirim e-posta gönderir. |
| [Öğretici: Azure Service Bus-Azure Event Grid tümleştirmesi örnekleri](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Event Grid, işlev uygulaması ve mantıksal uygulama için hizmet veri yolu konusu'ndan iletileri gönderir. |

## <a name="service-bus-queue-preview"></a>Service Bus kuyruğu (Önizleme)

Service Bus, kurumsal uygulamalar bölümündeki arabelleğe alma veya komut ve denetim senaryolarında kullanım için hizmet veri yolu Kuyrukları için doğrudan Event grid'de olaylarınızı yönlendirmek için bir olay işleyicisi kullanın. Önizleme, oturumları ve hizmet veri yolu konuları ile çalışmaz, ancak Service Bus kuyruklarına ilişkin tüm katmanlar ile çalışır.

Lütfen unutmayın, bir işleyici genel önizlemede olduğundan sırasında Service Bus, CLI veya PowerShell uzantısı bu olay aboneliği oluşturmak için kullanırken yüklemeniz gerekir.

### <a name="install-extension-for-azure-cli"></a>Uzantısı için Azure CLI'yı yükleme

Azure CLI için gereksinim duyduğunuz [Event Grid uzantısı](/cli/azure/azure-cli-extensions-list).

İçinde [CloudShell](/azure/cloud-shell/quickstart):

* Uzantıyı daha önce yüklediyseniz, güncelleştirebilirsiniz `az extension update -n eventgrid`.
* Uzantıyı daha önce yüklemediyseniz kullanarak yükleme `az extension add -n eventgrid`.

Yerel bir yüklemesi için:

1. [Azure CLI yükleme](/cli/azure/install-azure-cli). İle kontrol ederek son sürüme sahip olduğunuzdan emin olun `az --version`.
1. Uzantının önceki sürümleri kaldırmanız `az extension remove -n eventgrid`.
1. Yükleme `eventgrid` uzantısıyla `az extension add -n eventgrid`.

### <a name="install-module-for-powershell"></a>İçin PowerShell modülünü yükleme

PowerShell için gereksinim duyduğunuz [AzureRM.EventGrid Modülü](https://www.powershellgallery.com/packages/AzureRM.EventGrid/0.4.1-preview).

İçinde [CloudShell](/azure/cloud-shell/quickstart-powershell):

* İle modülünü yükleme `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`.

Yerel bir yüklemesi için:

1. PowerShell konsolunu yönetici olarak açın.
1. İle modülünü yükleme `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`.

Varsa `-AllowPrerelease` parametresi kullanılamaz, aşağıdaki adımları kullanın:

1. `Install-Module PowerShellGet -Force` öğesini çalıştırın.
1. `Update-Module PowerShellGet` öğesini çalıştırın.
1. PowerShell konsolunu kapatın.
1. PowerShell'i yönetici olarak yeniden başlatın.
1. Modül yükleme `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`.

### <a name="using-cli-to-add-a-service-bus-handler"></a>CLI kullanarak bir Service Bus işleyicisi ekleme

Aşağıdaki örnek, Azure CLI için abone olur ve bir Event Grid konusu, bir Service Bus kuyruğuna bağlanır:

```azurecli-interactive
# If you haven't already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid event-subscription create \
    --name <my-event-subscription> \
    --source-resource-id /subscriptions/{SubID}/resourceGroups/{RG}/providers/Microsoft.EventGrid/topics/topic1 \
    --endpoint-type servicebusqueue \
    --endpoint /subscriptions/{SubID}/resourceGroups/TestRG/providers/Microsoft.ServiceBus/namespaces/ns1/queues/queue1
```

## <a name="queue-storage"></a>Kuyruk Depolama

Kuyruk depolama çekilmesi gereken olaylarını almak için kullanın. Yanıt vermesi çok uzun sürer uzun süre çalışan bir işlem varsa, kuyruk depolama kullanabilir. Olayları kuyruk depolamaya göndererek uygulama çekme ve kendi zamanlamaya göre bir olay işleme.

|Unvan  |Açıklama  |
|---------|---------|
| [Hızlı Başlangıç: Azure kuyruk depolama ile Azure CLI ve Event Grid için özel olayları yönlendirmek](custom-event-to-queue-storage.md) | Bir kuyruk depolama için özel olayları göndermek nasıl açıklar. |

## <a name="webhooks"></a>WebHooks

Olaylara yanıt özelleştirilebilir uç noktaları için Web kancalarını kullanma.

|Unvan  |Açıklama  |
|---------|---------|
| Hızlı Başlangıç: oluşturma ve yönlendirme - ile özel olaylar [Azure CLI](custom-event-quickstart.md), [PowerShell](custom-event-quickstart-powershell.md), ve [portalı](custom-event-quickstart-portal.md). | Bir Web Kancasına özel olayları göndermek nasıl gösterir. |
| Hızlı Başlangıç: Bir özel web uç noktasına - ile Blob Depolama olaylarını yönlendirme [Azure CLI](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json), [PowerShell](../storage/blobs/storage-blob-event-quickstart-powershell.md?toc=%2fazure%2fevent-grid%2ftoc.json), ve [portalı](blob-event-quickstart-portal.md). | BLOB Depolama olayları için Web kancası gönderme işlemi gösterilmektedir. |
| [Hızlı Başlangıç: kapsayıcı kayıt defteri olayları gönderme](../container-registry/container-registry-event-grid-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Kapsayıcı kayıt defteri olayları göndermek için Azure CLI kullanma işlemi gösterilmektedir. |
| [Genel Bakış: bir HTTP uç noktasına olayları alma](receive-events.md) | Bir olay aboneliği olayları alma ve almak ve olayları seri durumdan çıkarmak için bir HTTP uç noktasını doğrulamak açıklar. |

## <a name="next-steps"></a>Sonraki adımlar

* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Event Grid ile hızla çalışmaya başlamak için bkz: [Azure Event Grid ile özel olaylar oluşturma ve yönlendirme](custom-event-quickstart.md).
