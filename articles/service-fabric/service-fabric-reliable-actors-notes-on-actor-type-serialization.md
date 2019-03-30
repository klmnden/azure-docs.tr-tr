---
title: Reliable Actors Notlar aktör türü seri hale getirme | Microsoft Docs
description: Service Fabric Reliable Actors durumları ve arabirimlerini tanımlamak için kullanılan seri hale getirilebilir sınıflar tanımlamak için temel gereksinimleri açıklanır
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: 6e50e4dc-969a-4a1c-b36c-b292d964c7e3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: c8eeeb0ade6ca002adf3211cbf49127be9b76edb
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58667524"
---
# <a name="notes-on-service-fabric-reliable-actors-type-serialization"></a>Service Fabric Reliable Actors hakkında notlar serileştirme türü
Tüm yöntemlerin bağımsız değişkenlerini, görev sonuç türleri döndürülen her bir yöntemin bir aktör arabirimi tarafından ve bir aktörün durum Yöneticisi'nde depolanan nesnelere olmalıdır [veri sözleşme seri hale getirilebilir](/dotnet/framework/wcf/feature-details/types-supported-by-the-data-contract-serializer). Bu tanımlı yöntem bağımsız değişkenleri için de geçerlidir [aktör olay arabirimleri](service-fabric-reliable-actors-events.md). (Aktör olay arabirim yöntemleri her zaman void döndürür.)

## <a name="custom-data-types"></a>Özel veri türleri
Bu örnekte, aşağıdaki aktör arabirimi adlı bir özel veri türü döndüren bir yöntem tanımlar `VoicemailBox`:

```csharp
public interface IVoiceMailBoxActor : IActor
{
    Task<VoicemailBox> GetMailBoxAsync();
}
```

```Java
public interface VoiceMailBoxActor extends Actor
{
    CompletableFuture<VoicemailBox> getMailBoxAsync();
}
```

Durum Yöneticisi depolamak için kullandığı bir aktör arabirimi uygulanan bir `VoicemailBox` nesnesi:

```csharp
[StatePersistence(StatePersistence.Persisted)]
public class VoiceMailBoxActor : Actor, IVoicemailBoxActor
{
    public VoiceMailBoxActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<VoicemailBox> GetMailboxAsync()
    {
        return this.StateManager.GetStateAsync<VoicemailBox>("Mailbox");
    }
}

```

```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class VoiceMailBoxActorImpl extends FabricActor implements VoicemailBoxActor
{
    public VoiceMailBoxActorImpl(ActorService actorService, ActorId actorId)
    {
         super(actorService, actorId);
    }

    public CompletableFuture<VoicemailBox> getMailBoxAsync()
    {
         return this.stateManager().getStateAsync("Mailbox");
    }
}

```

Bu örnekte, `VoicemailBox` nesne seri hale getirilmiş olduğunda:

* Nesne, çağıran bir aktör örneği arasında aktarılır.
* Nesne, burada, diske yazılır ve diğer düğümlere çoğaltılan durum Yöneticisi'nde kaydedilir.

Güvenilir aktör çerçevesinde DataContract serileştirme kullanır. Bu nedenle, özel veri nesneleri ve üyeleri ile Açıklama eklenmelidir **DataContract** ve **DataMember** öznitelikleri, sırasıyla.

```csharp
[DataContract]
public class Voicemail
{
    [DataMember]
    public Guid Id { get; set; }

    [DataMember]
    public string Message { get; set; }

    [DataMember]
    public DateTime ReceivedAt { get; set; }
}
```
```Java
public class Voicemail implements Serializable
{
    private static final long serialVersionUID = 42L;

    private UUID id;                    //getUUID() and setUUID()

    private String message;             //getMessage() and setMessage()

    private GregorianCalendar receivedAt; //getReceivedAt() and setReceivedAt()
}
```


```csharp
[DataContract]
public class VoicemailBox
{
    public VoicemailBox()
    {
        this.MessageList = new List<Voicemail>();
    }

    [DataMember]
    public List<Voicemail> MessageList { get; set; }

    [DataMember]
    public string Greeting { get; set; }
}
```
```Java
public class VoicemailBox implements Serializable
{
    static final long serialVersionUID = 42L;
    
    public VoicemailBox()
    {
        this.messageList = new ArrayList<Voicemail>();
    }

    private List<Voicemail> messageList;   //getMessageList() and setMessageList()

    private String greeting;               //getGreeting() and setGreeting()
}
```


## <a name="next-steps"></a>Sonraki adımlar
* [Aktör yaşam döngüsü ve atık toplama](service-fabric-reliable-actors-lifecycle.md)
* [Aktör süreölçerler ve anımsatıcılar](service-fabric-reliable-actors-timers-reminders.md)
* [Aktör olayları](service-fabric-reliable-actors-events.md)
* [Aktör'na yeniden giriş](service-fabric-reliable-actors-reentrancy.md)
* [Aktör çok biçimlilik ve nesne yönelimli tasarım desenleri](service-fabric-reliable-actors-polymorphism.md)
* [Aktör tanılama ve performans izleme](service-fabric-reliable-actors-diagnostics.md)
