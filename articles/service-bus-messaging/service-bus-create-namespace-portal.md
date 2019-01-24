---
title: Azure portalında Service Bus ad alanı oluşturma | Microsoft Docs
description: Azure portalını kullanarak bir Service Bus ad alanı oluşturun.
services: service-bus-messaging
documentationcenter: .net
author: axisc
manager: timlt
editor: spelluru
ms.assetid: fbb10e62-b133-4851-9d27-40bd844db3ba
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: c456fa392c3ba07ad26e300eff07a89fa849b354
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54856324"
---
# <a name="create-a-service-bus-namespace-using-the-azure-portal"></a>Azure portalı ile Service Bus ad alanı oluşturma

Ad alanı, tüm mesajlaşma bileşenlerini kapsayan bir kapsayıcıdır. Tek bir ad alanında birden fazla kuyruk ve konu bulunabilir ve ad alanları genellikle uygulama kapsayıcıları olarak görev yapar. Bir Service Bus ad alanı oluşturmak için iki yol vardır:

1. Azure portalı (bu makale)
2. [Resource Manager şablonları][create-namespace-using-arm]

## <a name="create-a-namespace-in-the-azure-portal"></a>Azure portalında bir ad alanı oluşturma

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Tebrikler! Bir Service Bus Mesajlaşması ad alanı oluşturdunuz.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşmasının daha gelişmiş özelliklerinden bazılarını gösteren Service Bus [GitHub örneklerine][github-samples] göz atın.

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure/azure-service-bus/tree/master/samples
