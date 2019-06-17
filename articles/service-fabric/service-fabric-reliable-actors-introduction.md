---
title: Service Fabric Reliable Actors hizmetine genel bakış | Microsoft Docs
description: Service Fabric Reliable Actors programlama modelini giriş.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: 7fdad07f-f2d6-4c74-804d-e0d56131f060
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/01/2017
ms.author: vturecek
ms.openlocfilehash: 5a237e23dffed76e6122e17b59c85d20ca7e1baf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60727207"
---
# <a name="introduction-to-service-fabric-reliable-actors"></a>Service Fabric Reliable Actors hizmetine giriş
Reliable Actors, temel bir Service Fabric uygulama altyapısıdır [sanal aktör](https://research.microsoft.com/en-us/projects/orleans/) deseni. Reliable Actors API Service Fabric tarafından sağlanan ölçeklenebilirlik ve güvenilirlik garantisi temel alan bir tek iş parçacıklı programlama modeli sağlar.

## <a name="what-are-actors"></a>Aktörler nelerdir?
Aktörün bir yalıtılmış, bağımsız işlem ve tek iş parçacıklı yürütme durumunu birimidir. [Gerçekleştiren deseni](https://en.wikipedia.org/wiki/Actor_model) , bu aktörler çok sayıda eşzamanlı olarak yürütebilir ve birbirinden eşzamanlı veya dağıtılmış sistemler için hesaplama bir modeldir. Aktörler birbiriyle iletişim kurabilir ve daha fazla aktör oluşturabilirler.

### <a name="when-to-use-reliable-actors"></a>Reliable Actors kullanıldığı durumlar
Service Fabric Reliable Actors aktör tasarım deseni uygulamasıdır. Tüm yazılım tasarım deseni ile gibi belirli bir desene kullanıp kullanmayacağınızı olup olmadığını bir yazılımı tasarlama üzerinde sorun göre yapılır karar deseni uyar.

Aktör tasarım olsa da Desen bir dizi dağıtılmış sistem sorunlarını ve senaryoları, desen ve uygulamadan framework kısıtlamaları göz önüne alarak yapılmalıdır dikkatli iyi bir uygun olamaz. Genel rehberlik sağlaması sorun veya senaryo, model gerçekleştiren deseni göz önünde bulundurun:

* Çok sayıda sorunu alanınızı içerir (binlerce veya daha fazlası) küçük, bağımsız ve ayrı birim durumu ve mantığı.
* Dış bileşenlerinden durumu aktörler kümesi sorgulama dahil olmak üzere önemli etkileşim gerektirmeyen tek iş parçacıklı nesneleriyle çalışma istiyorsunuz.
* Aktör örneklerinizi g/ç işlemleri vererek beklenmedik gecikmeler ile çağıranlar engellemez.

## <a name="actors-in-service-fabric"></a>Service fabric'te aktörler
Service Fabric'te Reliable Actors framework aktörler uygulanır: Üst kısmındaki yerleşik bir aktör desen tabanlı bir uygulama çerçevesi [Service Fabric güvenilir hizmetler](service-fabric-reliable-services-introduction.md). Yazdığınız her Reliable Actor hizmetinin aslında bir bölümlenmiş, durum bilgisi olan güvenilir hizmetidir.

Her aktör, bir .NET nesnesini .NET türünün bir örneği olduğu şekilde özdeş bir aktör türü örneği olarak tanımlanır. Örneğin, bir hesap makinesi işlevselliğini uygulayan bir aktör türü olabilir ve çeşitli düğümler bir küme genelinde dağıtılan birçok aktör türü olabilir. Her bir aktör, bir aktör kimliğine göre benzersiz olarak tanımlanır

## <a name="actor-lifetime"></a>Aktör ömrü
Service Fabric aktör sanal kendi ömürlerine kendi bellek içi gösterimine bağlanmayan anlamına gelir. Sonuç olarak, bunlar açıkça oluşturulmuş veya yok gerekmez. Reliable Actors çalışma zamanı, aktör kimliği için bir isteği aldığında bir aktör ilk zaman otomatik olarak etkinleştirir. Bir aktör, bir süre için kullanılmıyorsa, Reliable Actors çalışma zamanının çöp-bellekteki nesne toplar. Daha sonra yeniden etkinleştirilmesi gerekir, aktörün varlığı bilgisine de korur. Daha fazla ayrıntı için [aktör yaşam döngüsü ve atık toplama](service-fabric-reliable-actors-lifecycle.md).

Bu sanal aktör ömrü soyutlama sanal aktör modeli sonucunda bazı uyarılar gerçekleştirir ve aslında Reliable Actors uygulaması bu modelden bazen farklıdır.

* Bir aktör (aktör nesnenin oluşturulması neden) otomatik olarak etkinleştirilir ilk kez bir ileti, aktör kimliğini gönderilir Belirli bir süre sonra çöp olarak toplanacak aktör nesnedir. Gelecekte, aktör kodu yeniden kullanarak oluşturulması için yeni bir aktör nesne neden olur. Nesnenin yaşam süresi bir aktörün durumu outlives durum Yöneticisi'nde depolandığında.
* Herhangi bir aktör yöntemi arayan bir aktör kimliği için o aktör etkinleştirir. Bu nedenle aktör türleri çalışma zamanı tarafından örtük olarak çağırılamaz oluşturucularına sahiptir. Bu nedenle, hizmet tarafından aktörün oluşturucusuna parametre geçirilen ancak istemci kodu aktör türü oluşturucusuna parametreleri geçiremezsiniz. Aktör başlatma parametreleri istemciden gerektiriyorsa aktörler kısmen başlatılmış bir durumda diğer yöntemleri üzerinde çağrılır zamanına göre oluşturulabilir, sonucudur. Aktörün istemci etkinleştirme için tek giriş noktası yok.
* Reliable Actors aktör nesneleri örtük olarak oluşturmuş olsanız da; açıkça bir aktör ve durumunu silme becerisine sahip.

## <a name="distribution-and-failover"></a>Dağıtım ve yük devretme
Ölçeklenebilirlik ve güvenilirlik sağlamak için Service Fabric aktör küme genelinde dağıtır ve otomatik olarak onları başarısız düğümlerden sağlıklı olanlara gerekli olarak geçirir. Üzerinden bir Özet budur bir [bölümlenmiş, durum bilgisi olan Reliable Services](service-fabric-concepts-partitioning.md). Dağıtım, ölçeklenebilirlik, güvenilirlik ve otomatik yük devretme: all aktörler içinde çalışan olgu da durum bilgisi olan güvenilir hizmet olarak adlandırılan sağlanan *Actor hizmetinin*.

Aktörler Actor hizmetinin bölümler arasında dağıtılır ve bu bölümler bir Service Fabric kümesindeki düğümler arasında dağıtılır. Her hizmet bölümü aktörler kümesini içerir. Service Fabric, dağıtım ve yük devretme hizmet bölümlerini işlemlerini yönetir.

Örneğin, bir aktör hizmeti dokuz bölümleri varsayılan aktör bölüm yerleştirme kullanarak üç düğümlerine dağıtılmış olan thusly dağıtılmış:

![Reliable Actors dağıtım][2]

Aktör çerçevesinde sizin için bölüm düzeni ve anahtar aralığı ayarlarını yönetir. Bu, bazı seçenekler basitleştirir ancak bazı önemli noktalar da gerçekleştirir:

* Reliable Services bir bölümleme düzeni, (bir aralık bölümleme düzenini kullanırken) anahtar aralığı seçin ve bölüm sayısı olanak tanır. Reliable Actors (Tekdüzen Int64 düzeni) bölümleme aralığın sınırlıdır ve tam Int64 anahtar aralığı kullanmanızı gerektirir.
* Varsayılan olarak, aktör rastgele Tekdüzen dağıtımlarında kaynaklanan bölümlere yerleştirilir.
* Aktörler rastgele yerleştirildiğinden aktör işlemler her zaman serileştirme ve seri durumundan çıkarma, gecikme süresi ve ek yükü yansıtılmasını yöntemi arama verilerinin dahil olmak üzere, ağ iletişimi gerektirir beklenmelidir.
* Gelişmiş senaryolar Int64 aktör belirli bölümlere eşlemek kimlikleri kullanarak denetim aktör bölüm yerleştirme için mümkündür. Ancak, bunun yapılması bölümler arasında bu nedenle aktör dengesiz bir dağıtım sonuçlanabilir.

Aktör hizmeti nasıl bölümlenir ile ilgili daha fazla bilgi için [kavramları aktörler için bölümleme](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors).

## <a name="actor-communication"></a>Aktör iletişimi
Aktör etkileşimleri interface'i gerçekleştiren ve aynı arabirimi üzerinden bir aktör için bir proxy alır istemci tarafından paylaşılan bir arabirimde tanımlanır. Bu arabirim, aktör metotları zaman uyumsuz olarak çağırma için kullanıldığından, görev döndüren arabirimdeki her yöntem olmalıdır.

Yöntem çağrıları ve yanıtlarını sonuçta ağ isteklerine kümede, bu nedenle bağımsız değişkenler neden ve sonuç türleri döndürmeleri görevlerin platform tarafından seri hale getirilebilir olması gerekir. Özellikle, olmalıdır [veri sözleşme seri hale getirilebilir](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).

### <a name="the-actor-proxy"></a>Aktör proxy
Reliable Actors istemci API aktör örneği bir aktör istemci arasındaki iletişimi sağlar. Aktörün ile iletişim kurmak için bir istemci aktör arabirimi uygulayan bir aktör proxy nesnesi oluşturur. İstemci proxy nesnesi yöntemleri çağırarak aktör ile etkileşime girer. Aktör proxy istemci aktör ve aktör aktör iletişim için kullanılabilir.

```csharp
// Create a randomly distributed actor ID
ActorId actorId = ActorId.CreateRandom();

// This only creates a proxy object, it does not activate an actor or invoke any methods yet.
IMyActor myActor = ActorProxy.Create<IMyActor>(actorId, new Uri("fabric:/MyApp/MyActorService"));

// This will invoke a method on the actor. If an actor with the given ID does not exist, it will be activated by this method call.
await myActor.DoWorkAsync();
```

```java
// Create actor ID with some name
ActorId actorId = new ActorId("Actor1");

// This only creates a proxy object, it does not activate an actor or invoke any methods yet.
MyActor myActor = ActorProxyBase.create(actorId, new URI("fabric:/MyApp/MyActorService"), MyActor.class);

// This will invoke a method on the actor. If an actor with the given ID does not exist, it will be activated by this method call.
myActor.DoWorkAsync().get();
```


Aktör proxy nesnesi oluşturmak için kullanılan bilgilerin iki parça kimliği ve uygulama adı olduğuna dikkat edin. Uygulama adı tanımlar ancak kimliği aktör benzersiz olarak tanımlayan [Service Fabric uygulaması](service-fabric-reliable-actors-platform.md#application-model) aktör dağıtıldığı.

`ActorProxy`(C#) / `ActorProxyBase`İstemci tarafında (Java) sınıfı aktör Kimliğine göre bulun ve bir iletişim kanalı ile açmak için gerekli çözümlemesi gerçekleştirir. Aktör iletişim hatalarına ve yük devretme durumunda bulmak için de deneme. Sonuç olarak, ileti teslimi, aşağıdaki özelliklere sahiptir:

* İleti, en iyi çaba teslimatıdır.
* Aktör, aynı istemciden yinelenen iletileri alabilirsiniz.

## <a name="concurrency"></a>Eşzamanlılık
Reliable Actors çalışma zamanı, aktör yöntemleri erişmek için bir basit sırayla oynadıkları erişim modeli sağlar. Başka bir deyişle, birden fazla iş parçacığı herhangi bir zamanda bir aktör nesnenin kod içinde etkin olabilir. Veri erişimi için eşitleme mekanizmaları gerek olmadığından erişim sırayla oynadıkları eş zamanlı sistemleri büyük ölçüde kolaylaştırır. Ayrıca, ilgili özel konular her aktör örneğinin ile tek iş parçacıklı erişimi yapısı sistemleri tasarlanmalıdır anlamına gelir.

* Bir tek aktör örneği aynı anda birden fazla isteği işleyemiyor. Eş zamanlı istekleri işlemek üzere bekleniyorsa aktör örneği bir aktarım hızı performans sorununa neden olabilir.
* Bir dış istek aktörleri birine aynı anda yapılır ancak iki aktörler arasında döngüsel bir istek varsa aktörler birbirine kilitlenme. Actor çalışma zamanına otomatik olarak aktör çağrılarda zaman aşımına uğrar ve olası kilitlenme durumları kesmek için çağırana özel durum.

![Reliable Actors iletişimi][3]

### <a name="turn-based-access"></a>Kullanıma dayalı olarak erişimi
Bir bırakma diğer aktörler veya istemcilere gelen bir isteğe yanıt olarak bir aktör yöntemi tam olarak yürütülmesini veya tam olarak yürütülmesini oluşur bir [Zamanlayıcı/anımsatıcı](service-fabric-reliable-actors-timers-reminders.md) geri çağırma. Bu yöntemleri ve geri aramalar zaman uyumsuz olmasına karşın, aktör çalışma zamanı bunları Interleave değil. Yeni bir kullanıma izin verilmeden önce bir bırakma eksiksiz olması gerekir. Diğer bir deyişle, şu anda yürütülmekte olan bir aktör yöntemi veya Zamanlayıcı/anımsatıcı geri çağırma yeni bir yöntem çağrısından önce tam olarak bitmiş olmalıdır veya geri çağırma izin verilir. Bir yöntem veya geri çağırma yürütme yöntemden döndürdü veya geri çağırma ve yöntem veya geri çağırma tarafından döndürülen görev tamamlandığında tamamlanmış kabul edilir. Bu, sırayla oynadıkları eşzamanlılık bile farklı yöntemleri, zamanlayıcılar ve geri çağırmalar arasında dikkate değer vurgulama olur.

Aktörler çalışma zamanı tarafından bir bırakma başlangıcını ve kilidi serbest bırakma sonunda bir aktör başına kilit alınırken sırayla oynadıkları eşzamanlılık zorlar. Bu nedenle, sırayla oynadıkları eşzamanlılık aktör başına temelinde ve değil aktörler arasında zorunlu tutulur. Aktör yöntemleri ve Zamanlayıcı/anımsatıcı geri çağırmaları adına farklı aktör eşzamanlı olarak yürütebilir.

Aşağıdaki örnek, yukarıdaki kavramları göstermektedir. İki zaman uyumsuz yöntemleri uygulayan bir aktör türü göz önünde bulundurun (örneğin *Method1* ve *Method2*), bir Zamanlayıcı ve anımsatıcı bir. Aşağıdaki diyagramda iki aktörler adına bu yöntemleri ve geri aramalar yürütülmesi için bir zaman çizelgesi örneği gösterilmektedir (*ActorId1* ve *ActorId2*) bu aktör türe ait.

![Reliable Actors çalışma zamanı sırayla oynadıkları eşzamanlılık ve erişim][1]

Bu diyagram, bu kuralları aşağıdaki gibidir:

* Her dikey çizgi belirli bir aktör adına bir yöntem veya bir geri çağırma yürütülmesini mantıksal akışı gösterilmektedir.
* Dikey her satırda işaretlenmiş olayların kronolojik sırayla eskiler gerçekleşen yeni olaylar ile oluşur.
* Farklı renkler, farklı aktörler için karşılık gelen zaman çizelgeleri için kullanılır.
* Vurgulama aktör başına kilit bir yöntem veya geri çağırma adına tutulduğu süreyi belirtmek için kullanılır.

Dikkate alınması gereken bazı önemli noktalar:

* Sırada *Method1* adına yürütme *ActorId2* istemci isteğine yanıt olarak *xyz789*, başka bir istemci isteği (*abc123*) ulaşan, ayrıca gerektirir *Method1* tarafından yürütülecek *ActorId2*. Ancak, ikinci yürütülmesini *Method1* önceki yürütme tamamlanana kadar yeniden başlamaz. Benzer şekilde, kayıtlı bir anımsatıcı olarak *ActorId2* sırasında ateşlenir *Method1* istemci isteğine yanıt olarak yürütülen *xyz789*. Anımsatıcı geri çağırma yalnızca her iki yürütme sonra yürütülür *Method1* getirildiğinden. Tüm bu olduğu için zorunlu kılınmayı sırayla oynadıkları eşzamanlılık nedeniyle *ActorId2*.
* Benzer şekilde, sırayla oynadıkları eşzamanlılık ayrıca için zorunlu *ActorId1*tarafından yürütülmesi gösterildiği gibi *Method1*, *Method2*ve adınaZamanlayıcısıgeriçağırma *ActorId1* seri biçimde'olmuyor.
* Yürütülmesini *Method1* adına *ActorId1* adına yürütme ile çakışıyor *ActorId2*. Eşzamanlılık sırayla oynadıkları yalnızca bir aktör içinde ve aktörler arasında değil zorunlu olmasıdır.
* Bazı yöntemi/geri çağırma yürütmeleri `Task`(C#) / `CompletableFuture`yöntemin dönüşünün ardından döndürülen tarafından yöntemi/geri çağırma tamamlandıktan (Java). Bazı bazılarında, zaman uyumsuz işlem zaten yöntemi/geri döndürür zamanında tamamlandı. Her iki durumda da, yalnızca hem yöntemi/geri döndürür ve zaman uyumsuz işlem tamamlandıktan sonra aktör başına kilit serbest bırakılır.

### <a name="reentrancy"></a>Yeniden giriş
Aktörler çalışma zamanı'na yeniden giriş varsayılan olarak izin verir. Bu olması durumunda bir aktör yöntemi anlamına gelir *aktör A* üzerinde bir yöntemi çağıran *aktör B*, sırayla çağıran başka bir yöntem üzerinde *aktör A*, yöntemi çalışmasına izin. Aynı mantıksal çağrı zincirine bağlam parçası olmasıdır. Zamanlayıcı ve anımsatıcı tüm çağrıları ile yeni mantıksal çağrı bağlamına başlayın. Bkz: [Reliable Actors yeniden giriş](service-fabric-reliable-actors-reentrancy.md) daha fazla ayrıntı için.

### <a name="scope-of-concurrency-guarantees"></a>Eşzamanlılık garanti kapsamında
Aktörler çalışma zamanı, bu yöntemlerden birini çağırma burada denetlediği durumlarda bu eşzamanlılık garantileri sağlar. Örneğin, Zamanlayıcı ve anımsatıcı geri çağırmaları yanı sıra, istemci isteğine yanıt olarak gerçekleştirilen yöntem çağrıları için bu garantileri sağlar. Ancak, aktör kodu doğrudan aktörler çalışma zamanı tarafından sağlanan mekanizmaları dışında bu yöntemleri çağırırsa, çalışma zamanı eşzamanlılık konusunda garanti sağlayamaz. Örneğin, aktör yöntemler tarafından döndürülen görev ile ilişkili olmayan bazı görev bağlamında yöntemi çağrılırsa, çalışma zamanı tutarlılık garantileri sağlayamaz. Aktör, kendi oluşturan bir iş parçacığından yöntemi çağrılırsa, çalışma zamanı ayrıca tutarlılık garantileri sağlayamaz. Bu nedenle, arka plan işlemleri gerçekleştirmek için aktör kullanmalısınız [aktör süreölçerler ve anımsatıcılar aktör](service-fabric-reliable-actors-timers-reminders.md) sırayla oynadıkları eşzamanlılık Uy.

## <a name="next-steps"></a>Sonraki adımlar
İlk Reliable Actors hizmetinizi oluşturarak başlayın:
   * [.NET üzerinde Reliable Actors hizmetini kullanmaya başlama](service-fabric-reliable-actors-get-started.md)
   * [Üzerinde Java Reliable Actors hizmetini kullanmaya başlama](service-fabric-reliable-actors-get-started-java.md)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-introduction/concurrency.png
[2]: ./media/service-fabric-reliable-actors-introduction/distribution.png
[3]: ./media/service-fabric-reliable-actors-introduction/actor-communication.png
