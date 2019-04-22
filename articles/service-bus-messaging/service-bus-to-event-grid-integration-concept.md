---
title: Azure Service Bus - Event Grid tümleştirmesine genel bakış | Microsoft Docs
description: Service Bus mesajlaşması ve Event Grid tümleştirmesinin açıklaması
services: service-bus-messaging
documentationcenter: .net
author: axisc
manager: timlt
editor: spelluru
ms.assetid: f99766cb-8f4b-4baf-b061-4b1e2ae570e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/15/2018
ms.author: aschhab
ms.openlocfilehash: 9df321980db3a2481f0d8cc007546822fea46f9e
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59049855"
---
# <a name="azure-service-bus-to-event-grid-integration-overview"></a>Azure Service Bus - Event Grid tümleştirmesine Genel Bakış

Azure Service Bus, Azure Event Grid’e yeni bir tümleştirme başlatmıştır. Bu özelliğin temel kullanım modelinde, düşük ileti hacmine sahip Service Bus kuyruklarının veya aboneliklerinin sürekli olarak iletiler için yoklama yapan bir alıcısının olması gerekmez. 

Service Bus artık bir alıcı mevcut olmadığında ve bir kuyrukta veya abonelikte iletiler olduğunda olayları Event Grid’e gönderebilir. Service Bus ad alanlarınıza Event Grid abonelikleri oluşturabilir, bu olayları dinleyebilir ve bir alıcı başlatarak olaylara tepki verebilirsiniz. Bu özellik sayesinde Service Bus’ı reaktif programlama modellerinde kullanabilirsiniz.

Özelliği etkinleştirmek için gereken öğeler:

* En az bir Service Bus kuyruğu olan bir Service Bus Premium ad alanı veya en az bir aboneliği olan bir Service Bus konusu.
* Service Bus ad alanına katkıda bulunan erişimi.
* Ek olarak, Service Bus ad alanı için bir Event Grid aboneliğiniz olması da gerekir. Bu abonelik, alınacak iletiler olduğuna dair Event Grid’den bildirim alır. Normalde aboneler, web uygulaması ile iletişim kuran bir web kancası, Azure İşlevleri veya Azure App Service’in Logic Apps özelliği olabilir. Daha sonra abone iletileri işler. 

![19][]


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="verify-that-you-have-contributor-access"></a>Katkıda bulunan erişimine sahip olduğunuzu doğrulama
Service Bus ad alanınıza gidin ve ardından **erişim denetimi (IAM)** seçip **rol atamaları** sekmesi. Ad alanına katkıda bulunan erişimine sahip olduğunuzu doğrulayın. 

### <a name="events-and-event-schemas"></a>Olaylar ve olay şemaları

Service Bus şu anda iki senaryo için olaylar gönderir:

