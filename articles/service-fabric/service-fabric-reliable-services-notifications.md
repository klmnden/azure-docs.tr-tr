---
title: "Güvenilir hizmetler bildirimleri | Microsoft Docs"
description: "Service Fabric Reliable Services bildirimler için kavramsal belgeler"
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: masnider,vturecek
ms.assetid: cdc918dd-5e81-49c8-a03d-7ddcd12a9a76
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 6/29/2017
ms.author: mcoskun
ms.openlocfilehash: c6a53d851510ed5e6eec1f3ac0f636ad034a6d4c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="reliable-services-notifications"></a>Güvenilir hizmetler bildirimleri
Bildirimleri ilginizi bir nesneye yapılan değişiklikleri izlemek istemcilerinin izin verir. İki nesne türünün destek bildirimleri: *güvenilir durum Yöneticisi* ve *güvenilir sözlük*.

Bildirimleri kullanmak için yaygın nedenler şunlardır:

* Yapı ikincil dizinler gibi görünümlerde gerçekleştirilip veya filtrelenmiş görünümleri yinelemenin durumunun bir araya getirilir. Bir örnek, güvenilir sözlükteki tüm anahtarları sıralanmış bir dizindir.
* Gönderen izleme gibi verileri son bir saatte eklenen kullanıcıların sayısı.

Bildirimleri işlemleri uygulama bir parçası olarak tetiklenir. Bu yüzden, olası ve zaman uyumlu olaylar pahalı işlemleri eklememelisiniz kadar hızlı bildirimleri yapılmalıdır.

## <a name="reliable-state-manager-notifications"></a>Güvenilir durum Yöneticisi bildirimleri
Güvenilir durum Yöneticisi aşağıdaki olaylar için bildirimler sağlar:

* İşlem
  * İşleme
* Durum Yöneticisi
  * Yeniden oluşturma
  * Güvenilir bir duruma eklenmesi
  * Güvenilir durumunun kaldırma

Güvenilir durum Yöneticisi geçerli inflight işlemleri izler. Bir bildirim harekete neden olan işlem durumu yalnızca değişikliği kabul edilmek bir işlemdir.

Güvenilir durum Yöneticisi güvenilir sözlük ve güvenilir sırası gibi güvenilir durumları koleksiyonu tutar. Güvenilir durum Yöneticisi bu koleksiyon değiştiğinde bildirim ateşlenir: güvenilir durumu eklendiğinde veya kaldırıldığında ya da tüm koleksiyon yeniden oluşturulur.
Güvenilir durum Yöneticisi koleksiyonu üç durumda yeniden oluşturulur:

* Kurtarma: bir çoğaltma başladığında, önceki durumuna diskten kurtarır. Kurtarma sonunda kullandığı **NotifyStateManagerChangedEventArgs** kurtarılan güvenilir durumları kümesini içeren bir olay tetiklenecek.
* Tam kopyalama: bir çoğaltma yapılandırma kümesi katılabilmesi için önce oluşturulacak sahiptir. Bazı durumlarda, güvenilir durum yöneticisinin durum boşta ikincil çoğaltmaya uygulanması için birincil çoğaltmadan tam bir kopyasını gerektirir. İkincil çoğaltmadaki güvenilir durum Yöneticisi'nin kullandığı **NotifyStateManagerChangedEventArgs** birincil çoğaltmadan edinilen güvenilir durumları kümesini içeren bir olay tetiklenecek.
* Geri yükleme: olağanüstü durum kurtarma senaryolarında yinelemenin durumu bir yedekten geri yüklenebilir **RestoreAsync**. Böyle durumlarda, birincil çoğaltmadaki güvenilir durum Yöneticisi kullanan **NotifyStateManagerChangedEventArgs** yedekten geri güvenilir durumları kümesini içeren bir olay tetiklenecek.

İşlem bildirimleri ve/veya durum Yöneticisi bildirimleri kaydetmek için ile kaydetmeniz gerekir **TransactionChanged** veya **StateManagerChanged** güvenilir durumu Yöneticisi olayları. Durum bilgisi olan hizmet Oluşturucusu bu olay işleyicileri ile kaydetmek için ortak bir yerdir. Oluşturucu kaydolduğunuzda, kullanım ömrü süresince bir değişiklik nedeniyle herhangi bir bildirim kaçırılması olmaz **IReliableStateManager**.

```C#
public MyService(StatefulServiceContext context)
    : base(MyService.EndpointName, context, CreateReliableStateManager(context))
{
    this.StateManager.TransactionChanged += this.OnTransactionChangedHandler;
    this.StateManager.StateManagerChanged += this.OnStateManagerChangedHandler;
}
```

