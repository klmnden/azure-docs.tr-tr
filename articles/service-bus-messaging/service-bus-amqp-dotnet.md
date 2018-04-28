---
title: Azure Service Bus .NET ve AMQP 1.0 ile | Microsoft Docs
description: .NET gelen Azure hizmet veri yolu AMQP ile kullanma
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: 332bcb13-e287-4715-99ee-3d7d97396487
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/21/2017
ms.author: sethm
ms.openlocfilehash: 28b8d7a71f01d8633d020b99fbe6bc5c16f272b4
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="use-service-bus-from-net-with-amqp-10"></a>Kullanım hizmeti .NET AMQP 1.0 ile yolundan

Hizmet veri yolu paket sürümünü 2.1 veya üzeri AMQP 1.0 destek mevcuttur. Hizmet veri yolu bitten yükleyerek en son sürümüne sahip olmak [NuGet][NuGet].

## <a name="configure-net-applications-to-use-amqp-10"></a>AMQP 1.0 kullanmak için .NET uygulamaları yapılandır

Varsayılan olarak, hizmet veri yolu .NET istemci kitaplığını adanmış bir SOAP tabanlı protokolü kullanarak Service Bus hizmeti ile iletişim kurar. AMQP 1.0 yerine varsayılan kullanmak için sonraki bölümde açıklandığı gibi hizmet veri yolu bağlantı dizesi açık yapılandırmasına protokolü gerektirir. Bu değişiklik dışında AMQP 1.0 kullanırken uygulama kodu değişmeden kalır.

