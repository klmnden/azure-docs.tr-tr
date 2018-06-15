---
title: Güvenilir koleksiyonları ile çalışma | Microsoft Docs
description: Güvenilir koleksiyonları ile çalışmak için en iyi uygulamaları öğrenin.
services: service-fabric
documentationcenter: .net
author: rajak
manager: timlt
editor: ''
ms.assetid: 39e0cd6b-32c4-4b97-bbcf-33dad93dcad1
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/19/2017
ms.author: rajak
ms.openlocfilehash: 2568e116fdb3f80976d49787877d2ecf68f128ef
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34210826"
---
# <a name="working-with-reliable-collections"></a>Güvenilir Koleksiyonlar ile çalışma
Service Fabric, güvenilir koleksiyonlar aracılığıyla .NET geliştiricileri için kullanılabilir bir durum bilgisi olan programlama modeli sunar. Özellikle, Service Fabric güvenilir sözlük ve güvenilir sıra sınıfları sağlar. Bu sınıfların kullandığınızda, durumunuza (ölçeklenebilirlik) bölümlenmiş çoğaltılan (kullanılabilirlik için) ve (ACID semantiği için) bir bölüm içinde hareket gerçekleşti. Şimdi tipik bir güvenilir sözlük nesnesi kullanım bakın ve hangi kendi gerçekte yapılması bakın.

```csharp

///retry:

try {
   // Create a new Transaction object for this partition
   using (ITransaction tx = base.StateManager.CreateTransaction()) {
      // AddAsync takes key's write lock; if >4 secs, TimeoutException
      // Key & value put in temp dictionary (read your own writes),
      // serialized, redo/undo record is logged & sent to
      // secondary replicas
      await m_dic.AddAsync(tx, key, value, cancellationToken);

      // CommitAsync sends Commit record to log & secondary replicas
      // After quorum responds, all locks released
      await tx.CommitAsync();
   }
   // If CommitAsync not called, Dispose sends Abort
   // record to log & all locks released
}
catch (TimeoutException) {
   await Task.Delay(100, cancellationToken); goto retry;
}
```

Güvenilir sözlük nesnelerine (dışında ClearAsync) geri alınamaz değil, tüm işlemler ITransaction nesneyi gerektirir. Bu nesne herhangi ilişkili ve güvenilir bir sözlük ve/veya güvenilir yapmaya çalışmadan tüm değişiklikleri tek bir bölüm içindeki nesneleri sıraya. Bir ITransaction elde bölüm çağırarak nesne kullanıcının StateManager'ın CreateTransaction yöntemi.

Yukarıdaki kod ITransaction nesne güvenilir sözlüğün AddAsync yöntemine iletilir. Dahili olarak, bir anahtar kabul sözlük yöntemleri anahtarıyla ilişkilendirilmiş bir Okuyucu/Yazıcı kilit etkinleştirilir. Yöntemi anahtarın değerini değiştirirse, yöntem bir yazma kilidi anahtarı alır. ve ardından yöntemi yalnızca anahtarın değerini yazıyorsa, okuma kilidi anahtarı alınır. Anahtarın değerini yeni, geçirilen değerle AddAsync değiştirir olduğundan, anahtarın yazma kilidi alınır. Bu nedenle, 2 (veya daha fazla) iş parçacığı aynı anda aynı anahtarla değerleri eklemeye çalışırsanız, bir iş parçacığı yazma kilidi ve diğer iş parçacıklarından engeller. Varsayılan olarak, kilit 4 saniye için yöntemleri bloğu; 4 saniye sonra yöntemleri bir TimeoutException atar. Yöntemi aşırı Dilerseniz bir açık zaman aşımı değeri geçirmek izin yok.

Genellikle, bu yakalama ve tüm işlemi (yukarıdaki kodda gösterildiği gibi) denemeden tarafından bir TimeoutException tepki vermek için kodunuzu yazın. Basit kodumu yalnızca 100 milisaniyede her zaman geçirme Task.Delay aradığım. Ancak, gerçekte üstel geri alma gecikme çeşit kullanmayı devre dışı daha iyi olabilir.

