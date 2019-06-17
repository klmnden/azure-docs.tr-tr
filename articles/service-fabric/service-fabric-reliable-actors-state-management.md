---
title: Reliable Actors durum yönetimi | Microsoft Docs
description: Nasıl Reliable Actors durum yönetilen kalıcı ve yüksek kullanılabilirlik için çoğaltılır açıklar.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: 37cf466a-5293-44c0-a4e0-037e5d292214
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 65dd47ab21ca4b1c50e0f17b73e7bc4eae8a96e8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60725746"
---
# <a name="reliable-actors-state-management"></a>Reliable Actors durum yönetimi
Reliable Actors hem mantıksal hem de durum kapsülleyebilir tek iş parçacıklı nesneleridir. Aktörler Reliable Services üzerinde çalıştığından, bunların durumu güvenilir bir şekilde aynı Kalıcılık ve çoğaltma mekanizması kullanarak koruyabilir. Bu şekilde, etkinleştirme veya Çöp toplamadan sonra kaynak Dengeleme veya yükseltme işlemleri nedeniyle, bir kümedeki düğümler arasında geçici olarak taşındıklarında bağlı bir hatadan sonra durumlarını aktörler kaybetmeyin.

## <a name="state-persistence-and-replication"></a>Durum kalıcılığını ve çoğaltma
Tüm Reliable Actors değerlendirilir *durum bilgisi olan* her aktör örneği için benzersiz bir kimliği eşlediğinden Başka bir deyişle, aynı kimliği için yinelenen çağrıları aynı aktör örneğine yönlendirilir. Durum bilgisi olmayan bir sistemde Bunun aksine, istemci çağrıları her zaman aynı sunucuya yönlendirilecek garanti edilmez. Bu nedenle aktör Hizmetleri durum bilgisi olan hizmetler her zaman açıktır.

Durum bilgisi olan aktör değerlendirilir olsa da, bu durum güvenilir bir şekilde depolamalısınız anlamına gelmez. Aktör durumu kalıcılığını düzeyini seçebilir ve çoğaltma verilerine dayalı olarak depolama gereksinimlerini:

* **Kalıcı durum**: Durumu kalıcı hale disk ve en az üç kopyaya çoğaltılır. Kalıcı durum en sağlam durum depolama, durumu burada tam küme azaltsa kalıcı yapılabilir seçeneğidir.
* **Geçici bir durum**: Durum üç veya daha fazla kopyaya çoğaltılır ve yalnızca bellekte tutulur. Geçici bir durum düğüm hatası ve aktör hatası karşı ve yükseltmeleri ve kaynak Dengeleme sırasında esneklik sağlar. Ancak, durumu kalıcı disk. Tüm çoğaltmalar aynı anda kaybolursa, bu nedenle durumu de kaybolur.
* **Kalıcı durum**: Durumu olmayan çoğaltılmış veya diske yazılan yalnızca güvenilir bir şekilde durumunu korumak üzere gerekmeyen aktörler için kullanın.

Her düzeyde Kalıcılık yalnızca farklı olan *durumu sağlayıcısı* ve *çoğaltma* hizmetinizin yapılandırması. Durumu olup olmadığına yazılır disk durumu sağlayıcısı--durumunu depolayan bir güvenilir hizmet bileşeni bağlıdır. Çoğaltma hizmeti kaç yineleme ile dağıtıldığı bağlıdır. Reliable Services gibi ile hem durumu sağlayıcısı hem de yineleme sayısı kolayca el ile ayarlanabilir. Aktör çerçevesinde bir öznitelik, bir aktör üzerinde kullanılan, varsayılan durumu sağlayıcısı otomatik olarak seçer ve bu üç Kalıcılık ayarlarından birini elde etmek için yineleme sayısı için ayarları otomatik olarak oluşturduğu sağlar. StatePersistence öznitelik türetilmiş sınıf tarafından devralınan değil, her aktör türü StatePersistence düzeyiyle sağlamanız gerekir.

