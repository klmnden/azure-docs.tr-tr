---
title: Azure Event Hubs örnekleri | Microsoft Docs
description: Azure Event hubs'ı örnekleri
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/17/2018
ms.author: shvija
ms.openlocfilehash: fbde6e5a5ed053d6c151b3af25535c397a496ef4
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "40005343"
---
# <a name="event-hubs-samples"></a>Event Hubs örnekleri 
Event Hubs örnekleri bulabilirsiniz [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples). Bu örnekler, anahtar özelliklerini gösteren [Azure Event Hubs](/azure/event-hubs/). Bu makale, kategorilere ayırır ve her biri için bağlantılarla birlikte kullanılabilir örnekler açıklar.

## <a name="net-samples"></a>.NET örnekleri

| Örnek adı | Açıklama | 
| ----------- | ----------- | 
| [SampleSender](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) | Bu örnek, bir olay hub'ına olayları kümesini gönderen bir .NET Core konsol uygulaması yazma işlemi gösterilmektedir. |
| [SampleEHReceiver](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) | Bu örnek, bir etkinlik kümesi olay işleyicisi konağı kitaplığı kullanılarak bir olay hub'ından alan bir .NET Core konsol uygulaması yazma işlemi gösterilmektedir.  | 

## <a name="java-samples"></a>Java örnekleri

| Örnek adı | Açıklama | 
| ----------- | ----------- | 
| [SendBatch](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SendBatch)  | Bu örnek, olay hub'ına olayların toplu alma gösterilmektedir. | 
| [SimpleSend](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SimpleSend) | Bu örneği için olayları içe alma, olay hub'ına nasıl gösterir. |
| [AdvanceSendOptions](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/AdvancedSendOptions) | Bu örnek için olayları içe alma çeşitli seçenekleri Event Hubs ile gösterilmektedir. |
| [ReceiveByDateTime](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/ReceiveByDateTime) | Bu örnek, belirli bir tarih-saat uzaklığı kullanarak bir event hub bölümünden olayları alma gösterilmektedir. |
| [ReceiveUsingOffset](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/ReceiveUsingOffset) | Bu örnek, belirli veri uzaklığı kullanarak bir event hub bölümünden olayları alma gösterilmektedir. |  
| [ReceiveUsingSequenceNumber](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/ReceiveUsingSequenceNumber) | Bu örnek gösterir bir sıra numarası kullanarak bir olay hub'ı bölümleri nasıl alabilir. |   
| [EventProcessorSample](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/EventProcessorSample) |Bu örnek, aynı anda birden çok alıcı arasında otomatik bölüm seçimi ve yük devretme sağlayan olay işlemcisi konağı kullanarak event hub'ındaki olayları alma gösterilmektedir. | 
| [AutoScaleOnIngress](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Benchmarks/AutoScaleOnIngress) | Bu örnek nasıl bir olay hub'ı otomatik olarak yüksek yük ölçeğini artırabilir gösterilmektedir. Örnek yalnızca yapılandırılmış oranını olay hub'ı ölçeği artırma işleminin neden bir olay hub'ı aşan bir hızda olayları gönderir. | 
| [IngressBenchmark](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Benchmarks/IngressBenchmark) | Bu örnek, giriş oranı ölçüm sağlar. | 

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaleler de Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

- [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md)
- [Event Hubs özellikleri](event-hubs-features.md)
- [Event Hubs ile ilgili SSS](event-hubs-faq.md)