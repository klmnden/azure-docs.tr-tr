---
title: "Azure portalını kullanarak Service Bus ad alanı oluşturma | Microsoft Docs"
description: "Azure portalını kullanarak Service Bus ad alanı oluşturma."
services: service-bus-messaging
documentationcenter: .net
author: jtaubensee
manager: timlt
editor: 
ms.assetid: fbb10e62-b133-4851-9d27-40bd844db3ba
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 11/30/2016
ms.author: jotaub
translationtype: Human Translation
ms.sourcegitcommit: 4a972b9b8b52a90f27afda98d8bdc661016d1fe1
ms.openlocfilehash: ba7093f8a2c06ab4cecf11207174a4871435ae64
ms.lasthandoff: 02/28/2017


---
# <a name="create-a-service-bus-namespace-using-the-azure-portal"></a>Azure portalı ile Service Bus ad alanı oluşturma
Ad alanı, tüm mesajlaşma bileşenleri için ortak bir kapsayıcıdır. Tek bir ad alanında birden fazla kuyruk ve konu bulunabilir ve ad alanları genellikle uygulama kapsayıcıları olarak görev yapar. Şu anda bir Service Bus ad alanı oluşturmak için iki farklı yol vardır.

1. Azure portalı (bu makale)
2. [Resource Manager şablonları][create-namespace-using-arm]

## <a name="create-a-namespace-in-the-azure-portal"></a>Azure portalında bir ad alanı oluşturma
[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Tebrikler! Bir Service Bus Mesajlaşması ad alanı oluşturdunuz.

## <a name="next-steps"></a>Sonraki adımlar
Azure Service Bus Mesajlaşması'nın daha gelişmiş özelliklerinden bazılarını gösteren [GitHub örneklerine][github-samples] göz atın.

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples

