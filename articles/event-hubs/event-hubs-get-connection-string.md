---
title: Azure Event Hubs bağlantı dizesi - alın | Microsoft Docs
description: Bu makalede, istemcilerin Azure Event Hubs'a bağlanmak için kullanabileceği bir bağlantı dizesi alma yönergeleri sağlanır.
services: event-hubs
documentationcenter: na
author: spelluru
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.custom: seodec18
ms.date: 02/19/2019
ms.author: spelluru
ms.openlocfilehash: edd197fb6d578df064c67a422767e3e70a0c8142
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66158889"
---
# <a name="get-an-event-hubs-connection-string"></a>Bir Event hubs'ı bağlantı dizesini alma

Event Hubs'ı kullanmak için bir Event Hubs ad alanı oluşturmanız gerekir. Bir ad alanı birden çok olay hub'ları veya Kafka konularını kapsayan bir kapsayıcıdır. Bu ad benzersiz bir veren [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name). Bir ad alanı oluşturduktan sonra Event Hubs ile iletişim kurmak için gerekli bağlantı dizesini edinebilirsiniz.

Azure Event Hubs için bağlantı dizesi içinde gömülü aşağıdaki bileşenlere sahiptir,

* FQDN oluşturduğunuz EventHubs ad alanı, FQDN = (servicebus.windows.net tarafından izlenen EventHubs ad alanı adı içerir)
* SharedAccessKeyName, uygulamanızın SAS anahtarları için seçtiğiniz adı =
* SharedAccessKey oluşturulan anahtar değerini =.

Bağlantı dizesi şablonu şu şekilde görünür
```
Endpoint=sb://<FQDN>/;SharedAccessKeyName=<KeyName>;SharedAccessKey=<KeyValue>
```

Örnek bir bağlantı dizesi aşağıdaki gibi görünmelidir `Endpoint=sb://dummynamespace.servicebus.windows.net/;SharedAccessKeyName=DummyAccessKeyName;SharedAccessKey=5dOntTRytoC24opYThisAsit3is2B+OGY1US/fuL3ly=`

Bu makalede, bağlantı dizesini edinme çeşitli şekillerde sizi yönlendirir.

## <a name="get-connection-string-from-the-portal"></a>Portaldan bağlantı dizesini alma
1. [Azure portalda](https://portal.azure.com) oturum açın. 
2. Seçin **tüm hizmetleri** sol gezinti menüsünde. 
3. Seçin **Event Hubs** içinde **Analytics** bölümü. 
4. Event hubs listesinde, olay hub'ınızı seçin.
6. Üzerinde **olay hub'ları Namespace** sayfasında **paylaşılan erişim ilkeleri** sol menüsünde.

    ![Paylaşılan erişim ilkeleri menü öğesi](./media/event-hubs-get-connection-string/event-hubs-get-connection-string1.png)
7. Seçin bir **paylaşılan erişim ilkesi** ilkeleri listesinde. Varsayılan bir adı verilir: **RootManageSharedAccessPolicy**. Uygun izinleri (okuma, yazma) sahip bir ilke ekleyin ve bu ilkeyi kullanın. 

    ![Event hubs'ı paylaşılan erişim ilkeleri](./media/event-hubs-get-connection-string/event-hubs-get-connection-string2.png)
8. Seçin **kopyalama** düğmesinin yanındaki **bağlantı dizesi-birincil anahtar** alan. 

    ![Event Hubs - bağlantı dizesi alma](./media/event-hubs-get-connection-string/event-hubs-get-connection-string3.png)

## <a name="getting-the-connection-string-with-azure-powershell"></a>Azure PowerShell ile bağlantı dizesi alınıyor

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Kullanabileceğiniz [Get-AzEventHubNamespaceKey](/powershell/module/az.eventhub/get-azeventhubkey) aşağıda gösterildiği gibi belirli ilke/kural adı için bağlantı dizesini almak için:

```azurepowershell-interactive
Get-AzEventHubKey -ResourceGroupName dummyresourcegroup -NamespaceName dummynamespace -AuthorizationRuleName RootManageSharedAccessKey
```

## <a name="getting-the-connection-string-with-azure-cli"></a>Azure CLI ile bağlantı dizesi alınıyor
Ad alanı için bağlantı dizesini almak için aşağıdakileri kullanabilirsiniz:

```azurecli-interactive
az eventhubs namespace authorization-rule keys list --resource-group dummyresourcegroup --namespace-name dummynamespace --name RootManageSharedAccessKey
```

Event Hubs, Azure CLI komutları hakkında daha fazla bilgi için bkz. [Event Hubs için Azure CLI](/cli/azure/eventhubs).

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md)
* [Olay Hub’ı oluşturma](event-hubs-create.md)