Kilit edinilen sonra AddAsync ITransaction nesneyle ilişkili bir iç geçici sözlük anahtar ve değer nesne başvuruları ekler. Bu, okuma-bilgisayarınızı-kendi-yazma işlemleri semantiği ile sağlamak için gerçekleştirilir. Diğer bir deyişle, AddAsync çağırdıktan sonra dahi, henüz işlem taahhüt değil (aynı ITransaction nesnesi kullanarak) TryGetValueAsync sonraki çağrı değeri döndürür. Ardından, anahtarınızı AddAsync serileştirir ve değer bayt dizileri nesneleri ve bu bayt dizileri yerel düğüm üzerindeki bir günlük dosyasına ekler. Son olarak, aynı anahtar/değer bilgilere sahip oldukları için AddAsync bayt dizileri ikincil çoğaltmalar için gönderir. Anahtar/değer bilgilerini bir günlük dosyası için yazılmış olsa bile, ile ilişkili işlem kaydedilene kadar bilgileri sözlük parçası olarak kabul edilmez.

Yukarıdaki kod CommitAsync çağrısı tüm işlemdeki işlemlerini kaydeder. Özellikle, yerel düğümdeki günlük dosyasına kayıt bilgilerini ekler ve ayrıca ikincil çoğaltmalar için yürütme kaydı gönderir. Bir çoğaltma çekirdek (çoğunluğu) vermiş sonra tüm veri değişiklikleri kalıcı olarak kabul edilir ve diğer iş parçacıkları/işlemleri aynı anahtarlar işleyebileceğiniz şekilde ITransaction nesnesi yönetilebilir anahtarlarla ilişkili tüm kilitler serbest bırakılana ve bunların değerler.

CommitAsync (oluşturulan genellikle bir özel durum nedeniyle) çağrılmazsa ITransaction nesne atıldı. Service Fabric kaydedilmemiş ITransaction nesneyi atma, iptal bilgileri yerel düğümün günlük dosyasına ekler. ve hiçbir şey ikincil çoğaltmaları birine gönderilmesi gerekir. Ve daha sonra işlem yönetilebilir anahtarlarla ilişkili kilitleri serbest bırakılır.

## <a name="common-pitfalls-and-how-to-avoid-them"></a>Ortak Tuzaklar ve bunları önleme
Güvenilir koleksiyonları dahili olarak nasıl çalıştığını anlamak, bazı ortak misuses bunların bir göz atalım. Aşağıdaki kodu bakın:

```csharp
using (ITransaction tx = StateManager.CreateTransaction()) {
   // AddAsync serializes the name/user, logs the bytes,
   // & sends the bytes to the secondary replicas.
   await m_dic.AddAsync(tx, name, user);

   // The line below updates the property’s value in memory only; the
   // new value is NOT serialized, logged, & sent to secondary replicas.
   user.LastLogin = DateTime.UtcNow;  // Corruption!

   await tx.CommitAsync();
}
```

Normal bir .NET sözlüğü ile çalışırken, bir anahtar/değer sözlüğe ekleyin ve sonra (örneğin, LastLogin) bir özelliğin değerini değiştirin. Ancak, bu kod ile güvenilir bir sözlük düzgün çalışmaz. Unutmayın önceki tartışma AddAsync çağrısı bayt dizileri anahtar/değer nesnelere serileştirir ve ardından diziler yerel bir dosyaya kaydeder ve aynı zamanda ikincil çoğaltmalar için gönderir. Bir özellik daha sonra değiştirirseniz, bu özelliğin değeri yalnızca bellekte değiştirir; Yerel dosya veya çoğaltmalar için gönderilen verileri etkilemez. İşlem çökerse bellekte nedir hemen atılır. Yeni bir işlem başlatıldığında veya başka bir çoğaltma birincil hale gelirse, eski özellik değeri kullanılabilir olduğu.

Yeterli ne kadar kolay yukarıda gösterilen hata türü yapma olduğunu stres olamaz. Ve işlem arıza hakkında hata IF/olduğunda yalnızca öğreneceksiniz. Doğru bir şekilde kod yazmaya iki satırları tersine çevirmek için basitçe verilmiştir:


```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   user.LastLogin = DateTime.UtcNow;  // Do this BEFORE calling AddAsync
   await m_dic.AddAsync(tx, name, user);
   await tx.CommitAsync();
}
```

Aşağıda, bir ortak hata gösteren başka bir örnek verilmiştir:

