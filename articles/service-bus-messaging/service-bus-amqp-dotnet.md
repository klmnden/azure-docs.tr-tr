---
title: .NET ve AMQP 1.0 ile Azure Service Bus | Microsoft Docs
description: .NET Azure Service Bus ile AMQP kullanma
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: 332bcb13-e287-4715-99ee-3d7d97396487
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: 82301a17bb461b6d8733d5f046fe791ffbcf3ecb
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58885715"
---
# <a name="use-service-bus-from-net-with-amqp-10"></a>AMQP 1.0 ile Service Bus .NET kullanma

Service Bus paket sürüm 2.1 veya üzeri AMQP 1.0 desteği sunulmaktadır. Service Bus bitten indirerek en son sürüme sahip olmak [NuGet][NuGet].

## <a name="configure-net-applications-to-use-amqp-10"></a>AMQP 1.0 kullanmak için .NET uygulamaları yapılandır

Varsayılan olarak, hizmet veri yolu .NET istemci kitaplığı, adanmış bir SOAP tabanlı protokolü kullanarak Service Bus hizmeti ile iletişim kurar. AMQP 1.0 yerine varsayılan kullanmak için sonraki bölümde açıklandığı gibi protokol Service Bus bağlantı dizesi açık yapılandırma gerektirir. Bu değişikliğin dışında AMQP 1.0 kullanarak uygulama kodunu değişmeden kalır.

