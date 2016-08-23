<properties
   pageTitle="Azure portalını kullanarak Service Bus ad alanı oluşturma | Microsoft Azure"
   description="Service Bus kullanmaya başlamak için bir ad alanı gereklidir. Azure portalını kullanarak bir ad alanı oluşturma işlemi aşağıda gösterilmiştir."
   services="service-bus"
   documentationCenter=".net"
   authors="jtaubensee"
   manager="timlt"
   editor="sethmanheim"/>

<tags
   ms.service="service-bus"
   ms.devlang="tbd"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="dotnet"
   ms.workload="na"
   ms.date="06/07/2016"
   ms.author="jotaub@microsoft.com"/>

#Azure portalını kullanarak Service Bus ad alanı oluşturma
Ad alanı tüm mesajlaşma bileşenlerinize yönelik ortak bir kapsayıcıdır. Tek bir ad alanında birden fazla kuyruk ve konu bulunabilir ve ad alanları genellikle uygulama kapsayıcıları olarak görev yapar. Şu anda bir Service Bus ad alanı oluşturmak için 2 farklı yol vardır.

1.  Azure portalı (bu makale)

2.  [ARM Şablonları][create-namespace-using-arm]

##Azure portalında bir ad alanı oluşturma
[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Tebrikler! Bir Service Bus Ad Alanı oluşturdunuz.

##Sonraki adımlar

Azure Service Bus Mesajlaşmasının daha gelişmiş özelliklerinden bazılarını gösteren örnekleri içeren GitHub depomuza göz atın.
[https://github.com/Azure-Samples/azure-servicebus-messaging-samples][github-samples]

[create-namespace-using-arm]: ./service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples


<!--HONumber=Aug16_HO1-->