```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> user =
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (user.HasValue) {
      // The line below updates the property’s value in memory only; the
      // new value is NOT serialized, logged, & sent to secondary replicas.
      user.Value.LastLogin = DateTime.UtcNow; // Corruption!
      await tx.CommitAsync();
   }
}
```

Yeniden normal .NET sözlükler, yukarıdaki kod düzgün çalışır ve ortak bir desen: geliştirici bir değeri aramak için bir anahtar kullanır. Değeri varsa, geliştirici bir özelliğin değerini değiştirir. Ancak, güvenilir koleksiyonlarla bu kodu zaten ele alınan aynı sorunun yaşandığı: **güvenilir bir koleksiyona verdiğiniz sonra bir nesne değiştirmemelisiniz.**

Güvenilir bir koleksiyonda bir değer güncelleştirmek için doğru bir şekilde olan mevcut değeri bir başvuru almak ve bu başvuru değişmez tarafından başvurulan nesne göz önünde bulundurun. Ardından, özgün nesne tam bir kopyası olan yeni bir nesne oluşturun. Şimdi, bu yeni nesnenin durumunu değiştirin ve yeni koleksiyon nesnesine bayt dizileri için serileştirilir böylece yerel dosyanın sonuna ve çoğaltmalar için gönderilen yazma. Değişiklikler yaptıktan sonra bellek içi nesneleri, yerel dosya ve tüm çoğaltmaların aynı tam olarak aynı durum vardır. Tüm iyi!

Aşağıdaki kod, güvenilir bir koleksiyonda bir değer güncelleştirmek için doğru bir şekilde gösterir:

```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> currentUser =
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (currentUser.HasValue) {
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
İdeal olarak, yanlışlıkla değişmez göz önünde bulundurmanız gereken bir nesnenin durumu değiştirdiği kod üretmek zaman derleyici hatalarını raporlamak isteriz. Ancak, C# Derleyici bunu olanağına sahip değildir. Bu nedenle, olası Programcı hataları önlemek için türlerden tanımladığınız öneririz değişmez tür için güvenilir koleksiyonlarla kullanın. Bu özellikle, çekirdek değer türleri (örneğin, numaraları [Int32, UInt64, vb.], DateTime, Guid, TimeSpan ve benzeri) için takılıyor anlamına gelir. Ve tabi ki, ayrıca dize kullanabilirsiniz. Seri hale getirme olarak koleksiyon özellikleri önlemek en iyisidir ve bunları seri durumdan sık performansa zarar verebilir. Ancak, koleksiyon özellikleri kullanmak istiyorsanız, yüksek oranda kullanılmasını öneririz. NET'in değişmez koleksiyonlar kitaplığı ([System.Collections.Immutable](https://www.nuget.org/packages/System.Collections.Immutable/)). Bu kitaplık Merkezi'nden kullanılabilir http://nuget.org. Ayrıca, sınıflarınızı mühürleme ve mümkün olduğunda alanları salt okunur yapma öneririz.

Aşağıdaki kullanıcı bilgisi türünü, daha önce bahsedilen önerileri yararlanarak bir sabit türü tanımlamak gösterilmiştir.

```csharp

[DataContract]
// If you don’t seal, you must ensure that any derived classes are also immutable
public sealed class UserInfo {
   private static readonly IEnumerable<ItemId> NoBids = ImmutableList<ItemId>.Empty;

   public UserInfo(String email, IEnumerable<ItemId> itemsBidding = null) {
      Email = email;
      ItemsBidding = (itemsBidding == null) ? NoBids : itemsBidding.ToImmutableList();
   }

   [OnDeserialized]
   private void OnDeserialized(StreamingContext context) {
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
   public UserInfo AddItemBidding(ItemId itemId) {
      return new UserInfo(Email, ((ImmutableList<ItemId>)ItemsBidding).Add(itemId));
   }
}
```

Öğe kimliği türü ayrıca bir değişmez aşağıda gösterildiği gibi türüdür:

```csharp

[DataContract]
public struct ItemId {