### <a name="persisted-state"></a>Kalıcı durum
```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl  extends FabricActor implements MyActor
{
}
```  
Bu ayar, verileri diskte depolar ve otomatik olarak hizmet yineleme sayısı 3'e ayarlayan bir durumu sağlayıcısı kullanır.

### <a name="volatile-state"></a>Volatile durumu
```csharp
[StatePersistence(StatePersistence.Volatile)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Volatile)
class MyActorImpl extends FabricActor implements MyActor
{
}
```
Bu ayar, bir içindeki bellek-yalnızca durumu sağlayıcısı kullanır ve yineleme sayısı 3'e ayarlar.

### <a name="no-persisted-state"></a>Kalıcı durum yok
```csharp
[StatePersistence(StatePersistence.None)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.None)
class MyActorImpl extends FabricActor implements MyActor
{
}
```
Bu ayar, bir içindeki bellek-yalnızca durumu sağlayıcısı kullanır ve yineleme sayısı 1'e ayarlar.

### <a name="defaults-and-generated-settings"></a>Varsayılan değerleri ve oluşturulan ayarları
Kullanırken `StatePersistence` öznitelik, bir durum sağlayıcısı otomatik olarak seçilir, çalışma zamanında aktör hizmeti başladığında. Yineleme sayısı, ancak derleme zamanında Visual Studio aktör yapı araçları tarafından ayarlanır. Derleme araçları otomatik olarak oluşturmak bir *varsayılan hizmeti* ApplicationManifest.xml aktör hizmeti için. Parametreler için oluşturulan **en az çoğaltma kümesi boyutu** ve **hedef çoğaltma kümesi boyutu**.

Bu parametreleri el ile değiştirebilirsiniz. Ancak her zaman `StatePersistence` özniteliği değiştiğinde, seçilen varsayılan çoğaltma kümesi boyutu değerlerine parametrelerinin `StatePersistence` özniteliği, önceki tüm değerleri geçersiz kılma. Diğer bir deyişle, ServiceManifest.xml ayarlanan değerler *yalnızca* değiştirdiğinizde oluşturma zamanında geçersiz kılınan `StatePersistence` öznitelik değeri.

```xml
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application12Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="MyActorService_PartitionCount" DefaultValue="10" />
      <Parameter Name="MyActorService_MinReplicaSetSize" DefaultValue="3" />
      <Parameter Name="MyActorService_TargetReplicaSetSize" DefaultValue="3" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyActorPkg" ServiceManifestVersion="1.0.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MyActorService" GeneratedIdRef="77d965dc-85fb-488c-bd06-c6c1fe29d593|Persisted">
         <StatefulService ServiceTypeName="MyActorServiceType" TargetReplicaSetSize="[MyActorService_TargetReplicaSetSize]" MinReplicaSetSize="[MyActorService_MinReplicaSetSize]">
            <UniformInt64Partition PartitionCount="[MyActorService_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
         </StatefulService>
      </Service>
   </DefaultServices>
</ApplicationManifest>
```

## <a name="state-manager"></a>Durum Yöneticisi
Kendi durum Yöneticisi her aktör örneği var: güvenilir bir şekilde depolayan bir sözlüğü benzeri veri yapısı anahtar/değer çiftleri. Durum Yöneticisi durum sağlayıcısı çevresinde bir sarmalayıcı ' dir. Kalıcı ayar ne olursa olsun kullanıldığı verileri depolamak için kullanabilirsiniz. Çalışan bir aktör hizmeti volatile (içindeki bellek-yalnızca) durumu ayarından verileri korurken sıralı yükseltme aracılığıyla bir kalıcı durum ayarı için değiştirilebilir hiçbir garanti sağlamaz. Ancak, çalışan bir hizmet için çoğaltma sayısını değiştirmek mümkündür.

Durum Yöneticisi anahtarları dize olmalıdır. Değerleri geneldir ve herhangi bir tür olabilir özel türler dahil. Durum Yöneticisi'nde depolanan değerleri, diğer düğümlerle ağ üzerinden çoğaltma sırasında iletilebilir ve, bir aktörün durumu Kalıcılık ayarlarına bağlı olarak diske yazılan çünkü veri sözleşme seri hale getirilebilir olması gerekir.

