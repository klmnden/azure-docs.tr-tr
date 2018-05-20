---
title: Aktör güvenilir aktörler Notlar türü seri hale getirme | Microsoft Docs
description: Service Fabric Reliable Actors durumları ve arabirimleri tanımlamak için kullanılan seri hale getirilebilir sınıfları tanımlama için temel gereksinimler açıklanır
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: ''
ms.assetid: 6e50e4dc-969a-4a1c-b36c-b292d964c7e3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 4539351738d423704961eed6e616bd8ac5d682d1
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="notes-on-service-fabric-reliable-actors-type-serialization"></a>Service Fabric Reliable Actors Notlar serileştirme yazın
Bağımsız değişkenleri tüm yöntemlerin aktör arabirimdeki her yöntem tarafından döndürülen sonuç türleri görevleri ve bir aktör'ın durum Yöneticisi'nde depolanan nesneler olmalıdır [veri sözleşmesi seri hale getirilebilir](/dotnet/framework/wcf/feature-details/types-supported-by-the-data-contract-serializer). Bu durum tanımlanan yöntemler bağımsız değişkenleri için de geçerlidir [aktör olay arabirimleri](service-fabric-reliable-actors-events.md). (Aktör olay arabirim yöntemleri her zaman boş döndürmeleri.)

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

Arabirim durumu manager depolamak için kullandığı bir oyuncu tarafından uygulanır bir `VoicemailBox` nesnesi:

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

Bu örnekte, `VoicemailBox` nesne seri hale getirilmiş zaman:

* Nesne çağıran aktör örneği arasında iletilir.
* Nesne, burada, diske yazılır ve diğer düğümlere çoğaltılan durum Yöneticisi'nde kaydedilir.

Güvenilir aktör çerçevesi DataContract serileştirme kullanır. Bu nedenle, özel veri nesneleri ve üyeleri ile Açıklama eklenmelidir **DataContract** ve **DataMember** öznitelikleri, sırasıyla.

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
* [Aktör yaşam döngüsü ve çöp toplama](service-fabric-reliable-actors-lifecycle.md)
* [Aktör zamanlayıcılar ve anımsatıcıları](service-fabric-reliable-actors-timers-reminders.md)
* [Aktör olayları](service-fabric-reliable-actors-events.md)
* [Aktör yeniden giriş](service-fabric-reliable-actors-reentrancy.md)
* [Aktör çok biçimlilik ve nesne odaklı tasarım desenleri](service-fabric-reliable-actors-polymorphism.md)
* [Aktör tanılama ve performans izleme](service-fabric-reliable-actors-diagnostics.md)
