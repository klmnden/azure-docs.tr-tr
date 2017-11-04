---
title: "Azure Service Bus coğrafi olağanüstü durum kurtarma | Microsoft Docs"
description: "Coğrafi bölgeler için yük devretme kullanın ve Azure hizmet veri yolundaki olağanüstü durum kurtarma gerçekleştirmek nasıl"
services: service-bus-messaging
documentationcenter: 
author: christianwolf42
manager: timlt
editor: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2017
ms.author: sethm
ms.openlocfilehash: 49f2992245d694f85b7b1f1c34339f1445c9d699
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2017
---
# <a name="azure-service-bus-geo-disaster-recovery-preview"></a>Azure Service Bus coğrafi olağanüstü durum kurtarma (Önizleme)

Bölgesel veri merkezleri kapalı kalma süresi karşılaştığınızda farklı bir bölge veya veri merkezi içinde çalışmaya devam etmek veri işleme için önemlidir. Bu nedenle, *coğrafi olağanüstü durum kurtarma* ve *coğrafi çoğaltma* tüm kuruluş için önemli özelliklerdir. Azure Service Bus coğrafi olağanüstü durum kurtarma ve coğrafi çoğaltma, ad alanı düzeyinde destekler. 

Coğrafi olağanüstü durum kurtarma Önizleme şu anda yalnızca iki bölgelerde kullanılabilir (**Kuzey Orta ABD** ve **Orta Güney ABD)**.

## <a name="outages-and-disasters"></a>Kesintiler ve olağanüstü durumları

[En iyi uygulamalar için Service Bus kesintileri ve olağanüstü karşı uygulamalar insulating](service-bus-outages-disasters.md) makale dikkate almak önemlidir "kesintileri" ve "afetler," arasında bir ayrım sağlar. Bir *kesinti* geçici olarak kullanım dışı kalması Azure Service Bus olduğu ve bazı bileşenleri bir Mesajlaşma deposu veya hatta tüm veri merkezi gibi hizmet etkileyebilir. Sorun çözüldükten sonra Bununla birlikte, hizmet veri yolu yeniden kullanılabilir hale gelir. Genellikle, bir kesinti iletileri veya diğer veri kaybına neden olmaz. Bu tür bir kesinti örneği veri merkezinde bir güç kesintisi olabilir.

