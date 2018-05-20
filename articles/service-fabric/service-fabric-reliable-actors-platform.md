---
title: Service Fabric üzerinde Reliable Actors | Microsoft Docs
description: Reliable Actors Reliable Services üzerinde katmanlanmış ve Service Fabric platformundan özellikleriyle nasıl açıklanmaktadır.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: amanbha
ms.assetid: 45839a7f-0536-46f1-ae2b-8ba3556407fb
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 3/9/2018
ms.author: vturecek
ms.openlocfilehash: b2369f9468c54f10d01203841b6d7ba44b7ba2de
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="how-reliable-actors-use-the-service-fabric-platform"></a>Reliable Actors hizmeti tarafından Service Fabric platformunun kullanımı
Bu makalede, Azure Service Fabric platformda Reliable Actors nasıl çalıştığı açıklanmaktadır. Durum bilgisi olan güvenilir bir hizmet uygulaması içinde barındırılan bir çerçeve çalıştırmanız reliable Actors adlı *aktör hizmeti*. Aktör hizmeti yaşam döngüsü ve ileti gönderme, aktörleri yönetmek gerekli tüm bileşenleri içerir:

* Aktör çalışma zamanı yaşam döngüsü, atık toplama yönetir ve tek iş parçacıklı erişimini zorunlu kılar.
* Aktör hizmeti remoting dinleyicisi aktörler için uzaktan erişim çağrılarını kabul eder ve bunları uygun aktör örneğine yönlendirmek için bir dağıtıcı gönderir.
* Aktör durumu sağlayıcısı (örneğin, güvenilir koleksiyonları durumu sağlayıcısı) durum sağlayıcılarının sarmalar ve aktör durum yönetimi için bir bağdaştırıcı sağlar.

Bu bileşenlerin birlikte güvenilir aktör framework oluştururlar.

## <a name="service-layering"></a>Hizmet katmanlarını
Aktör hizmeti güvenilir bir hizmet olduğundan tüm [uygulama modeli](service-fabric-application-model.md), yaşam döngüsü, [paketleme](service-fabric-package-apps.md), [dağıtım](service-fabric-deploy-remove-applications.md), yükseltme ve ölçeklendirme Reliable Services kavramlarını aktör Hizmetleri aynı şekilde geçerlidir.

![Aktör hizmeti katmanlama][1]

Önceki diyagramda Service Fabric uygulama çerçeveleri ve kullanıcı kodu arasındaki ilişkiyi gösterir. Mavi öğeleri Reliable Services uygulama çerçevesi temsil eder, turuncu güvenilir aktör framework ve yeşil kullanıcı kodu temsil eder.

Güvenilir Hizmetleri'nde hizmetinizi devralır `StatefulService` sınıfı. Bu sınıf kendisini türetilmiş, `StatefulServiceBase` (veya `StatelessService` durum bilgisi olmayan hizmetler için). Reliable Actors içinde aktör hizmeti kullanın. Aktör hizmeti farklı bir uygulamasıdır `StatefulServiceBase` , aktörler çalıştırdığı aktör deseni uygulayan sınıf. Aktör hizmeti uygulaması yalnızca olduğundan `StatefulServiceBase`, türetilen kendi hizmet yazabilirsiniz `ActorService` ve hizmet düzeyi özellikleri devralırken yaptığınız şekilde uygulamak `StatefulService`, gibi:

* Hizmet yedekleme ve geri yükleme.
* İşlevselliği tüm aktörler, örneğin, bir devre kesici paylaşılan.
* Aktör hizmeti ve tek tek her aktör uzak yordam çağrıları.

> [!NOTE]
> Durum bilgisi olan hizmetler Java/Linux şu anda desteklenmemektedir.

Daha fazla bilgi için bkz: [aktör hizmetinizi hizmet düzeyi özelliklerini uygulama](service-fabric-reliable-actors-using.md).

## <a name="application-model"></a>Uygulama modeli
Uygulama modeli aynı olacak şekilde aktör Reliable Services hizmetleridir. Ancak, aktör framework derleme araçları uygulama modeli dosyaların bazıları sizin için oluşturur.

### <a name="service-manifest"></a>Hizmet bildirimi
Aktör framework derleme araçları aktör hizmetinizin ServiceManifest.xml dosyasının içeriği otomatik olarak oluşturur. Bu dosya içerir:

* Aktör hizmeti türü. Tür adı aktör'ın Proje adına göre oluşturulur. Aktör Kalıcılık öznitelikte bağlı olarak, HasPersistedState bayrağı da buna uygun olarak ayarlanır.
* Kod paketi.
* Yapılandırma paketi.
* Kaynaklar ve uç noktaları.

### <a name="application-manifest"></a>Uygulama bildirimi
Aktör framework derleme araçları varsayılan hizmet tanımı aktör hizmetiniz için otomatik olarak oluşturun. Derleme araçları varsayılan hizmet özelliklerini doldurun:

* Çoğaltma kümesi sayısı, aktör Kalıcılık özniteliği tarafından belirlenir. Aktör Kalıcılık öznitelikte her değiştiğinde çoğaltma kümesi sayısı varsayılan hizmet tanımında buna uygun olarak sıfırlanır.
* Bölüm düzeni ve aralığı Tekdüzen Int64 tam Int64 anahtar aralığıyla ayarlanır.

## <a name="service-fabric-partition-concepts-for-actors"></a>Aktörler için Service Fabric bölümü kavramlar
Aktör Hizmetleri bölümlenmiş durum bilgisi olan hizmetleridir. Aktör hizmeti her bölüm aktörler kümesini içerir. Hizmet bölümlerini Service Fabric birden çok düğümler üzerinden otomatik olarak dağıtılır. Aktör örnekleri sonuç olarak dağıtılır.

![Bölümleme aktör ve dağıtım][5]

Güvenilir hizmetler farklı bir bölüm düzeni ve bölüm anahtarı aralıkları ile oluşturulabilir. Aktör hizmeti aktörler bölümlere eşlemek için tam Int64 anahtar aralığıyla Int64 bölümleme düzenini kullanır.

### <a name="actor-id"></a>Aktör kimliği
Hizmetinde oluşturulan her aktör tarafından temsil edilen ilişkili benzersiz bir Kimliğe sahip `ActorId` sınıfı. `ActorId` rastgele kimlikleri oluşturarak service bölümler aktörler Tekdüzen dağıtım için kullanılabilecek bir donuk kimliği değeridir:

```csharp
ActorProxy.Create<IMyActor>(ActorId.CreateRandom());
```
```Java
ActorProxyBase.create<MyActor>(MyActor.class, ActorId.newId());
```


Her `ActorId` bir Int64 karma haline getirilir. Aktör hizmeti bir Int64 bölümleme düzeni tam Int64 anahtar aralığıyla kullanmalısınız nedeni budur. Ancak, özel kimlik değerleri için kullanılabilecek bir `ActorID`GUID/UUID, dizeleri ve Int64s dahil olmak üzere.

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

GUID/UUID ve dizeleri kullanırken, değerlerin bir Int64 karma hale getirilir. Ancak, ne zaman, açıkça sağlayan bir Int64 bir `ActorId`, Int64 daha fazla karma olmadan doğrudan bir bölüm eşler. Aktör yerleştirilir hangi bölümünü denetlemek için bu tekniği kullanabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar
* [Aktör durum yönetimi](service-fabric-reliable-actors-state-management.md)
* [Aktör yaşam döngüsü ve çöp toplama](service-fabric-reliable-actors-lifecycle.md)
* [Aktör API başvuru belgeleri](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [.NET örnek kod](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java örnek kod](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png
