---
title: "Azure Service Bus - Event Grid tümleştirmesine genel bakış | Microsoft Docs"
description: "Service Bus mesajlaşması ve Event Grid tümleştirmesinin açıklaması"
services: service-bus-messaging
documentationcenter: .net
author: ChristianWolf42
manager: timlt
editor: 
ms.assetid: f99766cb-8f4b-4baf-b061-4b1e2ae570e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 02/15/2018
ms.author: chwolf
ms.openlocfilehash: 17f6c107ea1adfd4463ff4cc205fe11d111acb84
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="azure-service-bus-to-azure-event-grid-integration-overview"></a>Azure Service Bus - Azure Event Grid tümleştirmesine Genel Bakış

Azure Service Bus, Azure Event Grid’e yeni bir tümleştirme başlatmıştır. Bu özellik, düşük ileti hacmine sahip Service Bus Kuyruklarının veya Aboneliklerinin her zaman iletiler için bir alıcı yoklamasının olmasına gerek kalmamasını sağlar. Service Bus artık bir alıcı mevcut olmadığında ve bir Kuyrukta veya Abonelikte iletiler olduğunda olayları Azure Event Grid’e gönderebilir. Service Bus ad alanlarınıza Azure Event Grid abonelikleri oluşturabilir ve bu olayları dinleyip bir alıcı başlatarak olaylara tepki verebilirsiniz. Bu özellik sayesinde Service Bus, reaktif programlama modellerinde kullanılabilir.

Özelliği etkinleştirmek için gerekenler:

* En az bir Service Bus Kuyruğu olan bir Azure Service Bus Premium ad alanı veya en az bir Aboneliği olan bir Service Bus Konusu.
* Azure Service Bus Ad Alanına katkıda bulunan erişimi.
* Ek olarak, Service Bus Ad Alanı için bir Azure Event Grid aboneliğiniz olması da gerekir. Bu abonelik, alınacak iletiler olduğuna dair Azure Event Grid’den bildirim alır. Normalde aboneler, daha sonra iletileri işleyecek bir Web Uygulaması ile iletişim kuran bir Web Kancası, Azure İşlevleri veya Mantıksal Uygulamalar olabilir. 

![19][]

### <a name="verify-that-you-have-contributor-access"></a>Katkıda bulunan erişimine sahip olduğunuzu doğrulama

Service Bus Ad Alanınıza gidin ve aşağıda gösterildiği gibi "Erişim denetimi (IAM)" seçeneğini belirleyin:

![1][]

### <a name="events-and-event-schemas"></a>Olaylar ve Olay Şemaları

Azure Service Bus şu anda iki senaryo için olaylar gönderir.

