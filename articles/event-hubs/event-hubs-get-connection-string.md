---
title: Azure Event hubs'ı bağlantı dizesi alma | Microsoft Docs
description: Bir Azure Event hubs'ı bağlantı dizesini alma
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.date: 10/15/2018
ms.author: shvija
ms.openlocfilehash: bfc82f2dc280c3528f38c9cb466473a76328e552
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285016"
---
# <a name="get-an-event-hubs-connection-string"></a>Bir Event hubs'ı bağlantı dizesini alma

Event Hubs'ı kullanmak için bir Event Hubs ad alanı oluşturmanız gerekir. Bir ad alanı birden çok olay hub'ları barındırmak kapsayan bir kapsayıcıdır / Kafka konularını. Bu ad benzersiz bir veren [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name). Bir ad alanı oluşturduktan sonra Event Hubs ile iletişim kurmak için gerekli bağlantı dizesini edinebilirsiniz.

Azure Event Hubs için bağlantı dizesi içinde gömülü aşağıdaki bileşenlere sahiptir,

* FQDN oluşturduğunuz EventHubs ad alanı, FQDN = (buna servicebus.windows.net tarafından izlenen EventHubs ad alanı adı dahildir)
* SharedAccessKeyName, uygulamanızın SAS anahtarları için seçtiğiniz adı =
* SharedAccessKey oluşturulan anahtar değerini =.

Bağlantı dizesi şablonu şu şekilde görünür
```
Endpoint=sb://<FQDN>/;SharedAccessKeyName=<KeyName>;SharedAccessKey=<KeyValue>
```

Örnek bir bağlantı dizesi aşağıdaki gibi görünmelidir `Endpoint=sb://dummynamespace.servicebus.windows.net/;SharedAccessKeyName=DummyAccessKeyName;SharedAccessKey=5dOntTRytoC24opYThisAsit3is2B+OGY1US/fuL3ly=`

Bu makalede, bağlantı dizesini edinme çeşitli şekillerde sizi yönlendirir.

## <a name="get-connection-string-from-the-portal"></a>Portaldan bağlantı dizesini alma

Event Hubs ad alanı oluşturduktan sonra portalın genel bakış bölümüne, bağlantı dizesi aşağıda gösterildiği gibi verebilirsiniz:

![Event Hubs bağlantı dizesi](./media/event-hubs-get-connection-string/event-hubs-get-connection-string1.png)

Genel Bakış bölümünde bağlantı dizesi tıkladığınızda, aşağıdaki çizimde gösterildiği gibi SAS ilkeleri sekmesini açar:

![Event hubs'ı SAS ilkeleri](./media/event-hubs-get-connection-string/event-hubs-get-connection-string2.png)

Yeni bir SAS ilkesi eklemek ve bağlantı dizesini alın veya sizin için önceden oluşturulmuş varsayılan ilkesini kullanın. İlke açıldığında, bağlantı dizesini gösterildiği alınır Aşağıda şekil:

![Event hubs'ı bağlantı dizesini alma](./media/event-hubs-get-connection-string/event-hubs-get-connection-string3.png)

## <a name="getting-the-connection-string-with-azure-powershell"></a>Azure PowerShell ile bağlantı dizesi alınıyor
Aşağıda gösterildiği gibi belirtin ilke/kural adı için bağlantı dizesini almak için Get-AzureRmEventHubNamespaceKey'nı kullanabilirsiniz:

```azurepowershell-interactive
Get-AzureRmEventHubKey -ResourceGroupName dummyresourcegroup -NamespaceName dummynamespace -AuthorizationRuleName RootManageSharedAccessKey
```

Başvurmak [Azure Event Hubs PowerShell Modülü](https://docs.microsoft.com/powershell/module/azurerm.eventhub/get-azurermeventhubkey) daha fazla ayrıntı için.

## <a name="getting-the-connection-string-with-azure-cli"></a>Azure CLI ile bağlantı dizesi alınıyor
Ad alanı için bağlantı dizesini almak için aşağıdakileri kullanabilirsiniz:

```azurecli-interactive
az eventhubs namespace authorization-rule keys list --resource-group dummyresourcegroup --namespace-name dummynamespace --name RootManageSharedAccessKey
```

Başvurmak [Event Hubs için Azure CLI](https://docs.microsoft.com/cli/azure/eventhubs) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md)
* [Olay Hub’ı oluşturma](event-hubs-create.md)
