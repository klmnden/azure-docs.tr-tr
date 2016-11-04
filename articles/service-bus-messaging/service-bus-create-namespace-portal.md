---
title: Azure portalını kullanarak Service Bus ad alanı oluşturma | Microsoft Docs
description: Service Bus kullanmaya başlamak için bir ad alanı gereklidir. Azure portalını kullanarak bir ad alanı oluşturma işlemi aşağıda gösterilmiştir.
services: service-bus
documentationcenter: .net
author: jtaubensee
manager: timlt
editor: ''

ms.service: service-bus
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/22/2016
ms.author: jotaub

---
# <a name="create-a-service-bus-namespace-using-the-azure-portal"></a>Azure portalı ile Service Bus ad alanı oluşturma
Ad alanı, tüm mesajlaşma bileşenleriniz için ortak bir kapsayıcıdır. Tek bir ad alanında birden fazla kuyruk ve konu bulunabilir ve ad alanları genellikle uygulama kapsayıcıları olarak görev yapar. Şu anda bir Service Bus ad alanı oluşturmak için iki farklı yol vardır.

1. Azure portalı (bu makale)
2. [Resource Manager şablonları][create-namespace-using-arm]

## <a name="create-a-namespace-in-the-azure-portal"></a>Azure portalında bir ad alanı oluşturma
[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Tebrikler! Bir Service Bus Mesajlaşması ad alanı oluşturdunuz.

## <a name="next-steps"></a>Sonraki adımlar
Azure Service Bus Mesajlaşması’nın daha gelişmiş özelliklerinden bazılarını gösteren [GitHub örneklerine][github-samples] göz atın.

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples


<!--HONumber=Oct16_HO3-->


