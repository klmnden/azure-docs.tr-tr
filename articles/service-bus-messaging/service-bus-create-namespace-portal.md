---
title: "Azure portalında Service Bus ad alanı oluşturma | Microsoft Docs"
description: "Azure portalını kullanarak bir Service Bus ad alanı oluşturun."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: fbb10e62-b133-4851-9d27-40bd844db3ba
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 02/22/2018
ms.author: sethm
ms.openlocfilehash: a24fa21848005d9768d26ae865680a4851e1dd81
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="create-a-service-bus-namespace-using-the-azure-portal"></a>Azure portalı ile Service Bus ad alanı oluşturma

Ad alanı, tüm mesajlaşma bileşenlerini kapsayan bir kapsayıcıdır. Tek bir ad alanında birden fazla kuyruk ve konu bulunabilir ve ad alanları genellikle uygulama kapsayıcıları olarak görev yapar. Bir Service Bus ad alanı oluşturmak için iki yol vardır:

1. Azure portalı (bu makale)
2. [Resource Manager şablonları][create-namespace-using-arm]

## <a name="create-a-namespace-in-the-azure-portal"></a>Azure portalında bir ad alanı oluşturma

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Tebrikler! Bir Service Bus Mesajlaşması ad alanı oluşturdunuz.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşmasının daha gelişmiş özelliklerinden bazılarını gösteren [GitHub örneklerimize][github-samples] göz atın.

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure/azure-service-bus/tree/master/samples
