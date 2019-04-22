---
title: Örnekleri - Azure Event Hubs'a | Microsoft Docs
description: Bu makalede github üzerindeki Azure Event Hubs için örnekleri listesini sağlar.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.custom: seodec18
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 1c1198733fb56303d328ee97152442d25dbe945a
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59682406"
---
# <a name="git-repositories-with-samples-for-azure-event-hubs"></a>Git depoları ile Azure Event Hubs için örnekleri 
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

## <a name="python-samples"></a>Python örnekleri
Azure olay hub'ları için Python örnekleri bulabilirsiniz [azure-event-hubs-python](https://github.com/Azure/azure-event-hubs-python/tree/master/examples) GitHub deposu.

## <a name="nodejs-samples"></a>Node.js örnekleri
Azure olay hub'ları için Node.js Örnekleri bulabilirsiniz [js için azure SDK'sı](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs/samples) GitHub deposu.

## <a name="go-samples"></a>Go örnekleri
Azure olay hub'ları için Go örnekleri bulabilirsiniz [azure-event-hubs-Git](https://github.com/Azure/azure-event-hubs-go/tree/master/_examples) GitHub deposu.

## <a name="azure-cli-samples"></a>Azure CLI örnekleri
Azure Event hubs Azure CLI örnekleri bulabilirsiniz [azure-event-hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples/Management/CLI) GitHub deposu.

## <a name="azure-powershell-samples"></a>Azure PowerShell örnekleri
Azure Event hubs Azure PowerShell örnekleri bulabilirsiniz [azure-event-hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples/Management/PowerShell) GitHub deposu.
 
## <a name="apache-kafka-samples"></a>Apache Kafka örnekleri
Apache Kafka özelliği için olay hub'ları için örnekler bulabilirsiniz [azure-event-hubs-için-kafka](https://github.com/Azure/azure-event-hubs-for-kafka) GitHub deposu.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaleler de Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

- [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md)
- [Event Hubs özellikleri](event-hubs-features.md)
- [Event Hubs ile ilgili SSS](event-hubs-faq.md)