Geçerli sürümde AMQP kullanırken desteklenmez birkaç API özellikler vardır. Desteklenmeyen bu özellikleri daha sonra bölümünde listelenen [desteklenmeyen özellikler, sınırlamalar ve davranış farklılıkları](#unsupported-features-restrictions-and-behavioral-differences). Bazı gelişmiş yapılandırma ayarları da farklı bir anlam AMQP kullanırken sahiptir.

### <a name="configuration-using-appconfig"></a>App.config kullanarak yapılandırma

App.config yapılandırma dosyası ayarlarını depolamak için kullanmak üzere uygulamalar için iyi bir uygulamadır. Hizmet veri yolu uygulamaları için hizmet veri yolu bağlantı dizesi depolamak için App.config kullanabilirsiniz. Örnek bir App.config dosyası aşağıdaki gibidir:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
    </appSettings>
</configuration>
```

Değeri `Microsoft.ServiceBus.ConnectionString` Service Bus bağlantısını yapılandırmak için kullanılan hizmet veri yolu bağlantı dizesi bir ayardır. Biçimi aşağıdaki gibidir:

`Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp`

Burada `namespace` ve `SAS key` alanından elde edilen [Azure portal] [ Azure portal] bir hizmet veri yolu ad alanı oluşturduğunuzda. Daha fazla bilgi için bkz: [Azure portalını kullanarak bir hizmet veri yolu ad alanı oluşturma][Create a Service Bus namespace using the Azure portal].

AMQP kullanırken, bağlantı dizesi ile sona `;TransportType=Amqp`. Bu gösterim hizmet veri yolu AMQP 1.0 kullanarak için kendi bağlantı kurmayı istemci kitaplığı bildirir.

## <a name="message-serialization"></a>İleti seri hale getirme

Varsayılan protokol kullanırken, .NET istemci kitaplığını varsayılan seri hale getirme davranışını kullanmaktır [DataContractSerializer] [ DataContractSerializer] türü seri hale getirmek için bir [BrokeredMessage] [ BrokeredMessage] istemci kitaplığı ile Service Bus hizmeti arasında aktarım için örneği. AMQP aktarım modu kullanırken, istemci kitaplığının AMQP tür sistemi serileştirmek için kullanır. [aracılı ileti] [ BrokeredMessage] bir AMQP iletisine. Bu seri hale getirme ileti aldı ve potansiyel olarak, örneğin, Service Bus erişmek için JMS API'sini kullanan bir Java uygulaması gibi farklı bir platform üzerinde çalışan bir alıcı uygulama tarafından yorumlanan sağlar.

Oluşturduğunuzda bir [BrokeredMessage] [ BrokeredMessage] örneği, ileti gövdesi görevini görecek Oluşturucu için bir parametre olarak bir .NET nesnesini sağlayabilir. AMQP ilkel türler için eşlenebilir nesneler için gövde AMQP veri türleriyle serileştirilir. Nesne doğrudan bir AMQP ilkel türe eşlenemez diğer bir deyişle, bir özel tür uygulama tarafından tanımlanan, ardından nesnesi kullanılarak serileştirilmiş [DataContractSerializer][DataContractSerializer], ve seri hale getirilmiş bayt bir AMQP veri iletisi gönderilir.

.NET olmayan istemcileri ile birlikte çalışabilirlik kolaylaştırmak için ileti gövdesi için AMQP türleri doğrudan sıralandığı .NET türlerini kullanın. Aşağıdaki tabloda bu türleri ve AMQP türü sistemine karşılık gelen eşleme ayrıntılarını verir.

| .NET gövdesi nesne türü | Eşlenen AMQP türü | AMQP gövde bölümü türü |
| --- | --- | --- |
| bool |boole |AMQP değeri |
| bayt |ubyte |AMQP değeri |
| ushort |ushort |AMQP değeri |
| uint |uint |AMQP değeri |
| ulong |ulong |AMQP değeri |
| sbyte |bayt |AMQP değeri |
| kısa |kısa |AMQP değeri |
| Int |Int |AMQP değeri |
| uzun |uzun |AMQP değeri |
| Kayan nokta |Kayan nokta |AMQP değeri |
| double |double |AMQP değeri |
| Ondalık |decimal128 |AMQP değeri |
| char |char |AMQP değeri |
| DateTime |timestamp |AMQP değeri |
| Guid |UUID |AMQP değeri |
| Byte] |İkili |AMQP değeri |
| string |string |AMQP değeri |
| System.Collections.IList |liste |AMQP değer: koleksiyonda bulunan öğeleri yalnızca, bu tabloda tanımlı olabilir. |
| System.Array |array |AMQP değer: koleksiyonda bulunan öğeleri yalnızca, bu tabloda tanımlı olabilir. |
| System.Collections.IDictionary |eşleme |AMQP değer: koleksiyonda bulunan öğeleri yalnızca, bu tabloda tanımlı olabilir. Not: yalnızca dize anahtarları desteklenir. |
| Uri |Dize açıklanan (aşağıdaki tabloya bakın) |AMQP değeri |
| DateTimeOffset |Uzun açıklanan (aşağıdaki tabloya bakın) |AMQP değeri |
| TimeSpan |Uzun açıklanan (aşağıya bakın) |AMQP değeri |
| Akış |İkili |AMQP verileri (birden çok olabilir). Stream nesnesi okunan ham bayt veri bölümler. |
| Başka bir nesne |İkili |AMQP verileri (birden çok olabilir). DataContractSerializer veya uygulama tarafından sağlanan bir seri hale getirici kullanan nesnenin seri hale getirilmiş ikili içerir. |

| .NET türü | Eşlenen AMQP türü açıklanan | Notlar |
| --- | --- | --- |
| Uri |`<type name=”uri” class=restricted source=”string”> <descriptor name=”com.microsoft:uri” /></type>` |Uri.AbsoluteUri |
| DateTimeOffset |`<type name=”datetime-offset” class=restricted source=”long”> <descriptor name=”com.microsoft:datetime-offset” /></type>` |DateTimeOffset.UtcTicks |
| TimeSpan |`<type name=”timespan” class=restricted source=”long”> <descriptor name=”com.microsoft:timespan” /></type> ` |TimeSpan.Ticks |

## <a name="behavioral-differences"></a>Davranış farklılıkları

AMQP, varsayılan protokol karşılaştırıldığında kullanırken hizmet veri yolu .NET API davranışını bazı küçük farklılıklar vardır:

* [OperationTimeout] [ OperationTimeout] özelliği yoksayılır.
* `MessageReceiver.Receive(TimeSpan.Zero)` olarak uygulanan `MessageReceiver.Receive(TimeSpan.FromSeconds(10))`.
* Kilit belirteçleri tarafından iletileri Tamamlanıyor yalnızca başlangıçta iletileri alınan ileti alıcı tarafından yapılabilir.

## <a name="control-amqp-protocol-settings"></a>Denetim AMQP protokolü ayarları

[.NET API'lerini](/dotnet/api/) AMQP protokolünü davranışını denetlemek için çeşitli ayarlar kullanıma:

* **[MessageReceiver.PrefetchCount](/dotnet/api/microsoft.servicebus.messaging.messagereceiver.prefetchcount?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessageReceiver_PrefetchCount)**: bağlantı uygulanan ilk kredi denetler. Varsayılan değer 0'dır.
* **[MessagingFactorySettings.AmqpTransportSettings.MaxFrameSize](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.maxframesize?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_MaxFrameSize)**: AMQP çerçeve sınırını sunulan bağlantıda anlaşması sırasında denetimleri zaman açın. Varsayılan 65.536 bayt'tır.
* **[MessagingFactorySettings.AmqpTransportSettings.BatchFlushInterval](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.batchflushinterval?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_BatchFlushInterval)**: aktarımları batchable varsa, bu değer değerlendirmeleri göndermek için en büyük gecikme belirler. Varsayılan olarak Gönderenler/alıcılar tarafından devralınır. Tek tek gönderenin alıcı 20 milisaniyedir Varsayılanı geçersiz kılabilirsiniz.
* **[MessagingFactorySettings.AmqpTransportSettings.UseSslStreamSecurity](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.usesslstreamsecurity?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_UseSslStreamSecurity)**: AMQP bağlantı bir SSL bağlantısı üzerinden kuran olup olmadığını denetler. Varsayılan değer **doğru**.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi hazır mısınız? Aşağıdaki bağlantıları ziyaret edin:

* [Hizmet veri yolu AMQP genel bakış]
* [AMQP 1.0 protokol kılavuzu]

[Create a Service Bus namespace using the Azure portal]: service-bus-create-namespace-portal.md
[DataContractSerializer]: https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer.aspx
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage?view=azureservicebus-4.0.0
[Microsoft.ServiceBus.Messaging.MessagingFactory.AcceptMessageSession]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory.acceptmessagesession?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessagingFactory_AcceptMessageSession
[OperationTimeout]: /dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout
[NuGet]: http://nuget.org/packages/WindowsAzure.ServiceBus/
[Azure portal]: https://portal.azure.com
[Hizmet veri yolu AMQP genel bakış]: service-bus-amqp-overview.md
[AMQP 1.0 protokol kılavuzu]: service-bus-amqp-protocol-guide.md

