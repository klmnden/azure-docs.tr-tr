---
title: Kullanılabilirlik ve tutarlılık - Azure Event Hubs | Microsoft Docs
description: Maksimum sayıda kullanılabilirlik ve tutarlılık bölümler kullanarak Azure Event Hubs ile sağlamak nasıl.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: 8f3637a1-bbd7-481e-be49-b3adf9510ba1
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: e5cad797b633d43bcc9ead657a60fca8aa6679bb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60822390"
---
# <a name="availability-and-consistency-in-event-hubs"></a>Kullanılabilirlik ve tutarlılık olay hub'ları

## <a name="overview"></a>Genel Bakış
Azure Event Hubs kullanan bir [modeli bölümleme](event-hubs-features.md#partitions) kullanılabilirliği ve tek bir olay hub'ının içinden paralelleştirme iyileştirmek için. Örneğin, bir olay hub'ı dört bölüm vardır ve bu bölümler birini bir sunucudan diğerine bir yük dengeleme işlemi taşınır, hala gönderip üç diğer bölümlerden alabilirsiniz. Ayrıca, daha fazla bölümlemeye sahip olmak, toplam üretilen iş iyileştirme daha fazla eşzamanlı okuyucular, verileri işlemeyi sahip olmanızı sağlar. Bölümlendirme ve dağıtılmış bir sistemde sıralama etkilerini anlama, çözüm tasarımı, önemli bir yönüdür.

Sıralama ve kullanılabilirlik arasındaki dengeyi açıklamanıza yardımcı olmak için bkz: [CAP Teoremi](https://en.wikipedia.org/wiki/CAP_theorem)Brewer'ın Teoremi olarak da bilinen. Bu Teoremi, tutarlılık, kullanılabilirlik ve dayanıklılık bölüm arasında seçim açıklanır. Bu ağ ile bölümlenmiş sistemleri olduğunu her zaman kullanılabilirlik ile tutarlılık arasındaki denge belirtir.

Brewer'ın Teoremi tutarlılık ve kullanılabilirlik gibi tanımlar:
* Bölüm dayanıklılık: veriler işlenirken bir bölümünde hata olduğunda bile devam etme olanağı bir veri işleme sistemin.
* Kullanılabilirlik: başarısız olmayan düğüm makul bir süre (ile hata veya zaman aşımları) içinde makul bir yanıt döndürür.
* Tutarlılık: Okuma, belirli bir istemcinin en son yazma döndürmek için sağlanır.

## <a name="partition-tolerance"></a>Bölüm dayanıklılık
Olay hub'ları bölümlenmiş verileri bir model temelinde oluşturulmuştur. Kurulum sırasında olay hub'ında bölüm sayısı yapılandırabilirsiniz, ancak bu değer daha sonra değiştiremezsiniz. Bölümler Event Hubs ile kullanmayı olduğundan, kullanılabilirlik ve tutarlılık için uygulamanızın hakkında bir karar vermeniz gerekir.

## <a name="availability"></a>Kullanılabilirlik
Event Hubs ile çalışmaya başlama en basit yolu, varsayılan davranışını kullanmaktır. Yeni bir oluşturursanız **[EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient)** kullanın ve nesne **[Gönder](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.sendasync?view=azure-dotnet#Microsoft_Azure_EventHubs_EventHubClient_SendAsync_Microsoft_Azure_EventHubs_EventData_)** yöntemi, olaylarınızı otomatik olarak dağıtılan arasında Olay hub'ınızdaki bölümler. Bu davranış, büyük miktarda zaman verir.

En fazla çalışma zamanını iste için kullanım örnekleri, bu model tercih edilir.

## <a name="consistency"></a>Tutarlılık
Bazı senaryolarda, olayların sırası önemli olabilir. Örneğin, bir silme komutundan önce bir güncelleştirme komutunu işlemek için arka uç sisteminizi isteyebilirsiniz. Bu örnekte, bir olaya bölüm anahtarı ayarlayın, veya kullanan bir `PartitionSender` olayları yalnızca belirli bir bölüme göndermek nesne. Bunun yapılması, bu olayları bölümünden okunduğunda, bunlar sırayla okunduğu sağlar.

Bu yapılandırma ile göndermekte olduğunuz belirli bölüm kullanılamıyorsa bir hata yanıtı alırsınız aklınızda bulundurun. Tek bir bölüm için benzeşim yoksa bir karşılaştırma noktası olarak, Event Hubs hizmeti için sonraki kullanılabilir bölüm olay gönderir.

Sıralama, çalışma zamanı, aynı zamanda en üst düzeye çıkarırken emin olmak için olası bir çözüm için olayları, olay işleme uygulaması bir parçası olarak olacaktır. Bunu yapmanın en kolay yolu, bir özel sıra numarası özelliğiyle etkinliğiniz damga sağlamaktır. Aşağıdaki kodda bir örnek gösterilir:

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

Bu örnekte, olay hub'ınızdaki kullanılabilir bölümlerinden olay gönderir ve karşılık gelen bir sıra numarası uygulamanızdan ayarlar. Bu çözüm, işleme uygulamanız tarafından tutulması durumuna gerektirir, ancak, Gönderenler kullanılabilir olması büyük olasılıkla bir uç nokta sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs hizmetine genel bakış](event-hubs-what-is-event-hubs.md)
* [Olay Hub’ı oluşturma](event-hubs-create.md)