Durum Yöneticisi durum, güvenilir bir sözlükte bulunan benzer yönetmek için ortak sözlüğü yöntemleri sunar.

Aktör durumunu yönetme örnekleri için okuma [erişimi kaydedin ve Reliable Actors durum Kaldır](service-fabric-reliable-actors-access-save-remove-state.md).

## <a name="best-practices"></a>En iyi uygulamalar
Bazı önerilen yöntemler ve sorun giderme ipuçları, aktör durumunu yönetmek için aşağıda verilmiştir.

### <a name="make-the-actor-state-as-granular-as-possible"></a>Aktör durumu mümkün olduğunca ayrıntılı olun
Bu, performans ve kaynak kullanımı, uygulamanız için önemlidir. Herhangi yazma / "durumu bir aktör adında" için bir güncelleştirme olduğunda, söz konusu "adlı duruma" karşılık gelen tam değeri serileştirilmiş ve ağ üzerinden ikincil çoğaltmalara gönderilen.  İkincil veritabanı yerel diske ve birincil çoğaltma yeniden yanıt yazın. Birincil, ikincil çoğaltmalar çekirdeği onayları aldığında, durumu, yerel bir diske yazar. Örneğin, değeri 20 üyeleri ve Boyut 1 MB olan bir sınıf olduğunu varsayın. Aşağıdakilerden biri olan sınıf üyeleri yalnızca değiştirilmiş olsa bile, 1 KB, 1 MB'ın tamamını için serileştirme ve ağ ve disk yazma maliyetini ödeme'kurmak son boyutu. Değer bir koleksiyon (örneğin, bir liste, dizi veya sözlük) ise bile onun üyeleri birini değiştirin benzer şekilde, maliyet tam koleksiyon için ödeme yaparsınız. Aktör sınıfı StateManager gibi bir sözlük arabirimidir. Her zaman bu sözlük üzerine aktör durumunu temsil eden veri yapısı modelini oluşturması gerekir.
 
### <a name="correctly-manage-the-actors-life-cycle"></a>Aktörün yaşam döngüsü yönetemez
Her bölümde bir aktör hizmetinin durumu boyutunu yönetme hakkında açık ilke olmalıdır. Aktör hizmetinizi sabit sayıda aktörler ve mümkün olduğunca bunları yeniden gerekir. Yeni aktör sürekli olarak oluşturursanız, kendi çalışmalarına tamamladıktan sonra silmeniz gerekir. Aktör çerçevesinde var. her aktör hakkında bazı meta verileri depolar. Bir aktör durumunu silme, aktör hakkındaki meta verileri kaldırmaz. Aktör silmeniz gerekir (bkz [aktörleri ve durumlarını silme](service-fabric-reliable-actors-lifecycle.md#manually-deleting-actors-and-their-state)) tüm bilgileri kaldırmak için ilgili bu sistemde depolanır. Ek bir denetim actor hizmetinin sorgu (bkz [aktörler numaralandırma](service-fabric-reliable-actors-enumerate.md)) zaman aktörler sayısı beklenen aralıkta olduğundan emin olmak için.
 
Şimdiye kadar veritabanı dosya boyutu bir aktör hizmetinin beklenen boyutunun ötesinde genişliyorsa görürseniz, önceki yönergeleri izlediğinizden emin olun. Bu yönergeleri okuduğundan ve hala veritabanı dosya boyutu sorunlarından olan durumunda [bir destek bileti açın](service-fabric-support.md) Yardım almak için ürün ekibi ile.

## <a name="next-steps"></a>Sonraki adımlar

Reliable Actors içinde depolanan durumu serileştirilmiş önce diske yazılan ve yüksek kullanılabilirlik için çoğaltılır. Daha fazla bilgi edinin [aktör türü serileştirme](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).

Ardından, daha fazla bilgi edinin [aktör tanılama ve performans izleme](service-fabric-reliable-actors-diagnostics.md).