A *olağanüstü durum* Service Bus kalıcı ya da daha uzun vadeli kaybı tanımlanan [ölçek birimi](service-bus-architecture.md#service-bus-scale-units) veya datacenter. Veri merkezi olabilir veya yeniden kullanılabilir olmaktan ya da aşağı saatlerce veya günlerce olması olabilir. Bu tür afetler yangın, taşmasını veya deprem gösterilebilir. Kalıcı hale bir olağanüstü durum bazı iletiler veya diğer veri kaybına neden olabilir. Ancak, çoğu durumda olması gerekir veri kaybı ve yedekleme veri merkezi başladıktan sonra iletileri kurtarılabilir.

Azure Service Bus coğrafi olağanüstü durum kurtarma özelliği bir olağanüstü durum kurtarma çözümüdür. Bu makalede açıklanan iş akışı ve kavramları olağanüstü durum senaryoları ve değil geçici ya da geçici kesintileri uygulanır.  

## <a name="basic-concepts-and-terms"></a>Temel kavramlar ve terimler

Olağanüstü Durum Kurtarma özelliği meta verileri olağanüstü durum kurtarma uygular ve birincil ve ikincil olağanüstü durum kurtarma ad alanları dayanır. Coğrafi olağanüstü durum kurtarma özelliği için kullanılabilir olduğunu unutmayın [Premium ad](service-bus-premium-messaging.md) yalnızca. Bağlantı bir diğer ad üzerinden yapılan gibi herhangi bir bağlantı dizesi değişiklik yapılması gerekmez.

Bu makalede aşağıdaki terimler kullanılır:

-  *Diğer ad*: ana bağlantı dizesi.

-  *Birincil/ikincil ad alanı*: diğer adı için karşılık gelen ad alanları açıklar. Birincil "etkin" ileti alıp, ikincil "pasif" ve iletileri almaz. Her ikisi arasındaki bir meta veri eşitleme, olduğundan her ikisi de iletileri uygulama kod değişiklikleri olmadan sorunsuz bir şekilde kabul edebilir.

-  *Meta veri*: Azure Service Bus nesneleri, gösterimi. Şu anda yalnızca meta veri destekliyoruz.

-  *Yük devretme*: ikincil ad alanı etkinleştirme işlemi. Yeniden kullanılabilir olduktan sonra eski birincil ad alanından iletileri çekme ve ad alanını silin. Başka bir yük devretme oluşturmak için bir eşleştirme için yeni bir ikincil ad alanı ekleyin. Eski birincil ad alanı bir yük devretme işleminden sonra yeniden kullanmak istiyorsanız, önce tüm var olan varlıkları ad alanından kaldırmanız gerekir. Bunu yapmadan önce tüm iletileri almak emin olun.

## <a name="failover-workflow"></a>Yük devretme iş akışı

Aşağıdaki bölümde ilk yük devretme ve bu noktadan İleri gitmektedir nasıl ayarlama sürecinin tamamı bir genel bakıştır.

![1][]

İlk birincil ve ikincil bir ad alanı ayarlayın ve sonra bir eşleştirme oluşturun. Bu eşleştirme bağlanmak için kullanabileceğiniz bir diğer ad sağlar. Bir diğer ad kullandığından, bağlantı dizelerinizi değiştirmeniz gerekmez. Yalnızca yeni ad alanları, yük devretme çifti için eklenebilir. Son olarak, bazı trigger mantığı (örneğin, ad alanı mevcut değil ve yük devretme başlatır, algılanan bazı iş mantığı) eklemeniz gerekir. Kullanılabilirlik ad alanını kullanarak denetleyebilirsiniz [ileti gözatma](message-browsing.md) Service Bus yeteneği.

İzleme ve olağanüstü durum kurtarma ayarladıktan sonra Yük devretme işlemi bakabilirsiniz. Bir yük devretme tetikleyici başlatır veya yük devretme manuel olarak başlatmak, iki adım gereklidir:

1. Başka bir kesinti durumunda yeniden yük devretme kullanabilmek ister. Bu nedenle, ikinci bir pasif ad alanı ayarlama ve eşleştirme güncelleştirin. 
2. Yeni ad alanı kullanılabilir olduğunda eski birincil ad alanından iletileri çeker. Bundan sonra yeniden kullanabilir veya eski birincil ad alanını silin.

![2][]

## <a name="set-up-disaster-recovery"></a>Olağanüstü durum kurtarma işlemini ayarlama

Bu bölümde kendi hizmet veri yolu coğrafi olağanüstü durum kurtarma kodunuzu nasıl oluşturulacağını açıklar. Bunu yapmak için iki ad alanı bağımsız konumlarda gerekir; Örneğin, Güney ABD ve Kuzey Orta ABD. Aşağıdaki örnek, Visual Studio 2017 kullanır.

1.  Yeni bir **konsol App(.Net Framework)** Visual Studio'da ve bir ad verin; örneğin, **SBGeoDR**.

2.  Aşağıdaki NuGet paketlerini yükleyin:
    1.  Microsoft.IdentityModel.Clients.activedirectory tarafından
    2.  Microsoft.Azure.Management.ServiceBus

3. Kullanmakta olduğunuz Newtonsoft.Json NuGet paketinin sürümünü, sürüm 10.0.3 olduğundan emin olun.

3.  Aşağıdakileri ekleyin `using` deyimleri kodunuz için:

    ```csharp
    using System.Threading;
    using Microsoft.Azure.Management.ServiceBus;
    using Microsoft.Azure.Management.ServiceBus.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

4. Değiştirme, `main()` yöntemi iki premium ad alanı eklemek için:

    ```csharp
    // 1. Create primary namespace (optional).

    var namespaceParams = new SBNamespace()
    {
        Location = "South Central US",
        Sku = new SBSku()
        {
            Name = SkuName.Premium,
            Capacity = 1
        }

    };

    var namespace1 = client.Namespaces.CreateOrUpdate(resourceGroupName, geoDRPrimaryNS, namespaceParams);

    // 2. Create secondary namespace (optional if you already have an empty namespace available).

    var namespaceParams2 = new SBNamespace()
    {
        Location = "North Central US",
        Sku = new SBSku()
        {
            Name = SkuName.Premium,
            Capacity = 1
        }

    };

    // If you re-run this program while namespaces are still paired this operation will fail with a bad request.
    // This is because we block all updates on secondary namespaces once it is paired.

    var namespace2 = client.Namespaces.CreateOrUpdate(resourceGroupName, geoDRSecondaryNS, namespaceParams2);
    ```

5. İki ad arasında eşleştirme etkinleştirin ve daha sonra varlıklara bağlanmak için kullandığınız diğer alın:

    ```csharp
    // 3. Pair the namespaces to enable DR.

    ArmDisasterRecovery drStatus = client.DisasterRecoveryConfigs.CreateOrUpdate(
        resourceGroupName,
        geoDRPrimaryNS,
        alias,
        new ArmDisasterRecovery { PartnerNamespace = geoDRSecondaryNS });

    // A similar loop can be used to check if other operations (Failover, BreakPairing, Delete) 
    // mentioned below have been successful.
    while (drStatus.ProvisioningState != ProvisioningStateDR.Succeeded)
    {
        Console.WriteLine("Waiting for DR to be set up. Current state: " +
        drStatus.ProvisioningState);
        drStatus = client.DisasterRecoveryConfigs.Get(
        resourceGroupName,
        geoDRPrimaryNS,
        alias);

        Thread.CurrentThread.Join(TimeSpan.FromSeconds(30));
    }
    ```

İki eşleştirilmiş ad alanları başarıyla oluşturdunuz. Artık meta veri eşitlemesini izlemek için varlıklar oluşturabilirsiniz. Daha sonra eşitlemek meta veriler için biraz zaman tanıyın hemen bir yük devretme gerçekleştirmek istiyorsanız. Örneğin, bir kısa bekleme süresini ekleyebilirsiniz:

```csharp
client.Topics.CreateOrUpdate(resourceGroupName, geoDRPrimaryNS, "myTopic", new SBTopic());
client.Subscriptions.CreateOrUpdate(resourceGroupName, geoDRPrimaryNS, "myTopic", "myTopic-Sub1", new SBSubscription());

// sleeping to allow metadata to sync across primary and secondary
Thread.Sleep(1000 * 60);
```

Bu noktada varlıkları portalı üzerinden veya Azure Resource Manager aracılığıyla ekleyin ve nasıl eşitlendikten bakın. Planınızı el ile yük devretme olmadığı sürece, birincil ad alanınızı izler ve yük devretme kullanılamaz hale gelirse başlatan bir uygulama oluşturmanız gerekir. 

## <a name="initiate-a-failover"></a>Bir yük devretme başlatın

Aşağıdaki kod, bir yük devretme başlatın gösterilmektedir:

```csharp
// Note that this failover operation is always run against the secondary namespace 
// (because primary might be down at time of failover).

client.DisasterRecoveryConfigs.FailOver(resourceGroupName, geoDRSecondaryNS, alias);
```

Yük devretme tetiklemek sonra pasif yeni bir ad alanı ekleyin ve eşleştirme yeniden oluşturun. Yeni bir eşleştirme oluşturmak için kodu önceki bölümde gösterilir. Ayrıca, yük devretme işlemi tamamlandıktan sonra eski birincil ad alanından iletileri kaldırmanız gerekir. Kuyruktan ileti alma örnekleri için bkz: [kuyruklarla çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md).

## <a name="how-to-disable-geo-disaster-recovery"></a>Coğrafi olağanüstü durum kurtarma devre dışı bırakma

Aşağıdaki kod, bir ad alanı eşleştirme devre dışı bırakma gösterir:

```csharp
client.DisasterRecoveryConfigs.BreakPairing(resourceGroupName, geoDRPrimaryNS, alias);
```

Aşağıdaki kod, oluşturduğunuz diğer siler:

```csharp
// Delete the DR config (alias).
// Note that this operation must run against the namespace to which the alias is currently pointing.
// If you break the pairing and want to delete the namespaces afterwards, you must delete the alias first.

client.DisasterRecoveryConfigs.Delete(resourceGroupName, geoDRPrimaryNS, alias);
```

## <a name="steps-after-a-failover-failback"></a>Bir yük devretme (yeniden çalışma) adımlar

Yük devretme sonrasında, aşağıdaki iki adımı gerçekleştirin:

1.  Yeni bir pasif ikincil ad alanı oluşturun. Kod, önceki bölümde gösterilir.
2.  Kalan iletileri, kuyruktan kaldırın.

## <a name="alias-connection-string-and-test-code"></a>Diğer ad bağlantı dizesi ve test kodu

Yük devretme işlemini test etmek isterseniz, diğer adı kullanarak birincil ad iletileri iter örnek bir uygulama yazabilirsiniz. Bunu yapmak için diğer ad bağlantı dizesi etkin ad alanından elde emin olun. Geçerli Önizleme sürümü ile doğrudan bağlantı dizesini almak için diğer arabirimi yoktur. Aşağıdaki kod örneği, önce ve yük devretme sonrasında bağlanır:

```csharp
var accessKeys = client.Namespaces.ListKeys(resourceGroupName, geoDRPrimaryNS, "RootManageSharedAccessKey");
var aliasPrimaryConnectionString = accessKeys.AliasPrimaryConnectionString;
var aliasSecondaryConnectionString = accessKeys.AliasSecondaryConnectionString;

if(aliasPrimaryConnectionString == null)
{
    accessKeys = client.Namespaces.ListKeys(resourceGroupName, geoDRSecondaryNS, "RootManageSharedAccessKey");
    aliasPrimaryConnectionString = accessKeys.AliasPrimaryConnectionString;
    aliasSecondaryConnectionString = accessKeys.AliasSecondaryConnectionString;
}
```

## <a name="next-steps"></a>Sonraki adımlar

- Coğrafi olağanüstü durum kurtarma Bkz [burada REST API Başvurusu](/rest/api/servicebus/disasterrecoveryconfigs).
- Coğrafi olağanüstü durum kurtarma işlemi Çalıştır [github'da örnek](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR2).
- Coğrafi olağanüstü durum kurtarma Bkz [bir diğer ad iletileri gönderir örnek](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/TestGeoDR/ConsoleApp1).

Service Bus Mesajlaşma hizmeti hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Service Bus ile ilgili temel bilgiler](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
* [REST API'si](/rest/api/servicebus/) 

[1]: ./media/service-bus-geo-dr/geo1.png
[2]: ./media/service-bus-geo-dr/geo2.png