Geçerli sürümde AMQP kullanılırken desteklenmez birkaç API özellikleri vardır. Bu desteklenmeyen özellikler bölümünde listelenen [davranışsal farklılıklar](#behavioral-differences). Bazı gelişmiş yapılandırma ayarları da farklı bir anlama AMQP kullanırken sahiptir.

### <a name="configuration-using-appconfig"></a>App.config kullanarak yapılandırma

App.config yapılandırma dosyası ayarlarını depolamak için kullanılacak uygulamalar için iyi bir uygulamadır. Hizmet veri yolu uygulamaları için Service Bus bağlantı dizesi depolamak için App.config kullanabilirsiniz. Örnek App.config dosyası aşağıdaki gibidir:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
    </appSettings>
</configuration>
```

Değerini `Microsoft.ServiceBus.ConnectionString` Service Bus bağlantı dizesi, Service Bus bağlantısını yapılandırmak için kullanılan bir ayardır. Biçimi aşağıdaki gibidir:

`Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp`

Burada `namespace` ve `SAS key` öğesinden elde edilir [Azure portalında] [ Azure portal] oluşturduğunuzda bir Service Bus ad alanı. Daha fazla bilgi için [Azure portalını kullanarak Service Bus ad alanı oluşturma][Create a Service Bus namespace using the Azure portal].

Bağlantı dizesi ile AMQP kullanırken ekleme `;TransportType=Amqp`. Bu gösterim AMQP 1.0 kullanarak Service Bus için bağlantı kurmak için istemci kitaplığı bildirir.

## <a name="message-serialization"></a>İleti seri hale getirme

Varsayılan protokol kullanırken, .NET İstemci Kitaplığı'nın varsayılan serileştirme davranışını kullanmaktır [DataContractSerializer] [ DataContractSerializer] türü seri hale getirmek için bir [BrokeredMessage] [ BrokeredMessage] taşıma istemci kitaplığı ile Service Bus hizmeti arasındaki için örneği. AMQP aktarım modunu kullanırken, istemci kitaplığının AMQP tür sistemi serileştirilmesi için kullanır. [aracılı ileti] [ BrokeredMessage] AMQP iletisine. Bu seri hale getirme ileti alındı ve potansiyel olarak, örneğin, Service Bus erişmeye JMS API'sini kullanan bir Java uygulaması gibi farklı bir platform üzerinde çalıştığı alan bir uygulama tarafından yorumlanan sağlar.

Oluşturduğunuzda bir [BrokeredMessage] [ BrokeredMessage] örneği, ileti gövdesi görevini görecek oluşturucusuna bir parametre olarak bir .NET nesnesini sağlayabilirsiniz. AMQP ilkel türler için eşleşen nesneler için gövdenin AMQP veri türlerinde seri hale getirilir. Nesne doğrudan bir AMQP ilkel türe eşlenemez diğer bir deyişle, özel bir tür, uygulama tarafından tanımlanan, ardından kullanılarak serileştirilmiş nesne [DataContractSerializer][DataContractSerializer], ve seri hale getirilmiş bayt bir AMQP veri iletisi gönderilir.

.NET olmayan istemcileri ile birlikte çalışabilirlik kolaylaştırmak için ileti gövdesi için AMQP türleri doğrudan seri hale getirilebilir .NET türlerini kullanın. Aşağıdaki tabloda, bu türleri ve AMQP tür sistemine karşılık gelen eşleme ayrıntıları.

| .NET gövde nesne türü | Eşlenen AMQP türü | AMQP gövde bölümü türü |
| --- | --- | --- |
| bool |boole |AMQP değeri |
| bayt |ubyte |AMQP değeri |
| ushort |ushort |AMQP değeri |
| uint |uint |AMQP değeri |
| ulong |ulong |AMQP değeri |
| sbyte |bayt |AMQP değeri |
| kısa |kısa |AMQP değeri |
| int |int |AMQP değeri |
| uzun |uzun |AMQP değeri |
| float |float |AMQP değeri |
| double |double |AMQP değeri |
| decimal |decimal128 |AMQP değeri |
| Char |Char |AMQP değeri |
| DateTime |timestamp |AMQP değeri |
| Guid |uuid |AMQP değeri |
| bayt] |İkili |AMQP değeri |
| string |string |AMQP değeri |
| System.Collections.IList |list |AMQP değeri: koleksiyonda yer alan öğeleri yalnızca, bu tabloda tanımlanan olabilir. |
| System.Array |array |AMQP değeri: koleksiyonda yer alan öğeleri yalnızca, bu tabloda tanımlanan olabilir. |
| System.Collections.IDictionary |map |AMQP değeri: koleksiyonda yer alan öğeleri yalnızca, bu tabloda tanımlanan olabilir. Not: yalnızca dize anahtarları desteklenir. |
| Uri |Dize açıklanan (aşağıdaki tabloya bakın) |AMQP değeri |
| DateTimeOffset |Uzun açıklanan (aşağıdaki tabloya bakın) |AMQP değeri |
| TimeSpan |Uzun açıklanan (aşağıdakilere bakın) |AMQP değeri |
| Akış |İkili |AMQP verileri (birden fazla olabilir). Veri bölümler Stream nesnesinden okuma ham bayt içerir. |
| Diğer nesne |İkili |AMQP verileri (birden fazla olabilir). DataContractSerializer veya uygulama tarafından sağlanan bir seri hale getirici kullanan nesne seri hale getirilmiş ikili içerir. |

| .NET türü | Türü eşleşen AMQP açıklanan | Notlar |
| --- | --- | --- |
| Uri |`<type name=”uri” class=restricted source=”string”> <descriptor name=”com.microsoft:uri” /></type>` |Uri.AbsoluteUri |
| DateTimeOffset |`<type name=”datetime-offset” class=restricted source=”long”> <descriptor name=”com.microsoft:datetime-offset” /></type>` |DateTimeOffset.UtcTicks |
| TimeSpan |`<type name=”timespan” class=restricted source=”long”> <descriptor name=”com.microsoft:timespan” /></type>` |TimeSpan.Ticks |

## <a name="behavioral-differences"></a>Davranışsal farklılıklar

AMQP, varsayılan protokole göre kullanırken hizmet veri yolu .NET API davranışını bazı küçük farklılıklar vardır:

* [OperationTimeout] [ OperationTimeout] özelliği yok sayılır.
* `MessageReceiver.Receive(TimeSpan.Zero)` olarak uygulanan `MessageReceiver.Receive(TimeSpan.FromSeconds(10))`.
* İletileri kilit belirteçleri ile Tamamlanıyor yalnızca başlangıçta iletileri alınan ileti alıcı tarafından yapılabilir.

## <a name="control-amqp-protocol-settings"></a>Denetim AMQP protokol ayarları

[.NET API'lerini](/dotnet/api/) AMQP protokolünü davranışını denetlemek için bazı ayarları kullanıma sunar:

* **[MessageReceiver.PrefetchCount](/dotnet/api/microsoft.servicebus.messaging.messagereceiver.prefetchcount?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessageReceiver_PrefetchCount)**: Bağlantı uygulanan başlangıç kredisi denetler. Varsayılan değer 0'dır.
* **[MessagingFactorySettings.AmqpTransportSettings.MaxFrameSize](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.maxframesize?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_MaxFrameSize)**: AMQP çerçeve sınırını bağlantıda anlaşması sırasında sunulan denetimler, zaman açın. Varsayılan 65.536 bayt'tır.
* **[MessagingFactorySettings.AmqpTransportSettings.BatchFlushInterval](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.batchflushinterval?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_BatchFlushInterval)**: Aktarımları batchable ise bu değer değerlendirmeleri göndermek için en büyük gecikme belirler. Varsayılan olarak Gönderenler/alıcılar tarafından devralınır. Tek tek gönderenin alıcı 20 milisaniyedir varsayılan geçersiz kılabilirsiniz.
* **[MessagingFactorySettings.AmqpTransportSettings.UseSslStreamSecurity](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.usesslstreamsecurity?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_UseSslStreamSecurity)**: AMQP bağlantıları bir SSL bağlantısı üzerinden kurulan olup olmadığını denetler. Varsayılan değer **true**.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi hazır mısınız? Aşağıdaki bağlantıları ziyaret edin:

* [Hizmet veri yolu AMQP genel bakış]
* [AMQP 1.0 protokol kılavuzu]

[Create a Service Bus namespace using the Azure portal]: service-bus-create-namespace-portal.md
[DataContractSerializer]: https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer.aspx
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage?view=azureservicebus-4.0.0
[Microsoft.ServiceBus.Messaging.MessagingFactory.AcceptMessageSession]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory.acceptmessagesession?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessagingFactory_AcceptMessageSession
[OperationTimeout]: /dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout
[NuGet]: https://nuget.org/packages/WindowsAzure.ServiceBus/
[Azure portal]: https://portal.azure.com
[Hizmet veri yolu AMQP genel bakış]: service-bus-amqp-overview.md
[AMQP 1.0 protokol kılavuzu]: service-bus-amqp-protocol-guide.md