**TransactionChanged** olay işleyicisi kullanan **NotifyTransactionChangedEventArgs** olay hakkındaki ayrıntıları sağlamak için. Eylem özelliği içerir (örneğin, **NotifyTransactionChangedAction.Commit**) değişikliğin türünü belirtir. Ayrıca, değiştirilen işlem başvuru sağlar TRANSACTION özelliği içerir.

> [!NOTE]
> Bugün, **TransactionChanged** yalnızca işlem tamamlanırsa olayları ortaya çıkar. Eylem sonra eşittir **NotifyTransactionChangedAction.Commit**. Ancak olayları işlem durumu değişiklikleri diğer türleri için gelecekte, yükseltilmiş. Eylem denetleniyor ve yalnızca beklediğiniz bir ise, olay işleme öneririz.
> 
> 

Aşağıda bir örnek verilmiştir **TransactionChanged** olay işleyicisi.

```C#
private void OnTransactionChangedHandler(object sender, NotifyTransactionChangedEventArgs e)
{
    if (e.Action == NotifyTransactionChangedAction.Commit)
    {
        this.lastCommitLsn = e.Transaction.CommitSequenceNumber;
        this.lastTransactionId = e.Transaction.TransactionId;

        this.lastCommittedTransactionList.Add(e.Transaction.TransactionId);
    }
}
```

**StateManagerChanged** olay işleyicisi kullanan **NotifyStateManagerChangedEventArgs** olay hakkındaki ayrıntıları sağlamak için.
**NotifyStateManagerChangedEventArgs** iki alt sınıfları olmayan: **NotifyStateManagerRebuildEventArgs** ve **NotifyStateManagerSingleEntityChangedEventArgs**.
Eylem özelliğinde kullanmak **NotifyStateManagerChangedEventArgs** yayınlanamıyor **NotifyStateManagerChangedEventArgs** doğru alt:

* **NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**
* **NotifyStateManagerChangedAction.Add** ve **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**

Aşağıda bir örnek verilmiştir **StateManagerChanged** bildirim işleyici.

```C#
public void OnStateManagerChangedHandler(object sender, NotifyStateManagerChangedEventArgs e)
{
    if (e.Action == NotifyStateManagerChangedAction.Rebuild)
    {
        this.ProcessStataManagerRebuildNotification(e);

        return;
    }

    this.ProcessStateManagerSingleEntityNotification(e);
}
```

## <a name="reliable-dictionary-notifications"></a>Güvenilir sözlük bildirimleri
Güvenilir sözlük aşağıdaki olaylar için bildirimler sağlar:

* Yeniden oluşturma: çağrılır **ReliableDictionary** durumuna kurtarılan ya da kopyalanmış yerel durumu ya da yedekleme kurtarıldı.
* Clear: çağrılır durumunu **ReliableDictionary** aracılığıyla temizlenmiş **ClearAsync** yöntemi.
* Ekleyin: adlı bir öğe için eklendiğinde **ReliableDictionary**.
* Güncelleştirme: bir öğe olarak adlandırılan **IReliableDictionary** güncelleştirildi.
* Kaldır: bir öğe adlı **IReliableDictionary** silinmiş olabilir.

Güvenilir sözlük bildirimleri almak için ile kaydetmeniz gerekir **DictionaryChanged** olay işleyicisini **IReliableDictionary**. Bu olay işleyicileri ile kaydetmek için bir ortak yerdir **ReliableStateManager.StateManagerChanged** bildirim ekleyin.
Ne zaman kaydetme **IReliableDictionary** eklenen **IReliableStateManager** herhangi bir bildirim kaçırılması olmaz sağlar.

```C#
private void ProcessStateManagerSingleEntityNotification(NotifyStateManagerChangedEventArgs e)
{
    var operation = e as NotifyStateManagerSingleEntityChangedEventArgs;

    if (operation.Action == NotifyStateManagerChangedAction.Add)
    {
        if (operation.ReliableState is IReliableDictionary<TKey, TValue>)
        {
            var dictionary = (IReliableDictionary<TKey, TValue>)operation.ReliableState;
            dictionary.RebuildNotificationAsyncCallback = this.OnDictionaryRebuildNotificationHandlerAsync;
            dictionary.DictionaryChanged += this.OnDictionaryChangedHandler;
            }
        }
    }
}
```

> [!NOTE]
> **ProcessStateManagerSingleEntityNotification** örnek yöntemidir, önceki **OnStateManagerChangedHandler** örnek çağrıları.
> 
> 

Önceki kod kümeleri **IReliableNotificationAsyncCallback** , bunların ile arabirim **DictionaryChanged**. Çünkü **NotifyDictionaryRebuildEventArgs** içeren bir **IAsyncEnumerable** arabirimi--, zaman uyumsuz olarak--sıralanması gerekiyor yeniden bildirimleri aracılığıyla harekete **RebuildNotificationAsyncCallback** yerine **OnDictionaryChangedHandler**.

