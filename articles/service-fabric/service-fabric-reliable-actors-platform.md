---
title: Service Fabric üzerinde Reliable Actors | Microsoft Docs
description: Reliable Actors Reliable Services üzerinde katmanlanmış ve Service Fabric platform özellikleriyle nasıl açıklanmaktadır.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: amanbha
ms.assetid: 45839a7f-0536-46f1-ae2b-8ba3556407fb
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 3/9/2018
ms.author: vturecek
ms.openlocfilehash: bc7569c9f230abb7677a8df9fc0cc0268e57296f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60725932"
---
# <a name="how-reliable-actors-use-the-service-fabric-platform"></a>Reliable Actors hizmeti tarafından Service Fabric platformunun kullanımı
Bu makalede, Reliable Actors Azure Service Fabric platformunda nasıl çalıştığı açıklanmaktadır. Reliable Actors hizmetini durum bilgisi olan güvenilir hizmet uygulaması içinde barındırılan bir çerçeve çalıştırmak adlı *actor hizmetinin*. Actor hizmetinin yaşam döngüsü ve ileti gönderme, aktörler için yönetmek gerekli tüm bileşenleri içerir:

* Actor çalışma zamanına yaşam döngüsü, çöp toplama, yönetir ve tek iş parçacıklı erişimini zorunlu kılar.
* Bir aktör hizmeti uzaktan iletişim dinleyicisi aktörler için uzaktan erişim çağrılarını kabul eder ve bunları uygun aktör örneğine yönlendirmek için bir dağıtıcı gönderir.
* Aktör durumu sağlayıcısı (örneğin, güvenilir koleksiyonlar durumu sağlayıcısı) durum sağlayıcılarının sarmalar ve aktör durumu yönetimi için bir bağdaştırıcı sağlar.

Bu bileşenler birlikte güvenilir aktör çerçevesinde oluşturur.

## <a name="service-layering"></a>Hizmet katmanlarını
Aktör hizmeti, güvenilir bir hizmet olduğundan tüm [uygulama modeli](service-fabric-application-model.md), yaşam döngüsü, [paketleme](service-fabric-package-apps.md), [dağıtım](service-fabric-deploy-remove-applications.md), yükseltme ve güvenilir kavramlarını ölçeklendirme Hizmetleri aktör Hizmetleri aynı şekilde geçerlidir.

![Aktör hizmeti katmanlama][1]

Önceki şemada Service Fabric uygulama çerçeveleri ve kullanıcı kodu arasındaki ilişkiyi gösterir. Reliable Services uygulaması çerçevesi mavi öğeleri temsil, turuncu güvenilir aktör çerçevesinde temsil eder ve yeşil kullanıcı kodunu temsil eder.

Reliable Services içinde hizmetinizi devralır `StatefulService` sınıfı. Bu sınıf kendi türetilmiş, `StatefulServiceBase` (veya `StatelessService` durum bilgisi olmayan hizmetler için). Reliable Actors actor hizmetinin kullanın. Actor hizmetinin farklı bir uygulamasıdır `StatefulServiceBase` , aktör çalıştırdığı gerçekleştiren deseni uygulayan sınıf. Aktör hizmeti yalnızca uygulaması olduğundan `StatefulServiceBase`, türetilen kendi hizmet yazabilirsiniz `ActorService` ve hizmet düzeyi özellikleri devralınırken olduğu gibi uygulamak `StatefulService`, gibi:

* Hizmet yedekleme ve geri yükleme.
* İşlevi, tüm aktör, örneğin, devre kesici paylaşılan.
* Uzak yordam çağrıları tek tek her aktör ve aktör hizmeti üzerinde.

Daha fazla bilgi için [aktör hizmetinizin hizmet düzeyi özelliklerini uygulama](service-fabric-reliable-actors-using.md).

## <a name="application-model"></a>Uygulama modeli
Aynı uygulama modelini, bu nedenle aktör Reliable Services hizmetleridir. Ancak, aktör framework derleme araçları bazı uygulama modeli dosyaları sizin için oluşturur.

