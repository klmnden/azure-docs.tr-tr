---
title: "Service Fabric Reliable Actors genel bakış | Microsoft Docs"
description: "Service Fabric Reliable Actors programlama modelini giriş."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 7fdad07f-f2d6-4c74-804d-e0d56131f060
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: e89be04a0d6fe90a89e293e67d42f0204eb7000a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-service-fabric-reliable-actors"></a>Service Fabric Reliable Actors hizmetine giriş
Güvenilir aktörler dayalı bir çerçevedir Service Fabric uygulaması [sanal aktör](http://research.microsoft.com/en-us/projects/orleans/) düzeni. Güvenilir aktörler API Service Fabric tarafından sağlanan ölçeklenebilirlik ve güvenilirlik garantileri üzerine kurulu bir tek iş parçacıklı programlama modeli sağlar.

## <a name="what-are-actors"></a>Aktör nelerdir?
Bir oyuncu hesaplama ve tek iş parçacıklı yürütme durumuyla yalıtılmış, bağımsız bir birimdir. [Aktör düzeni](https://en.wikipedia.org/wiki/Actor_model) eşzamanlı veya dağıtılmış sistemleri, aynı anda çok sayıda bu aktörler yürütün ve birbirlerine bağımsız olarak bir hesaplama modelidir. Aktör birbirleri ile iletişim kurabilir ve daha fazla aktörler oluşturabilirsiniz.

### <a name="when-to-use-reliable-actors"></a>Reliable Actors kullanma zamanı
Service Fabric Reliable Actors aktör tasarım deseni uygulamasıdır. Tüm yazılım tasarım deseni gibi belirli bir desene kullanıp kullanmayacağınızı olsun veya olmasın bir yazılım tasarım üzerinde sorun göre yapılır karar düzeni sığar.

Aktör tasarım rağmen düzeni iyi bir dağıtılmış sistemler sorun ve senaryo düzeni ve uyguladığı framework kısıtlamaları göz önünde bulundurulmalıdır dikkatli bir sayıya uygun olamaz. Genel bir yönerge olarak sorunu veya senaryo, model için aktör desen göz önünde bulundurun:

* Çok sayıda sorunu alanınızı içerir (binlerce veya daha fazla) küçük, bağımsız ve yalıtılmış birim durumu ve mantığı sayısı.
* Dış bileşenlerinden aktörler kümesi boyunca durumu sorgulama dahil olmak üzere önemli etkileşim gerektirmeyen tek iş parçacıklı nesneleri birlikte çalışmak istediğiniz.
* Aktör örneklerinizi g/ç işlemleri vererek beklenmeyen gecikme ile arayanlar engellemez.

## <a name="actors-in-service-fabric"></a>Service Fabric aktör
Service Fabric içinde aktörler Reliable Actors Framework'te uygulanır: üstünde bir aktör desen tabanlı uygulama altyapısıdır [Service Fabric Reliable Services](service-fabric-reliable-services-introduction.md). Yazdığınız her güvenilir aktör hizmeti aslında bir bölümlenmiş, durum bilgisi olan güvenilir hizmetidir.

Her aktör .NET türünün bir örneği bir .NET nesnesidir şekilde özdeş bir aktör türünün bir örneği olarak tanımlanır. Örneğin, bir hesap makinesi işlevselliğini hayata geçiren bir aktör türü olabilir ve çeşitli düğümlerde küme genelinde dağıtılan birçok aktörler türü olabilir. Her tür aktör aktör kimliği ile benzersiz olarak tanımlanır

### <a name="actor-lifetime"></a>Aktör yaşam süresi
Service Fabric aktör sanal, kendi ömürleri kendi bellek içi gösterimine bağlanmayan anlamına gelir. Sonuç olarak, bunlar açıkça otomatik olarak oluşturulan veya yok gerekmez. Reliable Actors çalışma zamanı bu aktör kimliği için bir istek alırsa aktör ilk zaman otomatik olarak etkinleştirir. Bir oyuncu bir süre kullanılmazsa Reliable Actors çalışma zamanı çöp-bellekteki nesne toplar. Daha sonra yeniden etkinleştirilmesi gerekir, aktör'ın varlığı bilgisini de korur. Daha fazla ayrıntı için bkz: [aktör yaşam döngüsü ve atık toplama](service-fabric-reliable-actors-lifecycle.md).

Bu sanal aktör ömrü soyutlama sanal aktör modeli sonucunda bazı uyarılar gerçekleştirir ve hatta Reliable Actors uygulamaya bazen bu modelden farklılık göstermesi.

* Bir oyuncu (oluşturulması için bir aktör nesne neden olan) otomatik olarak etkinleştirilir aktör kimliğini gönderilen bir ileti ilk kez Belirli bir süre sonra toplanacak aktör nesnesidir. Gelecekte, aktör kimliği yeniden kullanarak oluşturulması için yeni bir aktör nesne neden olur. Nesne ömrü bir aktör'in durumunu outlives durum Yöneticisi'nde depolanan olduğunda.
* Aktör kimliği için herhangi bir aktör yöntemini çağırmadan bu aktör etkinleştirir. Bu nedenle, çalışma zamanı tarafından dolaylı olarak adlandırılan kendi Oluşturucusu aktör türlerine sahip. Bu nedenle, parametreleri aktör'ın oluşturucuya hizmeti tarafından geçirilen ancak istemci kodu aktör tür oluşturucuya parametreleri geçiremezsiniz. Sonucu aktör başlatma parametreleri istemciden gerektiriyorsa aktörler kısmen başlatılmış bir durumda diğer yöntemleri, üzerine denir zamana göre oluşturulması, ' dir. İstemci bir aktör etkinleştirme için tek giriş noktası yok.
* Reliable Actors örtülü olarak aktör nesneleri oluştursanız da; açıkça bir aktör ve durumu silme becerisine sahip.

### <a name="distribution-and-failover"></a>Dağıtım ve yük devretme
Ölçeklenebilirlik ve güvenilirlik sağlamak için Service Fabric aktör küme genelinde dağıtır ve otomatik olarak onları başarısız düğümlerden gerektiği gibi sağlıklı olanlara geçirir. Bu bir üzerinden soyutlamadır bir [bölümlenmiş, durum bilgisi olan güvenilir hizmet](service-fabric-concepts-partitioning.md). Dağıtım, ölçeklenebilirlik, güvenilirlik ve otomatik yük devretme tümüdür aktörler içinde çalıştıran olgu, durum bilgisi olan güvenilir bir hizmet olarak adlandırılan sağlanan *aktör hizmeti*.

Aktör aktör hizmeti bölümleri arasında dağıtılır ve bu bölümler bir Service Fabric kümesindeki düğümler arasında dağıtılır. Her bir hizmet bölümü aktörler kümesini içerir. Service Fabric, dağıtım ve hizmet bölümlerini Yük Devretmesini yönetir.

Örneğin, üç düğümü varsayılan aktör bölüm yerleştirme kullanılarak dağıtılan dokuz bölümleri olan bir aktör hizmeti thusly dağıtılmış:

![Güvenilir aktörler dağıtım][2]

Aktör Framework sizin için bölüm düzeni ve anahtar aralığı ayarlarını yönetir. Bu, bazı seçenekleri basitleştirir ancak ayrıca bazı önem taşır:

* Güvenilir hizmetler, bölümleme düzeni, (bölümleme aralığı kullanırken) anahtar aralığı seçin ve sayısı bölüm olanak verir. Güvenilir aktörler (Tekdüzen Int64 düzeni) bölümleme aralığı için sınırlı ve tam Int64 anahtar aralığı kullanın gerektirir.
* Varsayılan olarak, aktörler rastgele Tekdüzen dağıtımlarında kaynaklanan bölümlere yerleştirilir.
* Aktör rastgele yerleştirildiğinden aktör işlemleri ağ iletişimi, seri hale getirme ve seri durumundan çıkarma gecikme süresi ve ek yükü yansıtılmasını yöntemi çağrısı verileri de dahil olmak üzere her zaman gerektirecektir beklenmelidir.
* Gelişmiş senaryolar Int64 aktör belirli bölümlere eşlemek kimlikleri kullanarak denetim aktör bölüm yerleştirme için mümkündür. Ancak, bunun bölümler böylece aktörler dengesiz bir dağıtım sonuçlanabilir.

Aktör hizmetleri nasıl bölümlenir üzerinde daha fazla bilgi için bkz [kavramları aktörleri bölümleme](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors).

### <a name="actor-communication"></a>Aktör iletişimi
Aktör etkileşimleri arabirimini uygulayan aktör ve aynı arabirimi aracılığıyla bir aktör için bir proxy alır istemci tarafından paylaşılan bir arabirim tanımlanır. Bu arabirimi gerçekleştiren yöntemleri zaman uyumsuz olarak çağırmak için kullanıldığından, arabirimdeki her yöntem görev döndürme olması gerekir.

Yöntem çağrılarını ve yanıtlarını sonuçta ağ isteklerine kümede, bu nedenle bağımsız değişkenler neden ve döndürmeleri görevleri sonuç türleri platform tarafından Serileştirilebilir olmalıdır. Özellikle, olmalıdır [veri sözleşmesi seri hale getirilebilir](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).

#### <a name="the-actor-proxy"></a>Aktör proxy
Reliable Actors İstemcisi API aktör örneği aktör istemci arasında iletişimi sağlar. Bir oyuncu ile iletişim kurmak için bir istemci aktör arabirimini uygulayan bir aktör proxy nesnesi oluşturur. İstemci, proxy nesnesinin yöntemlerde çağırarak aktör ile etkileşime girer. Aktör proxy istemci aktör ve aktör aktör iletişimi için kullanılabilir.

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


Aktör proxy nesnesi oluşturmak için kullanılan bilgileri iki parça aktör kimliği ve uygulama adı olduğunu unutmayın. Uygulama adı tanımlayan sırada gerçekleşen kimliği aktör benzersiz olarak tanımlayan [Service Fabric uygulaması](service-fabric-reliable-actors-platform.md#application-model) aktör dağıtıldığı.

`ActorProxy`(C#) / `ActorProxyBase`istemci tarafında (Java) sınıfı Kimliğine göre aktör bulun ve bir iletişim kanalı ile açmak için gerekli çözümlemesi gerçekleştirir. İletişim hataları ve yük devretme durumunda aktör bulmak için de deneme. Sonuç olarak, ileti teslimi, aşağıdaki özelliklere sahiptir:

* İleti teslimi en iyi çaba olur.
* Aktör aynı istemciden yinelenen iletileri alabilirsiniz.

### <a name="concurrency"></a>Eşzamanlılık
Reliable Actors çalışma zamanı aktör yöntemleri erişmek için bir basit Aç tabanlı erişim modeli sağlar. Başka bir deyişle, birden fazla iş parçacığı dilediğiniz zaman içinde aktör nesnenin kod etkin olabilir. Veri erişimi için eşitleme mekanizmaları için gerekli olduğu Aç tabanlı erişim eşzamanlı sistemleri büyük ölçüde basitleştirir. Ayrıca, sistemleri için özel hususlar her aktör örneğinin ile tek iş parçacıklı erişim yapısı tasarlanmalıdır anlamına gelir.

* Bir tek aktör örneği aynı anda birden fazla isteği işleyemiyor. Eş zamanlı istekleri işlemek üzere bekleniyorsa aktör örneği bir işleme performans sorunu neden olabilir.
* Bir dış istek aktörler birine aynı anda yapılır sırada iki aktörler arasında döngüsel bir istek varsa aktörler birbirine kilitlenme. Aktör çalışma zamanı otomatik olarak aktör çağrılarını zaman aşımına ve olası kilitlenme durumlarda kesmek için çağıran bir özel durum.

![Güvenilir aktörler iletişim][3]

#### <a name="turn-based-access"></a>Bırakma tabanlı erişim
Aktör yöntemi isteğine yanıt olarak bir diğer aktörler veya istemciler tam olarak yürütülmesini veya tam olarak yürütülmesini bir dönüş oluşur bir [Zamanlayıcı/anımsatıcı](service-fabric-reliable-actors-timers-reminders.md) geri çağırma. Bu yöntem ve geri aramalar zaman uyumsuz olmasına karşın, aktörler çalışma zamanı bunları Interleave değil. Yeni bir dönüş izin verilmeden önce bir dönüş tam olarak tamamlanmış olmalıdır. Diğer bir deyişle, şu anda yürütülen bir aktör yöntemi veya Zamanlayıcı/anımsatıcı geri bir yöntem için yeni bir çağrı önce tam olarak tamamlanmış olmalıdır veya geri çağırma izin verilir. Bir yöntem veya geri çağırma yürütme yönteminden döndürülen veya geri çağırma ve yöntem veya geri çağırma tarafından döndürülen görev tamamlandı tamamlandı olarak kabul edilir. Bu o Aç tabanlı eşzamanlılık bile farklı yöntemler, zamanlayıcılar ve geri aramalar arasında dikkate değer vurgulayan olur.

Aktör çalışma zamanı bir dönüş başlangıcını ve Aç sonunda kilidin açılması aktör başına kilit alınırken tarafından Aç tabanlı eşzamanlılık zorlar. Bu nedenle, dönüş tabanlı eşzamanlılık aktör başına temelinde arasında değil aktörler zorlanır. Aynı anda aktör yöntemleri ve Zamanlayıcı/anımsatıcı geri aramalar adına farklı aktörler yürütebilir.

Aşağıdaki örnek, yukarıdaki kavramları göstermektedir. İki zaman uyumsuz yöntemleri uygulayan bir aktör türü göz önünde bulundurun (örneğin, *Method1* ve *Method2*), bir Zamanlayıcı ve bir anımsatıcı. Aşağıdaki diyagramda iki aktörler adına bu yöntemlere ve geri aramalar yürütülmesi için bir zaman çizelgesi örneği gösterilmektedir (*ActorId1* ve *ActorId2*) bu aktör türüne ait.

![Güvenilir aktörler çalışma zamanı Aç tabanlı eşzamanlılık ve erişim][1]

Bu diyagramda, bu kuralları aşağıdaki gibidir:

* Her dikey çizgi belirli aktör adına bir yöntem ya da bir geri çağırma yürütme mantıksal akışı gösterilmektedir.
* Dikey her satırda işaretlenmiş olaylar kronolojik sırada eskiler yeni olayların ile oluşur.
* Farklı renk farklı aktörler için karşılık gelen zaman çizelgeleri için kullanılır.
* Vurgulama aktör başına kilit bir yöntem veya geri çağırma adına tutulduğu süreyi belirtmek için kullanılır.

Dikkate alınması gereken bazı önemli noktalar:

* Sırada *Method1* adına yürütüyor *ActorId2* istemci isteğine yanıt olarak *xyz789*, başka bir istemci isteği (*abc123*) ulaşan de gerektirir *Method1* tarafından yürütülecek *ActorId2*. Ancak, ikinci yürütülmesi *Method1* önceki yürütme tamamlanana kadar başlamaz. Benzer şekilde, bir anımsatıcı kayıtlı tarafından *ActorId2* sırasında ateşlenir *Method1* istemci isteğine yanıt olarak yürütülmekte olan *xyz789*. Yalnızca her iki yürütmeleri sonra anımsatıcı geri çağırma yürütülen *Method1* tamamlandığından. Tüm bu olduğu için zorlanan Aç tabanlı eşzamanlılık nedeniyle *ActorId2*.
* Benzer şekilde, bırakma tabanlı eşzamanlılık ayrıca için zorunlu *ActorId1*, yürütme işlemi tarafından gösterildiği gibi *Method1*, *Method2*ve adınaZamanlayıcıgeri *ActorId1* seri biçimde olmuyor.
* Yürütülmesi *Method1* adına *ActorId1* adına yürütülmesinin ile çakışıyor *ActorId2*. Bırakma tabanlı eşzamanlılık yalnızca bir aktör içinde arasında değil aktörler zorlanır olmasıdır.
* Bazı yöntemi/geri çağırma yürütmeleri `Task`(C#) / `CompletableFuture`(Java) tarafından yöntemi/geri araması bitirdiğinde metodu döndükten sonra döndürdü. Bazı bazılarında zaman uyumsuz işlem zaten yöntemi/geri çağırma döndürür zamanına göre bitirdi. Her iki durumda da, yalnızca hem yöntemi/geri döndürür ve zaman uyumsuz işlemi tamamlandıktan sonra aktör başına kilit yayımlanır.

#### <a name="reentrancy"></a>Yeniden giriş
Aktör çalışma zamanı yeniden giriş varsayılan olarak izin verir. Bu olması durumunda bir aktör yöntemi anlamına gelir *aktör A* bir yöntemi çağırır *aktör B*, sırayla çağıran başka bir yöntem üzerinde *aktör A*, yöntem çalışmasına izin verilip. Aynı mantıksal çağrı zincirine bağlam parçası olduğundan bu değildir. Tüm Zamanlayıcı ve anımsatıcı çağrıları yeni mantıksal çağrısı bağlamla başlatın. Bkz: [Reliable Actors yeniden giriş](service-fabric-reliable-actors-reentrancy.md) daha fazla ayrıntı için.

#### <a name="scope-of-concurrency-guarantees"></a>Eşzamanlılık garanti kapsamı
Aktör çalışma zamanı bu yöntemleri çağırma olduğu denetlediği durumlarda bu eşzamanlılık garantileri sağlar. Örneğin, Zamanlayıcı ve anımsatıcı geri aramalar yanı sıra, bir istemci isteğine yanıt olarak gerçekleştirilen yöntem çağrılarına için bu garantileri sağlar. Ancak, aktör kodu doğrudan aktörler çalışma zamanı tarafından sağlanan mekanizmaları dışında bu yöntemleri çağırırsa, çalışma zamanı eşzamanlılık garanti sağlayamaz. Örneğin, aktör yöntemler tarafından döndürülen görev ile ilişkili değil bazı görev bağlamında yöntemi çağrıldıysa, çalışma zamanı eşzamanlılık garanti sağlayamaz. Aktör, kendi oluşturduğu akıştan yöntemi çağrıldıysa, çalışma zamanı da eşzamanlılık garanti sağlayamaz. Bu nedenle, arka plan işlemleri gerçekleştirmek için aktörler kullanması gereken [aktör zamanlayıcılar ve aktör anımsatıcıları](service-fabric-reliable-actors-timers-reminders.md) Aç tabanlı eşzamanlılık saygılı olun.

## <a name="next-steps"></a>Sonraki adımlar
* İlk Reliable Actors hizmetiniz oluşturarak başlayın:
   * [.NET üzerinde Reliable Actors ile çalışmaya başlama](service-fabric-reliable-actors-get-started.md)
   * [Üzerinde Java Reliable Actors ile çalışmaya başlama](service-fabric-reliable-actors-get-started-java.md)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-introduction/concurrency.png
[2]: ./media/service-fabric-reliable-actors-introduction/distribution.png
[3]: ./media/service-fabric-reliable-actors-introduction/actor-communication.png
