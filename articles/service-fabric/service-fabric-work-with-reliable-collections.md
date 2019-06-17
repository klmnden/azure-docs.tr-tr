---
title: Güvenilir koleksiyonlar ile çalışma | Microsoft Docs
description: Güvenilir koleksiyonlar ile çalışma için en iyi uygulamaları öğrenin.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: 39e0cd6b-32c4-4b97-bbcf-33dad93dcad1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/22/2019
ms.author: aljo
ms.openlocfilehash: bb99e5984f91edb0cf40f3bdc485624b9ec59833
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60506748"
---
# <a name="working-with-reliable-collections"></a>Güvenilir Koleksiyonlar ile çalışma
Service Fabric, güvenilir koleksiyonlar aracılığıyla .NET geliştiricileri için kullanılabilen bir durum bilgisi olan programlama modeli sunar. Özellikle, Service Fabric, güvenilir bir sözlük ve güvenilir bir kuyruk sınıfları sağlar. Bu sınıfların kullandığınızda, durumunuzu (ölçeklenebilirlik) bölümlenmiş çoğaltılan (kullanılabilirlik için) ve bir bölüm (için ACID semantiği) içinde hareket gerçekleşti. Şimdi bir güvenilir bir sözlük nesnesi tipik bir kullanım sırasında bakın ve hangi gerçekten başardığını görün.

```csharp
try
{
   // Create a new Transaction object for this partition
   using (ITransaction tx = base.StateManager.CreateTransaction())
   {
      // AddAsync takes key's write lock; if >4 secs, TimeoutException
      // Key & value put in temp dictionary (read your own writes),
      // serialized, redo/undo record is logged & sent to secondary replicas
      await m_dic.AddAsync(tx, key, value, cancellationToken);

      // CommitAsync sends Commit record to log & secondary replicas
      // After quorum responds, all locks released
      await tx.CommitAsync();
   }
   // If CommitAsync isn't called, Dispose sends Abort
   // record to log & all locks released
}
catch (TimeoutException)
{
   // choose how to handle the situation where you couldn't get a lock on the file because it was 
   // already in use. You might delay and retry the operation
}
```

Güvenilir bir sözlük nesnelerine (hariç, geri alınamaz ClearAsync,), tüm işlemleri ITransaction nesneyi gerektirir. Bu nesne ile ilgili ve güvenilir bir sözlük ve/veya güvenilir yapmaya çalıştığınız tüm değişiklikleri tek bir bölüm içindeki nesneleri kuyruğa alın. Bir ITransaction edindiğiniz bölüm çağırarak kullanıcının StateManager'ın CreateTransaction yöntemi.

Yukarıdaki kod, bir güvenilir sözlüğün AddAsync yönteme ITransaction nesnesi geçirilir. Dahili olarak, bir anahtarı kabul edelim sözlük yöntemleri, anahtar ile ilişkili bir Okuyucu/Yazıcı kilidi alır. Yöntemi anahtarın değerini değiştirir, yöntem anahtarı bir yazma kilidi alır ve ardından yöntemi yalnızca anahtarın değerini okur, anahtarı bir okuma kilidi alınır. AddAsync anahtarının değerini yeni ve gönderilen değerle değiştiren olduğundan, anahtarın yazma kilidi alınır. Bu nedenle, 2 (veya daha fazla) iş parçacığı aynı anda aynı anahtarla değerler eklemek çalışırsanız, bir iş parçacığı yazma kilidi ve diğer iş parçacıklarını engeller. Varsayılan olarak, kilit 4 saniye için yöntem bloğu; 4 saniye sonra yöntemleri bir TimeoutException atar. Yöntem aşırı yüklemeleri tercih ediyorsanız, açık zaman aşımı değeri geçirmenize izin yok.

Genellikle, yakalama ve tüm işlemi (yukarıdaki kodda gösterildiği gibi) denemeden bir TimeoutException için tepki vermek için kodunuzu yazın. Basit kodum yalnızca her seferinde 100 milisaniye geçirme Task.Delay aradığım. Ancak, gerçekte bir tür bir üstel geri alma gecikme kullanrak daha iyi olabilir.

Kilit alınıncaya sonra AddAsync ITransaction nesneyle ilişkili bir iç geçici sözlük anahtarı ve değeri nesne başvuruları ekler. Bu, okuma-your-kendi-yazma işlemleri semantiğiyle sağlamak için gerçekleştirilir. Diğer bir deyişle, AddAsync çağırdıktan sonra bile, henüz işlem taahhüt değil (aynı ITransaction nesnesini kullanarak) TryGetValueAsync sonraki çağrı değeri döndürür. Ardından, anahtarınızı AddAsync serileştirir ve değeri için bayt dizileri nesneleri ve bu bayt dizileri yerel düğümde bir günlük dosyası ekler. Son olarak, aynı anahtar/değer bilgilere sahip oldukları için tüm ikincil çoğaltmalara bayt dizileri AddAsync gönderir. Anahtar/değer bilgiler bir günlük dosyasına yazılmış olsa da, bilgi ile ilişkili işlem kaydedilene kadar sözlük parçası dikkate alınmaz.