   [DataMember] public readonly String Seller;
   [DataMember] public readonly String ItemName;
   public ItemId(String seller, String itemName) {
      Seller = seller;
      ItemName = itemName;
   }
}
```

## <a name="schema-versioning-upgrades"></a>Şema sürüm oluşturma (yükseltme)
Dahili olarak, güvenilir koleksiyonları kullanarak nesnelerinizin seri hale. NET'in DataContractSerializer. Serileştirilmiş nesneler birincil kopyanın yerel diske yazılır ve ikincil çoğaltmalar için de iletilir. Hizmetinizi geliştikçe hizmetiniz için gerekli verileri (şema) türünü değiştirmek istersiniz olasıdır. Sürüm oluşturma dikkatli verilerinizin yaklaşımını gerekir. Öncelikle, her zaman eski verileri seri durumdan mümkün olmalıdır. Özellikle, bu seri durumdan çıkarma kodunuzu sonsuz geriye dönük olarak uyumlu olmalıdır anlamına gelir: sürüm 333 service kodunuzun sağlayabilmelidir güvenilir bir koleksiyonda 5 yıl önce hizmet kodunuzun 1 sürümü tarafından yerleştirilen veriler üzerinde işlem yaparsınız.

Ayrıca, hizmet yükseltilen bir yükseltme etki alanı aynı anda kodudur. Bu nedenle, yükseltme sırasında hizmet kodunuzun eşzamanlı çalışan iki farklı sürümü gerekir. Hizmet kodunuzu yeni sürümünü service kodunuzun eski sürümlerini yeni şema işlemek olmayabilir olarak yeni şemayı kullanmak zorunda kalmamak gerekir. Mümkün olduğunda, her sürüm 1 sürümü tarafından ileri doğru uyumlu olacak şekilde hizmetinizin tasarlamanız gerekir. Özellikle, bu hizmet kodunuzun V1 yalnızca açıkça işlemiyor hiçbir şema öğeleri yoksay mümkün olması anlamına gelir. Ancak, herhangi bir veri açıkça hakkında bilmiyor ve yalnızca geri çıkışı bir sözlük anahtar veya değer güncelleştirilirken yazma kaydedemediğini olmalıdır.

> [!WARNING]
> Bir anahtar şeması değiştirebilirsiniz, ancak, anahtarın karma kodu ve eşittir algoritmaları kararlı olduğundan emin olmalısınız. Bu algoritmalar birini nasıl çalıştığını değiştirirseniz, anahtarı güvenilir sözlük içinde herhangi bir zamanda yeniden aramak mümkün olmaz.
>
>

Alternatif olarak, ne genellikle 2 aşamalı yükseltme olarak bilinir gerçekleştirebilirsiniz. Aşama 2 yükseltme işlemine hizmetinizi V1 ile V2'ye yükseltme: V2 kodunu içerir, yeni şema değişikliği dağıtılacak bildiği ancak bu kodu yürütmez. V2 kod V1 veri okuduğunda üzerinde çalışır ve V1 verileri yazar. Tüm yükseltme etki alanları arasında yükseltme tamamlandıktan sonra daha sonra şekilde çalışan V2 örneklerini yükseltme tamamlandıktan sinyal. (Tek yönlü sinyal için bu yapılandırma yükseltme alma; Bu aşama 2 yükseltme bunu yapar.) Şimdi, V2 örnekleri V1 veri okuma, V2 veriye dönüştürme, üzerinde çalışır ve V2 verileri olarak yazamadı. Diğer örnekleri V2 verileri okurken dönüştürmeden zorunda değildir, bunlar yalnızca üzerinde çalışır ve V2 verileri yazma.

## <a name="next-steps"></a>Sonraki Adımlar
İleri uyumlu veri sözleşmeleri oluşturma hakkında bilgi edinmek için [İleri uyumlu veri sözleşmeleri](https://msdn.microsoft.com/library/ms731083.aspx).

Sürüm veri sözleşmelerinde en iyi yöntemleri öğrenmek için bkz: [veri sözleşmesi sürümü oluşturma](https://msdn.microsoft.com/library/ms731138.aspx).

Sürüm dayanıklı veri sözleşmeleri uygulamak öğrenmek için bkz: [sürüm toleranslı seri hale getirme geri çağrıları](https://msdn.microsoft.com/library/ms733734.aspx).

Birden çok sürümleri arasında çalışabilirler bir veri yapısını sağlamak öğrenmek için bkz: [IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx).