```C#
public async Task OnDictionaryRebuildNotificationHandlerAsync(
    IReliableDictionary<TKey, TValue> origin,
    NotifyDictionaryRebuildEventArgs<TKey, TValue> rebuildNotification)
{
    this.secondaryIndex.Clear();

    var enumerator = e.State.GetAsyncEnumerator();
    while (await enumerator.MoveNextAsync(CancellationToken.None))
    {
        this.secondaryIndex.Add(enumerator.Current.Key, enumerator.Current.Value);
    }
}
```

> [!NOTE]
> Önceki kodda, yeniden bildirim işleme bir parçası olarak önce işlenen toplam durum temizlenir. Güvenilir koleksiyonu ile yeni bir durum yeniden olduğundan, tüm önceki bildirimler önemli değildir.
> 
> 

**DictionaryChanged** olay işleyicisi kullanan **NotifyDictionaryChangedEventArgs** olay hakkındaki ayrıntıları sağlamak için.
**NotifyDictionaryChangedEventArgs** beş alt sınıfların sahiptir. Eylem özelliğini **NotifyDictionaryChangedEventArgs** yayınlanamıyor **NotifyDictionaryChangedEventArgs** doğru alt:

* **NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**
* **NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**
* **NotifyDictionaryChangedAction.Add** ve **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemAddedEventArgs**
* **NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**
* **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**

```C#
public void OnDictionaryChangedHandler(object sender, NotifyDictionaryChangedEventArgs<TKey, TValue> e)
{
    switch (e.Action)
    {
        case NotifyDictionaryChangedAction.Clear:
            var clearEvent = e as NotifyDictionaryClearEventArgs<TKey, TValue>;
            this.ProcessClearNotification(clearEvent);
            return;

        case NotifyDictionaryChangedAction.Add:
            var addEvent = e as NotifyDictionaryItemAddedEventArgs<TKey, TValue>;
            this.ProcessAddNotification(addEvent);
            return;

        case NotifyDictionaryChangedAction.Update:
            var updateEvent = e as NotifyDictionaryItemUpdatedEventArgs<TKey, TValue>;
            this.ProcessUpdateNotification(updateEvent);
            return;

        case NotifyDictionaryChangedAction.Remove:
            var deleteEvent = e as NotifyDictionaryItemRemovedEventArgs<TKey, TValue>;
            this.ProcessRemoveNotification(deleteEvent);
            return;

        default:
            break;
    }
}
```

## <a name="recommendations"></a>Öneriler
* *Yapmak* bildirim olayları mümkün olduğunca hızlı tamamlayın.
* *Sağlamadığı* pahalı işlemleri (örneğin, g/ç işlemleri) zaman uyumlu olaylar bir parçası olarak çalıştırmak.
* *Yapmak* olay işlemeden önce eylem türünü kontrol edin. Yeni Eylem türleri gelecekte eklenebilir.

Göz önünde bulundurmanız gereken bazı şeyler şunlardır:

* Bildirimler, bir işlemin yürütülmesini bir parçası olarak tetiklenir. Örneğin, bir geri bildirim geri yükleme işleminin son adımı olarak tetiklenir. Geri bildirim olayı işlenen kadar tamamlanmaz.
* Bildirimleri uygulanan işlemlerinin bir parçası olarak başlatıldığından istemciler yalnızca yerel olarak kaydedilmiş işlemleri için bildirim bakın. Ve işlemleri yalnızca yerel olarak kaydedilmiş olması garanti (diğer bir deyişle, oturum açmış), olabilir veya gelecekte geri olmayabilir.
* Yinele yolda uygulanan her işlem için tek bir bildirim tetiklenir. Bu işlem T1 Create(X), Delete(X) ve Create(X) içeriyorsa, o sırada X, silme işlemi için diğeri oluşturma yeniden oluşturulması için bir bildirim elde edersiniz anlamına gelir.
* Birden çok işlemleri içeren işlemleri için işlemleri, bunlar kullanıcı birincil çoğaltmadan alınan sırayla uygulanır.
* False ilerleme durumu işlenirken bir parçası olarak, bazı işlemler geri olabilir. Bildirimler, çoğaltma durumunu kararlı bir noktaya geri alma gibi geri alma işlemleri için oluşturulur. Önemli bir fark geri bildirimler, yinelenen anahtarlar olayları toplanır ' dir. Örneğin, T1 işlem geri alınamaz, Delete(X) için tek bir bildirim görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar
* [Güvenilir Koleksiyonlar](service-fabric-work-with-reliable-collections.md)
* [Güvenilir hizmetler hızlı başlangıç](service-fabric-reliable-services-quick-start.md)
* [Güvenilir hizmetler yedekleme ve geri yükleme (olağanüstü durum kurtarma)](service-fabric-reliable-services-backup-restore.md)
* [Güvenilir koleksiyonlar için Geliştirici Başvurusu](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