* [ActiveMessagesWithNoListenersAvailable](#active-messages-available-event)
* DeadletterMessagesAvailable

Ayrıca Service Bus, standart Event Grid güvenliğini ve [kimlik doğrulaması mekanizmalarını](https://docs.microsoft.com/azure/event-grid/security-authentication) da kullanır.

Daha fazla bilgi için bkz. [Azure Event Grid olay şemaları](https://docs.microsoft.com/azure/event-grid/event-schema).

#### <a name="active-messages-available-event"></a>Etkin İletiler Kullanılabilir olayı

Bir kuyrukta veya abonelikte etkin iletileriniz varsa ve bir alıcı dinleme gerçekleştirmiyorsa bu olay oluşturulur.

Bu olayın şeması:

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

#### <a name="dead-letter-messages-available-event"></a>Geçerliliğini Yitirmiş İletiler Kullanılabilir olayı

İletiler içeren ve etkin alıcılar içermeyen Geçerliliğini Yitirmiş kuyruk başına en az bir olay alırsınız.

Bu olayın şeması:

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

### <a name="how-many-events-are-emitted-and-how-often"></a>Ne sıklıkla ve kaç tane olay gönderilir?

Ad alanında birden fazla kuyruk ve konu veya abonelik varsa, kuyruk başına en az bir ve abonelik başına bir olay alırsınız. Service Bus varlığında bir ileti yoksa ve yeni bir ileti gelirse olaylar hemen gönderilir. Alternatif olarak, Service Bus etkin bir alıcı algılamazsa iki dakikada bir olaylar gönderilir. İleti taraması, olayları kesintiye uğratmaz.

Varsayılan olarak Service Bus, ad alanındaki tüm varlıklar için olaylar gönderir. Yalnızca belirli varlıklar için olaylar almak istiyorsanız sonraki bölüme bakın.

### <a name="use-filters-to-limit-where-you-get-events-from"></a>Olayları aldığınız yeri sınırlamak için filtreleri kullanma

Örneğin, yalnızca ad alanınızdaki bir abonelik veya kuyruktan olaylar almak istiyorsanız, Event Grid tarafından sağlanan *ile başlar* veya *ile biter* filtrelerini kullanabilirsiniz. Bazı arabirimlerde filtrelere, *Ön* ve *Sonek* filtreleri adı verilir. Tümü için değil, birden fazla kuyruk ve abonelik için olaylar almak istiyorsanız, birden fazla Event Grid aboneliği oluşturabilir ve her birine ilişkin bir filtre sağlayabilirsiniz.

## <a name="create-event-grid-subscriptions-for-service-bus-namespaces"></a>Service Bus ad alanları için Event Grid abonelikleri oluşturma

Service Bus ad alanları için Event Grid abonelikleri oluşturmanın üç farklı yolu vardır:

* Azure portalında
* [Azure CLI](#azure-cli-instructions)’da
* [PowerShell](#powershell-instructions)’de

## <a name="azure-portal-instructions"></a>Azure portalı yönergeleri

Yeni Event Grid aboneliği oluşturmak için aşağıdakileri yapın:
1. Azure portalında ad alanınıza gidin.
2. Sol bölmede **Event Grid**’i seçin. 
3. **Olay Aboneliği**’ni seçin.  

   Aşağıdaki resimde, bir Event Grid aboneliği içeren bir ad alanı gösterilmektedir:

   ![Event Grid abonelikleri](./media/service-bus-to-event-grid-integration-concept/sbtoeventgridportal.png)

   Aşağıdaki resimde, belirli bir filtreleme olmadan bir işleve veya web kancasına nasıl abone olunacağı gösterilmektedir:

   ![21][]

## <a name="azure-cli-instructions"></a>Azure CLI yönergeleri

İlk olarak, Azure CLI sürüm 2.0 veya üzerinin yüklü olduğundan emin olun. [Yükleyiciyi indirin](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). **Windows + X** tuşlarına basın ve yönetici izinleriyle yeni bir PowerShell konsolu açın. Alternatif olarak Azure portalındaki bir komut kabuğunu kullanabilirsiniz.

Şu kodu yürütün:

 ```azurecli-interactive
az login

az account set -s “THE SUBSCRIPTION YOU WANT TO USE”

$namespaceid=(az resource show --namespace Microsoft.ServiceBus --resource-type namespaces --name “<yourNamespace>“--resource-group “<Your Resource Group Name>” --query id --output tsv)

az eventgrid event-subscription create --resource-id $namespaceid --name “<YOUR EVENT GRID SUBSCRIPTION NAME (CAN BE ANY NOT EXISTING)>” --endpoint “<your_function_url>” --subject-ends-with “<YOUR SERVICE BUS SUBSCRIPTION NAME>”
```

## <a name="powershell-instructions"></a>PowerShell yönergeleri

Azure PowerShell’in yüklenmiş olduğundan emin olun. [Yükleyiciyi indirin](https://docs.microsoft.com/powershell/azure/install-Az-ps). **Windows + X** tuşlarına basın ve Yönetici izinleriyle yeni bir PowerShell konsolu açın. Alternatif olarak Azure portalındaki bir komut kabuğunu kullanabilirsiniz.

```powershell-interactive
Connect-AzAccount

Select-AzSubscription -SubscriptionName "<YOUR SUBSCRIPTION NAME>"

# This might be installed already
Install-Module Az.ServiceBus

$NSID = (Get-AzServiceBusNamespace -ResourceGroupName "<YOUR RESOURCE GROUP NAME>" -Na
mespaceName "<YOUR NAMESPACE NAME>").Id

New-AzEVentGridSubscription -EventSubscriptionName “<YOUR EVENT GRID SUBSCRIPTION NAME (CAN BE ANY NOT EXISTING)>” -ResourceId $NSID -Endpoint "<YOUR FUNCTION URL>” -SubjectEndsWith “<YOUR SERVICE BUS SUBSCRIPTION NAME>”
```

Buradan diğer kurulum seçeneklerini keşfedin veya olayların akışa alındığını test edin.

## <a name="next-steps"></a>Sonraki adımlar

* Service Bus ve Event Grid [örnekleri](service-bus-to-event-grid-integration-example.md) alın.
* [Event Grid](https://docs.microsoft.com/azure/event-grid/) hakkında daha fazla bilgi edinin.
* [Azure İşlevleri](https://docs.microsoft.com/azure/azure-functions/) hakkında daha fazla bilgi edinin.
* [Logic Apps](https://docs.microsoft.com/azure/logic-apps/) hakkında daha fazla bilgi edinin.
* [Service Bus](https://docs.microsoft.com/azure/service-bus/) hakkında daha fazla bilgi edinin.

[1]: ./media/service-bus-to-event-grid-integration-concept/sbtoeventgrid1.png
[19]: ./media/service-bus-to-event-grid-integration-concept/sbtoeventgriddiagram.png
[8]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid8.png
[9]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid9.png
[20]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal.png
[21]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal2.png