* [ActiveMessagesWithNoListenersAvailable](#active-messages-available-event)
* [DeadletterMessagesAvailable](#dead-lettered-messages-available-event)

Ayrıca standart Azure Event Grid Güvenliğini ve [kimlik doğrulaması mekanizmalarını](https://docs.microsoft.com/en-us/azure/event-grid/security-authentication) da kullanır.

Event Grid Olay Şemaları hakkında daha fazla bilgi edinmek için [bu](https://docs.microsoft.com/en-us/azure/event-grid/event-schema) bağlantıyı izleyin.

#### <a name="active-messages-available-event"></a>Etkin İletiler Kullanılabilir Olayı

Bir Kuyrukta veya Abonelikte etkin iletileriniz varsa ve bir alıcı dinleme gerçekleştirmiyorsa bu Olay oluşturulur.

Bu Olayın şeması:

```JSON
{
  "topic": "/subscriptions/<subscription id>/resourcegroups/DemoGroup/providers/Microsoft.ServiceBus/namespaces/<YOUR SERVICE BUS NAMESPACE WILL SHOW HERE>",
  "subject": "topics/<service bus topic>/subscriptions/<service bus subscription>",
  "eventType": "Microsoft.ServiceBus.ActiveMessagesAvailableWithNoListeners",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://YOUR-SERVICE-BUS-NAMESPACE-WILL-SHOW-HERE.servicebus.windows.net/TOPIC-NAME/subscriptions/SUBSCRIPTIONNAME/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}
```

#### <a name="dead-lettered-messages-available-event"></a>Geçerliliğini Yitirmiş İletiler Kullanılabilir Olayı

İletiler içeren ve etkin alıcılar içermeyen Geçerliliğini Yitirmiş Kuyruk başına en az bir olay alırsınız.

Bu Olayın şeması:

```JSON
[{
     "topic": "/subscriptions/<subscription id>/resourcegroups/DemoGroup/providers/Microsoft.ServiceBus/namespaces/<YOUR SERVICE BUS NAMESPACE WILL SHOW HERE>",
  "subject": "topics/<service bus topic>/subscriptions/<service bus subscription>",
  "eventType": "Microsoft.ServiceBus.DeadletterMessagesAvailableWithNoListener",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://YOUR-SERVICE-BUS-NAMESPACE-WILL-SHOW-HERE.servicebus.windows.net/TOPIC-NAME/subscriptions/SUBSCRIPTIONNAME/$deadletterqueue/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="how-often-and-how-many-events-are-emitted"></a>Ne sıklıkla ve kaç tane olay gönderilir?

Ad alanında birden fazla Kuyruk ve Konu / Abonelik varsa, Kuyruk başına en az bir ve Abonelik başına bir olay alırsınız. Service Bus varlığında bir ileti yoksa ve yeni bir ileti gelirse hemen veya Azure Service Bus etkin bir alıcı algılamadığı sürece her iki dakikada bir olaylar gönderilir. İleti taraması, olayları kesintiye uğratmaz.

Varsayılan olarak Azure Service Bus, ad alanındaki tüm varlıklar için olaylar gönderir. Yalnızca belirli varlıklar için olaylar almak istiyorsanız aşağıdaki filtreleme bölümüne bakın.

### <a name="filtering-limiting-from-where-you-get-events"></a>Olayları aldığınız yeri sınırlayarak filtreleme

Örneğin, yalnızca ad alanınızdaki bir Kuyruk veya bir Abonelik için olaylar almak istiyorsanız, Azure Event Grid tarafından sağlanan “ile başlar” veya “ile biter” filtrelerini kullanabilirsiniz. Bazı arabirimlerde filtrelere, “Ön” ve “Sonek” filtreleri adı verilir. Tümü için değil, birden fazla Kuyruk ve Abonelik için olaylar almak istiyorsanız, birden fazla farklı Azure Event Grid Aboneliği oluşturabilir ve her birine ilişkin bir filtre sağlayabilirsiniz.

## <a name="how-to-create-azure-event-grid-subscriptions-for-service-bus-namespaces"></a>Service Bus Ad Alanları için Azure Event Grid Abonelikleri oluşturma

Service Bus Ad Alanları için Event Grid Abonelikleri oluşturmanın üç farklı yolu vardır.

* [Azure portalı](#portal-instructions)
* [Azure CLI](#azure-cli-instructions)
* [PowerShell](#powershell-instructions)

## <a name="portal-instructions"></a>Portal yönergeleri

Yeni bir Azure Event Grid aboneliği oluşturmak için, Azure portalındaki ad alanınıza gidin ve Event Grid kamasını seçin. Aşağıdaki “+ Olay Aboneliği”ne tıklandığında, önceden birkaç Event Grid aboneliği olan bir ad alanı gösterilir.

![20][]

Aşağıdaki ekran görüntüsü, belirli bir filtreleme olmadan bir Azure İşlevi’ne veya Web Kancası’na abone olma işlemi örneğini gösterir:

![21][]

## <a name="azure-cli-instructions"></a>Azure CLI yönergeleri

İlk olarak en azından Azure CLI 2.0 sürümünün yüklenmiş olduğundan emin olun. Yükleyiciyi buradan indirebilirsiniz. Daha sonra “Windows + X” tuşlarına basın ve Yönetici izinleriyle yeni bir PowerShell konsolu açın. Alternatif olarak Azure portalındaki bir komut kabuğunu da kullanabilirsiniz.

Şu kodu yürütün:

```PowerShell
Az login

Aa account set -s “THE SUBSCRIPTION YOU WANT TO USE”

$namespaceid=(az resource show --namespace Microsoft.ServiceBus --resource-type namespaces --name “<yourNamespace>“--resource-group “<Your Resource Group Name>” --query id --output tsv)

az eventgrid event-subscription create --resource-id $namespaceid --name “<YOUR EVENT GRID SUBSCRIPTION NAME (CAN BE ANY NOT EXISTING)>” --endpoint “<your_function_url>” --subject-ends-with “<YOUR SERVICE BUS SUBSCRIPTION NAME>”
```

## <a name="powershell-instructions"></a>PowerShell yönergeleri

Azure PowerShell’in yüklenmiş olduğundan emin olun. Buradan bulabilirsiniz. Daha sonra “Windows + X” tuşlarına basın ve Yönetici izinleriyle yeni bir PowerShell konsolu açın. Alternatif olarak Azure portalındaki bir komut kabuğunu da kullanabilirsiniz.

```PowerShell
Login-AzureRmAccount

Select-AzureRmSubscription -SubscriptionName "<YOUR SUBSCRIPTION NAME>"

# This might be installed already
Install-Module AzureRM.ServiceBus

$NSID = (Get-AzureRmServiceBusNamespace -ResourceGroupName "<YOUR RESOURCE GROUP NAME>" -Na
mespaceName "<YOUR NAMESPACE NAME>").Id 

New-AzureRmEVentGridSubscription -EventSubscriptionName “<YOUR EVENT GRID SUBSCRIPTION NAME (CAN BE ANY NOT EXISTING)>” -ResourceId $NSID -Endpoint "<YOUR FUNCTION URL>” -SubjectEndsWith “<YOUR SERVICE BUS SUBSCRIPTION NAME>”
```

Buradan diğer kurulum seçeneklerini keşfedebilir veya [olayların akışa alınmasını test edebilirsiniz](#test-that-events-are-flowing).

## <a name="next-steps"></a>Sonraki adımlar

* Service Bus ve Event Grid [örnekleri](service-bus-to-event-grid-integration-example.md).
* [Azure Event Grid](https://docs.microsoft.com/en-us/azure/azure-functions/) hakkında daha fazla bilgi edinin.
* [Azure İşlevleri](https://docs.microsoft.com/en-us/azure/azure-functions/) hakkında daha fazla bilgi edinin.
* [Azure Logic Apps](https://docs.microsoft.com/en-us/azure/logic-apps/) hakkında daha fazla bilgi edinin.
* [Azure Service Bus](https://docs.microsoft.com/en-us/azure/azure-functions/) hakkında daha fazla bilgi edinin.

[1]: ./media/service-bus-to-event-grid-integration-concept/sbtoeventgrid1.png
[19]: ./media/service-bus-to-event-grid-integration-concept/sbtoeventgriddiagram.png
[8]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid8.png
[9]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid9.png
[20]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal.png
[21]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal2.png