### <a name="service-manifest"></a>Hizmet bildirimi
Aktör framework derleme araçları, aktör hizmetinizin ServiceManifest.xml dosyasının içeriğini otomatik olarak oluşturur. Bu dosya şunları içerir:

* Aktör hizmeti türü. Tür adı'nın aktörünüz proje adına göre oluşturulur. Aktörünüz Kalıcılık özniteliğini bağlı olarak, HasPersistedState'bayrağı da buna uygun olarak ayarlanır.
* Kod paketi.
* Yapılandırma paketi.
* Kaynaklar ve uç noktaları.

### <a name="application-manifest"></a>Uygulama bildirimi
Aktör framework derleme araçları, varsayılan hizmet tanımı aktör hizmeti için otomatik olarak oluşturun. Derleme araçları, varsayılan hizmet özellikleri doldurun:

* Çoğaltma kümesi sayısı aktörünüz Kalıcılık özniteliği tarafından belirlenir. Aktörünüz Kalıcılık özniteliğini her değiştirildiğinde varsayılan hizmet tanımındaki çoğaltma kümesi sayısı buna uygun olarak sıfırlanır.
* Bölüm düzeni ve aralığı için Tekdüzen Int64 tam Int64 anahtar aralığı ile ayarlanır.

## <a name="service-fabric-partition-concepts-for-actors"></a>Service Fabric aktörleri için bölüm kavramları
Aktör, bölümlenmiş durum bilgisi olan hizmetler hizmetleridir. Her bölüm bir aktör hizmetinin aktör kümesini içerir. Hizmeti bölümleri, Service fabric'te birden çok düğüm üzerinden otomatik olarak dağıtılır. Aktör örnekleri sonuç olarak dağıtılır.

![Aktör bölümleme ve dağıtım][5]

Reliable Services, farklı bir bölüm düzeni ve bölüm anahtar aralığı ile oluşturulabilir. Actor hizmetinin aktör bölümlere eşlemek için tam Int64 anahtar aralığı ile Int64 bölümleme şeması kullanır.

### <a name="actor-id"></a>Aktör Kimliği
Hizmette oluşturulan her aktör, tarafından temsil edilen ilişkili benzersiz bir kimlik vardır `ActorId` sınıfı. `ActorId` rastgele kimlikleri oluşturarak hizmeti bölümler arasında Tekdüzen aktörler dağıtım için kullanılabilecek bir donuk kimliği değerdir:

```csharp
ActorProxy.Create<IMyActor>(ActorId.CreateRandom());
```
```Java
ActorProxyBase.create<MyActor>(MyActor.class, ActorId.newId());
```


Her `ActorId` bir Int64 için karma uygulanır. Actor hizmetinin tam Int64 anahtar aralığı ile bir Int64 bölümleme düzenini kullanmalıdır nedeni budur. Ancak, özel kod değerleri için kullanılabilecek bir `ActorID`GUID/UUID, dizeleri ve Int64s dahil.

```csharp
ActorProxy.Create<IMyActor>(new ActorId(Guid.NewGuid()));
ActorProxy.Create<IMyActor>(new ActorId("myActorId"));
ActorProxy.Create<IMyActor>(new ActorId(1234));
```
```Java
ActorProxyBase.create(MyActor.class, new ActorId(UUID.randomUUID()));
ActorProxyBase.create(MyActor.class, new ActorId("myActorId"));
ActorProxyBase.create(MyActor.class, new ActorId(1234));
```

GUID/UUID ve dizeleri kullanırken, değerlerin bir Int64 için karma hale getirilir. Ancak, ne zaman, açıkça sağlamak için bir Int64 bir `ActorId`, Int64 daha fazla karma olmadan doğrudan bir bölüme eşler. Aktörleri yerleştirilir hangi bölümünün denetlemek için bu tekniği kullanabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar
* [Aktör durumu yönetimi](service-fabric-reliable-actors-state-management.md)
* [Aktör yaşam döngüsü ve atık toplama](service-fabric-reliable-actors-lifecycle.md)
* [Aktörler API başvuru belgeleri](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.actors?redirectedfrom=MSDN&view=azure-dotnet)
* [.NET örnek kodu](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java örnek kodu](https://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png
