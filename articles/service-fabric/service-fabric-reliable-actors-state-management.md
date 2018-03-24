---
title: Güvenilir aktörler durum yönetimi | Microsoft Docs
description: Nasıl Reliable Actors durumu yönetilen kalıcı ve yüksek kullanılabilirlik için çoğaltılan açıklar.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: ''
ms.assetid: 37cf466a-5293-44c0-a4e0-037e5d292214
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: d5d38e7fa80db3484c397d9840bbc6092e4f18bb
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="reliable-actors-state-management"></a>Güvenilir aktörler durum yönetimi
Güvenilir aktörler mantığı ve durumu sarmalayabilen tek iş parçacıklı nesneleridir. Aktör Reliable Services üzerinde çalıştığından, bunlar durum güvenilir bir şekilde aynı kalıcılığını ve çoğaltma mekanizmalarını kullanarak koruyabilirsiniz. Bu şekilde çöp toplamadan sonra veya kaynak Dengeleme ya da yükseltme nedeniyle kümedeki düğümler arasında taşındıklarında yeniden etkinleştirme sırasında hatadan sonra durumlarına aktörler kaybetmeyin.

## <a name="state-persistence-and-replication"></a>Durum kalıcılığını ve çoğaltma
Tüm Reliable Actors kabul *durum bilgisi olan* her aktör örneği için benzersiz bir kimliği eşlendiğinden Bu, aynı aktör kimliği için yinelenen çağrıları aynı aktör örneğini yönlendirilir anlamına gelir. Durum bilgisi olmayan bir sistemde Bunun aksine, istemci çağrılarını her zaman aynı sunucuya yönlendirilecek garanti edilmez. Bu nedenle, aktör Hizmetleri zaman durum bilgisi olan hizmetleridir.

Aktörler stateful kabul edilen olsa bile, durum güvenilir bir şekilde depolamalısınız anlamına gelmez. Aktör durumunun kalıcı olmasını düzeyini seçebilirsiniz ve çoğaltma kendi veri depolama gereksinimleri dayalı:

* **Durum kalıcı**: durum kalıcı için disk ve üç veya daha fazla çoğaltmalar için çoğaltılır. Kalıcı durum en sağlam durumu depolama burada durumu tam küme kesinti devam edebilir seçenektir.
* **Volatile durumu**: durumu üç veya daha fazla çoğaltmalar için çoğaltılır ve yalnızca bellekte depolanır. Volatile durumunu düğüm hatası ve aktör hatası karşı ve yükseltmeleri ve kaynak Dengeleme sırasında esnekliği sağlar. Ancak, durumu sürdürülmez diske. Tüm çoğaltmaları aynı anda kaybolursa, bu nedenle durumu da kaybolur.
* **Kalıcı durum yok**: durum çoğaltılan değil veya diske yazılan yalnızca durumunu güvenilir bir şekilde korumak için gerek yoktur aktörler için kullanın.

Her Kalıcılık yalnızca farklı düzeyidir *durum sağlayıcısı* ve *çoğaltma* hizmetinizin yapılandırma. Durum desteklemediğini yazılır disk durumu sağlayıcısı--durumu depolar güvenilir bir hizmet bileşeni bağlıdır. Çoğaltma hizmeti kaç çoğaltmaları dağıtıldığından bağlıdır. Güvenilir hizmetleriyle gibi durum sağlayıcısı ve çoğaltma sayısı kolayca el ile ayarlanabilir. Aktör framework bir öznitelik, bir aktör üzerinde kullanılan, varsayılan durumu sağlayıcısı otomatik olarak seçer ve bu üç Kalıcılık ayarlarından birini elde etmek için yineleme sayısı için ayarları otomatik olarak oluşturur sağlar. StatePersistence özniteliği türetilmiş sınıf tarafından devralınır değil, her aktör türü StatePersistence düzeyiyle sağlamanız gerekir.

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
Bu ayar, verilerin diskte depolar ve otomatik olarak hizmeti yineleme sayısı 3'e ayarlar durumu sağlayıcısı kullanır.

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
Bu ayar bir içindeki bellek-yalnızca durum sağlayıcısı kullanır ve çoğaltma sayısı 3'e ayarlar.

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
Bu ayar bir içindeki bellek-yalnızca durum sağlayıcısı kullanır ve çoğaltma sayısı 1'e ayarlar.

### <a name="defaults-and-generated-settings"></a>Varsayılanlar ve oluşturulan ayarları
Kullanırken `StatePersistence` öznitelik, bir durum sağlayıcısı otomatik olarak seçilir, çalışma zamanında aktör hizmeti başladığında. Çoğaltma sayısı, ancak, derleme zamanında Visual Studio aktör derleme araçları tarafından ayarlanır. Derleme araçları otomatik olarak oluşturacak bir *varsayılan hizmet* ApplicationManifest.xml aktör hizmeti için. Parametreler için oluşturulan **en küçük çoğaltma kümesi boyutu** ve **hedef çoğaltma kümesi boyutu**.

