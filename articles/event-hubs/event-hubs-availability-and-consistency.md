---
title: Kullanılabilirlik ve Azure Event Hubs tutarlılık | Microsoft Docs
description: Bölümler kullanılarak Azure Event Hubs ile en fazla miktarda kullanılabilirlik ve tutarlılık sağlamak nasıl.
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: 8f3637a1-bbd7-481e-be49-b3adf9510ba1
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/28/2017
ms.author: sethm
ms.openlocfilehash: e119406292ca1d805f831bc65e3ae6e583147c6d
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34700698"
---
# <a name="availability-and-consistency-in-event-hubs"></a>Kullanılabilirlik ve Event Hubs tutarlılığı

## <a name="overview"></a>Genel Bakış
Azure Event Hubs kullanan bir [modeli bölümleme](event-hubs-features.md#partitions) kullanılabilirliğini ve tek olay hub'ı içinde paralelleştirme artırmak için. Örneğin, dört bölüm bir event hub varsa ve bu bölümler birini bir sunucudan başka bir yük dengeleme işlemi taşınır, hala gönderebilir ve diğer üç bölümlerden alırsınız. Ayrıca, daha fazla bölümlemeye sahip olmak, toplam verimliliği artırma, veri işleme daha fazla eşzamanlı okuyucunun bağlanmasına olanak sağlar. Bölümlendirme ve dağıtılmış bir sistemde sıralama etkilerini anlama, çözüm tasarımı önemli bir yönüdür.

Sıralama ve kullanılabilirlik arasındaki dengelemeyi açıklamasına yardımcı olmak için bkz: [CAP Teoremi](https://en.wikipedia.org/wiki/CAP_theorem), Brewer'ın Teoremi olarak da bilinir. Bu Teoremi tutarlılık, kullanılabilirlik ve bölüm dayanıklılık arasında seçim açıklanır. Bu ağ tarafından bölümlenmiş sistemler için olduğundan her zaman tutarlılık ve kullanılabilirlik karşılaştırmasını belirtir.

Brewer'ın Teoremi tutarlılık ve kullanılabilirlik gibi tanımlar:
* Bölüm dayanıklılık: veriler işlenirken bir bölümünde hata olduğunda bile devam etme olanağı bir veri işleme sistemi.
* Kullanılabilirlik: başarısız olmayan düğüm makul bir süre (ile bir hata veya zaman aşımı) içinde makul bir yanıt döndürür.
* Tutarlılık: Okuma, belirtilen istemci için en son yazma döndürmek için sağlanır.

## <a name="partition-tolerance"></a>Bölüm dayanıklılık
Olay hub'ları bölümlenmiş veri modeli üzerine inşa edilmiştir. Kurulum sırasında olay hub'ınıza bölüm sayısı yapılandırabilirsiniz, ancak bu değer daha sonra değiştiremezsiniz. Bölümler Event Hubs ile kullanmalısınız olduğundan, kullanılabilirlik ve uygulamanız için tutarlılık hakkındaki kararınızı yapmanız gerekir.

## <a name="availability"></a>Kullanılabilirlik
Event Hubs ile çalışmaya başlamak için en basit yolu, varsayılan davranışı kullanmaktır. Yeni bir oluşturursanız **[EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient)** nesne ve kullanmak **[Gönder](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.sendasync?view=azure-dotnet#Microsoft_Azure_EventHubs_EventHubClient_SendAsync_Microsoft_Azure_EventHubs_EventData_)** yöntemi, olayları otomatik olarak dağıtılan arasında Olay hub'ınızı bölümler. Bu davranış, büyük miktarda zaman verir.

En fazla çalışma zamanını gerektirir kullanım örnekleri için bu model tercih edilir.

## <a name="consistency"></a>Tutarlılık
Bazı senaryolarda, olayları sıralama önemli olabilir. Örneğin, bir güncelleştirme komutu delete komutu önce işlemek için arka uç sisteminizi isteyebilirsiniz. Bu örnekte, bir olaya bölüm anahtarı ayarlayın veya kullanın bir `PartitionSender` yalnızca olayları belirli bir bölüme göndermek için nesne. Bu olaylar bölümünden okurken bunlar sırayla okunduğu sağlar.

Bu yapılandırma ile göndermekte olduğunuz belirli bölüm kullanılamıyorsa, bir hata yanıtı alırsınız göz önünde bulundurun. Tek bir bölüm için benzeşim yoksa bir karşılaştırma noktası olarak sonraki kullanılabilir bölüm, olay Olay hub'ları hizmeti gönderir.

Sıralama, ayrıca çalışma zamanı, en üst düzeye çıkarma sırasında emin olmak için olası bir çözüm için olayları, olay işleme uygulama bir parçası olarak olacaktır. Bunu yapmanın en kolay yolu olan bir özel sıra numarası özelliği, olay damga olmaktır. Aşağıdaki kodda bir örnek gösterilir:

```csharp
// Get the latest sequence number from your application
var sequenceNumber = GetNextSequenceNumber();
// Create a new EventData object by encoding a string as a byte array
var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));
// Set a custom sequence number property
data.Properties.Add("SequenceNumber", sequenceNumber);
// Send single message async
await eventHubClient.SendAsync(data);
```

Bu örnek olay hub'ınızı kullanılabilir bölümlerde biri, olay gönderir ve karşılık gelen sıra numarası uygulamanızdan ayarlar. Bu çözüm, işleme uygulamanız tarafından saklanacak durumu gerektiriyor, ancak kullanılabilir olması büyük olasılıkla bir uç nokta, Gönderenler sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Olay hub hizmetine genel bakış](event-hubs-what-is-event-hubs.md)
* [Olay Hub’ı oluşturma](event-hubs-create.md)