Yukarıdaki kod CommitAsync çağrısı tüm hareketin işlemlerini kaydeder. Özellikle, günlük dosyası yerel düğümde işleme bilgileri ekler ve ayrıca tüm ikincil çoğaltmalara yürütme kaydı gönderir. Bir ' % s'Çekirdeği (çoğu) çoğaltmaların vermiş sonra tüm veri değişiklikleri kalıcı olarak kabul edilir ve diğer iş parçacıkları/işlem aynı anahtarlar işleyebileceğiniz şekilde ITransaction nesnesi kullanılabilir anahtarlarla ilişkili kilitleri serbest bırakılır ve bunların değerler.

CommitAsync (oluşturulan genellikle bir özel durum nedeniyle) çağrılmazsa ITransaction nesneyi elden. Service Fabric işlenmemiş ITransaction nesneyi elden, iptal bilgileri yerel düğümün günlük dosyasına ekler. ve hiçbir şey ikincil çoğaltmalardan birine gönderilmesi gerekir. Ve ardından, işlem kullanılabilir anahtarlarla ilişkili kilitleri serbest bırakılır.

## <a name="common-pitfalls-and-how-to-avoid-them"></a>Yaygın görülen tehlikeleri ve bunların nasıl önleneceğini
Güvenilir koleksiyonlar dahili olarak nasıl çalıştığını anlamak, bazı yaygın misuses bunların bir göz atalım. Aşağıdaki kodu bakın:

```csharp
using (ITransaction tx = StateManager.CreateTransaction())
{
   // AddAsync serializes the name/user, logs the bytes,
   // & sends the bytes to the secondary replicas.
   await m_dic.AddAsync(tx, name, user);

   // The line below updates the property’s value in memory only; the
   // new value is NOT serialized, logged, & sent to secondary replicas.
   user.LastLogin = DateTime.UtcNow;  // Corruption!

   await tx.CommitAsync();
}
```

Normal .NET sözlük ile çalışırken, bir anahtar/değer sözlüğüne ekleyin ve ardından (örneğin, LastLogin) bir özelliğin değerini değiştirin. Ancak, bu kod ile güvenilir bir sözlük düzgün çalışmaz. Unutmayın önceki tartışmadan AddAsync çağrısı bayt dizileri anahtar/değer nesnelere serileştirir ve ardından diziler yerel bir dosyaya kaydeder ve aynı zamanda ikincil çoğaltmalara gönderir. Bir özellik daha sonra değiştirirseniz, bu özelliğin değeri yalnızca bellekte değiştirir; Yerel dosya ya da kopyaya gönderilen verileri etkilemez. Bellekte nedir, hemen işlem kilitlenirse oluşturulur. Yeni bir işlem başlatıldığında veya başka bir çoğaltması birincil hale gelirse, eski özellik değeri kullanılabilir olduğunu.

Yeterince ne kadar kolay, yukarıda gösterilen hata türü olmasını sağlamaktır stres olamaz. Ve işlem çökmesi hakkında bir hata olursa/olduğunda yalnızca öğreneceksiniz. Aşağıdaki iki satırı tersine çevirmek için kod yazmak için doğru şekilde teknolojidir:


```csharp
using (ITransaction tx = StateManager.CreateTransaction())
{
   user.LastLogin = DateTime.UtcNow;  // Do this BEFORE calling AddAsync
   await m_dic.AddAsync(tx, name, user);
   await tx.CommitAsync();
}
```

Sıkça gösteren başka bir örnek aşağıda verilmiştir:

```csharp
using (ITransaction tx = StateManager.CreateTransaction())
{
   // Use the user’s name to look up their data
   ConditionalValue<User> user = await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (user.HasValue)
   {
      // The line below updates the property’s value in memory only; the
      // new value is NOT serialized, logged, & sent to secondary replicas.
      user.Value.LastLogin = DateTime.UtcNow; // Corruption!
      await tx.CommitAsync();
   }
}
```

Yeniden normal .NET sözlükleri, yukarıdaki kod düzgün çalışır ve bir ortak desendir: geliştirici, bir değeri aramak için bir anahtar kullanır. Değeri varsa, geliştirici bir özelliğin değerini değiştirir. Bununla birlikte, güvenilir koleksiyonlar ile bu kodu olarak zaten ele alınan aynı sorun gösteriyor: **güvenilir bir koleksiyon için verdiğiniz sonra nesneyi değiştirmemelisiniz.**

