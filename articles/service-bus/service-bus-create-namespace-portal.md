<properties
    pageTitle="Azure portalını kullanarak Service Bus ad alanı oluşturma | Microsoft Azure"
    description="Service Bus kullanmaya başlamak için bir ad alanı gereklidir. Azure portalını kullanarak bir ad alanı oluşturma işlemi aşağıda gösterilmiştir."
    services="service-bus"
    documentationCenter=".net"
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="jotaub"/>


# Azure portalını kullanarak Service Bus ad alanı oluşturma

Ad alanı, tüm mesajlaşma bileşenleriniz için ortak bir kapsayıcıdır. Tek bir ad alanında birden fazla kuyruk ve konu bulunabilir ve ad alanları genellikle uygulama kapsayıcıları olarak görev yapar. Şu anda bir Service Bus ad alanı oluşturmak için iki farklı yol vardır.

1.  Azure portalı (bu makale)

2.  [Resource Manager şablonları][create-namespace-using-arm]

## Azure portalında bir ad alanı oluşturma

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Tebrikler! Bir Service Bus Mesajlaşması ad alanı oluşturdunuz.

## Sonraki adımlar

Azure Service Bus Mesajlaşması’nın daha gelişmiş özelliklerinden bazılarını gösteren örnekleri içeren [GitHub depomuza](https://github.com/Azure-Samples/azure-servicebus-messaging-samples][github-samples) göz atın.

[create-namespace-using-arm]: ../service-bus-messaging/service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples


<!--HONumber=Sep16_HO4-->