Bu parametreler el ile değiştirebilirsiniz. Ancak her zaman `StatePersistence` özniteliği değiştirildiğinde, varsayılan çoğaltma kümesi boyutu değerlere seçili parametrelerinin `StatePersistence` özniteliği, önceki tüm değerleri geçersiz kılma. Diğer bir deyişle, ServiceManifest.xml içinde ayarlanan değerler *yalnızca* değiştirdiğinizde derleme zamanında geçersiz kılınmış `StatePersistence` öznitelik değeri.

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application12Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
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
Kendi durum Yöneticisi her aktör örneği vardır: depoladıktan bir sözlük benzeri veri yapısı anahtar/değer çiftleri. Durum Yöneticisi durum sağlayıcısı çevresinde bir sarmalayıcı ' dir. Bağımsız olarak Kalıcılık ayarı kullanılan verileri depolamak için kullanabilirsiniz. Çalışan bir aktör hizmeti bir volatile (içindeki bellek-yalnızca) durumu ayarı verileri korurken yükseltme aracılığıyla kalıcı durum ayarı için değiştirilebilir garanti sağlamaz. Ancak, çalışan bir hizmetiniz için yineleme sayısı değiştirmek mümkündür.

Durum Yöneticisi anahtarları dize olmalıdır. Değerler genel ve herhangi bir türü olabilir özel türleri dahil olmak üzere. Diğer düğümlere ağ üzerinden çoğaltma sırasında iletilebilir ve bağlı olarak bir aktör'ın durum Kalıcılık ayarını diske yazılan çünkü durum Yöneticisi'nde depolanan değerleri veri sözleşmesi seri hale getirilebilir olmalıdır.

Durum Yöneticisi durum, güvenilir sözlükte bulunan benzer yönetmek için ortak sözlüğü yöntemlerini gösterir.

Aktör durumunu yönetme örnekler için okuma [erişimi kaydedin ve Reliable Actors durumu kaldırmak](service-fabric-reliable-actors-access-save-remove-state.md).

## <a name="best-practices"></a>En iyi uygulamalar
Bazı önerilen yöntemler ve sorun giderme ipuçları aktör durumunu yönetmek için aşağıda verilmiştir.

### <a name="make-the-actor-state-as-granular-as-possible"></a>Aktör durumunun olarak ayrıntılı olun
Bu, performans ve kaynak kullanımını uygulamanız için önemlidir. Tüm yazma / "durumu bir aktör adında" için güncelleştirme olduğunda, "adlandırılmış durumuna" karşılık gelen tüm değer sıralanabilir ve ikincil çoğaltmaları ağ üzerinden gönderilen.  İkincil kopya yerel diske ve birincil çoğaltma Yanıtla geri yazma. Birincil bir ikincil çoğaltmaları çekirdek onayları aldığında, durum yerel diske yazar. Örneğin, değeri 20 üyeleri ve boyutu 1 MB olan bir sınıf olduğunu varsayın. Aşağıdakilerden biri olan sınıf üyeleri yalnızca değiştirilmiş olsa bile tam 1 MB için seri hale getirme ve ağ ve disk yazma maliyetini ödeme yukarı son 1 KB Boyutunda boyut. Değer bir koleksiyon (örneğin, liste, dizi veya sözlük) ise, onun üyeleri birini değiştirirseniz, bile benzer şekilde, maliyet tam koleksiyon için ücret ödersiniz. Aktör sınıfının StateManager gibi bir sözlük arabirimidir. Her zaman bu sözlük üstünde aktör durumunu temsil eden veri yapısı model.
 
### <a name="correctly-manage-the-actors-life-cycle"></a>Aktör'ın ömrü doğru yönetme
Aktör hizmeti her bölüm durumda boyutunu yönetmeyle ilgili Temizle ilke olması gerekir. Aktör hizmeti aktörler sabit sayıda ve mümkün olduğunca çok yeniden gerekir. Sürekli yeni aktörler oluşturursanız, kendi çalışma tamamladıktan sonra silmeniz gerekir. Aktör framework var. her aktör hakkında bazı meta verileri depolar. Bir aktör durumunu silinmesi bu aktör hakkındaki meta verileri kaldırmaz. Aktör silmeniz gerekir (bkz [aktörler ve durumlarına silme](service-fabric-reliable-actors-lifecycle.md#manually-deleting-actors-and-their-state)) tüm bilgileri kaldırmak için yaklaşık sistemde depolanmış. Ek bir onay aktör Hizmeti sorgu (bkz [aktörler numaralandırma](service-fabric-reliable-actors-platform.md)) arada bir numara aktörler beklenen aralıkta emin olmak için.
 
Şimdiye kadar bir aktör hizmeti veritabanı dosya boyutunu beklenen boyutu artmaktadır görürseniz, yukarıdaki yönergeleri izleyerek emin olun. Bu yönergeleri izleyerek ve hala veritabanı dosya boyutu sorunlarından olan durumunda [bir destek bileti açmanız](service-fabric-support.md) Yardım almak için ürün ekibi ile.

## <a name="next-steps"></a>Sonraki adımlar

Reliable Actors içinde depolanan durumu serileştirilmiş, diske yazılan ve yüksek kullanılabilirlik için çoğaltılmış önce. Daha fazla bilgi edinmek [aktör türü seri hale getirme](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).

Ardından, daha fazla bilgi edinmek [aktör tanılama ve performans izlemesi](service-fabric-reliable-actors-diagnostics.md).