Güvenilir bir koleksiyondaki bir değer güncelleştirmek için doğru yoldur mevcut değere bir başvuru almak ve sabit başvuruya göre bu başvurulan nesne göz önünde bulundurun. Daha sonra orijinal nesneyi tam bir kopyası olan yeni bir nesne oluşturun. Şimdi, bu yeni bir nesne durumunu değiştirebilir ve koleksiyona yeni nesne bayt dizileri ile seri hale, böylece yerel dosyanın sonuna ve kopyaya gönderilen yazma. Değişiklikler yaptıktan sonra bellek içi nesneler, yerel dosya ve tüm çoğaltmalar aynı tam duruma sahip. Tüm iyi!

Aşağıdaki kod, doğru şekilde güvenilir bir koleksiyondaki bir değer güncelleştirme gösterir:

```csharp
using (ITransaction tx = StateManager.CreateTransaction())
{
   // Use the user’s name to look up their data
   ConditionalValue<User> currentUser = await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (currentUser.HasValue)
   {
      // Create new user object with the same state as the current user object.
      // NOTE: This must be a deep copy; not a shallow copy. Specifically, only
      // immutable state can be shared by currentUser & updatedUser object graphs.
      User updatedUser = new User(currentUser);

      // In the new object, modify any properties you desire
      updatedUser.LastLogin = DateTime.UtcNow;

      // Update the key’s value to the updateUser info
      await m_dic.SetValue(tx, name, updatedUser);
      await tx.CommitAsync();
   }
}
```

## <a name="define-immutable-data-types-to-prevent-programmer-error"></a>Programcı hatayı önlemek için sabit veri türlerini tanımlayın
Yanlışlıkla sabit göz önünde bulundurmanız gereken olduğunuz bir nesnenin durumu değiştirdiği kod üretir, ideal olarak, derleyici hataları bildirmek istiyoruz. Ancak, C# Derleyici bunu özelliği yok. Bu nedenle, programcı olası hataları önlemek için tanımladığınız türler öneririz sabit türleri için güvenilir koleksiyonlar ile kullanın. Bu özellikle, çekirdek değer türleri (örneğin, sayıları [Int32, UInt64, vb.], DateTime, Guid, TimeSpan ve benzeri) için takılıyor anlamına gelir. Ayrıca, dize de kullanabilirsiniz. Koleksiyon Özellikleri serileştirmek olarak önlemek idealdir ve bunları seri durumdan çıkarılırken sık performansı olumsuz etkileyebilir. Koleksiyon özellikleri kullanmak istiyorsanız, ancak yüksek oranda kullanılmasını öneririz. NET sabit koleksiyonlar kitaplığı ([gt;System.Collections.Immutable](https://www.nuget.org/packages/System.Collections.Immutable/)). Bu kitaplığı indirilerek kullanılabilir https://nuget.org. Ayrıca, sınıflar mühürleme ve mümkün olduğunda alanları salt okunur yapma öneririz.

Aşağıdaki UserInfo türünü tanımlayan yukarıda sözü edilen önerileri yararlanarak sabit bir tür gösterilmektedir.

```csharp
[DataContract]
// If you don’t seal, you must ensure that any derived classes are also immutable
public sealed class UserInfo
{
   private static readonly IEnumerable<ItemId> NoBids = ImmutableList<ItemId>.Empty;

   public UserInfo(String email, IEnumerable<ItemId> itemsBidding = null) 
   {
      Email = email;
      ItemsBidding = (itemsBidding == null) ? NoBids : itemsBidding.ToImmutableList();
   }

   [OnDeserialized]
   private void OnDeserialized(StreamingContext context)
   {
      // Convert the deserialized collection to an immutable collection
      ItemsBidding = ItemsBidding.ToImmutableList();
   }

   [DataMember]
   public readonly String Email;

   // Ideally, this would be a readonly field but it can't be because OnDeserialized
   // has to set it. So instead, the getter is public and the setter is private.
   [DataMember]
   public IEnumerable<ItemId> ItemsBidding { get; private set; }

   // Since each UserInfo object is immutable, we add a new ItemId to the ItemsBidding
   // collection by creating a new immutable UserInfo object with the added ItemId.
   public UserInfo AddItemBidding(ItemId itemId)
   {
      return new UserInfo(Email, ((ImmutableList<ItemId>)ItemsBidding).Add(itemId));
   }
}
```

Öğe kimliği türü de bir sabit burada gösterildiği gibi türüdür:

```csharp
[DataContract]
public struct ItemId
{
   [DataMember] public readonly String Seller;
   [DataMember] public readonly String ItemName;
   public ItemId(String seller, String itemName)
   {
      Seller = seller;
      ItemName = itemName;
   }
}
```

## <a name="schema-versioning-upgrades"></a>Şema sürümü (yükseltme)
Dahili olarak, güvenilir koleksiyonlar kullanarak, nesne seri hale. NET'in DataContractSerializer. Serileştirilmiş nesneler birincil kopyanın yerel diske yazılır ve ayrıca ikincil çoğaltmalara iletilir. Hizmetinizi geliştikçe hizmetiniz için gerekli veri (şema) türüne dönüştürmek istersiniz olasıdır. Verileriniz çok dikkatli yaklaşım sürüm oluşturma. İlk ve, her zaman eski verileri seri durumdan olmalıdır. Bu özellikle, seri durumundan çıkarma kodunuzu sonsuz geriye dönük olarak uyumlu olması gerektiği anlamına gelir: Hizmet kodunuzun sürüm 333 güvenilir koleksiyonda 5 yıl önce hizmeti kodunuza 1 sürümü tarafından yerleştirilen veri çalışabilir olmalıdır.

Ayrıca, hizmet yükseltilmiş bir yükseltme etki alanı aynı anda kodudur. Bu nedenle, yükseltme sırasında hizmet kodunuzun aynı anda çalışan iki farklı sürümlerini sahip. Hizmet kodunuzu eski sürümleri yeni şemayı işlemek mümkün olmayabilir gibi yeni şemayı kullanın. yeni sürümü hizmet kodunuzun olmamasına özen gösterin gerekir. Mümkün olduğunda, her sürümü hizmetinizin ileri doğru uyumlu bir sürüme göre olacak şekilde tasarlamanız gerekir. Özellikle, bu hizmet kodunuzun V1 açıkça işlemiyor herhangi bir şema öğeleri yoksay mümkün olması gerektiğini anlamına gelir. Ancak, herhangi bir veri açıkça hakkında bilmez ve yeniden kullanıma sözlük anahtarı veya değeri güncelleştirilirken yazma kaydedemezsiniz olmalıdır.

> [!WARNING]
> Bir anahtar şemasını değiştirebilirsiniz, ancak, anahtarın karma kod ve eşittir algoritmaları kararlı olduğunu emin olmanız gerekir. Bu algoritmalar birini nasıl çalışması değiştirirseniz, anahtarı güvenilir bir sözlük içinde hiç olmadığı kadar yeniden aramak mümkün olmayacaktır.
> Bir anahtar vardır, ancak kullanın--anahtar olarak dizesinin kendisinin kullanmayın String.GetHashCode sonucu anahtar olarak .NET dizeleri kullanılabilir.

Alternatif olarak, hangi genellikle iki bir yükseltme olarak adlandırılır gerçekleştirebilirsiniz. İki aşamalı bir yükseltmeye hizmetinizi V2 V1'den yükseltme: V2, kodu içeren yeni bir şema değişikliği ile nasıl bilir, ancak bu kod yürütülmez. V2 kod V1 verileri okur, üzerinde çalışır ve V1 verileri yazar. Tüm yükseltme etki alanları arasında yükseltme tamamlandıktan sonra daha sonra şekilde çalışan V2 örnekleri yükseltme tamamlandıktan sinyal verebilirsiniz. (Tek yönlü sinyaliyle bir yapılandırma yükseltmeyi alma için budur; bu iki aşamalı yükseltme bunu yapar.) Artık, V2 örnekleri okunan V1, V2 veri dönüştürme, üzerinde çalışır ve V2 veri yazılmasına yarar. Diğer örnekleri V2 veri okurken dönüştürmeniz gerekmez, bunlar yalnızca üzerinde çalışır ve V2 veri yazma.

## <a name="next-steps"></a>Sonraki Adımlar
İleri uyumlu veri sözleşmeleri oluşturma hakkında bilgi edinmek için [İleri uyumlu veri sözleşmeleri](https://msdn.microsoft.com/library/ms731083.aspx)

Sürüm oluşturma veri sözleşmelerinde en iyi yöntemleri öğrenmek için bkz: [veri sözleşmesi sürümü oluşturma](https://msdn.microsoft.com/library/ms731138.aspx)

Sürüme dayanıklı veri sözleşmeleri uygulama hakkında bilgi edinmek için bkz: [sürüme dayanıklı serileştirme geri çağırmaları](https://msdn.microsoft.com/library/ms733734.aspx)

Birden çok sürüm genelinde çalışabilirler veri yapısı sağlamayı öğrenmek için bkz: [IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx)